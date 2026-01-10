# Vibecoding Curriculum

> A systematic learning curriculum for AI-native development

**Built with [Claude Code](https://github.com/anthropics/claude-code)** | **English** | [한국어](./README.ko.md)

---

## What is Vibecoding?

**Vibecoding** is a new paradigm of developing with AI.

- Instead of writing code line by line, **express your intent**
- Instead of debugging alone, **solve problems through conversation**
- Use AI as your **pair programmer**
- **Develop faster** while maintaining quality

This curriculum teaches vibecoding through Claude Code, Anthropic's official CLI tool.

<details>
<summary><strong>Philosophy of Vibecoding</strong></summary>

### Old Way vs Vibecoding

**Before**, you had to write code line by line yourself. Memorize syntax, fix errors, search endlessly... Learning took years.

**Now it's different.** Tell the AI "make this for me" and the AI writes the code.

### Comparison: Making a Webpage

**Old way:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <style>
        body { font-family: sans-serif; }
        h1 { color: blue; }
    </style>
</head>
<body>
    <h1>Hello</h1>
</body>
</html>
```
You had to memorize and write all of this yourself.

**Vibecoding:**
```
"Make a webpage with a blue heading that says 'Hello'"
```
Say this and the AI generates the code above.

### Why "Vibe"?

"Vibe" means feeling or atmosphere. Even without knowing exact syntax, if you convey the **"feel" of what you want**, the AI understands and implements it.

Traditional coding required you to know everything yourself. Vibecoding means **creating together** with an AI partner.

</details>

---

## Audience

- **Complete beginners** learning to code for the first time
- **Beginning developers** who want to leverage AI tools
- **Working developers** who want to increase productivity

No coding experience required. We proceed step by step from the beginning.

---

## Learning Outcomes

After completing 20 chapters, you will be able to:

- **Build a portfolio website** and deploy it
- **Create simple apps** (TODO app, games, etc.)
- **Build automation tools** (file organizer, data processor)
- **Implement AI services** (document summarizer, translator)

---

## Curriculum Structure

### Part 1: Getting Started (Chapter 01-05)

Learn basic concepts and how to use Claude Code.

| Chapter | Topic | Description | Link |
|---------|-------|-------------|------|
| 01 | What is AI Coding? | Vibecoding concept, Claude Code intro | [EN](./Chapter01/README.md) / [KO](./Chapter01/README.ko.md) |
| 02 | Installation | Terminal, installation, login | [EN](./Chapter02/README.md) / [KO](./Chapter02/README.ko.md) |
| 03 | Your First Conversation | Permission modes, shortcuts, slash commands | [EN](./Chapter03/README.md) / [KO](./Chapter03/README.ko.md) |
| 04 | Working with Files | @mentions, reading/writing files | [EN](./Chapter04/README.md) / [KO](./Chapter04/README.ko.md) |
| 05 | Terminal Commands | Running commands, running projects | [EN](./Chapter05/README.md) / [KO](./Chapter05/README.ko.md) |

### Part 2: Core Features (Chapter 06-10)

Master the core features of Claude Code.

| Chapter | Topic | Description | Link |
|---------|-------|-------------|------|
| 06 | Effective Prompting | Good prompts, Plan mode | [EN](./Chapter06/README.md) / [KO](./Chapter06/README.ko.md) |
| 07 | Exploring Code | Glob, Grep, understanding projects | [EN](./Chapter07/README.md) / [KO](./Chapter07/README.ko.md) |
| 08 | Editing Code | Explore → Plan → Execute workflow | [EN](./Chapter08/README.md) / [KO](./Chapter08/README.ko.md) |
| 09 | Git Basics | Commits, branches, PRs | [EN](./Chapter09/README.md) / [KO](./Chapter09/README.ko.md) |
| 10 | Project Memory | CLAUDE.md, project settings | [EN](./Chapter10/README.md) / [KO](./Chapter10/README.ko.md) |

### Part 3: Practical Projects I (Chapter 11-14)

Build real projects.

| Chapter | Topic | Description | Link |
|---------|-------|-------------|------|
| 11 | Website Development | Building a portfolio site | [EN](./Chapter11/README.md) / [KO](./Chapter11/README.ko.md) |
| 12 | Deployment | Vercel, Railway | [EN](./Chapter12/README.md) / [KO](./Chapter12/README.ko.md) |
| 13 | Data Storage | Supabase integration | [EN](./Chapter13/README.md) / [KO](./Chapter13/README.ko.md) |
| 14 | Mini Games | Building fun games | [EN](./Chapter14/README.md) / [KO](./Chapter14/README.ko.md) |

### Part 4: Practical Projects II (Chapter 15-17)

Build more practical projects.

| Chapter | Topic | Description | Link |
|---------|-------|-------------|------|
| 15 | Building AI Tools | Chatbot, summarizer, translator | [EN](./Chapter15/README.md) / [KO](./Chapter15/README.ko.md) |
| 16 | Data Processing | CSV, JSON, automation | [EN](./Chapter16/README.md) / [KO](./Chapter16/README.ko.md) |
| 17 | API Integration | Weather app, Pokemon Pokedex | [EN](./Chapter17/README.md) / [KO](./Chapter17/README.ko.md) |

### Part 5: Advanced & Mastery (Chapter 18-20)

Learn advanced Claude Code features.

| Chapter | Topic | Description | Link |
|---------|-------|-------------|------|
| 18 | Advanced Features | Hooks, Skills, subagents | [EN](./Chapter18/README.md) / [KO](./Chapter18/README.ko.md) |
| 19 | Automation | MCP, CI/CD | [EN](./Chapter19/README.md) / [KO](./Chapter19/README.ko.md) |
| 20 | Mastery | Summary, next steps | [EN](./Chapter20/README.md) / [KO](./Chapter20/README.ko.md) |

---

## Learning Guide

### How to Proceed

* Complete chapters **in order**. Skipping may cause difficulty understanding later content.
* Don't just read—**run the code yourself**.
* Understanding one chapter **deeply** is more important than skimming through many.

### When Stuck

* First, ask Claude. "What is this?", "Why does this happen?"
* If stuck for more than 24 hours, review previous chapter content.
* Copy error messages exactly and show them to Claude.

<details>
<summary><strong>Mindset Before Starting</strong></summary>

### 1. You Don't Have to Be Perfect
At first, you might get weird results. Just say it again differently.

### 2. Errors Are Normal
In coding, errors aren't failures—they're part of the process. AI can fix errors too.

### 3. Start Small
Don't try to build something amazing right away. Start with "Hello World."

### 4. Ask When Curious
"What is this?", "Why do it this way?" Just ask the AI and it will explain.

</details>

---

## Pro Tips

Tips from the Claude Code team and experienced developers.

1. **Start in Plan mode** - Press `Shift+Tab` twice. Let Claude plan first.

2. **Specify verification methods** - Have Claude run tests, type checks, etc.

3. **Hold the same bar** - Review AI code like you would human code.

4. **Save CLAUDE.md in Git** - Update it whenever Claude makes mistakes.

5. **Use parallel work** - For complex tasks, use multiple terminals simultaneously.

<details>
<summary><strong>Additional Tips</strong></summary>

### Claude Code vs Web Chat

| Web Chat (ChatGPT, Claude.ai) | Claude Code |
|------------------------------|-------------|
| Copy code and paste into files | Creates and edits files directly |
| Cannot access local files | Understands your whole project |
| Run commands yourself | Runs commands for you |
| Explain context every time | Already knows the situation |

**Summary**: Web chat is "someone who tells you the recipe," Claude Code is "a chef who cooks for you."

### Writing Effective Prompts

**Bad example:**
```
> Fix the bug
```

**Good example:**
```
> Error occurs when clicking the login button in @src/login.js.
> Error: "Cannot read property 'email' of undefined"
> Please fix it.
```

The more specific you are, the better the results.

</details>

---

## Getting Started

### Requirements

- A computer (Mac, Windows, Linux)
- Internet connection

### Installation and Launch

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Run Claude Code
claude
```

Start with [Chapter 01: What is AI Coding?](./Chapter01/README.md).

---

## License

MIT License
