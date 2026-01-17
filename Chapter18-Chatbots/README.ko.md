# Chapter 18: 챗봇 만들기

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Discord 봇을 Claude와 함께 만드는 과정
- 봇 기능을 점진적으로 확장하는 방법
- 이벤트 기반 프로그래밍의 핵심
- Slack 봇으로 패턴 확장하기
- 봇을 24시간 운영하는 배포 방법

---

## 이전 챕터와의 연결

Chapter 17에서는 터미널에서 실행하는 CLI 도구를 만들었습니다. 이번 챕터에서는 **Discord나 Slack에서 동작하는 봇**을 만들어봅니다. CLI와 마찬가지로 텍스트 명령어를 처리하지만, 여러 사용자와 동시에 상호작용할 수 있습니다.

---

## 왜 필요합니까?

**챗봇이 빛나는 실제 상황들:**

- **게임 커뮤니티 관리** - 봇이 새 멤버 환영, 채팅 관리, 게임 나이트 투표를 할 수 있습니다
- **팀 워크플로우 자동화** - 일일 스탠드업 알림, 자주 묻는 질문 자동 응답, 미팅 스케줄링
- **참여도 높이기** - 미니게임, 퀴즈, 포인트 시스템으로 서버를 더 활발하게 만들 수 있습니다
- **개인 생산성** - 할 일 알림, 북마크 저장, 습관 추적을 도와주는 봇

챗봇을 한 번 만들면 **24시간 연중무휴**로 일합니다. 여러분이 자는 동안에도 작동합니다.

> 💡 **비전공자 팁**: 봇은 복잡해 보이지만, 실제로는 "특정 조건이 발생하면 특정 동작을 한다"는 간단한 규칙의 모음입니다. 조건 → 동작, 이것만 기억하세요!

---

## 쉬운 비유: 봇은 친절한 매장 직원과 같습니다

매장에 들어간다고 상상해 보시기 바랍니다:
- **직원 없이**: 모든 것을 직접 찾아야 하고, 어디에 무엇이 있는지 헤매야 합니다
- **친절한 직원과 함께**: 인사해주고, 질문에 답하고, 원하는 것을 안내해줍니다

챗봇도 마찬가지입니다:
- **봇 없이**: 같은 질문이 반복되고, 관리자가 24시간 접속해 있어야 합니다
- **봇과 함께**: 즉각적인 응답, 자동 관리, 모든 사람에게 일관된 경험을 제공합니다

봇은 **절대 쉬지 않는 가상 비서**입니다.

---

## 챗봇의 핵심 개념

### 1. 이벤트 기반 프로그래밍

챗봇은 **이벤트**에 반응합니다:

```javascript
// 이벤트 → 핸들러(처리 함수)
client.on('메시지가 왔을 때', (message) => {
    // 처리할 내용
})

client.on('멤버가 들어왔을 때', (member) => {
    // 환영 메시지
})

client.on('반응이 추가됐을 때', (reaction) => {
    // 역할 부여 등
})
```

> 💡 **비전공자 팁**: 이벤트는 "무엇이 발생했을 때"입니다. 문자가 왔을 때 알림이 울리는 것처럼, 봇도 특정 상황에 반응합니다.

### 2. 주요 Discord 이벤트

| 이벤트 | 발생 시점 | 활용 예시 |
|-------|---------|---------|
| `ready` | 봇이 시작됨 | 준비 완료 로그 |
| `interactionCreate` | 슬래시 명령어 사용 | 명령어 처리 |
| `messageCreate` | 메시지 전송 | 자동 응답 |
| `guildMemberAdd` | 멤버 입장 | 환영 메시지 |
| `guildMemberRemove` | 멤버 퇴장 | 로그 기록 |
| `messageReactionAdd` | 반응 추가 | 역할 부여 |

### 3. 슬래시 커맨드 vs 메시지 명령어

```
슬래시 커맨드 (권장):
/ping        → Discord가 자동완성 제공
/dice 6      → 옵션도 자동완성
/help        → 공식적이고 깔끔함

메시지 명령어 (레거시):
!ping        → 직접 파싱해야 함
!dice 6      → 에러 처리 복잡
!help        → 다른 봇과 충돌 가능
```

> 🔥 **프로 팁**: Discord는 2022년부터 슬래시 커맨드를 권장합니다. 새 봇은 슬래시 커맨드로 만드세요.

---

## 프로젝트: Discord 봇 만들기

### Step 1: 사전 준비 (한 번만 하면 됨)

Discord 개발자 포털에서 봇을 생성해야 합니다. Claude에게 물어보세요:

```
> Discord 봇을 만들려고 해.
> Discord Developer Portal에서 봇을 생성하고
> 서버에 초대하는 방법을 알려줘.
```

**Claude가 안내해주는 단계:**

1. **Discord Developer Portal 접속**
   - https://discord.com/developers/applications 로 이동
   - Discord 계정으로 로그인

2. **새 Application 생성**
   - "New Application" 클릭
   - 봇 이름 입력 (예: "My First Bot")

3. **Bot 설정**
   - 왼쪽 메뉴에서 "Bot" 선택
   - "Add Bot" 클릭
   - **토큰 복사** (매우 중요! 한 번만 보여줌)

4. **권한 설정**
   - "OAuth2" → "URL Generator" 메뉴
   - Scopes에서 `bot`과 `applications.commands` 선택
   - Bot Permissions에서 필요한 권한 선택

5. **서버에 초대**
   - 생성된 URL로 이동
   - 봇을 추가할 서버 선택

> ⚠️ **주의사항**: 토큰은 비밀번호와 같습니다. 절대 코드에 직접 넣지 마세요! 환경변수(.env)로 관리해야 합니다.

### Step 2: 프로젝트 시작

```
> Discord 봇 프로젝트를 만들어줘.
> discord.js를 사용하고,
> .env 파일로 토큰을 관리하게 해줘.
> 일단 봇이 온라인 되면 콘솔에 메시지만 출력하는 것부터.
```

**프로젝트 구조:**

```
discord-bot/
├── package.json
├── .env                  # 비밀 정보 (Git에 올리면 안 됨!)
├── .gitignore
├── src/
│   ├── index.js          # 메인 진입점
│   ├── commands/         # 명령어들
│   │   ├── ping.js
│   │   ├── dice.js
│   │   └── index.js
│   ├── events/           # 이벤트 핸들러들
│   │   ├── ready.js
│   │   ├── interactionCreate.js
│   │   └── guildMemberAdd.js
│   └── utils/            # 유틸리티
│       └── deploy-commands.js
└── README.md
```

### Step 3: 기본 봇 코드

```javascript
// src/index.js
require('dotenv').config()
const { Client, GatewayIntentBits, Collection } = require('discord.js')

// 클라이언트 생성 (필요한 권한 설정)
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,           // 서버 정보 접근
    GatewayIntentBits.GuildMessages,    // 메시지 이벤트
    GatewayIntentBits.GuildMembers,     // 멤버 이벤트
    GatewayIntentBits.MessageContent    // 메시지 내용 읽기 (필요시)
  ]
})

// 명령어 저장소
client.commands = new Collection()

// 봇이 준비되면
client.once('ready', () => {
  console.log(`✅ 봇이 ${client.user.tag}으로 온라인입니다!`)
  console.log(`📊 ${client.guilds.cache.size}개 서버에서 활동 중`)

  // 상태 메시지 설정
  client.user.setActivity('명령어 대기 중', { type: 'WATCHING' })
})

// 에러 핸들링
client.on('error', (error) => {
  console.error('❌ 클라이언트 에러:', error)
})

process.on('unhandledRejection', (error) => {
  console.error('❌ 처리되지 않은 거부:', error)
})

// 토큰으로 로그인
client.login(process.env.DISCORD_TOKEN)
```

**.env 파일:**

```env
# .env - 절대 Git에 올리지 마세요!
DISCORD_TOKEN=여기에_봇_토큰_입력
CLIENT_ID=여기에_애플리케이션_ID_입력
GUILD_ID=여기에_테스트_서버_ID_입력
```

**.gitignore:**

```gitignore
node_modules/
.env
*.log
```

### Step 4: 첫 번째 명령어 (Ping)

```
> /ping 명령어를 추가해줘.
> 봇의 응답 시간을 밀리초로 보여주는 기능이야.
```

```javascript
// src/commands/ping.js
const { SlashCommandBuilder } = require('discord.js')

module.exports = {
  // 명령어 정의
  data: new SlashCommandBuilder()
    .setName('ping')
    .setDescription('봇의 응답 시간을 확인합니다'),

  // 명령어 실행
  async execute(interaction) {
    // 현재 시간 기록
    const sent = await interaction.reply({
      content: '측정 중...',
      fetchReply: true
    })

    // 응답 시간 계산
    const latency = sent.createdTimestamp - interaction.createdTimestamp
    const apiLatency = Math.round(interaction.client.ws.ping)

    // 결과 표시
    await interaction.editReply(
      `🏓 퐁!\n` +
      `📡 응답 시간: ${latency}ms\n` +
      `💓 API 지연: ${apiLatency}ms`
    )
  }
}
```

### Step 5: 명령어 등록

Discord에 명령어를 등록해야 슬래시 명령어가 보입니다.

```javascript
// src/utils/deploy-commands.js
require('dotenv').config()
const { REST, Routes } = require('discord.js')
const fs = require('fs')
const path = require('path')

// 명령어 파일 읽기
const commands = []
const commandsPath = path.join(__dirname, '../commands')
const commandFiles = fs.readdirSync(commandsPath).filter(file => file.endsWith('.js'))

for (const file of commandFiles) {
  if (file === 'index.js') continue
  const command = require(path.join(commandsPath, file))
  if (command.data) {
    commands.push(command.data.toJSON())
    console.log(`📦 명령어 로드: ${command.data.name}`)
  }
}

// REST API 클라이언트
const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

// 등록 함수
async function deployCommands() {
  try {
    console.log(`🔄 ${commands.length}개 명령어를 등록합니다...`)

    // 길드 명령어로 등록 (즉시 반영, 개발용)
    const data = await rest.put(
      Routes.applicationGuildCommands(
        process.env.CLIENT_ID,
        process.env.GUILD_ID
      ),
      { body: commands }
    )

    console.log(`✅ ${data.length}개 명령어 등록 완료!`)

    // 전역 명령어로 등록하려면 (모든 서버, 1시간 소요)
    // Routes.applicationCommands(process.env.CLIENT_ID)

  } catch (error) {
    console.error('❌ 명령어 등록 실패:', error)
  }
}

deployCommands()
```

### Step 6: 명령어 핸들러

```javascript
// src/events/interactionCreate.js
module.exports = {
  name: 'interactionCreate',

  async execute(interaction) {
    // 슬래시 명령어가 아니면 무시
    if (!interaction.isChatInputCommand()) return

    // 명령어 찾기
    const command = interaction.client.commands.get(interaction.commandName)

    if (!command) {
      console.error(`❌ 명령어 없음: ${interaction.commandName}`)
      return
    }

    // 명령어 실행
    try {
      await command.execute(interaction)
    } catch (error) {
      console.error(`❌ 명령어 실행 오류:`, error)

      const errorMessage = '명령어 실행 중 오류가 발생했습니다.'

      if (interaction.replied || interaction.deferred) {
        await interaction.followUp({ content: errorMessage, ephemeral: true })
      } else {
        await interaction.reply({ content: errorMessage, ephemeral: true })
      }
    }
  }
}
```

> 💡 **비전공자 팁**: `ephemeral: true`는 해당 사용자에게만 보이는 메시지입니다. 에러 메시지나 민감한 정보에 사용합니다.

---

## 따라해보세요: 최소 동작 예제

복잡한 기능을 만들기 전에, 봇이 간단한 명령에 응답하는지 확인해봅시다:

**1. 최소한의 봇 파일 (`bot.js`) 만들기:**

```javascript
// bot.js - 가장 간단한 Discord 봇
require('dotenv').config()
const {
  Client,
  GatewayIntentBits,
  REST,
  Routes,
  SlashCommandBuilder
} = require('discord.js')

// 클라이언트 생성
const client = new Client({
  intents: [GatewayIntentBits.Guilds]
})

// 봇이 준비되면
client.once('ready', () => {
  console.log(`✅ 봇이 ${client.user.tag}으로 온라인 상태입니다!`)
})

// 슬래시 명령어에 응답
client.on('interactionCreate', async interaction => {
  if (!interaction.isChatInputCommand()) return

  if (interaction.commandName === 'ping') {
    await interaction.reply('🏓 퐁! 저 살아있어요!')
  }
})

// 토큰으로 로그인
client.login(process.env.DISCORD_TOKEN)
```

**2. `.env` 파일 만들기:**

```env
DISCORD_TOKEN=여기에_봇_토큰_입력
CLIENT_ID=여기에_애플리케이션_ID_입력
GUILD_ID=여기에_테스트_서버_ID_입력
```

**3. 슬래시 명령어 등록 (한 번만 실행):**

```javascript
// register-commands.js
require('dotenv').config()
const { REST, Routes, SlashCommandBuilder } = require('discord.js')

const commands = [
  new SlashCommandBuilder()
    .setName('ping')
    .setDescription('봇이 살아있는지 확인합니다')
].map(cmd => cmd.toJSON())

const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

rest.put(
  Routes.applicationGuildCommands(process.env.CLIENT_ID, process.env.GUILD_ID),
  { body: commands }
)
.then(() => console.log('✅ 명령어 등록 완료!'))
.catch(console.error)
```

**4. 실행:**

```bash
npm install discord.js dotenv
node register-commands.js  # 한 번만 실행
node bot.js                # 봇 시작
```

"봇이 온라인 상태입니다"가 보이고 Discord에서 `/ping`이 작동하면, 전체 프로젝트를 할 준비가 된 것입니다.

---

## 더 많은 명령어 만들기

### 주사위 명령어

```
> /dice 명령어를 추가해줘.
> 옵션으로 면 수를 받을 수 있게 하고 (기본값 6),
> 주사위 굴린 결과를 보여줘.
```

```javascript
// src/commands/dice.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')

module.exports = {
  data: new SlashCommandBuilder()
    .setName('dice')
    .setDescription('주사위를 굴립니다')
    .addIntegerOption(option =>
      option
        .setName('sides')
        .setDescription('주사위 면의 개수')
        .setMinValue(2)
        .setMaxValue(100)
    )
    .addIntegerOption(option =>
      option
        .setName('count')
        .setDescription('굴릴 주사위 개수')
        .setMinValue(1)
        .setMaxValue(10)
    ),

  async execute(interaction) {
    const sides = interaction.options.getInteger('sides') ?? 6
    const count = interaction.options.getInteger('count') ?? 1

    // 주사위 굴리기
    const rolls = []
    for (let i = 0; i < count; i++) {
      rolls.push(Math.floor(Math.random() * sides) + 1)
    }

    const total = rolls.reduce((a, b) => a + b, 0)

    // 예쁜 임베드로 결과 표시
    const embed = new EmbedBuilder()
      .setColor(0x00AE86)
      .setTitle('🎲 주사위 굴리기')
      .setDescription(`${sides}면체 주사위 ${count}개를 굴렸습니다!`)
      .addFields(
        { name: '결과', value: rolls.join(' + '), inline: true },
        { name: '합계', value: `**${total}**`, inline: true }
      )
      .setFooter({ text: `요청자: ${interaction.user.tag}` })
      .setTimestamp()

    await interaction.reply({ embeds: [embed] })
  }
}
```

> 🔥 **프로 팁**: `EmbedBuilder`를 사용하면 예쁜 카드 형태의 메시지를 보낼 수 있습니다. 색상, 이미지, 필드 등을 추가할 수 있어요.

### 투표 명령어

```
> /poll 명령어를 만들어줘.
> - 질문과 선택지 2-4개를 입력받아
> - 예쁜 임베드로 표시하고
> - 각 선택지에 자동으로 이모지 반응을 달아줘
```

```javascript
// src/commands/poll.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')

const EMOJIS = ['1️⃣', '2️⃣', '3️⃣', '4️⃣']

module.exports = {
  data: new SlashCommandBuilder()
    .setName('poll')
    .setDescription('투표를 만듭니다')
    .addStringOption(option =>
      option
        .setName('question')
        .setDescription('투표 질문')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('option1')
        .setDescription('선택지 1')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('option2')
        .setDescription('선택지 2')
        .setRequired(true)
    )
    .addStringOption(option =>
      option.setName('option3').setDescription('선택지 3')
    )
    .addStringOption(option =>
      option.setName('option4').setDescription('선택지 4')
    ),

  async execute(interaction) {
    const question = interaction.options.getString('question')
    const options = [
      interaction.options.getString('option1'),
      interaction.options.getString('option2'),
      interaction.options.getString('option3'),
      interaction.options.getString('option4')
    ].filter(Boolean)  // null/undefined 제거

    // 선택지 텍스트 생성
    const optionText = options
      .map((opt, i) => `${EMOJIS[i]} ${opt}`)
      .join('\n\n')

    // 임베드 생성
    const embed = new EmbedBuilder()
      .setColor(0x5865F2)
      .setTitle(`📊 ${question}`)
      .setDescription(optionText)
      .setFooter({
        text: `투표 생성자: ${interaction.user.tag}`,
        iconURL: interaction.user.displayAvatarURL()
      })
      .setTimestamp()

    // 투표 메시지 전송
    const pollMessage = await interaction.reply({
      embeds: [embed],
      fetchReply: true
    })

    // 자동으로 반응 추가
    for (let i = 0; i < options.length; i++) {
      await pollMessage.react(EMOJIS[i])
    }
  }
}
```

### 알림 명령어

```
> /remind 명령어를 만들어줘.
> - 몇 분 후에 알림을 받을지 입력
> - 알림 내용 입력
> - 시간이 되면 그 사용자를 멘션해서 알려줘
```

```javascript
// src/commands/remind.js
const { SlashCommandBuilder } = require('discord.js')

// 활성 타이머 저장 (메모리에만 저장 - 재시작시 초기화됨)
const activeReminders = new Map()

module.exports = {
  data: new SlashCommandBuilder()
    .setName('remind')
    .setDescription('알림을 설정합니다')
    .addIntegerOption(option =>
      option
        .setName('minutes')
        .setDescription('몇 분 후에 알림을 받을까요?')
        .setRequired(true)
        .setMinValue(1)
        .setMaxValue(60 * 24)  // 최대 24시간
    )
    .addStringOption(option =>
      option
        .setName('message')
        .setDescription('알림 내용')
        .setRequired(true)
    ),

  async execute(interaction) {
    const minutes = interaction.options.getInteger('minutes')
    const message = interaction.options.getString('message')
    const userId = interaction.user.id
    const channelId = interaction.channelId

    // 현재 시간 + 분
    const remindTime = new Date(Date.now() + minutes * 60 * 1000)
    const timeString = remindTime.toLocaleTimeString('ko-KR')

    // 타이머 설정
    const timerId = setTimeout(async () => {
      try {
        const channel = await interaction.client.channels.fetch(channelId)
        await channel.send(`⏰ <@${userId}> 알림!\n\n📝 ${message}`)
        activeReminders.delete(timerId)
      } catch (error) {
        console.error('알림 전송 실패:', error)
      }
    }, minutes * 60 * 1000)

    activeReminders.set(timerId, { userId, message, remindTime })

    // 확인 메시지
    await interaction.reply({
      content: `✅ 알림이 설정되었습니다!\n\n` +
               `⏰ 시간: ${timeString} (${minutes}분 후)\n` +
               `📝 내용: ${message}`,
      ephemeral: true  // 본인에게만 보임
    })
  }
}
```

---

## 이벤트 기반 기능

슬래시 커맨드 외에도 다양한 이벤트에 반응할 수 있습니다.

### 환영 메시지

```
> 새로운 멤버가 서버에 들어오면
> #welcome 채널에 환영 메시지를 보내줘.
> 그 사람 프로필 사진과 함께 예쁜 임베드로.
```

```javascript
// src/events/guildMemberAdd.js
const { EmbedBuilder } = require('discord.js')

module.exports = {
  name: 'guildMemberAdd',

  async execute(member) {
    // 환영 채널 찾기 (이름이 'welcome'인 채널)
    const welcomeChannel = member.guild.channels.cache.find(
      channel => channel.name === 'welcome'
    )

    if (!welcomeChannel) {
      console.log('환영 채널을 찾을 수 없습니다')
      return
    }

    // 서버 멤버 수
    const memberCount = member.guild.memberCount

    // 환영 임베드
    const embed = new EmbedBuilder()
      .setColor(0x57F287)
      .setTitle('🎉 새로운 멤버가 왔어요!')
      .setDescription(
        `${member}님, **${member.guild.name}**에 오신 것을 환영합니다!\n\n` +
        `이제 저희는 **${memberCount}**명이에요! 🎊`
      )
      .setThumbnail(member.user.displayAvatarURL({ dynamic: true, size: 256 }))
      .addFields(
        { name: '📅 가입일', value: `<t:${Math.floor(Date.now() / 1000)}:R>`, inline: true },
        { name: '🆔 ID', value: member.id, inline: true }
      )
      .setFooter({ text: '규칙을 읽고 인사해주세요!' })
      .setTimestamp()

    await welcomeChannel.send({ embeds: [embed] })

    // 선택: 자동으로 역할 부여
    try {
      const defaultRole = member.guild.roles.cache.find(role => role.name === 'Member')
      if (defaultRole) {
        await member.roles.add(defaultRole)
      }
    } catch (error) {
      console.error('역할 부여 실패:', error)
    }
  }
}
```

> ⚠️ **주의사항**: 멤버 이벤트를 사용하려면 봇에게 `SERVER MEMBERS INTENT` 권한이 필요합니다. Discord Developer Portal에서 활성화해야 해요.

### 자동 반응

```
> 메시지에 특정 단어가 포함되어 있으면
> 자동으로 이모지 반응을 달아줘.
```

```javascript
// src/events/messageCreate.js
module.exports = {
  name: 'messageCreate',

  async execute(message) {
    // 봇 메시지 무시
    if (message.author.bot) return

    // 자동 반응 규칙
    const reactions = {
      '축하': '🎉',
      'gg': '👏',
      'ㅋㅋㅋ': '😂',
      '좋아': '❤️',
      '슬프': '😢',
      'nice': '👍',
      '커피': '☕'
    }

    const content = message.content.toLowerCase()

    for (const [keyword, emoji] of Object.entries(reactions)) {
      if (content.includes(keyword)) {
        try {
          await message.react(emoji)
        } catch (error) {
          console.error('반응 추가 실패:', error)
        }
      }
    }

    // 봇이 멘션되면 응답
    if (message.mentions.has(message.client.user)) {
      await message.reply('무엇을 도와드릴까요? `/help`를 입력해보세요!')
    }
  }
}
```

---

## 실용적인 봇 예시

### 서버 관리 봇

```
> 서버 관리 봇을 만들어줘. 다음 명령어들이 필요해:
>
> /kick [유저] [사유]
> - 관리자만 사용 가능
> - 해당 유저를 서버에서 추방
>
> /clear [개수]
> - 관리자만 사용 가능
> - 최근 메시지 N개 삭제
>
> /serverinfo
> - 누구나 사용 가능
> - 서버 정보 (멤버 수, 생성일 등) 표시
```

```javascript
// src/commands/kick.js
const { SlashCommandBuilder, PermissionFlagsBits } = require('discord.js')

module.exports = {
  data: new SlashCommandBuilder()
    .setName('kick')
    .setDescription('유저를 서버에서 추방합니다')
    .addUserOption(option =>
      option
        .setName('user')
        .setDescription('추방할 유저')
        .setRequired(true)
    )
    .addStringOption(option =>
      option
        .setName('reason')
        .setDescription('추방 사유')
    )
    // 관리자 권한 필요
    .setDefaultMemberPermissions(PermissionFlagsBits.KickMembers),

  async execute(interaction) {
    const targetUser = interaction.options.getUser('user')
    const reason = interaction.options.getString('reason') ?? '사유 없음'

    const member = await interaction.guild.members.fetch(targetUser.id)

    // 권한 체크
    if (!member.kickable) {
      return interaction.reply({
        content: '❌ 이 유저를 추방할 수 없습니다. (권한 부족)',
        ephemeral: true
      })
    }

    try {
      await member.kick(reason)
      await interaction.reply(`✅ ${targetUser.tag}님이 추방되었습니다.\n📝 사유: ${reason}`)
    } catch (error) {
      await interaction.reply({
        content: '❌ 추방 중 오류가 발생했습니다.',
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
    .setDescription('메시지를 일괄 삭제합니다')
    .addIntegerOption(option =>
      option
        .setName('count')
        .setDescription('삭제할 메시지 개수 (1-100)')
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
      await interaction.editReply(`✅ ${deleted.size}개 메시지를 삭제했습니다.`)
    } catch (error) {
      await interaction.editReply('❌ 14일 이상 된 메시지는 삭제할 수 없습니다.')
    }
  }
}
```

### 미니 경제 시스템

```
> 미니 경제 시스템이 있는 봇을 만들어줘.
>
> /balance - 내 잔액 확인
> /daily - 하루에 한 번 100코인 받기
> /give [유저] [금액] - 다른 유저에게 송금
>
> 데이터는 JSON 파일로 저장해줘.
```

```javascript
// src/commands/daily.js
const { SlashCommandBuilder, EmbedBuilder } = require('discord.js')
const fs = require('fs')
const path = require('path')

const DATA_PATH = path.join(__dirname, '../../data/economy.json')

// 데이터 로드/저장 함수
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
const COOLDOWN_MS = 24 * 60 * 60 * 1000  // 24시간

module.exports = {
  data: new SlashCommandBuilder()
    .setName('daily')
    .setDescription('일일 보상을 받습니다 (100코인)'),

  async execute(interaction) {
    const userId = interaction.user.id
    const data = loadData()

    // 유저 데이터 초기화
    if (!data[userId]) {
      data[userId] = { balance: 0, lastDaily: 0 }
    }

    const userData = data[userId]
    const now = Date.now()
    const timeSinceLastDaily = now - userData.lastDaily

    // 쿨다운 체크
    if (timeSinceLastDaily < COOLDOWN_MS) {
      const remaining = COOLDOWN_MS - timeSinceLastDaily
      const hours = Math.floor(remaining / (60 * 60 * 1000))
      const minutes = Math.floor((remaining % (60 * 60 * 1000)) / (60 * 1000))

      return interaction.reply({
        content: `⏰ 다음 보상까지 ${hours}시간 ${minutes}분 남았습니다!`,
        ephemeral: true
      })
    }

    // 보상 지급
    userData.balance += DAILY_AMOUNT
    userData.lastDaily = now
    saveData(data)

    const embed = new EmbedBuilder()
      .setColor(0xFFD700)
      .setTitle('💰 일일 보상!')
      .setDescription(
        `**+${DAILY_AMOUNT}** 코인을 받았습니다!\n\n` +
        `현재 잔액: **${userData.balance}** 코인`
      )
      .setFooter({ text: '내일 다시 오세요!' })

    await interaction.reply({ embeds: [embed] })
  }
}
```

---

## Slack 봇으로 확장하기

Discord에서 익힌 패턴은 Slack에도 적용됩니다.

### 핵심 개념은 동일

| 개념 | Discord | Slack |
|------|---------|-------|
| 라이브러리 | discord.js | @slack/bolt |
| 명령어 | Slash Commands | Slash Commands |
| 이벤트 반응 | client.on('event') | app.event('event') |
| 메시지 응답 | interaction.reply() | say() |
| 토큰 | DISCORD_TOKEN | SLACK_BOT_TOKEN |

### Slack 봇 시작하기

```
> Slack 봇 프로젝트를 만들어줘.
> Bolt 프레임워크를 사용하고,
> /ping 명령어에 응답하는 기본 봇부터.
> Slack 앱 설정 방법도 알려줘.
```

```javascript
// app.js - Slack 봇
const { App } = require('@slack/bolt')
require('dotenv').config()

const app = new App({
  token: process.env.SLACK_BOT_TOKEN,
  signingSecret: process.env.SLACK_SIGNING_SECRET,
  socketMode: true,
  appToken: process.env.SLACK_APP_TOKEN
})

// 슬래시 명령어
app.command('/ping', async ({ command, ack, say }) => {
  await ack()
  await say(`🏓 퐁! 응답 시간: ${Date.now() - command.event_ts * 1000}ms`)
})

// 메시지 이벤트
app.message('안녕', async ({ message, say }) => {
  await say(`안녕하세요, <@${message.user}>님!`)
})

// 앱 멘션
app.event('app_mention', async ({ event, say }) => {
  await say(`네, <@${event.user}>님! 무엇을 도와드릴까요?`)
});

// 앱 시작
(async () => {
  await app.start(process.env.PORT || 3000)
  console.log('⚡ Slack 봇이 시작되었습니다!')
})()
```

### 업무 자동화 예시

```
> Slack 봇에 다음 기능을 추가해줘:
>
> /standup 명령어
> - 오늘 할 일, 어제 한 일, 막힌 점을 입력받는 모달 띄우기
> - 입력하면 #standup 채널에 예쁘게 정리해서 올리기
```

---

## 봇 배포하기

로컬에서만 실행하면 컴퓨터를 끄면 봇도 꺼집니다. 24시간 운영하려면 배포가 필요합니다.

### 무료 배포 옵션

| 서비스 | 특징 | 비용 |
|--------|------|------|
| Railway | 설정 쉬움, GitHub 연동 | $5 무료 크레딧/월 |
| Render | 무료 티어, 비활성 시 슬립 | 무료 (750시간/월) |
| Fly.io | Docker 기반, 무료 티어 | 무료 (제한적) |
| Replit | 코드 에디터 포함 | 무료 (제한적) |
| 개인 서버 | 완전 제어 | VPS 비용 |

### Railway 배포

```
> 이 Discord 봇을 Railway에 배포하는 방법 알려줘.
> 환경변수 설정하는 방법도.
```

**단계:**

1. Railway 가입 (https://railway.app)
2. GitHub 연동
3. "New Project" → "Deploy from GitHub repo"
4. 저장소 선택
5. 환경변수 설정:
   - `DISCORD_TOKEN`
   - `CLIENT_ID`
6. 배포 확인

### package.json 설정

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

> 💡 **비전공자 팁**: `engines` 필드를 설정하면 배포 플랫폼이 올바른 Node.js 버전을 사용합니다.

---

## 📝 실습 과제

### 레벨 1: 기본 (초보자)

여러분만의 Discord 봇을 만들어보세요:

```
> 다음 기능이 있는 Discord 봇을 만들어줘:
>
> 1. /quote - 랜덤 명언 보여주기
> 2. /choose [선택지들] - 여러 선택지 중 하나 골라주기
> 3. /8ball [질문] - 마법의 8볼처럼 랜덤 답변
>
> 기본 구조부터 시작해서 하나씩 추가해줘.
```

**체크리스트:**
- [ ] 봇이 온라인 상태인가?
- [ ] 슬래시 명령어가 자동완성되는가?
- [ ] 각 명령어가 올바르게 응답하는가?

### 레벨 2: 응용 (중급자)

```
> 할 일 관리 봇을 만들어줘.
> - /todo add [내용] - 할 일 추가
> - /todo list - 내 할 일 목록 보기
> - /todo done [번호] - 완료 표시
> - 데이터는 JSON으로 저장
```

**체크리스트:**
- [ ] 할 일이 추가되는가?
- [ ] 목록이 예쁘게 표시되는가?
- [ ] 봇 재시작 후에도 데이터가 유지되는가?

### 레벨 3: 도전 (심화)

```
> 음악 추천 봇을 만들어줘.
> /mood [기분] 명령어로 기분을 입력하면
> 그 기분에 맞는 노래 장르나 분위기를 추천해줘.
> (실제 음악 재생은 복잡하니까 추천만)
```

---

## 🏆 도전 과제

### 도전 1: 레벨 시스템

메시지를 보낼 때마다 XP 획득, 레벨업 알림:

```javascript
// 메시지당 10~20 XP
// 레벨업 공식: 필요 XP = 레벨 * 100
// 레벨업 시 축하 메시지
```

### 도전 2: 역할 메뉴

반응으로 역할 부여/제거:

```
/roles 명령어로 역할 선택 메시지 생성
🎮 → 게이머 역할
🎨 → 아티스트 역할
💻 → 개발자 역할
```

### 도전 3: 웹 대시보드

봇 설정을 웹에서 관리:

```
- Express.js 서버
- Discord OAuth2 로그인
- 서버별 설정 페이지
```

---

## 안 되면? 문제 해결 팁

### 봇은 온라인인데 명령어에 응답하지 않음

```bash
# 가장 흔한 문제: 명령어가 Discord에 등록되지 않음
node src/utils/deploy-commands.js

# Discord가 명령어를 전파하는 데 1-2분 정도 기다리세요
```

### "Missing Access" 또는 "Missing Permissions" 오류

1. Discord Developer Portal > 내 앱 > OAuth2 > URL Generator로 이동
2. `bot`과 `applications.commands` 스코프 선택
3. 필요한 권한 선택 (메시지 보내기, 슬래시 명령어 사용 등)
4. 생성된 URL로 봇을 서버에 다시 초대

### "Invalid token" 오류

```bash
# .env 파일을 확인하세요
# 여분의 공백이나 따옴표가 없어야 합니다
DISCORD_TOKEN=your_token_here  # 맞음
DISCORD_TOKEN="your_token_here"  # 틀림 - 따옴표 없이!
DISCORD_TOKEN= your_token_here   # 틀림 - 공백 없이!
```

### 로컬에서는 응답하는데 배포 후에는 안 됨

```bash
# 호스팅 플랫폼에서 환경변수가 설정되어 있는지 확인
# Railway/Render/Heroku 모두 자체 환경변수 설정이 있음

# 토큰이 읽히는지 테스트
console.log('토큰 존재:', !!process.env.DISCORD_TOKEN)
```

### 명령어 업데이트가 너무 오래 걸림

- **글로벌 명령어** (applicationCommands): 전 세계 업데이트에 최대 1시간 소요
- **길드 명령어** (applicationGuildCommands): 즉시 업데이트되지만 해당 서버에서만

개발 중에는 더 빠른 테스트를 위해 길드 명령어를 사용하시기 바랍니다.

---

## 자주 하는 실수

### 1. 봇 토큰 노출

```javascript
// ❌ 절대 이렇게 하지 마세요 - 토큰이 모든 사람에게 보여요!
client.login('MTIzNDU2Nzg5MDEyMzQ1Njc4.XXXXX.YYYYY')

// ✅ 항상 환경변수 사용
client.login(process.env.DISCORD_TOKEN)
```

실수로 토큰을 GitHub에 커밋했다면, Discord Developer Portal에서 **즉시 재생성**하시기 바랍니다.

### 2. Intents 설정을 빼먹음

```javascript
// ❌ 틀림 - 봇이 메시지 이벤트를 받지 못함
const client = new Client({ intents: [] })

// ✅ 맞음 - 필요한 이벤트 지정
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent  // 메시지 내용 읽기에 필요
  ]
})
```

### 3. Interaction 응답 처리를 잘못함

```javascript
// ❌ 틀림 - 두 번 응답하려고 하면 크래시
await interaction.reply('첫 번째 응답')
await interaction.reply('두 번째 응답')  // 오류!

// ✅ 맞음 - 추가 메시지는 followUp 사용
await interaction.reply('첫 번째 응답')
await interaction.followUp('두 번째 응답')

// 또는 지연된 응답은 editReply 사용
await interaction.deferReply()
// ... 처리 중 ...
await interaction.editReply('완료!')
```

### 4. 명령어가 등록되지 않음

```javascript
// CLIENT_ID와 DISCORD_TOKEN 둘 다 있는지 확인
// CLIENT_ID는 Discord Developer Portal의 Application ID
Routes.applicationCommands(process.env.CLIENT_ID)

// 올바른 Routes를 사용하고 있는지도 확인:
// - applicationCommands: 글로벌 명령어 (모든 서버, 업데이트 느림)
// - applicationGuildCommands: 길드 명령어 (한 서버, 즉시 업데이트)
```

### 5. 터미널 닫으면 봇이 오프라인됨

터미널을 닫으면 봇이 멈춥니다. 24시간 운영하려면:
- 호스팅 서비스 사용 (Railway, Render, Fly.io)
- 또는 PM2 같은 프로세스 관리자 사용: `pm2 start src/index.js`

### 6. 비동기 처리를 안 함

```javascript
// ❌ 틀림 - await 없으면 에러 발생 가능
async execute(interaction) {
  interaction.reply('응답')  // await 없음!
}

// ✅ 맞음 - 항상 await 사용
async execute(interaction) {
  await interaction.reply('응답')
}
```

---

## 용어 정리

| 용어 | 의미 |
|------|------|
| Intent | 봇이 받을 이벤트 종류 설정 |
| Slash Command | `/`로 시작하는 공식 명령어 |
| Embed | 카드 형태의 예쁜 메시지 |
| Guild | Discord 서버 (내부 용어) |
| Interaction | 사용자와 봇의 상호작용 |
| Token | 봇 인증용 비밀 키 |
| Client | 봇 프로그램 자체 |
| ephemeral | 본인에게만 보이는 메시지 |

---

## 다음 챕터 미리보기

챗봇을 만들었으니, 이제 **풀스택 앱**을 만들어봅니다!

- **Chapter 19**: 풀스택 앱 만들기
  - 프론트엔드 (React)
  - 백엔드 (Express)
  - 데이터베이스 (SQLite)

챗봇에서 배운 이벤트 처리, 비동기 프로그래밍은 풀스택 개발에서도 그대로 적용됩니다!

---

## 정리

이번 챕터에서 배운 것:
- [x] Discord 봇 프로젝트 시작하기
- [x] 슬래시 커맨드 추가하기
- [x] Embed로 예쁜 메시지 만들기
- [x] 이벤트에 반응하기
- [x] Slack 봇으로 패턴 확장
- [x] 봇 배포하기

챗봇의 핵심은 **"이 이벤트가 발생하면 이렇게 응답한다"**입니다. 이 패턴을 이해하면 어떤 플랫폼에서든 봇을 만들 수 있습니다.

Claude에게 요청할 때는:
1. 어떤 명령어/이벤트에 반응할지
2. 어떤 입력을 받을지
3. 어떤 출력을 할지

이 세 가지를 명확히 하면 원하는 봇을 만들 수 있습니다.

[Chapter 19: 풀스택 앱 만들기](../Chapter19-Backend-Basics/README.ko.md)로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Discord.js 공식 가이드](https://discordjs.guide/) - Discord 봇 개발 가이드
- [Discord Developer Portal](https://discord.com/developers/docs/intro) - Discord API 공식 문서
- [Slack Bolt 프레임워크](https://slack.dev/bolt-js/concepts) - Slack 봇 개발 가이드

**영상 자료:**
- [Discord 봇 만들기 튜토리얼 (YouTube)](https://www.youtube.com/results?search_query=discord.js+bot+tutorial+2024)
- [Slack 봇 개발 (YouTube)](https://www.youtube.com/results?search_query=slack+bolt+bot+tutorial)
- [Discord 슬래시 커맨드 (YouTube)](https://www.youtube.com/results?search_query=discord+slash+commands+tutorial)

**읽을거리:**
- [이벤트 기반 프로그래밍 이해](https://www.freecodecamp.org/news/event-driven-programming/) - 이벤트 드리븐 패턴
- [봇 설계 베스트 프랙티스](https://discord.com/developers/docs/topics/community-resources) - Discord 커뮤니티 리소스

**관련 도구:**
- [Discord.js](https://discord.js.org/) - Discord 봇 라이브러리
- [Slack Bolt](https://slack.dev/bolt-js/) - Slack 봇 프레임워크
- [Railway](https://railway.app/) - 봇 배포 플랫폼
- [Render](https://render.com/) - 무료 봇 호스팅

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
