# Chapter 04: Git Workflow

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, make sure you can:
- [ ] Use basic Git commands (add, commit, push, pull)
- [ ] Understand branches and merging
- [ ] Make code edits with Claude Code

---

## Introduction

Git is the backbone of modern development workflows. Claude Code integrates deeply with Git, helping you write better commit messages, create pull requests, and even review code changes. This chapter teaches you to leverage Claude for a streamlined Git workflow.

### Why Claude + Git?

- **Better Commits**: Claude analyzes changes and writes meaningful messages
- **PR Creation**: Automated, well-structured pull requests
- **Code Review**: AI-assisted review of changes
- **Conflict Resolution**: Help understanding and resolving merge conflicts

---

## Topics

### 1. Understanding Claude's Git Integration

Claude Code can:
- Read git status, diff, and log
- Stage and commit changes
- Create and manage branches
- Generate pull requests (via `gh` CLI)
- Never pushes without explicit permission

### 2. Commit Workflow

#### Let Claude Write Commit Messages

```bash
> Commit these changes with an appropriate message
```

Claude will:
1. Run `git status` to see changes
2. Run `git diff` to understand what changed
3. Check `git log` for commit style
4. Write a commit message following conventions

#### Commit Message Format

Claude follows this format by default:
```
type: concise description

Longer explanation if needed.

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Types**: feat, fix, docs, style, refactor, test, chore

### 3. Pull Request Creation

Ask Claude to create a PR:

```bash
> Create a pull request for these changes
```

Claude will:
1. Check if branch is pushed
2. Analyze all commits on the branch
3. Generate PR title and description
4. Use `gh pr create` to submit

**PR Format**:
```markdown
## Summary
- Bullet points of changes

## Test plan
- [ ] How to verify the changes

🤖 Generated with Claude Code
```

### 4. Code Review with Claude

#### Reviewing Your Own Changes
```bash
> Review my changes before I commit
> Are there any issues with the code I modified?
```

#### Reviewing a PR
```bash
> Review PR #123 and summarize the changes
> Check PR #123 for potential bugs or issues
```

Claude can:
- Summarize what changed
- Identify potential bugs
- Suggest improvements
- Check for security issues

### 5. Branch Management

```bash
> Create a new branch for the login feature
> What branches exist and what are they for?
> Merge the feature branch into main
```

### 6. Handling Merge Conflicts

```bash
> I have merge conflicts in @file.ts. Help me resolve them.
> Explain what each side of this conflict represents
```

Claude can:
- Explain what each version does
- Suggest the best resolution
- Apply the fix

### 7. Git Safety Rules

Claude Code follows strict safety rules:

| Safe | Unsafe (requires explicit permission) |
|------|--------------------------------------|
| `git status` | `git push --force` |
| `git diff` | `git reset --hard` |
| `git log` | `git push` (any) |
| `git add` | `git rebase -i` |
| `git commit` | Modifying git config |

---

## Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [Git Best Practices](https://git-scm.com/book/en/v2)

---

## Checklist

Answer these questions as if in an interview:

1. **How does Claude generate commit messages?**
   <details>
   <summary>Hint</summary>
   Analyzes git status/diff, checks log for style, follows conventional commits
   </details>

2. **What information should a good PR include?**
   <details>
   <summary>Hint</summary>
   Summary of changes, motivation/context, test plan, related issues
   </details>

3. **What git operations does Claude refuse to do automatically?**
   <details>
   <summary>Hint</summary>
   Force push, hard reset, push to remote, interactive rebase
   </details>

4. **How can Claude help with code review?**
   <details>
   <summary>Hint</summary>
   Summarize changes, identify bugs, suggest improvements, check security
   </details>

5. **How would you use Claude to resolve merge conflicts?**
   <details>
   <summary>Hint</summary>
   Ask to explain each side, suggest resolution, apply the fix
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Ask Claude to commit changes and verify the commit message follows conventions
- [ ] Create a feature branch and make multiple commits with Claude's help
- [ ] Ask Claude to create a pull request with proper summary and test plan
- [ ] Use Claude to review changes before committing
- [ ] Resolve a merge conflict with Claude's assistance

### Try These Prompts

```bash
> Commit these changes with an appropriate message
> Create a new branch for the login feature
> Create a pull request for these changes
> Review my changes before I commit
> I have merge conflicts in @file.ts. Help me resolve them
```

---

## Advanced

- [ ] Set up commit message templates that Claude follows
- [ ] Explore `gh` CLI integration for issue management
- [ ] Learn about git hooks and how they interact with Claude
- [ ] Practice reviewing complex PRs with multiple files
