# Chapter 16: Building Chatbots

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Building a Discord bot with Claude
- Progressively extending bot features
- Applying patterns to Slack bots

---

## Why Chatbots?

Chatbots are projects that produce actually usable results:
- Use directly in your Discord server
- Automate tasks in your team's Slack
- 24/7 automation tools

**Chatbot request tips:**

```
> Create a Discord bot.
> When /hello command is entered, respond with "Hello!",
> and when /dice command is entered, show a random number between 1-6.
```

Clearly describe command names, trigger conditions, and response content to get the bot you want.

---

## Project: Building a Discord Bot

### Step 1: Preparation (One-time setup)

You need to create a bot in the Discord Developer Portal. Ask Claude:

```
> I want to create a Discord bot.
> Show me how to create a bot in Discord Developer Portal
> and invite it to my server.
```

Steps Claude will guide you through:
1. Access Discord Developer Portal
2. Create new Application
3. Generate Bot token
4. Invite to server via OAuth2 URL

**Important**: The token is like a password. Never put it directly in code!

### Step 2: Start the Project

```
> Create a Discord bot project.
> Use discord.js,
> and manage the token with a .env file.
> Start with just printing a message to console when the bot comes online.
```

What Claude creates with this request:
- Project folder structure
- package.json
- .env file template
- Basic bot code

### Step 3: First Command

```
> Add a /ping command.
> It should show the bot's response time in milliseconds.
```

After confirming it works, add the next command:

```
> Add a /dice command.
> Accept number of sides as an option (default 6),
> and show the dice roll result.
```

**Request tip**: Don't request multiple commands at once. Add and test one at a time.

---

## Extending Features

### Poll Feature

```
> Create a /poll command.
> - Accept a question and 2-4 options
> - Display in a nice embed
> - Automatically add emoji reactions to each option
```

### Reminder Feature

```
> Create a /remind command.
> - Input how many minutes until reminder
> - Input reminder content
> - When time's up, mention the user and notify them
```

### Utility

```
> Create an /avatar command.
> When a user is selected, show their profile image in large size.
> If no one is selected, show the requester's own image.
```

---

## Event-based Features

Beyond slash commands, you can react to various events.

### Welcome Message

```
> When a new member joins the server,
> send a welcome message to the #welcome channel.
> Use a nice embed with their profile picture.
```

### Message Logging

```
> When a message is deleted, log it to the #logs channel.
> Show who deleted what message,
> and exclude bot messages.
```

### Auto Reactions

```
> When a message contains "lol",
> react with the 😂 emoji.
```

---

## Practical Bot Examples

### Server Management Bot

```
> Create a server management bot with these commands:
>
> /kick [user] [reason]
> - Admin only
> - Kick the user from server
>
> /clear [count]
> - Admin only
> - Delete the last N messages
>
> /serverinfo
> - Anyone can use
> - Show server info (member count, creation date, etc.)
```

### Mini Economy System

```
> Create a bot with a mini economy system.
>
> /balance - Check my balance
> /daily - Get 100 coins once per day
> /give [user] [amount] - Send to another user
>
> Save data in a JSON file.
> Use user ID as key, balance as value.
```

---

## Extending to Slack Bots

The patterns you learned with Discord apply to Slack too.

### Core Concepts are the Same

| Concept | Discord | Slack |
|---------|---------|-------|
| Commands | /ping | /ping |
| Event reaction | client.on('event') | app.event('event') |
| Message response | interaction.reply() | say() |

### Starting a Slack Bot

```
> Create a Slack bot project.
> Use the Bolt framework,
> starting with a basic bot that responds to /ping.
> Also show me how to set up the Slack app.
```

### Work Automation Examples

```
> Add these features to the Slack bot:
>
> /standup command
> - Open a modal to input today's tasks, yesterday's work, blockers
> - When submitted, format nicely and post to #standup channel
```

```
> When the bot is mentioned, respond automatically.
> If "meeting room" keyword is present, share meeting room booking link,
> otherwise respond with "How can I help you?"
```

---

## Deploying Your Bot

If you only run locally, the bot stops when you turn off your computer. Deploy for 24/7 operation.

```
> Show me how to deploy this Discord bot to Railway.
> Also how to set environment variables.
```

### Free Deployment Options

| Service | Features |
|---------|----------|
| Railway | $5 free credits/month, easy setup |
| Render | Free, sleeps when inactive |
| Fly.io | Has free tier |

---

## Debugging Tips

When the bot doesn't respond:

```
> The bot isn't responding to commands.
> No errors in console, and the bot shows as online.
> What could be the problem?
```

Things Claude will check:
- If slash commands are registered with Discord
- Bot permission settings
- Intents configuration
- If token is correct

---

## Practice

### Basic Task

Create your own Discord bot:

```
> Create a Discord bot with these features:
>
> 1. /quote - Show a random quote
> 2. /choose [options] - Pick one from multiple options
> 3. /8ball [question] - Random answer like a magic 8-ball
>
> Start with basic structure and add one at a time.
```

### Advanced Challenges

```
> Create a music recommendation bot.
> When /mood [feeling] command is entered,
> recommend song genres or vibes matching that mood.
> (Actual music playback is complex, so just recommendations)
```

```
> Create a todo management bot.
> - /todo add [content] - Add a todo
> - /todo list - View my todo list
> - /todo done [number] - Mark as complete
> - Save data as JSON
```

---

## Summary

What you learned in this chapter:
- [x] Starting a Discord bot project
- [x] Adding slash commands
- [x] Reacting to events
- [x] Extending patterns to Slack bots
- [x] Deploying bots

The core of chatbots is **"when this event happens, respond like this"**. Understanding this pattern lets you build bots on any platform.

When requesting from Claude:
1. What command/event to react to
2. What input to receive
3. What output to produce

Clarify these three things and you can build the bot you want.

In the next chapter, we'll build a full-stack app with both frontend and backend.

Proceed to [Chapter 17: Building Full-Stack Apps](../Chapter17/README.md).
