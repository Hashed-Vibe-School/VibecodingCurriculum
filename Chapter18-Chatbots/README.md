# Chapter 18: Building Chatbots

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn

- Building a Discord bot with Claude
- Progressively extending bot features
- Core concepts of event-driven programming
- Extending patterns to Slack bots
- Deploying bots for 24/7 operation

---

## Connection to Previous Chapter

In Chapter 17, we built CLI tools that run in the terminal. In this chapter, we'll build **bots that work in Discord or Slack**. Like CLI tools, they process text commands, but they can interact with multiple users simultaneously.

---

## Why Do You Need This?

**Real-world scenarios where chatbots shine:**

- **Managing your gaming community** - A bot can welcome new members, moderate chat, run polls for game nights
- **Automating team workflows** - Daily standup reminders, auto-responses to common questions, meeting scheduling
- **Building engagement** - Mini-games, trivia, points systems that make your server more interactive
- **Personal productivity** - A bot that reminds you of tasks, saves bookmarks, or tracks your habits

Once you build a chatbot, it works for you **24/7** - even while you sleep!

> **Beginner Tip**: Bots may seem complex, but they're actually just a collection of simple rules: "when a certain condition occurs, perform a certain action." Condition -> Action, just remember that!

---

## Simple Analogy: Bots are Like Helpful Store Employees

Imagine walking into a store:
- **Without employees**: You have to find everything yourself, figure out where things are
- **With a helpful employee**: They greet you, answer questions, guide you to what you need

Chatbots work the same way:
- **Without a bot**: Every question gets asked repeatedly, moderators must be online 24/7
- **With a bot**: Instant responses, automatic moderation, consistent experience for everyone

The bot is your **tireless virtual assistant** that never takes breaks!

---

## Core Concepts of Chatbots

### 1. Event-Driven Programming

Chatbots respond to **events**:

```javascript
// Event -> Handler (processing function)
client.on('messageCreate', (message) => {
    // Handle content
})

client.on('guildMemberAdd', (member) => {
    // Welcome message
})

client.on('messageReactionAdd', (reaction) => {
    // Assign roles, etc.
})
```

> **Beginner Tip**: Events are "when something happens." Just like your phone rings when a text arrives, bots react to specific situations.

### 2. Key Discord Events

| Event | When It Occurs | Use Case |
|-------|----------------|----------|
| `ready` | Bot starts | Ready log |
| `interactionCreate` | Slash command used | Command processing |
| `messageCreate` | Message sent | Auto-response |
| `guildMemberAdd` | Member joins | Welcome message |
| `guildMemberRemove` | Member leaves | Log record |
| `messageReactionAdd` | Reaction added | Role assignment |

### 3. Slash Commands vs Message Commands

```
Slash Commands (Recommended):
/ping        -> Discord provides autocomplete
/dice 6      -> Options are also autocompleted
/help        -> Official and clean

Message Commands (Legacy):
!ping        -> Must parse manually
!dice 6      -> Error handling is complex
!help        -> May conflict with other bots
```

> **Pro Tip**: Discord has recommended slash commands since 2022. Build new bots with slash commands.

---

## Project: Building a Discord Bot

### Step 1: Preparation (One-time setup)

You need to create a bot in the Discord Developer Portal. Ask Claude:

```
> I want to create a Discord bot.
> Show me how to create a bot in Discord Developer Portal
> and invite it to my server.
```

**Steps Claude will guide you through:**

1. **Access Discord Developer Portal**
   - Go to https://discord.com/developers/applications
   - Log in with your Discord account

2. **Create New Application**
   - Click "New Application"
   - Enter bot name (e.g., "My First Bot")

3. **Bot Settings**
   - Select "Bot" from the left menu
   - Click "Add Bot"
   - **Copy the token** (very important! Only shown once)

4. **Permission Settings**
   - "OAuth2" -> "URL Generator" menu
   - Select `bot` and `applications.commands` in Scopes
   - Select required permissions in Bot Permissions

5. **Invite to Server**
   - Go to the generated URL
   - Select the server to add the bot to

> **Warning**: The token is like a password. Never put it directly in code! Manage it with environment variables (.env).

### Step 2: Start the Project

```
> Create a Discord bot project.
> Use discord.js,
> and manage the token with a .env file.
> Start with just printing a message to console when the bot comes online.
```

**Project Structure:**

```
discord-bot/
├── package.json
├── .env                  # Secret info (Don't upload to Git!)
├── .gitignore
├── src/
│   ├── index.js          # Main entry point
│   ├── commands/         # Commands
│   │   ├── ping.js
│   │   ├── dice.js
│   │   └── index.js
│   ├── events/           # Event handlers
│   │   ├── ready.js
│   │   ├── interactionCreate.js
│   │   └── guildMemberAdd.js
│   └── utils/            # Utilities
│       └── deploy-commands.js
└── README.md
```

### Step 3: Basic Bot Code

```javascript
// src/index.js
require('dotenv').config()
const { Client, GatewayIntentBits, Collection } = require('discord.js')

// Create client (set required permissions)
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,           // Access server info
    GatewayIntentBits.GuildMessages,    // Message events
    GatewayIntentBits.GuildMembers,     // Member events
    GatewayIntentBits.MessageContent    // Read message content (if needed)
  ]
})

// Command storage
client.commands = new Collection()

// When bot is ready
client.once('ready', () => {
  console.log(`Bot is online as ${client.user.tag}!`)
  console.log(`Active in ${client.guilds.cache.size} servers`)

  // Set status message
  client.user.setActivity('Waiting for commands', { type: 'WATCHING' })
})

// Error handling
client.on('error', (error) => {
  console.error('Client error:', error)
})

process.on('unhandledRejection', (error) => {
  console.error('Unhandled rejection:', error)
})

// Login with token
client.login(process.env.DISCORD_TOKEN)
```

**.env file:**

```env
# .env - Never upload to Git!
DISCORD_TOKEN=your_bot_token_here
CLIENT_ID=your_application_id_here
GUILD_ID=your_test_server_id_here
```

**.gitignore:**

```gitignore
node_modules/
.env
*.log
```

### Step 4: First Command (Ping)

```
> Add a /ping command.
> It should show the bot's response time in milliseconds.
```

```javascript
// src/commands/ping.js
const { SlashCommandBuilder } = require('discord.js')

module.exports = {
  // Command definition
  data: new SlashCommandBuilder()
    .setName('ping')
    .setDescription('Check the bot response time'),

  // Command execution
  async execute(interaction) {
    // Record current time
    const sent = await interaction.reply({
      content: 'Measuring...',
      fetchReply: true
    })

    // Calculate response time
    const latency = sent.createdTimestamp - interaction.createdTimestamp
    const apiLatency = Math.round(interaction.client.ws.ping)

    // Display result
    await interaction.editReply(
      `Pong!\n` +
      `Response time: ${latency}ms\n` +
      `API latency: ${apiLatency}ms`
    )
  }
}
```

### Step 5: Command Registration

You need to register commands with Discord for slash commands to appear.

```javascript
// src/utils/deploy-commands.js
require('dotenv').config()
const { REST, Routes } = require('discord.js')
const fs = require('fs')
const path = require('path')

// Read command files
const commands = []
const commandsPath = path.join(__dirname, '../commands')
const commandFiles = fs.readdirSync(commandsPath).filter(file => file.endsWith('.js'))

for (const file of commandFiles) {
  if (file === 'index.js') continue
  const command = require(path.join(commandsPath, file))
  if (command.data) {
    commands.push(command.data.toJSON())
    console.log(`Command loaded: ${command.data.name}`)
  }
}

// REST API client
const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

// Registration function
async function deployCommands() {
  try {
    console.log(`Registering ${commands.length} commands...`)

    // Register as guild commands (instant update, for development)
    const data = await rest.put(
      Routes.applicationGuildCommands(
        process.env.CLIENT_ID,
        process.env.GUILD_ID
      ),
      { body: commands }
    )

    console.log(`${data.length} commands registered successfully!`)

    // To register as global commands (all servers, takes 1 hour)
    // Routes.applicationCommands(process.env.CLIENT_ID)

  } catch (error) {
    console.error('Command registration failed:', error)
  }
}

deployCommands()
```

### Step 6: Command Handler

```javascript
// src/events/interactionCreate.js
module.exports = {
  name: 'interactionCreate',

  async execute(interaction) {
    // Ignore if not a slash command
    if (!interaction.isChatInputCommand()) return

    // Find command
    const command = interaction.client.commands.get(interaction.commandName)

    if (!command) {
      console.error(`Command not found: ${interaction.commandName}`)
      return
    }

    // Execute command
    try {
      await command.execute(interaction)
    } catch (error) {
      console.error(`Command execution error:`, error)

      const errorMessage = 'An error occurred while executing the command.'

      if (interaction.replied || interaction.deferred) {
        await interaction.followUp({ content: errorMessage, ephemeral: true })
      } else {
        await interaction.reply({ content: errorMessage, ephemeral: true })
      }
    }
  }
}
```

> **Beginner Tip**: `ephemeral: true` means the message is only visible to that user. Use it for error messages or sensitive information.

---

## Try It Yourself: Minimal Working Example

Before building complex features, let's make sure your bot can respond to a simple command:

**1. Create a minimal bot file (`bot.js`):**

```javascript
// bot.js - Simplest possible Discord bot
require('dotenv').config()
const {
  Client,
  GatewayIntentBits,
  REST,
  Routes,
  SlashCommandBuilder
} = require('discord.js')

// Create client
const client = new Client({
  intents: [GatewayIntentBits.Guilds]
})

// When bot is ready
client.once('ready', () => {
  console.log(`Bot is online as ${client.user.tag}!`)
})

// Respond to slash commands
client.on('interactionCreate', async interaction => {
  if (!interaction.isChatInputCommand()) return

  if (interaction.commandName === 'ping') {
    await interaction.reply('Pong! I am alive!')
  }
})

// Login with token
client.login(process.env.DISCORD_TOKEN)
```

**2. Create `.env` file:**

```env
DISCORD_TOKEN=your_bot_token_here
CLIENT_ID=your_application_id_here
GUILD_ID=your_test_server_id_here
```

**3. Register slash command (run once):**

```javascript
// register-commands.js
require('dotenv').config()
const { REST, Routes, SlashCommandBuilder } = require('discord.js')

const commands = [
  new SlashCommandBuilder()
    .setName('ping')
    .setDescription('Check if bot is alive')
].map(cmd => cmd.toJSON())

const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

rest.put(
  Routes.applicationGuildCommands(process.env.CLIENT_ID, process.env.GUILD_ID),
  { body: commands }
)
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

## Creating More Commands

### Dice Command

```
> Add a /dice command.
> Accept number of sides as an option (default 6),
> and show the dice roll result.
```

```javascript
// src/commands/dice.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')

module.exports = {
  data: new SlashCommandBuilder()
    .setName('dice')
    .setDescription('Roll a dice')
    .addIntegerOption(option =>
      option
        .setName('sides')
        .setDescription('Number of sides on the dice')
        .setMinValue(2)
        .setMaxValue(100)
    )
    .addIntegerOption(option =>
      option
        .setName('count')
        .setDescription('Number of dice to roll')
        .setMinValue(1)
        .setMaxValue(10)
    ),

  async execute(interaction) {
    const sides = interaction.options.getInteger('sides') ?? 6
    const count = interaction.options.getInteger('count') ?? 1

    // Roll dice
    const rolls = []
    for (let i = 0; i < count; i++) {
      rolls.push(Math.floor(Math.random() * sides) + 1)
    }

    const total = rolls.reduce((a, b) => a + b, 0)

    // Display result with nice embed
    const embed = new EmbedBuilder()
      .setColor(0x00AE86)
      .setTitle('Dice Roll')
      .setDescription(`Rolled ${count} ${sides}-sided dice!`)
      .addFields(
        { name: 'Result', value: rolls.join(' + '), inline: true },
        { name: 'Total', value: `**${total}**`, inline: true }
      )
      .setFooter({ text: `Requested by: ${interaction.user.tag}` })
      .setTimestamp()

    await interaction.reply({ embeds: [embed] })
  }
}
```

> **Pro Tip**: Using `EmbedBuilder` allows you to send beautiful card-style messages. You can add colors, images, fields, and more.

### Poll Command

```
> Create a /poll command.
> - Accept a question and 2-4 options
> - Display in a nice embed
> - Automatically add emoji reactions to each option
```

```javascript
// src/commands/poll.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')

const EMOJIS = ['1️⃣', '2️⃣', '3️⃣', '4️⃣']

module.exports = {
  data: new SlashCommandBuilder()
    .setName('poll')
    .setDescription('Create a poll')
    .addStringOption(option =>
      option
        .setName('question')
        .setDescription('Poll question')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('option1')
        .setDescription('Option 1')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('option2')
        .setDescription('Option 2')
        .setRequired(true)
    )
    .addStringOption(option =>
      option.setName('option3').setDescription('Option 3')
    )
    .addStringOption(option =>
      option.setName('option4').setDescription('Option 4')
    ),

  async execute(interaction) {
    const question = interaction.options.getString('question')
    const options = [
      interaction.options.getString('option1'),
      interaction.options.getString('option2'),
      interaction.options.getString('option3'),
      interaction.options.getString('option4')
    ].filter(Boolean)  // Remove null/undefined

    // Generate option text
    const optionText = options
      .map((opt, i) => `${EMOJIS[i]} ${opt}`)
      .join('\n\n')

    // Create embed
    const embed = new EmbedBuilder()
      .setColor(0x5865F2)
      .setTitle(`${question}`)
      .setDescription(optionText)
      .setFooter({
        text: `Poll created by: ${interaction.user.tag}`,
        iconURL: interaction.user.displayAvatarURL()
      })
      .setTimestamp()

    // Send poll message
    const pollMessage = await interaction.reply({
      embeds: [embed],
      fetchReply: true
    })

    // Automatically add reactions
    for (let i = 0; i < options.length; i++) {
      await pollMessage.react(EMOJIS[i])
    }
  }
}
```

### Reminder Command

```
> Create a /remind command.
> - Input how many minutes until reminder
> - Input reminder content
> - When time's up, mention the user and notify them
```

```javascript
// src/commands/remind.js
const { SlashCommandBuilder } = require('discord.js')

// Active timer storage (in-memory only - resets on restart)
const activeReminders = new Map()

module.exports = {
  data: new SlashCommandBuilder()
    .setName('remind')
    .setDescription('Set a reminder')
    .addIntegerOption(option =>
      option
        .setName('minutes')
        .setDescription('How many minutes until the reminder?')
        .setRequired(true)
        .setMinValue(1)
        .setMaxValue(60 * 24)  // Max 24 hours
    )
    .addStringOption(option =>
      option
        .setName('message')
        .setDescription('Reminder content')
        .setRequired(true)
    ),

  async execute(interaction) {
    const minutes = interaction.options.getInteger('minutes')
    const message = interaction.options.getString('message')
    const userId = interaction.user.id
    const channelId = interaction.channelId

    // Current time + minutes
    const remindTime = new Date(Date.now() + minutes * 60 * 1000)
    const timeString = remindTime.toLocaleTimeString()

    // Set timer
    const timerId = setTimeout(async () => {
      try {
        const channel = await interaction.client.channels.fetch(channelId)
        await channel.send(`<@${userId}> Reminder!\n\n${message}`)
        activeReminders.delete(timerId)
      } catch (error) {
        console.error('Failed to send reminder:', error)
      }
    }, minutes * 60 * 1000)

    activeReminders.set(timerId, { userId, message, remindTime })

    // Confirmation message
    await interaction.reply({
      content: `Reminder set!\n\n` +
               `Time: ${timeString} (in ${minutes} minutes)\n` +
               `Content: ${message}`,
      ephemeral: true  // Only visible to requester
    })
  }
}
```

---

## Event-Based Features

Beyond slash commands, you can react to various events.

### Welcome Message

```
> When a new member joins the server,
> send a welcome message to the #welcome channel.
> Use a nice embed with their profile picture.
```

```javascript
// src/events/guildMemberAdd.js
const { EmbedBuilder } = require('discord.js')

module.exports = {
  name: 'guildMemberAdd',

  async execute(member) {
    // Find welcome channel (channel named 'welcome')
    const welcomeChannel = member.guild.channels.cache.find(
      channel => channel.name === 'welcome'
    )

    if (!welcomeChannel) {
      console.log('Welcome channel not found')
      return
    }

    // Server member count
    const memberCount = member.guild.memberCount

    // Welcome embed
    const embed = new EmbedBuilder()
      .setColor(0x57F287)
      .setTitle('A new member has arrived!')
      .setDescription(
        `Welcome to **${member.guild.name}**, ${member}!\n\n` +
        `We now have **${memberCount}** members!`
      )
      .setThumbnail(member.user.displayAvatarURL({ dynamic: true, size: 256 }))
      .addFields(
        { name: 'Joined', value: `<t:${Math.floor(Date.now() / 1000)}:R>`, inline: true },
        { name: 'ID', value: member.id, inline: true }
      )
      .setFooter({ text: 'Please read the rules and say hello!' })
      .setTimestamp()

    await welcomeChannel.send({ embeds: [embed] })

    // Optional: Auto-assign role
    try {
      const defaultRole = member.guild.roles.cache.find(role => role.name === 'Member')
      if (defaultRole) {
        await member.roles.add(defaultRole)
      }
    } catch (error) {
      console.error('Failed to assign role:', error)
    }
  }
}
```

> **Warning**: To use member events, your bot needs `SERVER MEMBERS INTENT` permission. You must enable this in the Discord Developer Portal.

### Auto Reactions

```
> When a message contains certain words,
> automatically add emoji reactions.
```

```javascript
// src/events/messageCreate.js
module.exports = {
  name: 'messageCreate',

  async execute(message) {
    // Ignore bot messages
    if (message.author.bot) return

    // Auto-reaction rules
    const reactions = {
      'congrats': '🎉',
      'gg': '👏',
      'lol': '😂',
      'love': '❤️',
      'sad': '😢',
      'nice': '👍',
      'coffee': '☕'
    }

    const content = message.content.toLowerCase()

    for (const [keyword, emoji] of Object.entries(reactions)) {
      if (content.includes(keyword)) {
        try {
          await message.react(emoji)
        } catch (error) {
          console.error('Failed to add reaction:', error)
        }
      }
    }

    // Respond when bot is mentioned
    if (message.mentions.has(message.client.user)) {
      await message.reply('How can I help you? Try `/help`!')
    }
  }
}
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

```javascript
// src/commands/kick.js
const { SlashCommandBuilder, PermissionFlagsBits } = require('discord.js')

module.exports = {
  data: new SlashCommandBuilder()
    .setName('kick')
    .setDescription('Kick a user from the server')
    .addUserOption(option =>
      option
        .setName('user')
        .setDescription('User to kick')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('reason')
        .setDescription('Reason for kick')
    )
    // Require admin permission
    .setDefaultMemberPermissions(PermissionFlagsBits.KickMembers),

  async execute(interaction) {
    const targetUser = interaction.options.getUser('user')
    const reason = interaction.options.getString('reason') ?? 'No reason provided'

    const member = await interaction.guild.members.fetch(targetUser.id)

    // Permission check
    if (!member.kickable) {
      return interaction.reply({
        content: 'Cannot kick this user. (Insufficient permissions)',
        ephemeral: true
      })
    }

    try {
      await member.kick(reason)
      await interaction.reply(`${targetUser.tag} has been kicked.\nReason: ${reason}`)
    } catch (error) {
      await interaction.reply({
        content: 'An error occurred while kicking.',
        ephemeral: true
      })
    }
  }
}
```

```javascript
// src/commands/clear.js
const { SlashCommandBuilder, PermissionFlagsBits } = require('discord.js')

module.exports = {
  data: new SlashCommandBuilder()
    .setName('clear')
    .setDescription('Bulk delete messages')
    .addIntegerOption(option =>
      option
        .setName('count')
        .setDescription('Number of messages to delete (1-100)')
        .setRequired(true)
        .setMinValue(1)
        .setMaxValue(100)
    )
    .setDefaultMemberPermissions(PermissionFlagsBits.ManageMessages),

  async execute(interaction) {
    const count = interaction.options.getInteger('count')

    await interaction.deferReply({ ephemeral: true })

    try {
      const deleted = await interaction.channel.bulkDelete(count, true)
      await interaction.editReply(`Deleted ${deleted.size} messages.`)
    } catch (error) {
      await interaction.editReply('Cannot delete messages older than 14 days.')
    }
  }
}
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
```

```javascript
// src/commands/daily.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')
const fs = require('fs')
const path = require('path')

const DATA_PATH = path.join(__dirname, '../../data/economy.json')

// Data load/save functions
function loadData() {
  if (!fs.existsSync(DATA_PATH)) {
    fs.mkdirSync(path.dirname(DATA_PATH), { recursive: true })
    fs.writeFileSync(DATA_PATH, '{}')
    return {}
  }
  return JSON.parse(fs.readFileSync(DATA_PATH, 'utf-8'))
}

function saveData(data) {
  fs.writeFileSync(DATA_PATH, JSON.stringify(data, null, 2))
}

const DAILY_AMOUNT = 100
const COOLDOWN_MS = 24 * 60 * 60 * 1000  // 24 hours

module.exports = {
  data: new SlashCommandBuilder()
    .setName('daily')
    .setDescription('Receive daily reward (100 coins)'),

  async execute(interaction) {
    const userId = interaction.user.id
    const data = loadData()

    // Initialize user data
    if (!data[userId]) {
      data[userId] = { balance: 0, lastDaily: 0 }
    }

    const userData = data[userId]
    const now = Date.now()
    const timeSinceLastDaily = now - userData.lastDaily

    // Cooldown check
    if (timeSinceLastDaily < COOLDOWN_MS) {
      const remaining = COOLDOWN_MS - timeSinceLastDaily
      const hours = Math.floor(remaining / (60 * 60 * 1000))
      const minutes = Math.floor((remaining % (60 * 60 * 1000)) / (60 * 1000))

      return interaction.reply({
        content: `${hours} hours and ${minutes} minutes until next reward!`,
        ephemeral: true
      })
    }

    // Give reward
    userData.balance += DAILY_AMOUNT
    userData.lastDaily = now
    saveData(data)

    const embed = new EmbedBuilder()
      .setColor(0xFFD700)
      .setTitle('Daily Reward!')
      .setDescription(
        `You received **+${DAILY_AMOUNT}** coins!\n\n` +
        `Current balance: **${userData.balance}** coins`
      )
      .setFooter({ text: 'Come back tomorrow!' })

    await interaction.reply({ embeds: [embed] })
  }
}
```

---

## Extending to Slack Bots

The patterns you learned with Discord apply to Slack too.

### Core Concepts are the Same

| Concept | Discord | Slack |
|---------|---------|-------|
| Library | discord.js | @slack/bolt |
| Commands | Slash Commands | Slash Commands |
| Event reaction | client.on('event') | app.event('event') |
| Message response | interaction.reply() | say() |
| Token | DISCORD_TOKEN | SLACK_BOT_TOKEN |

### Starting a Slack Bot

```
> Create a Slack bot project.
> Use the Bolt framework,
> starting with a basic bot that responds to /ping.
> Also show me how to set up the Slack app.
```

```javascript
// app.js - Slack bot
const { App } = require('@slack/bolt')
require('dotenv').config()

const app = new App({
  token: process.env.SLACK_BOT_TOKEN,
  signingSecret: process.env.SLACK_SIGNING_SECRET,
  socketMode: true,
  appToken: process.env.SLACK_APP_TOKEN
})

// Slash command
app.command('/ping', async ({ command, ack, say }) => {
  await ack()
  await say(`Pong! Response time: ${Date.now() - command.event_ts * 1000}ms`)
})

// Message event
app.message('hello', async ({ message, say }) => {
  await say(`Hello, <@${message.user}>!`)
})

// App mention
app.event('app_mention', async ({ event, say }) => {
  await say(`Yes, <@${event.user}>! How can I help you?`)
});

// Start app
(async () => {
  await app.start(process.env.PORT || 3000)
  console.log('Slack bot is running!')
})()
```

### Work Automation Examples

```
> Add these features to the Slack bot:
>
> /standup command
> - Open a modal to input today's tasks, yesterday's work, blockers
> - When submitted, format nicely and post to #standup channel
```

---

## Deploying Your Bot

If you only run locally, the bot stops when you turn off your computer. Deploy for 24/7 operation.

### Free Deployment Options

| Service | Features | Cost |
|---------|----------|------|
| Railway | Easy setup, GitHub integration | $5 free credits/month |
| Render | Free tier, sleeps when inactive | Free (750 hours/month) |
| Fly.io | Docker based, free tier | Free (limited) |
| Replit | Includes code editor | Free (limited) |
| Personal server | Complete control | VPS cost |

### Railway Deployment

```
> Show me how to deploy this Discord bot to Railway.
> Also how to set environment variables.
```

**Steps:**

1. Sign up for Railway (https://railway.app)
2. Connect GitHub
3. "New Project" -> "Deploy from GitHub repo"
4. Select repository
5. Set environment variables:
   - `DISCORD_TOKEN`
   - `CLIENT_ID`
6. Confirm deployment

### package.json Settings

```json
{
  "name": "discord-bot",
  "version": "1.0.0",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js",
    "deploy-commands": "node src/utils/deploy-commands.js"
  },
  "engines": {
    "node": ">=18.0.0"
  },
  "dependencies": {
    "discord.js": "^14.0.0",
    "dotenv": "^16.0.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  }
}
```

> **Beginner Tip**: Setting the `engines` field ensures the deployment platform uses the correct Node.js version.

---

## Practice

### Level 1: Basic (Beginner)

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

**Checklist:**
- [ ] Is the bot online?
- [ ] Do slash commands autocomplete?
- [ ] Does each command respond correctly?

### Level 2: Intermediate

```
> Create a todo management bot.
> - /todo add [content] - Add a todo
> - /todo list - View my todo list
> - /todo done [number] - Mark as complete
> - Save data as JSON
```

**Checklist:**
- [ ] Are todos being added?
- [ ] Is the list displayed nicely?
- [ ] Does data persist after bot restart?

### Level 3: Advanced

```
> Create a music recommendation bot.
> When /mood [feeling] command is entered,
> recommend song genres or vibes matching that mood.
> (Actual music playback is complex, so just recommendations)
```

---

## Challenges

### Challenge 1: Level System

XP gained per message, level-up notifications:

```javascript
// 10-20 XP per message
// Level-up formula: Required XP = Level * 100
// Congratulation message on level-up
```

### Challenge 2: Role Menu

Assign/remove roles via reactions:

```
/roles command creates role selection message
1️⃣ -> Gamer role
2️⃣ -> Artist role
3️⃣ -> Developer role
```

### Challenge 3: Web Dashboard

Manage bot settings via web:

```
- Express.js server
- Discord OAuth2 login
- Per-server settings page
```

---

## If It Doesn't Work? Troubleshooting Tips

### Bot is online but doesn't respond to commands

```bash
# Most common issue: Commands aren't registered with Discord
node src/utils/deploy-commands.js

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
- Or use a process manager like PM2: `pm2 start src/index.js`

### 6. Not Using Async/Await

```javascript
// WRONG - without await, errors can occur
async execute(interaction) {
  interaction.reply('Response')  // No await!
}

// CORRECT - always use await
async execute(interaction) {
  await interaction.reply('Response')
}
```

---

## Glossary

| Term | Meaning |
|------|---------|
| Intent | Configuration for what event types the bot receives |
| Slash Command | Official command starting with `/` |
| Embed | Card-style formatted message |
| Guild | Discord server (internal term) |
| Interaction | User-bot interaction |
| Token | Secret key for bot authentication |
| Client | The bot program itself |
| ephemeral | Message visible only to the requester |

---

## Next Chapter Preview

Now that we've built a chatbot, we'll create a **full-stack app**!

- **Chapter 19**: Building Full-Stack Apps
  - Frontend (React)
  - Backend (Express)
  - Database (SQLite)

The event handling and async programming learned from chatbots applies directly to full-stack development!

---

## Summary

What you learned in this chapter:
- [x] Starting a Discord bot project
- [x] Adding slash commands
- [x] Creating beautiful messages with Embeds
- [x] Reacting to events
- [x] Extending patterns to Slack bots
- [x] Deploying bots

The core of chatbots is **"when this event happens, respond like this"**. Understanding this pattern lets you build bots on any platform.

When requesting from Claude:
1. What command/event to react to
2. What input to receive
3. What output to produce

Clarify these three things and you can build the bot you want.

Proceed to [Chapter 19: Backend Basics](../Chapter19-Backend-Basics/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
