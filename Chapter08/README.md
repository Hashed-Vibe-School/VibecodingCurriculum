# Chapter 08: Editing Code

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Getting Claude to edit code
- Safe editing
- Improving with feedback

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
- If Claude writes code immediately, it might go the wrong direction
- Seeing the plan first lets you adjust the direction

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

## Summary

What you learned in this chapter:
- [x] Getting Claude to edit code
- [x] Explore, Plan, Execute workflow
- [x] Improving with feedback
- [x] Undoing
- [x] Safe editing

Move on to [Chapter 09: Git Basics](../Chapter09/README.md).
