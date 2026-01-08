# Chapter 02: Code Exploration

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, make sure you can:
- [ ] Use @-mentions to reference files
- [ ] Maintain multi-turn conversations effectively
- [ ] Understand the difference between permission modes

---

## Introduction

One of Claude Code's greatest strengths is its ability to explore and understand codebases. This chapter teaches you how to use Claude's exploration tools to quickly navigate unfamiliar projects, find specific code patterns, and understand complex architectures.

### Why Code Exploration Matters

- **Onboarding**: Quickly understand new projects
- **Debugging**: Locate the source of issues
- **Refactoring**: Find all related code before making changes
- **Learning**: Study how experienced developers structure code

---

## Topics

### 1. Core Exploration Tools

Claude Code uses three primary tools for code exploration:

| Tool | Purpose | Example Use |
|------|---------|-------------|
| **Glob** | Find files by pattern | `*.tsx`, `src/**/*.test.js` |
| **Grep** | Search file contents | Find all uses of a function |
| **Read** | View file contents | Examine specific code |

### 2. Using Glob for File Discovery

Glob helps you find files matching specific patterns:

```bash
# Ask Claude to find files
> Find all TypeScript files in the src directory
> Show me all test files in this project
> List all configuration files (*.config.*, *.json, *.yaml)
```

**Common Glob Patterns**:
| Pattern | Matches |
|---------|---------|
| `*.js` | All JS files in current dir |
| `**/*.ts` | All TS files recursively |
| `src/**/*.test.tsx` | Test files in src |
| `!node_modules/**` | Exclude node_modules |

### 3. Using Grep for Content Search

Grep searches inside files for specific patterns:

```bash
# Ask Claude to search
> Find all files that import "useState"
> Where is the "handleSubmit" function defined?
> Search for TODO comments in the codebase
> Find all API endpoints (look for router.get, router.post, etc.)
```

**Grep Tips**:
- Use regex for complex patterns
- Combine with file type filters
- Search for function definitions vs usages

### 4. Strategic Exploration Patterns

#### Understanding Project Structure
```bash
> What's the overall architecture of this project?
> Explain the folder structure and what each directory contains
> What frameworks and libraries does this project use?
```

#### Tracing Data Flow
```bash
> How does user data flow from the login form to the database?
> Trace the rechapter path for /api/users endpoint
> Where is the state for user authentication managed?
```

#### Finding Dependencies
```bash
> What files import the UserService class?
> Which components use the useAuth hook?
> What would be affected if I change the User type definition?
```

### 5. Exploration Workflow

A systematic approach to understanding code:

```
1. START BROAD
   > What does this project do?
   > What's the tech stack?

2. UNDERSTAND STRUCTURE
   > Show me the folder structure
   > What are the main entry points?

3. IDENTIFY KEY FILES
   > What are the most important files?
   > Where is the main business logic?

4. DEEP DIVE
   > Explain how @src/services/auth.ts works
   > Walk me through the @src/components/Dashboard.tsx component
```

### 6. Plan Mode for Safe Exploration

Use Plan Mode when you only want to explore without making changes:

```bash
# Start in plan mode
claude --permission-mode plan

# Or switch during session
> Switch to plan mode  # or press Shift+Tab twice
```

In Plan Mode:
- Claude can read any file
- No write operations are allowed
- Perfect for learning and auditing

---

## Resources

- [Claude Code Tools Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Glob Pattern Syntax](https://en.wikipedia.org/wiki/Glob_(programming))
- [Regular Expression Guide](https://regexlearn.com/)

---

## Checklist

Answer these questions as if in an interview:

1. **When would you use Glob vs Grep?**
   <details>
   <summary>Hint</summary>
   Glob: finding files by name/path pattern. Grep: searching file contents.
   </details>

2. **How would you find all files that use a specific function?**
   <details>
   <summary>Hint</summary>
   Grep for the function name, filter by import statements vs usage
   </details>

3. **What's the recommended approach for exploring a new codebase?**
   <details>
   <summary>Hint</summary>
   Start broad (architecture), understand structure, identify key files, then deep dive
   </details>

4. **Why is Plan Mode useful for code exploration?**
   <details>
   <summary>Hint</summary>
   Read-only access, no accidental modifications, safe for auditing
   </details>

5. **How would you trace data flow through an application?**
   <details>
   <summary>Hint</summary>
   Start at entry point, follow function calls, track state changes, identify boundaries
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Clone an unfamiliar open-source project and ask Claude to explain its architecture
- [ ] Use Glob to find files by pattern (e.g., all test files, all config files)
- [ ] Use Grep to search for specific patterns in file contents
- [ ] Trace data flow through an application by asking Claude follow-up questions
- [ ] Use Plan Mode to safely explore without making changes

### Try These Prompts

```bash
> What does this project do and what's the tech stack?
> Show me the folder structure and explain each directory
> Find all files that import "useState"
> Where is the main entry point of this application?
> Trace the rechapter path for /api/users endpoint
```

---

## Advanced

- [ ] Learn regex patterns for more powerful Grep searches
- [ ] Explore how Claude handles large codebases (token limits)
- [ ] Practice exploring codebases in languages you don't know
- [ ] Compare exploration strategies: Claude Code vs traditional IDE search
