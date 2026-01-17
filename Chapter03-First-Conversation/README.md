# Chapter 03: Starting Your First Conversation

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Learn the basic way to converse with Claude Code
- Understand permission modes (very important!)
- Know frequently used shortcuts and commands
- Learn effective questioning methods

---

## ⏱️ Estimated Time

- Reading: **25 minutes**
- Hands-on: **20 minutes**

---

## 📋 What You Need

- Claude Code installation complete (Chapter 02)
- Terminal access

---

## 🔗 Previous Chapter Review

In [Chapter 02](../Chapter02-Installation/README.md), we opened the terminal, installed Claude Code, and had our first conversation. Now it's time to learn how to have deeper conversations.

---

## Why Is This Necessary?

You've installed Claude Code. Now what? In this chapter, you'll learn how to actually converse and how to safely protect your files.

**Real situations:**
- Want to make a website, but Claude shouldn't accidentally delete important files
- Want to understand code before making changes while exploring a new project
- Want to make quick edits without clicking "approve" every time

### Easy Analogy: Giving Instructions to a Helper

Imagine you have a capable helper at home. Before they start working, you'd want to set some rules:

- **"Just look around first"** = Plan mode (read-only)
- **"Ask me before touching anything"** = Normal mode (ask for permission)
- **"You can clean without asking, but ask before throwing anything away"** = Accept Edits mode (auto-approve safe actions)

Permission modes are these kinds of rules you set for Claude.

---

## Basic Conversation Method

Claude Code converses like a chat. Just type after the `>`.

### Starting a Conversation

```bash
# Start Claude Code in terminal
claude
```

When you start, this screen appears:

```
╭────────────────────────────────────────╮
│ Welcome to Claude Code!                │
│                                        │
│ /help for commands, Shift+Tab for mode │
╰────────────────────────────────────────╯

>
```

Just type here.

### Basic Examples

```
> Hello!
```

```
> How's the weather today?
```

```
> What's in this folder?
```

You can ask anything. It understands English well.

> 💡 **Beginner Tip**
>
> Claude Code can handle everyday conversation, but it's optimized for coding-related tasks.
> You'll get more useful answers with specific requests like "Explain this project" rather than just "Hello."

---

## Permission Modes (⭐ Must Understand)

Claude Code has three modes. **This is the most important thing.**

### Why Is Permission Management Necessary?

LLMs "interpret" your requests to execute them. Most of the time it's accurate, but sometimes it may interpret differently than intended.

For example, if you say "Clean up this folder":
- It could mean organizing files by category
- It could mean deleting unnecessary files

That's why it's safer to get confirmation before important actions.

### Design Philosophy of Permission Modes

Claude Code's permission system follows these principles:

1. **Safety first**: By default, asks for confirmation on all changes
2. **Gradual trust**: Users can adjust the trust level
3. **Transparency**: Always shows what it's about to do
4. **Reversible**: Can recover even from mistakes

> 🔥 **Pro Tip**
>
> Permission modes are not about "trusting Claude."
> It's a safety mechanism to **confirm whether your request is clear**.
> Vague request → Possibility of unexpected results

### 🔵 1. Plan Mode (Safest)

```
📋 Plan Mode
Read-only. Cannot change anything.
```

**Features:**
- Can only read files
- Cannot create/modify/delete files
- Cannot execute terminal commands (read commands are okay)
- Optimal for planning

**When to use:**
- When first looking at a project
- When you want to understand how code works
- When making a plan before changes
- When only doing code review or analysis
- When you're afraid of making mistakes

**How to enter:**
```
> /plan
```
Or press `Shift + Tab` twice

**Real conversation example:**
```
Plan Mode 📋

> Explain this project structure

This project is a React-based web application.

Main structure:
├── src/
│   ├── components/  # UI components
│   ├── pages/       # Page components
│   └── utils/       # Utility functions
├── public/          # Static files
└── package.json     # Dependency list

Key files:
- src/App.js: Main app component
- src/index.js: Entry point
```

> 💡 **Beginner Tip**
>
> Even if you say "Create a file" in Plan mode, it won't be created.
> Claude will only tell you a plan like "It would be good to make it like this."
> To actually create it, you need to change modes.

### 🟡 2. Normal Mode (Default)

```
🔒 Normal Mode
Asks for confirmation before every change.
```

**Features:**
- Can read/write files (after confirmation)
- Can execute terminal commands (after confirmation)
- Requests approval for all changes
- Balance of safety and efficiency

**When to use:**
- When first learning Claude Code
- When handling important files
- When you want to verify changes one by one
- When working in production environment

When Claude tries to modify a file:
```
Claude wants to edit src/app.js
[Allow] [Deny] [Allow all]
```
It asks for confirmation like this. Select Allow and it's modified.

**Approval option explanations:**
| Option | Meaning | When to use? |
|--------|---------|--------------|
| `y` / Allow | Allow just this time | When you've checked the change and it's okay |
| `n` / Deny | Reject | When it's an unwanted change |
| `a` / Allow all | Allow all similar actions | When there are many repetitive tasks |

**Real conversation example:**
```
Normal Mode 🔒

> Create an index.html file

Creating file: index.html
──────────────────────────────
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Page</title>
</head>
<body>
    <h1>Hello!</h1>
</body>
</html>
──────────────────────────────

Allow this action? [y/n/a]
> y

✓ Created index.html
```

### 🟢 3. Accept Edits Mode (Fast)

```
⚡ Accept Edits Mode
File edits auto-approved. Only dangerous commands ask for confirmation.
```

**Features:**
- File read/write auto-approved
- Only dangerous commands ask for confirmation (rm, git push, etc.)
- Fastest work speed
- Recommended after building trust

**When to use:**
- When you're already comfortable
- When you want to work quickly
- When you can trust Claude
- For personal projects or experimental environments

**Actions that are auto-approved:**
- File creation
- File modification
- File reading
- Safe terminal commands (ls, cat, npm install, etc.)

**Actions that still ask for confirmation:**
- File deletion (rm)
- git push
- System commands
- sudo commands

**Real conversation example:**
```
Accept Edits Mode ⚡

> Create a login form component

✓ Created src/components/LoginForm.js
✓ Updated src/App.js (added import)
✓ Created src/styles/LoginForm.css

I've created the login form component. Features:
- Email/password input fields
- Validation
- Login button

Would you like to test it?
```

> ⚠️ **Caution**
>
> Even in Accept Edits mode, mistakes can happen.
> Using Git allows you to return to previous states anytime.
> Normal mode is recommended for important projects.

### Switching Modes

| Method | Description |
|--------|-------------|
| `Shift + Tab` | Cycle modes (Normal → Accept Edits → Plan → Normal) |
| `/plan` | Switch to Plan mode |
| `claude --permission-mode plan` | Start in Plan mode |

### Mode Comparison Table

| Feature | Plan 📋 | Normal 🔒 | Accept Edits ⚡ |
|---------|---------|-----------|-----------------|
| Read files | ✅ | ✅ | ✅ |
| Create/modify files | ❌ | ✅ (confirm) | ✅ (auto) |
| Delete files | ❌ | ✅ (confirm) | ✅ (confirm) |
| Safe commands | ✅ | ✅ (confirm) | ✅ (auto) |
| Dangerous commands | ❌ | ✅ (confirm) | ✅ (confirm) |
| Work speed | Slow | Medium | Fast |
| Safety | Highest | High | Medium |

### 🌟 Mode Selection Guide

```
┌─────────────────────────────────────────────────────┐
│           When Should I Use Which Mode?              │
├─────────────────────────────────────────────────────┤
│                                                     │
│   🆕 When first opening a project                   │
│   └─→ Plan mode to understand structure             │
│                                                     │
│   📝 When code modification is needed               │
│   └─→ Normal mode to verify one by one              │
│                                                     │
│   🚀 When you need to work fast                     │
│   └─→ Accept Edits mode (after getting comfortable) │
│                                                     │
│   🔍 When only doing code review/analysis           │
│   └─→ Stay in Plan mode                             │
│                                                     │
│   🏢 Company projects                               │
│   └─→ Normal mode recommended                       │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 🌟 Pro Tip

> **"Almost always start in Plan mode."**

When doing new work, press `Shift + Tab` twice to first enter Plan mode. Analyze the code and make a plan before executing, and you'll reduce situations where you have to redo things because it was "interpreted differently than intended."

---

## Frequently Used Shortcuts

No need to memorize. Just check when needed.

| Shortcut | Function |
|----------|----------|
| `Shift + Tab` | Switch permission mode |
| `Ctrl + C` | Cancel current task |
| `Esc Esc` | Undo (return to last state) |
| `Ctrl + L` | Clear screen (conversation preserved) |
| `Shift + Enter` | New line (when writing long messages) |

> 💡 **Beginner Tip**
>
> The most important ones are `Shift + Tab` (mode switch) and `Esc Esc` (undo).
> Just remember these two!

### Shortcut Details

#### Shift + Tab - Mode Switch
```
Normal 🔒 → Accept Edits ⚡ → Plan 📋 → Normal 🔒 ...
```
Cycles in order. You can check current mode at the top of the screen.

#### Esc Esc - Undo
Cancels the action Claude just did.
```
> Change the file contents
✓ Updated config.js

> (Esc Esc)
↩ Reverted changes to config.js
```

> 🔥 **Pro Tip**
>
> `Esc Esc` only undoes the last action.
> Use Git to undo multiple actions.

#### Ctrl + C - Cancel Task
Use when you want to stop Claude while it's generating a response.
```
> Write long code

Claude: Sure, I'll write that...
[=========>            ] 50%

(Ctrl + C)

Task cancelled.
```

#### Shift + Enter - New Line
You can write long requests across multiple lines.
```
> Create a website (Shift+Enter)
> - Put a logo in the header (Shift+Enter)
> - Divide into 3 sections (Shift+Enter)
> - Support dark mode (Enter to send)
```

### Mac Users Note

To use `Alt` key combinations on Mac, you need to enable "Option as Meta" in terminal settings.

**How to set up:**
1. Terminal > Preferences > Profiles
2. Keyboard tab
3. Check "Use Option as Meta key"

---

## Slash Commands

Special commands that start with `/`.

| Command | Description | Usage Example |
|---------|-------------|---------------|
| `/help` | View all commands | When you can't remember a command |
| `/clear` | Delete conversation history | When starting a new topic |
| `/exit` | Exit Claude Code | When work is done |
| `/cost` | View usage cost so far | API key users |
| `/model` | View/change model in use | When you want to use a different model |
| `/plan` | Switch to Plan mode | Read-only mode |

### /clear vs Ctrl+L

These look similar but are completely different!

| Category | `/clear` | `Ctrl+L` |
|----------|----------|----------|
| Screen | Cleared | Cleared |
| Conversation history | **Deleted** | Preserved |
| Claude's memory | **Forgets** | Remembers |
| Purpose | Start new topic | Just clean the screen |

**Example:**
```
> Create a button component
✓ Created Button.js

> /clear

> Modify the button I made earlier
Which button are you referring to?
(Claude forgot the previous conversation)
```

```
> Create a button component
✓ Created Button.js

> (Ctrl + L)

(Screen becomes clean)

> Modify the button I made earlier
Sure, I'll modify Button.js.
(Claude remembers)
```

> 💡 **Beginner Tip**
>
> If confused, just use `Ctrl+L`.
> Use `/clear` only when you really want to start from scratch.

---

## Resuming Sessions

You can continue previous conversations even after exiting and restarting Claude Code.

```bash
# Continue last conversation
claude --continue

# Choose from past conversations
claude --resume
```

**--continue usage example:**
```bash
# Yesterday's work
> Create a login page
✓ Created login.html
> /exit

# Continue today
$ claude --continue
> Add CSS to the login page I made yesterday
Sure, I'll add styles to login.html...
```

**--resume usage example:**
```bash
$ claude --resume

Past conversation list:
1. 2024-01-17 Login page work
2. 2024-01-16 API server build
3. 2024-01-15 Database setup

Select (1-3): 2

(2024-01-16 conversation restored)
```

---

## Effective Conversation Patterns

Methods for getting better results when talking with Claude Code.

### Pattern 1: Provide Context

**Bad example:**
```
> Create a button
```

**Good example:**
```
> This is a React project. Create a blue submit button.
> When clicked, send form data to API.
```

### Pattern 2: Request Step by Step

**Bad example:**
```
> Create a shopping mall
```

**Good example:**
```
> I'm going to build a shopping mall. First create the product list page.
> Product data can be hardcoded for now.
```

### Pattern 3: Specify Result Format

**Bad example:**
```
> Process user information
```

**Good example:**
```
> Create a function that processes user information.
> Input: { name: string, email: string }
> Output: { valid: boolean, errors: string[] }
```

### Pattern 4: State Constraints

```
> Create a login form.
> But with pure HTML/CSS/JS only, no external libraries.
> Include validation too.
```

### Pattern 5: Show Examples

```
> Parse API response in this format:
>
> Input: { "user": { "name": "John Doe" } }
> Output: "John Doe"
```

### Pattern 6: Ask Why

```
> Why did you recommend useReducer instead of useState?
```

### Pattern 7: Request Alternatives

```
> Is there another way besides this?
> Do it in a more performant way.
```

### Pattern 8: Request Review

```
> Check if there are any problems with the code I just made.
> Check for security vulnerabilities too.
```

### Pattern 9: Request Explanation

```
> Explain the code you just made so a beginner can understand.
> Add comments to each line too.
```

### Pattern 10: Request Comparison

```
> Compare the pros and cons of this method vs that method.
> Tell me when to use which.
```

---

## Good Questions vs Bad Questions

### Bad Question Examples

| Question | Problem |
|----------|---------|
| "Create a website" | Too vague. What kind of website? |
| "Fix the code" | Unclear what and how to fix |
| "It's not working" | No explanation of what's not working |
| "Make it the best" | Unclear criteria |
| "There's an error" | No information about what error |

### Good Question Examples

| Question | Why It's Good |
|----------|---------------|
| "Create a portfolio website. With intro, 3 projects, and contact sections." | Specific requirements |
| "@app.js Fix the null error on line 35" | File, location, and error type specified |
| "I get a 'Cannot read property' error. I'll show you the console log." | Shares error message |
| "Make it responsive so it looks good on mobile too" | Clear criteria |
| "Loading takes over 3 seconds. I want to reduce it to under 1 second." | Measurable goal |

### Question Improvement Example

**Before:**
```
> There's a bug
```

**After:**
```
> @src/api.js There's an error when loading user data.
> Console shows "TypeError: Cannot read property 'name' of undefined".
> It seems to happen when the user isn't logged in.
```

> 🔥 **Pro Tip**
>
> Asking good questions to Claude is like entering good search terms in a search engine.
> The more specific, the more accurate the answer.

---

## 🔨 Try It Yourself

### Exercise 1: Switching Modes

1. Start Claude Code: `claude`
2. Check current mode (displayed at top of screen)
3. Press `Shift + Tab` to switch mode
4. Confirm mode indicator changes
5. Press `Shift + Tab` again until you reach Plan mode

**Expected result:** Mode changes Normal → Accept Edits → Plan

### Exercise 2: Exploring in Plan Mode

```
> What's in this folder? Explain it.
```

**Expected result:** Claude shows file list and explains. Nothing is modified.

### Exercise 3: Creating a File in Normal Mode

1. Press `Shift + Tab` to switch to Normal mode
2. Type:
```
> Create hello.txt file. Write "Hello" inside.
```
3. Select `y` or `Allow` when approval is requested
4. Verify:
```
> Show me the contents of the file I just made.
```

**Expected result:** File is created, contents confirmed as "Hello"

### Exercise 4: Undo Practice

1. Make a file modification request:
```
> Change hello.txt contents to "Hello World"
```
2. Confirm change after approval
3. Press `Esc Esc` to undo
4. Check if file contents returned to original

### Exercise 5: Practice Various Question Patterns

```
> Explain this project structure (Plan mode)

> Create a simple calculator function (Normal mode)

> Explain the code you just made

> Is there another way?

> Add error handling too
```

---

## 🖼️ Expected Screens

### Mode Indicator

```
╭─ Plan Mode ────────────────────────────╮
│ 📋 Read-only - No changes allowed      │
╰────────────────────────────────────────╯

>
```

### Approval Request (Normal Mode)

```
Claude wants to create file: hello.txt

────────────────────────────────────────
Hello
────────────────────────────────────────

Allow this action? [y/n/a]
```

- `y`: Allow just this time
- `n`: Deny
- `a`: Allow all similar actions from now on

### File Modification Diff Display

```
Claude wants to edit hello.txt:

────────────────────────────────────────
- Hello
+ Hello World
────────────────────────────────────────

Allow this action? [y/n/a]
```

---

## 🎯 Mini Quiz

### Q1. Can you create files in Plan mode?
<details>
<summary>Show Answer</summary>

No. Plan mode is read-only.
To create files, you need to switch to Normal or Accept Edits mode.
</details>

### Q2. What's the biggest difference between Normal mode and Accept Edits mode?
<details>
<summary>Show Answer</summary>

- **Normal**: Asks for confirmation on all changes
- **Accept Edits**: File modifications auto-approved, only dangerous actions ask for confirmation
</details>

### Q3. What does Esc Esc shortcut do?
<details>
<summary>Show Answer</summary>

Undoes the action Claude just did.
</details>

### Q4. What's the difference between /clear and Ctrl+L?
<details>
<summary>Show Answer</summary>

- `/clear`: Conversation history is deleted and Claude forgets previous conversation
- `Ctrl+L`: Only the screen is cleared and conversation history is preserved
</details>

### Q5. What mode should you start with when opening a new project for the first time?
<details>
<summary>Show Answer</summary>

It's good to start in Plan mode.
First understand the project structure, make a plan, then start modifications - this is safer.
</details>

---

## 📝 Practice Exercises

### Difficulty 1: Basic (Required)

1. Start Claude Code and experience all three modes
2. Say "Create a file" in each mode and observe differences
3. Practice switching modes 5 times with `Shift + Tab`

### Difficulty 2: Intermediate

1. Get current folder structure analysis in Plan mode
2. Switch to Normal mode and create `practice.txt` file
3. Modify file contents and undo with `Esc Esc`
4. Experience the difference between `/clear` and `Ctrl+L` directly

### Difficulty 3: Challenge

1. Create HTML, CSS, JS files each in Normal mode
2. Request modifications so each file connects to each other
3. Switch to Accept Edits mode and quickly implement additional features
4. Recover from mistakes during work using undo

---

## 🏆 Challenge Tasks

1. **Continue conversation**: Try continuing previous conversation with `claude --continue`
2. **Custom start**: Set up Plan mode start with `claude --permission-mode plan`
3. **Efficient workflow**: Complete work following Plan → Normal → Accept Edits sequence

---

## Advanced: Effective Claude Code Usage

### Think → Plan → Execute

When first encountering an AI tool, you want to input something right away. But pausing for a moment to first organize "What am I trying to build?" makes results much better.

Plan Mode helps this process. When Claude analyzes code and suggests approaches, you can verify if that plan is correct before proceeding.

```
┌──────────────────────────────────────────────────┐
│              Effective Workflow                    │
├──────────────────────────────────────────────────┤
│                                                  │
│  1️⃣ Plan mode: "Analyze this project"            │
│     └─→ Understand structure, make plan          │
│                                                  │
│  2️⃣ Normal mode: "Modify this part"              │
│     └─→ Proceed while confirming important       │
│         changes                                  │
│                                                  │
│  3️⃣ Accept Edits mode: "Do the rest quickly"    │
│     └─→ Handle repetitive tasks quickly          │
│                                                  │
│  4️⃣ Plan mode: "Review what we've done so far"  │
│     └─→ Final verification                       │
│                                                  │
└──────────────────────────────────────────────────┘
```

### Permission Mode Selection Criteria

| Situation | Recommended Mode | Reason |
|-----------|------------------|--------|
| Low risk + Easy to undo | Accept Edits | Speed priority |
| Medium risk or first time | Normal | Proceed while confirming |
| High risk or hard to undo | Plan | Plan first |
| Code review | Plan | Only need reading |
| Learning purpose | Normal | Understand the process |
| Before production deployment | Plan → Normal | Be careful |

### Clear Request Makes Clear Results

**Vague request:**
```
> Create a website
```

**Clear request:**
```
> Create a portfolio page.
> Structure with intro, 3 projects, and contact sections, with a dark theme.
> Use React and Tailwind CSS.
```

### Iterative Pattern: Request → Verify → Improve

```
1. Request: "Create a login form"
2. Verify: "Add email validation too"
3. Improve: "Add password strength indicator too"
4. Complete: "Good, now clean up the styling"
```

---

## 🚨 Common Errors and Solutions

### Error 1: File Not Created in Plan Mode

**Situation:** Said "Create a file" but it only explains and doesn't create

**Cause:** Plan mode is read-only

**Solution:** Switch to Normal or Accept Edits mode with `Shift + Tab`

**Conversation example:**
```
Plan Mode 📋

> Create index.html

In Plan mode, files cannot be created.
It would be good to create with this structure:
- DOCTYPE declaration
- head section (meta, title)
- body section (content)

To actually create it, please switch to Normal mode (Shift+Tab).
```

### Error 2: Accidentally Modified Wrong File

**Situation:** Unwanted file was modified

**Solution:** `Esc Esc` (press Escape twice) to undo. Or:
```
> Cancel what you just did
```

### Error 3: Mode Switch Doesn't Work

**Situation:** `Shift + Tab` doesn't respond

**Solution:**
- Confirm you're holding `Shift` and pressing `Tab`
- Some terminals may have different key settings. Switch directly with `/plan` command

### Error 4: Claude Doesn't Remember Previous Conversation

**Situation:** Said "the file I made earlier" but Claude doesn't know

**Cause:** Used `/clear` or started a new session

**Solution:**
```bash
# Continue previous conversation
claude --continue
```

### Error 5: Too Many Approval Dialogs

**Situation:** Creating one file requires multiple approvals

**Solution:**
- Select `a` (Allow all) to auto-approve similar actions
- Switch to Accept Edits mode

---

## ❌ Common Mistakes

### 1. Forgetting Which Mode You're In
Always check the mode indicator before making a request.

> 💡 **Beginner Tip**
>
> Current mode is always displayed at the top of the screen.
> Plan 📋 / Normal 🔒 / Accept Edits ⚡

### 2. Accidentally Clicking "Deny"
If you accidentally denied, just make the request again. Just say "Do it again."

### 3. Not Using Plan Mode When Exploring
When first opening a project, resist the urge to modify right away. First understand the structure in Plan mode.

### 4. Typing Commands Instead of Natural Language
No need to type terminal commands. Instead of `ls -la`, say "Show me the files in this folder."

### 5. Not Reading Claude's Explanations
Claude often explains what it's going to do and why. Reading these explanations helps you learn and catch problems early.

### 6. Making Vague Requests
"Fix it" is worse than "Fix the null error." Be specific.

### 7. Requesting Too Much at Once
"Create a shopping mall" is worse than requesting step by step.

---

## 🆘 If It's Not Working?

| Symptom | Solution |
|---------|----------|
| Mode doesn't change | Check `Shift + Tab`, or use `/plan` command |
| Claude doesn't respond | Check internet connection, type `/help`, `/exit` and restart |
| Approval dialog doesn't appear | Might be in Plan or Accept Edits mode. Switch to Normal mode with `Shift + Tab` |
| Does unwanted action | Undo with `Esc Esc` |
| Doesn't remember previous conversation | Restore previous session with `claude --continue` |

---

## 💡 How Professionals Use It

### Code Review Scenario

```
Plan Mode 📋

> @src/auth.js Does this code have security vulnerabilities?

Security review results:

1. ⚠️ Password stored in plain text (line 45)
   → Need to hash with bcrypt

2. ⚠️ SQL injection possible (line 78)
   → Recommend using prepared statement

3. ✅ CORS settings appropriate

Would you like to fix these?
```

### Quick Prototyping

```
Accept Edits Mode ⚡

> Create a chat UI prototype

✓ Created components/Chat.js
✓ Created components/Message.js
✓ Created components/InputBox.js
✓ Updated App.js
✓ Created styles/chat.css

I've created a basic chat UI.
Check it out at localhost:3000.
```

### Careful Deployment Work

```
Normal Mode 🔒

> Prepare for production deployment

I'll perform the following tasks:
1. Check environment variables
2. Build optimization
3. Run tests
4. Execute deployment script

Would you like confirmation at each step? [y/n]
```

---

## ✅ Checklist

Before finishing your learning, confirm:

- [ ] I can converse with Claude Code
- [ ] I can explain the three permission modes (Plan, Normal, Accept Edits)
- [ ] I can switch modes with `Shift + Tab`
- [ ] I know a few slash commands (/help, /clear, /exit)
- [ ] I can undo with `Esc Esc`
- [ ] I know the difference between good and bad questions
- [ ] I know the difference between `/clear` and `Ctrl+L`

**Key point**: Start new work in Plan mode.

---

## 🔑 Key Terms

| Term | Description |
|------|-------------|
| **Plan mode** | Read-only mode. Cannot modify files |
| **Normal mode** | Default mode. All changes require confirmation |
| **Accept Edits mode** | File modifications auto-approved. Only dangerous actions confirm |
| **Slash commands** | Special commands starting with `/` |
| **Session** | One conversation unit with Claude Code |

Also refer to the [full glossary](../GLOSSARY.md).

---

## 📚 Learn More

**Official Documentation:**
- [Claude Code Usage Guide](https://docs.anthropic.com/en/docs/claude-code/using-claude-code) - Conversation and command guide
- [Claude Code Permission Modes](https://docs.anthropic.com/en/docs/claude-code/security) - Security and permission settings

**Video Resources:**
- [Claude Code Tutorial (YouTube)](https://www.youtube.com/results?search_query=claude+code+tutorial) - Claude Code usage tutorial
- [AI Coding Best Practices (YouTube)](https://www.youtube.com/results?search_query=ai+coding+best+practices) - AI coding best practices

**Reading Materials:**
- [Prompt Engineering Guide](https://www.promptingguide.ai/) - Effective prompt writing methods
- [Learn Prompting](https://learnprompting.org/) - Prompting learning resources

**Community:**
- [Reddit r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) - Claude user community
- [Claude Code Discussions](https://github.com/anthropics/claude-code/discussions) - Official GitHub discussions

---

## ➡️ Next Steps

In the next chapter, we'll learn how to handle files in detail.

**Next Chapter Preview:**
- Referencing files with `@`
- File creation, modification, deletion
- Handling multiple files simultaneously
- Using special prefixes (`@`, `!`, `#`)

Proceed to [Chapter 04: Reading and Writing Files](../Chapter04-Working-with-Files/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
