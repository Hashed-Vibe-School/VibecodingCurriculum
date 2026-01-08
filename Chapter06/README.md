# Chapter 06: Slash Commands

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, ensure you:
- [ ] Have completed Chapter 00-05 (Foundation)
- [ ] Understand CLAUDE.md and project memory
- [ ] Are comfortable with markdown syntax

---

## Introduction

Slash commands let you create reusable prompts that you or your team can invoke with a simple `/command`. Think of them as macros for common tasks—code review, debugging, documentation generation, and more.

### Why Slash Commands?

- **Consistency**: Same prompt structure every time
- **Speed**: No retyping complex instructions
- **Team Sharing**: Project commands are git-tracked
- **Best Practices**: Encode expert knowledge into commands

---

## Topics

### 1. Built-in Commands

Claude Code comes with many built-in commands:

#### Essential Commands

| Command | Description |
|---------|-------------|
| `/help` | Show all commands |
| `/clear` | Clear conversation |
| `/compact` | Compress context |
| `/model` | Change model |
| `/cost` | Show usage |
| `/config` | Configuration |
| `/doctor` | Diagnose issues |

#### Power User Commands

| Command | Description |
|---------|-------------|
| `/vim` | Enable full vim-style editing (h/j/k/l, ciw, dd, etc.) |
| `/context` | View what's consuming your token window |
| `/stats` | See your Claude Code usage statistics |
| `/rename <name>` | Name your current session |
| `/resume <name>` | Resume a named session |
| `/statusline` | Customize the status bar display |
| `/chrome` | Enable browser interaction for web tasks |

### 2. Creating Custom Commands

Commands are markdown files in the `.claude/commands/` directory.

**Project commands** (shared with team):
```
.claude/commands/
├── review.md
├── fix-issue.md
└── generate-tests.md
```

**Personal commands** (only for you):
```
~/.claude/commands/
├── my-review-style.md
└── daily-standup.md
```

### 3. Command File Structure

A simple command:

```markdown
<!-- .claude/commands/review.md -->
Review this code for:
1. Potential bugs
2. Performance issues
3. Security vulnerabilities
4. Code style violations

Provide specific line references and suggested fixes.
```

Invoke with: `/review`

### 4. Using Arguments

Use `$ARGUMENTS` placeholder for dynamic input:

```markdown
<!-- .claude/commands/fix-issue.md -->
Find and fix GitHub issue #$ARGUMENTS

Steps:
1. Read the issue description
2. Locate the relevant code
3. Implement a fix
4. Write tests if applicable
5. Summarize the changes
```

Invoke with: `/fix-issue 123`

### 5. Multi-Step Commands

Create complex workflows:

```markdown
<!-- .claude/commands/feature.md -->
# Implement Feature: $ARGUMENTS

## Phase 1: Research
- Understand the requirement
- Identify affected files
- Check for similar implementations

## Phase 2: Implementation
- Create necessary files
- Follow existing patterns
- Add proper error handling

## Phase 3: Quality
- Write unit tests
- Update documentation
- Self-review for issues

## Phase 4: Completion
- Summarize all changes
- List any follow-up tasks
```

### 6. Command Best Practices

#### Be Specific
```markdown
<!-- Bad -->
Review the code

<!-- Good -->
Review @src/auth/ for security issues:
- Check for SQL injection
- Verify input sanitization
- Ensure proper authentication checks
- Look for exposed secrets
```

#### Include Context
```markdown
<!-- Include project-specific context -->
Our tech stack uses React + TypeScript.
Follow the patterns in @src/components/Button.tsx.
Use our custom `useApi` hook for data fetching.
```

#### Add Constraints
```markdown
<!-- Set clear boundaries -->
Rules:
- Do not modify existing tests
- Keep changes backward compatible
- Maximum 3 files changed
```

### 7. Organizing Commands

Recommended structure:
```
.claude/commands/
├── code/
│   ├── review.md
│   ├── refactor.md
│   └── debug.md
├── docs/
│   ├── api-docs.md
│   └── readme.md
├── git/
│   ├── commit.md
│   └── pr.md
└── test/
    ├── unit.md
    └── e2e.md
```

---

## Resources

- [Claude Code Commands Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Markdown Guide](https://www.markdownguide.org/)

---

## Checklist

Answer these questions as if in an interview:

1. **What are slash commands and why use them?**
   <details>
   <summary>Hint</summary>
   Reusable prompt templates, consistency, speed, team sharing
   </details>

2. **Where do project vs personal commands live?**
   <details>
   <summary>Hint</summary>
   Project: .claude/commands/ (git tracked). Personal: ~/.claude/commands/
   </details>

3. **How do you pass dynamic arguments to commands?**
   <details>
   <summary>Hint</summary>
   Use $ARGUMENTS placeholder in the command file
   </details>

4. **What makes a good slash command?**
   <details>
   <summary>Hint</summary>
   Specific, includes context, has constraints, well-organized
   </details>

---

## Mini Project: Command Library

### Project Goals

Build a comprehensive slash command library by completing:

- [ ] Create `/review` command for code review with security and style checks
- [ ] Create `/fix-bug $ARGUMENTS` command to fix bugs by issue number
- [ ] Create `/test $ARGUMENTS` command to generate tests for a file
- [ ] Create `/docs $ARGUMENTS` command to generate documentation
- [ ] Create 4+ creative commands (e.g., `/explain`, `/optimize`, `/refactor`)
- [ ] Organize commands in proper folder structure
- [ ] Test each command works correctly

### Ideas to Try

- Create a `/security-audit` command for security-focused reviews
- Build a `/changelog` command to generate changelogs from commits
- Make a `/standup` command that generates daily standup summaries
- Create commands that chain together for complex workflows

---

## Advanced

- [ ] Create commands that call other commands
- [ ] Build commands for specific frameworks (React, Django, etc.)
- [ ] Create an onboarding command set for new team members
- [ ] Experiment with commands that generate other commands
