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

**Why does this work?**

LLMs have something called a "context window." They reference information within this window during conversations. When CLAUDE.md is read, its contents are included in the context, creating an effect as if you "just mentioned it."

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

Claude analyzes the project and creates it. It examines `package.json`, folder structure, existing code style, and more to understand the project's characteristics.

**For a better CLAUDE.md:**

```
> Create CLAUDE.md. Emphasize naming conventions and folder structure.
```

Tell Claude what to emphasize and it will customize accordingly.

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

Add these and from the next conversation, CLAUDE.md will be included in the context, and Claude will follow those rules.

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

## Advanced: Getting More from CLAUDE.md

### Add Reasons to Rules

"Do this because of that" is more effective than just "do this." Claude can make the right judgment in similar situations on its own.

```markdown
# Without reason
- Use TypeScript strict mode

# With reason
- Use TypeScript strict mode (prevents bugs from implicit any types)
```

### Conciseness is Key

The longer CLAUDE.md gets, the less effective it becomes. Like humans, Claude may miss things when given too many instructions.

Focus on **rules that apply right now** rather than project history or background. "Our team in 2023..." is not needed.

### Repeated Corrections = Signal to Add to CLAUDE.md

If you have given Claude the same feedback more than twice, that is a signal to add it to CLAUDE.md.

```
1st time: "Write comments in English" → Claude fixes it
2nd time: "English again. Use Korean" → Time to add to CLAUDE.md!
```

This accumulation means Claude understands your project better over time.

### Managing Long-term Work with External Memory

Conversation context eventually disappears, but files remain. For complex or long tasks, record progress in a file.

```markdown
# progress.md

## Current Status
- [x] Login UI complete
- [ ] API integration in progress
- [ ] Error handling pending

## Decisions Made
- Using JWT instead of sessions
- Token expiry: 7 days

## Next Steps
- Implement refresh token
```

This allows:
- Starting a new conversation and having Claude read the file to continue
- Clear visibility into progress
- Useful when sharing with teammates

### The Power of Continuous Improvement

CLAUDE.md is not a write-once document. It is a **living document** that you refine as the project progresses.

Add new rules as they emerge, delete rules that no longer apply. This small investment compounds, and the quality of collaboration with Claude noticeably improves.

---

## Summary

What you learned in this chapter:
- [x] What CLAUDE.md is
- [x] Where to create it
- [x] What to write
- [x] Personal vs Project memory
- [x] Updating rules

CLAUDE.md leverages the LLM's context mechanism. When well written, Claude starts each task already understanding your project context without you having to repeat the same explanations.

Congratulations. You have completed Part 2 (Core Features).

In the next Part, we will build real projects.

Move on to [Chapter 11: Website Development](../Chapter11/README.md).
