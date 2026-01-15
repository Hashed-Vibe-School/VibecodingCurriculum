# Chapter 18: Understanding the Architecture

**English** | [한국어](./README.ko.md)

## What You Will Learn

- How Claude Code works under the hood
- Understanding the tool system
- Using this knowledge to make better requests

---

## Why do you need this?

**Real-world scenario**: You ask Claude Code "Show me all the TypeScript files in this project and fix any errors" but it takes forever and costs a lot. Why? Because you don't understand how it works under the hood.

Understanding the architecture helps you:
- Know why some requests are fast and others are slow
- Debug when something goes wrong
- Save money by making efficient requests

### Simple Analogy: Restaurant Kitchen

Think of Claude Code like ordering at a restaurant:

```
You (Customer) --> Waiter (Claude Code CLI) --> Kitchen (Anthropic API)
                                                    |
                                                    v
                                              Chef prepares food
                                                    |
                                                    v
                              Waiter <-- brings back --> You get your meal
```

- The **waiter** (CLI) takes your order and communicates with the kitchen
- The **kitchen** (API) does the actual cooking (thinking)
- Every trip to the kitchen takes time (API round trip)
- If you order "surprise me with something good", the waiter has to make multiple trips to ask questions!

---

## Why Understand the Architecture?

Thinking of Claude Code as "magic" has limitations. Understanding how it works gives you:

- **More accurate requests**: Predict which tools will be used
- **Problem solving**: Figure out why something isn't working
- **Efficient usage**: Reduce unnecessary operations

---

## Claude Code's Overall Structure

### At a Glance

```
┌─────────────────────────────────────────────────────────────────┐
│                        You (Terminal)                            │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                       Claude Code CLI                            │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────────┐    │
│  │ Input Handler │  │ Tool Engine   │  │  Output Renderer  │    │
│  └───────────────┘  └───────────────┘  └───────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Anthropic API (Claude)                        │
│               Models: opus / sonnet / haiku available            │
└─────────────────────────────────────────────────────────────────┘
```

**Key point**: Claude doesn't run directly on your computer. It communicates through the API, and only tool execution happens locally.

### What is an API? (For Beginners)

An **API** (Application Programming Interface) is like a messenger between two programs. When you use Claude Code:

1. Your computer sends your message to Anthropic's servers
2. Claude (on their servers) thinks about your request
3. The response comes back to your computer
4. If Claude needs to do something (read a file, run a command), that happens on YOUR computer

**Why does this matter?**
- Claude can't see your files until it asks for them
- Every question Claude asks = another round trip = more time and cost
- Being specific upfront saves round trips!

### API Communication Cycle

Let's see what happens when you make a request:

```
 [1] Your request                     [2] Sent to API
      │                                 │
      ▼                                 ▼
┌──────────┐    Message + Tool Defs   ┌─────────────┐
│ CLI      │ ─────────────────────▶ │ Anthropic   │
│ Client   │                        │ API         │
└──────────┘                        └─────────────┘
      ▲                                 │
      │     Response (text + tool call) │
      └─────────────────────────────────┘
                    [3]

 [4] Tool executed locally            [5] Results back to API
      │                                 │
      ▼                                 ▼
┌──────────────────┐              ┌─────────────┐
│ Bash, Read, etc. │ ───────────▶ │ Claude      │
│ executed         │              │ decides next│
└──────────────────┘              └─────────────┘
```

### Why Does This Matter?

```
> Show me all files in this folder
```

When you make this request:
1. Claude uses `Glob` tool to list files → 1 API round trip
2. Receives results and decides next action → 2 API round trips
3. Uses `Read` if needed to view contents → 3 API round trips

**One request = multiple API round trips**. Being specific reduces round trips and speeds things up.

---

## The Tool System

Claude Code uses 18 built-in tools.

### Complete Tool List

| Category | Tool | Description |
|----------|------|-------------|
| **File Operations** | `Read` | Read files (supports images, PDF, Jupyter notebooks) |
| | `Write` | Create/overwrite files |
| | `Edit` | Edit files via string replacement |
| | `Glob` | Search files by pattern (e.g., `**/*.ts`) |
| | `Grep` | Search content (ripgrep-based) |
| **Execution** | `Bash` | Run shell commands |
| | `KillShell` | Terminate background shells |
| **Agents** | `Task` | Create and run subagents |
| | `TaskOutput` | Get background task results |
| **Web** | `WebFetch` | Fetch URL content (HTML→Markdown) |
| | `WebSearch` | Web search |
| **Planning** | `EnterPlanMode` | Enter plan mode |
| | `ExitPlanMode` | Exit plan mode and request approval |
| **User Interaction** | `AskUserQuestion` | Ask user questions (multiple choice) |
| | `TodoWrite` | Manage task list |
| **Other** | `Skill` | Run skills (/commit, /review-pr, etc.) |
| | `NotebookEdit` | Edit Jupyter notebook cells |

### Tool Limitations

| Tool | Limitation | Value |
|------|------------|-------|
| **Bash** | Default timeout | 2 minutes |
| | Max timeout | 10 minutes |
| | Output limit | 30,000 characters |
| **Read** | Default lines | 2,000 lines |
| | Line length | 2,000 characters |

### Tool Categories (Visual)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              Tool Execution Layer                            │
│                                                                             │
│  File Operations ───────────────────────────────────────────────────────    │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐              │
│  │  Read   │ │  Write  │ │  Edit   │ │  Glob   │ │  Grep   │              │
│  │ read    │ │ create  │ │ modify  │ │ search  │ │ content │              │
│  │ files   │ │ files   │ │ files   │ │ files   │ │ search  │              │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘              │
│                                                                             │
│  Execution ─────────────────────────────────────────────────────────────    │
│  ┌─────────┐ ┌─────────────────┐                                           │
│  │  Bash   │ │     Task        │                                           │
│  │ run     │ │ (subagents)     │                                           │
│  │ commands│ │                 │                                           │
│  └─────────┘ └─────────────────┘                                           │
│                                                                             │
│  Web ───────────────────────────────────────────────────────────────────    │
│  ┌─────────┐ ┌─────────┐                                                   │
│  │WebFetch │ │WebSearch│                                                   │
│  │fetch URL│ │web      │                                                   │
│  │         │ │search   │                                                   │
│  └─────────┘ └─────────┘                                                   │
│                                                                             │
│  User Interaction ──────────────────────────────────────────────────────    │
│  ┌─────────┐ ┌─────────┐ ┌───────────────┐ ┌───────────────┐              │
│  │TodoWrite│ │  Skill  │ │ EnterPlanMode │ │AskUserQuestion│              │
│  │task mgmt│ │run skill│ │  plan mode    │ │ ask user      │              │
│  └─────────┘ └─────────┘ └───────────────┘ └───────────────┘              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Tool Execution Flow

When a tool is called, it's processed like this:

```
    Claude Response
         │
         ▼
  ┌──────────────┐
  │ Parse        │
  │ Response     │
  │ (text +      │
  │  tool calls) │
  └──────┬───────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│ Text   │ │ Tool   │
│ Output │ │ Execute│
└────────┘ └───┬────┘
               │
         ┌─────┴─────┐
         ▼           ▼
    ┌────────┐  ┌────────┐
    │ Success│  │ Failure│
    │ Result │  │ Error  │
    └───┬────┘  └───┬────┘
        │           │
        └─────┬─────┘
              ▼
    ┌─────────────────┐
    │ Send result to  │
    │ API (next turn) │
    └─────────────────┘
```

### Why Does This Matter?

**Different tools are used based on how you request:**

```
# Uses Glob (fast)
> How many tsx files are in the src folder?

# Uses Bash (unnecessarily complex)
> Use find command to count tsx files in src folder
```

For tasks where Claude Code has dedicated tools, requesting those tools is more efficient.

### AskUserQuestion Tool

This tool allows Claude to ask you questions during task execution.

**When it's used:**
- Clarifying ambiguous requirements
- Choosing between multiple approaches
- Getting preferences for implementation details

**Example interaction:**

```
Claude: I need to clarify something about the authentication.

┌─ Auth method ────────────────────────────────────────┐
│ Which authentication method should I use?           │
│                                                      │
│ ○ JWT (Recommended)                                  │
│   Stateless, good for APIs                          │
│                                                      │
│ ○ Session-based                                      │
│   Traditional, server-side storage                  │
│                                                      │
│ ○ OAuth                                              │
│   Third-party login (Google, GitHub)                │
└──────────────────────────────────────────────────────┘
```

**Why this matters:**
Instead of guessing or making assumptions, Claude can ask you directly. This leads to better results that match your actual needs.

---

## The Subagent System

Complex tasks are split into multiple "subagents."

### Subagent Architecture

```
                          ┌─────────────────┐
                          │   Main Agent    │
                          │ (talks with you)│
                          └────────┬────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    │              │              │
                    ▼              ▼              ▼
         ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
         │   Task 1     │ │   Task 2     │ │   Task 3     │
         │  (explore)   │ │  (analyze)   │ │  (execute)   │
         └──────────────┘ └──────────────┘ └──────────────┘
```

### Subagent Types

| Type | Purpose | Available Tools |
|------|---------|----------------|
| `Explore` | Codebase exploration, file search | Read, Glob, Grep (no Edit/Write) |
| `Plan` | Implementation planning, architecture | Read, Glob, Grep (no Edit/Write) |
| `Bash` | Git operations, command execution | Bash only |
| `general-purpose` | Complex multi-step tasks | All tools (*) |
| `claude-code-guide` | Claude Code usage help | Glob, Grep, Read, WebFetch, WebSearch |
| `statusline-setup` | Status line configuration | Read, Edit |

### Parallel vs Sequential Execution

```
Parallel Execution (independent tasks):
─────────────────────────────────────────────
    ┌──────────┐     ┌──────────┐     ┌──────────┐
    │ Agent A  │     │ Agent B  │     │ Agent C  │
    │ (search1)│     │ (search2)│     │ (search3)│
    └────┬─────┘     └────┬─────┘     └────┬─────┘
         │                │                │
         └────────────────┼────────────────┘
                          │
                          ▼
                   ┌──────────────┐
                   │ Merge Results│
                   └──────────────┘

Sequential Execution (dependent tasks):
─────────────────────────────────────────────
    ┌──────────┐     ┌──────────┐     ┌──────────┐
    │ Agent A  │ ──▶ │ Agent B  │ ──▶ │ Agent C  │
    │ (analyze)│     │ (uses A  │     │ (uses B  │
    │          │     │  result) │     │  result) │
    └──────────┘     └──────────┘     └──────────┘
```

### Why Does This Matter?

**Large tasks are automatically split:**

```
> Analyze this entire project and create a refactoring plan
```

Internally:
1. `Explore` agent analyzes project structure
2. `Plan` agent creates the plan
3. Main agent synthesizes results

**Splitting tasks yourself gives more control:**

```
# All at once (agent splits automatically)
> Analyze project and refactor

# Step by step (more precise control)
> First, just analyze the project structure
> (after reviewing)
> Create a refactoring plan for this part
> (after reviewing plan)
> OK, execute it
```

---

## Model Selection

Claude Code supports multiple models.

| Model | Characteristics | When to Use |
|-------|-----------------|-------------|
| **Opus** | Smartest, expensive | Complex design, hard bugs |
| **Sonnet** | Balanced performance | General tasks (default) |
| **Haiku** | Fast and cheap | Simple tasks, repetitive work |

### Practical Strategy

```
# Planning phase: smart model
> /model opus
> How should I design this system?

# Implementation phase: fast model
> /model sonnet
> Build it according to the plan above
```

Adjust the **cost vs quality tradeoff** based on the situation.

---

## Context Management

### Quality Drops as Conversations Get Long

Claude has limits on how much information it can process at once. As conversations lengthen:
- Earlier content is poorly remembered
- Overall context comprehension blurs
- Response quality degrades

### Solutions

**1. Compress with /compact**
```
> /compact
```
Summarizes the conversation to save tokens.

**2. Separate conversations by topic**
```
# Doing everything in one conversation (contexts mix)
> Make auth feature
> Also modify DB schema
> Also make frontend form

# Separating by topic (more focused)
> /clear
> Let's focus on auth. Make the backend API first.
```

**3. Use CLAUDE.md**

CLAUDE.md is preserved after `/clear`. Put project rules here and context survives even when starting fresh conversations.

---

## Permissions and Security

### Security Policy Flow

```
  User Request
       │
       ▼
┌──────────────┐    No        ┌──────────────┐
│ Dangerous    │ ────────────▶ │ Execute      │
│ operation?   │               │ directly     │
└──────┬───────┘               └──────────────┘
       │ Yes
       ▼
┌──────────────┐    Deny      ┌──────────────┐
│ Request user │ ────────────▶ │ Cancel       │
│ approval     │               │ operation    │
└──────┬───────┘               └──────────────┘
       │ Approve
       ▼
┌──────────────┐
│ Execute in   │
│ sandbox      │
└──────────────┘
```

### Why Does It Ask for Approval?

Claude Code requests approval before potentially dangerous operations:
- File modification/deletion
- Command execution
- External API calls

### Auto-approve Settings

Auto-approve frequently used safe tools:

```json
// settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

**Caution**: Be careful with `Edit`, `Write`, `Bash`.

### Sandbox Mode

Bash commands run in a sandbox by default. This protects your system while working.

---

## Practical Tips

### 1. Mention Tools Explicitly

```
# Implicit (Claude must infer)
> How is error handling done in this project?

# Explicit (faster)
> Use grep to search for "catch" keyword and show error handling patterns
```

### 2. One Thing at a Time

```
# Multiple tasks at once (can cause confusion)
> Create file, write tests, and commit

# Step by step (more accurate)
> Create the file first
> (after checking) Write tests
> (after checking) Commit
```

### 3. Verify Before Proceeding

If Claude modified a file, verify the result before moving to the next step. You can immediately fix anything that's wrong.

---

## Try it yourself

Here's a simple exercise to understand the tool system better:

### Exercise 1: Watch the Tools

```
> How many files are in this folder?
```

Watch Claude's response. Did it use `Glob` or `Bash`? The result is the same, but the efficiency differs.

### Exercise 2: Compare Approaches

Try these two requests and notice the difference:

```
# Approach A - Vague
> Tell me about this project

# Approach B - Specific
> Read the package.json and tell me what dependencies this project uses
```

Approach B is faster because Claude knows exactly which tool to use (Read).

### Exercise 3: Count the Round Trips

```
> Find all TODO comments in this project and list them
```

Watch how many tool calls happen. Each one is an API round trip!

---

## If it doesn't work?

### Problem: Claude seems slow or stuck

**Possible causes:**
1. Your request requires many tool calls (round trips)
2. You're processing a very large file
3. Network issues with the API

**Solutions:**
- Break down large requests into smaller steps
- Be specific about what you want
- Check your internet connection
- Use `/clear` to start fresh if context got confused

### Problem: Claude uses the wrong tool

**Example:** You ask to search for text, but Claude uses `Bash` with `grep` instead of the built-in `Grep` tool.

**Solution:** Be explicit about tools when needed:
```
# Instead of
> search for "TODO" in this project

# Try
> Use grep tool to search for "TODO" in all files
```

### Problem: Permission denied errors

**Cause:** Claude Code needs permission for dangerous operations.

**Solution:** Press `y` to approve, or configure auto-approve in settings for safe tools.

---

## Common mistakes

1. **Asking for too much at once**
   - Bad: "Analyze this project, fix all bugs, add tests, and deploy"
   - Good: "First, analyze the project structure"

2. **Not being specific about locations**
   - Bad: "Show me the config file"
   - Good: "Show me the tsconfig.json file"

3. **Forgetting context limits**
   - Long conversations lose context
   - Use `/compact` or `/clear` periodically

4. **Ignoring the tool output**
   - Always check what Claude actually did before moving on
   - You can catch mistakes early

5. **Not understanding costs**
   - More round trips = more API calls = more cost
   - Specific requests are cheaper!

---

## Summary

What you learned in this chapter:
- [x] Claude Code's overall architecture and API communication flow
- [x] 18 built-in tools system
- [x] How subagents work and parallel execution
- [x] Model selection and context management
- [x] Permissions and security policies

**Key point**: Claude Code isn't magic—it's a system. Understanding the system helps you use it better.

In the next chapter, you'll learn how to customize this system.

Proceed to [Chapter 19: Advanced Configuration](../Chapter19/README.md).
