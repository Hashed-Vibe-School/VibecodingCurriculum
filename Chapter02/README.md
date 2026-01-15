# Chapter 02: Installing Claude Code

**English** | [한국어](./README.ko.md)

## What You'll Learn

- The concept of a terminal
- Claude Code installation methods
- Login and first run

---

## Why Do You Need This?

Before you can talk to Claude Code, you need to install it on your computer. Think of it like downloading a messaging app before you can chat with friends.

**Real-world scenario**: You have an idea for a personal website. You want Claude to build it for you. But first, Claude needs to be on your computer so it can actually create and save files.

### Simple Analogy: Hiring an Assistant

Imagine you hired a brilliant assistant who can write code, but they're waiting outside your office. Before they can help you:

1. **Open the door** (open the terminal)
2. **Let them in** (install Claude Code)
3. **Introduce yourself** (login)

That's exactly what we're doing in this chapter.

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

## Try It Yourself

Let's make sure everything is working. Follow these steps:

### Step 1: Open Terminal
- Mac: Press `Command + Space`, type "Terminal", press Enter
- Windows: Press `Windows key`, type "PowerShell", press Enter

### Step 2: Check Installation
```bash
claude --version
```
You should see a version number like `2.1.3`.

### Step 3: Start Claude Code
```bash
claude
```
You should see the welcome screen with `>` prompt.

### Step 4: Say Hello
```
> Hello! Can you hear me?
```
Claude should respond. If it does, you're all set.

### Step 5: Exit
Type `/exit` or press `Ctrl + C` twice.

---

## Common Mistakes

### 1. Typing in the Wrong Place
Make sure you're typing in the terminal, not in a text editor or browser. The terminal is the black/white window with text.

### 2. Copying Commands Wrong
When copying installation commands, make sure to copy the entire line. Sometimes the first or last character gets missed.

### 3. Not Waiting for Installation
Installation can take a minute or two. Don't close the terminal while it's still running. Wait for it to say "Installation complete" or similar.

### 4. Forgetting to Restart Terminal
After installation, the terminal needs to be closed and reopened. This refreshes the settings so it knows Claude Code exists.

### 5. VPN or Firewall Issues
If login fails, try disabling VPN. Some corporate firewalls block the login process.

---

## If It Doesn't Work...

**Terminal won't open?**
- Mac: Try searching for "Terminal" in Spotlight (Command + Space)
- Windows: Try right-clicking the Start button and selecting "Windows Terminal" or "PowerShell"

**Installation command fails?**
- Check your internet connection
- Try a different installation method (npm if you have Node.js, or Homebrew on Mac)

**"claude: command not found" after installation?**
- Close the terminal completely and reopen it
- If still not working, restart your computer

**Login page doesn't open?**
- Try opening [claude.ai](https://claude.ai) manually in your browser first
- Make sure you're logged in there, then try `claude` again

**Still stuck?**
- Run `claude /doctor` to see what's wrong
- Check the [official documentation](https://docs.anthropic.com/claude-code) for latest troubleshooting

---

## Summary

Completed in this chapter:
- [x] Opening the terminal
- [x] Installing Claude Code
- [x] Login
- [x] First conversation

The next chapter covers conversation fundamentals with Claude Code.

Proceed to [Chapter 03: Your First Conversation](../Chapter03/README.md).
