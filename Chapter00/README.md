# Chapter 00: Getting Started

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, make sure you have:
- [ ] Do you have Node.js (v18+) installed?
- [ ] Are you comfortable using a terminal/command line?
- [ ] Do you have a code editor (VS Code recommended)?

---

## Introduction

Claude Code is a CLI (Command Line Interface) tool that brings AI-assisted development directly to your terminal. Unlike web-based chat interfaces, Claude Code can directly read, write, and execute code in your local environment.

### Claude.ai vs Claude Code

| Claude.ai (Web) | Claude Code (CLI) |
|-----------------|-------------------|
| Runs in browser | Runs in terminal |
| Requires copy-pasting code | Direct file read/write |
| Only uses conversation context | Recognizes entire project structure |
| Manual code application | Automatic code editing and execution |

### Why Claude Code?

- **Direct File Access**: Read and modify files without copy-pasting. Reference files with `@filename` and Claude reads them directly.
- **Context Awareness**: Understands your entire project structure and recognizes dependencies between related files.
- **Tool Integration**: Runs terminal commands directly: `git commit`, `npm install`, test execution, etc.
- **Privacy**: Your code stays local. Only prompts and necessary context are sent to the API.

---

## Topics

### 1. Installation

```bash
# Native binary (recommended - fastest)
curl -fsSL https://claude.ai/install.sh | bash

# Or via npm
npm install -g @anthropic-ai/claude-code
```

> **Note**: The native binary is recommended for better performance and automatic updates.

### 2. Authentication

```bash
# Start Claude Code and follow the authentication prompts
claude

# Or use API key directly
export ANTHROPIC_API_KEY="your-api-key"
claude
```

### 3. Basic Commands

| Command | Description |
|---------|-------------|
| `claude` | Start interactive session |
| `claude "prompt"` | One-shot command |
| `claude -p "prompt"` | Print mode (non-interactive) |
| `/help` | Show all available commands |
| `/clear` | Clear conversation history |
| `/exit` or `Ctrl+C` | Exit Claude Code |

### 4. Session Management

Continue your work across sessions:

```bash
# Resume the last session
claude --continue

# Pick from past sessions
claude --resume

# Name your session for later
/rename my-feature-work
```

### 5. Essential Shortcuts

| Shortcut | Action |
|----------|--------|
| `Esc Esc` | Rewind to previous checkpoint |
| `Shift+Tab` or `Alt+M` | Toggle permission modes (Normal → Accept Edits → Plan) |
| `Alt+P` / `Option+P` | Switch models during session |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+L` | Clear terminal screen (keeps conversation history) |
| `Ctrl+B` | Move current task to background |
| `Shift+Enter` | Newline in input |

> **Note**: On macOS, you may need to enable "Option as Meta" in your terminal settings to use Alt key combinations.

### 6. Permission Modes (Important!)

Claude Code has three permission modes to control what actions require approval. **Understanding this concept is key to using Claude Code effectively.**

#### Plan Mode

```
📋 Plan Mode - Read-only
Can only read and analyze files. All modifications, creation, and command execution are blocked.
```

**When to use?**
- When first exploring a new project/codebase
- When planning before making big changes
- When you want to understand code (without modifications)

#### Normal Mode (Default)

```
🔒 Normal Mode - Confirm every action
Asks for approval before all file edits and command execution.
```

**When to use?**
- When first learning Claude Code
- When modifying important code
- When you want to verify each change one by one

#### Accept Edits Mode

```
⚡ Accept Edits Mode - Auto-approve file edits
File modifications are auto-approved. Only dangerous commands (rm, git push, etc.) require confirmation.
```

**When to use?**
- When working quickly on a project you know well
- When doing repetitive modifications
- When you trust Claude enough

#### How to Switch Modes

```bash
# Start with CLI option
claude --permission-mode plan
claude --permission-mode acceptEdits

# Switch during session
Shift+Tab  # Cycle modes: Normal → Accept Edits → Plan → Normal
/plan      # Enter Plan mode directly
```

> **Pro Tip**: "Almost always start in Plan mode." When starting new work, press `Shift+Tab` to enter Plan mode. Let Claude understand the code and create a plan first, then switch to Normal or Accept Edits for better results.

### 7. First Interaction

```bash
# Start Claude Code
claude

# Try these prompts:
> What files are in this directory?
> Explain what this project does
> Create a simple hello.py file
```

---

## Resources

- [Official Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [GitHub Repository](https://github.com/anthropics/claude-code)
- [Anthropic API Pricing](https://www.anthropic.com/pricing)

---

## Checklist

Answer these questions as if in an interview:

1. **What is Claude Code and how does it differ from ChatGPT or Claude.ai?**
   <details>
   <summary>Hint</summary>
   Think about: local file access, CLI vs web, context awareness, tool execution
   </details>

2. **What are the three permission modes and when would you use each?**
   <details>
   <summary>Hint</summary>
   Normal, Accept Edits, Plan Mode - choose based on trust level and task type
   </details>

3. **How does Claude Code handle your code privacy?**
   <details>
   <summary>Hint</summary>
   What gets sent to the API? What stays local?
   </details>

4. **What is the difference between `claude "prompt"` and `claude -p "prompt"`?**
   <details>
   <summary>Hint</summary>
   Interactive vs print mode, output format differences
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Install Claude Code on your system
- [ ] Start a Claude Code session and explore basic commands
- [ ] Create a simple "Hello World" program using Claude
- [ ] Modify the program to accept user input
- [ ] Try each permission mode (Default, Accept Edits, Plan)
- [ ] Use Plan Mode to explore an open-source project
- [ ] Understand what happens when you close and reopen a session

### Try These Prompts

```bash
> What can you help me with?
> Create a Python script that greets users by name
> Explain how permission modes work
> Explore the Express.js repository and explain its structure
```

---

## Advanced

### Changing Models

The default model (Opus 4.5) handles most tasks well. Change models when needed:

```bash
claude --model claude-sonnet-4-5-20250929  # For faster responses
claude --model claude-haiku                 # For simple tasks (lint, etc.)
```

### Explore Claude Config Directory

Check what's in the `~/.claude/` directory:

```bash
ls -la ~/.claude/
# settings.json  - Global settings
# CLAUDE.md      - Personal global memory
# projects/      - Project-specific settings

# View config file contents
cat ~/.claude/settings.json
```

### Session Continuity

When you need to continue work later:

```bash
# Continue yesterday's work
claude --continue

# Pick from past sessions
claude --resume

# Continue sessions across devices
claude --teleport
# (Share session via QR code or code)
```

### IDE Extensions (Optional)

You can also use Claude Code in IDEs like VS Code, Cursor, and Zed. Search for "Claude Code" in the extension marketplace.

IDE extensions are convenient for visual diffs and file references, but all core Claude Code features are available in the terminal.

### Pro Tips

> **"Give Claude a way to verify its work"** - This is the most important tip. Ask Claude to run tests, check types, or validate output after making changes. This dramatically improves code quality.
