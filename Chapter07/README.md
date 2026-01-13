# Chapter 07: Exploring Code

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Understanding project structure
- Finding files (Glob)
- Searching code (Grep)

---

## Why is Code Exploration Important?

When starting a new project, you often wonder "what does this code do?"

Instead of reading all the code yourself, ask Claude and it will identify and explain the key points.

**Effective exploration requests:**

```
> Find the authentication-related code in this project
> What does the src/utils folder do?
```

Specify a feature or folder for more accurate answers.

---

## Understanding Project Structure

### Asking About Overall Structure

```
> Explain this project structure
```

```
> What does this project do?
```

```
> What tech stack is being used?
```

Claude analyzes the folder structure and key files to explain.

### Looking at Specific Folders

```
> @src/ What's inside this folder?
```

```
> What's the role of the components folder?
```

---

## Finding Files (Glob)

Glob finds files by name patterns.

### Common Patterns

| Request | What Claude Does |
|---------|------------------|
| "Find all js files" | Searches `*.js` pattern |
| "Find test files" | Searches `*.test.js` or `*.spec.js` |
| "Find config files" | Searches `*.config.*`, `*.json`, `*.yaml` |

### Examples

```
> Find all js files in the src folder
```

```
> Where are the test files?
```

```
> Find package.json
```

### Pattern Syntax (Reference)

No need to memorize this. Make natural language requests and Claude converts them to appropriate patterns.

| Pattern | Meaning |
|---------|---------|
| `*.js` | Files ending in js |
| `**/*.ts` | ts files in all folders |
| `src/**/*` | All files in src |

---

## Searching Code (Grep)

Grep finds specific text inside files.

### Common Searches

```
> Find files that use "useState"
```

```
> Where is the "handleSubmit" function defined?
```

```
> Find all TODO comments
```

### Finding Function Usage

```
> Where is the fetchUser function called?
```

Claude finds all files and locations where that function is used.

### Finding API Endpoints

```
> Find all API endpoints
```

```
> Find paths starting with /api/
```

---

## Exploration Workflow

Order for understanding a new project:

### Step 1: Get the Big Picture

```
> What is this project? Explain briefly.
```

### Step 2: Understand Structure

```
> Show me the folder structure and explain what each folder does.
```

### Step 3: Find Entry Points

```
> Where does this app start? What's the main file?
```

### Step 4: Explore Areas of Interest

```
> How is the login feature implemented?
> Find related files and explain.
```

---

## Safe Exploration with Plan Mode

Use Plan mode when exploring without changing files.

```
> /plan
> Analyze this project. Explain only, do not modify.
```

In Plan mode:
- Reading files: Allowed
- Modifying files: Not allowed
- Running commands: Not allowed

Safe exploration.

---

## Practice

### Practice 1: Exploring Open Source

Clone any project from GitHub and explore.

```bash
git clone https://github.com/expressjs/express.git
cd express
claude
```

```
> /plan
> Explain this project structure
> What are the main files?
> What can I do with this project?
```

### Practice 2: Finding Files

```
> Find all test files
> Where are the config files?
> Find the README file
```

### Practice 3: Searching Code

```
> Find files that use "export"
> Find all "TODO" comments
> Find lines containing "error"
```

---

## Tips for Large Projects

When projects are large:

### Limit Scope

```
> Only analyze the src/auth/ folder
```

Do not look at everything at once. Focus on the part you are interested in.

### Ask Step by Step

```
# Do not read everything
> First, just show me the folder structure.
> I'll look at the api folder in detail from there.
```

---

## Mini Project: Codebase Analysis Report

Analyze an open source project and create a report.

### Goals

- Understand real project structures
- Build code exploration skills

### Choose a Project

```bash
# Recommended projects (by size)
git clone https://github.com/sindresorhus/ora.git      # Small
git clone https://github.com/expressjs/express.git    # Medium
git clone https://github.com/facebook/react.git       # Large
```

### Analyze It

```
> /plan
> Analyze this project and create a report:
> 1. Project purpose
> 2. Tech stack
> 3. Folder structure
> 4. Key file explanations
> 5. Code style characteristics
```

### Advanced Analysis (For Experts)

```
> Draw an architecture diagram of this project

> Analyze the dependency graph

> How is test coverage structured?

> Find potential performance bottlenecks
```

---

## Summary

What you learned in this chapter:
- [x] Understanding project structure
- [x] Finding files (name patterns)
- [x] Searching code (content search)
- [x] Exploration workflow
- [x] Safe exploration with Plan mode

Move on to [Chapter 08: Editing Code](../Chapter08/README.md).
