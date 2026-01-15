# Chapter 24: Team Collaboration

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Standardizing Claude Code in teams
- Sharing settings and prompts
- Building collaboration workflows

---

## Why do you need this?

**Real-world scenario**: Your team loves Claude Code, but everyone uses it differently. One person uses certain commands, another has different settings. New team members are confused. Code reviews are inconsistent.

Standardizing Claude Code for your team means everyone works the same way, and it's easier to onboard new members.

### Simple Analogy: Team Playbook

Think of team configuration like a sports team playbook:
- Everyone knows the same plays (commands)
- Everyone uses the same terminology (agents)
- New players can learn quickly (onboarding)

---

## Why Learn Team Collaboration?

Using alone vs in a team is different:

- **Consistency**: Different approaches per member → confusion
- **Onboarding**: New members should start immediately
- **Quality**: Improve code quality across the team

---

## Team Standardization

### What to Share

```
my-project/
├── .claude/
│   ├── settings.json    # Team settings
│   ├── commands/        # Team Commands
│   ├── agents/          # Team Agents
│   └── skills/          # Team Skills
├── CLAUDE.md            # Project rules
└── src/
```

Commit these folders to Git and the whole team works in the same environment.

### CLAUDE.md Team Version

```markdown
# Project: My Team App

## Team Info
- Team: Frontend Team
- Contact: #frontend-dev (Slack)

## Tech Stack
- React 18 + TypeScript
- Tailwind CSS
- Jest + React Testing Library
- GitHub Actions

## Coding Conventions
- Components: Functional, PascalCase
- Hooks: use prefix, camelCase
- File names: kebab-case
- Tests: *.test.tsx

## Git Rules
- Commit messages: Conventional Commits
- PR required: No direct push to main
- Reviewers: Minimum 1

## Common Commands
- npm run dev: Dev server
- npm test: Tests
- npm run lint: Lint
- npm run build: Build

## Important Folders
- src/components/: Reusable components
- src/features/: Feature modules
- src/hooks/: Custom hooks
- src/utils/: Utilities
```

---

## Team Commands

### Standard Workflow

```markdown
<!-- .claude/commands/pr.md -->
# PR Creation Workflow

## Pre-checks
- [ ] Tests passing: $(npm test)
- [ ] Lint passing: $(npm run lint)
- [ ] Build successful: $(npm run build)

## PR Title
feat|fix|docs|refactor: Brief description

## PR Body
### Changes
- What was changed

### How to Test
- How to test this

### Screenshots
(For UI changes)
```

```
> /pr
```

### Onboarding Helper

```markdown
<!-- .claude/commands/onboarding.md -->
# New Member Onboarding

Welcome! You're new here.

## 1. Environment Setup
```bash
npm install
cp .env.example .env.local
npm run dev
```

## 2. Project Structure
$(tree -L 2 src/)

## 3. Important Files
- CLAUDE.md: Project rules
- .claude/commands/: Available commands
- src/: Source code

## 4. First Task
- Start with simple issues
- Questions in #frontend-dev channel

Ask if you have questions!
```

```
> /onboarding
```

### Code Review Request

```markdown
<!-- .claude/commands/review-request.md -->
# Review Request

## Changed Files
$(git diff --name-only HEAD~1)

## Changes Summary
$ARGUMENTS

## Review Points
- Is the logic correct
- Are there better approaches
- Are tests sufficient

Reviewer: @team-lead
```

```
> /review-request "Added validation logic to login form"
```

---

## Team Agents

### Senior Reviewer

```markdown
<!-- .claude/agents/senior-reviewer.md -->
# Senior Developer

## Role
You are a 10-year senior developer.
Do thorough code reviews, but give constructive feedback.

## Review Perspective
- Architecture: Is the design appropriate
- Performance: Any unnecessary operations
- Maintainability: Easy for others to understand
- Testing: Sufficiently tested

## Feedback Style
- Present problems with improvement directions
- "This could be better" instead of "This is bad"
- Mention what's done well too
```

```
> @senior-reviewer check this PR
```

### Junior Helper

```markdown
<!-- .claude/agents/junior-helper.md -->
# Junior Helper

## Role
Help junior developers.
Explain difficult concepts simply, guide step by step.

## Principles
- It's okay not to know
- Use many examples
- Explain why
- Encourage questions

## Explanation Style
1. Core concept first
2. Simple example
3. Apply to actual code
4. Suggest practice problems
```

```
> @junior-helper what is React's useEffect?
```

---

## Collaboration Workflow

### Issue → Implement → Review → Merge

```
┌─────────────────────────────────────────────────────────────────┐
│                        Team Workflow                             │
└─────────────────────────────────────────────────────────────────┘

 1. Issue Assigned
      │
      ▼
┌──────────────┐
│ /issue-start │  ← Create branch, analyze issue
└──────┬───────┘
       │
       ▼
 2. Implementation
      │
      ▼
┌──────────────┐
│ /pr          │  ← Create PR, verify tests
└──────┬───────┘
       │
       ▼
 3. Review
      │
      ▼
┌──────────────┐
│@senior-reviewer│  ← Code review
└──────┬───────┘
       │
       ▼
 4. Merge
      │
      ▼
┌──────────────┐
│ Auto Deploy  │  ← CI/CD
└──────────────┘
```

### Issue Start Command

```markdown
<!-- .claude/commands/issue-start.md -->
# Start Issue

Issue number: $ARGUMENTS

## 1. Check Issue
$(gh issue view $ARGUMENTS)

## 2. Create Branch
```bash
git checkout -b feature/$ARGUMENTS
```

## 3. Work Plan
Analyze the issue and create a work plan.

## 4. Start
Begin implementation once plan is confirmed.
```

```
> /issue-start 42
```

---

## Team Onboarding Automation

### New Member Checklist

```markdown
<!-- .claude/commands/new-member.md -->
# New Member Checklist

Welcome! Follow this order.

## Day 1
- [ ] Clone Git repository
- [ ] Set up dev environment (npm install)
- [ ] Verify local run
- [ ] Read CLAUDE.md
- [ ] Join team Slack channels

## Day 2
- [ ] Understand project structure
- [ ] Get assigned simple good-first-issue
- [ ] Submit first PR

## Week 1
- [ ] Read main feature code
- [ ] Attend team meetings
- [ ] Pair programming 1 time

Ask anytime if you have questions!
```

### Environment Setup Automation

```markdown
<!-- .claude/commands/setup.md -->
# Dev Environment Setup

## 1. Install Dependencies
```bash
npm install
```

## 2. Set Environment Variables
```bash
cp .env.example .env.local
```

Set these in .env.local:
- DATABASE_URL: Local DB address
- API_KEY: Dev API key (ask team)

## 3. Database Setup
```bash
npm run db:migrate
npm run db:seed
```

## 4. Verify Run
```bash
npm run dev
```

Check at http://localhost:3000.
```

---

## Team Rules Documentation

### Claude Usage in README

```markdown
# Project README

## Claude Code Usage

### Available Commands
| Command | Description |
|---------|-------------|
| /pr | Create PR |
| /review | Request code review |
| /issue-start <number> | Start issue |
| /onboarding | Onboarding guide |

### Available Agents
| Agent | Description |
|-------|-------------|
| @senior-reviewer | Senior perspective review |
| @junior-helper | Junior helper |

### Team Rules
- All code changes via PR
- Verify tests pass before PR
- Merge after reviewer approval
```

---

## Summary

What you learned in this chapter:
- [x] Team settings standardization (.claude/ folder)
- [x] Team Commands and Agents
- [x] Building collaboration workflows
- [x] Onboarding automation
- [x] Team rules documentation

**Key point**: Good team settings let all members work consistently.

---

## Quick Tips for Success

1. **Start small** - Don't create 20 commands at once. Start with 2-3 essential ones.

2. **Document everything** - If it's not in the README, it doesn't exist for new members.

3. **Iterate** - Your first version won't be perfect. Improve based on feedback.

4. **Lead by example** - Use the team tools yourself consistently.

5. **Celebrate wins** - When team tools save time, share that success!

---

## Congratulations!

You've completed all 24 chapters of the Vibecoding Curriculum.

Now you can use Claude Code to:
- Build and deploy websites
- Create apps and automation tools
- Configure advanced workflows (Hooks, Skills, MCP)
- Collaborate effectively with your team

**Remember:**
- You don't need to be perfect from the start
- Keep improving with feedback
- Claude is always ready to help

**Go build something amazing!**
