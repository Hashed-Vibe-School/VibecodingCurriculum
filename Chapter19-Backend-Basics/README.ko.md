# Chapter 19: 백엔드 기초 - 서버와 데이터베이스

[English](./README.md) | **한국어**

---

## 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이전 챕터 복습

[Chapter 18: 챗봇 만들기](../Chapter18-Chatbots/README.ko.md)에서 Discord와 Slack 봇을 만들었습니다. 이벤트 기반 프로그래밍과 외부 API 연동을 배웠죠.

이번 챕터에서는 웹 서비스의 **백엔드**를 만드는 방법을 배웁니다. 다음 챕터에서 프론트엔드와 연결해서 완전한 풀스택 앱을 완성할 거예요.

---

## 이 챕터에서 배우는 것

- 프론트엔드와 백엔드의 역할 이해
- Express로 웹 서버 만들기
- REST API 설계와 구현
- SQLite로 데이터 저장하기
- CRUD 연산 (생성, 조회, 수정, 삭제)

---

## 왜 백엔드가 필요한가요?

지금까지 우리가 만든 앱들을 생각해 보세요:
- 브라우저를 새로고침하면? → 데이터가 사라짐
- 다른 기기에서 접속하면? → 데이터가 없음
- 여러 사람이 사용하면? → 데이터 공유 불가

**백엔드가 있으면:**
- 데이터가 서버에 영구 저장됨
- 어느 기기에서든 같은 데이터에 접근
- 여러 사용자가 각자의 데이터를 가질 수 있음
- 보안이 필요한 로직을 안전하게 처리

> 💡 **비전공자 팁**: 프론트엔드만 있는 앱은 "기초 없는 집"과 같아요. 보기에는 좋지만, 데이터를 오래 보관할 수가 없죠. 백엔드는 그 집의 기초 공사입니다!

---

## 쉬운 비유: 레스토랑으로 이해하기

웹 애플리케이션을 레스토랑에 비유해봅시다:

```
┌─────────────────────────────────────────────────────────────────┐
│                     레스토랑 = 웹 애플리케이션                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🍽️ 식당 홀 (프론트엔드)           🍳 주방 (백엔드)              │
│  ─────────────────────          ────────────────               │
│  • 손님이 보는 공간              • 실제 요리가 이루어지는 곳      │
│  • 메뉴판, 테이블, 인테리어       • 요리사, 레시피, 조리 과정      │
│  • 웨이터가 주문을 받음           • 손님은 들어올 수 없음 (보안)   │
│                                                                 │
│  💬 주문서 = API 요청            📦 창고 = 데이터베이스           │
│  ─────────────────              ───────────────────            │
│  "토마토 파스타 하나요"          • 모든 재료가 정리되어 있음       │
│  → POST /api/orders             • 필요할 때 꺼내서 사용          │
│    { menu: "파스타" }            • 재고 관리                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

음식을 주문하면:
1. **손님(사용자)** → 웨이터(프론트엔드)에게 주문
2. **웨이터(프론트엔드)** → 주방(백엔드)에 주문서(API 요청) 전달
3. **주방(백엔드)** → 창고(데이터베이스)에서 재료 가져옴
4. **주방(백엔드)** → 요리 완성 후 웨이터에게 전달 (API 응답)
5. **웨이터(프론트엔드)** → 손님에게 음식 서빙

> 🔥 **중요**: 손님이 주방에 직접 들어가면 안 되듯이, 프론트엔드가 데이터베이스에 직접 접근하면 안 됩니다. 항상 백엔드를 통해야 보안이 유지됩니다!

---

## 프론트엔드 vs 백엔드 비교

| 구분 | 프론트엔드 | 백엔드 |
|------|-----------|--------|
| **위치** | 브라우저 (사용자 컴퓨터) | 서버 (원격 컴퓨터) |
| **역할** | 화면 표시, 사용자 입력 처리 | 데이터 처리, 비즈니스 로직 |
| **언어** | HTML, CSS, JavaScript | JavaScript(Node.js), Python, Java 등 |
| **보안** | 코드가 사용자에게 노출됨 | 코드가 사용자에게 보이지 않음 |
| **데이터** | 임시 저장 (새로고침하면 사라짐) | 영구 저장 (데이터베이스) |

```
┌────────────────────────────────────────────────────────────────┐
│              프론트엔드만 vs 백엔드 포함 비교                    │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ❌ 프론트엔드만                 ✅ 백엔드 포함                  │
│  ─────────────                   ──────────────                │
│                                                                │
│  ┌─────────────┐                ┌─────────────┐               │
│  │   브라우저   │                │   브라우저   │               │
│  │             │                │  (React 등) │               │
│  │ localStorage │               └──────┬──────┘               │
│  │  (임시저장)  │                       │                      │
│  └─────────────┘                       │ API 요청/응답         │
│        │                               ▼                       │
│        │ 새로고침                ┌─────────────┐               │
│        ▼ = 일부 유지             │   서버      │               │
│        │ 다른 기기               │  (Express)  │               │
│        ▼ = 데이터 없음           └──────┬──────┘               │
│                                         │                      │
│                                         ▼                      │
│                                  ┌─────────────┐              │
│                                  │ 데이터베이스 │              │
│                                  │  (SQLite)   │              │
│                                  └─────────────┘              │
│                                  어디서든 접근 가능             │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ 실습 환경 준비

백엔드 개발을 시작하기 전에 필요한 것들을 설치합니다.

### Step 1: 프로젝트 폴더 만들기

```bash
# 프로젝트 폴더 생성
mkdir my-first-backend
cd my-first-backend

# package.json 생성 (Node.js 프로젝트 초기화)
npm init -y
```

> 💡 **npm init -y란?** Node.js 프로젝트의 설정 파일(package.json)을 기본값으로 자동 생성합니다. `-y`는 모든 질문에 "yes"로 대답한다는 의미예요.

### Step 2: 필요한 패키지 설치

```bash
npm install express better-sqlite3
```

설치되는 패키지:
- **express**: 웹 서버를 쉽게 만들 수 있는 프레임워크
- **better-sqlite3**: SQLite 데이터베이스를 사용하기 위한 라이브러리

### Step 3: 폴더 구조

```
my-first-backend/
├── package.json      # 프로젝트 설정
├── package-lock.json # 패키지 버전 고정
├── node_modules/     # 설치된 패키지들
├── index.js          # 메인 서버 파일 (우리가 만들 것)
└── database.db       # 데이터베이스 파일 (자동 생성됨)
```

---

## Part 1: 가장 간단한 서버 만들기

### Hello World 서버

```javascript
// index.js - 세상에서 가장 간단한 웹 서버

// 1. Express 라이브러리 가져오기
const express = require('express')

// 2. Express 앱 생성
const app = express()

// 3. "/" 경로로 접속하면 "Hello World!" 응답
app.get('/', (req, res) => {
  res.send('Hello World!')
})

// 4. 서버 시작 (포트 3001에서 대기)
app.listen(3001, () => {
  console.log('🚀 서버가 http://localhost:3001 에서 실행 중!')
})
```

### 실행해보기

```bash
node index.js
```

브라우저에서 `http://localhost:3001`에 접속하면 "Hello World!"가 보입니다!

> 💡 **포트(Port)란?** 컴퓨터의 "문 번호"라고 생각하세요. 3001번 문으로 들어오는 요청을 우리 서버가 처리합니다. 다른 프로그램과 충돌하지 않도록 보통 3000~9000 사이의 번호를 사용해요.

### 🎯 실습 문제 1-1: 인사 서버

위 코드를 수정해서:
1. `/hello` 경로로 접속하면 "안녕하세요!"를 응답
2. `/bye` 경로로 접속하면 "안녕히 가세요!"를 응답

<details>
<summary>정답 보기</summary>

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
  res.send('메인 페이지입니다')
})

app.get('/hello', (req, res) => {
  res.send('안녕하세요!')
})

app.get('/bye', (req, res) => {
  res.send('안녕히 가세요!')
})

app.listen(3001, () => {
  console.log('🚀 서버가 http://localhost:3001 에서 실행 중!')
})
```

</details>

---

## Part 2: JSON으로 데이터 주고받기

실제 API는 텍스트가 아니라 **JSON** 형식으로 데이터를 주고받습니다.

### JSON이란?

JSON(JavaScript Object Notation)은 데이터를 표현하는 형식입니다:

```json
{
  "name": "홍길동",
  "age": 25,
  "hobbies": ["독서", "게임", "운동"]
}
```

### JSON 응답하기

```javascript
// index.js
const express = require('express')
const app = express()

// JSON으로 응답하기
app.get('/api/user', (req, res) => {
  res.json({
    id: 1,
    name: '홍길동',
    email: 'hong@example.com'
  })
})

// 여러 개의 데이터 (배열)
app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: '홍길동' },
    { id: 2, name: '김철수' },
    { id: 3, name: '이영희' }
  ])
})

app.listen(3001, () => {
  console.log('🚀 서버 실행 중: http://localhost:3001')
})
```

### 테스트하기

브라우저에서 `http://localhost:3001/api/users`에 접속하면 JSON 데이터가 보입니다.

또는 터미널에서 curl 명령어로 테스트:

```bash
curl http://localhost:3001/api/users
```

> 💡 **res.send() vs res.json()**:
> - `res.send('텍스트')`: 일반 텍스트 응답
> - `res.json(객체)`: JSON 형식으로 응답 (자동으로 Content-Type 헤더 설정)

---

## Part 3: HTTP 메서드 이해하기

API는 **HTTP 메서드**를 사용해서 어떤 작업을 할지 구분합니다.

### CRUD와 HTTP 메서드

| 작업 | CRUD | HTTP 메서드 | 예시 | 설명 |
|------|------|-------------|------|------|
| 만들기 | Create | **POST** | `POST /api/todos` | 새 할 일 추가 |
| 읽기 | Read | **GET** | `GET /api/todos` | 할 일 목록 조회 |
| 수정 | Update | **PUT/PATCH** | `PATCH /api/todos/1` | 1번 할 일 수정 |
| 삭제 | Delete | **DELETE** | `DELETE /api/todos/1` | 1번 할 일 삭제 |

### 비유로 이해하기

```
┌─────────────────────────────────────────────────────────────────┐
│                    HTTP 메서드 = 도서관 작업                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📚 GET = "이 책 보여주세요"                                      │
│     → 책을 가져와서 보여줌 (데이터 변경 없음)                      │
│                                                                 │
│  📝 POST = "새 책을 등록해주세요"                                  │
│     → 새로운 책을 도서관에 추가                                   │
│                                                                 │
│  ✏️ PATCH = "이 책의 상태를 '대출중'으로 바꿔주세요"               │
│     → 책의 일부 정보만 수정                                       │
│                                                                 │
│  🗑️ DELETE = "이 책을 폐기해주세요"                               │
│     → 책을 도서관에서 제거                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 모든 메서드 사용해보기

```javascript
const express = require('express')
const app = express()

// JSON 요청 본문을 파싱하기 위한 미들웨어
app.use(express.json())

// 임시 데이터 저장소 (나중에 데이터베이스로 교체)
let todos = [
  { id: 1, text: '장보기', completed: false },
  { id: 2, text: '운동하기', completed: true }
]
let nextId = 3

// ═══════════════════════════════════════════════════════════════
// GET: 모든 할 일 조회
// ═══════════════════════════════════════════════════════════════
app.get('/api/todos', (req, res) => {
  res.json(todos)
})

// ═══════════════════════════════════════════════════════════════
// GET: 특정 할 일 조회 (URL 파라미터 사용)
// ═══════════════════════════════════════════════════════════════
app.get('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)  // URL에서 id 추출
  const todo = todos.find(t => t.id === id)

  if (!todo) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }

  res.json(todo)
})

// ═══════════════════════════════════════════════════════════════
// POST: 새 할 일 추가
// ═══════════════════════════════════════════════════════════════
app.post('/api/todos', (req, res) => {
  const { text } = req.body  // 요청 본문에서 text 추출

  if (!text) {
    return res.status(400).json({ error: '할 일 내용을 입력해주세요' })
  }

  const newTodo = {
    id: nextId++,
    text: text,
    completed: false
  }

  todos.push(newTodo)
  res.status(201).json(newTodo)  // 201: Created
})

// ═══════════════════════════════════════════════════════════════
// PATCH: 할 일 수정 (일부만)
// ═══════════════════════════════════════════════════════════════
app.patch('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const todo = todos.find(t => t.id === id)

  if (!todo) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }

  // 요청 본문에 있는 필드만 업데이트
  if (req.body.text !== undefined) {
    todo.text = req.body.text
  }
  if (req.body.completed !== undefined) {
    todo.completed = req.body.completed
  }

  res.json(todo)
})

// ═══════════════════════════════════════════════════════════════
// DELETE: 할 일 삭제
// ═══════════════════════════════════════════════════════════════
app.delete('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const index = todos.findIndex(t => t.id === id)

  if (index === -1) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }

  todos.splice(index, 1)  // 배열에서 제거
  res.json({ success: true, message: '삭제되었습니다' })
})

app.listen(3001, () => {
  console.log('🚀 서버 실행 중: http://localhost:3001')
})
```

### curl로 테스트하기

```bash
# GET: 모든 할 일 조회
curl http://localhost:3001/api/todos

# GET: 1번 할 일 조회
curl http://localhost:3001/api/todos/1

# POST: 새 할 일 추가
curl -X POST http://localhost:3001/api/todos \
  -H "Content-Type: application/json" \
  -d '{"text": "공부하기"}'

# PATCH: 1번 할 일 완료로 변경
curl -X PATCH http://localhost:3001/api/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"completed": true}'

# DELETE: 2번 할 일 삭제
curl -X DELETE http://localhost:3001/api/todos/2
```

> 💡 **req.params vs req.body**:
> - `req.params`: URL에 포함된 값 (예: `/api/todos/1`의 1)
> - `req.body`: 요청 본문에 포함된 JSON 데이터

### 🎯 실습 문제 3-1: 메모 API

할 일(todos) API를 참고해서 메모(memos) API를 만들어보세요:
- `GET /api/memos` - 모든 메모 조회
- `POST /api/memos` - 새 메모 추가 (title, content 필드)
- `DELETE /api/memos/:id` - 메모 삭제

<details>
<summary>정답 보기</summary>

```javascript
const express = require('express')
const app = express()
app.use(express.json())

let memos = []
let nextId = 1

app.get('/api/memos', (req, res) => {
  res.json(memos)
})

app.post('/api/memos', (req, res) => {
  const { title, content } = req.body

  if (!title) {
    return res.status(400).json({ error: '제목을 입력해주세요' })
  }

  const newMemo = {
    id: nextId++,
    title,
    content: content || '',
    createdAt: new Date().toISOString()
  }

  memos.push(newMemo)
  res.status(201).json(newMemo)
})

app.delete('/api/memos/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const index = memos.findIndex(m => m.id === id)

  if (index === -1) {
    return res.status(404).json({ error: '메모를 찾을 수 없습니다' })
  }

  memos.splice(index, 1)
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('🚀 서버 실행 중!')
})
```

</details>

---

## Part 4: 데이터베이스 연결하기

지금까지는 데이터를 변수(메모리)에 저장했습니다. 서버를 재시작하면 데이터가 사라지죠. 이제 **데이터베이스**를 사용해서 영구적으로 저장해봅시다.

### SQLite란?

SQLite는 파일 기반의 가벼운 데이터베이스입니다:
- 별도 설치 필요 없음 (파일 하나로 동작)
- 작은 프로젝트에 적합
- 학습용으로 좋음

```
┌─────────────────────────────────────────────────────────────────┐
│                    데이터베이스 = 엑셀 파일                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 엑셀처럼 "표" 형태로 데이터 저장                               │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ todos 테이블 (표)                                        │   │
│  ├─────┬────────────────┬───────────┬─────────────────────┤   │
│  │ id  │ text           │ completed │ created_at          │   │
│  ├─────┼────────────────┼───────────┼─────────────────────┤   │
│  │ 1   │ 장보기          │ 0         │ 2025-01-17 10:00   │   │
│  │ 2   │ 운동하기        │ 1         │ 2025-01-17 11:00   │   │
│  │ 3   │ 공부하기        │ 0         │ 2025-01-17 12:00   │   │
│  └─────┴────────────────┴───────────┴─────────────────────┘   │
│                                                                 │
│  • id: 각 행을 구분하는 고유 번호 (자동 증가)                      │
│  • text: 할 일 내용                                              │
│  • completed: 완료 여부 (0=미완료, 1=완료)                        │
│  • created_at: 생성 시간                                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### SQLite 연결하기

```javascript
// index.js
const express = require('express')
const Database = require('better-sqlite3')

const app = express()
app.use(express.json())

// ═══════════════════════════════════════════════════════════════
// 데이터베이스 연결
// ═══════════════════════════════════════════════════════════════
const db = new Database('todos.db')  // todos.db 파일 생성/연결

// ═══════════════════════════════════════════════════════════════
// 테이블 생성 (없으면 새로 만들기)
// ═══════════════════════════════════════════════════════════════
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

console.log('✅ 데이터베이스 연결 완료!')
```

> 💡 **SQL 문법 설명**:
> - `CREATE TABLE IF NOT EXISTS`: 테이블이 없을 때만 생성
> - `INTEGER PRIMARY KEY AUTOINCREMENT`: 자동 증가하는 고유 번호
> - `TEXT NOT NULL`: 문자열, 필수 입력
> - `DEFAULT 0`: 값을 안 넣으면 기본값 0 사용

### CRUD with 데이터베이스

이제 메모리 대신 데이터베이스를 사용하도록 API를 수정합니다:

```javascript
// index.js - 전체 코드
const express = require('express')
const Database = require('better-sqlite3')

const app = express()
app.use(express.json())

// 데이터베이스 연결
const db = new Database('todos.db')

// 테이블 생성
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// ═══════════════════════════════════════════════════════════════
// GET: 모든 할 일 조회
// ═══════════════════════════════════════════════════════════════
app.get('/api/todos', (req, res) => {
  try {
    // SQL 쿼리 실행: 모든 행을 최신순으로 가져오기
    const todos = db.prepare(
      'SELECT * FROM todos ORDER BY created_at DESC'
    ).all()

    res.json(todos)
  } catch (error) {
    console.error('조회 오류:', error)
    res.status(500).json({ error: '데이터를 불러오는데 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// GET: 특정 할 일 조회
// ═══════════════════════════════════════════════════════════════
app.get('/api/todos/:id', (req, res) => {
  try {
    const id = req.params.id

    // SQL 쿼리: id가 일치하는 행 1개 가져오기
    const todo = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    if (!todo) {
      return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
    }

    res.json(todo)
  } catch (error) {
    console.error('조회 오류:', error)
    res.status(500).json({ error: '데이터를 불러오는데 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// POST: 새 할 일 추가
// ═══════════════════════════════════════════════════════════════
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  // 입력값 검증
  if (!text || typeof text !== 'string') {
    return res.status(400).json({ error: '할 일 내용을 입력해주세요' })
  }

  if (text.trim().length === 0) {
    return res.status(400).json({ error: '빈 내용은 추가할 수 없습니다' })
  }

  if (text.length > 500) {
    return res.status(400).json({ error: '500자 이내로 입력해주세요' })
  }

  try {
    // SQL 쿼리: 새 행 삽입
    const result = db.prepare(
      'INSERT INTO todos (text) VALUES (?)'
    ).run(text.trim())

    // 방금 추가한 데이터 조회해서 반환
    const newTodo = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(result.lastInsertRowid)

    res.status(201).json(newTodo)
  } catch (error) {
    console.error('추가 오류:', error)
    res.status(500).json({ error: '할 일 추가에 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// PATCH: 할 일 수정
// ═══════════════════════════════════════════════════════════════
app.patch('/api/todos/:id', (req, res) => {
  const id = req.params.id
  const { text, completed } = req.body

  try {
    // 먼저 해당 할 일이 존재하는지 확인
    const existing = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    if (!existing) {
      return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
    }

    // 업데이트할 필드 결정
    const newText = text !== undefined ? text.trim() : existing.text
    const newCompleted = completed !== undefined ? (completed ? 1 : 0) : existing.completed

    // SQL 쿼리: 행 업데이트
    db.prepare(
      'UPDATE todos SET text = ?, completed = ? WHERE id = ?'
    ).run(newText, newCompleted, id)

    // 업데이트된 데이터 반환
    const updated = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    res.json(updated)
  } catch (error) {
    console.error('수정 오류:', error)
    res.status(500).json({ error: '수정에 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// DELETE: 할 일 삭제
// ═══════════════════════════════════════════════════════════════
app.delete('/api/todos/:id', (req, res) => {
  const id = req.params.id

  try {
    // SQL 쿼리: 행 삭제
    const result = db.prepare(
      'DELETE FROM todos WHERE id = ?'
    ).run(id)

    // 삭제된 행이 없으면 404
    if (result.changes === 0) {
      return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
    }

    res.json({ success: true, message: '삭제되었습니다' })
  } catch (error) {
    console.error('삭제 오류:', error)
    res.status(500).json({ error: '삭제에 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// 서버 시작
// ═══════════════════════════════════════════════════════════════
const PORT = 3001

app.listen(PORT, () => {
  console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
  console.log(`🚀 서버가 http://localhost:${PORT} 에서 실행 중!`)
  console.log('━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━')
  console.log('')
  console.log('📝 API 목록:')
  console.log('   GET    /api/todos     - 모든 할 일 조회')
  console.log('   GET    /api/todos/:id - 특정 할 일 조회')
  console.log('   POST   /api/todos     - 새 할 일 추가')
  console.log('   PATCH  /api/todos/:id - 할 일 수정')
  console.log('   DELETE /api/todos/:id - 할 일 삭제')
  console.log('')
})
```

### 주요 SQL 명령어

| 명령어 | 용도 | 예시 |
|--------|------|------|
| `SELECT` | 데이터 조회 | `SELECT * FROM todos` |
| `INSERT` | 데이터 추가 | `INSERT INTO todos (text) VALUES ('장보기')` |
| `UPDATE` | 데이터 수정 | `UPDATE todos SET completed = 1 WHERE id = 1` |
| `DELETE` | 데이터 삭제 | `DELETE FROM todos WHERE id = 1` |

> 💡 **`?` 플레이스홀더**: SQL에서 `?`는 나중에 값이 들어갈 자리를 표시합니다. 이렇게 하면 SQL Injection 공격을 방지할 수 있어요!

### 🎯 실습 문제 4-1: 통계 API 추가

다음 API를 추가해보세요:
- `GET /api/todos/stats` - 통계 반환
  - 전체 개수
  - 완료된 개수
  - 미완료 개수

<details>
<summary>힌트</summary>

SQL의 `COUNT` 함수를 사용하세요:
```sql
SELECT COUNT(*) as total FROM todos
SELECT COUNT(*) as completed FROM todos WHERE completed = 1
```

</details>

<details>
<summary>정답 보기</summary>

```javascript
// 주의: 이 라우트는 /api/todos/:id 보다 먼저 정의해야 합니다!
// 그렇지 않으면 'stats'가 :id로 인식됩니다.

app.get('/api/todos/stats', (req, res) => {
  try {
    const total = db.prepare(
      'SELECT COUNT(*) as count FROM todos'
    ).get().count

    const completed = db.prepare(
      'SELECT COUNT(*) as count FROM todos WHERE completed = 1'
    ).get().count

    res.json({
      total,
      completed,
      incomplete: total - completed
    })
  } catch (error) {
    console.error('통계 조회 오류:', error)
    res.status(500).json({ error: '통계를 불러오는데 실패했습니다' })
  }
})
```

</details>

---

## Part 5: 에러 처리와 검증

안정적인 API를 만들려면 **에러 처리**가 중요합니다.

### HTTP 상태 코드

| 코드 | 의미 | 사용 시점 |
|------|------|----------|
| 200 | OK | 성공 |
| 201 | Created | 새 리소스 생성 성공 |
| 400 | Bad Request | 잘못된 요청 (입력값 오류) |
| 404 | Not Found | 리소스를 찾을 수 없음 |
| 500 | Internal Server Error | 서버 내부 오류 |

### 입력값 검증 예시

```javascript
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  // 1. 필수값 확인
  if (!text) {
    return res.status(400).json({
      error: '할 일 내용을 입력해주세요',
      field: 'text'
    })
  }

  // 2. 타입 확인
  if (typeof text !== 'string') {
    return res.status(400).json({
      error: '할 일 내용은 문자열이어야 합니다',
      field: 'text'
    })
  }

  // 3. 길이 확인
  const trimmed = text.trim()
  if (trimmed.length === 0) {
    return res.status(400).json({
      error: '빈 내용은 추가할 수 없습니다',
      field: 'text'
    })
  }

  if (trimmed.length > 500) {
    return res.status(400).json({
      error: '500자 이내로 입력해주세요',
      field: 'text',
      maxLength: 500,
      currentLength: trimmed.length
    })
  }

  // 모든 검증 통과 → 데이터 저장
  try {
    const result = db.prepare(
      'INSERT INTO todos (text) VALUES (?)'
    ).run(trimmed)

    res.status(201).json({
      id: result.lastInsertRowid,
      text: trimmed,
      completed: 0
    })
  } catch (error) {
    console.error('DB 오류:', error)
    res.status(500).json({ error: '서버 오류가 발생했습니다' })
  }
})
```

### try-catch로 에러 잡기

```javascript
app.get('/api/todos', (req, res) => {
  try {
    // 정상 실행되는 코드
    const todos = db.prepare('SELECT * FROM todos').all()
    res.json(todos)
  } catch (error) {
    // 에러가 발생하면 이쪽으로
    console.error('에러 발생:', error)
    res.status(500).json({ error: '서버 오류가 발생했습니다' })
  }
})
```

> 🔥 **중요**: `try-catch` 없이 에러가 발생하면 서버가 죽을 수 있습니다! 모든 API에 에러 처리를 추가하세요.

---

## Part 6: 미들웨어 이해하기

미들웨어는 **요청과 응답 사이에서 실행되는 함수**입니다.

```
┌─────────────────────────────────────────────────────────────────┐
│                      미들웨어 동작 흐름                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  요청 ──▶ [미들웨어1] ──▶ [미들웨어2] ──▶ [라우트 핸들러] ──▶ 응답  │
│                                                                 │
│  예시:                                                          │
│  POST /api/todos                                                │
│       │                                                         │
│       ▼                                                         │
│  [express.json()]  ← JSON 파싱                                  │
│       │                                                         │
│       ▼                                                         │
│  [logRequest()]    ← 로그 기록                                   │
│       │                                                         │
│       ▼                                                         │
│  [app.post 핸들러] ← 실제 처리                                   │
│       │                                                         │
│       ▼                                                         │
│  응답 전송                                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 자주 사용하는 미들웨어

```javascript
const express = require('express')
const app = express()

// ═══════════════════════════════════════════════════════════════
// 1. JSON 파싱 미들웨어 (내장)
// ═══════════════════════════════════════════════════════════════
app.use(express.json())
// → 요청 본문의 JSON을 자동으로 파싱해서 req.body에 넣어줌

// ═══════════════════════════════════════════════════════════════
// 2. 요청 로깅 미들웨어 (직접 만들기)
// ═══════════════════════════════════════════════════════════════
app.use((req, res, next) => {
  const timestamp = new Date().toISOString()
  console.log(`[${timestamp}] ${req.method} ${req.url}`)
  next()  // 다음 미들웨어로 넘기기
})

// ═══════════════════════════════════════════════════════════════
// 3. 에러 처리 미들웨어 (4개의 매개변수)
// ═══════════════════════════════════════════════════════════════
app.use((err, req, res, next) => {
  console.error('에러 발생:', err)
  res.status(500).json({ error: '서버 오류가 발생했습니다' })
})
```

> 💡 **next()란?** 미들웨어에서 다음 미들웨어로 제어를 넘기는 함수입니다. `next()`를 호출하지 않으면 요청이 거기서 멈춥니다!

---

## 🎯 미니 퀴즈

### 퀴즈 1: HTTP 메서드

새 데이터를 추가할 때 사용하는 HTTP 메서드는?

A) GET
B) POST
C) PATCH
D) DELETE

<details>
<summary>정답 보기</summary>

**정답: B) POST**

- GET: 데이터 조회
- POST: 새 데이터 생성
- PATCH: 데이터 일부 수정
- DELETE: 데이터 삭제

</details>

### 퀴즈 2: 상태 코드

존재하지 않는 리소스에 접근할 때 반환해야 하는 상태 코드는?

A) 200
B) 201
C) 400
D) 404

<details>
<summary>정답 보기</summary>

**정답: D) 404**

- 200: OK (성공)
- 201: Created (생성 성공)
- 400: Bad Request (잘못된 요청)
- 404: Not Found (찾을 수 없음)

</details>

### 퀴즈 3: SQL 명령어

데이터를 수정할 때 사용하는 SQL 명령어는?

A) SELECT
B) INSERT
C) UPDATE
D) DELETE

<details>
<summary>정답 보기</summary>

**정답: C) UPDATE**

- SELECT: 조회
- INSERT: 추가
- UPDATE: 수정
- DELETE: 삭제

</details>

### 퀴즈 4: 미들웨어

다음 코드에서 `next()`의 역할은?

```javascript
app.use((req, res, next) => {
  console.log('로그')
  next()
})
```

A) 응답을 보냄
B) 다음 미들웨어로 제어를 넘김
C) 에러를 발생시킴
D) 요청을 취소함

<details>
<summary>정답 보기</summary>

**정답: B) 다음 미들웨어로 제어를 넘김**

`next()`를 호출하지 않으면 요청이 그 미들웨어에서 멈추고, 응답도 보내지지 않습니다.

</details>

---

## 📝 실습 과제

### 기본 과제: 메모 API 완성하기

다음 기능을 가진 메모 API를 만들어보세요:

**데이터베이스 스키마:**
```sql
CREATE TABLE memos (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  content TEXT,
  created_at TEXT DEFAULT CURRENT_TIMESTAMP,
  updated_at TEXT DEFAULT CURRENT_TIMESTAMP
)
```

**API 엔드포인트:**
- `GET /api/memos` - 모든 메모 조회
- `GET /api/memos/:id` - 특정 메모 조회
- `POST /api/memos` - 새 메모 추가 (title 필수, content 선택)
- `PATCH /api/memos/:id` - 메모 수정
- `DELETE /api/memos/:id` - 메모 삭제

**추가 기능 (도전):**
- `GET /api/memos/search?q=검색어` - 제목이나 내용으로 검색

### 심화 과제: 북마크 API

URL 북마크를 저장하는 API를 만들어보세요:

**스키마:**
```sql
CREATE TABLE bookmarks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  url TEXT NOT NULL,
  title TEXT NOT NULL,
  description TEXT,
  created_at TEXT DEFAULT CURRENT_TIMESTAMP
)
```

**API:**
- 기본 CRUD + 검색 기능

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **API** | 프로그램들이 서로 소통하는 방법/규칙 |
| **REST API** | URL과 HTTP 메서드를 사용하는 API 설계 방식 |
| **Express** | Node.js 웹 서버 프레임워크 |
| **SQLite** | 파일 기반의 가벼운 데이터베이스 |
| **CRUD** | Create, Read, Update, Delete의 약자 |
| **미들웨어** | 요청과 응답 사이에서 실행되는 함수 |
| **HTTP 메서드** | GET, POST, PUT, PATCH, DELETE 등 요청의 종류 |
| **상태 코드** | 200, 404, 500 등 응답의 결과를 나타내는 숫자 |
| **JSON** | 데이터를 표현하는 형식 |
| **쿼리** | 데이터베이스에 보내는 명령/질문 |

---

## 다음 챕터 미리보기

[Chapter 20: 풀스택 앱 완성하기](../Chapter20-Full-Stack-Apps/README.ko.md)에서는:
- React 프론트엔드와 백엔드 연결
- CORS 설정
- 인증 시스템 (JWT)
- 배포 준비

이번 챕터에서 만든 백엔드 API를 실제 화면과 연결해서 완전한 풀스택 앱을 만들어봅니다!

---

## 요약

1. **백엔드**는 데이터 저장과 비즈니스 로직을 담당
2. **Express**로 웹 서버를 쉽게 만들 수 있음
3. **HTTP 메서드**로 CRUD 작업 구분: GET(조회), POST(생성), PATCH(수정), DELETE(삭제)
4. **SQLite**로 데이터를 영구 저장
5. **에러 처리**와 **입력값 검증**은 필수!
6. **미들웨어**로 공통 로직 처리

백엔드 기초를 익혔으니, 다음 챕터에서 프론트엔드와 연결해봅시다!

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Express.js 공식 가이드](https://expressjs.com/ko/) - Express 프레임워크 한글 문서
- [MDN HTTP 개요](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview) - HTTP 프로토콜 이해
- [SQLite 문서](https://www.sqlite.org/docs.html) - SQLite 공식 문서

**영상 자료:**
- [Express.js 입문 (YouTube)](https://www.youtube.com/results?search_query=express.js+tutorial+beginner+korean)
- [REST API 설계 (YouTube)](https://www.youtube.com/results?search_query=rest+api+design+tutorial)
- [SQL 기초 (YouTube)](https://www.youtube.com/results?search_query=sql+tutorial+beginner+korean)

**읽을거리:**
- [REST API 설계 가이드](https://restfulapi.net/) - RESTful API 설계 원칙
- [HTTP 상태 코드 레퍼런스](https://httpstatuses.com/) - 상태 코드 완전 가이드
- [API 보안 베스트 프랙티스](https://owasp.org/www-project-api-security/) - OWASP API 보안 가이드

**관련 도구:**
- [Postman](https://www.postman.com/) - API 테스트 도구
- [Insomnia](https://insomnia.rest/) - REST 클라이언트
- [DB Browser for SQLite](https://sqlitebrowser.org/) - SQLite GUI 도구
- [Thunder Client](https://www.thunderclient.com/) - VS Code API 테스트 확장

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
