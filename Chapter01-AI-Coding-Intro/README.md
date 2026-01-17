# Chapter 01: What is AI Coding?

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

By the end of this chapter, you will understand:

- What it actually means to code with AI
- The core concept of Vibecoding as a new development approach
- What Claude Code is and why it's different from other tools
- What kinds of things you can build with AI coding
- What you'll learn through this curriculum

---

## ⏱️ Estimated Time

- Reading: **20-25 minutes**
- Hands-on: None (conceptual learning chapter)

This chapter is theory-focused. Take your time reading and focus on understanding the concepts.

---

## 📋 What You Need

- Nothing (theory learning chapter)
- Just an open mind and curiosity!

---

## 🗺️ Journey Preview: See the Forest First

Before diving into details, let me show you where this entire curriculum is headed. It's like looking at a map before a trip.

### Analogy: Learning to Cook

Let me compare this curriculum to **learning to cook**:

```
┌─────────────────────────────────────────────────────────────────┐
│                    🍳 Vibecoding = Learning to Cook              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 1: Tour the Kitchen (Chapter 1-5)                         │
│  ─────────────────────────────────────                          │
│  "Learn what cooking is and what kitchen tools are"             │
│                                                                 │
│  → Understand what cooking (AI coding) is                       │
│  → Install and explore the kitchen (Claude Code)                │
│  → Learn basic tools like knives and cutting boards (terminal)  │
│                                                                 │
│  🎯 After this stage: "Ah, so this is how it works!" clicks     │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 2-3: Master the Basics (Chapter 6-16)                     │
│  ────────────────────────────────────                           │
│  "Basic dishes like scrambled eggs and instant noodles"         │
│                                                                 │
│  → Learn to read recipes (code)                                 │
│  → Make simple dishes (webpages, small apps)                    │
│  → Learn to fix mistakes                                        │
│                                                                 │
│  🎯 After this stage: You can create websites and games         │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 4: Serious Cooking (Chapter 17-20)                        │
│  ────────────────────────────────────                           │
│  "Hosting guests with a full course meal"                       │
│                                                                 │
│  → Build automation tools (eliminate repetitive work)           │
│  → Create chatbots (AI services)                                │
│  → Build real apps that store data                              │
│                                                                 │
│  🎯 After this stage: You can create usable apps                │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 5: Become a Pro Chef (Chapter 21-27)                      │
│  ────────────────────────────────────                           │
│  "Set up your kitchen your way and run it efficiently"          │
│                                                                 │
│  → Understand Claude Code internals                             │
│  → Create your own settings and automation                      │
│  → Collaborate with teams                                       │
│                                                                 │
│  🎯 After this stage: You'll be a Claude Code master            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Chapter Milestones

| Chapter | What You Can Do When You Reach Here |
|---------|-------------------------------------|
| **Chapter 5** | Claude Code installed, basic conversation possible |
| **Chapter 10** | Can read and modify code, fix simple bugs |
| **Chapter 16** | Create visible results like websites and games |
| **Chapter 20** | Develop "real" apps that store data |
| **Chapter 27** | Master Claude Code completely, including team collaboration |

### How Long Will It Take?

- **2 hours daily** consistently: About **2 weeks**
- **1 hour daily** consistently: About **4 weeks**
- **Weekends only** intensively: About **4 weeks**

> 💡 **Beginner Tip**
>
> You don't need to understand everything perfectly from the start!
>
> When learning to cook, you follow recipes at first too.
> Later, as you get comfortable, you'll think "Ah, that's why we do it this way."
>
> **Just follow along first, understand later.** That's the fastest path.

### Where Are You Now?

```
    ⭐ You are here! (Chapter 1)
    │
    ▼
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│ Part 1  │ → │ Part 2  │ → │ Part 3  │ → │ Part 4  │ → │ Part 5  │
│ Start   │   │ Core    │   │ Practice│   │ Real    │   │ Advanced│
│ Ch.1-5  │   │ Ch.6-10 │   │ Ch.11-16│   │ Ch.17-20│   │ Ch.21-27│
└─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘
   Setup       Basics      First Works   Real Apps      Master
```

Now that you've seen the big picture, shall we take the first step?

---

## Before We Begin: A Message for You

**"I'm not a developer, can I learn this?"**

If you're asking this question, you're exactly the target audience for this curriculum. This curriculum was created for people who know nothing about coding.

You've probably heard **"Coding is hard."** That's true, it **used to be** hard. But now we have AI.

### The Difference Between Then and Now

| Before (Pre-2022) | Now (AI Era) |
|-------------------|--------------|
| Had to memorize programming language syntax | Just tell AI what you want |
| Had to search and solve errors yourself | Just ask AI "Why the error?" |
| Needed to know multiple concepts to write one line | Can create results without knowing concepts |
| Took years to learn | Can complete first project in hours |

**Key point**: The barrier to entry for coding has essentially disappeared. Follow this curriculum, and you can build something today.

---

## Why Is This Necessary?

Let's look at real situations that AI coding can solve:

### Situation 1: Side Project

> "I need a personal portfolio website. Hiring a developer is too expensive, but I don't know how to code to make it myself."

**AI Coding Solution**: Just tell Claude "Create a website that introduces me. Include my name, experience, and contact info, with a simple design." It's done in 30 minutes.

### Situation 2: Work Automation

> "Every week I extract and organize specific data from 100 Excel files. It's so repetitive."

**AI Coding Solution**: Tell Claude "Read the Excel files in this folder, extract only columns A and C, and merge them into one file," and an automation script is created.

### Situation 3: App Idea

> "I have an idea for a simple todo app. Existing apps are too complicated, and I need an app with just the features I want."

**AI Coding Solution**: Just request step by step from Claude. "Add a task adding feature" → "Add a completion check feature" → "Add a delete feature too"

### Situation 4: Learning Purpose

> "I want to learn coding, but books and courses are too difficult and I gave up."

**AI Coding Solution**: Learn by building. When you ask Claude "Explain what this code does," it explains kindly. Practice comes before theory.

---

## Easy Analogy: The Interpreter

The easiest way to understand AI coding is to compare it to an **interpreter**.

### Scenario: French Letter

You want to write a letter to a friend in France. But you don't know French.

**Old Way** (Traditional coding learning):
1. Enroll in French school
2. Learn grammar, vocabulary, and writing for 6 months to 1 year
3. Make mistakes, fix them, keep learning
4. Finally write the letter

**AI Way** (AI coding):
1. Go to a friend who knows French well (AI)
2. Say "Write a letter with this content in French for me"
3. They write it
4. Say "Make this part more friendly" and they revise it

**Key Difference**:
- Before: I have to do everything myself
- Now: AI, the expert, does it for me

### So What Do I Have to Do?

Good question. **What you need to do is "clearly communicate what you want."**

"Write a letter" is less effective than "Write a letter to my friend Pierre, talking about the memories of meeting in Paris last year, and inviting him to visit the US this year."

This is exactly what **Prompting** is. It's one of the core skills you'll learn in this curriculum.

---

## What is AI Coding?

### Definition

**AI Coding** is a method of creating software through conversation with artificial intelligence.

Before, it was like this:
```
Idea in developer's head → Developer writes code directly → Software
```

Now it's like this:
```
Your idea → Explain to AI → AI writes code → Software
```

### Why Has This Become Possible?

**LLMs (Large Language Models)** like Claude have learned billions of lines of code from the internet.

Where they learned:
- **GitHub**: Tens of millions of open-source projects
- **Stack Overflow**: Tens of millions of developer questions and answers
- **Tech blogs and documentation**: Official documentation and tutorials for all programming languages

Because they've learned all of this, AI can:
- Write code in any language
- Know various styles and patterns
- Infer causes and solutions when seeing errors
- Implement complex logic

> 💡 **Beginner Tip**
>
> It's okay if you don't exactly know what **LLM (Large Language Model)** means.
>
> Simply put, it's "an AI that has read and learned almost everything on the internet."
> It can do various things beyond coding, like writing, translation, and summarization.
>
> For detailed terminology, refer to the [Glossary](../GLOSSARY.md).

### It's Not Magic, It's Pattern Matching

AI doesn't "understand" code. **It recognizes patterns.**

**Example**: When you say "Create a webpage with a blue title"

AI thinks like this:
1. "Blue title" → Connects to `color: blue` or `text-blue-500` patterns from countless code it's seen before
2. "Webpage" → HTML structure patterns (`<html>`, `<head>`, `<body>`)
3. Combines these patterns to generate code

Result:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <style>
        h1 { color: blue; }
    </style>
</head>
<body>
    <h1>Hello</h1>
</body>
</html>
```

**Important**: Because AI combines patterns, the clearer your request, the more accurate the result.

---

## Traditional Coding vs AI Coding: Detailed Comparison

### Example: Creating a Simple Webpage

**Goal**: A webpage with a blue title "Hello" and below it "My name is John Doe"

#### Traditional Method

First, you need to learn what HTML is:
- What are HTML tags (`<html>`, `<head>`, `<body>`, etc.)
- Tag structure and syntax
- Opening and closing rules

Then you need to learn CSS:
- What is CSS
- What are Selectors
- Properties and Values

And then write the code yourself:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Page</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
        }
        h1 {
            color: blue;
            font-size: 32px;
        }
        p {
            color: #333;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Hello</h1>
    <p>My name is John Doe.</p>
</body>
</html>
```

To write this code:
- Understanding basic HTML structure: 2-3 hours
- Understanding basic CSS: 2-3 hours
- Writing and testing yourself: 1 hour
- **Total time: About 5-7 hours** (for first-time learners)

#### AI Coding Method

Tell Claude like this:

```
Create a webpage that shows "Hello" in blue as a title,
and "My name is John Doe" below it
```

Claude generates code similar to above.

- **Total time: 30 seconds**

#### Summary of Differences

| Item | Traditional Method | AI Method |
|------|-------------------|-----------|
| Required prior knowledge | HTML, CSS basics | None |
| Time required | Hours to days | Seconds to minutes |
| When errors occur | Search and solve yourself | Ask "Why the error?" |
| Modification request | Modify code yourself | "Make the title bigger" |
| Learning curve | Steep | Gentle |

---

## What is Vibecoding?

### Origin of the Term

**Vibe** means "feeling" or "atmosphere."

**Vibecoding** means "coding by feel." Even without knowing exact syntax or rules, you convey the "feel" you want to AI, and AI understands and implements it.

This term was first used by Andrej Karpathy (former Tesla AI Director).

### The 3-Step Cycle of Vibecoding

Vibecoding repeats the following 3 steps:

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   1. Convey Intent  →  2. Check Result  →  3. Refine with  │
│        ↑                                    Feedback │      │
│        └────────────────────────────────────────────┘       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Step 1: Convey Intent

Describe what you want in natural language.

```
> Create a counter that increases by 1 when a button is clicked
```

#### Step 2: Check Result

Check the result that AI created. Judge whether it matches what you wanted or not.

```
(Claude generates code)
(Check in browser)
"Hmm, the button works but the number is too small..."
```

#### Step 3: Refine with Feedback

Give feedback on what's lacking.

```
> Make the number size bigger. About 48px.
> And change the button color to blue.
```

Repeat this cycle a few times, and the desired result is complete.

### Core Principles of Vibecoding

#### Principle 1: There's No Perfect First Try

The probability of a perfect result on the first try is low. That's normal. Just refine it through conversation.

#### Principle 2: Start Small

```
# Bad Example
> Create a user management system with login, signup, password recovery, and profile editing features

# Good Example
> First just create a login form. Just email and password input.
```

It's better to break it into small units and complete them one by one.

#### Principle 3: Be Specific

```
# Vague Example
> Create a pretty button

# Specific Example
> Create a button that says "Submit" with white text on a blue background.
> Make the corners rounded, and change to a darker blue when hovering.
```

The more specific, the closer to what you want.

#### Principle 4: AI is a Partner

AI is not a tool but a collaboration partner. You can ask questions, ask for opinions, and request suggestions.

```
> What's the best way to build this feature?
> Which of these two methods is better?
> Is there a better way?
```

---

## What is Claude Code?

### Definition

**Claude Code** is an AI coding tool made by Anthropic. It runs in the terminal (command input window) and can write code, create/modify files, and execute commands through conversation.

### Difference Between Web Chat and Claude Code

You can also generate code in web chats like ChatGPT or Claude.ai. But there are important differences.

#### Limitations of Web Chat

1. **Cannot execute code directly**
   - Web chat: Just shows code → You have to copy and paste into a file
   - Claude Code: Creates and saves files directly

2. **Doesn't know the current situation**
   - Web chat: You have to explain project structure every time
   - Claude Code: Reads folders and files directly to understand

3. **Cannot execute commands**
   - Web chat: "Run npm install" → Only tells you the command, you have to run it
   - Claude Code: Executes commands directly and shows results

4. **Difficult to do continuous work**
   - Web chat: Remembers previous conversation context, but doesn't know file state
   - Claude Code: Can do continuous work while seeing actual file state

#### Understanding Through Analogy

| Web Chat | Claude Code |
|----------|-------------|
| Cooking YouTuber who tells you the recipe | Chef who cooks directly in the kitchen |
| Just says "Do it like this" | Preps ingredients and cooks directly |
| You have to make the result yourself | Result comes out immediately |

### Comparison with Other AI Coding Tools

Claude Code is not the only option. There are other popular tools:

| Tool | Type | Pros | Cons |
|------|------|------|------|
| **Claude Code** | CLI (Terminal) | Works in all environments, lightweight and fast, transferable learning | Need to be familiar with terminal |
| **Cursor** | IDE (Editor) | VS Code based, visual editing, code autocomplete | Heavy, locked to specific IDE |
| **Windsurf** | IDE (Editor) | AI-centric design, deep code understanding | New tool with small ecosystem |
| **GitHub Copilot** | Extension | Works in existing editors, specializes in code autocomplete | Limited for conversational work |

### Why Learn Claude Code?

1. **Versatility**: Works in all terminals. Mac, Windows, Linux all possible.

2. **Lightweight**: Can use alongside editors you already use without installing a separate editor.

3. **Transferable Learning**: Understanding CLI-based AI tool principles makes it easy to adapt to other tools.

4. **Direct Execution**: Can create/modify files and execute commands all in one place.

> 💡 **Beginner Tip**
>
> You don't need to know what **CLI** is.
>
> Simply put, it's a way of commanding the computer by typing with keyboard instead of using a mouse.
> It's also called a "terminal." We'll learn more in Chapter 02.

---

## What You Can Do with Claude Code

### Creating Websites

```
> My name is John Smith, and I'm a web developer.
> Create a portfolio website for me.
> The main page should have a self-introduction, project list, and contact info.
```

Claude generates HTML, CSS, and JavaScript files.

### Creating Apps

```
> Create a todo list app.
> It should have add, delete, and completion check features.
> Make it work in the browser.
```

Claude creates a Todo app for you.

### Automating File Organization

```
> Organize the photos in this folder into folders by year.
> Based on the photo file creation date.
```

Claude creates and runs a script that analyzes and organizes files.

### Data Processing

```
> Read the data.csv file,
> extract only rows where sales are over $10,000
> and save as result.csv.
```

Claude creates and runs a data processing script.

### Modifying Existing Code

```
> @src/login.js in this file
> there's an email format validation part,
> modify it to also allow .co.uk and .org domains.
```

Claude reads and modifies the existing file.

### Finding and Fixing Bugs

```
> I'm getting this error:
> "TypeError: Cannot read property 'map' of undefined"
>
> @src/ProductList.js I think it's this file, fix it.
```

Claude analyzes the error cause and fixes it.

### Code Explanation

```
> @src/utils/dateFormat.js Explain what this file does.
> In a way that non-developers can understand.
```

Claude explains the code line by line kindly.

---

## 🖼️ Preview: Claude Code Screen

We haven't installed it yet, but let's preview what Claude Code looks like.

### Start Screen

```
╭────────────────────────────────────────╮
│          Welcome to Claude Code!        │
│                                         │
│  Type your message and press Enter.     │
│  Use @ to mention files.                │
│  Type /help for more commands.          │
╰────────────────────────────────────────╯

/home/user/my-project
>
```

Just type what you want after the `>`.

### File Creation Example

```
> Create a webpage with a blue title

Claude: Sure, I'll create that. I'll create an index.html file.

┌─ Creating file: index.html ───────────────────────────┐
│                                                       │
│ <!DOCTYPE html>                                       │
│ <html lang="en">                                      │
│ <head>                                                │
│     <meta charset="UTF-8">                            │
│     <title>My Page</title>                            │
│     <style>                                           │
│         h1 { color: blue; }                           │
│     </style>                                          │
│ </head>                                               │
│ <body>                                                │
│     <h1>Hello</h1>                                    │
│ </body>                                               │
│ </html>                                               │
│                                                       │
└───────────────────────────────────────────────────────┘

I've created the file! Would you like to open it in the browser?

> Yes, open it

Claude: Sure, I'll open it in the browser.

Running: open index.html

The page has opened in the browser.
```

### Modification Request Example

```
> Make the title bigger and center it

Claude: Sure, I'll modify the style.

┌─ Editing: index.html ─────────────────────────────────┐
│                                                       │
│ - h1 { color: blue; }                                 │
│ + h1 {                                                │
│ +     color: blue;                                    │
│ +     font-size: 48px;                                │
│ +     text-align: center;                             │
│ + }                                                   │
│                                                       │
└───────────────────────────────────────────────────────┘

Modified. Please refresh your browser.
```

---

## Learning Outcomes: When You Complete This Curriculum

Upon completing all 26 chapters, you'll be able to:

### 🌐 Website Creation and Deployment
- Create a personal portfolio website
- Create a blog
- Create a landing page
- Deploy to the real internet so anyone can see it

### 📱 App Development
- Create a Todo app
- Create simple games
- Create apps that save and load data

### 🤖 Building Automation Tools
- Automate file organization
- Automate data processing
- Create repetitive task scripts

### 🧠 AI Service Integration
- Create a document summarizer
- Create a translator
- Create a chatbot

**Key point**: You're not "learning" to code, you're learning to **"build" with AI**.

---

## Mindset Before Starting: 4 Principles for Success

### Principle 1: It Doesn't Have to Be Perfect

The probability of a perfect result on the first try is low. That's normal.

**Real Situation**:
```
Me: Create a button
Claude: (creates button)
Me: Hmm, I don't like the color. Change it to blue.
Claude: (changes to blue)
Me: Make the size bigger too.
Claude: (changes size)
```

It's natural to refine through conversation like this.

### Principle 2: Errors Are Normal

In coding, errors are **not failure but process**.

Even professional developers encounter errors dozens of times a day. There's no need to feel frustrated when an error occurs.

**When an error occurs**:
```
Me: I got an error. This message appeared: "TypeError: ..."
Claude: Ah, this error is because of ... You can fix it like this.
(Fixes)
```

Claude can fix errors too.

### Principle 3: Start Small

Don't try to build something complex from the start.

**Bad Start**:
```
> Create a portal site like Google
```

**Good Start**:
```
> Create a webpage that says "Hello World"
```

Start with something small and add features one by one.

### Principle 4: Ask When Curious

Don't just skip parts you don't understand.

```
> What does 'const' mean in this code?
> Why do I need to put a semicolon here?
> I don't understand this part. Explain it more simply.
```

Claude explains kindly. Asking questions is not embarrassing.

---

## 🔨 Try It Yourself: Preview Experience

We haven't installed Claude Code yet, but let's preview what conversations look like.

### Example 1: Self-Introduction Webpage

```
Me: Create a webpage that shows my name "John Smith" in big blue letters.
    Add an introduction below that says "I'm a college student dreaming of becoming a developer."

Claude: Sure, I'll create that. I'll create an index.html file.

(Generates code)

I've created the file. Would you like to open it in the browser?

Me: Yes

Claude: (Opens in browser)

How does it look? Let me know if there's anything you'd like to modify.

Me: Change the background color to light gray

Claude: Sure, I'll modify the background color.

(Modifies)

Done. Please refresh.
```

### Example 2: File Automation

```
Me: Rename all .jpeg files in this folder to .jpg

Claude: Sure, let me first check the files in the folder.

(Runs ls command)

There are 12 .jpeg files. I'll rename them.

(Runs rename)

Done. Changed the extension of 12 files to .jpg.
```

### Example 3: Code Debugging

```
Me: I get an error when running this code.

const name = "John Doe"
console.log(Name)

Claude: I found the cause of the error. JavaScript is case-sensitive.

The variable name is 'name' (lowercase), but when printing you used 'Name' (capital N).

Fix it like this:
console.log(name)  // lowercase 'n'

Me: Ah, I see. I didn't know it distinguishes case.

Claude: Yes, in JavaScript 'name', 'Name', and 'NAME' are all different variables.
It can be confusing at first, but generally variables use
camelCase style starting with lowercase.
Example: userName, isLoggedIn, totalPrice
```

**Key point**: Even without coding knowledge, you can solve problems through conversation.

---

## ❌ Common Mistakes

### Mistake 1: Thinking "I Need to Learn Coding First"

**Wrong thinking**: "I should learn Python basics first and then use Claude Code"

**Right approach**: Use Claude Code first and learn the concepts you need as you go. Practice before theory.

### Mistake 2: Expecting the First Result to Be Perfect

**Wrong expectation**: "It should come out exactly as I want in one try, right?"

**Reality**: The first result is about 80%. The remaining 20% is refined through feedback.

### Mistake 3: Requesting Too Vaguely

**Bad request**: "Create a cool website"

**Good request**: "Create a landing page with a big title on the main page, 3 service introduction cards, and contact info at the bottom. Use a blue color scheme."

### Mistake 4: Giving Up After One Error

**Bad reaction**: "An error... I'm just not cut out for this..."

**Good reaction**: "I got an error. This message appeared: [error message]. Why is this happening?"

---

## 🆘 If It's Not Working?

### Q: It feels too complicated

**A**: That's normal! Take it slow, one chapter at a time. You don't need to understand everything at once. You'll naturally get it as you practice.

### Q: I'm worried about whether I can do this

**A**: If you can send an email, you can use Claude Code. Just request things conversationally. No special abilities needed.

### Q: I'm worried about breaking something

**A**: Claude Code is safe. It always asks permission before changing files. If you decline, it won't change anything. Even if you make a mistake, you can undo it.

### Q: I have a question, but where do I ask?

**A**:
1. Ask Claude directly: "What is this?", "Why do it this way?"
2. Ask in this curriculum's Discord
3. If curious about terminology, check the [Glossary](../GLOSSARY.md)

---

## ✅ Checklist

Before finishing this chapter, confirm the following:

### Concept Understanding
- [ ] I can explain what AI coding is in one sentence
- [ ] I understand the 3-step cycle of vibecoding (Convey intent → Check result → Feedback)
- [ ] I can name 2+ differences between traditional coding and AI coding

### Understanding Claude Code
- [ ] I know how Claude Code differs from web chat
- [ ] I know 3+ things I can do with Claude Code
- [ ] I know there are other AI coding tools (Cursor, Copilot, etc.)

### Mindset
- [ ] I understand it's okay if the first result isn't perfect
- [ ] I understand errors are a process, not failure
- [ ] I understand I should start small

---

## 📚 Learn More (Optional)

This section is optional. It's for those who want to go deeper.

### History of AI Coding
- 2021: GitHub Copilot released (first AI coding assistant)
- 2022: ChatGPT released, popularization
- 2023: More powerful models like Claude, GPT-4 appeared
- 2024: Specialized tools like Claude Code, Cursor developed
- 2025: AI coding becomes mainstream, non-developers can create apps

### Recommended Resources

**Official Documentation:**
- [Claude Code Official Documentation](https://docs.anthropic.com/en/docs/claude-code) - All features and usage of Claude Code
- [Anthropic Official Blog](https://www.anthropic.com/news) - AI development trends and new features

**Video Resources:**
- [What is Vibe Coding? (YouTube)](https://www.youtube.com/results?search_query=vibe+coding+ai) - Videos explaining the vibecoding concept
- [Claude AI Tutorial (YouTube)](https://www.youtube.com/results?search_query=claude+ai+tutorial) - Claude usage tutorials

**Reading Materials:**
- [Andrej Karpathy's "Vibecoding" Tweet](https://twitter.com/karpathy/status/1886192184808149383) - Origin of the vibecoding term
- [Building Effective Agents (Anthropic)](https://www.anthropic.com/research/building-effective-agents) - AI agent design guide

**Community:**
- [Claude Code GitHub](https://github.com/anthropics/claude-code) - Official repository and issue tracker
- [Anthropic Discord](https://discord.gg/anthropic) - Community questions and discussions

---

## ➡️ Next Steps

Congratulations! You've completed Chapter 01.

In the next chapter, we'll **actually install Claude Code**. We provide detailed instructions for both Mac and Windows.

Proceed to [Chapter 02: Installing Claude Code](../Chapter02-Installation/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
