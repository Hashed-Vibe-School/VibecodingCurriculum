# Chapter 10: Project Memory

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What CLAUDE.md is
- Saving project information
- Making Claude remember your project

---

## What is CLAUDE.md?

CLAUDE.md is a "note for Claude."

Claude reads this file at the start of every new conversation. This eliminates the need to repeat the same things every time.

### Why Do You Need It?

Without CLAUDE.md:
```
> Make a button
# Claude makes it in any random style

> I need to use Tailwind CSS...
> Make the button again. With Tailwind.
```

With CLAUDE.md:
```
> Make a button
# Claude reads CLAUDE.md and automatically uses Tailwind
```

No need to say "use Tailwind," "use TypeScript" every time.

---

## Creating CLAUDE.md

### Where to Put It?

Create it at the root of your project folder.

```
my-project/
├── CLAUDE.md       <- Here
├── package.json
├── src/
└── ...
```

### Have Claude Create It

```
> Create a CLAUDE.md for this project
```

Claude analyzes the project and creates it automatically.

### Create It Yourself

Simple example:

```markdown
# My Project

## Tech Stack
- React
- TypeScript
- Tailwind CSS

## Rules
- Use functional components
- Write comments in Korean
```

---

## What to Write?

### 1. Tech Stack

Specify what the project uses.

```markdown
## Tech Stack
- Frontend: React + TypeScript
- Styling: Tailwind CSS
- Package manager: npm
```

### 2. Folder Structure

Specify where files are located.

```markdown
## Folder Structure
- `src/components/` - Components
- `src/pages/` - Pages
- `src/utils/` - Utility functions
```

### 3. Coding Rules

Specify the code style.

```markdown
## Coding Rules
- Variable names in English
- Comments in Korean
- 2-space indentation
```

### 4. Restrictions

Specify what is forbidden.

```markdown
## Do Not
- No `any` type
- No inline styles
- Do not leave console.log
```

### 5. Common Commands

```markdown
## Commands
- Dev server: `npm run dev`
- Build: `npm run build`
- Test: `npm test`
```

---

## Real Examples

### Simple Website Project

```markdown
# My Portfolio

## Description
Personal portfolio website

## Tech Stack
- HTML, CSS, JavaScript
- Styling: Tailwind CSS

## Rules
- File names in lowercase (e.g., about.html)
- Use Tailwind default colors
- Mobile responsive required
```

### React Project

```markdown
# Todo App

## Tech Stack
- React 18
- TypeScript
- Tailwind CSS

## Folder Structure
- `src/components/` - Reusable components
- `src/hooks/` - Custom hooks
- `src/types/` - TypeScript types

## Coding Rules
- Components: PascalCase (e.g., TodoItem.tsx)
- Hooks: camelCase (e.g., useTodos.ts)
- Functional components + hooks

## Do Not
- No class components
- No any type
- Keep components under 200 lines

## Commands
- `npm run dev` - Dev server
- `npm run build` - Build
- `npm test` - Test
```

---

## Personal Memory vs Project Memory

### Project Memory (For Teams)

Location: `projectfolder/CLAUDE.md`

- Shared by the whole team
- Saved in Git
- Project rules

### Personal Memory (Your Settings)

Location: `~/.claude/CLAUDE.md` (home folder)

- Only you use it
- Applies to all projects
- Personal preferences

```markdown
# My Settings

## Preferences
- Explain in Korean
- Add lots of comments to code
- Always show example code
```

### Priority

```
Project CLAUDE.md > Personal CLAUDE.md
```

Project rules take priority.

---

## When Claude Breaks Rules

Update CLAUDE.md.

### Example Situations

If Claude used `var`:

```markdown
## Do Not
- No var, use const or let
```

If Claude wrote comments in English:

```markdown
## Rules
- Comments must be in Korean
```

Add these and it will not happen again.

---

## Response Language Setting

If Claude keeps responding in English, change it with a settings file.

`.claude/settings.json` file:

```json
{
  "language": "korean"
}
```

Or in CLAUDE.md:

```markdown
## Language
- Always respond in Korean
```

---

## Practice

### Practice 1: Create CLAUDE.md

```
# Create new folder
> Create my-test-project folder and go into it

# Create CLAUDE.md
> Create a CLAUDE.md for this project.
> It's a React + TypeScript project.
```

### Practice 2: Check Rules

```
# Check if Claude knows the rules
> What are the rules for this project?

# Check if it follows the rules
> Create a button component
```

### Practice 3: Add Rules

```
> Add "Test files must end in .test.ts" to CLAUDE.md
```

---

## Tips: Managing CLAUDE.md

### Keep It Short

Claude reads it every time, so too long is wasteful.

```markdown
# Bad (too long)
Our project started in January 2024
and was originally JavaScript but we switched to TypeScript
because... (100 lines)

# Good (concise)
## Tech Stack
- TypeScript
- React
```

### Update Regularly

- Add new rules when they emerge
- Remove unused rules
- Add immediately when Claude makes mistakes

### Save in Git

Version control CLAUDE.md like code.

```
> Commit the CLAUDE.md changes
```

---

## Summary

What you learned in this chapter:
- [x] What CLAUDE.md is
- [x] Where to create it
- [x] What to write
- [x] Personal vs Project memory
- [x] Updating rules

When CLAUDE.md is well written, Claude understands your project exactly.

Congratulations. You have completed Part 2 (Core Features).

In the next Part, we will build real projects.

Move on to [Chapter 11: Website Development](../Chapter11/README.md).
