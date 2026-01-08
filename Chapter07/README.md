# Chapter 07: Hooks

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, ensure you:
- [ ] Have completed Chapter 00-06
- [ ] Understand JSON configuration files
- [ ] Are familiar with shell scripting basics

---

## Introduction

Hooks allow you to run custom shell commands automatically in response to Claude Code events. They're like Git hooks, but for your AI assistant—enabling validation, formatting, logging, and custom workflows.

### Why Hooks?

- **Automation**: Auto-format code after edits
- **Validation**: Check changes before they're applied
- **Integration**: Connect to external tools and services
- **Compliance**: Enforce rules automatically

---

## Topics

### 1. Hook Events

Claude Code supports these hook events:

| Event | When it fires | Matcher Support |
|-------|---------------|-----------------|
| `PreToolUse` | Before tool execution (can block) | Yes |
| `PostToolUse` | After tool completes | Yes |
| `PermissionRequest` | On permission dialogs | Yes |
| `Notification` | When sending notifications | Yes (optional) |
| `UserPromptSubmit` | Before processing user input | No |
| `Stop` | When main agent finishes | No |
| `SubagentStop` | When subagent finishes | No |
| `PreCompact` | Before context compression | Yes |
| `SessionStart` | When session begins | Yes |
| `SessionEnd` | When session ends | No |

### 2. Hook Configuration

Hooks are configured in settings files:

**Settings file locations**:
- `~/.claude/settings.json` - User settings
- `.claude/settings.json` - Project settings
- `.claude/settings.local.json` - Local project settings (not committed)

**Project hooks** (`.claude/settings.json`):
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**User hooks** (`~/.claude/settings.json`):
```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Session started at $(date)' >> ~/.claude/session.log"
          }
        ]
      }
    ]
  }
}
```

### 3. Hook Structure

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolName or regex",
        "hooks": [
          {
            "type": "command",
            "command": "your-shell-command",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

### 4. Available Environment Variables

Hooks can access these environment variables:

| Variable | Description |
|----------|-------------|
| `CLAUDE_PROJECT_DIR` | Absolute path to project root |
| `CLAUDE_CODE_REMOTE` | "true" for web, empty/unset for CLI |
| `CLAUDE_ENV_FILE` | File path to persist env vars (SessionStart only) |

### 5. Hook Input (via stdin)

All hooks receive JSON via stdin:

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Edit",
  "tool_input": { ... }
}
```

### 6. Matcher Patterns

Matchers control when hooks run (case-sensitive):

| Pattern | Matches |
|---------|---------|
| `Edit` | Only Edit tool |
| `Bash` | Only Bash tool |
| `Edit\|Write` | Edit or Write (regex) |
| `Notebook.*` | Tools starting with Notebook (regex) |
| `*` or `""` | All tools |

### 7. Hook Return Values

**Exit Codes**:
- **0**: Success (stdout processed for JSON or context)
- **2**: Blocking error (only stderr used, execution halts)
- **Other**: Non-blocking error (stderr shown in verbose mode)

**JSON Output Format** (Exit Code 0):
```json
{
  "decision": "approve|block|deny|ask",
  "reason": "Explanation",
  "continue": true,
  "stopReason": "Optional message when continue=false",
  "suppressOutput": false,
  "systemMessage": "Optional user warning"
}
```

### 8. Common Hook Patterns

#### Auto-Format After Edit
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

#### Log All Tool Usage
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date): tool used\" >> ~/.claude/tool.log"
          }
        ]
      }
    ]
  }
}
```

### 9. Debugging Hooks

```bash
# Test hook command manually
CLAUDE_PROJECT_DIR="/path/to/project" ./your-hook-script.sh

# View hook logs
claude --debug

# Check settings
claude /config
```

### 10. Security Considerations

> ⚠️ **USE AT YOUR OWN RISK**: Hooks execute arbitrary shell commands on your system.

**Best practices**:
1. Validate and sanitize inputs
2. Always quote shell variables: `"$VAR"` not `$VAR`
3. Block path traversal (check for `..`)
4. Use absolute paths for scripts
5. Avoid sensitive files (`.env`, `.git/`, keys)

---

## Resources

- [Claude Code Documentation](https://code.claude.com/docs)
- [Hooks Reference](https://code.claude.com/docs/en/hooks)
- [Settings Reference](https://code.claude.com/docs/en/settings)
- [Shell Scripting Guide](https://www.shellscript.sh/)

---

## Checklist

Answer these questions as if in an interview:

1. **What are hooks and when would you use them?**
   <details>
   <summary>Hint</summary>
   Auto-run commands on events: formatting, validation, logging, integration
   </details>

2. **What's the difference between PreToolUse and PostToolUse?**
   <details>
   <summary>Hint</summary>
   Pre: before tool runs (can block). Post: after tool completes.
   </details>

3. **How do matchers work in hook configuration?**
   <details>
   <summary>Hint</summary>
   Filter by tool name or regex pattern (case-sensitive)
   </details>

4. **How can hooks block dangerous operations?**
   <details>
   <summary>Hint</summary>
   Return exit code 2 or JSON with "decision": "block"
   </details>

---

## Mini Project: Development Workflow Automation

### Project Goals

Build a comprehensive hooks system by completing:

- [ ] Create auto-formatting hook that runs Prettier/ESLint after edits
- [ ] Create session logging hook that tracks session times and tool usage
- [ ] Add 2+ optional hooks (notifications, backup, etc.)
- [ ] Configure all hooks in `.claude/settings.json`
- [ ] Test that hooks work correctly

### Ideas to Try

- Create a hook that sends Slack/Discord notifications on certain events
- Build a backup hook that saves files before destructive operations
- Implement conditional behavior by parsing stdin JSON

---

## Advanced

### stdin JSON Parsing Example

Parse stdin JSON in your hook to extract information:

```bash
#!/bin/bash
# .claude/hooks/format-on-edit.sh

# Read JSON from stdin
INPUT=$(cat)

# Extract file path with jq
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

if [ -n "$FILE_PATH" ] && [ -f "$CLAUDE_PROJECT_DIR/$FILE_PATH" ]; then
  npx prettier --write "$CLAUDE_PROJECT_DIR/$FILE_PATH"
fi

exit 0
```

### Hook Debugging Tips

When hooks don't work as expected:

```bash
# 1. Test script standalone
echo '{"tool_input":{"file_path":"test.ts"}}' | CLAUDE_PROJECT_DIR="$(pwd)" ./hook.sh

# 2. Run in debug mode
claude --debug

# 3. Check hook settings
cat .claude/settings.json | jq '.hooks'
```
