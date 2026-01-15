# Chapter 06: Effective Prompting

**English** | [한국어](./README.ko.md)

## What You Will Learn

- How to communicate with Claude effectively
- Using Plan mode
- Breaking down complex tasks

---

## Why Do You Need This?

Imagine ordering at a restaurant. If you just say "give me food," the waiter has no idea what you want. But if you say "I'd like a medium-rare steak with mashed potatoes," you get exactly what you want.

Prompting works the same way. The more clearly you communicate with Claude, the better results you get.

**Real-world scenarios:**
- You have a bug but just say "fix it" - Claude doesn't know what's broken
- You want a website but just say "make a website" - Claude doesn't know what kind
- You need a function but don't explain what it should do - Claude guesses (often wrong)

**Think of prompting like giving directions:**
- Bad: "Go that way" (vague, unhelpful)
- Good: "Turn left at the coffee shop, then go straight for 2 blocks" (clear, actionable)

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

When LLMs receive a request, they immediately start generating in the direction that seems most appropriate. But this "seems appropriate" doesn't always match your intent. Reviewing the plan in Plan mode first lets you adjust direction before execution.

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

LLMs go through an internal "reasoning process" when generating responses. More complex problems require more reasoning. Certain keywords can guide Claude to use more reasoning time.

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

## Try It Yourself

Let's practice prompting right now. Open Claude Code and try these:

### Exercise 1: Turn a Bad Prompt into a Good One

```
# Start with this bad prompt:
> Make a button

# Now try this improved version:
> Make a blue button with the text "Submit".
> It should be 200px wide and have rounded corners.
> When I hover over it, make it slightly darker.
```

See the difference? The second prompt gives Claude exactly what you want.

### Exercise 2: Use Context

```
# Try this in any project folder:
> @package.json What framework is this project using?
> Now create a component that fits this project's style.
```

### Exercise 3: Step-by-Step Approach

```
> I want to build a contact form. Let's do it step by step.
> First, just create the HTML structure with name, email, and message fields.
```

Wait for Claude to finish, check the result, then continue:

```
> Good. Now add CSS to make it look nice.
```

---

## If It Doesn't Work?

**Claude is not responding as expected?**
- Check if your prompt is too vague
- Add more context: what file, what error, what you expected

**Claude made something completely different?**
- Press `Esc Esc` to undo
- Try breaking your request into smaller pieces
- Use Plan mode first: `/plan How should I build this?`

**Claude keeps making the same mistake?**
- Be more explicit: "Do NOT use inline styles" instead of "use good styling"
- Reference an existing file: "Follow the same pattern as @src/Button.js"

**Still stuck?**
- Start a new conversation (sometimes context gets confusing)
- Try `ultrathink` for complex problems

---

## Common Mistakes

### Mistake 1: Being Too Vague
```
# Bad
> Fix the code

# Good
> In @src/login.js line 42, the email validation is failing.
> It allows "test@" without a domain. Fix this.
```

### Mistake 2: Asking for Everything at Once
```
# Bad
> Build me a complete e-commerce site with user auth,
> payment, inventory, admin panel, and analytics

# Good
> Let's start with user registration.
> Just the signup form for now.
```

### Mistake 3: Not Providing Examples
```
# Bad
> Format dates nicely

# Good
> Format dates like this: "January 15, 2024"
> Input: "2024-01-15"
> Output: "January 15, 2024"
```

### Mistake 4: Forgetting to Mention Constraints
```
# Bad
> Add form validation

# Good
> Add form validation.
> - Email must have @ and a domain
> - Password must be at least 8 characters
> - Show error messages in red below each field
```

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
