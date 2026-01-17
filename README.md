# Vibecoding Curriculum

> **The Most Comprehensive Claude Code Learning Curriculum** - From Complete Beginner to AI-Native Developer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Chapters](https://img.shields.io/badge/Chapters-30-blue.svg)](#curriculum-structure)
[![Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-blueviolet.svg)](https://github.com/anthropics/claude-code)
[![Beginner Friendly](https://img.shields.io/badge/Beginner-Friendly-green.svg)](#audience)

**English** | [한국어](./README.ko.md)

---

## 🎯 Who Is This For?

> **Built for all levels** — from complete beginners to experienced vibecoding developers.

| Your Level | What You'll Get |
|------------|-----------------|
| **"What is coding?"** | Start from scratch, complete your first project with AI |
| **"I've coded before"** | 10x your productivity with AI tools |
| **"Already vibecoding"** | Optimize workflow with advanced features (Hooks, MCP, Skills) |
| **"Want to introduce to team"** | Team collaboration, CI/CD, onboarding strategies |

---

## Why This Curriculum?

This curriculum goes beyond the official Claude Code tutorial to provide:

| Official Tutorial | This Curriculum |
|-------------------|-----------------|
| Quick start guide | **30 chapters** of systematic learning |
| Assumes coding knowledge | **Beginner-friendly** with no prerequisites |
| Reference documentation | **Hands-on projects** you can actually use |
| English only | **Bilingual** (English + Korean) |
| Tool-focused | **Concept + Practice + Real Projects** |

**What makes us different:**
- **Complete beginner path**: Start from "What is coding?" to building full-stack apps
- **Progressive complexity**: Each chapter builds on the previous
- **Real-world projects**: Portfolio sites, games, chatbots, CLI tools, full-stack apps
- **Learn More sections**: Curated resources (videos, docs, tools) in every chapter
- **Beginner Tips**: Explains technical concepts in simple terms throughout

---

## 💬 Community

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Join_Community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

- Questions and Answers
- Project Sharing
- Study Groups

---

## Quick Start

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Run Claude Code
claude
```

Ready? Start with [Chapter 01: What is AI Coding?](./Chapter01-AI-Coding-Intro/README.md)!

---

## What is Vibecoding?

**Vibecoding** is a new paradigm of developing with AI, coined by Andrej Karpathy (former Tesla AI Director).

```
Traditional Coding:
You → Learn syntax → Write code → Debug → Software

Vibecoding:
You → Express intent → AI writes code → Refine together → Software
```

### The 3-Step Cycle

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   1. Express Intent  →  2. Check Result  →  3. Refine  │
│         ↑                                      │       │
│         └──────────────────────────────────────┘       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

1. **Express Intent**: Tell Claude what you want in natural language
2. **Check Result**: See if it matches your expectations
3. **Refine**: Give feedback to improve

Repeat until perfect. No syntax memorization needed.

<details>
<summary><strong>See Vibecoding in Action</strong></summary>

### Example: Making a Webpage

**Traditional coding (hours of learning required):**
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

**Vibecoding (30 seconds):**
```
You: Make a webpage with a blue heading that says 'Hello'
Claude: [creates the file instantly]
You: Make the heading bigger and center it
Claude: [updates the file]
```

The AI handles syntax. You focus on **what** you want, not **how** to code it.

</details>

---

## Audience

| Level | Description | Starting Point |
|-------|-------------|----------------|
| **Complete Beginner** | Never coded before | Chapter 01 |
| **Beginner** | Know basics, want AI tools | Chapter 06 |
| **Intermediate** | Comfortable with code, want productivity | Chapter 17 |
| **Advanced** | Want to master Claude Code | Chapter 21 |

No coding experience required for complete beginners. We start from scratch.

---

## Learning Outcomes

### After 30 Chapters, You Can Build:

| Project Type | Examples |
|--------------|----------|
| **Websites** | Portfolio, blog, landing page |
| **Web Apps** | Todo app, dashboard, e-commerce |
| **Games** | Snake, memory match, reaction games |
| **CLI Tools** | File organizer, data processor |
| **Chatbots** | Discord bot, Slack bot |
| **Full-Stack Apps** | Complete apps with database & auth |
| **Automation** | CI/CD pipelines, GitHub Actions |
| **Web3 Dapps** | Farcaster Frames, Base miniapps, Smart Contracts |

### Skills You'll Gain:

- Effective prompting techniques
- Code navigation and debugging
- Git version control
- Project architecture understanding
- Team collaboration workflows
- Advanced Claude Code features (Hooks, Skills, MCP)

---

## Why Claude Code?

> **"There's Google Jules, Cursor, Windsurf... Why Claude Code?"**

| Criteria | Claude Code | Other Tools |
|----------|-------------|-------------|
| **Terminal Native** | ✅ CLI-based, works anywhere | Often IDE-dependent |
| **Transparent Operation** | ✅ Shows every action explicitly | Sometimes black-box |
| **Learning Value** | ✅ Learn principles as you go | Convenience can hide fundamentals |
| **Extensibility** | ✅ Hooks, MCP, Skills, etc. | Varies by tool |
| **Cost** | ✅ Pay for what you use (API) | Mostly subscription-based |

> 💡 **Future Plans**: We plan to expand curricula for Cursor, Windsurf, Jules, and other tools.

**Compatibility Note**: Core concepts learned here (prompting, debugging, project structure) apply to ANY AI coding tool.

---

## 📚 Confused by Terminology?

If you encounter unfamiliar terms, check the [Glossary](./GLOSSARY.md)!

Every chapter also includes **Beginner Tips** that explain technical concepts in simple terms.

---

## Curriculum Structure

### Learning Path Overview

Think of this curriculum like learning to cook with an AI sous chef:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Your Learning Journey                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 1: Getting Started (Ch.01-05)                             │
│  "Learn the kitchen tools" 🔪                                   │
│  → What is AI coding? Install Claude Code. Basic commands.      │
│  🎯 Milestone: First conversation with Claude                   │
│  💬 Like: Setting up your kitchen and meeting your AI chef      │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 2: Core Features (Ch.06-11)                               │
│  "Learn to cook basic dishes" 🍳                                │
│  → Prompting, code navigation, Git, project memory              │
│  🎯 Milestone: Modify existing code confidently                 │
│  💬 Like: Making scrambled eggs and following recipes           │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 3: Practical Projects I (Ch.12-16)                        │
│  "Cook your first real meals" 🍝                                │
│  → Websites, deployment, databases, games                       │
│  🎯 Milestone: Deploy your portfolio online                     │
│  💬 Like: Cooking dinner for yourself and posting pics online   │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 4: Practical Projects II (Ch.17-20)                       │
│  "Host a dinner party" 🎉                                       │
│  → CLI tools, chatbots, backend, full-stack                     │
│  🎯 Milestone: Build a complete app with auth                   │
│  💬 Like: Cooking for friends - multiple dishes, timing matters │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 5: Advanced (Ch.21-27)                                    │
│  "Become a master chef" 👨‍🍳                                      │
│  → Architecture, Hooks, MCP, CI/CD, team workflows              │
│  🎯 Milestone: Master-level Claude Code usage                   │
│  💬 Like: Running a restaurant - systems, staff, efficiency     │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 6: Web3 Development (Ch.28-30)                            │
│  "Open your own restaurant on the blockchain" 🌐                │
│  → Wallet, Frames, Smart Contracts, NFT Community               │
│  🎯 Milestone: NFT-Gated Community (Final Project!)             │
│  💬 Like: Running a members-only club with crypto membership    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Estimated Time

| Pace | Duration |
|------|----------|
| 2 hours/day | ~2 weeks |
| 1 hour/day | ~4 weeks |
| Weekends only | ~4 weeks |

---

### Part 1: Getting Started (Chapter 01-05)

Learn basic concepts and how to use Claude Code.

| Chapter | Topic | What You'll Learn | Link |
|---------|-------|-------------------|------|
| 01 | What is AI Coding? | Vibecoding concept, why Claude Code | [Go](./Chapter01-AI-Coding-Intro/README.md) |
| 02 | Installation | Terminal basics, install Claude Code | [Go](./Chapter02-Installation/README.md) |
| 03 | First Conversation | Permission modes, shortcuts, slash commands | [Go](./Chapter03-First-Conversation/README.md) |
| 04 | Working with Files | @mentions, create/read/edit files | [Go](./Chapter04-Working-with-Files/README.md) |
| 05 | Terminal Commands | Run commands, execute projects | [Go](./Chapter05-Terminal-Commands/README.md) |

---

### Part 2: Core Features (Chapter 06-11)

Master the core features of Claude Code.

| Chapter | Topic | What You'll Learn | Link |
|---------|-------|-------------------|------|
| 06 | Project Structure | Navigate folders, understand codebases | [Go](./Chapter06-Project-Structure/README.md) |
| 07 | Context & Memory | How Claude remembers, context management | [Go](./Chapter07-Context-and-Memory/README.md) |
| 08 | Effective Prompting | Write better prompts, Plan mode | [Go](./Chapter08-Effective-Prompting/README.md) |
| 09 | Exploring Code | Glob, Grep, find anything | [Go](./Chapter09-Exploring-Code/README.md) |
| 10 | Editing Code | Explore → Plan → Execute workflow | [Go](./Chapter10-Editing-Code/README.md) |
| 11 | Git Basics | Commits, branches, pull requests | [Go](./Chapter11-Git-Basics/README.md) |

### 🎉 After Part 2

> **Congratulations!** You now have all the fundamental skills.
>
> **You can:**
> - Navigate any codebase
> - Write effective prompts
> - Modify existing code
> - Use Git for version control
>
> **Ready to build real things!**

---

### Part 3: Practical Projects I (Chapter 12-16)

Build real projects and deploy them.

| Chapter | Topic | What You'll Build | Link |
|---------|-------|-------------------|------|
| 12 | Project Memory | CLAUDE.md for project-specific instructions | [Go](./Chapter12-Project-Memory/README.md) |
| 13 | Website Development | Personal portfolio website | [Go](./Chapter13-Website-Development/README.md) |
| 14 | Deployment | Deploy to Vercel/Railway | [Go](./Chapter14-Deployment/README.md) |
| 15 | Data Storage | Connect to Supabase database | [Go](./Chapter15-Data-Storage/README.md) |
| 16 | Mini Games | Fun browser games | [Go](./Chapter16-Mini-Games/README.md) |

### 🎉 After Part 3

> **You're now a web developer!**
>
> **You've built:**
> - A portfolio site anyone can visit
> - Apps that store real data
> - Games you can share with friends

---

### Part 4: Practical Projects II (Chapter 17-20)

Build more practical, production-ready projects.

| Chapter | Topic | What You'll Build | Link |
|---------|-------|-------------------|------|
| 17 | CLI Tools | Command-line utilities | [Go](./Chapter17-CLI-Tools/README.md) |
| 18 | Chatbots | Discord/Slack bots | [Go](./Chapter18-Chatbots/README.md) |
| 19 | Backend Basics | REST API with Express + SQLite | [Go](./Chapter19-Backend-Basics/README.md) |
| 20 | Full-Stack Apps | Complete app with auth (JWT) | [Go](./Chapter20-Full-Stack-Apps/README.md) |

### 🎉 After Part 4

> **You can build production apps!**
>
> **You understand:**
> - Frontend + Backend architecture
> - Database integration
> - User authentication
> - API design

---

### Part 5: Advanced (Chapter 21-27)

Master Claude Code and become a power user.

| Chapter | Topic | What You'll Master | Link |
|---------|-------|-------------------|------|
| 21 | Architecture | How Claude Code works internally | [Go](./Chapter21-Architecture/README.md) |
| 22 | Advanced Config | CLAUDE.md tiers, settings.json | [Go](./Chapter22-Advanced-Config/README.md) |
| 23 | Hooks & Commands | Automation triggers, custom commands | [Go](./Chapter23-Hooks-and-Commands/README.md) |
| 24 | Agents & Skills | Specialized AI assistants | [Go](./Chapter24-Agents-and-Skills/README.md) |
| 25 | MCP Integration | Connect external services | [Go](./Chapter25-MCP-Integration/README.md) |
| 26 | CI/CD Automation | GitHub Actions, auto deployment | [Go](./Chapter26-CI-CD-Automation/README.md) |
| 27 | Team Collaboration | Team workflows, onboarding | [Go](./Chapter27-Team-Collaboration/README.md) |

### 🎉 After Part 5

> **You're now a Claude Code power user!**
>
> **You understand:**
> - How Claude Code works internally
> - Advanced configuration and customization
> - Team collaboration patterns

---

### Part 6: Web3 Development (Chapter 28-30)

"Enter the matrix" - Blockchain and decentralized applications await!

```
🎮 Your Web3 Evolution:

Ch.28: CONSUMER 🛒     Ch.29: BUILDER 🔨     Ch.30: CREATOR ⚡
"Order from         →  "Open your own    →  "Design the
 the menu"              food truck"           whole kitchen"

Use existing dapps     Build social Web3     Write smart contracts
Connect wallet         Create Frames         Deploy your own code
Claim tokens/NFTs      Viral distribution    NFT-gated communities
```

| Chapter | Topic | What You'll Build | Link |
|---------|-------|-------------------|------|
| 28 | Web3 Basics | Wallet connection, token/NFT claim site | [Go](./Chapter28-Web3-Basics/README.md) |
| 29 | Farcaster Frames | Interactive poll/quiz Frames | [Go](./Chapter29-Farcaster-Frames/README.md) |
| 30 | Base Smart Contracts | NFT-Gated Community (Final Project) | [Go](./Chapter30-Base-Smart-Contracts/README.md) |

> 💡 **Fun Fact**: By the end of Part 6, you'll have built an NFT membership system - the same tech used by exclusive communities like Pudgy Penguins or Bored Ape Yacht Club (just on testnet, and way cooler because YOU built it!)

### 🎉 After Completing Everything

> **You've become a Vibecoding Master + Web3 Developer!**
>
> **What's next:**
> - Start your own projects
> - Contribute to open source
> - Build and deploy dapps
> - Join Web3 communities (Farcaster, Base)
> - Teach others
> - Stay updated on AI tools

---

## Learning Guide

### How to Proceed

| Do | Don't |
|----|-------|
| ✅ Start from [your level](#audience), proceed in order | ❌ Jump to the middle without prior knowledge |
| ✅ Ask Claude directly, check the results | ❌ Just read without executing |
| ✅ Give feedback when results differ | ❌ Give up if first result isn't perfect |
| ✅ Ask Claude "why?" when stuck | ❌ Ignore error messages |

> 💡 **Vibecoding Tip**: You don't need to type code yourself!
> **Describe** what you want to Claude, check the result, and give feedback.

### When You're Stuck

1. **Ask Claude**: "What is this?", "Why does this error happen?"
2. **Check the Glossary**: [GLOSSARY.md](./GLOSSARY.md)
3. **Ask the Community**: [Discord](https://discord.gg/your-invite-link)
4. **Review previous chapter**: Often the answer is there

### Common Beginner Mistakes

| Mistake | Solution |
|---------|----------|
| Vague prompts ("fix bug") | Be specific (file, error, context) |
| Expecting perfection first try | Iterate with feedback |
| Not reading error messages | Copy-paste to Claude |
| Skipping chapters | Each chapter builds on previous |

---

## Pro Tips

From the Claude Code team and experienced developers:

### Essential Tips

1. **Start in Plan mode** - `Shift+Tab` twice. Let Claude think first.
2. **Specify verification** - "Run tests after changes"
3. **Review AI code** - Same standards as human code
4. **Commit CLAUDE.md to Git** - Update when Claude makes mistakes
5. **Use multiple terminals** - Parallel work for complex tasks

### Claude Code vs Web Chat

| Feature | Web Chat | Claude Code |
|---------|----------|-------------|
| File access | ❌ Copy-paste | ✅ Direct read/write |
| Command execution | ❌ Manual | ✅ Automatic |
| Project context | ❌ Explain each time | ✅ Understands structure |
| Continuous work | ❌ Session-based | ✅ Persistent |

**TL;DR**: Web chat is a consultant. Claude Code is a pair programmer in your terminal.

---

## What Each Chapter Includes

Every chapter in this curriculum provides:

- **🎯 Clear learning objectives**
- **⏱️ Estimated time**
- **📋 Prerequisites**
- **📝 Step-by-step tutorials**
- **🔨 Hands-on exercises**
- **💡 Beginner Tips** - Technical concepts explained simply
- **❌ Common mistakes** - What to avoid
- **🆘 Troubleshooting** - When things go wrong
- **✅ Checklist** - Verify your understanding
- **📖 Learn More** - Curated videos, docs, and tools

---

## Contributing

Found a typo? Have a suggestion? PRs welcome!

## License

MIT License

---

<p align="center">
  <strong>Ready to start your AI-native development journey?</strong><br>
  <a href="./Chapter01-AI-Coding-Intro/README.md">Begin Chapter 01 →</a>
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
