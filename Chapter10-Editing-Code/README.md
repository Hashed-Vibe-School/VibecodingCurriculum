# Chapter 10: Editing Code

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🔗 Connection to Previous Chapter

In Chapter 09, you learned how to **read and explore** code. Now that you understand the code, it's time to **modify** it.

The relationship between code exploration and editing:
```
Exploration (Chapter 09)        Editing (Chapter 10)
+-------------------+           +-------------------+
| Reading code      |    ->     | Modifying code    |
| Understanding     |    ->     | Adding features   |
|   structure       |           |                   |
| Finding patterns  |    ->     | Refactoring       |
| Locating bugs     |    ->     | Fixing bugs       |
+-------------------+           +-------------------+
```

> **Previous chapter review**: You used Glob and Grep to find files and read code to understand it. Now you can make actual modifications based on that understanding.

---

## 🎯 What You Will Learn in This Chapter

- Getting Claude to modify code
- Mastering the Explore -> Plan -> Execute workflow
- Editing safely
- Improving incrementally with feedback
- Using refactoring patterns
- Ensuring quality with TDD style

---

## Why Do You Need This?

Imagine writing an essay. Instead of typing every sentence yourself, you tell an assistant what you want, and they write it for you. You review it, suggest changes, and they revise.

Code editing with Claude works exactly the same way. You describe what you want, Claude writes the code, and you review and refine.

### The Real Value of Code Editing

**Traditional developer vs AI-collaborative developer:**

| Task | Traditional Way | AI Collaboration Way |
|------|----------------|----------------------|
| Bug fix | Analyze code yourself -> fix | "Fix this error" -> review -> approve |
| Add feature | Design -> code -> test | Explain -> confirm plan -> implement -> test |
| Refactoring | Find patterns -> manual edit | "Remove duplicates" -> auto-apply |
| Learning | Read docs -> trial and error | "How about this change?" -> instant feedback |

> 💡 **Beginner Tip**: The difference between writing code yourself and instructing AI is like the difference between cooking yourself and explaining a recipe to a chef. Both produce delicious food, just different methods.

**Real-world scenarios:**
- You want to add a new feature but aren't sure how to implement it
- There's a bug in the code and you need to fix it fast
- You want to refactor messy code into something cleaner
- You're learning and want to see how something should be written
- You need to add new code that matches existing code style

**Think of it like having a pair programmer:**
- Traditional coding: You type everything yourself
- With Claude: You describe what you want, Claude types, you guide and approve

### The Psychology of Code Editing: Overcoming Fear

Many non-developers are afraid of modifying code:

```
"What if I break it?"
    |
    v
Git is there so you can always revert!

"What if I touch parts I don't understand?"
    |
    v
Ask Claude to explain first!

"What if I fix the wrong thing?"
    |
    v
Look at the diff and confirm before approving!
```

> 🔥 **Pro Tip**: The most important thing in code editing is **confidence**. With Git and Claude, you can undo almost any mistake. Don't be afraid to try!

---

## Code Editing Basics

Claude Code can directly modify code. It fixes files without copy-paste.

### Simple Example

```
> @index.html Change the title to "My Website"
```

Claude opens the file, edits it, and saves.

### Reviewing Changes: How to Read Diffs

Before editing, Claude shows what will change:

```diff
- <h1>Hello World</h1>
+ <h1>My Website</h1>
```

- Red (-): Being deleted
- Green (+): Being added

Select `Allow` if okay, `Deny` if not.

### Deep Dive into Reading Diffs

Real diffs can be more complex:

```diff
@@ -15,8 +15,12 @@ function validateEmail(email) {
   if (!email) {
-    return false;
+    return { valid: false, error: "Please enter an email" };
   }

+  if (!email.includes('@')) {
+    return { valid: false, error: "Not a valid email format" };
+  }
+
   const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
-  return emailRegex.test(email);
+  return { valid: emailRegex.test(email), error: null };
 }
```

**How to read diffs:**
1. `@@ -15,8 +15,12 @@`: Starting at line 15, originally 8 lines becomes 12 lines
2. `-` lines: Code being deleted
3. `+` lines: Code being added
4. Lines starting with space: Unchanged lines shown for context

> 💡 **Beginner Tip**: Diffs look confusing at first, but think of `-` as "this goes away" and `+` as "this is new". It becomes familiar after seeing it a few times.

---

## 🎯 Mini Quiz 1: Reading Diffs

Look at the following diff and answer the questions:

```diff
- const greeting = "Hello";
+ const greeting = "Hello there";
+ const farewell = "Goodbye";
```

**Q1: How many lines does this diff delete and add?**

<details>
<summary>See Answer</summary>

**Answer**: 1 line deleted, 2 lines added

- 1 `-` line: `const greeting = "Hello";` deleted
- 2 `+` lines: English greeting and farewell added

</details>

**Q2: What is the value of the `greeting` variable after this change?**

<details>
<summary>See Answer</summary>

**Answer**: "Hello there"

The original "Hello" value is changed to "Hello there".

</details>

---

## Recommended Workflow: Explore -> Plan -> Execute

This is the method recommended by Anthropic. Following this workflow greatly reduces mistakes.

### Overall Workflow Picture

```
+-------------------------------------------------------------+
|                   Code Editing Workflow                      |
+-------------------------------------------------------------+
|                                                             |
|   1. Explore                  "What does this code do?"     |
|         |                                                   |
|         v                                                   |
|   2. Plan                     "How about changing it        |
|         |                      like this?"                  |
|         v                                                   |
|   3. Execute (Code)           "OK, do it that way"          |
|         |                                                   |
|         v                                                   |
|   4. Verify                   "Run the tests"               |
|         |                                                   |
|         v                                                   |
|   5. Iterate                  "Just modify this part more"  |
|                                                             |
+-------------------------------------------------------------+
```

### Step 1: Explore

First, understand the code.

```
> @src/login.js What does this file do?
```

**Good exploration questions:**
```
> @src/auth/ How are the files in this folder connected?
> How does the validateEmail function in @src/utils/validation.js work?
> Where is user data managed in this project?
> What other components does @src/components/LoginForm.jsx use?
```

### Step 2: Plan

Create an editing plan. **This step is the most important.**

```
> I want to add password validation here.
> How should I do it? Don't write code, just plan.
```

Or use Plan mode:
```
> /plan
> How should I add password validation?
```

**Things to confirm in the planning stage:**
1. Which files will be modified
2. In what order to modify them
3. Are there new files to create
4. Which patterns from existing code to follow
5. How to test

### Step 3: Execute (Code)

Edit according to the plan.

```
> OK, edit according to that plan.
```

Or step by step:
```
> Just do step 1 of the plan first.
```

### Step 4: Verify

Check if it works.

```
> Run the tests to verify.
```

```
> Run npm run dev and check if it works.
```

### Step 5: Iterate

Modify and verify again as needed.

```
> Most of it's good, but the error message is too stiff. Make it friendlier.
```

### Why Do It This Way?

When LLMs receive a request, they select the most appropriate-looking pattern from their training. But even "password validation" has multiple implementation approaches (regex, library, custom logic, etc.). Reviewing the plan first:
- Lets you know in advance which direction Claude is heading
- Lets you correct course before any code is written
- Lets you discover unexpected side effects in advance

> ⚠️ **Warning**: Skipping the planning step can result in Claude implementing in an unexpected direction. The more complex the feature, the more important planning becomes.

---

## Real Conversation Examples: Explore -> Plan -> Execute

### Example 1: Adding a Feature

```
User: @src/components/ProductCard.jsx What does this component do?

Claude: This component is a UI component that displays product information in card form.
        - Displays product image, name, and price
        - Clicking navigates to product detail page
        - Currently no 'Add to Cart' button

User: I want to add an Add to Cart button here.
      How should I do it? Don't write code, just plan.

Claude: OK. Here's how to proceed:

        1. Add button to ProductCard.jsx
           - Place "Add to Cart" button below the price

        2. Connect click handler
           - Use existing CartContext's addToCart function
           - Already defined in @src/context/CartContext.jsx

        3. Show loading/success state (optional)
           - Show "Adding..." briefly when button clicked
           - Show "Added!" or check icon when done

        I'll match the style of other buttons in this project
        (@src/components/Button.jsx).

User: Good, but I don't think loading state is needed.
      Just do steps 1 and 2.

Claude: OK, I'll just add the button and click handler.
        [Shows diff]

        - Imported addToCart from CartContext
        - Added button below price
        - Used existing Button component style

User: The button is too big. Make it smaller.

Claude: I'll reduce the button size.
        [Shows diff]

        - Added size="sm" prop to change to small button
```

### Example 2: Bug Fix

```
User: Login isn't working. I enter ID and password and click the button but nothing happens.

Claude: Let me check the login-related code.

        Looking at @src/pages/Login.jsx, I see the login button
        doesn't have an onClick handler connected. The button code looks like:

        <button type="button">Login</button>

        Problems:
        1. No onClick handler
        2. type is "button" instead of "submit"

        How should I fix it?

        Option A: Add onClick={handleLogin} to button
        Option B: Change type to "submit" and connect onSubmit to form

        Other forms in this project use Option B,
        so I recommend B for consistency.

User: Go with Option B.

Claude: [Shows diff]

        Fixed:
        - Changed button type to "submit"
        - Added onSubmit={handleLogin} to form tag
        - Added e.preventDefault() to handleLogin (prevents page refresh)

User: Let me test it. Run npm run dev.

Claude: Running npm run dev. Check at http://localhost:3000.
        Test if the login button works!
```

### Example 3: Refactoring

```
User: @src/utils/helpers.js is too long. Clean it up a bit.

Claude: Let me analyze @src/utils/helpers.js.

        Current state:
        - Total 450 lines
        - 25 functions mixed together
        - String related: formatDate, formatCurrency, truncateText etc. 8 total
        - Validation related: validateEmail, validatePhone etc. 7 total
        - Array related: groupBy, sortBy etc. 6 total
        - Other: 4

        I suggest this separation plan:

        @src/utils/
        ├── formatters.js    (string formatting functions)
        ├── validators.js    (validation functions)
        ├── arrayUtils.js    (array utilities)
        └── helpers.js       (other)

        I'll also re-export everything from helpers.js so that
        existing code importing @src/utils/helpers.js
        won't need to be modified.

        Proceed with this plan?

User: Good. But don't do everything at once,
      separate just formatters.js first.

Claude: OK, I'll just separate formatters.js first.

        [New file created: src/utils/formatters.js]
        [Removed those functions from helpers.js and added import]

        Should I run the tests?

User: Yeah, run the tests.

Claude: Running npm test.

        All tests passed! (47 passed)

        Should I separate validators.js next?
```

---

## Improving with Feedback

The first result doesn't need to be perfect. Provide feedback to improve.

### Types of Feedback

| Feedback Type | Example | Claude's Response |
|--------------|---------|-------------------|
| Style change | "Make the button blue" | Only modifies CSS/style code |
| Feature change | "Also show an alert on click" | Adds feature logic |
| Structure change | "Make this a separate component" | File separation and refactoring |
| Delete request | "Don't need this feature" | Removes that code |
| Undo | "Cancel that thing from earlier" | Restores previous state |

### Example

```
> Make a login form
```

If the result doesn't suit you:

```
> The error messages are too stiff. Make them friendlier.
```

```
> The button is too small. Make it bigger.
```

```
> Add an eye icon when entering password so they can see it.
```

2-3 rounds of feedback makes it much better.

### How to Give Effective Feedback

**Vague feedback (ineffective)**
```
> This is bad
> Make it better
> Something's weird
```

**Specific feedback (effective)**
```
> The button color doesn't match the background well. Change it to a darker blue.
> There's no loading indicator after form submission so users might click multiple times.
> The error message is in English. Change it to Korean.
```

> 💡 **Beginner Tip**: When giving feedback, saying both "what the problem is" and "how you'd like it fixed" works well. "This is bad" is less effective than "The color is bad. Change it to a brighter color."

---

## Undoing

You can undo if something went wrong. Remember that you can always undo mistakes!

### Undo Options

| Method | Shortcut/Command | When to Use |
|--------|------------------|-------------|
| Stop operation | `Esc` | While Claude is working |
| Undo last action | `Esc Esc` | Cancel just-made change |
| Cancel completely | `Ctrl + C` | Cancel everything and start fresh |
| Git undo | "Revert to last commit" | Cancel multiple changes at once |
| Go to specific point | "Revert to pre-refactoring backup" | Move to specific commit |

### Example

If Claude is going in a wrong direction:

```
# Press Esc Esc then
> Oh, not that. Let me explain again.
> Keep the existing login logic, just add OAuth.
```

If multiple changes got tangled:

```
> Wait, undo everything from earlier.
> Let's go back to the last commit state.
```

### Undo Guide by Scenario

**Scenario 1: When Claude modified the wrong file**
```
> Wait, not Login.jsx, I need Register.jsx modified.
> Undo what you just did to Login.jsx and modify Register.jsx.
```

**Scenario 2: When the modification broke something else**
```
> After this modification, the cart page doesn't work.
> Check what went wrong and redo it without touching the cart.
```

**Scenario 3: When the whole direction is wrong**
```
> No, I don't think this approach is right.
> Undo everything and let's try a different way.
```

---

## Good vs Bad Edit Requests

### Edit Request Comparison Table

| Bad Request | Why It's Bad | Good Request |
|-------------|-------------|--------------|
| "Fix the bug" | Don't know which bug | "The login button won't click. Fix it" |
| "Make it better" | Don't know what's lacking | "Improve code readability" |
| "Edit this" | Don't know what or how | "Change the title color to red" |
| "Getting an error" | Don't know which error | "Getting TypeError: undefined error" |

### Bad Example

```
> Fix the bug
```
Don't know what to fix.

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

### Good Request Examples by Situation

**Bug fix request:**
```
> In @src/cart/CartItem.jsx, if I change quantity to 0,
> it shows negative price. Limit minimum quantity to 1.
```

**Feature addition request:**
```
> Add search autocomplete to @src/components/SearchBar.jsx.
> Pull names from the products data in this project.
```

**Style modification request:**
```
> Change the hero section background in @src/pages/Home.jsx
> to the project's primary color (#3B82F6).
```

**Refactoring request:**
```
> @src/hooks/useAuth.js is too long.
> Separate token-related logic to useToken.js.
> Make it so places using useAuth don't need modification.
```

---

## TDD Style: Tests First

Writing tests first tells Claude exactly what to build.

### Benefits of TDD

```
Traditional way:
Write code -> Write tests -> Find bugs -> Fix code -> Repeat

TDD way:
Write tests -> Confirm tests fail -> Write code -> Tests pass -> Done!
```

When tests come first:
1. Requirements become clear
2. Claude knows exactly what to build
3. Completion criteria are clear (tests pass = done)

### Example

```
# Step 1: Tests first
> Write tests for formatDate function.
> "2024-01-15" should return "January 15, 2024".
> Empty string should throw an error.

# Step 2: Run tests (confirm failure)
> Run the tests.

# Step 3: Implement
> Now make the function pass the tests.

# Step 4: Verify again
> Run the tests again.
```

### Detailed TDD Example

```
User: Write tests for formatCurrency function first.
      - 1000 -> "$1,000"
      - 1500000 -> "$1,500,000"
      - 0 -> "$0"
      - Negative -> error
      - String -> error

Claude: I've written the tests.

        @src/utils/__tests__/formatCurrency.test.js
        [Shows test code]

User: Run the tests.

Claude: Test results:
        x formatCurrency is not defined

        All 5 tests failed.
        Because the function doesn't exist yet.

User: Now create the function.

Claude: I've created the formatCurrency function.

        @src/utils/formatCurrency.js
        [Shows function code]

        Should I run the tests?

User: Yes.

Claude: Test results:
        ✓ Converts 1000 to "$1,000"
        ✓ Converts 1500000 to "$1,500,000"
        ✓ Converts 0 to "$0"
        ✓ Error on negative
        ✓ Error on string

        All 5 tests passed!
```

> 🔥 **Pro Tip**: TDD seems cumbersome at first, but it saves tremendous time when building complex features. With tests in place, you can refactor confidently.

---

## Refactoring Patterns

Refactoring is improving code structure while keeping behavior the same.

### Major Refactoring Patterns

| Pattern | When to Use | Example Request |
|---------|-------------|-----------------|
| Extract Function | Code is too long | "Separate this part into a function" |
| Rename | Name is unclear | "Rename this variable" |
| Move File | Improving structure | "Move this file to a different folder" |
| Remove Duplicates | Same code repeating | "Remove duplicate code" |
| Add Types | Increasing stability | "Add TypeScript types" |

### Extract Function

```
> Extract the validation logic from lines 50-70 in @src/App.js
> into a validateForm function.
```

**More detailed:**
```
> The handleSubmit function in @src/pages/Checkout.jsx is too long.
> Separate the address validation logic (lines 30-50) into validateAddress function.
> Keep it in the same file, we can move it later if needed.
```

### Rename

```
> Rename getUserData function to fetchUserProfile.
> In all files.
```

**Caution when renaming:**
```
> Rename the data variable to user.
> But don't touch other data variables in @src/api/.
```

### Move File

```
> Move @src/utils/helpers.js
> to @src/lib/helpers.js.
> Update all import statements too.
```

### Remove Duplicate Code

```
> @src/pages/Login.jsx and @src/pages/Register.jsx
> have similar form validation code. Make a common function.
```

### Before and After Refactoring Example

**Before refactoring:**
```javascript
// Same code repeated in multiple places
function handleLogin() {
  if (!email) {
    setError("Please enter email");
    return;
  }
  if (!email.includes("@")) {
    setError("Invalid email format");
    return;
  }
  // ... login logic
}

function handleRegister() {
  if (!email) {
    setError("Please enter email");
    return;
  }
  if (!email.includes("@")) {
    setError("Invalid email format");
    return;
  }
  // ... registration logic
}
```

**After refactoring:**
```javascript
// Extracted to common function
function validateEmail(email) {
  if (!email) return "Please enter email";
  if (!email.includes("@")) return "Invalid email format";
  return null;
}

function handleLogin() {
  const emailError = validateEmail(email);
  if (emailError) {
    setError(emailError);
    return;
  }
  // ... login logic
}

function handleRegister() {
  const emailError = validateEmail(email);
  if (emailError) {
    setError(emailError);
    return;
  }
  // ... registration logic
}
```

---

## 🎯 Mini Quiz 2: Refactoring Patterns

What refactoring pattern should you use in these situations?

**Q1: A 100-line function does 3 different things.**

<details>
<summary>See Answer</summary>

**Answer**: Extract Function

Separate the 100-line function into 3 smaller functions. Each function handles one task.

```
> Separate this function into 3 smaller functions.
> Each function should do one thing.
```

</details>

**Q2: There's a variable named `d`, and you don't know what it means.**

<details>
<summary>See Answer</summary>

**Answer**: Rename

Rename the variable to something meaningful.

```
> What does the d variable mean?
> Figure out what it means and rename it appropriately.
```

</details>

**Q3: The `utils/helpers.js` file has 100 functions and is hard to manage.**

<details>
<summary>See Answer</summary>

**Answer**: Move File (File separation)

Separate files by function.

```
> Separate helpers.js into multiple files by function.
> e.g.: validators.js, formatters.js, dateUtils.js etc.
> Keep existing imports working.
```

</details>

---

## Editing Safely

### Using Git - The Most Important Safety Net

Commit before major edits.

```
> Commit current state. Backup before refactoring.
```

If problems occur:

```
> Something went wrong. Revert to last commit.
```

### Git Best Practices

```
Before work:
> Show git status

Backup before starting:
> Commit current state. "Backup before refactoring"

During work:
> Commit up to here. "Function extraction complete"

If problems occur:
> Revert to last commit

Or to specific commit:
> Revert to "Backup before refactoring" commit
```

> ⚠️ **Warning**: Don't do major refactoring without Git! There's no way to recover if something goes wrong.

### Using Permission Modes

| Mode | When to Use | Description |
|------|-------------|-------------|
| **Plan mode** | When exploring/planning | No code edits, analysis only |
| **Normal mode** | When learning | Confirm before each edit |
| **Accept Edits** | When comfortable | Auto-approve edits |

**Using Plan mode:**
```
> /plan
> Explain this project structure.
> What files need to be modified to add login?
```

In Plan mode, there's no risk of accidentally modifying code.

### Safe Editing Checklist

- [ ] Committed current state with Git?
- [ ] Reviewed plan first?
- [ ] Read and understood the diff?
- [ ] If there are tests, did they pass?
- [ ] Checked if it affects other places?

---

## 📝 Practice Exercises

### 🟢 Beginner: Basic Editing

**Goal**: Learn basic code editing.

```
# Create file
> Create hello.html. A page that shows "Hello World".

# Edit
> Change the title to "Hello".

# Add
> Add a button. Show alert on click.

# Add style
> Make the button pretty. Rounded corners with blue background.
```

**Check points:**
- Can you read and understand diffs?
- Can you request modifications through feedback?

### 🟡 Intermediate: Explore -> Plan -> Execute

**Goal**: Learn systematic editing workflow.

```
# Explore
> @package.json What does this project do?

# Plan
> I want to add a user profile feature. Make a plan.

# Review plan
> Step 2 looks too complex. Is there a simpler way?

# Execute
> OK, start with step 1 of the revised plan.

# Test
> Check if it works.

# Feedback
> Also add profile picture upload.
```

### 🔴 Advanced: Refactoring Project

**Goal**: Learn editing skills while refactoring real code.

```
# Create sample code
> Create legacy.js with these conditions:
> - About 150 lines of complex code
> - Has duplicate code
> - Unclear function names
> - No comments
> - Legacy syntax using var

# Analyze
> @legacy.js Analyze this code. Make a list of improvements.

# Plan
> Create improvement order and plan.

# Step-by-step refactoring
> Step 1: Separate duplicate code into functions
> [Test]
> Step 2: Rename functions to be meaningful
> [Test]
> Step 3: Change var to let/const
> [Test]
> Step 4: Add comments where needed
```

---

## 🏆 Challenge Tasks

### Challenge 1: Open Source Refactoring

Try refactoring a real open source project:

```bash
git clone https://github.com/sindresorhus/ora.git
cd ora
claude
```

```
> Explain this project structure
> Find parts that can be improved
> Pick one and create a refactoring plan
> [After reviewing plan] Go ahead
```

### Challenge 2: Implement Feature with TDD

Build a new feature with test-driven development:

```
> Write tests first for these requirements:
>
> formatRelativeTime function
> - "just now" (under 1 minute)
> - "N minutes ago" (under 1 hour)
> - "N hours ago" (under 24 hours)
> - "N days ago" (under 7 days)
> - "N weeks ago" (under 4 weeks)
> - "N months ago" (under 12 months)
> - "N years ago"

> Run tests (confirm failure)

> Now make the function pass the tests

> Run tests again

> Add edge case tests:
> - Future time input
> - Invalid format input
```

### Challenge 3: Legacy Code Modernization

Convert old JavaScript to modern syntax:

```
> Modernize this code to ES6+ syntax:
> - var -> let/const
> - function -> arrow function (where appropriate)
> - Promise -> async/await
> - String concatenation -> template literals

> Convert to TypeScript. Add proper types.

> Add unit tests. For each function.
```

---

## 🚨 Error Scenarios and Solutions

### Scenario 1: Claude made unwanted changes

**Situation**: Tried to change login button style but it changed the whole page layout.

```
User: Change the login button color.
Claude: [Button color change + whole layout change]
```

**Solution:**
```
> Wait, you changed too much.
> Just change the button color and revert the layout.
```

Or:
```
# Press Esc Esc to undo
> ONLY change the button color. Don't touch anything else.
```

### Scenario 2: Modification broke something else

**Situation**: Modified the login form, but the signup form uses the same component and broke.

```
User: npm run dev
Claude: Error! Button component props error in Register.jsx
```

**Solution:**
```
> Oh, Register.jsx uses the same Button component.
> Check Register.jsx and fix it together.
```

### Scenario 3: Claude is editing the wrong file

**Situation**: Asked to edit Login.jsx but Claude is editing LoginForm.jsx.

```
User: Edit the login page
Claude: I'll modify @src/components/LoginForm.jsx...
```

**Solution:**
```
> No, not LoginForm.jsx, edit @src/pages/Login.jsx.
> Let's use the exact file path.
```

### Scenario 4: Changes weren't saved

**Situation**: Claude says it edited but opening the file shows no changes.

**Solution:**
```
> Was the file saved? Show me the contents of @src/login.js.
```

You might have selected "Deny" when Claude asked for permission.
```
> Edit and save it again. I'll Allow this time.
```

### Scenario 5: Claude keeps using unwanted patterns

**Situation**: Told not to use inline styles but keeps using them.

**Solution:**

Add to CLAUDE.md:
```markdown
## Coding Rules
- Never use inline styles
- Use CSS modules or styled-components
- Always ask before adding new dependencies
```

Or specify each time:
```
> Add styles using CSS modules. Don't use inline styles.
```

---

## ❌ Common Mistakes

### Mistake 1: Not Reviewing Changes

```
# Bad - Accepting everything without looking
> Make this better
[Claude changes 20 lines]
[Click Allow without reading]

# Good - Review before accepting
> Make this better
[Claude shows diff]
[Read diff carefully]
[Notice something wrong]
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
> Rewrite this entire file in TypeScript

# Good
> First, just add TypeScript types to the function parameters
> [Review]
> Now add types to the return values
> [Review]
> Now add an interface for the user object
```

> 💡 **Beginner Tip**: "Small and often" is key. When you try to make big changes at once, it's hard to find what's wrong when problems occur. Making small changes multiple times lets you verify at each step, which is safer.

---

## 🎯 Mini Quiz 3: Comprehensive

**Q1: What should you do first before adding a new feature?**

A) Start writing code immediately
B) Create and review a plan
C) Write tests
D) Git commit

<details>
<summary>See Answer</summary>

**Answer**: Both B and D!

Order:
1. D) Git commit - Backup current state
2. B) Create and review plan - Confirm direction
3. Then implement

C) Writing tests is good when doing TDD, but not required.
A) Writing code immediately is the pattern to avoid most.

</details>

**Q2: What should you do when Claude is modifying in an unexpected direction?**

<details>
<summary>See Answer</summary>

**Answer**:
1. Press `Esc` to stop the operation
2. Explain what you want more clearly
3. Use `Esc Esc` to undo if needed

```
> Wait, stop.
> What I wanted was [clear explanation].
> Do it again.
```

</details>

**Q3: Why is the request "Separate duplicate code into a function" effective?**

<details>
<summary>See Answer</summary>

**Answer**:
- What to do is clear (find duplicate code)
- How to do it is clear (separate into function)
- Result is clear (deduplicated code)

To make it even better:
```
> Separate the duplicate email validation code
> in @src/pages/Login.jsx and @src/pages/Register.jsx
> into validateEmail function in @src/utils/validation.js.
```

</details>

---

## 📚 Glossary

| Term | Description |
|------|-------------|
| **diff** | Format showing file changes. `-` is deletion, `+` is addition |
| **Refactoring** | Improving code structure while keeping behavior the same |
| **TDD** | Test-Driven Development. Writing tests first, then code |
| **Extract Function** | Refactoring that separates part of long code into a separate function |
| **Plan mode** | Mode that only analyzes without modifying code |
| **Commit** | Saving current state in Git |
| **Undo** | Restoring to previous state |

---

## ✅ Summary

What you learned in this chapter:
- [x] Getting Claude to modify code
- [x] Reading and understanding diffs
- [x] Explore -> Plan -> Execute workflow
- [x] Improving incrementally with feedback
- [x] Undoing and using Git
- [x] Refactoring patterns
- [x] TDD style development
- [x] Editing safely

### Key Points Summary

1. **Plan first**: Don't code right away, plan first
2. **Check diff**: Always review changes before approving
3. **Use Git**: Always commit before major edits
4. **Small and often**: Don't do too much at once, do a little at a time
5. **Specific requests**: Clear requests instead of vague ones

---

## ➡️ Next Chapter Preview

In [Chapter 11: Git Basics](../Chapter11-Git-Basics/README.md):

- Understanding Git basic concepts
- Version control with Claude
- Using branches
- Collaboration workflows

Now that you've learned code editing, you'll learn how to safely manage the code you've edited!

```
This chapter: Editing Code        ->     Next chapter: Git Basics
+---------------------+                  +---------------------+
| Modifying code      |       ->         | Managing history    |
| Refactoring         |       ->         | Version control     |
| Undoing             |       ->         | Branches            |
+---------------------+                  +---------------------+
```

---

## 📖 Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code) - Official documentation
- [Anthropic's Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering) - Best practices guide

**Video Resources:**
- [Code Refactoring Best Practices (YouTube)](https://www.youtube.com/results?search_query=code+refactoring+best+practices) - Refactoring techniques
- [TDD Tutorial (YouTube)](https://www.youtube.com/results?search_query=test+driven+development+tutorial) - Test-Driven Development guide

**Reading Materials:**
- [Martin Fowler - Refactoring](https://refactoring.com/) - Refactoring patterns reference
- [Clean Code Principles](https://www.google.com/search?q=clean+code+principles) - Code quality principles

**Related Tools:**
- [Git Documentation](https://git-scm.com/doc) - Version control reference
- [Jest](https://jestjs.io/) - JavaScript testing framework
- [Vitest](https://vitest.dev/) - Modern testing framework

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
