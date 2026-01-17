# Chapter 21: Understanding Architecture

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Previous Chapter Review

In [Chapter 20: Building Full-Stack Apps](../Chapter20-Full-Stack-Apps/README.md), we connected a React frontend with a backend and implemented JWT authentication.

Congratulations! If you've made it this far, you now have **the skills to build complete apps**.

### Before Moving to Part 5

Let's take a moment to look back. So far we have:

```
Part 1-3: Claude Code basics
    |
Part 4: Building real apps (CLI tools, chatbots, full-stack apps)
    |
Part 5: Using it better (starting now!)
```

Through Part 4, we focused on **"what can you build with Claude Code"**.

In Part 5, we'll learn **"how to use Claude Code more effectively"**.

### Why Should You Understand Architecture?

When driving a car, you don't need to know how the engine works. But if you understand how the engine operates:
- You can guess what's wrong when you hear strange sounds
- You can understand driving techniques that improve fuel efficiency
- You can accurately explain the problem to a mechanic when something breaks

Claude Code is the same. If you understand how it works internally:
- You can understand why some requests are fast and others are slow
- You can understand request methods that save costs
- You can solve problems yourself when they occur

> **Beginner Tip**: This chapter might feel a bit technical. But don't worry! We'll explain every concept with easy analogies. It's okay if you don't understand 100%. Even just knowing the general principles will help you use Claude Code better.

---

## What You Will Learn

- How Claude Code works
- Understanding the tool system
- Using this knowledge to make better requests

---

## Why Is This Needed?

**Real situation**: You requested "find all TypeScript files in this project and fix the errors" but it took a long time and cost a lot. Why? Because you don't understand how it works internally.

If you understand the architecture:
- You can know which requests are fast and which are slow
- You can debug when problems occur
- You can save costs with efficient requests

### Easy Analogy: Restaurant Kitchen

Think of Claude Code like ordering at a restaurant:

```
You (customer) --> Waiter (Claude Code CLI) --> Kitchen (Anthropic API)
                                                      |
                                                      v
                                                Chef prepares food
                                                      |
                                                      v
                                Waiter <-- brings food --> You eat
```

- The **waiter** (CLI) takes your order and communicates with the kitchen
- The actual cooking (thinking) happens in the **kitchen** (API)
- Every trip to the kitchen takes time (API round trip)
- If you say "just make something delicious," the waiter has to make multiple trips

---

## Why Should You Know the Architecture?

If you think of Claude Code as a "magic tool," there are limitations. Understanding how it works allows:

- **More accurate requests**: You can predict which tools will be used
- **Problem solving**: You can figure out why things aren't working yourself
- **Efficient usage**: Reduce unnecessary work

---

## Overall Structure of Claude Code

### At a Glance

```
+------------------------------------------------------------------+
|                        You (Terminal)                              |
+------------------------------------------------------------------+
                               |
                               v
+------------------------------------------------------------------+
|                       Claude Code CLI                              |
|  +---------------+  +---------------+  +-------------------+       |
|  | Input Handler |  | Tool Engine   |  | Output Renderer   |       |
|  +---------------+  +---------------+  +-------------------+       |
+------------------------------------------------------------------+
                               |
                               v
+------------------------------------------------------------------+
|                    Anthropic API (Claude)                          |
|               Models: opus / sonnet / haiku selectable             |
+------------------------------------------------------------------+
```

**Key Point**: Claude doesn't run directly on your computer. It communicates through API, and only tool execution happens locally.

### What is an API? (For Beginners)

**API** (Application Programming Interface) is like a messenger between two programs. When using Claude Code:

1. Your computer sends a message to Anthropic servers
2. Claude (on Anthropic servers) processes the request
3. The response returns to your computer
4. When Claude needs to do something (read files, execute commands, etc.), that happens on your computer

**Why does this matter?**
- Claude can't see your files until you specifically request it
- Every time Claude asks a question = another round trip = more time and cost
- Being specific from the start reduces the number of round trips

### API Communication Cycle

Let's see what happens when you make a request:

```
 [1] Your request                    [2] Send to API
      |                                 |
      v                                 v
+----------+    Message + tool defs   +-------------+
| CLI      | -----------------------> | Anthropic   |
| Client   |                          | API         |
+----------+                          +-------------+
      ^                                 |
      |     Response (text + tool calls)|
      +---------------------------------+
                    [3]

 [4] Execute tool locally            [5] Send result back to API
      |                                 |
      v                                 v
+------------------+              +-------------+
| Bash, Read, etc. | -----------> | Claude      |
| execute          |              | decides next|
+------------------+              +-------------+
```

### What's the Benefit of Knowing This?

```
> Show me all files in this folder
```

When you make this request:
1. Claude queries file list with `Glob` tool -> API round trip 1
2. Receives result and decides next action -> API round trip 2
3. If needed, checks file content with `Read` -> API round trip 3

**One request = can become multiple API round trips**. So being specific reduces round trips and is faster.

---

## Tool System

Claude Code uses 17 built-in tools.

### Complete Tool List

| Category | Tool | Description |
|----------|------|-------------|
| **File Operations** | `Read` | Read files (supports images, PDF, Jupyter notebooks) |
| | `Write` | Create/overwrite files |
| | `Edit` | String replacement-based file editing |
| | `Glob` | Search files by pattern (e.g., `**/*.ts`) |
| | `Grep` | Content search (ripgrep-based) |
| **Execution** | `Bash` | Execute shell commands |
| | `KillShell` | Terminate background shells |
| **Agents** | `Task` | Create and run sub-agents |
| | `TaskOutput` | Get background task results |
| **Web** | `WebFetch` | Fetch URL content (HTML->Markdown) |
| | `WebSearch` | Web search |
| **Planning** | `EnterPlanMode` | Enter plan mode |
| | `ExitPlanMode` | Exit plan mode and request approval |
| **User Interaction** | `AskUserQuestion` | Ask user questions (multiple choice) |
| | `TodoWrite` | Manage task lists |
| **Other** | `Skill` | Execute skills (/commit, /review-pr, etc.) |
| | `NotebookEdit` | Edit Jupyter notebook cells |

### Tool Limitations

| Tool | Limit | Value |
|------|-------|-------|
| **Bash** | Default timeout | 2 minutes |
| | Max timeout | 10 minutes |
| | Output limit | 30,000 characters |
| **Read** | Default lines | 2,000 lines |
| | Line length | 2,000 characters |

### Tool Classification (Visualization)

```
+-----------------------------------------------------------------------------+
|                              Tool Execution Layer                            |
|                                                                              |
|  File Operations --------------------------------------------------------    |
|  +---------+ +---------+ +---------+ +---------+ +---------+                |
|  |  Read   | |  Write  | |  Edit   | |  Glob   | |  Grep   |                |
|  |read file| |create   | |modify   | |search   | |content  |                |
|  |         | |file     | |file     | |files    | |search   |                |
|  +---------+ +---------+ +---------+ +---------+ +---------+                |
|                                                                              |
|  Execution ---------------------------------------------------------------   |
|  +---------+ +-----------------+                                            |
|  |  Bash   | |     Task        |                                            |
|  |command  | | (sub-agent)     |                                            |
|  |execute  | |                 |                                            |
|  +---------+ +-----------------+                                            |
|                                                                              |
|  Web ---------------------------------------------------------------------   |
|  +---------+ +---------+                                                    |
|  |WebFetch | |WebSearch|                                                    |
|  |fetch URL| |web      |                                                    |
|  |         | |search   |                                                    |
|  +---------+ +---------+                                                    |
|                                                                              |
|  User Interaction -------------------------------------------------------    |
|  +---------+ +---------+ +---------------+ +---------------+                |
|  |TodoWrite| |  Skill  | | EnterPlanMode | |AskUserQuestion|                |
|  |task mgmt| |execute  | | plan mode     | |ask user       |                |
|  |         | |skill    | |               | |               |                |
|  +---------+ +---------+ +---------------+ +---------------+                |
+-----------------------------------------------------------------------------+
```

### Tool Execution Flow

When a tool is called, it's processed in this flow:

```
    Claude Response
         |
         v
  +--------------+
  | Parse        |
  | response     |
  | (text +      |
  |  tool calls) |
  +------+-------+
         |
    +----+----+
    v         v
+--------+ +--------+
| Text   | | Tool   |
| output | | execute|
+--------+ +---+----+
               |
         +-----+-----+
         v           v
    +--------+  +--------+
    | Success|  | Failure|
    | result |  | error  |
    +---+----+  +---+----+
        |           |
        +-----+-----+
              v
    +-----------------+
    | Send result to  |
    | API (next turn) |
    +-----------------+
```

### What's the Benefit of Knowing This?

**Different tools are used depending on how you request:**

```
# Uses Glob (fast)
> How many tsx files are in the src folder?

# Uses Bash (unnecessarily complex)
> Use the find command to count tsx files in the src folder
```

For tasks that have dedicated Claude Code tools, it's more efficient to request using those tools.

### AskUserQuestion Tool

This tool allows Claude to ask you questions during task execution.

**When is it used?**
- Clarifying ambiguous requirements
- Choosing between multiple approaches
- Confirming preferences for implementation details

**Example interaction:**

```
Claude: I need clarification about authentication.

+-- Authentication Method -------------------------------------+
| Which authentication method should I use?                    |
|                                                              |
| o JWT (recommended)                                          |
|   Stateless, suitable for APIs                               |
|                                                              |
| o Session-based                                              |
|   Traditional approach, server-side storage                  |
|                                                              |
| o OAuth                                                      |
|   Social login (Google, GitHub)                              |
+--------------------------------------------------------------+
```

**Why is this important?**
Instead of guessing or making assumptions, Claude can ask directly. This leads to better results that match actual requirements.

---

## Sub-Agent System

Complex tasks are divided into multiple "sub-agents" for execution.

### Sub-Agent Architecture

```
                          +-----------------+
                          |   Main Agent    |
                          | (talks with you)|
                          +--------+--------+
                                   |
                    +--------------+--------------+
                    |              |              |
                    v              v              v
         +--------------+ +--------------+ +--------------+
         |   Task 1     | |   Task 2     | |   Task 3     |
         |  (explore)   | |  (analyze)   | |  (execute)   |
         +--------------+ +--------------+ +--------------+
```

### Sub-Agent Types

| Type | Purpose | Available Tools |
|------|---------|-----------------|
| `Explore` | Explore codebase, search files | Read, Glob, Grep (no Edit/Write) |
| `Plan` | Create implementation plans, design architecture | Read, Glob, Grep (no Edit/Write) |
| `Bash` | Git operations, command execution | Bash only |
| `general-purpose` | Complex multi-step tasks | All tools (*) |
| `claude-code-guide` | Claude Code usage guide | Glob, Grep, Read, WebFetch, WebSearch |
| `statusline-setup` | Status line setup | Read, Edit |

### Parallel vs Sequential Execution

```
Parallel Execution (independent tasks):
---------------------------------------------
    +----------+     +----------+     +----------+
    | Agent A  |     | Agent B  |     | Agent C  |
    | (search1)|     | (search2)|     | (search3)|
    +----+-----+     +----+-----+     +----+-----+
         |                |                |
         +----------------+----------------+
                          |
                          v
                   +--------------+
                   | Combine      |
                   | results      |
                   +--------------+

Sequential Execution (dependent tasks):
---------------------------------------------
    +----------+     +----------+     +----------+
    | Agent A  | --> | Agent B  | --> | Agent C  |
    | (analyze)|     | (uses A  |     | (uses B  |
    |          |     |  result) |     |  result) |
    +----------+     +----------+     +----------+
```

### What's the Benefit of Knowing This?

**Large tasks are automatically split:**

```
> Analyze this entire project and create a refactoring plan
```

Internally:
1. `Explore` agent understands project structure
2. `Plan` agent creates the plan
3. Main agent synthesizes results

**Splitting tasks in advance makes them more accurate:**

```
# Do everything at once (agent splits automatically)
> Analyze the project and refactor it

# Step by step (more precise control)
> First, just analyze the project structure
> (after checking result)
> Create a refactoring plan for this part
> (after checking plan)
> Good, execute it
```

---

## Model Selection

Claude Code supports multiple models.

| Model | Characteristics | When to Use |
|-------|-----------------|-------------|
| **Opus** | Smartest, highest cost | Complex design, difficult bugs |
| **Sonnet** | Balanced performance | General tasks (default) |
| **Haiku** | Fast and cheap | Simple tasks, repetitive tasks |

### Practical Strategy

```
# Planning phase: Use smart model
> /model opus
> How should I design this system?

# Implementation phase: Use fast model
> /model sonnet
> Implement according to the plan above
```

You can adjust the **cost vs quality trade-off** according to the situation.

---

## Context Management

### Quality Decreases as Conversations Get Longer

Claude has a limit on the amount of information it can process at once. As conversations get longer:
- It doesn't remember early content well
- Overall context understanding becomes blurry
- Response quality degrades

### Solutions

**1. Compress with /compact**
```
> /compact
```
Summarizes the conversation so far to save tokens.

**2. Separate conversations by topic**
```
# Doing everything in one conversation (contexts get mixed)
> Create authentication feature
> Also modify the DB schema
> Also create the frontend form

# Separating by topic (more focused)
> /clear
> Let's focus on authentication. Create the backend API first.
```

**3. Utilize CLAUDE.md**

CLAUDE.md is preserved even after `/clear`. If you write project rules here, context is preserved even when starting a new conversation.

---

## Permissions and Security

### Security Policy Flow

```
  User Request
       |
       v
+--------------+    No        +--------------+
| Dangerous    | -----------> | Execute      |
| operation?   |              | immediately  |
+------+-------+              +--------------+
       | Yes
       v
+--------------+    Deny      +--------------+
| Request user | -----------> | Cancel       |
| approval     |              | operation    |
+------+-------+              +--------------+
       | Approve
       v
+--------------+
| Execute in   |
| sandbox      |
+--------------+
```

### Why Does It Request Approval?

Claude Code requests approval before potentially dangerous operations:
- File modification/deletion
- Command execution
- External API calls

### Auto-Approval Settings

You can auto-approve frequently used safe tools:

```json
// settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

**Caution**: Be careful with `Edit`, `Write`, `Bash` settings.

### Sandbox Mode

Bash commands run in a sandbox by default. This protects your system while working.

---

## Practical Tips

### 1. Explicitly Mention Tools

```
# Implicit (Claude has to infer)
> How is error handling done in this project?

# Explicit (faster)
> Search for "catch" keyword with grep and show me the error handling patterns
```

### 2. One Thing at a Time

```
# Multiple tasks at once (can cause confusion)
> Create the file, write tests, and commit

# Step by step (more accurate)
> Create the file first
> (after checking) Write tests
> (after checking) Commit
```

### 3. Check Results Before Proceeding

If Claude modified files, check the results before proceeding to the next step. You can fix problems immediately if something went wrong.

---

## Try It Yourself

Simple exercises to understand the tool system:

### Exercise 1: Observe Tools

```
> How many files are in this folder?
```

Observe Claude's response. Did it use `Glob` or `Bash`? The result is the same but efficiency differs.

### Exercise 2: Compare Approaches

Try two requests and feel the difference:

```
# Approach A - Vague
> Tell me about this project

# Approach B - Specific
> Read package.json and tell me what dependencies this project uses
```

Approach B is faster because Claude knows exactly which tool (Read) to use.

### Exercise 3: Count Round Trips

```
> Find all TODO comments in this project and show me a list
```

Observe how many tool calls happen. Each one is an API round trip.

---

## What If It's Not Working?

### Problem: Claude seems slow or stuck

**Possible causes:**
1. Request requires many tool calls (round trips)
2. Processing very large files
3. Network issues with API

**Solutions:**
- Break large requests into smaller steps
- Be specific about what you want
- Check your internet connection
- Start fresh with `/clear` if context is confused

### Problem: Claude uses the wrong tool

**Example:** You requested text search, but Claude runs `grep` via `Bash` instead of using the built-in `Grep` tool.

**Solution:** Explicitly mention tools when needed:
```
# Instead of this
> Search for "TODO" in this project

# Try this
> Search for "TODO" in all files using the grep tool
```

### Problem: Permission denied errors

**Cause:** Claude Code needs permission for dangerous operations.

**Solution:** Press `y` to approve, or configure auto-approval in settings for safe tools.

---

## Common Mistakes

1. **Requesting too much at once**
   - Bad: "Analyze this project, fix all bugs, add tests, and deploy"
   - Good: "First, analyze the project structure"

2. **Not being specific about location**
   - Bad: "Show me the config file"
   - Good: "Show me the tsconfig.json file"

3. **Forgetting context limitations**
   - Long conversations lose context
   - Use `/compact` or `/clear` periodically

4. **Ignoring tool output**
   - Always check what Claude actually did before moving on
   - You can catch mistakes early

5. **Not understanding costs**
   - More round trips = more API calls = higher costs
   - Specific requests are cheaper

---

## Summary

What we learned in this chapter:
- [x] Overall Claude Code architecture and API communication flow
- [x] 18 built-in tools system
- [x] How sub-agents work and parallel execution
- [x] Model selection and context management
- [x] Permissions and security policies

**Key Point**: Claude Code is not magic, it's a system. Understanding the system helps you use it better.

In the next chapter, we'll learn how to customize this system.

Continue to [Chapter 22: Advanced Configuration](../Chapter22-Advanced-Config/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Official Docs](https://docs.anthropic.com/en/docs/claude-code) - Complete feature guide for Claude Code
- [Claude Code Architecture](https://docs.anthropic.com/en/docs/claude-code/overview) - Detailed internal structure explanation
- [Claude Extended Thinking](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking) - Ultrathink feature explanation

**Video Resources:**
- [How LLMs Work (YouTube)](https://www.youtube.com/results?search_query=how+LLM+works+explained) - LLM working principles explained
- [Claude Code Architecture (YouTube)](https://www.youtube.com/results?search_query=claude+code+architecture) - Claude Code structure explained
- [AI Agents Explained (YouTube)](https://www.youtube.com/results?search_query=AI+agents+explained+tutorial) - AI agent concept explained

**Reading Materials:**
- [Building Effective Agents (Anthropic)](https://www.anthropic.com/research/building-effective-agents) - AI agent design principles
- [LLM Architecture Overview](https://www.anthropic.com/news/claude-3-family) - Claude model architecture
- [Context Windows Explained](https://docs.anthropic.com/en/docs/build-with-claude/context-windows) - Understanding context windows

**Community:**
- [Claude Code GitHub](https://github.com/anthropics/claude-code) - Official repository
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) - Reddit community

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
