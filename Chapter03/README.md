# Chapter 03: Your First Conversation

**English** | [한국어](./README.ko.md)

## What You'll Learn

- Basic conversation methods with Claude Code
- Understanding permission modes (important)
- Frequently used shortcuts

---

## Why Do You Need This?

You've installed Claude Code. Now what? This chapter teaches you how to actually talk to it and keep your files safe while doing so.

**Real-world scenarios:**
- You want to build a website, but you don't want Claude to accidentally delete important files
- You're exploring a new project and want to understand the code before making changes
- You need to make quick edits without clicking "approve" every single time

### Simple Analogy: Giving Instructions to a Helper

Imagine you have a very capable helper at your house. Before they start working, you'd want to set some ground rules:

- **"Just look around first"** = Plan Mode (read-only)
- **"Ask me before touching anything"** = Normal Mode (asks permission)
- **"Go ahead and clean, but ask before throwing things away"** = Accept Edits Mode (auto-approves safe actions)

Permission modes are these ground rules for Claude.

---

## Basic Conversation Method

Claude Code operates as a chat interface. Type input after the `>` prompt.

### Starting a Conversation

```bash
# Start Claude Code in terminal
claude
```

This screen appears:

```
>
```

Type input here.

### Examples

```
> Hello!
```

```
> What's the weather like?
```

```
> What's in this folder?
```

Any question works. Multiple languages including Korean are supported.

---

## Permission Modes (Essential)

Claude Code has three modes. **This is the most important concept.**

### Why Permission Management?

LLMs "interpret" your requests and execute them. They're usually accurate, but occasionally interpret differently than intended.

For example, "clean up this folder" could mean:
- Organizing files by category
- Deleting unnecessary files

The LLM selects the most appropriate interpretation from context, but cannot be 100% certain. That's why confirmation before important operations is safer.

### 1. Plan Mode (Safest)

```
Plan Mode
Read-only. Cannot modify anything.
```

**Use when:**
- First examining a project
- Understanding code behavior
- Planning before changes

**How to enter:**
```
> /plan
```
Or press `Shift + Tab` twice

### 2. Normal Mode (Default)

```
Normal Mode
Asks before making any changes.
```

**Use when:**
- First learning Claude Code
- Working with important files
- Verifying each change individually

When Claude attempts to modify a file:
```
Claude wants to edit src/app.js
[Allow] [Deny] [Allow all]
```
Select Allow to proceed.

### 3. Accept Edits Mode (Fast)

```
Accept Edits Mode
File edits auto-approved. Only asks for dangerous commands.
```

**Use when:**
- Already comfortable with Claude Code
- Working quickly
- Trusting Claude's decisions

### Mode Switching

| Method | Description |
|--------|-------------|
| `Shift + Tab` | Cycle modes (Normal → Accept Edits → Plan → Normal) |
| `/plan` | Switch to Plan mode |
| `claude --permission-mode plan` | Start in Plan mode |

### Pro Tip

> "Almost always start in Plan mode."

For new work, press `Shift + Tab` twice to enter Plan mode. LLMs generate better code when they understand the full context first. Analyzing code and planning before execution reduces situations where "it interpreted differently than intended, now I need to redo it."

---

## Frequently Used Shortcuts

No need to memorize. Reference when needed.

| Shortcut | Function |
|----------|----------|
| `Shift + Tab` | Change permission mode |
| `Ctrl + C` | Cancel current operation |
| `Esc Esc` | Undo (revert to previous state) |
| `Ctrl + L` | Clear screen (preserves conversation) |
| `Shift + Enter` | Line break (for long messages) |

### Note for Mac Users

To use `Alt` key combinations on Mac, enable "Option as Meta" in terminal settings.

---

## Slash Commands

Special commands starting with `/`.

| Command | Description |
|---------|-------------|
| `/help` | View all commands |
| `/clear` | Clear conversation history |
| `/exit` | Exit Claude Code |
| `/cost` | View usage cost |
| `/model` | View/change model |

### /clear vs Ctrl+L

- `/clear`: Deletes conversation. Claude forgets previous context.
- `Ctrl+L`: Clears screen only. Claude retains memory.

---

## Continuing Sessions

Previous conversations can be continued after closing Claude Code.

```bash
# Continue last conversation
claude --continue

# Select from past conversations
claude --resume

# Name current session (for easier retrieval)
/rename my-project-work
```

---

## Practice

1. Start Claude Code: `claude`
2. Switch to Plan mode: `Shift + Tab` twice (or `/plan`)
3. Examine the folder:
   ```
   > What's in this folder? Explain.
   ```
4. Switch to Normal mode: `Shift + Tab`
5. Create a simple file:
   ```
   > Create a file called test.txt. Write "Hello World" in it.
   ```
6. When prompted, select `y` or `Allow`
7. Verify the result:
   ```
   > Show me the contents of the file you just created.
   ```

---

## Advanced: Using Claude Code Effectively

### Think → Plan → Execute

When starting with an AI tool, the instinct is to type immediately. But pausing to clarify "what am I trying to build?" leads to much better results.

Plan Mode helps with this. Claude analyzes the code and suggests an approach, and you can verify the plan before proceeding. Getting the direction right early prevents "do I need to redo this?" situations later.

### Choosing the Right Permission Mode

If you are unsure which mode to use, consider two factors:

**1. Risk level of the task**
- Creating new files → Low
- Modifying existing code → Medium
- Config files or database operations → High

**2. Can it be undone?**
- Tracked by Git → Easy to revert
- External service calls → Hard to undo

| Situation | Recommended Mode |
|-----------|-----------------|
| Low risk + Easy to revert | Accept Edits |
| Medium risk or unfamiliar task | Normal |
| High risk or hard to undo | Plan |

### Clear Requests Lead to Clear Results

Saying "make a website" forces Claude to choose from countless possibilities. But "make a portfolio page with intro, 3 projects, and contact sections in dark theme" lets Claude understand exactly what you want.

When making requests, consider:
- **What**: What result do you want
- **Where**: Which file/location
- **How**: Any specific approach you prefer
- **Why**: Context enables better judgment

### Design Decisions Together

For complex features, rather than jumping into implementation, discuss the structure with Claude in Plan Mode first.

```
> /plan
> I want to add user authentication.
> What options are there, and what fits this project?
```

When Claude explains options like JWT, sessions, or OAuth, you choose and then proceed with implementation. This prevents "built it but it doesn't match the requirements" situations.

---

## Try It Yourself

Let's practice what you learned. Open Claude Code and follow along:

### Exercise 1: Mode Switching
```
1. Start Claude Code: claude
2. Check current mode (shown in the interface)
3. Press Shift + Tab to switch modes
4. Notice how the mode indicator changes
5. Press Shift + Tab again until you're in Plan mode
```

### Exercise 2: First Real Conversation
```
> What's in this folder?
```
Claude will show you the files. In Plan mode, it can only read, not change anything.

### Exercise 3: Create Something Simple
```
1. Press Shift + Tab to switch to Normal mode
2. Type: Create a file called hello.txt with "Hello World" inside
3. When Claude asks for permission, click Allow
4. Type: Show me what's in hello.txt
```

Congratulations! You just created your first file with Claude Code.

---

## Common Mistakes

### 1. Forgetting Which Mode You're In
Always check the mode indicator before making requests. If you're in Plan mode and ask Claude to create a file, it will explain what it would do but won't actually do it.

### 2. Clicking "Deny" by Accident
If you accidentally deny a change, just ask Claude again. "Do that again" or "Yes, please create that file" works fine.

### 3. Not Using Plan Mode for Exploration
When you first open a project, resist the urge to immediately start changing things. Use Plan mode to understand the structure first.

### 4. Typing Commands Instead of Natural Language
You don't need to type terminal commands. Instead of `ls -la`, just say "show me the files in this folder." Claude understands both, but natural language is easier.

### 5. Not Reading Claude's Explanations
Claude often explains what it's about to do and why. Reading these explanations helps you learn and catch potential issues.

---

## If It Doesn't Work...

**Mode won't switch?**
Make sure you're pressing Shift + Tab (not just Tab). Some terminals require specific key combinations.

**Claude isn't responding?**
- Check your internet connection
- Try typing `/help` to see if Claude is active
- Exit with `/exit` and restart

**Permission dialogs not appearing?**
You might be in Plan mode (read-only) or Accept Edits mode (auto-approve). Press `Shift + Tab` to switch to Normal mode.

**Claude did something you didn't want?**
- Press `Esc Esc` (Escape twice) to undo the last change
- Or ask Claude: "Undo that last change"

**Confused about what Claude is doing?**
Just ask! "What did you just do?" or "Explain that change" works great.

---

## Summary

Covered in this chapter:
- [x] Conversation methods with Claude Code
- [x] Three permission modes (Plan, Normal, Accept Edits)
- [x] Frequently used shortcuts
- [x] Slash commands

**Key takeaway**: Start new work in Plan mode.

The next chapter covers file operations in detail.

Proceed to [Chapter 04: Working with Files](../Chapter04/README.md).
