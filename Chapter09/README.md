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

**Think of Git like a time machine for your code:**
- Without Git: You accidentally delete important code. Gone forever.
- With Git: You go back in time and recover it in seconds.

**Or like multiple save files in a video game:**
- Each commit = a save point you can return to
- Each branch = a different storyline you can explore
- If you mess up = just load your last save

### Basic Concepts

| Term | Meaning |
|------|---------|
| **Repository** | A project folder managed by Git |
| **Commit** | Saving changes (like a save point) |
| **Branch** | An independent workspace |
| **Push** | Upload from your computer to server |
| **Pull** | Download from server to your computer |

---

## What is GitHub?

**GitHub** is a cloud service for storing and sharing Git repositories.

### Git vs GitHub

| Git | GitHub |
|-----|--------|
| Tool on your computer | Website (github.com) |
| Manages version history | Stores code online |
| Works offline | Enables collaboration |
| Free software | Free + paid plans |

**Analogy**: Git is like a camera (takes photos), GitHub is like a photo album website (stores and shares photos).

### What Can You Do on GitHub?

- **Backup**: Store your code in the cloud
- **Collaborate**: Work with others on the same project
- **Open Source**: Share code publicly
- **Pull Requests**: Request code reviews before merging
- **Issues**: Track bugs and feature requests

### Connecting to GitHub

```
> Connect this project to GitHub
```

Claude will:
1. Check if you're logged in to GitHub
2. Create a repository if needed
3. Push your code

---

## Check Git Installation

```
> Is git installed?
```

Claude runs a terminal command to check. If Git isn't installed, it will guide you through OS-specific installation steps.

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

**Tips for better commit messages:**

```
> Commit the changes. Use Conventional Commits format.
> This change is a bug fix.
```

Provide context for more accurate messages. Hints like "new feature", "bug fix", or "refactoring" help.

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

**Tips for conflict resolution:**

```
> Resolve the conflict. Keep my branch's login logic.
> I need both versions. Merge them together.
```

Tell Claude which version you prefer, or if you need both—it will resolve accordingly.

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

## Try It Yourself

Let's practice Git with Claude right now:

### Exercise 1: Your First Repository

```
# Create a new folder and initialize Git
> Create a folder called git-practice and initialize it as a git repository

# Create a simple file
> Create index.html with a basic "Hello World" page

# Make your first commit
> Commit this as "Initial commit"

# Check the status
> Show me the git status
```

### Exercise 2: Make Changes and Commit

```
# Modify the file
> Change the title to "My First Git Project"

# See what changed
> Show me what changed (git diff)

# Commit the change
> Commit this change

# View history
> Show me the commit history
```

### Exercise 3: Try Branches

```
# Create a new branch
> Create a branch called feature/new-header

# Make a change in the branch
> Add a header with my name

# Commit it
> Commit this

# Go back to main
> Switch to the main branch

# Notice the header is not there (it's in the other branch)
> Is the header there?
```

---

## If It Doesn't Work?

**"not a git repository" error?**
- You're not in a Git folder. Initialize first:
- Ask Claude: "Initialize git in this folder"

**"nothing to commit" message?**
- You haven't changed any files since the last commit
- Make a change first, then commit

**"conflict" when merging?**
- Two branches changed the same line
- Ask Claude: "Help me resolve this merge conflict"

**Accidentally committed something wrong?**
- Don't panic! Ask: "Undo the last commit but keep my changes"
- Or: "Show me the last 5 commits" to find where to go back

**Pushed something you shouldn't have?**
- Be careful: This is harder to undo
- For sensitive files, contact your team lead
- For code mistakes: Create a new commit that fixes it

**Git commands confusing?**
- You don't need to memorize them!
- Just tell Claude what you want: "I want to see my changes" or "Save my work"

---

## Common Mistakes

### Mistake 1: Forgetting to Commit Regularly

```
# Bad - One huge commit after days of work
> Commit all changes as "did stuff"

# Good - Small, frequent commits
> Commit the login form
> Commit the validation
> Commit the error handling
```

### Mistake 2: Vague Commit Messages

```
# Bad
> Commit as "fixed things"

# Good
> Commit this. It fixes the email validation bug.
```

### Mistake 3: Working on Main Branch for Everything

```
# Bad - All changes directly on main
> [make changes on main]
> Commit

# Good - Use branches for features
> Create a branch for the new feature
> [make changes]
> Commit
> Create a PR to merge into main
```

### Mistake 4: Not Checking Status Before Committing

```
# Bad - Blindly commit everything
> Commit all changes

# Good - Check what you're committing
> What files have changed?
> [review the list]
> Commit only the login-related files
```

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

You do not need to memorize Git commands. What matters is **understanding the concepts**. If you know concepts like "commits are save points" and "branches are workspaces," you can communicate your intent to Claude, and Claude will execute the appropriate commands.

Move on to [Chapter 10: Project Memory](../Chapter10/README.md).
