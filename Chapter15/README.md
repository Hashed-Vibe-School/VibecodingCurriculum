# Chapter 15: Building AI Tools

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Using AI APIs
- Building a chatbot
- Building an image generator

---

## What is an AI API?

An AI API is a service that lets you use artificial intelligence features.

Instead of building AI yourself, you call the API and AI does the work:
- Generate text
- Generate images
- Translate
- Summarize

---

## Getting Started with OpenAI API

This section uses OpenAI, the most popular AI API.

### Step 1: Get API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up (credit card required)
3. Create a key in API Keys menu
4. Save the key somewhere safe

### Step 2: Project Setup

```
> Create an AI chatbot project.
> Will use OpenAI API.
> Manage API key with environment variables.
```

### Environment Variable Setup

`.env` file:
```
OPENAI_API_KEY=sk-your-api-key-here
```

**Warning**: Never upload `.env` file to GitHub!

---

## Project 1: Simple Chatbot

### Goal

- User enters a question
- AI responds
- Show conversation history

### Building It

```
> Make a simple AI chatbot.
> User asks questions, gets answers from OpenAI API.
> Show conversation history too.
```

### Core Code (Node.js)

```javascript
import OpenAI from 'openai'

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
})

async function chat(message) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      { role: "user", content: message }
    ]
  })

  return response.choices[0].message.content
}

// Usage
const answer = await chat("Hello!")
console.log(answer)
```

### Improving It

```
> Make it remember conversation context
```
→ Message history management and context passing

```
> Add system prompt.
> "You are a friendly assistant"
```
→ Controlling AI behavior with system prompts

```
> Show loading indicator while waiting for response
```
→ Async state management for better UX

---

## Project 2: Text Summarizer

### Goal

- Paste long text
- AI summarizes in 3 sentences
- Extract key keywords

### Building It

```
> Make a text summarizer.
> Summarize long text in 3 sentences.
> Also extract key keywords.
```

### Core Logic

```javascript
async function summarize(text) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      {
        role: "system",
        content: "You are a text summarization expert. Summarize concisely."
      },
      {
        role: "user",
        content: `Summarize the following in 3 sentences and extract 5 key keywords:\n\n${text}`
      }
    ]
  })

  return response.choices[0].message.content
}
```

### Improving It

```
> Let user choose summary length (short/medium/long)
```
→ Dynamic prompt generation pattern

```
> Summarize foreign text in English
```
→ Multi-language handling and translation

```
> Add copy button
```
→ Clipboard API usage

---

## Project 3: Image Generator

### Using DALL-E API

You can generate images with OpenAI's DALL-E.

### Building It

```
> Make an image generator.
> Enter a description, generate image with DALL-E.
> Display generated image on screen.
```

### Core Code

```javascript
async function generateImage(prompt) {
  const response = await openai.images.generate({
    model: "dall-e-3",
    prompt: prompt,
    n: 1,
    size: "1024x1024"
  })

  return response.data[0].url
}

// Usage
const imageUrl = await generateImage("A cat eating pizza in space")
```

### Improving It

```
> Let user choose image size
```
→ API parameter configuration pattern

```
> Show loading animation while generating
```
→ Long-running request state handling

```
> Add image download button
```
→ Blob creation and download links

---

## Project 4: Translator

### Goal

- Input text
- Auto-detect language
- Translate to desired language

### Building It

```
> Make an AI translator.
> Auto-detect input language.
> Support English, Korean, Japanese, Chinese.
```

### Core Logic

```javascript
async function translate(text, targetLanguage) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      {
        role: "system",
        content: `You are a translator. Translate the given text to ${targetLanguage}. Only output the translation, nothing else.`
      },
      {
        role: "user",
        content: text
      }
    ]
  })

  return response.choices[0].message.content
}
```

---

## Managing API Costs

### Check Pricing

OpenAI API charges based on usage.

| Model | Price (per 1K tokens) |
|-------|----------------------|
| GPT-3.5 Turbo | ~$0.001 |
| GPT-4 | ~$0.03 |
| DALL-E 3 | ~$0.04/image |

### Cost Saving Tips

```
> Add API call limit feature.
> Maximum 10 times per day.

> Add response caching.
> Use saved answer for same questions.
```

### Monitor Usage

Check usage in OpenAI dashboard.
Setting a budget limit keeps you safe.

---

## Free Alternatives

If OpenAI is too expensive, there are free options:

| Service | Features |
|---------|----------|
| Claude API | Anthropic's AI |
| Hugging Face | Free open source models |
| Ollama | Run AI locally |

```
> Show me how to make a local AI chatbot with Ollama
```

---

## Security Considerations

### Protecting API Keys

```
# Never do this
const apiKey = "sk-1234567890"  // Directly in code

# Do this
const apiKey = process.env.OPENAI_API_KEY  // Use environment variable
```

### .gitignore Setup

```
.env
.env.local
*.env
```

### Don't Call API from Frontend

API key can be exposed. Always call through a backend server.

---

## Practice: Build an AI Tool

### Basic Task

Complete one of the projects above.

### Extra Challenges

```
> Make a code explainer tool.
> Paste code and AI explains it.

> Make an email writing assistant.
> Enter key points, converts to polite email.

> Make a quiz generator.
> Enter a topic, AI creates quiz questions.
```

---

## Mini Project: AI Multi-Tool

Combine multiple AI features into one app.

### Goals

- Integrate multiple AI features
- User-friendly UI

### Build It

```
> Create an AI multi-tool.
> One app that supports:
> - Chatbot
> - Text summarization
> - Translation
> - Image generation
> Switch features with tabs or sidebar
```

### Advanced Challenges (For Experts)

```
> Add conversation history save/load feature

> Implement streaming responses (text appears character by character)

> Add prompt template management

> Allow selecting between AI models (GPT-4, Claude, etc.)
```

---

## Summary

What you learned in this chapter:
- [x] How to use AI APIs
- [x] Building a chatbot
- [x] Image generation
- [x] Translation/summarization tools
- [x] Managing API costs

In the next chapter, we'll learn how to process and analyze data.

[Chapter 16: Data Processing](../Chapter16/README.md)
