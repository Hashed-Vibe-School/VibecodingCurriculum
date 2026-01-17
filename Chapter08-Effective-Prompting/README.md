# Chapter 08: Effective Prompting

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Understand the **scientific principles** of prompting
- Master **20 prompting patterns**
- Learn how to write **optimal prompts for different situations**
- Learn **advanced usage** of Plan mode and Ultrathink
- Learn **prompt debugging** techniques

---

## ⏱️ Estimated Time

- Reading: **40 minutes**
- Practice: **60 minutes**

---

## 📋 Prerequisites

- Claude Code installed and logged in
- Understanding of context and memory (Chapter 07)

---

## 🔗 Connection to Previous Chapter

In Chapter 07, you learned how to set project rules with CLAUDE.md. Now let's learn how to communicate effectively with Claude **with every request**. Good prompts are just as important as a good CLAUDE.md!

---

## Why is Prompting Important?

Imagine ordering at a restaurant. If you just say "give me food," the staff has no idea what you want. But if you say "I'd like a medium-rare steak with mashed potatoes," you get exactly what you want.

Prompting works the same way. The more clearly you communicate with Claude, the better results you get.

**Real-world scenarios:**
- You have a bug but just say "fix it" - Claude doesn't know what to fix
- You want a website but just say "make a website" - Claude doesn't know what kind
- You need a function but don't explain what it should do - Claude guesses (often wrong)

> 💡 **Beginner Tip**
>
> **Prompting is like giving directions:**
> - Bad: "Go that way" (vague and unhelpful)
> - Good: "Turn left at the coffee shop, then go straight for 2 blocks" (clear and actionable)
>
> Just like specific directions get you to your destination faster,
> specific prompts get you the results you want faster.

---

## The Science of Prompting

### How LLMs Process Prompts

LLMs work by predicting the next word. The more specific your prompt, the narrower the prediction range becomes, and the closer you get to your desired result.

```
Vague prompt:
"Make code" → Infinite possibilities → Hard to predict

Specific prompt:
"Create an email validation function in React.
Use regex and return error messages."
→ Narrow possibilities → Accurate prediction
```

### Relationship Between Prompt Quality and Results

| Prompt Quality | Example | Result |
|---------------|---------|--------|
| Very vague | "Fix it" | Claude doesn't know what to do |
| Vague | "Fix the button" | Which button? What's wrong? |
| Average | "The login button won't click" | Slightly better |
| Specific | "onClick in @src/Login.tsx button doesn't work" | Can diagnose accurately |
| Very specific | "@src/Login.tsx line 15 onClick, clicking doesn't call handleSubmit" | Can fix immediately |

> 🔥 **Pro Tip**
>
> **Prompt specificity = Result accuracy**
>
> Spending 1 more minute making your prompt specific
> can save 10 minutes of revision work.

---

## 5 Principles of Good Prompts

### 1. Be Specific

```
# Bad
> Make a website

# Good
> Make a self-introduction page.
> - 3 sections: name, photo, bio
> - Blue gradient background
> - Responsive, looks good on mobile too
> - Use Tailwind CSS
```

### 2. Provide Context

```
# Bad
> Add a function

# Good
> Add a date format function to @src/utils.js.
> Similar style to the other functions there.
> Input: "2024-01-15"
> Output: "January 15, 2024"
```

### 3. One at a Time

```
# Bad
> Make login, signup, and password reset

# Good
> Let's make the login feature first.
> A form that takes email and password.
> Submit sends a POST request to /api/login.
```

### 4. Provide Examples

```
# Bad
> Format the date

# Good
> Format dates like this:
> Input: "2024-01-15" → Output: "January 15, 2024"
> Input: "2024-12-25" → Output: "December 25, 2024"
```

### 5. Specify Constraints

```
# Bad
> Make a validation function

# Good
> Make an email validation function.
> - Use regex
> - Return false for empty strings
> - Return false if no @
> - Return false if domain has no .
> - Don't use external libraries
```

---

## 20 Prompting Patterns

### Basic Patterns (1-5)

#### 1. Role Assignment Pattern

Give Claude a specific role.

```
> You are a senior React developer.
> Analyze this component from a code review perspective.
```

```
> You are a UX expert.
> Suggest ways to improve this form's usability.
```

#### 2. Step-by-Step Pattern

Break complex tasks into steps.

```
> Let's build a shopping mall. Let's do it step by step.
>
> Step 1: Product list page
> Step 2: Product detail page
> Step 3: Shopping cart
> Step 4: Checkout
>
> Let's start with Step 1 first.
```

#### 3. Reference Pattern

Have Claude reference existing code.

```
> In the style of @src/components/Button.tsx,
> create an Input component.
```

```
> Like @src/api/users.ts,
> create a products API too.
```

#### 4. Comparison Pattern

Have Claude compare options.

```
> For state management, Redux vs Zustand vs Jotai,
> which would be best for this project?
> Compare pros and cons.
```

#### 5. Constraint Pattern

Specify what you don't want.

```
> Make a login form.
> - Don't use external libraries
> - Don't use CSS-in-JS
> - Don't use class components
> Use only Tailwind CSS.
```

### Analysis Patterns (6-10)

#### 6. Explanation Request Pattern

Ask how code works.

```
> @src/hooks/useAuth.ts
> Explain what this code does line by line.
> So that a non-developer can understand.
```

#### 7. Problem Diagnosis Pattern

Have Claude analyze error causes.

```
> I'm getting this error:
> "Cannot read property 'map' of undefined"
>
> Analyze why this error occurs
> in @src/ProductList.tsx.
```

#### 8. Tradeoff Analysis Pattern

Have Claude analyze pros and cons of decisions.

```
> If I convert this function to async/await,
> what are the advantages and disadvantages?
> Compare with the current callback approach.
```

#### 9. Review Pattern

Have Claude review code.

```
> @src/utils/validate.ts
> Review this code.
> - Potential bugs
> - Performance issues
> - Readability improvements
> Find each of these.
```

#### 10. Optimization Pattern

Request performance improvements.

```
> @src/components/Dashboard.tsx
> This component is slow.
> Find ways to optimize re-rendering.
> Consider React.memo, useMemo, useCallback.
```

### Generation Patterns (11-15)

#### 11. Template Pattern

Have Claude create reusable structures.

```
> Create a CRUD API endpoint template.
> I'll reuse it for users, products, orders later.
```

#### 12. Extension Pattern

Add features to existing code.

```
> @src/Button.tsx
> Add a loading state to this button.
> Show a spinner when isLoading prop is received.
```

#### 13. Conversion Pattern

Convert code to a different form.

```
> @src/utils.js
> Convert this JavaScript file to TypeScript.
> Define proper types too.
```

#### 14. Test Generation Pattern

Have Claude create test code.

```
> @src/utils/calculate.ts
> Create unit tests for these functions.
> Use Jest, include edge cases.
```

#### 15. Documentation Pattern

Have Claude generate documentation.

```
> @src/api/
> Document all API functions in this folder
> in JSDoc format.
```

### Advanced Patterns (16-20)

#### 16. Simulation Pattern

Have Claude assume situations and respond.

```
> Assume possible security vulnerabilities
> in this login system and suggest countermeasures.
```

#### 17. Debugging Guide Pattern

Get guidance on the debugging process.

```
> Form submission isn't working.
> Where should I check to find the cause?
> Create a step-by-step checklist.
```

#### 18. Refactoring Pattern

Have Claude improve code structure.

```
> @src/pages/Dashboard.tsx
> This file is 500 lines.
> Refactor by separating components.
> So each component has a single responsibility.
```

#### 19. Migration Pattern

Change the tech stack.

```
> Migrate this project from React 17 to 18.
> Create a list of required changes
> and apply them one by one.
```

#### 20. Architecture Pattern

Have Claude design the overall structure.

```
> ultrathink
> Design the folder structure and
> component architecture for this shopping mall project.
> Consider scalability and maintainability.
```

---

## Deep Dive into Plan Mode

Plan mode makes Claude plan before executing.

### Why Plan Mode is Important

When LLMs receive a request, they immediately start generating in what seems like the most appropriate direction. But this "seems appropriate" doesn't always match your intent.

```
# Without Plan mode
> Make an authentication system

[Claude immediately starts implementing JWT-based]
[But I wanted session-based...]
[Have to redo everything after it's done]
```

```
# Using Plan mode
> /plan
> Make an authentication system

Claude: Authentication system implementation plan:
1. Choose between JWT vs session
2. Login/logout API
3. Protected route middleware
...
Which approach do you prefer?

> Go with session-based
[Proceeds exactly in the direction I wanted!]
```

### How to Use

```
> /plan
```
Or press `Shift + Tab` twice

### Plan Mode Usage Patterns

#### Pattern 1: Large-scale Feature Development

```
> /plan
> Make a payment system.
> Stripe integration, supporting both subscription and one-time payments.

Claude: Payment system implementation plan:

Phase 1: Stripe Setup
- API key configuration
- Webhook endpoint

Phase 2: One-time Payments
- Payment form UI
- Checkout Session creation
- Payment completion handling

Phase 3: Subscription Payments
- Define subscription plans
- Subscription creation/cancellation
- Subscription status management

Shall we proceed in this order?
```

#### Pattern 2: Review Before Refactoring

```
> /plan
> @src/pages/Dashboard.tsx
> I want to refactor this 500-line file.
> How should I split it up?

Claude: Refactoring plan:

Current structure analysis:
- Chart rendering (100 lines)
- Statistics calculation (80 lines)
- Filter logic (70 lines)
- Data fetching (50 lines)
- UI layout (200 lines)

Suggested separation:
1. DashboardLayout.tsx - Layout
2. StatisticsPanel.tsx - Statistics
3. ChartSection.tsx - Charts
4. useFilters.ts - Filter hook
5. useDashboardData.ts - Data hook

Shall we proceed with this?
```

#### Pattern 3: Technology Selection

```
> /plan
> I want to introduce a form validation library.
> Which is better: react-hook-form, formik, or Zod?

Claude: Form validation library comparison:

react-hook-form:
- Pros: Good performance, small bundle
- Cons: Learning curve

Formik:
- Pros: Intuitive, good documentation
- Cons: Many re-renders

Zod:
- Pros: Type-safe, reusable
- Cons: Needs separate form library

For this project's characteristics, I recommend react-hook-form + Zod combination.
Reason: You're using TypeScript, and performance seems important with many forms.

Shall we proceed with this combination?
```

> 💡 **Beginner Tip**
>
> **When should you use Plan mode?**
>
> - When developing new features
> - Before major changes
> - When choosing between multiple approaches
> - Before complex refactoring
>
> It's unnecessary for small fixes (bug fixes, text changes).

---

## Ultrathink: Solving Complex Problems

LLMs go through an internal "reasoning process" when generating responses. More complex problems require more reasoning. Certain keywords can guide Claude to use more reasoning time.

### Easy Analogy: Taking an Exam

```
┌─────────────────────────────────────────────────────────────────┐
│                  Ultrathink = More Time to Think                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📝 Simple question: "1 + 1 = ?"                               │
│     → Answer immediately: 2 (no thinking needed)               │
│     → Request without keyword                                   │
│                                                                 │
│  📝 Normal question: "Find the bug in this code"               │
│     → Read code → Analyze → Respond                            │
│     → "think" keyword                                          │
│                                                                 │
│  📝 Hard question: "Design this system's architecture"         │
│     → Understand requirements → Review options → Analyze       │
│       pros/cons → Decide                                       │
│     → "ultrathink" keyword                                     │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  💰 Cost: More thinking = Higher cost                          │
│     Using ultrathink for simple questions =                    │
│     Spending 3 hours to solve 1+1                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Beginner Tip**
>
> ultrathink is an instruction saying **"think more deeply"**.
>
> When a doctor sees a patient:
> - Cold symptoms → Diagnose immediately (default)
> - Complex symptoms → Multiple tests then judge (think hard)
> - Suspected rare disease → Specialist consultation, review papers (ultrathink)
>
> It's the same with Claude. Only use ultrathink for complex problems!

### Keyword Levels

| Keyword | Reasoning Depth | When to Use | Token Usage |
|---------|----------------|-------------|-------------|
| (none) | Default | Simple tasks | Low |
| `think` | Normal | General tasks | Normal |
| `think hard` | Deep | Complex problems | High |
| `ultrathink` | Maximum | Architecture decisions | Very high |

### Ultrathink Usage Examples

#### Architecture Design

```
> ultrathink
> How should I design this project structure?
> - Current: Monolithic Next.js app
> - Requirement: Prepare for microservices transition
> - Constraint: Need gradual migration
> Make it easy to add features later.
```

#### Complex Bug Analysis

```
> ultrathink
> Analyze this bug:
> - Symptom: Sometimes payment happens twice
> - Reproduction: When network is slow
> - Related file: @src/payment/checkout.ts
> Find the root cause and solution.
```

#### Performance Optimization

```
> ultrathink
> @src/components/DataGrid.tsx
> It lags when displaying 10,000 rows.
> Review all options like virtualization, memoization, chunk loading
> and suggest the optimal solution.
```

> ⚠️ **Warning**
>
> ultrathink **consumes many tokens**.
> Don't use it for simple tasks.
>
> **When to use:**
> - Overall architecture decisions
> - Complex bug analysis
> - Problems involving multiple systems
>
> **When not needed:**
> - Simple code generation
> - Text modifications
> - Basic bug fixes

---

## Optimal Prompts for Different Situations

### Bug Fixes

```
# Basic
> In @src/login.js, clicking the login button causes an error.
> "Cannot read property 'email' of undefined"
> Fix it.

# Better version
> Bug fix request:
>
> File: @src/login.js
> Symptom: Error when clicking login button
> Error: "Cannot read property 'email' of undefined"
> Reproduction: Click button without entering email
>
> Analyze the cause and fix it.
> Also add defensive code to prevent similar bugs.
```

### New Feature Implementation

```
# Basic
> Make dark mode

# Better version
> Add dark mode feature.
>
> Requirements:
> - Toggle button to switch
> - Detect system setting (prefers-color-scheme)
> - Save selection to localStorage
> - Persist after page refresh
>
> Reference: Add toggle button to @src/components/Header.tsx
> Style: Use Tailwind dark: classes
```

### Code Refactoring

```
# Basic
> Clean up this code

# Better version
> Refactor @src/utils/validation.ts.
>
> Current problems:
> - Functions are too long (over 100 lines)
> - Lots of duplicate code
> - Hard to test
>
> Request:
> 1. Separate functions (each under 20 lines)
> 2. Remove duplication
> 3. Convert to pure functions
> 4. Verify existing behavior is maintained
```

### API Integration

```
# Basic
> Connect the API

# Better version
> Integrate the user API.
>
> Endpoints:
> - GET /api/users - List query
> - GET /api/users/:id - Detail query
> - POST /api/users - Create
> - PUT /api/users/:id - Update
> - DELETE /api/users/:id - Delete
>
> Requirements:
> - Use React Query
> - Error handling (toast notifications)
> - Show loading state
> - Define TypeScript types
>
> Reference @src/types/User.ts for types
```

---

## Prompt Debugging

When results don't come out as expected, here's how to debug your prompts.

### Diagnostic Checklist

If you're not getting desired results, check the following:

| Check Item | Question | Solution |
|-----------|----------|----------|
| Specificity | Is it too vague? | Add filename, line number, specific requirements |
| Context | Did I provide necessary background info? | Reference related files with `@` |
| Scope | Did I ask for too much at once? | Split the task |
| Examples | Did I show the desired format? | Add input/output examples |
| Constraints | Did I specify what not to do? | Add constraints |

### Debugging Examples

**Problem: Claude used an unwanted library**

```
# Original prompt
> Add form validation

# Claude used Yup library (didn't want that)

# Fixed prompt
> Add form validation.
> Use pure JavaScript, no external libraries.
> Implement directly using regex.
```

**Problem: Result doesn't match existing code style**

```
# Original prompt
> Make an Input component

# Claude wrote in a different style

# Fixed prompt
> In the style of @src/components/Button.tsx,
> make an Input component.
> Same pattern, same prop structure.
```

**Problem: Result is too complex**

```
# Original prompt
> Make a user profile page

# Claude created 10 files, complex state management...

# Fixed prompt
> Make a user profile page.
> Keep it simple.
> In 1 file.
> No state management.
> Static UI only.
```

### Iterative Improvement Pattern

```
1st attempt:
> Make a login form
→ Result is too simple

2nd attempt (more specific):
> Make a login form.
> Email, password fields.
> Show error messages.
> Show loading state.
→ Result is better, but style is different

3rd attempt (add reference):
> In the style of @src/components/SignupForm.tsx,
> make a login form.
> Email, password fields.
> Include error messages and loading state.
→ Desired result!
```

> 🔥 **Pro Tip**
>
> **Prompt improvement is an iterative process.**
>
> Don't expect perfect results on the first try.
> Looking at results and improving prompts bit by bit is more efficient.
>
> Usually 2-3 iterations gets you the desired result.

---

## 🎯 Mini Quiz

### Quiz 1
Which is the **best** prompt?

A) "Fix the code"
B) "Fix the bug"
C) "@src/login.js line 42 fix the null error"
D) "Solve the problem"

<details>
<summary>See Answer</summary>

**Answer: C**

It **specifically** states the filename, line number, and error type.
Claude can know exactly what to do.

</details>

### Quiz 2
Which is the **most appropriate** situation to use Plan mode?

A) Fixing a typo
B) Implementing an entire payment system
C) Changing a button color
D) Adding a comment

<details>
<summary>See Answer</summary>

**Answer: B**

Plan mode is used for **large-scale, complex features**.
A payment system is a complex feature requiring multiple steps and decisions.
The rest are simple modifications that don't need Plan mode.

</details>

### Quiz 3
What's wrong with this prompt?

```
> Make login, signup, password reset, profile editing,
> notification settings, and payment integration all at once
```

<details>
<summary>See Answer</summary>

**Problem: Requesting too much at once**

Requesting 6 features at once means:
- Each feature's quality suffers
- Hard to change direction midway
- Hard to identify the cause when errors occur

**Solution: Request one at a time**

</details>

---

## 📝 Practice Exercises

### 🟢 Beginner: Writing Specific Prompts

Convert the following bad prompt into a good prompt.

```
# Bad prompt
> Fix the error

# Convert to a good prompt
> ???
```

Hint: Include filename, error message, and reproduction steps.

### 🟡 Intermediate: Using Plan Mode

```
> /plan
> I want to make a blog system.
> I need post list, post detail, and post creation features.
> How should I proceed?
```

Review Claude's plan and execute if it looks good.

### 🔴 Advanced: Prompt Debugging

Intentionally start with a vague prompt and get the desired result through 3 iterations.

```
# 1st
> Make a form

# 2nd (improve based on result)
> ???

# 3rd (improve based on result)
> ???
```

---

## 🏆 Challenge Tasks

### Challenge 1: Try All 20 Patterns

Use each pattern at least once and select your Top 5 most useful patterns.

### Challenge 2: Create a Prompt Library

Collect frequently used prompts into a personal library.

```markdown
# My Prompt Library

## Bug Fix
> File: @[filename]
> Symptom: [description]
> Error: [error message]
> Reproduction: [reproduction steps]
> Fix it.

## Component Creation
> In the style of @[reference file],
> make [component name].
> [requirement list]

...
```

### Challenge 3: Ultrathink Experiment

Use ultrathink on complex problems and compare result quality with normal mode.

---

## Real Conversation Examples

### Example 1: Step-by-step Improvement

```
# Step 1: Vague request
> Make a button

[Claude creates basic button]

# Step 2: Style improvement
> Make it prettier. Rounded, add hover effect.

[Claude improves style]

# Step 3: Add functionality
> Add loading state too. Show spinner when isLoading is true.

[Claude adds loading functionality]

# Step 4: Add variants
> Add variant prop. Three types: primary, secondary, danger.

[Complete button component!]
```

### Example 2: Large-scale Feature with Plan Mode

```
> /plan
> I want to add a comment system to this blog.

Claude: Comment system implementation plan:

Phase 1: Data Model
- Define Comment type
- Design API endpoints

Phase 2: Backend
- GET /api/comments - Comment list
- POST /api/comments - Create comment
- DELETE /api/comments/:id - Delete comment

Phase 3: Frontend
- CommentList component
- CommentForm component
- useComments hook

Phase 4: Feature Extension
- Reply support
- Edit functionality

What order should we proceed in?

> Good, let's start with Phase 1.

[Claude implements Phase 1]

> Proceed to the next phase.

[Claude implements Phase 2]

...
```

### Example 3: Complex Bug Analysis

```
> ultrathink
> There's a strange bug.
>
> Symptoms:
> - Sometimes adding to cart from product list adds twice
> - Not always, just sometimes
> - Happens more often when network is slow
>
> Related files:
> @src/components/ProductCard.tsx
> @src/hooks/useCart.ts
> @src/api/cart.ts
>
> Find the root cause and suggest a solution.

Claude: [Detailed analysis in progress]

Analysis results:

Root cause:
No duplicate request prevention logic in addToCart function.
When button is double-clicked or network delays,
the same request is sent multiple times.

Impact scope:
- ProductCard's onClick
- useCart's addToCart
- No API-level duplicate prevention

Solution:

Phase 1: Button-level prevention
- Disable button after click
- Show loading state

Phase 2: Hook-level prevention
- Track ongoing requests
- Block duplicate requests

Phase 3: API-level prevention
- Use idempotency key
- Check duplicates on server

Which level should we apply first?
```

---

## 🚨 Common Errors and Solutions

### Error 1: "Claude behaves differently than expected"

**Situation:** Gave a prompt but got completely different results

**Cause:** Prompt is too vague or Claude interpreted it differently

**Solution:**
- Add examples to prompt
- Specify what you don't want
- Confirm with Plan mode first

```
> Make a button. Example:
> <Button variant="primary">Save</Button>
>
> - Don't use external libraries
> - Don't use inline styles
```

### Error 2: "Result is too complex"

**Situation:** Asked for something simple but 10 files were created

**Cause:** No scope limitation in prompt

**Solution:**
```
> Make a button.
> Handle it in this one file, don't create additional files.
> Keep it as simple as possible.
```

### Error 3: "Doesn't match existing style"

**Situation:** New code has different style from project

**Cause:** Didn't provide reference file

**Solution:**
```
> In the style of @src/components/Button.tsx,
> make an Input component.
```

### Error 4: "Direction went off track midway"

**Situation:** As conversation got longer, strayed from original goal

**Cause:** Lost context

**Solution:**
```
> Wait, let me organize.
> Original goal: Make login form
> Done so far: Email field
> Next: Password field and submit button
>
> Let's continue.
```

---

## ❌ Common Mistakes

### 1. Being Too Vague

```
# Bad
> Fix the code

# Good
> At @src/login.js line 42, email validation is failing.
> It passes "test@" without a domain. Fix this.
```

### 2. Asking for Everything at Once

```
# Bad
> Make a complete shopping mall with user auth, payment, inventory,
> admin panel, and analytics

# Good
> Let's start with user registration.
> Just the signup form for now.
```

### 3. Not Providing Examples

```
# Bad
> Format the date nicely

# Good
> Format dates like this: "January 15, 2024"
> Input: "2024-01-15"
> Output: "January 15, 2024"
```

### 4. Not Mentioning Constraints

```
# Bad
> Add form validation

# Good
> Add form validation.
> - Email must have @ and a domain
> - Password must be at least 8 characters
> - Show error messages in red below each field
> - Don't use external libraries
```

### 5. Continuing Without Checking Results

```
# Bad
> Make A
> Make B too
> Make C too
[Continuing without checking...]

# Good
> Make A
[Check result]
> Good! Now make B
[Check result]
> That worked. Make C too
```

---

## 🆘 If It Doesn't Work?

| Symptom | Solution |
|---------|----------|
| Not getting desired results | Make prompt more specific |
| Result is too complex | Add "keep it simple", "in 1 file" |
| Style is different | Reference existing file with `@` |
| Wrong direction | Esc Esc to revert and request again |
| Can't solve complex problem | Use ultrathink |
| Need to confirm before starting | Use /plan |

---

## 📚 Glossary

| Term | Description |
|------|-------------|
| **Prompt** | The request/instruction sent to AI |
| **Prompt Engineering** | The skill of writing effective prompts |
| **Plan Mode** | A mode that creates a plan before execution |
| **Ultrathink** | A keyword that triggers deep reasoning |
| **Token** | The unit AI uses to process text |
| **Context** | The conversation's background information |

---

## ✅ Checklist

Check before finishing your learning:

- [ ] I know the 5 principles of good prompts
- [ ] I can use at least 10 prompting patterns
- [ ] I know when and how to use Plan mode
- [ ] I know when to use Ultrathink
- [ ] I know how to debug prompts
- [ ] I can write optimal prompts for different situations

---

## ➡️ Next Chapter Preview

In the next chapter, you will learn **Exploring Code**.

What you'll learn:
- The importance of code reading
- Glob and Grep search strategies
- Navigating large codebases
- Project analysis techniques

Proceed to [Chapter 09: Exploring Code](../Chapter09-Exploring-Code/README.md).

---

## 📖 Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Usage Guide](https://docs.anthropic.com/en/docs/claude-code) - Official usage documentation
- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering) - Official prompt engineering guide
- [Claude Extended Thinking](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking) - Ultrathink/extended thinking feature

**Video Resources:**
- [Prompt Engineering Basics (YouTube)](https://www.youtube.com/results?search_query=prompt+engineering+tutorial+beginners) - Prompting fundamentals course
- [Claude AI Prompting Tips (YouTube)](https://www.youtube.com/results?search_query=claude+ai+prompting+tips) - Claude prompting tips

**Reading Materials:**
- [Anthropic Prompt Library](https://docs.anthropic.com/en/prompt-library/library) - Prompt examples library
- [Chain of Thought Prompting](https://www.anthropic.com/news/claude-2-1-prompting) - Step-by-step reasoning prompting

**Community:**
- [Anthropic Discord](https://discord.gg/anthropic) - Community questions and discussions
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) - Reddit community

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
