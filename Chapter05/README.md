# Chapter 05: Project Memory

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, make sure you can:
- [ ] Navigate and explore codebases with Claude
- [ ] Make code edits and commits
- [ ] Understand the concept of context in LLM conversations

---

## Introduction

Every project has tribal knowledge—coding conventions, architectural decisions, team preferences—that new team members (and AI assistants) need to learn. Claude Code's project memory system lets you capture this knowledge so Claude understands your project from the start.

### Why Project Memory Matters

Without memory, you'd repeat the same instructions every session:
- "We use Tailwind CSS for styling"
- "Always use TypeScript strict mode"
- "Follow the existing file structure"

With CLAUDE.md, Claude knows this automatically.

---

## Topics

### 1. CLAUDE.md Overview

CLAUDE.md is a special markdown file that Claude reads at the start of every session.

**Locations** (in priority order):
1. `./CLAUDE.md` - Project root (highest priority)
2. `./.claude/CLAUDE.md` - Hidden claude folder
3. `~/.claude/CLAUDE.md` - User global (lowest priority)

### 2. What to Include

#### Project Architecture
```markdown
# Project Overview

## Tech Stack
- Frontend: React 18 + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL with Prisma ORM
- Styling: Tailwind CSS

## Directory Structure
- `src/components/` - React components
- `src/pages/` - Page components (Next.js routing)
- `src/api/` - API route handlers
- `src/lib/` - Utility functions
```

#### Coding Conventions
```markdown
## Coding Standards

### TypeScript
- Use strict mode
- Prefer interfaces over types for objects
- Always define return types for functions

### Naming
- Components: PascalCase (UserProfile.tsx)
- Utilities: camelCase (formatDate.ts)
- Constants: UPPER_SNAKE_CASE

### File Organization
- One component per file
- Co-locate tests with source files
- Group by feature, not type
```

#### Team Preferences
```markdown
## Preferences

- Use functional components with hooks (no class components)
- Prefer named exports over default exports
- Write JSDoc comments for public APIs only
- Keep components under 200 lines
```

#### Forbidden Patterns
```markdown
## Do NOT

- Don't use `any` type in TypeScript
- Don't commit .env files
- Don't use inline styles (use Tailwind)
- Don't add new dependencies without discussion
```

### 3. Effective CLAUDE.md Structure

```markdown
# Project Name

## Quick Reference
[Most important things Claude should know]

## Architecture
[High-level system design]

## Conventions
[Coding standards and patterns]

## Common Tasks
[How to run, test, deploy]

## Gotchas
[Things that might be confusing]
```

### 4. Team vs Personal Memory

| File | Scope | Use Case |
|------|-------|----------|
| `./CLAUDE.md` | Project (git tracked) | Team-wide standards |
| `~/.claude/CLAUDE.md` | Personal | Individual preferences |

**Personal preferences example**:
```markdown
# My Preferences

- I prefer verbose explanations
- Always include examples in code comments
- I use Vim keybindings
```

### 5. Memory Best Practices

> **Pro Tip**: "Share CLAUDE.md in git, and update it whenever Claude does something wrong. This creates a feedback loop that improves Claude's behavior over time."

#### Keep It Concise
- Claude reads this every session
- Long files waste tokens
- Link to detailed docs instead of inline

#### Update Regularly
- Add new conventions as they evolve
- Remove outdated information
- Review quarterly
- **Update when Claude makes mistakes** - Add rules to prevent repeated errors

#### Be Specific
```markdown
# Bad
Use good variable names

# Good
Use descriptive variable names:
- `userData` not `d`
- `isLoading` not `loading`
- `handleSubmit` not `submit`
```

### 6. Memory Hierarchy

Claude combines memory from multiple sources:

```
1. Project CLAUDE.md (specific rules)
      ↓
2. Parent directory CLAUDE.md (inherited)
      ↓
3. User CLAUDE.md (personal preferences)
```

---

## Resources

- [Claude Code Memory Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Markdown Guide](https://www.markdownguide.org/)

---

## Checklist

Answer these questions as if in an interview:

1. **What is CLAUDE.md and where should it be placed?**
   <details>
   <summary>Hint</summary>
   Project memory file, can be in root, .claude folder, or home directory
   </details>

2. **What types of information should you include?**
   <details>
   <summary>Hint</summary>
   Tech stack, conventions, preferences, forbidden patterns, common tasks
   </details>

3. **How does the memory hierarchy work?**
   <details>
   <summary>Hint</summary>
   Project > Parent directories > User global, more specific overrides general
   </details>

4. **What's the difference between project and personal memory?**
   <details>
   <summary>Hint</summary>
   Project: git tracked, team-wide. Personal: individual preferences, not shared.
   </details>

5. **How should you balance detail vs brevity in CLAUDE.md?**
   <details>
   <summary>Hint</summary>
   Concise to save tokens, link to docs for detail, update regularly
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Create a CLAUDE.md file in your project root with tech stack and directory structure
- [ ] Add at least 5 coding conventions and 3 "do not" rules to your CLAUDE.md
- [ ] Start a new Claude session and verify Claude understands your project conventions
- [ ] Create a personal memory file at `~/.claude/CLAUDE.md` with your preferences
- [ ] Ask Claude to add a feature and verify it follows the conventions in CLAUDE.md

### Try These Prompts

```bash
> Explain this project based on the CLAUDE.md
> Add a new component following our coding conventions
> What are the "do not" rules for this project?
> Show me how to run the tests based on project documentation
```

---

## Advanced

- [ ] Set up parent directory CLAUDE.md for monorepo structure
- [ ] Create different CLAUDE.md for different branches
- [ ] Experiment with dynamic memory (environment-specific rules)
- [ ] Compare Claude's behavior with and without CLAUDE.md

### Pro Tips

> **"Hold the same bar for Claude's code as you would for a human's"** - Review AI-generated code with the same rigor you'd apply to a colleague's PR. This keeps code quality high and helps you learn what to add to CLAUDE.md.
