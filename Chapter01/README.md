# Chapter 01: Conversation & Context

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, make sure you can:
- [ ] Start and exit Claude Code
- [ ] Understand the three permission modes
- [ ] Navigate directories in the terminal

---

## Introduction

Effective communication with Claude Code is not just about asking questions—it's about providing the right context. This chapter focuses on how to structure your prompts, reference files, and maintain conversation context for optimal results.

### Why Context Matters

Claude Code can only help effectively when it understands:
- **What** you're trying to accomplish
- **Where** the relevant code is located
- **Why** you need to make changes
- **How** the existing system works

---

## Topics

### 1. Effective Prompting Techniques

#### Be Specific
```bash
# Bad
> fix the bug

# Good
> Fix the authentication bug in src/auth.ts where users can't log in
> after password reset. The error message is "Invalid token"
```

#### Provide Context
```bash
# Bad
> add a button

# Good
> Add a "Save Draft" button to the BlogEditor component that saves
> the current content to localStorage. Follow the existing button
> styling in the codebase.
```

#### Break Down Complex Tasks
```bash
# Instead of one huge rechapter, break it down:
> First, let's understand the current authentication flow.
> Can you explain how login works in this project?

# Then follow up:
> Now let's add OAuth support. What files would need to change?
```

### 2. Special Prefixes

Claude Code has powerful prefix shortcuts:

| Prefix | Description | Example |
|--------|-------------|---------|
| `@` | Reference files/folders | `@src/auth.ts` |
| `!` | Execute bash command and inject output | `!git status` |
| `#` | Save to Claude's memory | `# Always use bun` |

```bash
# Execute bash and inject output (no token waste)
> !bun test
# Claude sees the test results instantly

# Save a rule to memory
> # Use TypeScript strict mode in this project
# Claude prompts for save location
```

### 3. File References with @-mentions

Reference files directly in your prompts using `@`:

```bash
> Look at @src/components/Button.tsx and create a similar
> component for form inputs

> Compare @package.json with @package-lock.json and check
> for version mismatches

> The bug is somewhere between @api/routes.ts and @api/handlers.ts
```

**Autocomplete**: Type `@` and start typing the filename—Claude Code will autocomplete.

### 4. Multi-turn Conversations

Claude Code maintains conversation context within a session:

```bash
> What does the UserService class do?
# Claude explains UserService

> How does it connect to the database?
# Claude uses previous context to explain DB connection

> Add a method to get users by email
# Claude knows we're still talking about UserService
```

#### Managing Context

| Command | Description |
|---------|-------------|
| `/clear` | Clear conversation history |
| `/compact` | Summarize and compress context |
| `/history` | View conversation history |
| `Ctrl+L` | Clear terminal (keeps context) |

### 5. Conversation Patterns

#### Exploration Pattern
```bash
> What is the architecture of this project?
> How does data flow from frontend to backend?
> Where are API endpoints defined?
```

#### Debugging Pattern
```bash
> I'm getting this error: [paste error]
> The error occurs when I try to [action]
> Here's the relevant code: @path/to/file.ts
```

#### Implementation Pattern
```bash
> I want to add [feature]
> Here are the requirements: [list]
> Please follow the patterns in @existing/similar/code.ts
```

### 6. Advanced Prompting Features

#### Ultrathink - Control Thinking Depth

Include these keywords to control how deeply Claude thinks before responding:

| Keyword | Thinking Budget | Use Case |
|---------|-----------------|----------|
| `think` | 4k tokens | Standard tasks |
| `think hard` | 10k tokens | Complex problems |
| `ultrathink` | 32k tokens | Architectural decisions |

```bash
> ultrathink about the best architecture for a real-time chat system
```

#### Prompt Stashing

Press `Ctrl+S` to stash your current prompt draft, send something else, and it auto-restores later—like git stash for prompts.

#### Prompt Suggestions

After Claude completes a task, grayed-out follow-up suggestions appear:
- Press `Tab` to edit/accept
- Press `Enter` to run immediately

### 7. Slash Commands Overview

| Command | Description |
|---------|-------------|
| `/help` | List all commands |
| `/model` | Change or view current model |
| `/cost` | Show token usage and costs |
| `/config` | View/modify configuration |
| `/bug` | Report a bug |
| `/doctor` | Diagnose common issues |
| `/context` | View what's consuming your token window |

---

## Resources

- [Prompt Engineering Guide](https://docs.anthropic.com/en/docs/prompt-engineering)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)

---

## Checklist

Answer these questions as if in an interview:

1. **What makes a prompt "effective" when working with Claude Code?**
   <details>
   <summary>Hint</summary>
   Specificity, context, clear goals, breaking down complexity
   </details>

2. **How do @-mentions work and why are they useful?**
   <details>
   <summary>Hint</summary>
   Direct file references, autocomplete, providing context without pasting code
   </details>

3. **When should you use `/clear` vs `/compact`?**
   <details>
   <summary>Hint</summary>
   /clear: start fresh. /compact: keep context but reduce token usage
   </details>

4. **How does conversation context affect Claude's responses?**
   <details>
   <summary>Hint</summary>
   Multi-turn understanding, building on previous answers, context window limits
   </details>

5. **What information should you provide when asking Claude to fix a bug?**
   <details>
   <summary>Hint</summary>
   Error message, steps to reproduce, relevant files (@-mention), expected vs actual behavior
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Ask Claude to explain a project structure and follow up with 3-4 questions that build on each answer
- [ ] Use @-mentions to reference specific files in your prompts
- [ ] Practice the debugging pattern: introduce a bug and ask Claude to find it with good context clues
- [ ] Practice the implementation pattern: rechapter a feature starting broad, then narrow down
- [ ] Use `/clear` or `/compact` to manage conversation context

### Try These Prompts

```bash
> What is the architecture of this project?
> Look at @src/components/Button.tsx and explain how it works
> I'm getting this error: [paste error]. The error occurs when I try to [action]
> Add a new utility function following the pattern in @src/utils/helpers.ts
```

---

## Advanced

- [ ] Explore `/compact` and understand how context compression works
- [ ] Try the same task with different prompt styles and compare results
- [ ] Learn about token limits and how to manage long conversations
- [ ] Experiment with providing code snippets directly vs using @-mentions
