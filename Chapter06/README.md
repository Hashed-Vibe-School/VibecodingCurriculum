# Chapter 06: Effective Prompting

**English** | [한국어](./README.ko.md)

## What You Will Learn

- How to communicate with Claude effectively
- Using Plan mode
- Breaking down complex tasks

---

## What is Prompting?

Prompting is how you communicate with AI. The same question phrased differently can produce very different results.

### Bad vs Good Examples

**Bad:**
```
> Fix the bug
```
Claude does not know what to fix.

**Good:**
```
> In @src/login.js, I get an error when clicking the login button.
> The error is "Cannot read property 'email' of undefined".
> Please fix it.
```
Claude knows exactly what to do.

---

## 3 Principles of Good Prompts

### 1. Be Specific

```
# Bad
> Make a website

# Good
> Make a self-introduction page.
> It should have sections for name, photo, and bio.
> Use a blue background.
```

### 2. Provide Context

```
# Bad
> Add a function

# Good
> Add a date format function to @src/utils.js.
> Similar style to the other functions there.
> It should convert "2024-01-15" to "January 15, 2024".
```

### 3. One Thing at a Time

```
# Bad
> Make login and signup and password reset

# Good
> Start with login first.
> A form that takes email and password.
```

---

## Using Plan Mode

Plan mode makes Claude plan before executing.

### Why Plan Mode?

If Claude writes code immediately, it might go in the wrong direction. Review the plan first in Plan mode, then execute if it looks good.

### How to Use

```
> /plan
```
Or press `Shift + Tab` twice

### Example

```
> /plan
> How should I build the login feature?
```

Claude shows the plan:
```
Login feature implementation plan:

1. Create src/components/LoginForm.js
   - Email, password input fields
   - Submit button

2. Create src/api/auth.js
   - Login API call function

3. Modify existing App.js
   - Add login state management

Proceed with this plan?
```

If it looks good, switch to Normal mode to execute.

---

## Working Step by Step

Break down complex tasks.

### Bad Example

```
> Make a shopping mall
```
Too large of a request.

### Good Example

```
# Step 1: Understand
> What features does a shopping mall usually have?

# Step 2: Plan
> Start with the product list page.
> How should I build it?

# Step 3: Execute
> Build it according to that plan.

# Step 4: Verify
> Check if it works. Run it.
```

---

## Providing Feedback

If the first result is not what you want, provide feedback.

### Example

```
> Make a button
```
If the result is too small:

```
> The button is too small. Make it bigger.
> Make the text bolder too.
```

Keep refining.

### Undoing

If it went completely wrong:
- `Esc Esc` (Esc twice): Revert to previous state
- `Ctrl + C`: Cancel current operation

---

## Ultrathink: Solving Complex Problems

For difficult problems, make Claude think more deeply.

### Keywords

| Keyword | Effect | When to Use |
|---------|--------|-------------|
| `think` | Normal level | General tasks |
| `think hard` | Deep thinking | Complex problems |
| `ultrathink` | Maximum depth | Architecture decisions |

### Example

```
> ultrathink
> How should I design this project structure?
> So it's easy to add features later.
```

---

## Practice

### Practice 1: Being Specific

Turn a bad prompt into a good one:

```
# Bad
> Fix the error

# Turn this into something specific
> ???
```

### Practice 2: Using Plan Mode

```
> /plan
> I want to make a todo list app.
> It should have add, delete, and check-complete features.
> How should I build it?
```

Review the plan and execute if it looks good.

### Practice 3: Step by Step

```
# Step 1
> I want to make a simple calculator. What do I need?

# Step 2
> Start with addition.

# Step 3
> Add subtraction, multiplication, and division.

# Step 4
> Make it look nice.
```

---

## Frequently Used Patterns

### When Exploring

```
> Explain the project structure
> What does this function do?
> How does the data flow?
```

### When Fixing Bugs

```
> I'm getting this error message: [error message]
> @filename I think the problem is in this file
> Fix it
```

### When Building New Features

```
> I want to build [feature]
> Requirements: [list]
> Reference @existingfile and use a similar style
```

---

## Advanced: Prompting Techniques

### Limit the Scope

The more freedom AI has, the more it may go in unexpected directions. "Make a button" could result in 10 helper functions and 3 utility files.

Being clear about scope prevents this:

```
> Create a button component.
> Handle it in this one file, do not create additional files.
```

### Provide Background Context

The same request can have different optimal solutions depending on context.

| No Context | With Context |
|-----------|--------------|
| "Optimize this function" | "This function is called on every page load so it needs to be fast" |
| "Build login" | "This is for a hackathon prototype. Does not need to be complex" |

When Claude knows "why you are doing this," it can choose appropriate tradeoffs.

### When Results Are Not Good

If you are not getting desired results, review your request:

- Was it too vague?
- Did I omit important context?
- Did I ask for too much at once?

Improving your request improves results. This is often a communication issue, not a model limitation.

### The Power of Breaking Things Down

Complex systems are combinations of small parts. AI agents work similarly—they perform small tasks well and combine them to create complex results.

When requesting large tasks at once:
- Hard to identify where things went wrong
- Difficult to change direction

When breaking into small units:
- You can verify each step's results
- If something fails, just redo that step
- Progress is clear

```
# All at once
> Build the entire payment system

# Step by step
> First, just make the payment button UI
> (after verification) Connect the payment API
> (after verification) Create the payment complete page
```

### The Iterative Improvement Cycle

You do not need to be perfect from the start. Working with AI is an iterative refinement process:

1. Try something
2. Check the result
3. Provide feedback on what is lacking
4. Get a better result

Spinning this cycle quickly is more efficient than trying to craft the perfect request on the first try.

---

## Summary

What you learned in this chapter:
- [x] Being specific
- [x] Providing context
- [x] One thing at a time
- [x] Using Plan mode
- [x] Working step by step
- [x] Providing feedback

**Key point**: Good communication leads to good results.

Move on to [Chapter 07: Exploring Code](../Chapter07/README.md).
