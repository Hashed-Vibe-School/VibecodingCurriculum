# Chapter 16: Building Chatbots

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Building a Discord bot with Claude
- Progressively extending bot features
- Applying patterns to Slack bots

---

## Why Do You Need This?

**Real-world scenarios where chatbots shine:**

- **Managing your gaming community** - A bot can welcome new members, moderate chat, run polls for game nights
- **Automating team workflows** - Daily standup reminders, auto-responses to common questions, meeting scheduling
- **Building engagement** - Mini-games, trivia, points systems that make your server more interactive
- **Personal productivity** - A bot that reminds you of tasks, saves bookmarks, or tracks your habits

Once you build a chatbot, it works for you **24/7** - even while you sleep!

---

## Simple Analogy: Bots are Like Helpful Store Employees

Imagine walking into a store:
- **Without employees**: You have to find everything yourself, figure out where things are
- **With a helpful employee**: They greet you, answer questions, guide you to what you need

Chatbots work the same way:
- **Without a bot**: Every question gets asked repeatedly, moderators must be online 24/7
- **With a bot**: Instant responses, automatic moderation, consistent experience for everyone

The bot is your tireless virtual assistant that never takes breaks!

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

## Try It Yourself: Minimal Working Example

Before building complex features, let's make sure your bot can respond to a simple command:

**1. Create a minimal bot file (`bot.js`):**

```javascript
// bot.js - Simplest possible Discord bot
require('dotenv').config()
const { Client, GatewayIntentBits, REST, Routes, SlashCommandBuilder } = require('discord.js')

const client = new Client({ intents: [GatewayIntentBits.Guilds] })

// When bot is ready
client.once('ready', () => {
  console.log(`Bot is online as ${client.user.tag}!`)
})

// Respond to /ping command
client.on('interactionCreate', async interaction => {
  if (!interaction.isChatInputCommand()) return

  if (interaction.commandName === 'ping') {
    await interaction.reply('Pong! I am alive!')
  }
})

// Login with your token
client.login(process.env.DISCORD_TOKEN)
```

**2. Create `.env` file (keep this secret!):**

```
DISCORD_TOKEN=your_bot_token_here
```

**3. Register the slash command (run once):**

```javascript
// register-commands.js - Run this once to register /ping
require('dotenv').config()
const { REST, Routes, SlashCommandBuilder } = require('discord.js')

const commands = [
  new SlashCommandBuilder().setName('ping').setDescription('Check if bot is alive')
].map(cmd => cmd.toJSON())

const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

rest.put(Routes.applicationCommands(process.env.CLIENT_ID), { body: commands })
  .then(() => console.log('Commands registered!'))
  .catch(console.error)
```

**4. Run it:**

```bash
npm install discord.js dotenv
node register-commands.js  # Run once
node bot.js                # Start the bot
```

If you see "Bot is online" and `/ping` works in Discord, you're ready for the full project!

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

## If It Doesn't Work? Troubleshooting Tips

### Bot is online but doesn't respond to commands

```bash
# Most common issue: Commands aren't registered with Discord
# Run your register-commands.js script again
node register-commands.js

# Wait 1-2 minutes for Discord to propagate the commands
```

### "Missing Access" or "Missing Permissions" error

1. Go to Discord Developer Portal > Your App > OAuth2 > URL Generator
2. Select `bot` and `applications.commands` scopes
3. Select required permissions (Send Messages, Use Slash Commands, etc.)
4. Use the generated URL to re-invite the bot to your server

### "Invalid token" error

```bash
# Check your .env file
# Make sure there are no extra spaces or quotes
DISCORD_TOKEN=your_token_here  # Correct
DISCORD_TOKEN="your_token_here"  # Wrong - no quotes!
DISCORD_TOKEN= your_token_here   # Wrong - no space!
```

### Bot responds locally but not after deployment

```bash
# Make sure environment variables are set in your hosting platform
# Railway/Render/Heroku all have their own env var settings

# Test that the token is being read
console.log('Token exists:', !!process.env.DISCORD_TOKEN)
```

### Commands take forever to update

- **Global commands** (applicationCommands): Takes up to 1 hour to update globally
- **Guild commands** (applicationGuildCommands): Updates instantly but only in that server

Use guild commands during development for faster testing!

---

## Common Mistakes

### 1. Exposing Your Bot Token

```javascript
// NEVER do this - your token is visible to everyone!
client.login('MTIzNDU2Nzg5MDEyMzQ1Njc4.XXXXX.YYYYY')

// ALWAYS use environment variables
client.login(process.env.DISCORD_TOKEN)
```

If you accidentally commit a token to GitHub, **regenerate it immediately** in the Discord Developer Portal!

### 2. Forgetting to Set Intents

```javascript
// WRONG - bot won't receive message events
const client = new Client({ intents: [] })

// CORRECT - specify what events you need
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent  // Required for reading message content
  ]
})
```

### 3. Not Handling Interaction Responses

```javascript
// WRONG - crashes if you try to reply twice
await interaction.reply('First response')
await interaction.reply('Second response')  // Error!

// CORRECT - use followUp for additional messages
await interaction.reply('First response')
await interaction.followUp('Second response')

// Or use editReply for deferred responses
await interaction.deferReply()
// ... do some processing ...
await interaction.editReply('Done!')
```

### 4. Commands Not Registering

```javascript
// Make sure you have both CLIENT_ID and DISCORD_TOKEN
// CLIENT_ID is your Application ID from Discord Developer Portal
Routes.applicationCommands(process.env.CLIENT_ID)

// Also check you're using the right Routes:
// - applicationCommands: Global commands (all servers, slow to update)
// - applicationGuildCommands: Guild commands (one server, instant update)
```

### 5. Bot Going Offline After Closing Terminal

Your bot stops when you close the terminal! For 24/7 uptime:
- Use a hosting service (Railway, Render, Fly.io)
- Or use a process manager like PM2: `pm2 start bot.js`

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
