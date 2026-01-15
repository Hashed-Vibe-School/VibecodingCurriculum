# Chapter 08: Editing Code

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Getting Claude to edit code
- Safe editing
- Improving with feedback

---

## Why Do You Need This?

Imagine you're writing an essay. Instead of typing every sentence yourself, you tell an assistant what you want, and they write it for you. You review it, suggest changes, and they revise.

Code editing with Claude works the same way. You describe what you want, Claude writes the code, you review and refine.

**Real-world scenarios:**
- You want to add a new feature but aren't sure how to implement it
- There's a bug in the code and you need to fix it fast
- You want to refactor messy code into something cleaner
- You're learning and want to see how something should be written

**Think of it like having a pair programmer:**
- Traditional coding: You type everything yourself
- With Claude: You describe what you want, Claude types, you guide and approve

---

## Code Editing Basics

Claude Code can directly edit code. It modifies files without copy-paste.

### Simple Example

```
> @index.html Change the title to "My Website"
```

Claude opens the file, edits it, and saves.

### Reviewing Changes

Before editing, Claude shows what will change:

```diff
- <h1>Hello World</h1>
+ <h1>My Website</h1>
```

- Red (-): Being deleted
- Green (+): Being added

Select `Allow` if okay, `Deny` if not.

---

## Recommended Workflow: Explore, Plan, Execute

This is the method recommended by Anthropic.

### Step 1: Explore

First, understand the code.

```
> @src/login.js What does this file do?
```

### Step 2: Plan

Create an editing plan.

```
> I want to add password validation here.
> How should I do it? Do not write code yet, just plan.
```

### Step 3: Execute (Code)

Edit according to the plan.

```
> Okay, edit according to that plan.
```

### Step 4: Verify

Check if it works.

```
> Run the tests to verify.
```

**Why do this?**

When LLMs receive a request, they select the most appropriate pattern from their training. But even "password validation" has multiple implementation approaches (regex, libraries, custom logic, etc.). Reviewing the plan first:
- Shows you which direction Claude is heading
- Lets you correct course before any code is written

---

## Improving with Feedback

The first result does not need to be perfect. Provide feedback.

### Example

```
> Make a login form
```

If the result does not suit you:

```
> The error messages are too stiff. Make them friendlier.
```

```
> The button is too small. Make it bigger.
```

2-3 rounds of feedback makes it much better.

---

## Undoing

Undo if something went wrong.

| Shortcut | What it does |
|----------|--------------|
| `Esc` | Stop current operation |
| `Esc Esc` | Revert to previous state |
| `Ctrl + C` | Cancel completely |

### Example

If Claude is going in a wrong direction:

```
# Press Esc Esc then
> Not that. Let me explain again.
> Keep the existing login logic, just add OAuth.
```

---

## Good vs Bad Edit Requests

### Bad Example

```
> Fix the bug
```
Does not know what to fix.

### Good Example

```
> In the validateEmail function in @src/utils.js,
> there's a bug where invalid emails like "test@" pass through.
> Fix it to check that the domain part has a dot (.).
```

### Even Better (Reference Existing Patterns)

```
> Add a validatePhone function to @src/utils.js.
> Similar style to validateEmail in @src/utils.js.
```

Referencing existing code produces consistent style.

---

## TDD Style: Tests First

Writing tests first tells Claude exactly what to build.

### Example

```
# Step 1: Tests first
> Write tests for formatDate function.
> "2024-01-15" should return "January 15, 2024".
> Empty string should throw an error.

# Step 2: Run tests (verify failure)
> Run the tests.

# Step 3: Implement
> Now make the function pass the tests.

# Step 4: Verify again
> Run the tests again.
```

---

## Refactoring Patterns

### Extract Function

```
> Extract the validation logic from lines 50-70 in @src/App.js
> into a validateForm function.
```

### Rename

```
> Rename getUserData function to fetchUserProfile.
> In all files.
```

### Move File

```
> Move @src/utils/helpers.js
> to @src/lib/helpers.js.
> Update all import statements too.
```

---

## Safe Editing

### Using Git

Commit before major edits.

```
> Commit current state. Backup before refactoring.
```

If problems occur:

```
> Something went wrong. Revert to last commit.
```

### Using Permission Modes

- **Plan mode**: When exploring (no edits allowed)
- **Normal mode**: When learning (confirm each time)
- **Accept Edits**: When comfortable (auto-approve)

---

## Practice

### Practice 1: Basic Editing

```
# Create file
> Create hello.html. A page that shows "Hello World".

# Edit
> Change the title to "Hello".

# Add
> Add a button. Alert on click.
```

### Practice 2: Explore, Plan, Execute

```
# Explore
> @package.json What does this project do?

# Plan
> I want to add a user profile feature. Make a plan.

# Execute
> Start with step 1.
```

### Practice 3: Improve with Feedback

```
> Make a simple calculator HTML.

# After seeing first result
> The design is too plain. Make it prettier.

# After seeing again
> The buttons are too small. Make them bigger.
```

---

## Mini Project: Code Refactoring

Practice editing skills by refactoring real code.

### Goals

- Master code editing skills
- Experience refactoring patterns

### Preparation: Create Sample Code

```
> Create legacy.js with the following:
> - About 100 lines of complex code
> - Has duplicate code
> - Unclear function names
> - No comments
```

### Refactoring

```
> @legacy.js Analyze this code. What can be improved?

> Separate duplicate code into functions

> Rename functions to be meaningful

> Add comments where needed
```

### Advanced Challenges (For Experts)

```
> Modernize this code to ES6+ syntax

> Convert to TypeScript. Add proper types.

> Add unit tests. For each function.
```

### Challenge

Try refactoring a real open source project:

```bash
git clone https://github.com/sindresorhus/ora.git
cd ora
claude
```

```
> Find parts of this project that can be improved
> Pick one and create a refactoring plan
```

---

## Try It Yourself

Let's practice code editing right now:

### Exercise 1: Create and Edit a Simple File

```
# Create a file
> Create a file called greeting.js with a function that says "Hello World"

# Review what Claude created, then modify
> Change it to say "Hello, [name]" where name is a parameter

# Add more
> Add another function that says "Goodbye, [name]"
```

### Exercise 2: The Explore-Plan-Execute Flow

```
# Step 1: Explore
> @package.json What does this project do?

# Step 2: Plan
> I want to add a dark mode toggle. How should I do it? Don't code yet.

# Step 3: Execute
> Looks good. Let's start with step 1.

# Step 4: Review
> Run it and show me if it works.
```

### Exercise 3: Iterative Improvement

```
> Create a simple contact form in HTML

# After seeing the result
> The form looks plain. Add some CSS to make it look professional.

# After seeing the result
> The submit button needs to be bigger and blue.

# After seeing the result
> Add form validation - email must have @ and message must not be empty.
```

---

## If It Doesn't Work?

**Claude made changes you didn't want?**
- Press `Esc Esc` to undo immediately
- Say: "Undo that. I wanted [what you actually wanted]"

**The edit broke something?**
- Check if you committed before editing (use Git!)
- Say: "Something broke. Revert to the last commit."

**Claude is editing the wrong file?**
- Be specific: "@src/components/Button.js modify this file"
- Check you're mentioning the right path

**Changes look correct but aren't saved?**
- Make sure you selected "Allow" when Claude asked for permission
- Try: "Save the changes to [filename]"

**Claude keeps doing something you don't want?**
- Add it to CLAUDE.md: "Never use inline styles"
- Be explicit: "Do NOT add new dependencies"

---

## Common Mistakes

### Mistake 1: Not Reviewing Changes
```
# Bad - Just accepting everything without looking
> Make this better
[Claude changes 20 lines]
[You click Allow without reading]

# Good - Review before accepting
> Make this better
[Claude shows diff]
[You read the diff carefully]
[You see something wrong]
> Wait, don't change line 15. Keep that part.
```

### Mistake 2: Skipping the Plan Step
```
# Bad - Jump straight to coding
> Add user authentication to my app

# Good - Plan first
> /plan
> I want to add user authentication. What's the best approach?
> [Review plan]
> That looks good. Let's start.
```

### Mistake 3: Giving Vague Edit Requests
```
# Bad
> Fix the bug

# Good
> In @src/login.js, the login button doesn't work.
> When clicked, it should call the loginUser function.
> Currently nothing happens.
```

### Mistake 4: Not Using Git for Safety
```
# Bad - No safety net
> Refactor the entire codebase
[Something breaks, no way to recover]

# Good - Commit first
> Commit current changes as "backup before refactoring"
> Now refactor the utils folder
[Something breaks]
> Revert to the backup commit
```

### Mistake 5: Editing Too Much at Once
```
# Bad
> Rewrite this entire file to use TypeScript

# Good
> First, just add TypeScript types to the function parameters
> [Review]
> Now add types to the return values
> [Review]
> Now add an interface for the user object
```

---

## Summary

What you learned in this chapter:
- [x] Getting Claude to edit code
- [x] Explore, Plan, Execute workflow
- [x] Improving with feedback
- [x] Undoing
- [x] Safe editing

Move on to [Chapter 09: Git Basics](../Chapter09/README.md).
