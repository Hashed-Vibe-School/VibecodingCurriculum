# Chapter 11: Git Basics

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You'll Learn in This Chapter

- What Git is and why it's essential
- Committing and version control with Claude
- Safe collaboration with branches and PRs
- Real-world tips for resolving conflicts

---

## Connection to Previous Chapter

In Chapter 10, we learned prompt writing and how to communicate effectively with Claude. Now, before starting real projects, we need to learn **how to safely manage code**.

Because no matter how excellent your code is, if you accidentally delete it or modify it incorrectly, all your efforts go to waste. Git is the **safety net for your code** that prevents such tragedies.

---

## What is Git?

Git is a tool for managing versions of your code. But this single sentence doesn't capture Git's true value.

### Git in Real Life

Let's imagine what would have happened without Git:

**Scenario 1: Working Alone**
```
my_project_v1.zip
my_project_v2.zip
my_project_v2_final.zip
my_project_v2_final_realfinal.zip
my_project_v2_final_realfinal_edit.zip
```

Have you seen folder names like this? These are traces of trying to manage versions without Git. Nobody knows which one is actually the latest or what was changed when.

**Scenario 2: Working as a Team**
```
"I sent you the file I modified!"
"What? I also modified that file... it got overwritten!"
"Who deleted my code?!"
```

This is the nightmare of collaboration. Git solves all these problems.

### Why Do You Need It?

| Situation | Without Git | With Git |
|-----------|-------------|----------|
| **Undoing** | Important code accidentally deleted -> unrecoverable | Restore previous version with one command |
| **Collaboration** | "I'm editing this file, don't touch it" | Work on the same file simultaneously, then auto-merge |
| **History** | "Who changed this and why?" | Complete record of who changed what, when, and why |
| **Experimentation** | If trying a new feature fails, start over | Safely experiment in a branch, then merge |

> **Beginner Tip**: Think of Git as a "time machine for code." You can go back to the past and even create parallel universes (branches)!

### Mental Model of Git

**Think of it like save files in a video game:**

```
Video Game               Git
-----------------------------------------
Save Point         =     Commit
Different Storyline =    Branch
Load Save          =     Checkout
Recovery after Game Over = Reset
```

- Each commit = A save point you can return to
- Each branch = A different storyline to explore
- When you mess up = Load the last save

### Basic Concepts

| Term | Meaning | Simple Analogy |
|------|---------|----------------|
| **Repository (Repo)** | Project folder managed by Git | The entire game |
| **Commit** | Saving your changes | Saving the game |
| **Branch** | Independent workspace | Different ending routes |
| **Push** | Upload from my computer to server | Cloud save |
| **Pull** | Download from server to my computer | Load save |
| **Merge** | Combining branches | Integrating storylines |

> **Caution**: "Installing" and "using" Git are different. Even if Git is installed on your computer, you need to initialize each project folder with `git init` to use it.

---

## What is GitHub?

**GitHub** is a cloud service for storing and sharing Git repositories online.

### Git vs GitHub

| Git | GitHub |
|-----|--------|
| Tool on my computer | Website (github.com) |
| Manages version history | Stores code online |
| Works offline | Enables collaboration |
| Free software | Free + paid plans |
| Command-based | Provides GUI |

**Analogy**: Git is like a camera (taking photos), GitHub is like a photo-sharing site (storing and sharing).

> **Beginner Tip**: Think of Git as Microsoft Word's "Track Changes" feature, and GitHub as a cloud storage like Google Drive. You need both for complete collaboration.

### What You Can Do on GitHub

- **Backup**: Store code in the cloud (safe even if your computer breaks)
- **Collaboration**: Work on the same project with others
- **Open Source**: Share code publicly
- **Pull Request**: Request review before merging code
- **Issues**: Track bugs and feature requests
- **Actions**: Automated testing and deployment

### Connecting to GitHub

```
> Connect this project to GitHub
```

Claude will:
1. Check if you're logged into GitHub
2. Create a repository if needed
3. Push the code

**More Specific Request:**

```
> Upload this project to GitHub.
> - Repository name: my-first-project
> - Make it Public
> - Include README.md too
```

> **Pro Tip**: When creating a GitHub repository, understand the difference between Private and Public. Public means anyone can see it, Private means only invited people can see it. Generally, use Public for portfolios and Private for company projects.

---

## Checking Git Installation

```
> Is git installed?
```

Claude will run a terminal command to check. If it's not installed, it will guide you through installation for your operating system.

### Installation Check Conversation Example

**Success Case:**
```
User: Is git installed?

Claude: Let me check the Git version.
[Running: git --version]

Output: git version 2.42.0

Yes, Git version 2.42.0 is installed.
You're ready to use it.
```

**Failure Case (Not Installed):**
```
User: Is git installed?

Claude: Let me check the Git version.
[Running: git --version]

Output: 'git' is not recognized as an internal or external command,
operable program or batch file.

Git is not installed.
Let me show you how to install it on Windows:

1. Go to https://git-scm.com/download/win
2. Click "Click here to download"
3. Run the installer (keep default settings, click Next)
4. Restart the terminal after installation

Would you like to install it?
```

> **Beginner Tip**: Most options during installation can be left as default. However, for "Adjusting your PATH environment," select "Git from the command line and also from 3rd-party software."

---

## Making Commits

A commit is "saving the current state." It's like a save point in a game.

### The Importance of Commits

Committing frequently means:
- You can return to the most recent working state when problems occur
- You can clearly track change history
- Team members can see what work was done

Not committing means:
- Start over if you make a mistake
- "What did I change yesterday?" relies on memory
- Confusion during collaboration

> **Caution**: A commit is a "local save." You need to Push to upload to GitHub. If you only commit without pushing, it's only saved on your computer!

### Having Claude Make Commits

```
> Commit what I've changed so far
```

Claude will:
1. Check what changed (`git status`)
2. Analyze the changes (`git diff`)
3. Write an appropriate commit message
4. Execute the commit

**Actual Conversation Example:**

```
User: Commit what I've changed so far

Claude: Let me check the changes first.

[Running: git status]

Changed files:
- src/App.jsx (modified)
- src/components/Button.jsx (new file)
- src/styles/button.css (new file)

It looks like you added a button component.
I'll commit with the following:

[Running: git add .]
[Running: git commit -m "feat: Add button component

- Create Button.jsx component
- Add button styles CSS
- Use button in App.jsx"]

Commit complete!
Commit hash: a1b2c3d
```

**If you want better commit messages:**

```
> Commit the changes. Use Conventional Commits format.
> This change is a bug fix.
```

Providing context leads to more accurate messages. Give hints like "new feature," "bug fix," or "refactoring."

### Commit Message Format

Claude writes commit messages in this format:

```
feat: Add login functionality

- Create email/password input form
- Connect to login API
```

**Types:**

| Type | Meaning | Example |
|------|---------|---------|
| `feat` | New feature | Add registration form |
| `fix` | Bug fix | Fix login not working bug |
| `docs` | Documentation change | Update README |
| `style` | Code style (no behavior change) | Fix indentation |
| `refactor` | Refactoring | Split functions |
| `test` | Add/modify tests | Add login tests |
| `chore` | Other (build, config, etc.) | Update packages |

> **Pro Tip**: Good commit messages explain "why" rather than "what." "Increase button color contrast for accessibility" is better than "Change button color."

### Commit Frequency

**When should you commit?**

```
Good commit timing:
- When a feature is complete
- When you fix a bug
- When you make a meaningful change
- Before leaving work/taking a break

Bad commit timing:
- A week's work all at once
- When code is broken
- With a message like "not done yet"
```

> **Beginner Tip**: "Work in progress" commits are fine. Saving frequently is safer even if it's not perfect. You can clean it up later.

---

## Checking Status

### Viewing Current Status

```
> Check git status
```

Claude will run `git status` and show you:
- Modified files
- Newly added files
- Deleted files
- Uncommitted changes

**Actual Conversation Example:**

```
User: Check git status

Claude: [Running: git status]

Current branch: main

Modified files (not staged):
  modified:   src/App.jsx
  modified:   src/styles/app.css

Untracked new files:
  src/components/Header.jsx
  src/components/Footer.jsx

3 changes are not committed.
Would you like to commit?
```

### Viewing Changes

```
> Show me what changed
```

Claude will show changes using `git diff`.

**Actual Conversation Example:**

```
User: Show me what changed in App.jsx

Claude: [Running: git diff src/App.jsx]

@@ -5,6 +5,7 @@
 function App() {
   return (
     <div>
+      <Header />      // This line was added
       <h1>Hello World</h1>
+      <Footer />      // This line was added
     </div>
   )
 }

You added Header and Footer components.
```

> **Beginner Tip**: In diff, `+` means added lines, `-` means deleted lines. They may also be displayed in green and red.

---

## Using Branches

A branch is a "workspace." You can work separately without affecting the main code.

### Why You Need Branches

Let's imagine a real situation:

```
Situation: Website is running well
Task: Add new login feature

Without branches:
1. Start working directly on main
2. Login feature half done (not working yet)
3. Urgent! Bug found on existing page!
4. Login code and bug fix get mixed up
5. Confusion about what to deploy

With branches:
1. Create feature/login branch
2. Working on login feature
3. Urgent bug -> switch to main, fix, deploy
4. Switch back to feature/login, continue working
5. Merge to main when complete
```

### Branch Naming Conventions

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New feature | feature/login |
| `fix/` | Bug fix | fix/header-overlap |
| `hotfix/` | Urgent fix | hotfix/security-patch |
| `refactor/` | Code improvement | refactor/api-cleanup |

### Creating a Branch

```
> Create a branch for the login feature
```

Claude will run a command like `git checkout -b feature/login`.

**Actual Conversation Example:**

```
User: Create a branch for the login feature

Claude: I'll create a new branch.

[Running: git checkout -b feature/login]

Created and switched to 'feature/login' branch.

Current status:
- Previous branch: main
- Current branch: feature/login

You can now freely work on the login feature in this branch.
The main branch is not affected.
```

### Checking Branches

```
> What branch am I on?
```

```
> Show me the list of branches
```

**Actual Conversation Example:**

```
User: Show me the list of branches

Claude: [Running: git branch -a]

Branch list:
* feature/login      <- current branch
  main
  feature/signup

Remote branches:
  origin/main
  origin/feature/signup

There are 5 branches in total.
You are currently on the feature/login branch.
```

### Switching Branches

```
> Switch to main branch
```

**Situation to be careful about:**

```
User: Switch to main

Claude: [Running: git checkout main]

Wait! There are uncommitted changes:
- src/Login.jsx (modified)

Options:
1. Commit changes and switch
2. Stash changes temporarily and switch
3. Discard changes and switch (Warning: unrecoverable)

What would you like to do?
```

> **Caution**: If you switch branches with uncommitted changes, the changes may follow you or cause conflicts. Always commit or stash before switching.

---

## Creating Pull Requests (PR)

A PR is a request to "merge my changes into main."

### Why PRs Are Needed

- **Code Review**: Others can review the code
- **Safe Merge**: Detect conflicts or bugs early
- **Documentation**: Record why this change was made
- **Collaboration**: Team members can understand the changes

### Having Claude Create a PR

```
> Create a PR for this branch
```

Claude will:
1. Analyze changes
2. Write PR title and description
3. Create PR on GitHub

**Actual Conversation Example:**

```
User: Create a PR for this branch

Claude: Let me check the changes before creating the PR.

[Running: git log main..HEAD]

Commits in this branch:
1. feat: Implement login form UI
2. feat: Connect login API
3. fix: Fix password validation bug

I'll create the PR:

[Running: gh pr create --title "feat: Add login functionality" --body "..."]

PR created!
URL: https://github.com/user/repo/pull/42

Would you like to assign reviewers?
```

### PR Format

```markdown
## Summary
- Implement login form UI
- Add email/password validation
- Connect login API

## Test Plan
- [x] Test normal login
- [x] Test incorrect password
- [x] Test email format validation

## Screenshots
(Attach screenshots if available)
```

> **Pro Tip**: If you explain "why" this change is needed in the PR description, reviewers will understand more easily. "Implemented email-based login system for user authentication" is better than simply "Add login."

---

## Resolving Conflicts

When multiple people modify the same file, a "conflict" can occur.

### Why Conflicts Happen

```
Situation:
1. A creates feature-a branch from main
2. B also creates feature-b branch from main
3. A and B modify the same line in the same file differently
4. A merges to main first
5. When B tries to merge -> conflict!

Git asks: "Which version should I use?"
```

### When You Get a Conflict

```
> There's a merge conflict. Help me resolve it.
```

Claude will:
1. Explain what each version is
2. Suggest how to combine them
3. Apply the fix

**Detailed Conversation Example:**

```
User: There's a merge conflict. Help me resolve it.

Claude: Let me check the conflict.

[Running: git status]

Conflict file:
- src/App.jsx

Let me show you the conflict:

<<<<<<< HEAD (current branch)
function App() {
  return <div className="app-blue">Hello</div>
}
=======
function App() {
  return <div className="app-green">Hello</div>
}
>>>>>>> main (branch being merged)

Current branch: Blue background applied
main branch: Green background applied

How would you like to resolve this?
1. Keep current branch (blue)
2. Keep main branch (green)
3. Use both (e.g., conditionally)
4. Specify manually

User: Go with blue

Claude: I'll keep the blue version and resolve the conflict.

[After file modification]
[Running: git add src/App.jsx]
[Running: git commit -m "resolve: Resolve App.jsx conflict (keep blue background)"]

Conflict resolved!
```

**Tips for Conflict Resolution Requests:**

```
> Resolve the conflict. I want to keep my branch's login logic.
> I need both versions. Please combine them.
```

Telling Claude which version you prefer, or if you need both, helps resolve it according to your intent.

> **Caution**: Be careful not to accidentally delete someone else's code when resolving conflicts. All conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) must be removed.

---

## Git Safety Rules

Claude asks before running dangerous git commands.

### Safe Commands (Can Run Automatically)

- `git status` - Check status
- `git diff` - View changes
- `git log` - View history
- `git add` - Add files
- `git commit` - Make commits
- `git branch` - Check branches
- `git checkout` - Switch branches

### Dangerous Commands (Need Confirmation)

| Command | Risk Level | Reason |
|---------|-----------|--------|
| `git push` | Medium | Upload to server (hard to undo) |
| `git push --force` | Very High | Overwrite server history |
| `git reset --hard` | Very High | Permanently delete changes |
| `git clean -fd` | Very High | Delete untracked files |
| `git rebase` | Medium | Modify commit history |

**Actual Conversation Example:**

```
User: I want to undo the last commit

Claude: There are several ways to undo the last commit:

1. Undo commit only, keep changes (safe)
   git reset --soft HEAD~1

2. Undo commit and staging, keep changes (safe)
   git reset --mixed HEAD~1

3. Delete commit and all changes (dangerous)
   git reset --hard HEAD~1

Which method would you like?
Recommended: Option 1 (undo commit while keeping changes)
```

---

## Mini Quiz

Let's check if you understand the concepts!

### Quiz 1
Q: What's the difference between `git commit` and `git push`?

<details>
<summary>View Answer</summary>

- `git commit`: Save changes on my computer (local save)
- `git push`: Upload saved commits to server (GitHub)

If you only commit, it's only on your computer; you need to push to upload to GitHub.
</details>

### Quiz 2
Q: What's the biggest reason to use branches?

<details>
<summary>View Answer</summary>

Because you can safely develop new features without affecting the main code (main branch). Even if the code you're working on is unstable, main always works properly.
</details>

### Quiz 3
Q: If you run "git status" and get "nothing to commit, working tree clean"?

<details>
<summary>View Answer</summary>

It means no files have changed since the last commit. It's a clean state with nothing new to commit.
</details>

---

## Practice Exercises

### Level 1: Basic Workflow (Beginner)

```
# 1. Create new folder and initialize Git
> Create my-project folder and initialize git

# 2. Create a file
> Create index.html

# 3. Make a commit
> Commit it

# 4. Modify the file
> Change the title

# 5. Commit again
> Commit the changes

# 6. Check history
> Show commit history
```

**Completion Check:**
- [ ] Folder is initialized as a git repository
- [ ] At least 2 commits exist
- [ ] Can view commit history with `git log`

### Level 2: Using Branches (Intermediate)

```
# 1. Create new branch
> Create feature/button branch

# 2. Modify file
> Add a button

# 3. Commit
> Commit it

# 4. Go back to main
> Switch to main branch

# 5. Check
> Is there a button? (Should not be - it's in a different branch)

# 6. Check branch list
> Show branch list
```

**Completion Check:**
- [ ] New branch is created
- [ ] Commit is made in the branch
- [ ] Changes are not visible in main
- [ ] Can freely switch between branches

### Level 3: GitHub Integration (Advanced)

```
# 1. Create and connect GitHub repository
> Upload this project to GitHub. Name the repository git-practice.

# 2. Make changes and commit
> Add my name to README
> Commit it

# 3. Push
> Push to GitHub

# 4. Create branch and PR
> Create feature/about-me branch
> Add introduction section
> Commit and create a PR
```

**Completion Check:**
- [ ] Repository is created on GitHub
- [ ] Local commits are reflected on GitHub
- [ ] PR is created

---

## Challenge Exercises

If you want to understand Git more deeply, try these challenges!

### Challenge 1: Commit Message Archaeology
```
> Show me the last 10 commit history.
> Also tell me which files changed in each commit.
```

### Challenge 2: Time Travel
```
> Show me what index.html looked like 3 commits ago
> (Just view, don't revert)
```

### Challenge 3: Conflict Simulation
Intentionally create a conflict and resolve it:
```
# Modify header in branch A
# Modify same header differently in branch B
# Attempt merge -> conflict!
# Resolve conflict
```

---

## When Problems Occur

**Getting "not a git repository" error?**
- You're not in a Git folder. Initialize first:
- Tell Claude: "Initialize git in this folder"

**Getting "nothing to commit" message?**
- No files have changed since the last commit
- Make changes first, then commit

**Getting "conflict" when merging?**
- Two branches modified the same lines
- Tell Claude: "Help me resolve this merge conflict"

**Accidentally committed the wrong thing?**
- Don't panic. "Undo the last commit but keep the changes"
- Or: "Show me the last 5 commits" to find where to go back

**Pushed something you shouldn't have?**
- Warning: This is hard to undo
- For sensitive files, contact your team lead
- For code mistakes: make a new commit that fixes it

**Git commands confusing you?**
- You don't need to memorize them!
- Tell Claude what you want: "I want to see my changes" or "Save my work"

**Getting "Permission denied" error?**
- GitHub authentication issue
- Tell Claude: "Help me set up GitHub authentication"

---

## Common Mistakes

### Mistake 1: Not Committing Regularly

```
# Bad example - one huge commit after days of work
> Commit all changes as "did work"

# Good example - small, frequent commits
> Commit login form
> Commit validation
> Commit error handling
```

**Why is this a problem?**
- Hard to find where things went wrong when problems occur
- When reverting, other good changes are also lost
- History becomes meaningless

### Mistake 2: Vague Commit Messages

```
# Bad examples
> Commit as "fixed"
> Commit as "updated"
> Commit as "changes"

# Good examples
> Commit it. I fixed the email validation bug.
> Commit it. I added a password visibility toggle to the login form.
```

**Why is this a problem?**
- Later you can't tell "what did I change back then?"
- Team members don't know the intent of changes

### Mistake 3: Working on Everything in Main Branch

```
# Bad example - all changes directly on main
> [make changes on main]
> commit

# Good example - use branches for features
> Create a branch for the new feature
> [make changes]
> commit
> Create a PR to merge into main
```

**Why is this a problem?**
- Main is always in an unstable state
- No clean deployable version
- Confusion during collaboration

### Mistake 4: Not Checking Status Before Committing

```
# Bad example - blindly commit everything
> Commit all changes

# Good example - check what you're committing
> What files changed?
> [review list]
> Commit only the login-related files
```

**Why is this a problem?**
- Temporary or test files get mixed in
- Secret files like .env might get committed

### Mistake 5: Not Setting Up .gitignore

```
# Bad example - committing node_modules
(thousands of files uploaded to GitHub)

# Good example - set up .gitignore
> Create a .gitignore file. Ignore node_modules and .env.
```

**Why is this a problem?**
- Repository becomes huge
- Push/pull becomes slow
- Risk of exposing secret information

---

## Glossary

| Term | Description |
|------|-------------|
| **Repository (Repo)** | Project folder managed by Git |
| **Commit** | Unit of saving changes |
| **Branch** | Independent workspace |
| **Merge** | Combining branches |
| **Pull Request (PR)** | Request to merge code |
| **Clone** | Copy a remote repository |
| **Fork** | Copy someone else's repository to your account |
| **Staging** | Selecting files to commit |
| **HEAD** | The currently checked out commit |
| **Origin** | Default name for remote repository |

---

## Summary

What we learned in this chapter:
- [x] What Git is and why it's essential
- [x] Saving changes with commits
- [x] Checking status and viewing changes
- [x] Working safely with branches
- [x] Requesting code merges with PRs
- [x] How to resolve conflicts

You don't need to memorize Git commands. What's important is **understanding the concepts**. If you understand concepts like "commit is a save point" and "branch is a workspace," you can convey your intent to Claude, and Claude will execute the appropriate commands.

> **Key Point**: Git removes the fear of making mistakes. When you have the safety net of being able to undo anytime, you can experiment more boldly and learn.

---

## Next Chapter Preview

In Chapter 12, we'll learn about **Project Memory (CLAUDE.md)**.

Until now, you had to repeatedly explain project settings every time you talked with Claude, right? With CLAUDE.md, Claude "remembers" the project rules, so you don't have to repeat the same explanations.

If Git is the **memory of code**, CLAUDE.md is the **memory of AI**.

Continue to [Chapter 12: Project Memory](../Chapter12-Project-Memory/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [Git Official Documentation](https://git-scm.com/doc) - Complete Git guide
- [GitHub Guides](https://docs.github.com/en) - Official GitHub documentation
- [Pro Git Book](https://git-scm.com/book/en/v2) - Free Git textbook

**Video Resources:**
- [Git Tutorial for Beginners (YouTube)](https://www.youtube.com/results?search_query=git+tutorial+beginners) - Git basics
- [GitHub Tutorial (YouTube)](https://www.youtube.com/results?search_query=github+tutorial) - GitHub usage

**Reading Materials:**
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive Git learning
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) - Comprehensive Git guides

**Related Tools:**
- [GitHub Desktop](https://desktop.github.com/) - Git GUI client
- [GitKraken](https://www.gitkraken.com/) - Visual Git client
- [Sourcetree](https://www.sourcetreeapp.com/) - Free Git GUI

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
