# Chapter 16: 챗봇 만들기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- Discord 봇을 Claude와 함께 만드는 과정
- 봇 기능을 점진적으로 확장하는 방법
- Slack 봇으로 패턴 확장하기

---

## 왜 필요합니까?

**챗봇이 빛나는 실제 상황들:**

- **게임 커뮤니티 관리** - 봇이 새 멤버 환영, 채팅 관리, 게임 나이트 투표를 할 수 있습니다
- **팀 워크플로우 자동화** - 일일 스탠드업 알림, 자주 묻는 질문 자동 응답, 미팅 스케줄링
- **참여도 높이기** - 미니게임, 퀴즈, 포인트 시스템으로 서버를 더 활발하게 만들 수 있습니다
- **개인 생산성** - 할 일 알림, 북마크 저장, 습관 추적을 도와주는 봇

챗봇을 한 번 만들면 **24시간 연중무휴**로 일합니다. 여러분이 자는 동안에도 작동합니다.

---

## 쉬운 비유: 봇은 친절한 매장 직원과 같습니다

매장에 들어간다고 상상해 보시기 바랍니다:
- **직원 없이**: 모든 것을 직접 찾아야 하고, 어디에 무엇이 있는지 헤매야 합니다
- **친절한 직원과 함께**: 인사해주고, 질문에 답하고, 원하는 것을 안내해줍니다

챗봇도 마찬가지입니다:
- **봇 없이**: 같은 질문이 반복되고, 관리자가 24시간 접속해 있어야 합니다
- **봇과 함께**: 즉각적인 응답, 자동 관리, 모든 사람에게 일관된 경험을 제공합니다

봇은 절대 쉬지 않는 가상 비서입니다.

---

## 왜 챗봇입니까?

챗봇은 실제로 사용할 수 있는 결과물이 나오는 프로젝트입니다:
- 여러분의 Discord 서버에서 바로 사용
- 팀 Slack에서 업무 자동화
- 24시간 동작하는 자동화 도구

**챗봇 요청 팁:**

```
> Discord 봇을 만들어줘.
> /hello 명령어를 입력하면 "안녕하세요!"라고 응답하고,
> /dice 명령어를 입력하면 1-6 사이 랜덤 숫자를 보여줘.
```

명령어 이름, 트리거 조건, 응답 내용을 명확히 설명하면 원하는 봇을 얻을 수 있습니다.

---

## 프로젝트: Discord 봇 만들기

### Step 1: 사전 준비 (한 번만 하면 됨)

Discord 개발자 포털에서 봇을 생성해야 합니다. Claude에게 물어보세요:

```
> Discord 봇을 만들려고 해.
> Discord Developer Portal에서 봇을 생성하고
> 서버에 초대하는 방법을 알려줘.
```

Claude가 안내해주는 단계:
1. Discord Developer Portal 접속
2. 새 Application 생성
3. Bot 토큰 발급
4. OAuth2 URL로 서버 초대

**중요**: 토큰은 비밀번호와 같습니다. 절대 코드에 직접 넣지 마세요!

### Step 2: 프로젝트 시작

```
> Discord 봇 프로젝트를 만들어줘.
> discord.js를 사용하고,
> .env 파일로 토큰을 관리하게 해줘.
> 일단 봇이 온라인 되면 콘솔에 메시지만 출력하는 것부터.
```

이 요청으로 Claude가 만들어주는 것:
- 프로젝트 폴더 구조
- package.json
- .env 파일 템플릿
- 기본 봇 코드

### Step 3: 첫 번째 명령어

```
> /ping 명령어를 추가해줘.
> 봇의 응답 시간을 밀리초로 보여주는 기능이야.
```

작동 확인 후 다음 명령어를 추가합니다:

```
> /dice 명령어를 추가해줘.
> 옵션으로 면 수를 받을 수 있게 하고 (기본값 6),
> 주사위 굴린 결과를 보여줘.
```

**요청 팁**: 한 번에 여러 명령어를 요청하지 말고, 하나씩 추가하며 테스트하세요.

---

## 따라해보세요: 최소 동작 예제

복잡한 기능을 만들기 전에, 봇이 간단한 명령에 응답하는지 확인해봅시다:

**1. 최소한의 봇 파일 (`bot.js`) 만들기:**

```javascript
// bot.js - 가장 간단한 Discord 봇
require('dotenv').config()
const { Client, GatewayIntentBits, REST, Routes, SlashCommandBuilder } = require('discord.js')

const client = new Client({ intents: [GatewayIntentBits.Guilds] })

// 봇이 준비되면
client.once('ready', () => {
  console.log(`봇이 ${client.user.tag}로 온라인 상태입니다!`)
})

// /ping 명령어에 응답
client.on('interactionCreate', async interaction => {
  if (!interaction.isChatInputCommand()) return

  if (interaction.commandName === 'ping') {
    await interaction.reply('퐁! 저 살아있어요!')
  }
})

// 토큰으로 로그인
client.login(process.env.DISCORD_TOKEN)
```

**2. `.env` 파일 만들기 (비밀로 유지하세요!):**

```
DISCORD_TOKEN=여기에_봇_토큰_입력
```

**3. 슬래시 명령어 등록 (한 번만 실행):**

```javascript
// register-commands.js - /ping을 등록하려면 한 번만 실행
require('dotenv').config()
const { REST, Routes, SlashCommandBuilder } = require('discord.js')

const commands = [
  new SlashCommandBuilder().setName('ping').setDescription('봇이 살아있는지 확인')
].map(cmd => cmd.toJSON())

const rest = new REST({ version: '10' }).setToken(process.env.DISCORD_TOKEN)

rest.put(Routes.applicationCommands(process.env.CLIENT_ID), { body: commands })
  .then(() => console.log('명령어 등록 완료!'))
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

## 기능 확장하기

### 투표 기능

```
> /poll 명령어를 만들어줘.
> - 질문과 선택지 2-4개를 입력받아
> - 예쁜 임베드로 표시하고
> - 각 선택지에 자동으로 이모지 반응을 달아줘
```

### 알림 기능

```
> /remind 명령어를 만들어줘.
> - 몇 분 후에 알림을 받을지 입력
> - 알림 내용 입력
> - 시간이 되면 그 사용자를 멘션해서 알려줘
```

### 유틸리티

```
> /avatar 명령어를 만들어줘.
> 유저를 선택하면 그 유저의 프로필 이미지를 크게 보여줘.
> 아무도 선택 안 하면 자기 자신 이미지.
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

### 메시지 로깅

```
> 메시지가 삭제되면 #logs 채널에 기록해줘.
> 누가 어떤 메시지를 삭제했는지 보여주고,
> 봇의 메시지는 제외해.
```

### 자동 반응

```
> 메시지에 "ㅋㅋㅋ"가 포함되어 있으면
> 😂 이모지로 반응해줘.
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

### 미니 경제 시스템

```
> 미니 경제 시스템이 있는 봇을 만들어줘.
>
> /balance - 내 잔액 확인
> /daily - 하루에 한 번 100코인 받기
> /give [유저] [금액] - 다른 유저에게 송금
>
> 데이터는 JSON 파일로 저장해줘.
> 유저 ID를 키로, 잔액을 값으로.
```

---

## Slack 봇으로 확장하기

Discord에서 익힌 패턴은 Slack에도 적용됩니다.

### 핵심 개념은 동일

| 개념 | Discord | Slack |
|------|---------|-------|
| 명령어 | /ping | /ping |
| 이벤트 반응 | client.on('event') | app.event('event') |
| 메시지 응답 | interaction.reply() | say() |

### Slack 봇 시작하기

```
> Slack 봇 프로젝트를 만들어줘.
> Bolt 프레임워크를 사용하고,
> /ping 명령어에 응답하는 기본 봇부터.
> Slack 앱 설정 방법도 알려줘.
```

### 업무 자동화 예시

```
> Slack 봇에 다음 기능을 추가해줘:
>
> /standup 명령어
> - 오늘 할 일, 어제 한 일, 막힌 점을 입력받는 모달 띄우기
> - 입력하면 #standup 채널에 예쁘게 정리해서 올리기
```

```
> 봇이 멘션되면 자동으로 응답하게 해줘.
> "회의실 예약" 키워드가 있으면 회의실 예약 링크 안내,
> 그 외에는 "무엇을 도와드릴까요?" 응답.
```

---

## 봇 배포하기

로컬에서만 실행하면 컴퓨터를 끄면 봇도 꺼집니다. 24시간 운영하려면 배포가 필요합니다.

```
> 이 Discord 봇을 Railway에 배포하는 방법 알려줘.
> 환경변수 설정하는 방법도.
```

### 무료 배포 옵션

| 서비스 | 특징 |
|--------|------|
| Railway | $5 무료 크레딧/월, 설정 쉬움 |
| Render | 무료, 비활성 시 슬립 |
| Fly.io | 무료 티어 있음 |

---

## 디버깅 팁

봇이 응답하지 않을 때:

```
> 봇이 명령어에 응답하지 않아.
> 콘솔에는 에러가 없고, 봇은 온라인 상태야.
> 뭐가 문제일 수 있을까?
```

Claude가 체크해줄 것들:
- 슬래시 커맨드가 Discord에 등록되었는지
- 봇 권한 설정
- Intents 설정
- 토큰이 올바른지

---

## 실습 과제

### 기본 과제

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

### 심화 과제

```
> 음악 추천 봇을 만들어줘.
> /mood [기분] 명령어로 기분을 입력하면
> 그 기분에 맞는 노래 장르나 분위기를 추천해줘.
> (실제 음악 재생은 복잡하니까 추천만)
```

```
> 할 일 관리 봇을 만들어줘.
> - /todo add [내용] - 할 일 추가
> - /todo list - 내 할 일 목록 보기
> - /todo done [번호] - 완료 표시
> - 데이터는 JSON으로 저장
```

---

## 안 되면? 문제 해결 팁

### 봇은 온라인인데 명령어에 응답하지 않음

```bash
# 가장 흔한 문제: 명령어가 Discord에 등록되지 않음
# register-commands.js 스크립트를 다시 실행하세요
node register-commands.js

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
// 절대 이렇게 하지 마세요 - 토큰이 모든 사람에게 보여요!
client.login('MTIzNDU2Nzg5MDEyMzQ1Njc4.XXXXX.YYYYY')

// 항상 환경변수 사용
client.login(process.env.DISCORD_TOKEN)
```

실수로 토큰을 GitHub에 커밋했다면, Discord Developer Portal에서 **즉시 재생성**하시기 바랍니다.

### 2. Intents 설정을 빼먹음

```javascript
// 틀림 - 봇이 메시지 이벤트를 받지 못함
const client = new Client({ intents: [] })

// 맞음 - 필요한 이벤트 지정
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
// 틀림 - 두 번 응답하려고 하면 크래시
await interaction.reply('첫 번째 응답')
await interaction.reply('두 번째 응답')  // 오류!

// 맞음 - 추가 메시지는 followUp 사용
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
- 또는 PM2 같은 프로세스 관리자 사용: `pm2 start bot.js`

---

## 정리

이번 챕터에서 배운 것:
- [x] Discord 봇 프로젝트 시작하기
- [x] 슬래시 커맨드 추가하기
- [x] 이벤트에 반응하기
- [x] Slack 봇으로 패턴 확장
- [x] 봇 배포하기

챗봇의 핵심은 **"이 이벤트가 발생하면 이렇게 응답한다"**입니다. 이 패턴을 이해하면 어떤 플랫폼에서든 봇을 만들 수 있습니다.

Claude에게 요청할 때는:
1. 어떤 명령어/이벤트에 반응할지
2. 어떤 입력을 받을지
3. 어떤 출력을 할지

이 세 가지를 명확히 하면 원하는 봇을 만들 수 있습니다.

다음 챕터에서는 프론트엔드와 백엔드를 모두 갖춘 풀스택 앱을 만들어봅니다.

[Chapter 17: 풀스택 앱 만들기](../Chapter17/README.ko.md)로 넘어가세요.
