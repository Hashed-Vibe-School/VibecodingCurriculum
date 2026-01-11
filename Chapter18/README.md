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

## Advanced: Understanding How AI Agents Work

To get more from AI agents like Claude Code, understanding how they work helps.

### Complex Tasks Through Tool Composition

Claude Code performs complex tasks by **combining basic tools** like file reading, editing, and command execution. When you say "make a website," internally it:

1. Checks the file system (what kind of project)
2. Creates/modifies necessary files
3. Runs build commands
4. Verifies results

These steps are composed together.

Understanding this structure explains why smaller requests are more accurate—you can verify each tool worked correctly along the way.

### Handling Unexpected Requests

When basic tools are well-equipped, Claude can solve problems in ways you might not have anticipated. Tasks like "convert all images in this folder to WebP" are not predefined, but can be accomplished by combining existing tools.

This is why Claude Code is more than a simple code generator.

---

## Advanced: Managing Conversation Context

### Quality Drops as Conversations Get Long

AI models have limits on how much information they can process at once. As conversations lengthen, earlier content may not be remembered well, and overall context comprehension can blur.

`/compact` summarizes the conversation, but if quality has already degraded, the effect is limited.

### Separate Conversations by Topic

Handling auth, DB, and frontend in one conversation mixes contexts. Starting new conversations per topic lets each task stay focused.

### Conversation Reset Technique

When you feel quality has dropped:

1. Save current progress to a file (progress.md, etc.)
2. Reset conversation with `/clear`
3. Have Claude read the saved file to restore context

CLAUDE.md is preserved after `/clear`, so project rules remain.

---

## Advanced: Model Selection

Claude Code lets you choose between different models.

| Model | Characteristics | When to Use |
|-------|----------------|-------------|
| Sonnet | Fast and cost-effective | Clear tasks, simple implementation, refactoring |
| Opus | Deep reasoning, higher cost | Complex design decisions, architecture discussions |

**Practical tip**: Use a more powerful model for planning, a faster model for implementation. This balances cost and quality.

```
> /model opus
> How should I design this system?

(after plan is set)
> /model sonnet
> Build it according to the plan above
```

---

## Advanced: When You Get Stuck

### Start Fresh

If the conversation is tangled, `/clear` may be the fastest solution. A clean state is often better than accumulated confusion.

### Narrow the Scope

If a large task is not working, break it down.

```
# Not working
> Build the entire payment system

# Step by step
> Just make the payment button UI first
```

### Show with Examples

If explaining with words is difficult, show an example of the desired result. Claude recognizes patterns well.

```
> Response format should be like this:
> { "status": "ok", "data": [...] }
```

### Approach from a Different Angle

If repeating the same request does not work, express the problem differently.

```
# Not working
> Fix the state change logic

# Different approach
> Reimplement this as a state machine pattern
```

Changing perspective often leads to sudden success.

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
