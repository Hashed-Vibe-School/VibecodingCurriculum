# Chapter 09: Git Basics

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What Git is
- Committing with Claude
- Creating branches and PRs

---

## What is Git?

Git is a tool for managing versions of code.

### Why Do You Need It?

- **Undo**: If something goes wrong, go back to a previous version
- **Collaboration**: Multiple people can work on the same code simultaneously
- **History**: See who changed what and when

### Basic Concepts

| Term | Meaning |
|------|---------|
| **Repository** | A project folder managed by Git |
| **Commit** | Saving changes (like a save point) |
| **Branch** | An independent workspace |
| **Push** | Upload from your computer to server |
| **Pull** | Download from server to your computer |

---

## Check Git Installation

```
> Is git installed?
```

Claude checks. If not, it provides installation instructions.

---

## Committing

A commit is "saving the current state." Like a save point in a game.

### Let Claude Commit

```
> Commit the changes made so far
```

Claude will:
1. Check what changed (`git status`)
2. Analyze the changes (`git diff`)
3. Write an appropriate commit message
4. Execute the commit

### Commit Message Format

Claude writes commit messages like this:

```
feat: Add login feature

- Created email/password input form
- Connected login API
```

**Type meanings**:
| Type | Meaning |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation change |
| `style` | Code style (no behavior change) |
| `refactor` | Refactoring |

---

## Checking Status

### View Current Status

```
> Check git status
```

Claude runs `git status` and shows:
- Modified files
- Newly added files
- Uncommitted changes

### View Changes

```
> Show me what changed
```

Claude uses `git diff` to show the changes.

---

## Using Branches

Branches are "workspaces." Work separately without affecting the main code.

### Create a Branch

```
> Create a branch for the login feature
```

Claude runs something like `git checkout -b feature/login`.

### Check Branches

```
> What branch am I on?
```

```
> Show me the branch list
```

### Switch Branches

```
> Move to the main branch
```

---

## Creating Pull Requests (PR)

A PR is "requesting to merge my changes into main."

### Let Claude Create a PR

```
> Create a PR for this branch
```

Claude will:
1. Analyze the changes
2. Write PR title and description
3. Create PR on GitHub

### PR Format

```markdown
## Summary
- Implemented login form
- Added password validation

## Test Plan
- [x] Login success test
- [x] Wrong password test
```

---

## Resolving Conflicts

When multiple people modify the same file, "conflicts" can occur.

### When Conflicts Happen

```
> There's a merge conflict. Help me resolve it.
```

Claude will:
1. Explain what each version is
2. Suggest how to merge
3. Apply the fix

---

## Git Safety Rules

Claude asks before dangerous git commands.

### Safe Commands (Can Auto-execute)

- `git status` - Check status
- `git diff` - View changes
- `git log` - View history
- `git add` - Stage files
- `git commit` - Commit

### Dangerous Commands (Need Confirmation)

- `git push` - Upload to server
- `git push --force` - Force push (very dangerous)
- `git reset --hard` - Delete changes (unrecoverable)

---

## Practice

### Practice 1: Basic Workflow

```
# 1. Create new folder and initialize Git
> Create my-project folder and initialize git

# 2. Create file
> Create index.html

# 3. Commit
> Commit it

# 4. Modify file
> Change the title

# 5. Commit again
> Commit the changes
```

### Practice 2: Using Branches

```
# 1. Create new branch
> Create feature/button branch

# 2. Modify file
> Add a button

# 3. Commit
> Commit it

# 4. Go back to main
> Move to main branch

# 5. Check
> Is the button there? (Should be no - it's in a different branch)
```

### Practice 3: View History

```
> Show me the commit history
```

```
> What changed in the last commit?
```

---

## Common Mistakes

### "nothing to commit"

Nothing to commit. Check if you modified a file.

### "not a git repository"

Not a Git repository. Initialize with `git init`.

### Accidentally Committed

```
> Undo the last commit (keep the changes)
```

---

## Summary

What you learned in this chapter:
- [x] What Git is
- [x] Committing
- [x] Checking status
- [x] Using branches
- [x] Creating PRs

Git may seem difficult at first, but Claude handles it for you.

Move on to [Chapter 10: Project Memory](../Chapter10/README.md).
