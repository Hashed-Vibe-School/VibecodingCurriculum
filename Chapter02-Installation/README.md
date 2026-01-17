# Chapter 02: Installing Claude Code

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Understand what a terminal is
- Know the difference between GUI and CLI
- Install Claude Code
- Log in and run it for the first time
- Understand the difference between API keys and subscriptions

---

## ⏱️ Estimated Time

- Reading: **20 minutes**
- Hands-on: **20 minutes** (including installation)

---

## 📋 What You Need

- Internet connection
- Claude account (create one if you don't have it)
- Claude Pro subscription or API key

---

## 🔗 Previous Chapter Review

In [Chapter 01](../Chapter01-AI-Coding-Intro/README.md), we learned what vibecoding is and why coding with AI is the future. Now it's time to install the tool.

---

## Why Is This Necessary?

To talk with Claude Code, you first need to install it on your computer. It's like downloading a messenger app before you can chat with a friend.

**Real situation**: You have an idea for a personal website. You want Claude to make it. But Claude needs to be on your computer first to actually create and save files.

### Easy Analogy: Hiring an Assistant

Imagine you've hired a brilliant assistant who's great at writing code, but they're still waiting outside your office. To get help, you need to:

1. **Open the door** (open terminal)
2. **Let them in** (install Claude Code)
3. **Introduce yourselves** (log in)

That's what we're doing in this chapter.

### Why Terminal Instead of Web?

You might think, "Can't I just chat on the claude.ai website?"

| Web Claude (claude.ai) | Claude Code (Terminal) |
|------------------------|------------------------|
| Only conversation | Conversation + file creation/modification |
| Copy and paste code | Execute code directly |
| Doesn't know project structure | Understands entire project |
| One file at a time | Multiple files simultaneously |

Claude Code works **directly** on your computer. That's why you need a terminal.

---

## What is a Terminal?

A terminal is a window where you communicate with your computer through text.

Normally, you click folders with your mouse and press app icons to run them. The terminal does the same thing with text.

```
# Example
cd Documents        # Move to Documents folder
ls                  # View file list in current folder
```

### GUI vs CLI: Two Ways to Use a Computer

There are two main ways to communicate with a computer:

#### GUI (Graphical User Interface)

The way we normally use computers.
- Click folder icons with mouse
- Press buttons to run apps
- Drag and drop to move files

**Pros:** Intuitive, easy to learn
**Cons:** Repetitive tasks are tedious, hard to automate

#### CLI (Command Line Interface)

A way of giving commands through text.
- Type commands with keyboard
- Results output as text
- Can automate with scripts

**Pros:** Fast, can automate, precise control
**Cons:** Need to know commands

> 💡 **Beginner Tip**
>
> It might look difficult, but don't worry!
> Claude has learned countless terminal commands, so if you say "Show me the file list in this folder," it runs automatically.
> **You just need to know how to open the terminal.**

### Why Do Developers Like the Terminal?

1. **Speed**: Typing is faster than moving a mouse
2. **Automation**: Can make scripts for repetitive tasks
3. **Remote work**: Terminal is essential when connecting to servers
4. **Precision**: Can do exactly what you want

> 🔥 **Pro Tip**
>
> The terminal might feel scary at first, but with Claude Code, you can speak in natural language so there's no need to memorize commands. As you get used to it, you'll feel the power of the terminal.

### How to Open Terminal

**Mac:**
1. Open Spotlight with `Command + Space`
2. Type "Terminal"
3. Press Enter

> 💡 **Beginner Tip (Mac)**
>
> If you're not familiar with Spotlight:
> 1. Open Finder
> 2. Click "Go" > "Utilities" in the top menu
> 3. Double-click "Terminal" app
>
> Since you'll use it often, it's convenient to pin it to the Dock!

**Windows:**
1. Press `Windows key`
2. Type "PowerShell"
3. Press Enter

> 💡 **Beginner Tip (Windows)**
>
> There are two options: PowerShell and Command Prompt (cmd). PowerShell is more modern, so use PowerShell.
> Start with normal execution, not "Run as administrator."

**Linux:**
1. `Ctrl + Alt + T` (most distributions)
2. Or search for "Terminal" in the app list

### Recommended Terminal Apps

The default terminal works fine, but using a better terminal app makes work much more enjoyable.

#### Mac: iTerm2 (Highly Recommended)

Instead of the default terminal, we recommend [iTerm2](https://iterm2.com).

```
┌─────────────────────────────────────────────────────────────────┐
│                    iTerm2 vs Default Terminal                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ✅ Split screen: View multiple terminals in one window         │
│     (Can run Claude Code while doing other work)                │
│                                                                 │
│  ✅ Search: Easily find previously entered commands             │
│                                                                 │
│  ✅ Auto-complete: Smarter auto-complete reduces typing         │
│                                                                 │
│  ✅ Themes/Colors: Choose eye-friendly color themes             │
│                                                                 │
│  ✅ Shortcuts: More shortcuts for faster work                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Installation:**
1. Download from [iterm2.com](https://iterm2.com)
2. Or via Homebrew: `brew install --cask iterm2`

#### All Platforms: VS Code Built-in Terminal

If you already use VS Code, the built-in terminal is convenient.

```
┌─────────────────────────────────────────────────────────────────┐
│              Benefits of VS Code Built-in Terminal              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ✅ Code and terminal in one screen: No switching back and     │
│     forth                                                       │
│                                                                 │
│  ✅ Auto file path recognition: Click error messages to jump   │
│     to the file                                                 │
│                                                                 │
│  ✅ Multiple terminal tabs: Open several terminals for         │
│     different purposes                                          │
│                                                                 │
│  ✅ Integrated environment: Everything for development in      │
│     one place                                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**How to use:**
- Press `` Ctrl + ` `` (backtick) in VS Code
- Or menu: View → Terminal

> 💡 **Beginner Tip**
>
> It's okay to start with the default terminal!
> After getting comfortable with Claude Code, try switching to iTerm2 or VS Code terminal.
> You can switch anytime.

### Understanding the Terminal Ecosystem

Terminals are like cars - there are many types. Let's understand through an analogy.

```
┌─────────────────────────────────────────────────────────────────┐
│           🚗 Terminal = Car, Which one should you drive?        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📦 Default Terminal = Driving school car                       │
│  └─ Mac Terminal, Windows PowerShell                            │
│     "Basic car for learning. Does everything but fewer extras"  │
│                                                                 │
│  🚀 Enhanced Terminal = New car with options                    │
│  └─ iTerm2, Windows Terminal, Alacritty, Hyper                  │
│     "GPS, backup camera, heated seats! More comfortable & fast" │
│                                                                 │
│  🤖 AI Terminal = Self-driving car                              │
│  └─ Warp, Amazon Q (formerly Fig)                               │
│     "Just say where to go. AI helps even if you don't know      │
│      commands"                                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### What is an Orchestration Layer?

A complex term, but simply explained:

```
┌─────────────────────────────────────────────────────────────────┐
│        🎼 Orchestration = Conductor leading the orchestra       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Think of an orchestra:                                         │
│                                                                 │
│  🎻 Violin, 🎺 Trumpet, 🥁 Drums... each playing alone is noisy │
│                                                                 │
│  But with a 🎩 conductor?                                       │
│  → "Violin first, then trumpet, drums later"                    │
│  → Beautiful music!                                             │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  Same in the terminal:                                          │
│                                                                 │
│  Read files, modify code, run commands... each is complex       │
│                                                                 │
│  But with a 🤖 AI conductor (orchestration layer)?              │
│  → "Read this file, modify there, run tests"                    │
│  → AI handles everything in order!                              │
│                                                                 │
│  Tools like Warp and Cursor play this "AI conductor" role.      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Beginner Tip**
>
> "Orchestration layer" means **"AI coordinating multiple tasks for you."**
> Like ordering "one steak please" at a restaurant, and the chef cooks, plates, and serves it automatically.

#### Top 5 Free Terminals

All free, each with different strengths.

| Terminal | Platform | Features | Recommended For |
|----------|----------|----------|-----------------|
| **iTerm2** | Mac | Split screen, search, auto-complete, 20 years of stability | Almost essential for Mac users |
| **Windows Terminal** | Windows | Official MS app, tabs, PowerShell/WSL integration | Default choice for Windows users |
| **Warp** | Mac, Linux | Built-in AI, command auto-complete, modern UI, team sharing | Those who want AI assistance |
| **Alacritty** | All platforms | GPU accelerated for speed, config file customization | Those who value speed |
| **Hyper** | All platforms | Web tech based, infinite extension via plugins, pretty themes | Those who like customization |

```
┌─────────────────────────────────────────────────────────────────┐
│                    Terminal Selection Guide                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "I use Mac, what should I use?"                                │
│  └─→ iTerm2 (Most reliable and stable)                          │
│                                                                 │
│  "What about Windows?"                                          │
│  └─→ Windows Terminal (Free from MS Store)                      │
│                                                                 │
│  "I want AI help"                                               │
│  └─→ Warp (Can ask when you don't know commands)                │
│                                                                 │
│  "I like fast"                                                  │
│  └─→ Alacritty (Noticeably faster)                              │
│                                                                 │
│  "I want to customize nicely"                                   │
│  └─→ Hyper (Rich themes and plugins)                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Download links:**
- iTerm2: [iterm2.com](https://iterm2.com)
- Windows Terminal: Search "Windows Terminal" in Microsoft Store
- Warp: [warp.dev](https://www.warp.dev)
- Alacritty: [github.com/alacritty/alacritty](https://github.com/alacritty/alacritty)
- Hyper: [hyper.is](https://hyper.is)

> 🔥 **Pro Tip**
>
> **Warp** is a terminal with built-in AI that lets you ask in natural language when you don't know commands.
> Even more powerful when used with Claude Code.
> However, we recommend understanding the basics with the default terminal first, then switching later.

#### AI Code Editors: Cursor and Friends

```
┌─────────────────────────────────────────────────────────────────┐
│        🏠 Analogy: What tools for home repair?                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📞 Claude Code = Expert repairman you call                     │
│  ──────────────────────────────────────────                     │
│  "Hello, please fix the faucet" → Repairman comes and fixes     │
│  → You don't have to do it yourself. Just ask and expert        │
│    handles it!                                                  │
│  → But you wait for repairman (switch between terminal ↔ editor)│
│                                                                 │
│  🏪 Cursor = AI butler living in your house                     │
│  ──────────────────────────────────────────                     │
│  "Fix this" → Fixed immediately right beside you                │
│  → When you try something, "Is this what you're trying to do?"  │
│    suggests ahead                                               │
│  → Everything happens in one place (solved inside the editor)   │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  Both do the same thing. Just different approaches!             │
│  - Claude Code: Chat with AI in terminal → Files modified       │
│  - Cursor: Chat with AI in editor → Files modified              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Popular AI Code Editors:**

| Editor | Analogy | Features | Price |
|--------|---------|----------|-------|
| **Cursor** | 🏆 Most popular AI butler | VS Code based, most widely used | Free + Pro $20/mo |
| **Windsurf** | 🌊 Rides the code flow like a surfer | Strong code context understanding | Free + Pro |
| **Zed** | ⚡ Lightning fast editor | Ultra-fast, team collaboration features | Free |

```
┌─────────────────────────────────────────────────────────────────┐
│          🎯 What Cursor Does = Quick-witted Assistant           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📝 I'm typing code...                                          │
│     function add(a, b) {                                        │
│       ret|  ← typed this much                                   │
│                                                                 │
│  💡 Cursor: "Are you trying to type return a + b;?"             │
│     → Press Tab to complete!                                    │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  🔍 Select code block and say "make this cleaner"               │
│  💡 Cursor: Refactors and shows right there                     │
│     → Apply if you like, cancel if not                          │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  🐛 "Find and fix bugs in this project"                         │
│  💡 Cursor: Searches files for issues → Suggests fixes → Apply  │
│     → Like a detective finding clues                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**What should you use?**

```
┌─────────────────────────────────────────────────────────────────┐
│                    Situation-based Recommendation                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  "I'm just learning"                                            │
│  └─→ Claude Code (Follow this curriculum!)                      │
│      Understanding principles helps with any tool later         │
│                                                                 │
│  "I do lots of terminal work (git, servers, deployment)"        │
│  └─→ Claude Code (Terminal is home base)                        │
│                                                                 │
│  "Code editing is my main work"                                 │
│  └─→ Cursor (Edit directly in the editor)                       │
│                                                                 │
│  "I need to do both"                                            │
│  └─→ Run Claude Code inside Cursor (use both!)                  │
│                                                                 │
│  "I want to save money"                                         │
│  └─→ Claude Code (One Pro subscription covers everything)       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Beginner Tip**
>
> **Claude Code alone is enough for beginners!**
>
> Like learning to drive with manual transmission first makes automatic easy later,
> Understanding "what AI does" with Claude Code makes Cursor easy to pick up.
>
> After completing this curriculum and using Cursor, you'll think "Ah, so that's how it works!"

#### Terminology

- **Terminal Emulator**: Program that opens terminal windows (iTerm2, Windows Terminal, etc.)
- **Shell**: Program that interprets commands inside the terminal (bash, zsh, fish, etc.)
- **Prompt**: The `>` or `$` symbol waiting for command input

```
┌─────────────────────────────────────────────────────────────────┐
│  Terminal Emulator (iTerm2, Windows Terminal...)                │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Shell (zsh, bash, fish...)                             │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Prompt: user@mac ~ $                           │    │    │
│  │  │  This is where you enter commands               │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Beginner Tip**
>
> Looks complicated but don't worry!
> For now, just know "open terminal and type claude."
> Learn the rest one by one when needed.

---

## 🖼️ Expected Terminal Screen

### Mac Terminal

```
Last login: Fri Jan 17 10:30:00 on ttys000
username@MacBook ~ %
```

Enter commands after the `%` or `$` symbol.

**Screen layout explanation:**
- `username`: Your user name
- `MacBook`: Computer name
- `~`: Current location (home folder)
- `%`: Waiting for command input

### Windows PowerShell

```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\username>
```

Enter commands after the `>` symbol.

**Screen layout explanation:**
- `PS`: Means PowerShell
- `C:\Users\username`: Current location (home folder)
- `>`: Waiting for command input

### Terminal Basic Controls

| Control | Description |
|---------|-------------|
| Up/Down arrow | Recall previous commands |
| Tab | Auto-complete (file names, etc.) |
| Ctrl + C | Cancel current running command |
| Ctrl + L | Clear screen (same as clear) |
| Right-click | Paste copied text (Windows) |
| Cmd + V | Paste (Mac) |

---

## Installing Claude Code

### Method 1: Installation Script (Recommended)

The simplest method. Open terminal and copy-paste the command below.

**Mac / Linux:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

> ⚠️ **Caution**
>
> Be careful not to include leading or trailing spaces when copying the command.
> The entire command should be copied as one line.

**Expected installation screen:**
```
Downloading Claude Code...
Installing dependencies...
Setting up PATH...
Installation complete!

Run 'claude' to start.
```

When installation is complete, you're done.

### Method 2: npm (Node.js Users)

If you have Node.js installed:
```bash
npm install -g @anthropic-ai/claude-code
```

> 💡 **Beginner Tip**
>
> If you don't know what Node.js is, use Method 1.
> Method 2 is for those who already have a development environment set up.

### Method 3: Homebrew (Mac Users)

If you use Homebrew:
```bash
brew install claude-code
```

### Verify Installation

Verify that installation completed successfully:
```bash
claude --version
```

**Expected result:**
```
2.1.3
```
If a version number is output, you're successful.

> ⚠️ **Caution**
>
> The version number may differ depending on when you install.
> As long as a number appears, it's normal.

---

## Logging In

When you run Claude Code for the first time, you need to log in.

### 1. Start Claude Code

In terminal:
```bash
claude
```

### 2. Login Screen

A browser window will open. Log in with your Claude account.

If you don't have an account, you can create one at [claude.ai](https://claude.ai).

**Login process:**
1. Browser opens automatically
2. Log in with Claude account (or sign up)
3. Click "Allow Claude Code connection" button
4. Return to terminal and it connects automatically

### 3. Login Complete

Once login is complete, Claude Code runs in the terminal.

**Expected screen:**
```
╭────────────────────────────────────────╮
│ Welcome to Claude Code!                │
│                                        │
│ Type your message below.               │
╰────────────────────────────────────────╯

>
```

When the `>` prompt appears, you're ready to chat.

---

## Pricing Guide

Claude Code is a paid service.

### API Key vs Subscription: What's the Difference?

These two concepts can be confusing at first. Let me explain simply.

| Category | Claude Pro/Max Subscription | API Key Usage |
|----------|----------------------------|---------------|
| **Analogy** | Monthly buffet restaurant | Pay for what you eat |
| **Payment method** | Fixed $20/100 per month | Variable based on usage |
| **Usage limit** | Yes (daily/monthly limit) | No (up to balance) |
| **Setup difficulty** | Easy (just log in) | Slightly complex (key registration required) |
| **Recommended for** | General users, learners | Developers, heavy users |

### When Subscription is Right

- Just starting to learn Claude Code
- Only using a few hours per day
- Simple personal projects
- Want predictable costs

### When API Key is Right

- Developing all day
- Analyzing large codebases
- Sharing in team projects
- Usage varies a lot

> 💡 **Beginner Tip**
>
> For beginners, subscription is definitely recommended!
> 1. Setup is much simpler (just log in and done)
> 2. Costs are predictable
> 3. You can switch to API key later if needed

### How to Use

1. **Claude Pro/Max Subscription**: From $20/month. Sufficient for normal usage.
2. **API Key Usage**: Pay as you go. For developers.

If you're just starting, Claude Pro subscription is recommended. Check at [claude.ai/pricing](https://claude.ai/pricing).

### Using API Key (Optional)

If you have an API key:

**Mac/Linux:**
```bash
export ANTHROPIC_API_KEY="your-api-key"
claude
```

**Windows PowerShell:**
```powershell
$env:ANTHROPIC_API_KEY="your-api-key"
claude
```

> 🔥 **Pro Tip**
>
> If entering the API key every time is annoying, you can set it permanently as an environment variable:
> - Mac/Linux: Add export statement to `~/.bashrc` or `~/.zshrc`
> - Windows: Add ANTHROPIC_API_KEY to system environment variables

### How to Get an API Key

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in
3. Click "API Keys" in the left menu
4. Click "Create Key" button
5. Enter key name (e.g., "my-claude-code")
6. Copy the generated key (you can only see it at this time!)

> ⚠️ **Caution**
>
> The API key is only displayed once when created.
> Make sure to save it somewhere safe!
> If you lose it, you'll have to create a new one.

---

## First Conversation

Once you've logged in, let's do a simple test.

```
> Hello! What can you do?
```

Claude will introduce itself.

**Expected response:**
```
Hello! I'm Claude Code. I can help you with things like:

- Creating, modifying, deleting files
- Writing and explaining code
- Running terminal commands
- Analyzing project structure
- Finding and fixing bugs
- Various other development tasks

What would you like help with?
```

### Simple Tests

**Test 1: View File List**
```
> What's in this folder?
```

Claude understands the "view file list" request, runs the appropriate terminal command (`ls` or `dir`), and shows the result.

**Test 2: Create a File**
```
> Create a hello.txt file. Write "Hello" inside.
```

The file is created. Open the folder directly to verify.

**Test 3: Check Current Location**
```
> Where am I now?
```

It tells you the current working folder path.

**Test 4: Simple Calculation**
```
> What's 25 times 17?
```

Claude answers general questions too, not just coding.

---

## Exiting

To exit Claude Code:

- Type `/exit`
- Or press `Ctrl + C` twice

> 💡 **Beginner Tip**
>
> `Ctrl + C` once cancels the current task.
> Pressing it twice in a row completely exits Claude Code.

---

## 🔨 Try It Yourself

Let's verify everything is working properly.

### Step 1: Open Terminal
- Mac: Press `Command + Space`, type "Terminal", press Enter
- Windows: Press `Windows key`, type "PowerShell", press Enter

### Step 2: Verify Installation
```bash
claude --version
```

**Expected result:** Version number like `2.1.3`

**If it doesn't work:** See "Troubleshooting" section below

### Step 3: Start Claude Code
```bash
claude
```

**Expected result:** Welcome screen with `>` prompt

### Step 4: Say Hello
```
> Hello! Can you hear me?
```

**Expected result:** Claude responds

### Step 5: Create a File
```
> Create a test.txt file. Write "Test successful!" inside.
```

**Expected result:** File creation approval request → Allow → File created

### Step 6: Verify
```
> Show me the contents of the file I just created
```

**Expected result:** "Test successful!" is displayed

### Step 7: Exit
Type `/exit` or press `Ctrl + C` twice.

---

## 🎯 Mini Quiz

Check your understanding!

### Q1. What is a terminal?
<details>
<summary>Show Answer</summary>

A window for communicating with your computer through text. You control the computer by typing commands with the keyboard instead of using a mouse.
</details>

### Q2. What's the difference between GUI and CLI?
<details>
<summary>Show Answer</summary>

- **GUI**: Graphical method of clicking with mouse (e.g., Windows Explorer)
- **CLI**: Method of typing text commands (e.g., Terminal)
</details>

### Q3. What's the biggest difference between Claude Pro subscription and API key?
<details>
<summary>Show Answer</summary>

- **Subscription**: Fixed monthly fee (like a buffet)
- **API Key**: Cost varies based on usage (like pay-per-use)
</details>

### Q4. What does `claude --version` check?
<details>
<summary>Show Answer</summary>

It checks if Claude Code is installed and what version it is.
</details>

---

## 📝 Practice Exercises

### Difficulty 1: Basic (Required)

1. Open terminal and verify installation with `claude --version`
2. Start Claude Code with `claude`
3. Say "Hello!"
4. Exit with `/exit`

### Difficulty 2: Intermediate

1. Create `greeting.txt` file in Claude Code
2. Put your name and today's date in the file
3. Read the file contents again to verify

### Difficulty 3: Challenge

1. Ask Claude to "Explain this folder structure"
2. Create new folder `my-project`
3. Create `README.md` file inside (with project description)

---

## 🏆 Challenge Tasks

**Additional exercises for those who want advanced learning.**

1. **Set up environment variable**: Try setting API key as a permanent environment variable
2. **Try different installation method**: Also try installing with npm or Homebrew
3. **Customize terminal**: Change terminal theme or font

---

## 🚨 Common Errors and Solutions

### Error 1: "command not found: claude"

**Shows on screen:**
```
zsh: command not found: claude
```
or
```
'claude' is not recognized as an internal or external command
```

**Cause:** Terminal wasn't restarted after installation

**Solution:**
1. Close terminal completely
2. Open new terminal
3. Try `claude --version` again

If still doesn't work:
```bash
# Mac/Linux
source ~/.bashrc
# or
source ~/.zshrc
```

### Error 2: Login Doesn't Work

**Shows on screen:**
Browser doesn't open, or terminal doesn't respond after login

**Solution:**
1. Check if you're logged into [claude.ai](https://claude.ai) in browser
2. Try a different browser
3. Turn off VPN if it's on and try again

### Error 3: "Permission denied"

**Shows on screen:**
```
Error: EACCES: permission denied
```

**Solution (Mac/Linux):**
```bash
sudo npm install -g @anthropic-ai/claude-code
```

**Solution (Windows):**
Run PowerShell as administrator.

### Error 4: npm Command Not Found

**Shows on screen:**
```
npm: command not found
```

**Cause:** Node.js is not installed

**Solution:** Use the installation script method (Method 1)

### Error 5: Installation Script Fails

**Shows on screen:**
```
curl: (7) Failed to connect
```
or
```
Unable to connect to host
```

**Cause:** Internet connection issue or firewall

**Solution:**
1. Check internet connection
2. Turn off VPN
3. Temporarily disable firewall
4. Try on a different network

### Error 6: "The term 'claude' is not recognized"

**Shows on screen (Windows):**
```
The term 'claude' is not recognized as the name of a cmdlet
```

**Cause:** Claude not registered in environment variable PATH

**Solution:**
1. Close and reopen PowerShell
2. Restart computer
3. Reinstall if still doesn't work

### Error 7: API Key Authentication Failed

**Shows on screen:**
```
Invalid API key
```
or
```
Authentication failed
```

**Cause:** API key is wrong or expired

**Solution:**
1. Check for spaces before/after API key
2. Issue a new API key
3. Verify environment variable is set correctly

### Error 8: Network Timeout

**Shows on screen:**
```
Request timeout
```
or
```
Network error
```

**Cause:** Network is unstable or server issue

**Solution:**
1. Try again after a few minutes
2. Switch to a different network
3. Check [status.anthropic.com](https://status.anthropic.com)

### Error 9: Subscription Expired

**Shows on screen:**
```
Subscription expired
```
or
```
Usage limit exceeded
```

**Cause:** Claude Pro subscription expired or limit exceeded

**Solution:**
1. Check subscription status at [claude.ai/settings](https://claude.ai/settings)
2. Wait until next month or upgrade plan
3. Switch to API key method

### Error 10: Windows Script Execution Policy Error

**Shows on screen:**
```
execution of scripts is disabled on this system
```

**Cause:** Windows security policy blocks script execution

**Solution:**
In administrator PowerShell:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### If Still Not Working

```bash
claude /doctor
```
This command diagnoses problems.

---

## ❌ Common Mistakes

### 1. Typing in the Wrong Place
Confirm you're typing in the terminal. Not in a text editor or browser. The terminal is a black/white window with text.

> 💡 **Beginner Tip**
>
> The terminal usually has white text on black background, or black text on white background.
> There's a `$`, `%`, or `>` symbol where you type commands.

### 2. Copying Commands Wrong
Make sure you've copied the entire line when copying installation commands. Sometimes the first or last characters get cut off.

> ⚠️ **Caution**
>
> Especially the `|` (pipe) symbol must not be missing.
> Copy the entire `curl -fsSL https://claude.ai/install.sh | bash`.

### 3. Not Waiting for Installation to Complete
Installation can take 1-2 minutes. Don't close the terminal while it's running. Wait until you see a "Installation complete" message.

### 4. Not Restarting Terminal After Installation
After installation, you need to close and reopen the terminal. That's how it recognizes that Claude Code exists.

> 🔥 **Pro Tip**
>
> On Mac, you can apply new settings without closing the terminal with `source ~/.zshrc`.

### 5. VPN or Firewall Issues
If login doesn't work, try turning off VPN. Some company firewalls may block the login process.

### 6. Using the Wrong Terminal
On Windows, PowerShell is recommended over Command Prompt (cmd). On Mac, use the default Terminal app.

### 7. Getting Confused with Multiple Terminal Windows
Having multiple terminal windows open can be confusing. Start with just one open for practice.

---

## 🆘 If It's Not Working?

| Symptom | Solution |
|---------|----------|
| Terminal won't open | Mac: Search "Terminal" in Spotlight / Windows: Right-click Start button → "PowerShell" |
| Installation command fails | Check internet connection, try different installation method |
| "claude: command not found" | Close and reopen terminal completely |
| Login page won't open | First go to [claude.ai](https://claude.ai) directly to verify login |
| API key error | Issue new key, remove leading/trailing spaces |
| Still stuck | Run `claude /doctor`, check [official documentation](https://docs.anthropic.com/claude-code) |

---

## 💡 How Professionals Use It

### Real Developer Usage of Claude Code

1. **Code review**: "Review this code. Let me know if there are problems."
2. **Refactoring**: "Clean up this function."
3. **Documentation**: "Add comments to this code."
4. **Writing tests**: "Create test code for this function."
5. **Debugging**: "Tell me why this error occurs."

### Why Terminal-Based?

Professional developers already use terminals a lot:
- Git commands
- npm/yarn package management
- Server access and management
- Build and deployment

When Claude Code runs in the terminal, it integrates naturally into existing workflows.

---

## ✅ Checklist

Before finishing your learning, confirm:

- [ ] I can open a terminal
- [ ] I can explain the difference between GUI and CLI
- [ ] Claude Code is installed (`claude --version` works)
- [ ] I successfully logged in
- [ ] I had my first conversation
- [ ] I know the difference between API key and subscription
- [ ] I can exit with `/exit`

---

## 🔑 Key Terms

Terms you learned in this chapter. Also refer to the [full glossary](../GLOSSARY.md).

| Term | Description |
|------|-------------|
| **Terminal** | A program for communicating with the computer through text |
| **CLI** | Command Line Interface |
| **GUI** | Graphical User Interface |
| **API Key** | Secret code used by programs to access services |
| **Environment Variable** | Settings stored in the operating system |

---

## 📚 Learn More

**Official Documentation:**
- [Claude Code Installation Guide](https://docs.anthropic.com/en/docs/claude-code/getting-started) - Official installation guide
- [Node.js Official Site](https://nodejs.org/) - Node.js installation

**Video Resources:**
- [Terminal Basics (YouTube)](https://www.youtube.com/results?search_query=terminal+basics+tutorial) - Terminal basics tutorial
- [iTerm2 Setup Guide (YouTube)](https://www.youtube.com/results?search_query=iterm2+setup+guide) - iTerm2 setup guide
- [Windows Terminal Tutorial (YouTube)](https://www.youtube.com/results?search_query=windows+terminal+tutorial) - Windows Terminal tutorial

**Reading Materials:**
- [The Art of Command Line (GitHub)](https://github.com/jlevy/the-art-of-command-line) - Master the command line
- [Oh My Zsh](https://ohmyz.sh/) - Zsh shell enhancement framework

**Terminal App Downloads:**
- [iTerm2](https://iterm2.com) - Mac terminal
- [Windows Terminal](https://aka.ms/terminal) - Windows terminal
- [Warp](https://www.warp.dev) - AI-based terminal

---

## ➡️ Next Steps

In the next chapter, we'll learn how to have deeper conversations with Claude Code. Understanding permission modes allows you to work safely and efficiently.

**Next Chapter Preview:**
- What are permission modes? (Plan, Normal, Accept Edits)
- Why is permission management important?
- How to switch modes

Proceed to [Chapter 03: Starting Your First Conversation](../Chapter03-First-Conversation/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
