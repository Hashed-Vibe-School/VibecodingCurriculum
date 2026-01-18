# Chapter 22: Advanced Configuration

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while studying, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn in This Chapter

- Complete understanding of CLAUDE.md 3-tier configuration system
- settings.json detailed options and advanced settings
- Permission management and custom configuration strategies
- Environment-specific settings and team collaboration configuration

---

## Review of Previous Chapter

In Chapter 21, we learned about project structure and basic configuration. In this chapter, we go one step further and cover **advanced configuration**. When you properly understand configuration, Claude Code will understand your work style as perfectly as a long-time colleague.

> **Beginner Tip**: Configuration is a skill you learn once and use forever. It may look complicated at first, but it's not difficult if you follow along step by step!

---

## Why Do You Need This?

**Real-world scenario**: Every time you start Claude Code, you type "Use TypeScript, follow our team conventions, don't use any type..." It's tiring to type this every time, and you sometimes forget.

Configuration files remember the rules once and apply them automatically every time.

### Real-World Applications in the Industry

Let's look at why configuration files are important in real work:

| Situation | Without Config | With Config |
|-----------|----------------|-------------|
| Starting a new project | Explain rules every time | Start working immediately |
| Team member onboarding | Verbally communicate rules | Rules apply automatically |
| Mistake prevention | Can execute dangerous commands | Automatically blocked |
| Work consistency | Different results per person | Always same quality |

### Simple Analogy: Smartphone Settings

Think of configuration like your smartphone settings:
- **Default ringtone** = Claude's default behavior
- **Custom ringtone** = Personal CLAUDE.md
- **Work profile** = Project-specific settings

Set it once, and it applies automatically every time. No need to repeat yourself.

> **Pro Tip**: If you commit configuration files to Git, your entire team can work with the same rules. This is the beginning of "Infrastructure as Code"!

---

## Common Traps Beginners Fall Into

### Trap 1: "I'll do configuration later"

Many people think "I'll code first and configure later." But this leads to:
- Repeatedly typing the same rules every time
- Different results from each team member
- Accidentally executing dangerous commands

> **Warning**: Invest just 10 minutes at the start of a project to create basic settings. You'll save hours later!

### Trap 2: "Configuration files look too complicated"

JSON syntax might look difficult at first. But:
- You only actually use a few options
- You can start with copy-paste
- Error messages will kindly guide you

### Trap 3: "Once configured, I'm done"

Configuration should evolve as your project grows:
- Update when adopting new technologies
- Reflect when team rules change
- Modify when security policies strengthen

---

## JSON Basics (For Beginners)

Many parts of Claude Code configuration use JSON format. Let's learn the basics:

### What is JSON?

JSON (JavaScript Object Notation) is a simple way to store data. It looks like this:

```json
{
  "name": "value",
  "number": 42,
  "boolean": true,
  "list": ["item1", "item2"],
  "nested": {
    "inner": "value"
  }
}
```

> **Beginner Tip**: JSON is similar to Excel's "name: value" format. Think of the left side as the title and the right side as the content!

### Key Rules

1. **Curly braces `{}`** wrap objects (key-value pairs)
2. **Square brackets `[]`** wrap lists (arrays)
3. **Keys must be in double quotes** `"like this"`
4. **Commas separate items** but NO comma after the last item
5. **No comments allowed** (unlike JavaScript)

### Data Types Summary

| Type | Example | Description |
|------|---------|-------------|
| String | `"hello"` | Text wrapped in quotes |
| Number | `42`, `3.14` | Numbers without quotes |
| Boolean | `true`, `false` | True/False |
| Array | `["a", "b"]` | List of multiple values |
| Object | `{"key": "value"}` | Key-value pairs |
| null | `null` | No value |

### Common Mistakes and Solutions

```json
// BAD - trailing comma
{
  "name": "value",  // <-- error here!
}

// GOOD - no trailing comma
{
  "name": "value"
}

// BAD - single quotes
{
  'name': 'value'
}

// GOOD - double quotes
{
  "name": "value"
}

// BAD - using comments
{
  "name": "value"  // this is a comment
}

// GOOD - no comments
{
  "name": "value"
}
```

> **Pro Tip**: When you open a JSON file in VS Code, it shows syntax errors in real-time. If you see a red line, check that part!

---

## Mini Quiz: JSON Basics

Which of the following is valid JSON?

**A:**
```json
{
  'name': 'John'
}
```

**B:**
```json
{
  "name": "John",
}
```

**C:**
```json
{
  "name": "John"
}
```

<details>
<summary>View Answer</summary>

**Answer: C**

- A: Uses single quotes (error)
- B: Has trailing comma (error)
- C: Correct JSON format

</details>

---

## Your First Configuration (Start Here!)

Before diving into all the options, let's create a minimal working configuration:

### Step 1: Create CLAUDE.md in Your Project

Create a `CLAUDE.md` file in your project root:

```markdown
# Project Rules

This is a TypeScript project. Use strict types.
```

That's it! Claude will now read this file and follow your rules.

> **Beginner Tip**: CLAUDE.md is like a "work guidebook" you give to a new employee. When Claude enters the project, it reads this file first!

### Step 2: Create Personal Settings

Create the settings folder and file:

```bash
# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$HOME\.claude"
New-Item -ItemType File -Force -Path "$HOME\.claude\settings.json"

# Mac/Linux
mkdir -p ~/.claude
touch ~/.claude/settings.json
```

Add this minimal config:

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

Now safe read-only tools won't ask for permission every time.

### Step 3: Verify It Works

```
> Tell me what's in my CLAUDE.md
```

Claude should be able to read the file and tell you its contents.

### Real Conversation Example: Checking Configuration

```
User: Tell me what's in my CLAUDE.md

Claude: Let me check the CLAUDE.md file.

Here are the contents of the CLAUDE.md file in your project root:

# Project Rules
This is a TypeScript project. Use strict types.

I'll work according to these rules. When writing TypeScript code,
I'll use strict type definitions and avoid using the any type.

Let me know if you'd like to add more rules!
```

---

## Why Should You Understand Configuration?

Claude Code works well with defaults. But understanding configuration gives you:

- **Consistent work**: No need to repeat the same rules every time
- **Safe usage**: Prevent dangerous actions in advance
- **Custom environment**: Build your own workflow

### ROI of Configuration (Return on Investment)

| Investment Time | Time Saved | ROI |
|-----------------|------------|-----|
| 10 min config | Save 5 min daily | 100 min saved after 20 days |
| 30 min config | Save 10 min daily | 100 min saved after 10 days |
| 1 hour config | Save 15 min daily | Time recovered after 4 days |

> **Pro Tip**: Start by automating the most frequent repetitive tasks. Start small and expand gradually.

---

## CLAUDE.md 3-Tier System

Claude Code reads CLAUDE.md files from 3 levels.

### Hierarchy Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLAUDE.md Priority                           │
└─────────────────────────────────────────────────────────────────┘

[1] Project Level (Highest Priority) ⭐
    ./CLAUDE.md
    └── Rules specific to current project
    └── Example: "Use React 18", "No any type"

[2] Root Level (Medium Priority)
    ~/.claude/CLAUDE.md
    └── Personal rules applied to all projects
    └── Example: "Respond in English", "Commit messages in English"

[3] User Level (Lowest Priority)
    System defaults
    └── Claude Code default behavior
```

### Priority Conflict Resolution

If different rules about the same topic exist at multiple levels:

```markdown
# ~/.claude/CLAUDE.md (Root Level)
Please respond in English for all responses.

# ./CLAUDE.md (Project Level)
Please respond in Korean for all responses.
```

In this case, the **project level** rule takes priority. Claude will respond in Korean.

> **Beginner Tip**: Think of it like company rules < team rules < project rules in terms of priority!

### Why Does This Matter?

**When you need different rules per project:**

```markdown
# Project A's CLAUDE.md
This project uses TypeScript + React.
Tests are written with Jest.
CSS uses Tailwind.

# Project B's CLAUDE.md
This project uses Python + FastAPI.
Tests are written with pytest.
Documentation uses Sphinx.
```

**Common rules go in root level:**

```markdown
# ~/.claude/CLAUDE.md
All commit messages are written in English.
Always check for security vulnerabilities during code review.
Function names start with a verb.
Comments explain "why", code explains "what".
```

### Real Conversation Example: Project-Specific Rules

**In Project A:**
```
User: Create a button component

Claude: I'll create a button component using React, TypeScript, and Tailwind.

// components/Button.tsx
interface ButtonProps {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
  onClick?: () => void;
}

export function Button({
  children,
  variant = 'primary',
  onClick
}: ButtonProps) {
  const baseClasses = "px-4 py-2 rounded font-medium transition-colors";
  const variantClasses = {
    primary: "bg-blue-500 text-white hover:bg-blue-600",
    secondary: "bg-gray-200 text-gray-800 hover:bg-gray-300"
  };

  return (
    <button
      className={`${baseClasses} ${variantClasses[variant]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

**In Project B (same request):**
```
User: Create a button component

Claude: FastAPI projects don't have frontend components.
Are you perhaps wanting to create an API endpoint?

Or I can create an endpoint that returns HTML with a button
using FastAPI's HTMLResponse:

# routes/ui.py
from fastapi import APIRouter
from fastapi.responses import HTMLResponse

router = APIRouter()

@router.get("/button", response_class=HTMLResponse)
async def get_button():
    return """
    <button class="btn btn-primary">
        Click Here
    </button>
    """
```

---

## CLAUDE.md Writing Guide

### Characteristics of Good CLAUDE.md

A good CLAUDE.md includes the following sections:

```markdown
# Project Overview
This project is a user authentication service.
Main features: Login, Registration, Password Reset, Social Login

# Tech Stack
- Backend: Express.js + TypeScript
- Database: PostgreSQL
- ORM: Prisma
- Cache: Redis
- Testing: Jest + Supertest

# Coding Conventions
- Function names: camelCase (e.g., getUserById)
- File names: kebab-case (e.g., user-service.ts)
- Types/Interfaces: PascalCase (e.g., UserProfile)
- Constants: UPPER_SNAKE_CASE (e.g., MAX_LOGIN_ATTEMPTS)

# Architecture Rules
- Controller → Service → Repository layer structure
- Business logic only in service layer
- Database access only in repository layer

# Common Commands
- npm run dev: Start dev server
- npm test: Run tests
- npm run lint: Check lint
- npm run build: Production build

# Important Files
- src/config/: Environment settings (DB, cache, external APIs)
- src/middleware/: Auth, logging, error handling middleware
- prisma/schema.prisma: DB schema definition

# Things to Never Do
- Don't log sensitive info with console.log
- Don't use any type
- Don't hardcode passwords/API keys
```

### Effective Request Tips

```markdown
# Instead of this
"Handle errors well"

# Be specific like this
## Error Handling Rules
- All API responses use { success, data, error } format
- HTTP error codes: only 400/401/403/404/500
- Error logs recorded with winston
- User-facing messages and developer logs are separate
```

> **Warning**: If CLAUDE.md is too long, Claude might miss important rules. Keep it concise with key rules!

### Bad CLAUDE.md vs Good CLAUDE.md

**Bad Example:**
```markdown
# Project
It's a React project. Make it well. No bugs. Fast.
```

**Good Example:**
```markdown
# Project: E-commerce Admin Dashboard

## Tech Stack
- React 18 + TypeScript 5
- State management: Zustand
- Styling: Tailwind CSS + shadcn/ui
- Data fetching: TanStack Query

## Code Style
- Components: Functional + Arrow functions
- Props type: interface (not type)
- Event handlers: handle prefix (handleClick, handleSubmit)

## Performance Rules
- Always use key in list rendering
- Use useMemo for heavy calculations
- Use useCallback for callbacks

## Testing
- Components: React Testing Library
- Utilities: Jest
- Coverage target: 80% or higher
```

---

## settings.json Detailed Configuration

### File Location

```
# Global settings (applied to all projects)
~/.claude/settings.json

# Project settings (applied only to that project)
./.claude/settings.json
```

### Full Structure

```json
{
  "permissions": {
    "autoApprove": [],
    "deny": []
  },
  "model": {
    "default": "sonnet",
    "preferredForPlanning": "opus"
  },
  "behavior": {
    "autoCompact": true,
    "compactThreshold": 80000
  },
  "output": {
    "format": "markdown",
    "verbosity": "normal"
  }
}
```

### Permission Settings Details

```json
{
  "permissions": {
    // Tools to auto-approve - no need to press "y" every time
    "autoApprove": [
      "Read",      // File reading - safe
      "Glob",      // File search - safe
      "Grep",      // Content search - safe
      "WebFetch"   // URL fetching - safe
    ],

    // Patterns to always block - prevent mistakes
    "deny": [
      "rm -rf",           // Recursive delete
      "DROP TABLE",       // DB table deletion
      "DROP DATABASE",    // DB deletion
      "force push",       // Force push
      "--no-verify"       // Hook bypass
    ]
  }
}
```

### Tool Risk Analysis

| Tool | Risk Level | Auto-Approve Recommended | Description |
|------|------------|--------------------------|-------------|
| Read | Low | Yes | Only reads files |
| Glob | Low | Yes | Only finds files |
| Grep | Low | Yes | Only searches content |
| WebFetch | Low | Yes | Only reads URLs |
| Edit | Medium | Depends | Modifies files |
| Write | Medium | Depends | Creates new files |
| Bash | High | No | Can execute any command |

> **Beginner Tip**: Start by auto-approving only Read, Glob, and Grep. Add more as you get comfortable!

### Why Does This Matter?

**Reduce approval fatigue:**

Auto-approving read-only tools means no need to press "y" every time.

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

**Block dangerous commands:**

Block commands that shouldn't be run by mistake.

```json
{
  "permissions": {
    "deny": [
      "rm -rf /",
      "rm -rf ~",
      "DROP DATABASE",
      "git push --force origin main",
      "git reset --hard"
    ]
  }
}
```

### Real Conversation Example: Blocking in Action

```
User: Delete this folder

Claude: I tried to execute the rm -rf command,
but it was blocked by the deny rule in settings.json.

This command is protected because it can be dangerous.
If you really want to delete it, run it directly in the terminal
or modify the deny rule.

Would you like me to show you the list of files to delete first instead?
```

---

## Model Settings

### Specify Default Model

```json
{
  "model": {
    "default": "sonnet"
  }
}
```

### Model Strategy by Task

```json
{
  "model": {
    "default": "sonnet",
    "preferredForPlanning": "opus",
    "preferredForSimpleTasks": "haiku"
  }
}
```

### Model Characteristics Comparison

| Model | Strengths | Weaknesses | Recommended Tasks |
|-------|-----------|------------|-------------------|
| **Opus** | Deep thinking, complex problems | Slow, high cost | Architecture design, complex refactoring |
| **Sonnet** | Balanced performance | Mid-level | Most development work |
| **Haiku** | Fast, cheap | Weak on complex problems | Simple tasks, quick questions |

### Why Does This Matter?

**Auto-adjust cost and quality:**

- Complex planning: Opus (quality first)
- General work: Sonnet (balanced)
- Simple tasks: Haiku (speed first)

```
> /model opus
> Design this system architecture

> /model haiku
> Add logging to this function
```

> **Pro Tip**: If you're worried about API costs at month-end, set Haiku as default and use Sonnet/Opus only for complex tasks!

---

## Output Settings

### Format Settings

```json
{
  "output": {
    "format": "markdown",
    "codeBlockStyle": "fenced",
    "verbosity": "normal"
  }
}
```

### Verbosity Control

```json
{
  "output": {
    // "minimal": essentials only - for experienced users
    // "normal": default - for general users
    // "verbose": detailed explanations - for learners
    "verbosity": "normal"
  }
}
```

### Response Examples by Verbosity

**minimal:**
```
Function added.
```

**normal:**
```
Added formatDate function to utils.ts.
Converts ISO format date strings to "YYYY-MM-DD" format.
```

**verbose:**
```
Added formatDate function to utils.ts.

## Function Description
The formatDate function converts ISO 8601 format date strings
(e.g., "2024-01-15T09:30:00Z") to
Korean format "YYYY년 MM월 DD일".

## Implementation Details
- Uses Date object's toLocaleDateString method
- Locale: 'ko-KR'
- Options: year, month, day set to numeric

## Usage Example
formatDate("2024-01-15T09:30:00Z")
// Result: "2024년 1월 15일"

## Test Suggestion
Would you like me to add unit tests for this function?
```

---

## Environment-Specific Settings

### Development Environment Settings

```json
// ~/.claude/settings.json (for development)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep", "Edit", "Write", "Bash"]
  },
  "behavior": {
    "sandbox": false
  },
  "output": {
    "verbosity": "verbose"
  }
}
```

> **Warning**: Even in development environments, put dangerous commands like `rm -rf` and `DROP` in the deny list!

### Production Environment Settings

```json
// ~/.claude/settings.json (for production)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "rm",
      "DROP",
      "DELETE",
      "TRUNCATE",
      "force",
      "--hard",
      "sudo"
    ]
  },
  "behavior": {
    "sandbox": true
  },
  "output": {
    "verbosity": "normal"
  }
}
```

### Environment-Specific File Structure

```
~/.claude/
├── settings.json          # Default (for development)
├── settings.prod.json     # Production backup
└── settings.safe.json     # Safe mode backup
```

Swap files when needed:
```bash
# When working in production
cp ~/.claude/settings.prod.json ~/.claude/settings.json

# When returning to development
cp ~/.claude/settings.dev.json ~/.claude/settings.json
```

> **Pro Tip**: Always apply safe settings when using Claude Code on production servers!

---

## Project-Specific Settings

### Using the .claude/ Folder

```
my-project/
├── .claude/
│   ├── settings.json    # Project-specific settings
│   ├── commands/        # Project-specific commands
│   └── templates/       # Frequently used prompts
├── CLAUDE.md            # Project rules
└── src/
```

### Project-Specific settings.json

```json
// my-project/.claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "npm publish",           // Prevent accidental publish
      "git push origin main",  // Prevent direct push to main
      "prisma migrate deploy"  // Prevent production migration
    ]
  }
}
```

### Settings Priority

```
Project settings > User settings > System defaults

./.claude/settings.json
      ↓ (overrides)
~/.claude/settings.json
      ↓ (overrides)
System defaults
```

---

## Practical Configuration Examples

### Frontend Project

```markdown
# CLAUDE.md
## Project
React + TypeScript + Tailwind CSS based dashboard

## Conventions
- Components are functional + arrow functions
- State management with Zustand
- Styles use Tailwind classes (no inline styles)
- Icons use lucide-react

## Component Structure
src/components/
├── ui/          # Base UI components (Button, Input, etc.)
├── layout/      # Layout components (Header, Sidebar, etc.)
└── features/    # Feature-specific components

## Testing
- Test files: *.test.tsx
- Use React Testing Library
- At least 1 test per component

## Forbidden
- No any type
- No inline style
- No console.log in commits
- No hardcoded colors (use Tailwind)
```

```json
// .claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      ": any",
      "style={{",
      "console.log",
      "#[0-9a-fA-F]{6}"
    ]
  }
}
```

### Backend Project

```markdown
# CLAUDE.md
## Project
Express + TypeScript + Prisma based REST API

## API Rules
- Follow RESTful design principles
- Response format: { success: boolean, data?: T, error?: string }
- Authentication: JWT Bearer token
- Pagination: { page, limit, total, items }

## Layer Structure
src/
├── controllers/  # HTTP request handling
├── services/     # Business logic
├── repositories/ # Data access
├── middleware/   # Auth, logging, error handling
└── utils/        # Utility functions

## Security
- Watch for SQL injection (auto-prevented with Prisma)
- Input validation required (use zod)
- Don't log sensitive info (password, token, apiKey)
- Apply rate limiting

## DB
- Migration: prisma migrate dev
- Seed: prisma db seed
- Never run prisma migrate deploy in dev environment
```

```json
// .claude/settings.json
{
  "permissions": {
    "deny": [
      "DROP TABLE",
      "DROP DATABASE",
      "DELETE FROM",
      "TRUNCATE",
      "console.log(password",
      "console.log(token",
      "console.log(apiKey",
      "prisma migrate deploy"
    ]
  }
}
```

### Full-Stack Project

```markdown
# CLAUDE.md
## Structure
my-app/
├── frontend/     # Next.js 14 (App Router)
├── backend/      # NestJS
├── shared/       # Shared type definitions
└── infrastructure/ # Docker, K8s config

## Dev Servers
- frontend: npm run dev (port 3000)
- backend: npm run start:dev (port 4000)
- Run simultaneously: npm run dev:all

## Environment Variables
- .env.local: Local settings (not in git)
- .env.example: Template (required variables list)
- .env.test: Test settings

## Type Sharing
Import types from shared/types.ts on both sides
```

---

## Configuration Debugging

### Check Current Settings

```
> /config
```

Shows currently applied settings.

### Checklist When Settings Don't Work

1. **Check file location**:
   ```bash
   ls ~/.claude/settings.json
   ls ./.claude/settings.json
   ```

2. **Check JSON syntax**:
   ```bash
   # Mac/Linux
   cat ~/.claude/settings.json | jq .

   # Windows PowerShell
   Get-Content ~/.claude/settings.json | ConvertFrom-Json
   ```

3. **Restart Claude Code**: May be needed after settings changes

4. **Clear cache**:
   ```bash
   # Completely quit Claude Code and restart
   ```

### Real Conversation Example: Troubleshooting

```
User: I set up auto-approve but it keeps asking

Claude: It seems the settings aren't being applied properly.
Let me check a few things.

1. Checking settings file location:
   Does the ~/.claude/settings.json file exist?

2. Checking JSON format:
   Let me validate with jq...

Error found! There's a trailing comma on line 12:
"autoApprove": ["Read", "Glob",]
                            ↑ Delete this comma

Please restart Claude Code after fixing this.
```

---

## Practice Exercises

### Exercise 1: Create Your First CLAUDE.md (Beginner)

1. Navigate to your project folder
2. Create a `CLAUDE.md` file with the following content:
   ```markdown
   # My Project

   ## Rules
   - All responses in English
   - Add comments to code
   ```
3. Start Claude Code and ask "What are the rules for this project?"

**Success Criteria**: Claude reads and responds with the rules from CLAUDE.md.

### Exercise 2: Set Up Auto-Approve (Beginner)

1. Create `~/.claude/settings.json`
2. Add auto-approve for read-only tools:
   ```json
   {
     "permissions": {
       "autoApprove": ["Read", "Glob", "Grep"]
     }
   }
   ```
3. Restart Claude Code and ask "What files are in the src folder?"

**Success Criteria**: Shows file list without asking for approval each time.

### Exercise 3: Add Blocking Patterns (Intermediate)

Add patterns to block dangerous commands:

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "rm -rf",
      "DROP",
      "DELETE FROM"
    ]
  }
}
```

**Test Method**:
- Try asking Claude to delete something with `rm -rf`
- Success if Claude shows a blocking message!

### Exercise 4: Create Project-Specific Settings (Advanced)

1. Create a `.claude/` folder in your project
2. Write a project-specific settings.json
3. Commit with CLAUDE.md to Git
4. Verify different settings apply in different projects

---

## Challenge Tasks

### Challenge 1: Write a Perfect CLAUDE.md

Write a perfect CLAUDE.md for your actual project.
Include:
- Project overview
- Tech stack
- Coding conventions
- Architecture rules
- Forbidden items

### Challenge 2: Create a Team Configuration Package

Create a configuration package that can be shared with your team:
- `.claude/` folder structure
- settings.json
- CLAUDE.md
- README (usage instructions)

### Challenge 3: Create an Environment Settings Switcher

Create a script that applies different settings for dev/test/production environments.

---

## Troubleshooting

### Problem: CLAUDE.md is not being read

**Possible causes:**
1. File is not named exactly `CLAUDE.md` (case-sensitive)
2. File is in the wrong location
3. File encoding issue (must be UTF-8)

**Solutions:**
- Check file name: `ls -la CLAUDE.md`
- Verify it's in the project root (where you run `claude`)
- Check content: `cat CLAUDE.md`

### Problem: settings.json changes not applied

**Possible causes:**
1. Invalid JSON syntax
2. File in wrong location
3. Claude Code needs restart

**Solutions:**
- Validate JSON: `cat ~/.claude/settings.json | jq .`
- Check location: must be `~/.claude/settings.json`
- Restart Claude Code

### Problem: Auto-approve not working

**Possible causes:**
1. Tool name spelled wrong (case-sensitive)
2. JSON syntax error

**Solutions:**
- Check exact tool names: `Read`, `Glob`, `Grep`, `Edit`, `Write`, `Bash`
- Validate JSON format

---

## Common Mistakes

1. **Trailing commas in JSON**
   ```json
   // BAD
   { "autoApprove": ["Read",] }

   // GOOD
   { "autoApprove": ["Read"] }
   ```

2. **Wrong file location**
   - Settings: `~/.claude/settings.json` (NOT `~/settings.json`)
   - Project rules: `./CLAUDE.md` in project root

3. **Case sensitivity**
   - `CLAUDE.md` is not the same as `claude.md`
   - `Read` is not the same as `read`

4. **Being too restrictive**
   - Denying too many patterns can make Claude useless
   - Start permissive, add restrictions as needed

5. **Forgetting project-specific needs**
   - What works for one project might break another
   - Use project-level `.claude/settings.json` for exceptions

---

## Glossary

| Term | Description |
|------|-------------|
| **CLAUDE.md** | Markdown file that tells Claude the project rules |
| **settings.json** | JSON file that configures Claude Code behavior |
| **autoApprove** | List of tools to auto-approve |
| **deny** | List of command patterns to block |
| **JSON** | JavaScript Object Notation, a data format |
| **Markdown** | Simple document format (.md files) |

---

## Summary

What you learned in this chapter:
- [x] CLAUDE.md 3-tier system (project/root/user)
- [x] settings.json detailed options
- [x] Permission management (auto-approve/deny)
- [x] Environment-specific settings strategies
- [x] Project-specific custom settings
- [x] Configuration debugging methods

**Key Point**: Good configuration reduces repetitive work and prevents mistakes.

---

## Next Chapter Preview

In Chapter 23, you'll learn about **Hooks and Commands**:
- Automatically run lint when files are modified
- Save frequently used prompts as commands
- Full automation of repetitive tasks

Now that you've mastered configuration, you're ready to enter the world of automation!

Proceed to [Chapter 23: Hooks & Commands](../Chapter23-Hooks-and-Commands/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
