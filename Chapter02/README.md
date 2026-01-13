# Chapter 02: Installing Claude Code

**English** | [한국어](./README.ko.md)

## What You'll Learn

- The concept of a terminal
- Claude Code installation methods
- Login and first run

---

## What is a Terminal?

A terminal is a window for communicating with your computer using text.

Normally, folders are clicked and apps are launched with icons. A terminal performs the same operations with text commands.

```
# Examples
cd Documents        # Go to Documents folder
ls                  # List files in current folder
```

This may appear complex, but don't worry. Claude has learned countless terminal commands, so when you say "show me the files in this folder," it knows to run the appropriate command (`ls`). For now, knowing how to open a terminal is sufficient.

### Opening a Terminal

**Mac:**
1. Press `Command + Space` to open Spotlight
2. Type "Terminal"
3. Press Enter

**Windows:**
1. Press the `Windows key`
2. Type "PowerShell"
3. Press Enter

---

## Claude Code Installation

### Method 1: Install Script (Recommended)

The simplest method. Open your terminal and paste this command.

**Mac / Linux:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

Installation is complete when the process finishes.

### Method 2: Homebrew (Mac Users)

For Homebrew users:
```bash
brew install --cask claude-code
```

### Method 3: npm (Node.js Users)

For those with Node.js installed:
```bash
npm install -g @anthropic-ai/claude-code
```

### Verify Installation

To confirm successful installation:
```bash
claude --version
```

A version number (e.g., `2.1.3`) indicates success.

---

## Login

Claude Code requires login on first run.

### 1. Start Claude Code

In your terminal:
```bash
claude
```

### 2. Login Screen

A browser window opens. Log in with your Claude account.

Create an account at [claude.ai](https://claude.ai) if needed.

### 3. Login Complete

After login, Claude Code runs in your terminal.

```
╭────────────────────────────────────────╮
│ Welcome to Claude Code!                │
│                                        │
│ Type your message below.               │
╰────────────────────────────────────────╯

>
```

The `>` prompt indicates readiness for conversation.

---

## Pricing Information

Claude Code is a paid service.

### Usage Options

1. **Claude Pro/Max Subscription**: Starting at $20/month. Sufficient for regular use.
2. **API Key**: Pay-as-you-go. For developers.

For beginners, Claude Pro subscription is recommended. See [claude.ai/pricing](https://claude.ai/pricing).

### Using with API Key (Optional)

For API key users:
```bash
export ANTHROPIC_API_KEY="your-api-key"
claude
```

---

## First Conversation

With login complete, test with a simple message.

```
> Hi! What can you do?
```

Claude introduces itself.

### Simple Test

```
> What's in this folder?
```

Claude understands the request for "file listing" and executes the appropriate terminal command (`ls` or `dir`), then displays the results.

```
> Create a file called hello.txt. Write "Hello World" inside.
```

A file is created. Open the folder to verify.

---

## Exit

To exit Claude Code:

- Type `/exit`
- Or press `Ctrl + C` twice

---

## Troubleshooting

### "command not found" Error

Close and reopen the terminal. If the issue persists:
```bash
# Mac/Linux
source ~/.bashrc
# or
source ~/.zshrc
```

### Login Issues

1. Verify login status at [claude.ai](https://claude.ai) in the browser
2. Try a different browser
3. Disable VPN if active

### Persistent Issues

```bash
claude /doctor
```
This command diagnoses the problem.

---

## Summary

Completed in this chapter:
- [x] Opening the terminal
- [x] Installing Claude Code
- [x] Login
- [x] First conversation

The next chapter covers conversation fundamentals with Claude Code.

Proceed to [Chapter 03: Your First Conversation](../Chapter03/README.md).
