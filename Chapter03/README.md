# Chapter 03: Code Editing

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, you should be able to:
- [ ] Navigate codebases using Glob and Grep
- [ ] Reference files with @-mentions
- [ ] Understand permission modes (especially Accept Edits)

---

## Introduction

Code editing is where Claude Code truly shines. But many developers start by saying "fix this bug" right away. This causes Claude to jump straight into coding, and the results often don't fit your codebase well.

This chapter focuses on the workflow officially recommended by Anthropic, teaching you code editing methods that actually boost productivity.

### Core Principle

> **"Explore → Plan → Code → Commit"** - This is the workflow most emphasized in Anthropic's official guide. Without these steps, Claude jumps straight to coding, and the results may not match your codebase.

---

## Topics

### 1. Core Workflow: Explore → Plan → Code → Commit

This is Anthropic's recommended 4-step workflow. **Without steps 1-2, Claude jumps straight to coding.**

```bash
# Step 1: Explore - Understand first (without writing code)
> Read @src/auth/login.ts and explain how the authentication flow works
> What error handling patterns does this codebase use?

# Step 2: Plan - Create a plan
> I want to add OAuth support. Create a plan for what files need to change
> and in what order. Don't write any code yet.

# Step 3: Code - Implement (incrementally)
> Let's start with step 1 of the plan: create the OAuth config file
> Now implement step 2: add the OAuth routes

# Step 4: Commit - Verify and commit
> Run the tests to make sure nothing broke
> Create a commit with these changes
```

**Why is this important?**
- Claude understands existing code patterns first
- You control the direction through planning
- Incremental implementation makes rollback easy if problems occur

### 2. The Power of Iteration: 2-3 Times is Enough

> **"The first version might be good, but after 2-3 iterations it will typically look much better."** - Anthropic Engineering

```bash
# 1st: Basic implementation
> Create a login form component

# 2nd: Incorporate feedback
> Add validation and error messages to the form
> The error message should appear below each field

# 3rd: Polish
> Add loading state and disable the button while submitting
> Follow the button styling in @components/Button.tsx
```

**Effective feedback when iterating:**
- Point out specific problems
- Provide existing code to reference
- Describe the desired outcome

### 3. Test-Driven Development (TDD) Approach

Writing tests first gives Claude clear targets.

```bash
# Step 1: Write tests first
> Write tests for a validateEmail function that should:
> - Return true for valid emails like "user@example.com"
> - Return false for invalid emails like "test@" or "@example.com"
> - Return false for empty strings

# Step 2: Run tests (confirm failure)
> Run the tests - they should fail since we haven't implemented yet

# Step 3: Implement
> Now implement validateEmail to pass all the tests

# Step 4: Re-run tests (confirm success)
> Run the tests again to confirm they pass
```

**Benefits of TDD:**
- Claude knows exactly what to implement
- Edge cases are defined upfront
- Automatic verification possible

### 4. Course Correction: Fixing Wrong Directions

When Claude is heading in the wrong direction:

| Shortcut | Action |
|----------|--------|
| `Esc` | Stop current operation |
| `Esc Esc` (twice) | Revert to previous state and retry |
| `Ctrl+C` | Cancel completely |

```bash
# When Claude goes in the wrong direction
> [Press Esc twice]

# Restart with more specific instructions
> Let me clarify: don't create a new file. Instead, modify the existing
> @src/auth/login.ts to add the OAuth logic alongside the current login.
```

### 5. Edit vs Write Tools

| Tool | When to Use | Behavior |
|------|-------------|----------|
| **Edit** | Modify part of existing file | Replace specific text only |
| **Write** | Create new file | Write entire file |

When Claude shows a diff:
```diff
- const result = data.map(item => item.value);
+ const result = data
+   .filter(item => item.isActive)
+   .map(item => item.value);
```

**Before accepting:**
- Is this the expected change?
- Are there unintended side effects?
- Does it match the existing code style?

### 6. Effective Edit Requests

#### Bad vs Good Requests

```bash
# Bad example - Claude has to guess
> fix the bug

# Good example - specific location, problem, desired behavior
> In @src/utils/validation.ts, the validateEmail function returns true
> for invalid emails like "test@". Fix this by checking that the domain
> part exists and contains at least one dot.
```

#### Referencing Existing Patterns

```bash
# Good example - provide pattern to follow
> Add a new endpoint for /api/products following the same pattern
> as @api/users.ts. Include validation middleware and error handling.
```

### 7. UI Work: Using Visual Feedback

When working on UI, sharing screenshots lets Claude "see" the results.

```bash
# Share screenshot after first implementation
> Here's how the form looks now: @screenshot.png
> The spacing between fields is too tight. Increase it to match
> the design in @designs/form-mockup.png

# Iterate
> Better! But the error message color doesn't match our theme.
> Use the error color from @styles/theme.ts
```

### 8. Refactoring Patterns

```bash
# Extract function
> Extract the validation logic in @src/components/Form.tsx (lines 45-60)
> into a separate validateForm function in @src/utils/validation.ts

# Rename (across project)
> Rename "getUserData" to "fetchUserProfile" across all files

# Move code
> Move the helper functions from @src/components/helpers.ts
> to @src/utils/helpers.ts and update all imports

# Add types
> Add TypeScript types to @src/api/client.js and rename to .ts
```

### 9. Safety Measures

#### Using Permission Modes

```bash
# When exploring: Plan Mode (read-only)
claude --permission-mode plan

# Trusted projects: Accept Edits
claude --permission-mode acceptEdits

# Default: Default (confirm each time)
claude
```

#### Git as a Safety Net

```bash
# Commit before major changes
> Let's commit the current state before the big refactor

# When problems occur
> The refactor broke something. Let's revert to the last commit
> and try a different approach
```

---

## Resources

- [Claude Code: Best practices for agentic coding](https://www.anthropic.com/engineering/claude-code-best-practices) - Anthropic Official Guide
- [Refactoring Guru](https://refactoring.guru/) - Refactoring Patterns

---

## Checklist

Answer these questions as if in an interview:

1. **What is Anthropic's recommended code editing workflow?**
   <details>
   <summary>Hint</summary>
   Explore → Plan → Code → Commit. Without steps 1-2, Claude jumps straight to coding.
   </details>

2. **Why are 2-3 iterations important?**
   <details>
   <summary>Hint</summary>
   First version is okay but gets much better with iteration. Feedback allows direction adjustment.
   </details>

3. **What are the benefits of the TDD approach?**
   <details>
   <summary>Hint</summary>
   Provides clear targets, defines edge cases, enables automatic verification.
   </details>

4. **What do you do when Claude goes in the wrong direction?**
   <details>
   <summary>Hint</summary>
   Press Esc twice to revert, restart with more specific instructions.
   </details>

5. **What are the characteristics of a good edit request?**
   <details>
   <summary>Hint</summary>
   Specific location, problem description, desired behavior, pattern to reference.
   </details>

---

## Mini Project

### Learning Goals

Complete these tasks to master this chapter:

- [ ] Implement a feature using the **Explore → Plan → Code → Commit** workflow
- [ ] Write tests first, then ask Claude to implement (TDD)
- [ ] Improve the first result with 2-3 rounds of feedback
- [ ] Experience correcting wrong direction with `Esc Esc`
- [ ] Generate new code by referencing existing patterns with @-mentions

### Try These Prompts

```bash
# Explore phase
> Explain how error handling works in this project. Show me examples.

# Plan phase
> I want to add user profile editing. Create a plan without writing code.

# TDD approach
> Write tests for a formatCurrency function first, then implement it.

# Iterative improvement
> This works, but add better error messages and loading states.

# Pattern reference
> Create a new API endpoint following the pattern in @api/users.ts
```

---

## Advanced

- [ ] Break complex refactoring into multiple stages
- [ ] Iteratively improve UI components using screenshot feedback
- [ ] Safe batch modifications in large codebases
- [ ] Combine Claude's editing with IDE refactoring tools
