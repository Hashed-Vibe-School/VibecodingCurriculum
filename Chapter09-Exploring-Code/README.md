# Chapter 09: Exploring Code

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Understand the **importance of code reading** and strategies
- Master **Glob** and **Grep** search tools
- Learn **large codebase** navigation techniques
- Build ability to write **project analysis reports**
- Establish effective **exploration workflows**

---

## ⏱️ Estimated Time

- Reading: **35 minutes**
- Practice: **50 minutes**

---

## 📋 Prerequisites

- Claude Code installed and logged in
- Understanding of effective prompting (Chapter 08)

---

## 🔗 Connection to Previous Chapter

In Chapter 08, you learned effective prompting. Now let's use those skills to **explore** and **understand** codebases. The ability to read and understand code is just as important as the ability to write code!

---

## Why is Code Exploration Important?

Imagine your first day at a new job. The codebase has thousands of files. Reading everything would take weeks. But you need to fix a bug today.

This is where code exploration with Claude shines. Instead of manually hunting through folders, just ask Claude to find what you need.

**Real-world scenarios:**
- You inherited a project and need to understand how it works
- There's a bug somewhere but you don't know which file to look at
- You want to add a feature but need to find similar code to reference
- You're reviewing someone's code and need to understand the context
- You need to maintain legacy code

> 💡 **Beginner Tip**
>
> **Think of Claude as your code tour guide:**
> - Without Claude: Wandering through unfamiliar streets looking for a restaurant
> - With Claude: Having a local friend who knows exactly where everything is
>
> You don't need to read all the code yourself.
> Ask Claude and it will identify the key points and explain them.

---

## The Importance of Code Reading

### Why is Code Reading Important?

Developers **spend more time reading code than writing it**.

| Activity | Time Percentage |
|----------|-----------------|
| Reading/understanding code | 60-70% |
| Writing code | 20-30% |
| Debugging/testing | 10-20% |

The ability to quickly understand code equals productivity!

### Effective Code Reading Strategies

#### 1. Top-Down Approach

Start by grasping the big picture, then dive into details.

```
Step 1: Understand project overview
> What is this project? Explain in one sentence.

Step 2: Understand structure
> Show me the folder structure. Explain each folder's role.

Step 3: Find entry point
> Where does the app start? What's the main file?

Step 4: Explore specific features
> How is the login feature implemented?
```

#### 2. Feature-based Approach

Follow and understand the feature you're interested in.

```
> What happens when the user clicks the button?
> Where does the data flow from and to?
> Where does this API call start and end?
```

#### 3. Backward Tracing Approach

Start from the result and trace back to the cause.

```
> Where is this screen rendered?
> Where does this error message come from?
> Where does this data come from?
```

> 🔥 **Pro Tip**
>
> **The 80/20 Rule of Code Reading:**
>
> **20% of the code** handles **80% of the functionality** in a project.
> Identify that core 20% first.
>
> Questions to find the core:
> - "What are the 5 most important files?"
> - "Where is the business logic?"
> - "How are user requests processed?"

---

## Search Tools: Glob and Grep

Claude Code uses two powerful search tools.

### Glob: Find by File Name

Glob searches for files by name using **patterns**.

#### Basic Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `*.js` | Files ending in .js | app.js, utils.js |
| `*.{ts,tsx}` | .ts or .tsx files | Button.tsx, api.ts |
| `**/*.js` | .js files in all folders | src/app.js, lib/utils.js |
| `src/**/*` | All files in src | src/..., src/components/... |
| `**/test*` | Files starting with test | test.js, testing.md |
| `**/*config*` | Files containing config | tsconfig.json, vite.config.ts |

#### Glob Usage Examples

```
# Find JavaScript/TypeScript files
> Find all TypeScript files in this project

# Find test files
> Where are the test files?
> Find files ending in .test.ts or .spec.ts

# Find config files
> Find all files with config in the name

# Find in specific folder
> Find all .tsx files in src/components
```

> 💡 **Beginner Tip**
>
> **You don't need to memorize patterns!**
>
> Make natural language requests and Claude converts them to appropriate patterns.
> Just say "Find files related to Button" and it works.

### Grep: Find in File Contents

Grep searches for text inside file **contents**.

#### Grep Usage Examples

```
# Find specific function
> Where is the "handleSubmit" function defined?

# Find specific text
> Find all "TODO" comments

# Find specific patterns
> Find all places with console.log remaining

# Find API endpoints
> Find all code containing "/api/"

# Find error messages
> Where does the "User not found" message come from?
```

#### Advanced Grep Patterns

```
# Find function definitions
> Find where "function fetchUser" is defined

# Find import statements
> Find all files that import useState

# Find variable usage
> Find all places that use the isLoading variable

# Regex patterns (advanced)
> Find regex that validates email format
```

### Glob vs Grep Comparison

| Characteristic | Glob | Grep |
|----------------|------|------|
| Search target | File names | File contents |
| Use case | "Find Button.tsx" | "Where is handleClick called?" |
| Question type | "What files exist?" | "Where is this code?" |

> ⚠️ **Warning**
>
> **node_modules is excluded from searches.**
>
> Installed package code is not included in search results.
> This is intentional. It prevents thousands of unnecessary results.

---

## Understanding Project Structure

### Asking About Overall Structure

```
> Explain this project structure
```

Claude analyzes the folder structure and key files to explain.

```
> What does this project do?
```

Analyzes README.md and code to tell you the purpose.

```
> What tech stack is being used?
```

Analyzes package.json and config files to tell you the tech stack.

### Looking at Specific Folders

```
> @src/ What's inside this folder?
```

```
> What's the role of the components folder?
```

```
> What custom hooks are in the hooks folder?
```

### Understanding Entry Points and Flow

```
# Where the app starts
> What's the entry point of this app?
> Find the main function or index file

# Routing structure
> Explain the routing structure of this app
> What pages exist?

# Data flow
> How does user data flow?
> From API call to screen display
```

---

## Exploration Workflow

A systematic order for understanding new projects.

### 5-Step Exploration Workflow

```
+-------------------------------------------------------------+
|  Step 1: Project Overview                                    |
|  > What is this project? Explain briefly.                    |
|  > What's the tech stack?                                    |
+-------------------------------------------------------------+
|  Step 2: Folder Structure                                    |
|  > Show folder structure and explain each folder's role.     |
|  > What are the 3 most important folders?                    |
+-------------------------------------------------------------+
|  Step 3: Entry Point                                         |
|  > Where does this app start? What's the main file?          |
|  > Explain the scripts in package.json                       |
+-------------------------------------------------------------+
|  Step 4: Core Files                                          |
|  > Tell me the 5 most important files                        |
|  > Where is the business logic?                              |
+-------------------------------------------------------------+
|  Step 5: Specific Features                                   |
|  > How is [feature of interest] implemented?                 |
|  > Find related files and explain.                           |
+-------------------------------------------------------------+
```

### Practical Application Example

```
# Step 1: Overview
> What is this project?

Claude: This project is a blog platform built with Next.js.
You can write posts in Markdown, and it supports tag-based categorization and search.

# Step 2: Structure
> Explain the folder structure

Claude:
src/
├── app/          # Next.js App Router (pages)
├── components/   # UI components
├── lib/          # Utility functions
├── content/      # Markdown post files
└── styles/       # CSS styles

# Step 3: Entry point
> Where's the main page?

Claude: src/app/page.tsx is the main page.
It displays the list of recent posts.

# Step 4: Core files
> Tell me the 5 most important files

Claude:
1. src/app/page.tsx - Main page
2. src/lib/posts.ts - Post data processing
3. src/components/PostCard.tsx - Post card UI
4. src/app/posts/[slug]/page.tsx - Post detail page
5. src/lib/markdown.ts - Markdown parsing

# Step 5: Specific feature
> How is the search feature implemented?

Claude: The search page is at @src/app/search/page.tsx.
@src/lib/search.ts implements fuzzy search using Fuse.js.
The search index is generated at build time.
```

---

## Safe Exploration with Plan Mode

Use Plan mode when you want to explore without changing files.

```
> /plan
> Analyze this project. Don't modify, just explain.
```

### What You Can Do in Plan Mode

| Action | Plan Mode | Normal Mode |
|--------|-----------|-------------|
| Reading files | O | O |
| Analyzing structure | O | O |
| Modifying files | X | O |
| Running commands | X | O |
| Creating files | X | O |

### Plan Mode Usage Examples

```
> /plan
> Analyze this project's architecture.
> What patterns are being used.
> Are there areas for improvement.
> Just analyze, don't modify.

Claude: [Provides detailed analysis]
Architecture analysis:
1. Current pattern: Feature-based structure
2. State management: Using React Query
3. Styling: Tailwind CSS
...

Improvement suggestions:
1. Need to strengthen type safety
2. Lack of consistency in error handling
...

[Analysis complete without modifying any files]
```

> 💡 **Beginner Tip**
>
> **When Plan mode is useful:**
>
> - When analyzing a project for the first time
> - When reviewing someone else's code
> - Before deciding what to modify
> - When you don't want to accidentally change files

---

## Navigating Large Codebases

Strategies for when projects are large.

### Limiting Scope

```
# If the entire project is too large
> Only analyze the src/auth/ folder

# Focus on specific feature
> Only find payment-related code

# Specific file types only
> Among TypeScript files, find ones with "user" in the name
```

### Asking Step by Step

```
# Don't read everything at once, gradually
> First, just show me the folder structure.
[Check result]

> Among those, I'll look at the api folder in detail.
[Check result]

> Explain the api/users.ts file.
[Check result]

> In this file, explain the createUser function.
```

### Identify Core First

```
# Start with what's important
> Tell me only the 5 most core files in this project

# Frequently modified files
> Based on git log, tell me the most frequently modified files

# Files with many dependencies
> Find files that are imported by other files the most
```

> 🔥 **Pro Tip**
>
> **Large project navigation strategy:**
>
> 1. **Read README first**: Understand project overview
> 2. **Check package.json**: Understand technologies used
> 3. **Find entry point**: Understand where app starts
> 4. **Focus on area of interest**: Don't need to see everything
> 5. **Gradually expand**: Broaden scope when needed

---

## Exploration Patterns for Specific Situations

### When Finding Bugs

```
> Where does this error message come from?
> Search for "User not found" message

> Find all places that call this function
> List files that use the fetchUser function

> Find where this state changes
> Find all places where isAuthenticated value changes
```

### When Adding Features

```
> Is there similar functionality somewhere?
> How are other API endpoints implemented?

> Find a reference component
> Show me form component examples

> Should I follow this pattern?
> How do I make it consistent with existing code?
```

### When Reviewing Code

```
> Explain the relationship between files changed in this PR

> Could this change affect other places?
> Find places that might have side effects

> What about test coverage?
> Is there code that tests this function?
```

### When Refactoring

```
> Find duplicate code
> Is there similar logic in multiple places?

> Find unused code
> Are there files not imported anywhere?

> Find high-complexity functions
> Are there functions over 50 lines?
```

---

## 🎯 Mini Quiz

### Quiz 1
What is the **correct** difference between Glob and Grep?

A) Glob searches file contents, Grep searches file names
B) Glob searches file names, Grep searches file contents
C) Both search only file names
D) Both search only file contents

<details>
<summary>See Answer</summary>

**Answer: B**

- **Glob**: Searches by file name patterns (e.g., `*.tsx`, `*config*`)
- **Grep**: Searches for text in file contents (e.g., `handleSubmit`, `TODO`)

</details>

### Quiz 2
What is the **best** strategy when first exploring a large project?

A) Read all files from beginning to end
B) Open files randomly
C) Grasp the overall structure first, then gradually look at details
D) Read only documentation, not code

<details>
<summary>See Answer</summary>

**Answer: C**

**Top-Down approach** is effective:
1. Grasp overall overview
2. Understand folder structure
3. Identify core files
4. Detailed analysis of area of interest

</details>

### Quiz 3
What **cannot** you do in Plan mode?

A) Reading files
B) Analyzing project structure
C) Modifying files
D) Requesting code explanations

<details>
<summary>See Answer</summary>

**Answer: C**

**File modification is not possible** in Plan mode.
Only reading and analysis are possible, so you can prevent accidentally changing code.

</details>

---

## 📝 Practice Exercises

### 🟢 Beginner: Exploring Open Source Projects

Clone a small project from GitHub and explore it.

```bash
git clone https://github.com/sindresorhus/ora.git
cd ora
claude
```

```
> /plan
> Explain this project structure
> What are the main files?
> What can I do with this project?
```

### 🟡 Intermediate: File Search Practice

```
# Search by file name
> Find all test files
> Where are the config files?
> Find the README file

# Search by file content
> Find files that use "export"
> Find all "TODO" comments
> Find lines containing "error"
```

### 🔴 Advanced: Codebase Analysis Report

Analyze a medium-sized project and write a report.

```bash
git clone https://github.com/expressjs/express.git
cd express
claude
```

```
> /plan
> Analyze this project and create a report:
> 1. Project purpose
> 2. Tech stack
> 3. Folder structure
> 4. Key file explanations
> 5. Code style characteristics
> 6. Architecture patterns
```

---

## 🏆 Challenge Tasks

### Challenge 1: Large Project Navigation

Analyze a large project like React or Vue.

```bash
git clone https://github.com/facebook/react.git
cd react
claude
```

```
> /plan
> Analyze only the 5 core folders of this project
> Don't look at everything, just the core
```

### Challenge 2: Dependency Graph Analysis

Analyze the module dependencies of a project.

```
> Find the most imported files in this project
> Check if there are circular dependencies
> Draw a dependency graph overview
```

### Challenge 3: History Analysis

Analyze a project using Git history.

```
> Based on git log, tell me the 10 most frequently modified files
> What's the most active area in the last 3 months?
> Who are the main contributors?
```

---

## Real Conversation Examples

### Example 1: Exploring a New Project

```
> /plan
> Give me a 1-minute summary of this project.
> What is it? What are the main files?

Claude: This project is a todo management web application.

Tech stack:
- Next.js 14 (App Router)
- TypeScript
- Prisma (database)
- Tailwind CSS

Main files:
1. src/app/page.tsx - Main page (todo list)
2. src/components/TodoItem.tsx - Individual todo component
3. src/lib/db.ts - Database connection
4. src/app/api/todos/route.ts - API endpoint

How to run: npm run dev
```

### Example 2: Exploring Specific Features

```
> Find authentication-related code

Claude: Found authentication-related files:

File list:
- src/lib/auth.ts (authentication logic)
- src/app/api/auth/[...nextauth]/route.ts (NextAuth setup)
- src/components/LoginForm.tsx (login form)
- src/components/SignupForm.tsx (signup form)
- src/middleware.ts (authentication middleware)

Authentication method: Using NextAuth.js
Supported providers: Google, GitHub, Email
Session storage: JWT method
```

### Example 3: Bug Location Tracking

```
> Where does the "Cannot add to cart" error message come from?

Claude: Found the error message.

Location: src/lib/cart.ts (line 42)

Code:
```typescript
if (product.stock < quantity) {
  throw new Error("Cannot add to cart");
}
```

This error occurs when trying to add more quantity than the product's stock.

Call path:
1. src/components/ProductCard.tsx (addToCart button click)
2. src/hooks/useCart.ts (addItem function)
3. src/lib/cart.ts (validateStock function) <- Error occurs here
```

### Example 4: Tracing Code Flow

```
> What happens when the user clicks the login button?
> Explain the entire flow

Claude: Traced the login flow:

1. Button click (src/components/LoginForm.tsx)
   └─ handleSubmit function called

2. API request (src/lib/api/auth.ts)
   └─ POST /api/auth/login called

3. Server processing (src/app/api/auth/login/route.ts)
   └─ Email/password verification
   └─ JWT token generation

4. Response processing (src/lib/api/auth.ts)
   └─ Token saved to cookie

5. State update (src/contexts/AuthContext.tsx)
   └─ user state updated
   └─ isAuthenticated = true

6. UI update (src/components/Header.tsx)
   └─ Login button → Changes to profile menu

Related files: 7, entire flow takes about 0.5 seconds
```

---

## 🚨 Common Errors and Solutions

### Error 1: "File not found"

**Situation:** Claude can't find a file that definitely exists

**Cause:**
- Not in the correct directory
- File name is different than expected

**Solution:**
```
> Which folder are we in now?
> Find files with Button in the name in the current folder
> Search case-insensitively
```

### Error 2: "Too many results"

**Situation:** Over 100 search results

**Cause:**
- Search scope is too broad

**Solution:**
```
# Narrow scope
> Find only Button files inside the components folder
> Search only in .ts files
> Search only inside the src folder
```

### Error 3: "Can't access project"

**Situation:** Claude can't read project files

**Cause:**
- Claude Code not run from inside the project folder

**Solution:**
```bash
cd /path/to/project
claude
```

### Error 4: "Exploration takes too long"

**Situation:** Very slow responses

**Cause:**
- Project is too large
- Request scope is too broad

**Solution:**
```
# Instead of "everything", be specific
> Just analyze the src folder, not the entire project
> Tell me just the 5 most important files, not all files
```

---

## ❌ Common Mistakes

### 1. Trying to Understand Everything at Once

```
# Bad
> Explain every file in this project

# Good
> First, just show me the folder structure.
> Then I'll pick which part to explore.
```

### 2. Not Using Plan Mode

```
# Risky - Claude might try to "fix" things it finds
> Analyze this project and fix any issues

# Safe - Read only
> /plan
> Analyze this project. Just explain, don't change anything.
```

### 3. Vague Search Requests

```
# Bad
> Find the function

# Good
> Find where the "handleLogin" function is defined
```

### 4. Ignoring Project Context

```
# Bad (in a Python project)
> Find all .js files

# Good
> First, tell me what type of project this is.
> Then find the main source files.
```

### 5. Going Too Deep

```
# Bad
> Trace all dependencies of this function to 10 levels deep

# Good
> Just tell me the functions this function directly calls
> I'll go deeper if needed
```

---

## 🆘 If It Doesn't Work?

| Symptom | Solution |
|---------|----------|
| Can't find file | Check folder location, use more specific name |
| Too many results | Narrow search scope, limit to folder |
| Too slow | Limit scope, "core 5" instead of "everything" |
| Structure is complex | Top-down approach, explore one area at a time |
| Code doesn't make sense | "Explain so a beginner can understand" |

---

## 📚 Glossary

| Term | Description |
|------|-------------|
| **Glob** | File name pattern matching tool |
| **Grep** | File content text search tool |
| **Entry Point** | The first file where the app starts |
| **Dependency** | A relationship where one module references another |
| **RegEx** | Regular expression, text pattern matching syntax |
| **Top-Down** | Approach from the whole to the details |
| **Code Reading** | The activity of reading and understanding code |

---

## ✅ Checklist

Check before finishing your learning:

- [ ] I know the difference between Glob and Grep
- [ ] I can search by file name
- [ ] I can search for text in file contents
- [ ] I understand the 5-step exploration workflow
- [ ] I can safely explore with Plan mode
- [ ] I know large project navigation strategies

---

## Mini Project: Codebase Analysis Report

Analyze an open source project and create a report.

### Goals

- Understand real project structures
- Build code exploration skills
- Build analysis report writing skills

### Choose a Project

```bash
# Recommended projects by size
# Small (under 50 files)
git clone https://github.com/sindresorhus/ora.git

# Medium (100-500 files)
git clone https://github.com/expressjs/express.git

# Large (over 500 files)
git clone https://github.com/facebook/react.git
```

### Analysis Template

```
> /plan
> Analyze this project and create a report:
>
> 1. Project Overview
>    - Purpose and use case
>    - Main features
>
> 2. Tech Stack
>    - Language/framework
>    - Key libraries
>
> 3. Folder Structure
>    - Each folder's role
>    - Structure patterns
>
> 4. Core Files (5)
>    - Filename and role
>    - Reason for importance
>
> 5. Architecture Characteristics
>    - Patterns used
>    - Pros and cons
>
> 6. Code Style
>    - Naming conventions
>    - Notable points
```

### Advanced Analysis (For Experts)

```
> Draw an architecture diagram of this project

> Analyze the dependency graph

> How is test coverage structured?

> Find potential performance bottlenecks
```

---

## ➡️ Next Chapter Preview

In the next chapter, you will learn **Debugging**.

What you'll learn:
- Debugging mindset
- Approaches by error type
- Systematic debugging process
- Finding bugs with Claude

Proceed to [Chapter 10: Editing Code](../Chapter10-Editing-Code/README.md).

---

## 📖 Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Getting Started](https://docs.anthropic.com/en/docs/claude-code) - Official documentation
- [Glob Patterns Guide](https://docs.anthropic.com/en/docs/claude-code/files#glob-patterns) - File pattern matching guide

**Video Resources:**
- [Code Reading Skills (YouTube)](https://www.youtube.com/results?search_query=code+reading+skills+tutorial) - Code reading skill development
- [Codebase Navigation (YouTube)](https://www.youtube.com/results?search_query=codebase+navigation+tips) - Large codebase navigation tips

**Reading Materials:**
- [How to Read Code Effectively](https://www.google.com/search?q=how+to+read+code+effectively) - Code reading strategies
- [Understanding Large Codebases](https://www.google.com/search?q=understanding+large+codebases) - Large project comprehension

**Related Tools:**
- [ripgrep](https://github.com/BurntSushi/ripgrep) - Fast search tool
- [fd](https://github.com/sharkdp/fd) - Fast file search

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
