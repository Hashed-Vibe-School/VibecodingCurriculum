# Chapter 19: Advanced Configuration

**English** | [한국어](./README.ko.md)

## What You Will Learn

- CLAUDE.md 3-tier configuration system
- settings.json detailed options
- Permission management and custom settings

---

## Why do you need this?

**Real-world scenario**: Every time you start Claude Code, you type "Use TypeScript, follow our team conventions, don't use any type..." This gets tiring and you sometimes forget.

Configuration files solve this by remembering your rules once and applying them every time.

### Simple Analogy: Your Phone Settings

Think of configuration like your phone settings:
- **Default ringtone** = Claude's default behavior
- **Custom ringtone** = Your personal CLAUDE.md
- **Work profile** = Project-specific settings

You set it once, and it just works every time. No need to repeat yourself!

---

## JSON Basics (For Beginners)

Many Claude Code settings use JSON format. Here's a quick primer:

### What is JSON?

JSON (JavaScript Object Notation) is a simple way to store data. It looks like this:

```json
{
  "name": "value",
  "number": 42,
  "list": ["item1", "item2"],
  "nested": {
    "inner": "value"
  }
}
```

### Key Rules

1. **Curly braces `{}`** wrap objects (key-value pairs)
2. **Square brackets `[]`** wrap lists (arrays)
3. **Keys must be in double quotes** `"like this"`
4. **Commas separate items** but NO comma after the last item
5. **No comments allowed** (unlike JavaScript)

### Common Mistakes

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
```

---

## Your First Config (Start Here!)

Before diving into all the options, let's create a minimal working config:

### Step 1: Create CLAUDE.md in Your Project

Create a file called `CLAUDE.md` in your project root:

```markdown
# Project Rules

This is a TypeScript project. Use strict types.
```

That's it! Claude will now read this file and follow your rules.

### Step 2: Create Your Personal Settings

Create the settings folder and file:

```bash
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
> What's in my CLAUDE.md?
```

Claude should be able to read it and tell you the contents.

---

## Why Understand Configuration?

Claude Code works well with defaults. But understanding configuration gives you:

- **Consistent work**: No need to repeat the same rules every time
- **Safe usage**: Prevent dangerous actions in advance
- **Custom environment**: Build your own workflow

---

## CLAUDE.md 3-Tier System

Claude Code reads CLAUDE.md files from 3 levels.

### Hierarchy Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLAUDE.md Priority                            │
└─────────────────────────────────────────────────────────────────┘

[1] Project Level (Highest Priority)
    ./CLAUDE.md
    └── Rules specific to current project

[2] Root Level (Medium Priority)
    ~/.claude/CLAUDE.md
    └── Personal rules applied to all projects

[3] User Level (Lowest Priority)
    System defaults
    └── Claude Code default behavior
```

### Why Does This Matter?

**When you need different rules per project:**

```markdown
# Project A's CLAUDE.md
This project uses TypeScript + React.
Tests are written with Jest.

# Project B's CLAUDE.md
This project uses Python + FastAPI.
Tests are written with pytest.
```

**Common rules go in root level:**

```markdown
# ~/.claude/CLAUDE.md
All commit messages are written in English.
Always check for security vulnerabilities during code review.
```

---

## CLAUDE.md Writing Guide

### Characteristics of Good CLAUDE.md

```markdown
# Project Overview
This project is a user authentication service.

# Tech Stack
- Backend: Express.js + TypeScript
- Database: PostgreSQL
- ORM: Prisma

# Coding Conventions
- Function names: camelCase
- File names: kebab-case
- Types: PascalCase

# Common Commands
- npm run dev: Start dev server
- npm test: Run tests
- npm run lint: Check lint

# Important Files
- src/config/: Environment settings
- src/middleware/: Auth, logging middleware
- prisma/schema.prisma: DB schema
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
```

---

## settings.json Detailed Configuration

### File Location

```
~/.claude/settings.json
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

### Permission Settings

```json
{
  "permissions": {
    // Tools to auto-approve
    "autoApprove": [
      "Read",      // File reading
      "Glob",      // File search
      "Grep",      // Content search
      "WebFetch"   // URL fetching
    ],

    // Patterns to always block
    "deny": [
      "rm -rf",
      "DROP TABLE",
      "force push"
    ]
  }
}
```

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
      "DROP DATABASE",
      "git push --force origin main"
    ]
  }
}
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
    // "minimal": essentials only
    // "normal": default
    // "verbose": detailed explanations
    "verbosity": "normal"
  }
}
```

---

## Environment-Specific Settings

### Development Environment

```json
// ~/.claude/settings.json (for development)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep", "Edit", "Write", "Bash"]
  },
  "behavior": {
    "sandbox": false
  }
}
```

### Production Environment

```json
// ~/.claude/settings.json (for production)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": ["rm", "DROP", "DELETE", "force"]
  },
  "behavior": {
    "sandbox": true
  }
}
```

### Why Does This Matter?

**Safety level matching the environment:**

- Development: Auto-approve many things for fast work
- Production: Strict limits to prevent mistakes

---

## Project-Specific Settings

### Using .claude/ Folder

```
my-project/
├── .claude/
│   ├── settings.json    # Project-specific settings
│   └── templates/       # Frequently used prompts
├── CLAUDE.md           # Project rules
└── src/
```

### Project-Specific settings.json

```json
// my-project/.claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": ["npm publish"]
  }
}
```

---

## Practical Configuration Examples

### Frontend Project

```markdown
# CLAUDE.md
## Project
React + TypeScript + Tailwind CSS

## Conventions
- Components are functional
- State management with Zustand
- Styles use Tailwind classes

## Testing
- Test files: *.test.tsx
- Use React Testing Library

## Forbidden
- No any type
- No inline styles
```

```json
// .claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [": any", "style={{"]
  }
}
```

### Backend Project

```markdown
# CLAUDE.md
## Project
Express + TypeScript + Prisma

## API Rules
- RESTful design
- Response: { success, data, error }
- Auth: JWT Bearer tokens

## Security
- Watch for SQL injection
- Input validation required
- No logging sensitive info

## DB
- Migration: prisma migrate
- Seed: prisma db seed
```

```json
// .claude/settings.json
{
  "permissions": {
    "deny": [
      "DROP TABLE",
      "DELETE FROM",
      "console.log(password",
      "console.log(token"
    ]
  }
}
```

### Full-Stack Project

```markdown
# CLAUDE.md
## Structure
- frontend/: Next.js
- backend/: NestJS
- shared/: Shared types

## Dev Servers
- frontend: npm run dev (port 3000)
- backend: npm run start:dev (port 4000)

## Environment Variables
- .env.local: Local config (not in git)
- .env.example: Template
```

---

## Configuration Debugging

### Check Current Settings

```
> /config
```

Shows currently applied settings.

### When Settings Don't Work

1. **Check file location**: `~/.claude/settings.json`
2. **Check JSON syntax**: commas, quotes, etc.
3. **Restart Claude Code**: May be needed after changes

```bash
# Validate JSON syntax
cat ~/.claude/settings.json | jq .
```

---

## Try it yourself

### Exercise 1: Create Your First CLAUDE.md

1. Navigate to a project folder
2. Create a `CLAUDE.md` file with your project rules
3. Start Claude Code and ask "What are the rules for this project?"

### Exercise 2: Set Up Auto-Approve

1. Create `~/.claude/settings.json`
2. Add auto-approve for read-only tools
3. Try searching for files - notice you don't need to press 'y' anymore!

### Exercise 3: Add a Deny Pattern

Add a pattern to block dangerous commands:

```json
{
  "permissions": {
    "deny": ["rm -rf"]
  }
}
```

Try asking Claude to delete something with `rm -rf` - it should be blocked!

---

## If it doesn't work?

### Problem: CLAUDE.md is not being read

**Possible causes:**
1. File is not named exactly `CLAUDE.md` (case-sensitive)
2. File is in the wrong location
3. File has encoding issues

**Solutions:**
- Check the file name: `ls -la CLAUDE.md`
- Make sure it's in the project root (where you run `claude`)
- Verify content: `cat CLAUDE.md`

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

## Common mistakes

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

## Summary

What you learned in this chapter:
- [x] CLAUDE.md 3-tier system (project/root/user)
- [x] settings.json detailed options
- [x] Permission management (auto-approve/deny)
- [x] Environment-specific settings strategy
- [x] Project-specific custom settings

**Key point**: Good configuration reduces repetitive work and prevents mistakes.

In the next chapter, you'll learn more powerful automation with Hooks and Commands.

Proceed to [Chapter 20: Hooks & Commands](../Chapter20/README.md).
