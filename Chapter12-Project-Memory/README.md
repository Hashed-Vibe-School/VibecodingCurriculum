# Chapter 12: Project Memory

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You'll Learn in This Chapter

- What CLAUDE.md is and why it's powerful
- How to write effective project memory
- How to make Claude "remember" your project
- Advanced techniques for using CLAUDE.md in practice

---

## Connection to Previous Chapter

In Chapter 11, we learned Git. If Git is a tool that **remembers code versions**, CLAUDE.md, which we'll learn now, is a tool that **makes AI remember your project**.

You've probably had this experience when talking with Claude:

```
"Use Tailwind CSS"
(new conversation starts)
"Oh right, use Tailwind CSS"
(another new conversation starts)
"Use Tailwind CSS..."
```

Repeating the same thing is inefficient. CLAUDE.md solves this problem.

---

## What is CLAUDE.md?

CLAUDE.md is a "project manual for Claude." Every time a new conversation starts, Claude automatically reads this file. So you don't have to repeat the same things every time.

### Why Does This Work?

LLMs (Large Language Models) have something called a "context window." They reference information within this window during conversations. When CLAUDE.md is read, its contents are included in the context, creating an effect as if you "just mentioned it."

```
+-------------------------------------+
|         Context Window              |
|  +-------------------------------+  |
|  |  CLAUDE.md Content (auto-load) |  |
|  |  - Project rules              |  |
|  |  - Tech stack                 |  |
|  |  - Coding conventions         |  |
|  +-------------------------------+  |
|  +-------------------------------+  |
|  |  User conversation            |  |
|  |  "Make a button"              |  |
|  +-------------------------------+  |
|           |                         |
|  Claude creates button following    |
|  the rules                          |
+-------------------------------------+
```

> **Beginner Tip**: Think of CLAUDE.md as an "onboarding document for a new team member." It's like a document explaining "this is how our team works" to someone new to the team.

### Why Do You Need It?

**Without CLAUDE.md:**
```
User: Make a button

Claude: (What style should I use?)
I made a button. (basic CSS style)

User: Oh, we need to use Tailwind CSS...
       Make the button again. With Tailwind.

Claude: (starting over) I made the button with Tailwind.

User: And we also need to use TypeScript...
```

**With CLAUDE.md:**
```
User: Make a button

Claude: (reads CLAUDE.md: Tailwind + TypeScript project)
I made a button using TypeScript and Tailwind CSS.
```

No need to say "use Tailwind," "use TypeScript" every time!

### CLAUDE.md in Real Work

In real companies, CLAUDE.md provides this value:

| Situation | Without CLAUDE.md | With CLAUDE.md |
|-----------|-------------------|----------------|
| **New feature development** | Explain tech stack every time | Start work immediately |
| **Bug fixes** | Repeat coding conventions | Fix with consistent style |
| **Team member changes** | Verbally pass on rules (risk of missing things) | Share documented rules |
| **Project handoff** | Long explanation needed | Transfer context with one file |

> **Pro Tip**: A good CLAUDE.md also helps new team members adapt to the project quickly. It's not just a document for AI, but a "living document" for the project.

---

## Creating CLAUDE.md

### Where to Put It?

Create it at the top (root) of your project folder.

```
my-project/
├── CLAUDE.md       <- Here!
├── package.json
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
└── ...
```

> **Caution**: The file name must be exactly `CLAUDE.md`. It's case-sensitive! `claude.md` or `Claude.md` may not be recognized.

### Have Claude Create It

**Basic Request:**
```
> Create a CLAUDE.md for this project
```

Claude will analyze the project and create it. It examines `package.json`, folder structure, existing code style, and more to understand the project's characteristics.

**Actual Conversation Example:**

```
User: Create a CLAUDE.md for this project

Claude: Let me analyze the project.

[Checking package.json]
- React 18
- TypeScript
- Tailwind CSS
- Vite

[Checking folder structure]
- src/components/ - Components
- src/pages/ - Pages
- src/hooks/ - Custom hooks

[Analyzing existing code style]
- Using functional components
- PascalCase component names
- Korean comments

I've created CLAUDE.md:

# My Project

## Tech Stack
- React 18 + TypeScript
- Tailwind CSS
- Vite

## Folder Structure
- `src/components/` - Reusable components
- `src/pages/` - Page components
- `src/hooks/` - Custom hooks

## Coding Rules
- Use only functional components
- Component names: PascalCase
- Comments: Write in Korean
```

**For a Better CLAUDE.md:**

```
> Create CLAUDE.md. Emphasize naming conventions and folder structure.
> Our team only uses functional components and never uses the any type.
```

Tell Claude what to emphasize and it will customize accordingly.

### Create It Yourself

You can also create it yourself without Claude. Simple example:

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

> **Beginner Tip**: The easiest approach is to have Claude create it first, then edit it yourself afterward.

---

## What to Write?

Let's learn the key components of an effective CLAUDE.md.

### 1. Tech Stack (Required)

Tell the project what it uses.

```markdown
## Tech Stack
- Frontend: React 18 + TypeScript
- Styling: Tailwind CSS
- State: Zustand
- API: React Query
- Package manager: pnpm
```

**Why is this important?**
- Claude uses correct syntax and patterns
- Uses version-appropriate APIs
- Prevents incompatible code

### 2. Folder Structure (Required)

Tell it where files are located.

```markdown
## Folder Structure
- `src/components/` - Reusable UI components
- `src/components/ui/` - Basic UI elements (Button, Input, etc.)
- `src/pages/` - Page components
- `src/hooks/` - Custom hooks
- `src/utils/` - Utility functions
- `src/types/` - TypeScript type definitions
- `src/api/` - API-related functions
```

**Why is this important?**
- Claude creates new files in the correct locations
- Easily finds existing files
- Maintains project structure consistency

### 3. Coding Rules (Required)

Tell it the code style.

```markdown
## Coding Rules

### Naming
- Components: PascalCase (e.g., `UserProfile.tsx`)
- Hooks: camelCase + use prefix (e.g., `useAuth.ts`)
- Utils: camelCase (e.g., `formatDate.ts`)
- Constants: SCREAMING_SNAKE_CASE (e.g., `MAX_RETRY_COUNT`)

### Style
- Indentation: 2 spaces
- Strings: single quotes (`'`)
- Semicolons: don't use

### Components
- Use only functional components
- Define props with interfaces
- One component per file
```

### 4. Things Not To Do (Very Important!)

Clearly state what's forbidden.

```markdown
## Forbidden (Never Do)
- Never use `any` type
- No inline styles (use Tailwind)
- No committing `console.log`
- No class components
- No jQuery
- No single component over 200 lines
```

**Why is this important?**
Claude follows explicit instructions like "don't do this" well. "Never use X" is more effective than vaguely saying "avoid X."

> **Pro Tip**: If you also explain the reason for forbidden things, Claude understands better. Example: "Never use `any` type (to maintain type safety)"

### 5. Common Commands

```markdown
## Commands
- Dev server: `pnpm dev`
- Build: `pnpm build`
- Test: `pnpm test`
- Lint: `pnpm lint`
- Type check: `pnpm typecheck`
```

### 6. Environment Settings (Optional)

```markdown
## Environment Variables
- `VITE_API_URL` - Backend API URL
- `VITE_GA_ID` - Google Analytics ID

## Branch Strategy
- `main` - Production
- `develop` - Development
- `feature/*` - Feature development
```

---

## Real Examples: Various Project Types

### Example 1: Simple Website Project

```markdown
# My Portfolio

## Description
Personal portfolio website.

## Tech Stack
- HTML5, CSS3, JavaScript (ES6+)
- Styling: Tailwind CSS (CDN)
- Icons: Font Awesome

## File Structure
- `index.html` - Main page
- `about.html` - About page
- `projects.html` - Project list
- `css/` - Style files
- `js/` - JavaScript files
- `images/` - Image files

## Rules
- File names use lowercase and hyphens (e.g., about-me.html)
- Use Tailwind default colors
- Mobile responsive required (mobile-first)
- Prefer WebP format for images

## Forbidden
- No jQuery
- No inline styles
- Minimize !important usage
```

### Example 2: React + TypeScript Project

```markdown
# Todo App

## Tech Stack
- React 18
- TypeScript 5.x
- Tailwind CSS
- Zustand (state management)
- React Query (server state)
- Vite (build tool)

## Folder Structure
```
src/
├── components/       # Reusable components
│   ├── ui/          # Basic UI (Button, Input, Modal)
│   └── features/    # Feature-specific components
├── pages/           # Page components
├── hooks/           # Custom hooks
├── stores/          # Zustand stores
├── types/           # TypeScript types
├── utils/           # Utility functions
├── api/             # API functions
└── constants/       # Constants
```

## Coding Rules
- Components: PascalCase (e.g., TodoItem.tsx)
- Hooks: camelCase + use (e.g., useTodos.ts)
- Types: PascalCase + suffix (e.g., TodoItemProps, UserResponse)
- Constants: SCREAMING_SNAKE_CASE
- Use only functional components + hooks
- Max 150 lines per component

## Component Writing Pattern
```tsx
// Good example
interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
  variant?: 'primary' | 'secondary'
}

export function Button({ children, onClick, variant = 'primary' }: ButtonProps) {
  return (...)
}
```

## Forbidden
- Never use any type
- No class components
- No inline styles
- No committing console.log
- No hardcoded strings (separate into constants)

## Commands
- `pnpm dev` - Dev server (http://localhost:5173)
- `pnpm build` - Production build
- `pnpm preview` - Build preview
- `pnpm lint` - ESLint check
- `pnpm test` - Run tests
```

### Example 3: Backend API Project

```markdown
# User API Service

## Tech Stack
- Node.js 20.x
- Express.js
- TypeScript
- PostgreSQL
- Prisma ORM
- Jest (testing)

## Folder Structure
```
src/
├── controllers/     # Request handling
├── services/        # Business logic
├── repositories/    # Database access
├── middlewares/     # Middlewares
├── routes/          # Route definitions
├── types/           # TypeScript types
├── utils/           # Utilities
└── prisma/          # Prisma schema
```

## API Rules
- Follow RESTful design principles
- Response format: { success: boolean, data?: T, error?: string }
- Error handling: try-catch + next(error)
- Authentication: JWT Bearer token

## Naming Rules
- Controllers: `UserController.ts`
- Services: `UserService.ts`
- Routes: `/api/v1/users`
- Functions: camelCase (e.g., `getUserById`)

## Forbidden
- No any type
- No direct SQL queries (use Prisma)
- No hardcoded secrets (use environment variables)
- No console.log (use logger)

## Commands
- `pnpm dev` - Dev server (nodemon)
- `pnpm build` - TypeScript compile
- `pnpm start` - Production run
- `pnpm test` - Jest tests
- `pnpm db:migrate` - DB migration
- `pnpm db:seed` - Insert seed data
```

---

## Personal Memory vs Project Memory

There are two types of memory in Claude.

### Project Memory (For Teams)

Location: `projectfolder/CLAUDE.md`

- **Shared by the whole team**
- Saved in Git
- Defines project rules
- Applied equally to all team members

### Personal Memory (Your Settings)

Location: `~/.claude/CLAUDE.md` (home folder)

- **Only you use it**
- Applies to all projects
- Personal preferences

```markdown
# My Personal Settings

## Preferences
- Always explain in Korean
- Add lots of comments to code
- Always show example code
- Explain the plan before making changes

## Frequently Used Frameworks
- Prefer React + TypeScript
- Prefer Tailwind CSS
- Use pnpm
```

### Priority

```
Project CLAUDE.md > Personal CLAUDE.md
```

Project rules take priority. If the project says "Use CSS Modules," the personal setting "Prefer Tailwind" is ignored.

> **Beginner Tip**: Initially, just add "Explain in English" to your personal CLAUDE.md. Write project rules in each project's CLAUDE.md.

---

## What If Claude Breaks the Rules?

Even with CLAUDE.md, Claude may occasionally break rules. When this happens, update CLAUDE.md.

### Pattern 1: When Rules Are Vague

**Problem:**
```markdown
## Rules
- Prefer TypeScript
```

Claude: "JavaScript seemed okay too, so I wrote it in JS"

**Solution:**
```markdown
## Rules
- TypeScript is required. Never use JavaScript.
```

### Pattern 2: When Rules Are Missing

**Situation:** Claude used `var`

**Solution:**
```markdown
## Forbidden
- Never use var, only use const or let
```

### Pattern 3: When Rules Conflict

**Problem:**
```markdown
- Write concise code
- Add detailed comments everywhere
```

**Solution:** Specify priority
```markdown
## Code Style
1. First, write readable code
2. Add comments only for complex logic
3. Self-explanatory code doesn't need comments
```

### Actual Conversation Example: Handling Rule Violations

```
User: Make a button

Claude: (creates button with inline style)

User: We don't use inline styles. Redo it with Tailwind.
       And add "Never use inline styles" to CLAUDE.md.

Claude: Sorry, I missed the CLAUDE.md.
        I'll redo it with Tailwind and update CLAUDE.md.

[File modified]

I won't use inline styles from now on.
```

> **Pro Tip**: If you've given Claude the same feedback more than twice, that's a signal to add it to CLAUDE.md!

---

## Setting Response Language

If Claude keeps responding in English, you can change it with a settings file.

### Method 1: Settings File

`.claude/settings.json` file:

```json
{
  "language": "korean"
}
```

### Method 2: Add to CLAUDE.md

```markdown
## Language
- Always respond in Korean
- Write code comments in Korean too
- Use English only for variable names
```

---

## Advanced: Getting More from CLAUDE.md

### Technique 1: Add Reasons to Rules

"Do this because of that" is more effective than just "do this." Claude can make the right judgment in similar situations on its own.

```markdown
# Without reason
- Use TypeScript strict mode

# With reason (more effective)
- Use TypeScript strict mode (prevents bugs from implicit any types)
- Component 150 line limit (for maintainability and testability)
- One function, one role (for easier debugging and reuse)
```

### Technique 2: Include Example Code

Showing examples is more accurate than just listing rules.

```markdown
## Component Writing Pattern

### Good Example
```tsx
interface ButtonProps {
  children: React.ReactNode
  variant?: 'primary' | 'secondary'
  onClick?: () => void
}

export function Button({ children, variant = 'primary', onClick }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  )
}
```

### Bad Example
```tsx
// any type
function Button(props: any) { ... }

// inline style
<button style={{color: 'red'}}>Click</button>
```
```

### Technique 3: Conciseness is Key

The longer CLAUDE.md gets, the less effective it becomes. Like humans, Claude may miss things when given too many instructions.

```markdown
# Bad example (too long)
Our project started in January 2024
and was originally JavaScript but we switched to TypeScript
because... (100 lines)

# Good example (concise)
## Tech Stack
- TypeScript (strict mode)
- React 18
- Tailwind CSS
```

**Recommended length: 50-100 lines or less**

### Technique 4: Repeated Corrections = Signal to Add to CLAUDE.md

```
1st time: "Write comments in Korean" -> Claude fixes it
2nd time: "English again. Use Korean" -> Time to add to CLAUDE.md!
```

This accumulation means Claude understands your project better over time.

### Technique 5: Managing Long-term Work with External Memory

Conversation context eventually disappears, but files remain. For complex or long tasks, record progress in a file.

```markdown
# progress.md

## Current Status
- [x] Login UI complete
- [x] Login API connection
- [ ] Implementing error handling
- [ ] Password recovery planned

## Decisions Made
- Using JWT instead of sessions
- Token expiry: 7 days
- Refresh token implementation planned

## Next Steps
- Error message UI
- Implement refresh token
- Logout functionality
```

This allows:
- Starting a new conversation and having Claude read the file to continue
- Clear visibility into progress
- Useful when sharing with teammates

---

## Mini Quiz

### Quiz 1
Q: Where should CLAUDE.md be located?

<details>
<summary>View Answer</summary>

It should be at the top (root) of the project folder.
```
my-project/
├── CLAUDE.md  <- Here!
├── src/
└── ...
```
</details>

### Quiz 2
Q: What happens when project CLAUDE.md and personal CLAUDE.md conflict?

<details>
<summary>View Answer</summary>

Project CLAUDE.md takes priority. Project rules are applied more importantly than personal settings.
</details>

### Quiz 3
Q: If CLAUDE.md says "Prefer TypeScript" but Claude used JavaScript?

<details>
<summary>View Answer</summary>

Because the rule is vague. You need to modify it to be more explicit like "Always use TypeScript. Never use JavaScript."
</details>

---

## Practice Exercises

### Level 1: Creating Your First CLAUDE.md (Beginner)

```
# Create new folder
> Create my-project folder and go into it

# Ask Claude to create CLAUDE.md
> Create a CLAUDE.md for this project.
> It's a React + TypeScript project using Tailwind CSS.
```

**Completion Check:**
- [ ] CLAUDE.md file is created
- [ ] Tech stack is specified
- [ ] Folder structure is defined

### Level 2: Testing Rules (Intermediate)

```
# Ask Claude to create something
> Create a button component

# Check if Claude followed the rules
> Did you use TypeScript and Tailwind like the CLAUDE.md says?

# Add rules if there are problems
> Add "Never use inline styles" to CLAUDE.md
```

**Completion Check:**
- [ ] Button component is created following rules
- [ ] TypeScript is used
- [ ] Tailwind is used

### Level 3: Building Comprehensive CLAUDE.md (Advanced)

Create detailed CLAUDE.md like a real project:

```
> Create CLAUDE.md with the following:
> 1. Tech stack (React, TypeScript, Tailwind, Zustand)
> 2. Detailed folder structure
> 3. Naming rules (components, hooks, utils)
> 4. Coding conventions (with good/bad examples)
> 5. Forbidden items (with reasons)
> 6. Command list
```

**Completion Check:**
- [ ] All sections are included
- [ ] Example code is present
- [ ] Forbidden items have reasons

---

## Challenge Exercises

### Challenge 1: Rule Violation Detection
```
> Intentionally write code that breaks the CLAUDE.md rules.
> Then explain why it's a rule violation.
```

### Challenge 2: Team CLAUDE.md Simulation
Create CLAUDE.md for a virtual 3-person team project:
- Frontend developer
- Backend developer
- Designer

### Challenge 3: Auto Update
```
> Summarize the feedback I gave today
> and suggest any rules that should be added to CLAUDE.md.
```

---

## When Problems Occur

**Claude doesn't seem to read CLAUDE.md?**
- Check the file name: Must be exactly `CLAUDE.md` (case-sensitive)
- Check the location: Must be in the project root folder
- Try: "Read CLAUDE.md and tell me what's in it"

**Claude ignores certain rules?**
- Make the rule more explicit and direct
- Bad: "Prefer TypeScript"
- Good: "Always use TypeScript. Never use JavaScript."

**CLAUDE.md is getting too long?**
- Keep it under 50-100 lines if possible
- Remove outdated rules
- Focus on rules that apply now, not project history

**Rules conflict with each other?**
- Prioritize them: Put important rules first
- Be specific: "For components use X, for utils use Y"

**Personal and project rules clash?**
- Project rules (local CLAUDE.md) override personal rules
- If needed, adjust your personal settings for that project

---

## Common Mistakes

### Mistake 1: Writing a Novel Instead of Rules

```markdown
# Bad example - too much backstory
Our team started this project in 2023 because we decided
that we needed a better way to manage our inventory.
After many discussions, we chose React because...
(continues for 200 lines)

# Good example - just the rules
## Tech Stack
- React 18, TypeScript, Tailwind CSS

## Rules
- Functional components only
- No inline styles
```

### Mistake 2: Being Too Vague

```markdown
# Bad
- Write good code
- Use best practices
- Keep it clean

# Good
- Use TypeScript strict mode
- No any types allowed
- Components under 200 lines
- One function, one role
```

### Mistake 3: Not Updating When Rules Change

```
# Problem: Team decided to switch from CSS Modules to Tailwind
# But CLAUDE.md still says "Use CSS Modules"

# Solution
> Update CLAUDE.md: We now use Tailwind CSS instead of CSS Modules
```

### Mistake 4: Not Explaining Why

```markdown
# Bad - rule without reason
- Keep components under 200 lines

# Good - rule with reason (Claude understands better)
- Keep components under 200 lines (for maintainability and easier testing)
```

### Mistake 5: Not Testing the Rules

```
# Bad - adding rules without checking
> Add to CLAUDE.md: "Use camelCase for filenames"
[Never actually check if Claude follows it]

# Good - test after adding
> Add to CLAUDE.md: "Use camelCase for filenames"
> Create a utility file
> (Check: Is the filename in camelCase?)
```

---

## Glossary

| Term | Description |
|------|-------------|
| **CLAUDE.md** | Markdown file that delivers project information to Claude |
| **Context window** | The amount of information an LLM can reference at once |
| **Project memory** | Per-project CLAUDE.md (team shared) |
| **Personal memory** | CLAUDE.md in home folder (personal settings) |
| **Coding conventions** | Code writing rules and style |
| **Naming rules** | Rules for naming variables, functions, files, etc. |

---

## Summary

What we learned in this chapter:
- [x] What CLAUDE.md is and why it's needed
- [x] Where to create it and what to write
- [x] Difference between personal vs project memory
- [x] How to handle rule violations
- [x] Effective CLAUDE.md writing techniques

CLAUDE.md leverages the LLM's context mechanism. When well written, Claude starts each task already understanding your project context without you having to repeat the same explanations.

> **Key Point**: CLAUDE.md is not a write-once document. It's a **living document** that you refine as the project progresses. Add new rules when they emerge, delete rules that no longer apply.

Congratulations! You've completed Part 2 (Core Features).

---

## Next Chapter Preview

In Chapter 13, we'll practice **Website Development**.

We'll use everything we've learned so far to build an actual portfolio website from start to finish:
- HTML/CSS/JavaScript basics
- Step-by-step development with Claude
- Design feedback and iteration
- Responsive web implementation

Theory is over. Now let's actually build something!

Continue to [Chapter 13: Website Development](../Chapter13-Website-Development/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Memory Guide](https://docs.anthropic.com/en/docs/claude-code/memory) - Official CLAUDE.md documentation
- [Anthropic Prompt Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering) - Effective instruction writing

**Video Resources:**
- [Claude Code Configuration (YouTube)](https://www.youtube.com/results?search_query=claude+code+configuration+tutorial) - Claude Code setup guide
- [Project Documentation (YouTube)](https://www.youtube.com/results?search_query=project+documentation+best+practices) - Documentation best practices

**Reading:**
- [Markdown Guide](https://www.markdownguide.org/) - How to write Markdown
- [Make a README](https://www.makeareadme.com/) - README writing guide

**Related Tools:**
- [Markdown Editor](https://stackedit.io/) - Online Markdown editor
- [Claude Code GitHub](https://github.com/anthropics/claude-code) - Official repository

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
