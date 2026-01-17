# Chapter 07: Context and Memory

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Deeply understand the **concept and mechanics** of context
- Learn **best practices** for CLAUDE.md files
- Learn to save to memory with the `#` prefix
- Understand **memory limitations** and effective **workarounds**
- Master effective **conversation management strategies**

---

## ⏱️ Estimated Time

- Reading: **30 minutes**
- Practice: **30 minutes**

---

## 📋 Prerequisites

- Claude Code installed and logged in
- Understanding of project structure (Chapter 06)

---

## 🔗 Connection to Previous Chapter

In Chapter 06, we learned about project structure. Now let's learn how to make Claude **remember** that structure and **understand** our project rules. This is the essence of "context".

---

## Why Does Context Matter?

When you chat with Claude, it remembers the conversation so far. But when you start a new conversation, it forgets everything. If you had to re-explain your project every time, that would be inefficient.

**Real-world situations:**
- You explained project rules to Claude yesterday, but have to explain them again today
- Claude writes code that doesn't match your project style
- There are rules this project should always follow, but you have to mention them every time
- When conversations get long, Claude forgets what you said earlier

### Easy Analogy: An Operations Manual for Your Assistant

Explaining everything from scratch every time a new assistant comes is inefficient. Instead, if you create an operations manual, even when assistants change, they just need to read the manual.

`CLAUDE.md` is exactly this manual. Claude automatically reads it every time it opens your project.

> 💡 **Beginner Tip**
>
> Context is the **background knowledge of a conversation**.
> You can say "let's go to that cafe" to a friend because
> they already know which cafe you mean.
>
> It's the same with Claude. With context, it can give more accurate answers.

---

## How AI Context Works

### How LLMs "Remember"

LLMs (Large Language Models) don't actually "remember". They re-read the entire conversation content and respond each time.

```
[System Prompt]
[CLAUDE.md Content]        ← Automatically loaded
[Previous conversation...] ← All conversations accumulate
[Current question]
```

**Important points:**
- All conversations stack in a "context window"
- This space has a **limit** (token limit)
- When space fills up, older content gets forgotten first

> 🔥 **Pro Tip**
>
> Claude's context window is approximately **200,000 tokens**.
> In English, that's roughly **150,000-400,000 characters**.
>
> It seems like a lot, but as conversations get longer or you reference large files, it fills up quickly.

### What Are Tokens?

Tokens are the units AI uses to process text.

#### Easy Analogy: Lego Blocks

```
┌─────────────────────────────────────────────────────────────────┐
│                    Tokens = Lego Blocks                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Sentence: "Hello, nice to meet you!"                           │
│                                                                 │
│  How AI sees it:                                                │
│  ┌─────┐ ┌────┐ ┌────┐ ┌────┐ ┌─────┐                          │
│  │Hello│ │,   │ │nice│ │to  │ │meet │ ... ← About 6 tokens     │
│  └─────┘ └────┘ └────┘ └────┘ └─────┘                          │
│                                                                 │
│  Just like building a house with Lego has a block limit,        │
│  AI also has a limit on how many tokens it can process at once. │
│                                                                 │
│  More tokens = Higher cost + Longer processing time             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Beginner Tip**
>
> Think of tokens as **"AI's reading units"**.
>
> Just like we read text word by word,
> AI processes text token by token.
>
> Different languages use different amounts of tokens.
> "Hello" is 1 token, but longer words may be 2-3 tokens.

| Example | Approximate Tokens |
|---------|-------------------|
| "Hello" | 1 token |
| One sentence (10 words) | 10-15 tokens |
| One paragraph (50 words) | 50-75 tokens |
| 1,000 characters of code | 200-400 tokens |
| Average file | 500-2,000 tokens |

**Why Does Token Count Matter?**

1. **Cost**: More tokens mean higher costs
2. **Speed**: More tokens mean slower responses
3. **Limits**: Exceeding the context window (~200k tokens) causes older content to be forgotten

### Types of Context

| Type | Description | Example | Persistence |
|------|-------------|---------|-------------|
| **Conversation Context** | Content from current conversation | "the button I made earlier..." | Disappears when conversation ends |
| **File Context** | File content referenced with `@` | "@src/app.js" | Only in that conversation |
| **Project Context** | CLAUDE.md file | Project rules | Auto-loads every time |
| **Memory Context** | Content saved with `#` | "# Use TypeScript" | Permanently saved |

---

## CLAUDE.md File Deep Guide

`CLAUDE.md` is a special file that Claude automatically reads when opening a project.

### Location and Priority

```
my-project/
├── CLAUDE.md         ← Project-specific (highest priority)
├── src/
├── package.json
└── ...

~/.claude/
└── CLAUDE.md         ← Global settings (applies to all projects)
```

**Priority**: Project CLAUDE.md > Global CLAUDE.md

### CLAUDE.md Best Practices

#### 1. Basic Structure

```markdown
# Project Guide

## Project Introduction
This project is a todo management web app.
Users can add, edit, and delete todos.

## Tech Stack
- React 18
- TypeScript 5.0
- Tailwind CSS 3.4
- Next.js 14 (App Router)

## Coding Rules
- Use only functional components
- Korean comments required
- Variable names in camelCase
- Component names in PascalCase
- One component per file

## Folder Structure
- src/components: UI components
- src/app: Page routing
- src/hooks: Custom hooks
- src/lib: Utility functions

## Prohibited
- No class components
- No any type
- No leaving console.log (delete after debugging)
- No inline styles

## Preferred Patterns
- Use try-catch for error handling
- Manage API calls in src/lib/api.ts
- Use React Query for state management
```

> 💡 **Beginner Tip**
>
> Think of CLAUDE.md as a **letter to Claude**.
> It's a request saying "please work on this project like this".
>
> Start simple and add more as needed.

#### 2. Writing Effective Rules

**Vague rules (bad example):**
```markdown
- Write code cleanly
- Use good variable names
- Handle errors well
```

**Specific rules (good example):**
```markdown
- Keep functions under 20 lines
- Use descriptive names for variables (e.g., userEmail, isLoading)
- Wrap all API calls in try-catch and show toast notification on error
```

> ⚠️ **Warning**
>
> **Too many rules can backfire!**
>
> - 100 rules → Claude gets confused
> - 10-15 rules → Clear and focused
>
> **Priority**: Place the most important rules at the top.

#### 3. Situational CLAUDE.md Examples

**For personal projects (simple)**
```markdown
# My Blog

## Tech
- Next.js 14, TypeScript, Tailwind

## Rules
- Korean comments
- Functional components
- File names in kebab-case

## Notes
- Keep design minimal
- Need dark mode support
```

**For team projects (detailed)**
```markdown
# E-Commerce Platform

## Project Overview
B2C shopping platform. Provides product search, cart, and payment features.

## Team Conventions
- Branch naming: feature/feature-name, fix/bug-name
- Commit messages: [TYPE] description (e.g., [FEAT] Add cart)
- Must use PR template

## Architecture
- Frontend: Next.js 14 App Router
- Backend: API Routes + Prisma
- Database: PostgreSQL
- Auth: NextAuth.js

## Folder Structure
src/
├── app/           # Page routing
├── components/    # UI components
│   ├── common/    # Common (Button, Modal, etc.)
│   └── features/  # Feature-specific (Cart, Product, etc.)
├── hooks/         # Custom hooks
├── lib/           # Utilities
├── types/         # TypeScript types
└── styles/        # Global styles

## Coding Rules
- Use TypeScript strict mode
- Use export default for components
- Prefix hooks with use (useCart, useAuth)
- Start API functions with verbs (fetchProducts, createOrder)

## Prohibited
- No any type
- No hardcoded strings (use constants file)
- No direct DOM manipulation

## Testing
- Tests required for important business logic
- Test files in __tests__ folder
- Naming: filename.test.ts

## Reference Documents
- API docs: /docs/api.md
- Design system: Figma link
```

---

## Saving to Memory with # Prefix

Using the `#` prefix saves information to Claude's memory.

### Usage

```
> # This project uses TypeScript
```

```
> # Always respond in English
```

```
> # Always add comments to code
```

### Where It's Saved and How to Check

Content saved with `#` is automatically added to the `CLAUDE.md` file.

```
> # Always style button components with Tailwind

Saved to CLAUDE.md:
"Always style button components with Tailwind"
```

To verify:
```
> @CLAUDE.md Show me the contents
```

### Effective Memory Usage Patterns

**Adding rules on the fly**
```
# Save a rule you realized while working
> # In this project, dates are always in YYYY-MM-DD format
```

**Saving project preferences**
```
> # Use Lucide React for icons
> # Use Zod for form validation
> # Use Zustand for state management
```

**Saving work context**
```
> # Currently developing cart feature
> # Next task: Payment integration
```

> 🔥 **Pro Tip**
>
> Use `#` syntax when you need **immediate saving**.
> For unchanging rules like project settings, it's better to write them directly in CLAUDE.md.
>
> **When to use which:**
> - Quick notes → `# prefix`
> - Systematic rules → Edit CLAUDE.md directly

---

## Memory Limitations and Workarounds

### When Context Window Gets Full

As conversations get longer, older content gets cut off. You need to be aware of this and handle it.

**Symptoms:**
- Claude doesn't know what you said earlier
- Keeps making the same mistakes
- Feels like it's asking "what were we talking about?"

**Solutions:**

#### 1. Re-mention Key Content

```
> What we're building is a todo list app,
> using React + TypeScript.
> We're currently adding a delete feature.
> Add a confirmation modal to the deleteTodo function we made earlier.
```

#### 2. Start a New Conversation

For completely new work:
```
> /clear
```

This clears previous conversation content and starts fresh.

#### 3. Request a Summary

```
> Summarize what we were doing earlier
```

This lets you check what Claude remembers.

### Efficient Context Usage

| Situation | Recommended Method |
|-----------|-------------------|
| Short tasks | Complete in one conversation |
| Long tasks | Break into stages |
| Multiple feature development | Start new conversation for each feature |
| Bug fixes | Reference only related files with `@` |

> 💡 **Beginner Tip**
>
> **Think of conversations as "work units".**
>
> - Creating login feature → One conversation
> - Creating signup feature → New conversation
> - Design modifications → New conversation
>
> This way each conversation stays focused, reducing chances of losing context.

---

## Effective Conversation Management Strategies

### 1. When Starting a New Conversation

CLAUDE.md loads automatically when you enter a project.
If you need additional context, provide it early in the conversation:

```
> I'm currently working on the login feature.
> @src/auth/login.js I'm modifying this file.
> Last time I made the basic form, today I'm adding validation logic.
```

### 2. When Conversations Get Long

Claude may forget earlier content as conversations lengthen.

**Method 1: Request Summary**
```
> Summarize the requirements we discussed earlier
```

**Method 2: Reconfirm Key Points**
```
> What we're building is a todo list app,
> and we're currently adding delete functionality.
> Add the confirmation modal before deletion.
```

**Method 3: Mid-point Summary**
```
> Summarize what we've done so far:
> 1. What files we created
> 2. What features we implemented
> 3. What we need to do next
```

### 3. When Switching Tasks

When moving to a completely different task:

```
> /clear
```

Or explicitly change context:
```
> Okay, login is done.
> Now let's make the dashboard page.
> I'll explain from the beginning...
```

### 4. Conversation Session Management Pattern

```
┌─────────────────────────────────────────────────────────┐
│                      Starting Work                        │
│  "Today's task: Create user profile page"                 │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  Step 1: Planning                                         │
│  > What features does the profile page need?              │
│                                                           │
│  Step 2: Basic Structure                                  │
│  > Create the profile page component                      │
│                                                           │
│  Step 3: Detailed Features                                │
│  > Add profile photo upload feature                       │
│  > Add info edit form                                     │
│                                                           │
│  Step 4: Wrap Up                                          │
│  > Test what we've made so far                            │
│  > Clean up code and write commit message                 │
│                                                           │
├─────────────────────────────────────────────────────────┤
│                      Work Complete                        │
│  /clear or start new conversation                         │
└─────────────────────────────────────────────────────────┘
```

---

## Real Conversation Examples

### Example 1: Seeing CLAUDE.md Effect

**Before setting CLAUDE.md:**
```
> Make a button

// Button.js
class Button extends React.Component {
  render() {
    return <button style={{padding: '10px'}}>{this.props.text}</button>
  }
}
```
(Class component + inline styles used - unwanted patterns)

**After setting CLAUDE.md:**
```markdown
# CLAUDE.md
- Use only functional components
- Style with Tailwind CSS
- Use TypeScript
```

```
> Make a button

// Button.tsx
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
}

// Button component - handles click events
export function Button({ children, onClick }: ButtonProps) {
  return (
    <button
      onClick={onClick}
      className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
    >
      {children}
    </button>
  );
}
```
(Functional + TypeScript + Tailwind - exactly as configured!)

### Example 2: Using Memory Saving

```
> # The API endpoint base URL is /api/v2

Saved to CLAUDE.md:
"The API endpoint base URL is /api/v2"
```

Later:
```
> Make a function to fetch user list

// fetchUsers function - retrieves user list
export async function fetchUsers() {
  const response = await fetch('/api/v2/users');  // Uses saved base URL
  return response.json();
}
```

### Example 3: Working While Maintaining Context

```
> I'm making a todo list app.
> Let's start with the add todo feature.

[Claude writes addTodo function]
```

```
> Great! Now add delete functionality.

[Claude writes deleteTodo in consistent style with addTodo]
```

```
> Add complete toggle feature too.

[Claude writes toggleTodo following same pattern]
```

Context is maintained, resulting in consistent code!

### Example 4: When Context Is Lost

```
> Add error handling to the function we made earlier

I'm sorry, I need to confirm which function you're referring to.
We've worked on several functions in this conversation:
1. fetchUsers
2. createUser
3. deleteUser
Which function should I add error handling to?
```

**Solution:**
```
> Add error handling to the fetchUsers function.
> Wrap in try-catch, log to console on error, and return empty array.

// Modified fetchUsers
export async function fetchUsers() {
  try {
    const response = await fetch('/api/v2/users');
    if (!response.ok) throw new Error('Failed to fetch');
    return response.json();
  } catch (error) {
    console.error('Failed to fetch user list:', error);
    return [];
  }
}
```

---

## 🎯 Mini Quiz

### Quiz 1
Which statement about context windows is **incorrect**?

A) It's the space where conversation content accumulates
B) It has unlimited capacity
C) When full, older content is forgotten first
D) CLAUDE.md is also included in context

<details>
<summary>See Answer</summary>

**Answer: B**

The context window has **limited capacity**.
For Claude, it's approximately 200,000 tokens.

</details>

### Quiz 2
Which is the **correct** location for the CLAUDE.md file?

A) src/CLAUDE.md
B) CLAUDE.md in project root
C) node_modules/CLAUDE.md
D) .vscode/CLAUDE.md

<details>
<summary>See Answer</summary>

**Answer: B**

CLAUDE.md must be in the **project root** (top-level folder).
If it's inside src/, it won't be automatically read.

</details>

### Quiz 3
Which is the **correct** usage of the `#` prefix?

A) `#remember this` (no space)
B) `# remember this` (with space)
C) `## remember this` (two #)
D) `remember: remember this`

<details>
<summary>See Answer</summary>

**Answer: B**

You need a **space** after `#`.
`# remember this` (O)
`#remember this` (X)

</details>

---

## 📝 Practice Exercises

### 🟢 Beginner: Create CLAUDE.md

```
> Create a CLAUDE.md file.
> This project is a React web app,
> uses English comments,
> and styles with Tailwind CSS.
```

**Expected Result:** CLAUDE.md file created

### 🟡 Intermediate: Memory Saving and Testing

```
# Step 1: Save rules
> # Add English explanations as comments for variable names
> # Write functions as arrow functions

# Step 2: Verify saved
> @CLAUDE.md Show me the contents

# Step 3: Test rules
> Make an email validation function
```

**Expected Result:** Function is written following saved rules

### 🔴 Advanced: Professional CLAUDE.md Writing

```
> Create a CLAUDE.md. Include the following:
> - Project structure explanation
> - Naming conventions (files, variables, functions, components)
> - List of prohibited patterns
> - List of preferred libraries
> - Testing rules
> - Git commit message format
```

---

## 🏆 Challenge Tasks

### Challenge 1: Write a Team CLAUDE.md

Write a CLAUDE.md for a hypothetical team project.

**Requirements:**
- Project overview
- Team conventions
- Branch strategy
- Code review rules
- Deployment process

### Challenge 2: Context Management Experiment

Conduct a long conversation to experience context limitations.

```
# Step 1: Provide information
> This project is a shopping mall. (list detailed requirements)

# Step 2: Request multiple feature implementations

# Step 3: Test if early content is remembered
> What was this project again?
> What was the first requirement I mentioned?
```

Experience context limitations and practice workarounds.

---

## 🚨 Common Errors and Solutions

### Error 1: "Claude doesn't follow the rules"

**Situation:** Wrote rules in CLAUDE.md but Claude ignores them

**Cause:** File location is wrong, or content is vague

**Solution:**
```
> Where is CLAUDE.md?
> @CLAUDE.md Show me the contents
```

Check if the file is in the project root.
If rules are vague, make them more specific.

### Error 2: "Forgets context"

**Situation:** Claude doesn't know what you said earlier

**Cause:** Conversation got too long, or you started a new conversation

**Solution:**
```
> Summarize what we were doing earlier
```

Or re-mention important context:
```
> We were making the login feature. @src/login.js this file.
> Add domain validation to the validateEmail function we made earlier.
```

### Error 3: "# isn't working"

**Situation:** Saved with `#` but it's not reflected

**Cause:** Syntax error or need space after prefix

**Solution:**
Put a space right after `#`:
```
> # Respond in English (O)
> #Respond in English (X)
```

### Error 4: "CLAUDE.md is too long and slowing things down"

**Situation:** Responses are slow, rules aren't being applied well

**Cause:** CLAUDE.md is too long and taking up too much context

**Solution:**
- Reduce to 10-15 core rules
- Delete less important rules
- Place most important rules at the top

---

## ❌ Common Mistakes

### 1. Putting CLAUDE.md in Wrong Location
It should be in the **project root**, not `src/CLAUDE.md`.

```
my-project/           # Here!
├── CLAUDE.md         ← Correct location
├── src/
│   └── CLAUDE.md     ← Wrong location
```

### 2. Too Many Rules
If there are 100 rules, Claude gets confused too. Limit to 10-15 core rules.

> 💡 **Beginner Tip**
>
> Remember the **"less is more"** principle.
>
> Following 5 rules well is better than
> loosely following 50 rules.

### 3. Writing Vague Rules
```markdown
# Bad example
- Write code prettily
- Name variables well

# Good example
- Functions under 20 lines
- Variable names in camelCase, use descriptive names explaining their role
```

### 4. Making Requests Without Context
```
# Bad example
> Fix it

# Good example
> @src/login.js Fix the error on line 42 of this file
> The domain validation in validateEmail function isn't working
```

### 5. Not Organizing Long Conversations

Long conversations need mid-point organization:
```
> Summarize what we've done so far
> Make a list of what to do next
```

---

## 🖼️ Expected Screens

### After Creating CLAUDE.md

```
> # This project uses Next.js

Saved to CLAUDE.md:
"This project uses Next.js"

I will now use Next.js patterns when writing code.
```

### Code with Rules Applied

```
> Make a button

// Button.tsx
// Button component - handles click actions
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  variant?: 'primary' | 'secondary';
}

export function Button({
  children,
  onClick,
  variant = 'primary'
}: ButtonProps) {
  // Button styles - apply different styles based on variant
  const baseStyle = "px-4 py-2 rounded font-medium transition-colors";
  const variantStyle = variant === 'primary'
    ? "bg-blue-500 text-white hover:bg-blue-600"
    : "bg-gray-200 text-gray-800 hover:bg-gray-300";

  return (
    <button
      onClick={onClick}
      className={`${baseStyle} ${variantStyle}`}
    >
      {children}
    </button>
  );
}
```

(TypeScript, English comments, Tailwind CSS all applied!)

---

## Mini Project: Create Your Own CLAUDE.md

Create your own project guide.

### 🟢 Easy: Basic Settings

```
> Create a CLAUDE.md.
> - Respond in English
> - Add comments to code
> - Include error handling
```

### 🟡 Medium: Detailed Settings

```
> Add this content to CLAUDE.md:
> - Project: Personal blog
> - Tech: React, TypeScript, Tailwind
> - Style: Functional components, separate custom hooks
> - Rules: Always include error handling, show loading states
```

### 🔴 Challenge: Professional Settings

```
> Create a CLAUDE.md. Include:
> - Project structure explanation (role of each folder)
> - Naming conventions (filenames, variables, functions, components)
> - List of prohibited patterns (any type, console.log, etc.)
> - List of preferred libraries
> - Testing rules
> - API call patterns
> - Error handling patterns
```

---

## 🆘 If It's Not Working?

| Symptom | Solution |
|---------|----------|
| CLAUDE.md isn't being read | Check file location (project root) |
| Rules aren't being followed | Write rules more specifically |
| Context is being forgotten | Re-mention key content |
| # isn't working | Add space after # |
| Responses are slow | Reduce CLAUDE.md length |
| Inconsistent code | Reference existing files: `@src/Button.tsx in the same style` |

---

## 📚 Glossary

| Term | Description |
|------|-------------|
| **Context** | Background information of a conversation |
| **Token** | The smallest unit AI uses to process text |
| **Context Window** | Space where conversation content is stored (limited capacity) |
| **Prompt** | Request/instruction sent to AI |
| **Memory** | Information that persists between conversations (CLAUDE.md) |

---

## ✅ Checklist

Check before finishing your learning:

- [ ] I know what context is
- [ ] I understand context window limitations
- [ ] I can create a CLAUDE.md file
- [ ] I know CLAUDE.md best practices
- [ ] I can save to memory with # prefix
- [ ] I know how to maintain context in long conversations
- [ ] I know how to work around context limitations

---

## 🎉 Starting Part 2!

Congratulations! You've entered Part 2 (Core Skills).

In Part 1, you learned the basics of Claude Code.
In Part 2, you'll learn how to use Claude more effectively.

**What you've learned so far:**
- Installation and basic usage
- Terminal commands
- Project structure
- Context and memory ← Current

**What you'll learn next:**
- Effective prompting
- Code navigation
- Debugging

---

## ➡️ Next Chapter Preview

In the next chapter, we'll learn **Effective Prompting Techniques**.

What you'll learn:
- The science behind prompting
- Various prompting patterns
- How to write optimal prompts for different situations
- Plan mode and Ultrathink usage
- Prompt debugging

Proceed to [Chapter 08: Effective Prompting](../Chapter08-Effective-Prompting/README.md).

---

## 📖 Learn More

### Recommended Resources

**Official Documentation:**
- [Claude Code Memory Guide](https://docs.anthropic.com/en/docs/claude-code/memory) - How to configure CLAUDE.md
- [Understanding Claude Context Windows](https://docs.anthropic.com/en/docs/build-with-claude/context-windows) - Token and context management

**Video Resources:**
- [Context Window Explained (YouTube)](https://www.youtube.com/results?search_query=LLM+context+window+explained) - Context window concept explanation
- [Claude Code CLAUDE.md Usage (YouTube)](https://www.youtube.com/results?search_query=claude+code+CLAUDE.md+tutorial) - Practical CLAUDE.md usage

**Reading Materials:**
- [Anthropic - Long Context Prompting Tips](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) - Tips for managing long context
- [Understanding Tokens in AI](https://platform.openai.com/tokenizer) - Understanding tokens (OpenAI tokenizer)

**Related Tools:**
- [Tokenizer Tool](https://platform.openai.com/tokenizer) - Token counting tool
- [Claude Code GitHub](https://github.com/anthropics/claude-code) - Official repository

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
