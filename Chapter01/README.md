# Chapter 01: What is AI Coding?

**English** | [한국어](./README.ko.md)

## What You'll Learn

- What coding with AI means
- The concept of Vibecoding
- What Claude Code is and why to use it

---

## Before We Start

No coding knowledge is required. This curriculum starts from scratch.

You might think, "But I'm not a developer." Correct--for now. In the AI era, anyone can build their own ideas.

---

## Why Do You Need This?

Think about these everyday situations:

- **"I want to make a simple website for my side project, but hiring a developer is expensive..."**
- **"I have a repetitive task at work that I wish I could automate..."**
- **"I have an app idea but no coding skills to build it..."**
- **"I want to learn coding but don't know where to start..."**

AI coding solves all of these. You don't need years of study anymore. Just tell AI what you want, and it builds it for you.

### Simple Analogy: The Translator

Imagine you want to write a letter in French, but you only speak English.

**Old way**: Study French for years, learn grammar, vocabulary, practice writing...

**AI way**: Tell a fluent French speaker what you want to say, and they write it for you.

AI coding works the same way. You speak "human" (what you want), AI speaks "code" (how to build it). Claude Code is your translator between ideas and working software.

---

## What is AI Coding?

Previously, code had to be written line by line. Memorizing syntax, fixing errors, searching for solutions... learning took years.

Now it's different.

**Tell AI "make this for me" and AI writes the code.**

### Why Is This Possible?

LLMs (Large Language Models) like Claude have learned from vast amounts of code on the internet. They've seen billions of lines from GitHub repositories, Stack Overflow answers, and technical blogs.

So when you say "a web page with a blue heading":
- "blue heading" → `color: blue` pattern
- "web page" → HTML structure pattern

The model combines learned patterns to generate code. It's not magic—it's **pattern matching**.

Clear communication leads to better results. That's why this curriculum exists.

### Example: Creating a Simple Web Page

**Traditional Approach:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <style>
        body { font-family: sans-serif; }
        h1 { color: blue; }
    </style>
</head>
<body>
    <h1>Hello</h1>
    <p>My name is John.</p>
</body>
</html>
```
All of this had to be memorized and written manually.

**AI Coding Approach:**
```
"Make a web page with a blue heading that says 'Hello',
and below it 'My name is John'"
```
When an LLM receives this request, it combines the most appropriate patterns from its training. It already knows web page structure (`<html>`, `<body>`), heading styles (`color: blue`), and text layout.

---

## What is Vibecoding?

**Vibecoding** is a new way of creating code through conversation with AI.

"Vibe" means feeling or atmosphere. Without knowing exact syntax, conveying the "feel" of what you want is enough for AI to understand and implement it.

### Core Principles of Vibecoding

1. **Express intent**: "Make the number go up when I click the button"
2. **Iterate through conversation**: "The number is too small. Make it bigger"
3. **Collaborate**: AI suggests, you choose, AI revises

Traditional coding required knowing everything independently. Vibecoding means building with AI as a partner.

---

## What is Claude Code?

**Claude Code** is an AI coding tool made by Anthropic.

### Difference from Web Chat (ChatGPT, Claude.ai)

Code can be generated in web chats like ChatGPT or Claude.ai, but there are limitations:

| Web Chat | Claude Code |
|----------|-------------|
| Copy code and paste into files manually | Creates and edits files directly |
| No local file access | Understands the entire project |
| Run commands manually | Executes commands on your behalf |
| Explain context every time | Recognizes current context |

**Summary**: Web chat provides the recipe; Claude Code does the cooking.

### Other AI Coding Tools

Claude Code isn't the only AI coding tool. Here are popular options:

| Tool | Type | Description |
|------|------|-------------|
| **Claude Code** | CLI (Terminal) | Anthropic's official CLI. Works in any terminal. |
| **Cursor** | IDE | VS Code-based editor with built-in AI. |
| **Windsurf** | IDE | AI-first code editor by Codeium. |
| **GitHub Copilot** | Extension | AI assistant for VS Code, JetBrains, etc. |

**Why learn Claude Code?**
- Works in any terminal, no IDE lock-in
- Direct file and command access
- Pairs well with any editor you already use
- Understanding CLI-based AI helps with all tools

This curriculum focuses on Claude Code, but the concepts apply to other AI tools too.

### What Claude Code Can Do

- Create websites
- Build apps
- Automate file organization
- Process data
- Modify existing code
- Find and fix bugs

---

## Learning Outcomes

After completing 24 chapters, you will be able to:

- Create and deploy a **portfolio website**
- Build **simple apps** (TODO app, games, etc.)
- Create **automation tools** (file organization, data processing)
- Build **AI services** (document summarizer, translator)

This curriculum teaches not just coding, but **creating with AI**.

---

## Mindset Before Starting

### 1. Perfection Is Not Required
Initial results may be unexpected. Simply try again.

### 2. Errors Are Normal
Errors in coding are not failures—they are part of the process. AI fixes errors too.

### 3. Start Small
Do not attempt something complex from the start. Begin with "Hello World."

### 4. Ask When Curious
"What is this?", "Why this approach?" Ask AI and it will explain.

---

## Try It Yourself (Preview)

You haven't installed Claude Code yet, but here's a sneak peek of what conversations look like:

**Example 1: Making a web page**
```
You: Make a webpage that shows my name "Alex" in big blue letters

Claude: I'll create that for you...
[Creates index.html with styled heading]
```

**Example 2: Automating a task**
```
You: Rename all .jpeg files in this folder to .jpg

Claude: I'll rename those files...
[Renames all files automatically]
```

**Example 3: Fixing code**
```
You: This code isn't working. Fix it.
[paste broken code]

Claude: I see the issue. The error is...
[Explains and fixes the code]
```

See? No coding knowledge needed. Just describe what you want in plain English.

---

## Common Mistakes (Before You Start)

### 1. Thinking You Need to Learn Code First
You don't. That's the whole point. Start with Claude Code, and you'll pick up coding concepts naturally as you go.

### 2. Expecting Perfect Results on the First Try
AI is smart, but not psychic. If the first result isn't what you wanted, just say "that's not quite right, I meant..." and try again.

### 3. Being Too Vague
"Make something cool" is hard for AI to interpret. "Make a blue button that says Click Me" is much clearer.

### 4. Giving Up After One Error
Errors happen to everyone, even professional developers. When something goes wrong, just tell Claude about the error and ask for help.

---

## If It Doesn't Work...

**Feeling overwhelmed?**
That's normal. Take it one chapter at a time. You don't need to understand everything right away.

**Not sure if this is for you?**
If you can write an email, you can use Claude Code. It's just having a conversation.

**Worried about breaking something?**
Claude Code has safety features. It asks for permission before making changes, and you can always say no.

**Questions before starting?**
Great! Curiosity is your best tool. Write down your questions and ask Claude as you learn.

---

## Next Steps

The next chapter covers Claude Code installation. Instructions are provided for both Mac and Windows.

Proceed to [Chapter 02: Installing Claude Code](../Chapter02/README.md).
