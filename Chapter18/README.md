# Chapter 18: Advanced Features

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Using slash commands
- Automation with Hooks
- Using Skills

---

## Mastering Slash Commands

This section covers all available slash commands.

### Basic Commands

| Command | Function |
|---------|----------|
| `/help` | View help |
| `/clear` | Clear conversation |
| `/compact` | Summarize conversation (save tokens) |

### Status Commands

| Command | Function |
|---------|----------|
| `/status` | Check current status |
| `/cost` | Check costs |

### Configuration Commands

| Command | Function |
|---------|----------|
| `/config` | Open settings |
| `/model` | Change model |
| `/permissions` | Set permissions |

### Memory Commands

| Command | Function |
|---------|----------|
| `/memory` | Manage memory |
| `/init` | Initialize project settings |

---

## Commonly Used Slash Commands

### /compact - Compress Conversation

Long conversations use lots of tokens. Compress with `/compact`.

```
> /compact
```

Claude summarizes the conversation so far.
Context is preserved while tokens are saved.

### /clear - Start Fresh

```
> /clear
```

Starts a completely new conversation.
Forgets all previous content.

### /config - Change Settings

```
> /config
```

You can change Claude Code settings:
- Model selection
- Permission settings
- Theme change

---

## Hooks: The Beginning of Automation

Hooks are scripts that run automatically when certain events occur.

### What Are Hooks?

For example:
- When saving a file → Auto-format
- When committing → Auto-run tests
- When Claude edits code → Auto-lint

### Setting Up Hooks

Configure in `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "echo 'Claude is about to modify a file'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm run lint --fix"
      }
    ]
  }
}
```

### Common Hooks

#### Auto-format After File Edit

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "prettier --write \"${file}\""
      }
    ]
  }
}
```

#### Run Tests Before Commit

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit*)",
        "command": "npm test"
      }
    ]
  }
}
```

### Types of Hooks

| Hook | When It Runs |
|------|-------------|
| `PreToolUse` | Before tool use |
| `PostToolUse` | After tool use |
| `Notification` | When notification occurs |
| `Stop` | When Claude stops |

---

## Skills: Extension Features

Skills are ways to add new capabilities to Claude Code.

### Using Built-in Skills

Claude Code includes built-in Skills:

```
> /commit
```
Automatically analyzes changes and writes commit message.

```
> /review-pr
```
Reviews a PR for you.

### Creating Custom Skills

Add a markdown file to the `.claude/commands/` folder:

```markdown
# .claude/commands/deploy.md

Deploys the project.

## Steps
1. Run build
2. Verify tests
3. Deploy to Vercel
4. Check results

## Commands
- npm run build
- npm test
- vercel --prod
```

Now you can use the `/deploy` command!

### Skills Examples

#### Code Review Request

```markdown
# .claude/commands/review.md

Please review the code in this file.

## Check for
- Potential bugs
- Performance issues
- Security vulnerabilities
- Readability

## Output format
Each issue as:
- Location: Line number
- Problem: Description
- Suggestion: Improvement
```

#### Test Generation

```markdown
# .claude/commands/gen-tests.md

Generates test file for the current file.

## Rules
- Filename: originalfile.test.ts
- Tests for all functions
- Include edge cases
- Use Jest
```

---

## Subagents

For complex tasks, Claude uses multiple "subagents."

### What Are Subagents?

The main Claude delegates specific tasks to specialized agents:

- **Explore agent**: Codebase analysis
- **Plan agent**: Work planning
- **Execute agent**: Code writing

### Using Subagents

When you request large tasks, subagents work automatically:

```
> Analyze this entire project
> Find performance improvement points
> Create a refactoring plan
```

Claude:
1. Uses explore agent to analyze project
2. Uses plan agent to create improvement plan
3. Combines results and presents them

---

## Advanced Configuration

### Full settings.json Example

```json
{
  "model": "claude-sonnet-4-20250514",
  "language": "english",
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "denyByDefault": false
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "prettier --write \"${file}\""
      }
    ]
  },
  "customInstructions": "Always respond in English and add comments to code."
}
```

### Auto-approve Permissions

Auto-approve frequently used safe tools:

```json
{
  "permissions": {
    "autoApprove": [
      "Read",
      "Glob",
      "Grep",
      "WebFetch"
    ]
  }
}
```

### Custom Instructions

```json
{
  "customInstructions": "When writing code, always use TypeScript strict mode and add JSDoc comments to all functions."
}
```

---

## Practice: Your Own Workflow

### Basic Tasks

```
# 1. Create Custom Skill
> Add your own skill to .claude/commands/ folder

# 2. Set Up Hook
> Configure auto-format hook after file edits

# 3. Optimize Permissions
> Set frequently used tools to auto-approve
```

### Extra Challenges

```
> Create a deployment automation Skill
> Test + build + deploy in one go

> Create a daily report generation Hook
> Auto-summarize when work day ends
```

---

## Summary

What you learned in this chapter:
- [x] Mastering slash commands
- [x] Automation with Hooks
- [x] Creating and using Skills
- [x] Understanding subagents
- [x] Advanced configuration

In the next chapter, we cover more powerful automation tools: MCP and CI/CD.

[Chapter 19: Automation](../Chapter19/README.md)
