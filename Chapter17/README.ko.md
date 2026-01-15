# Chapter 17: 풀스택 앱 만들기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 프론트엔드와 백엔드를 함께 만드는 방법
- API 설계와 구현
- 데이터 저장과 조회

---

## 왜 필요합니까?

**풀스택 기술이 빛나는 실제 상황들:**

- **어떤 기기에서도 작동하는 투두 앱 만들기** - 데이터가 브라우저가 아닌 데이터베이스에 저장되어 있어서 어디서든 동기화됩니다
- **여러 사용자를 위한 서비스 만들기** - 각 사람이 자신만의 계정과 데이터를 안전하게 분리해서 가집니다
- **실제 비즈니스 운영하기** - 결제 처리, 이메일 발송, 사용자 인증 등 모두 백엔드 로직이 필요합니다
- **오프라인에서도 작동하는 앱 만들기** - 로컬에 데이터를 저장하고 온라인일 때 동기화합니다

백엔드 없이는 여러분의 앱은 기초 없는 집과 같습니다. 보기에는 좋지만 오래 가지 못합니다.

---

## 쉬운 비유: 풀스택은 레스토랑과 같습니다

레스토랑을 생각해 보시기 바랍니다:
- **프론트엔드 (식당 홀)**: 손님이 보는 것 - 메뉴, 테이블, 인테리어, 주문받는 웨이터
- **백엔드 (주방)**: 실제 작업이 일어나는 곳 - 요리, 재료 저장, 레시피 관리
- **데이터베이스 (창고/저장실)**: 모든 재료와 물품이 정리되어 있는 곳

음식을 주문하면:
1. 웨이터(프론트엔드)에게 원하는 걸 말합니다
2. 웨이터가 주문을 주방(백엔드)에 전달합니다
3. 주방이 저장실(데이터베이스)에서 재료를 가져옵니다
4. 음식이 준비되어 웨이터를 통해 다시 전달됩니다

웹 앱도 정확히 같은 방식으로 작동합니다.

---

## 왜 풀스택입니까?

지금까지 만든 프로젝트들은 대부분 프론트엔드만 있었습니다. 하지만 실제 서비스는:
- 여러 사용자의 데이터를 저장해야 하고
- 데이터를 영구적으로 보관해야 하며
- 보안이 필요한 로직이 있습니다

이런 것들을 처리하려면 백엔드가 필요합니다.

**풀스택 요청 팁:**

```
> 북마크 저장 앱을 만들어줘.
> 프론트엔드는 React,
> 백엔드는 Express,
> 데이터베이스는 SQLite.
>
> 기능:
> - 북마크 추가 (URL, 제목)
> - 북마크 목록 보기
> - 북마크 삭제
```

프론트/백엔드 기술 스택과 필요한 기능을 명시하면 전체 구조를 잡아줍니다.

---

## 프로젝트: 할 일 관리 앱

간단하지만 완전한 풀스택 앱을 만들어봅시다.

### Step 1: 프로젝트 시작

```
> 할 일 관리 풀스택 앱을 만들어줘.
>
> 구조:
> - frontend/ (React + Vite)
> - backend/ (Express + SQLite)
>
> 일단 프로젝트 폴더 구조만 잡아줘.
```

Claude가 만드는 구조:
```
todo-app/
├── frontend/
│   ├── src/
│   │   ├── App.jsx
│   │   └── components/
│   └── package.json
├── backend/
│   ├── index.js
│   ├── routes/
│   └── package.json
└── README.md
```

### Step 2: 백엔드 API 만들기

```
> 백엔드에 할 일 API를 만들어줘.
>
> 엔드포인트:
> GET /api/todos - 모든 할 일 조회
> POST /api/todos - 새 할 일 추가
> PATCH /api/todos/:id - 완료 상태 토글
> DELETE /api/todos/:id - 할 일 삭제
>
> SQLite로 데이터 저장.
```

Claude가 만드는 API:

```javascript
// backend/index.js
const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')

const app = express()
const db = new Database('todos.db')

app.use(cors())
app.use(express.json())

// 테이블 생성
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// 전체 조회
app.get('/api/todos', (req, res) => {
  const todos = db.prepare('SELECT * FROM todos ORDER BY created_at DESC').all()
  res.json(todos)
})

// 추가
app.post('/api/todos', (req, res) => {
  const { text } = req.body
  const result = db.prepare('INSERT INTO todos (text) VALUES (?)').run(text)
  res.json({ id: result.lastInsertRowid, text, completed: 0 })
})

// 토글
app.patch('/api/todos/:id', (req, res) => {
  const { id } = req.params
  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)
  res.json({ success: true })
})

// 삭제
app.delete('/api/todos/:id', (req, res) => {
  const { id } = req.params
  db.prepare('DELETE FROM todos WHERE id = ?').run(id)
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('Server running on http://localhost:3001')
})
```

### Step 3: 프론트엔드 연결

```
> 프론트엔드에서 백엔드 API를 호출하는 코드 만들어줘.
>
> - 할 일 목록 표시
> - 새 할 일 추가 폼
> - 체크박스로 완료 토글
> - 삭제 버튼
```

```javascript
// frontend/src/App.jsx
import { useState, useEffect } from 'react'

const API_URL = 'http://localhost:3001/api/todos'

function App() {
  const [todos, setTodos] = useState([])
  const [newTodo, setNewTodo] = useState('')

  useEffect(() => {
    fetch(API_URL)
      .then(res => res.json())
      .then(setTodos)
  }, [])

  const addTodo = async () => {
    if (!newTodo.trim()) return
    const res = await fetch(API_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ text: newTodo })
    })
    const todo = await res.json()
    setTodos([todo, ...todos])
    setNewTodo('')
  }

  const toggleTodo = async (id) => {
    await fetch(`${API_URL}/${id}`, { method: 'PATCH' })
    setTodos(todos.map(t =>
      t.id === id ? { ...t, completed: !t.completed } : t
    ))
  }

  const deleteTodo = async (id) => {
    await fetch(`${API_URL}/${id}`, { method: 'DELETE' })
    setTodos(todos.filter(t => t.id !== id))
  }

  return (
    <div>
      <h1>할 일 목록</h1>
      <input
        value={newTodo}
        onChange={e => setNewTodo(e.target.value)}
        placeholder="할 일 입력"
      />
      <button onClick={addTodo}>추가</button>

      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{
              textDecoration: todo.completed ? 'line-through' : 'none'
            }}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>삭제</button>
          </li>
        ))}
      </ul>
    </div>
  )
}

export default App
```

### Step 4: 실행

```bash
# 터미널 1: 백엔드
cd backend && npm install && node index.js

# 터미널 2: 프론트엔드
cd frontend && npm install && npm run dev
```

---

## 따라해보세요: 최소 동작 예제

전체 투두 앱을 만들기 전에, 프론트엔드와 백엔드가 서로 통신할 수 있는지 확인해봅시다:

**1. 최소한의 백엔드 (`server.js`) 만들기:**

```javascript
// server.js - 가장 간단한 Express 서버
const express = require('express')
const cors = require('cors')

const app = express()
app.use(cors())
app.use(express.json())

// 메모리 내 데이터 (테스트용)
let message = '백엔드에서 인사드립니다!'

// GET: 데이터 조회
app.get('/api/message', (req, res) => {
  res.json({ message })
})

// POST: 데이터 업데이트
app.post('/api/message', (req, res) => {
  message = req.body.message
  res.json({ success: true, message })
})

app.listen(3001, () => {
  console.log('백엔드 실행 중: http://localhost:3001')
})
```

**2. curl로 테스트 (또는 브라우저에서):**

```bash
# 서버 시작
npm init -y && npm install express cors
node server.js

# 다른 터미널에서 API 테스트
curl http://localhost:3001/api/message
# {"message":"백엔드에서 인사드립니다!"}

curl -X POST http://localhost:3001/api/message \
  -H "Content-Type: application/json" \
  -d '{"message":"업데이트됨!"}'
# {"success":true,"message":"업데이트됨!"}
```

**3. 프론트엔드에서 연결:**

```javascript
// React 컴포넌트에서
const [message, setMessage] = useState('')

// 로드 시 데이터 가져오기
useEffect(() => {
  fetch('http://localhost:3001/api/message')
    .then(res => res.json())
    .then(data => setMessage(data.message))
}, [])
```

React 앱에서 "백엔드에서 인사드립니다!"가 보이면 연결에 성공한 것입니다. 이제 전체 투두 앱을 만들 수 있습니다.

---

## 기능 확장하기

### 카테고리 추가

```
> 할 일에 카테고리 기능을 추가해줘.
> - 카테고리 생성/삭제
> - 할 일에 카테고리 지정
> - 카테고리별 필터링
```

### 마감일 기능

```
> 할 일에 마감일을 설정할 수 있게 해줘.
> - 날짜 선택 UI
> - 마감일 순 정렬
> - 오늘 마감인 것 강조
```

### 검색 기능

```
> 할 일 검색 기능 추가해줘.
> - 텍스트로 검색
> - 검색어 하이라이트
```

---

## 두 번째 프로젝트: 메모 앱

마크다운을 지원하는 메모 앱을 만들어봅시다.

### 기본 설정

```
> 마크다운 메모 앱을 만들어줘.
>
> 프론트엔드: React
> 백엔드: Express + SQLite
>
> 기능:
> - 메모 작성 (마크다운 지원)
> - 메모 목록 (사이드바)
> - 실시간 미리보기
> - 메모 삭제
```

### 마크다운 렌더링

```
> react-markdown 라이브러리로 마크다운 렌더링해줘.
> 코드 블록에는 syntax highlighting도 적용해줘.
```

### 자동 저장

```
> 메모 작성 중에 자동 저장되게 해줘.
> 타이핑 멈춘 후 1초 뒤에 저장.
> 저장 중일 때 상태 표시.
```

---

## 인증 추가하기

실제 서비스에는 로그인이 필요합니다.

### 간단한 인증

```
> 간단한 로그인 기능을 추가해줘.
> - 회원가입 (이메일, 비밀번호)
> - 로그인
> - JWT 토큰 사용
> - 로그인한 사용자만 자기 할 일 보기
```

### 백엔드 인증

```javascript
// 로그인 API
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body
  const user = db.prepare('SELECT * FROM users WHERE email = ?').get(email)

  if (!user || !await bcrypt.compare(password, user.password)) {
    return res.status(401).json({ error: '로그인 실패' })
  }

  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET)
  res.json({ token })
})

// 인증 미들웨어
function auth(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1]
  if (!token) return res.status(401).json({ error: '인증 필요' })

  try {
    const { userId } = jwt.verify(token, process.env.JWT_SECRET)
    req.userId = userId
    next()
  } catch {
    res.status(401).json({ error: '유효하지 않은 토큰' })
  }
}

// 보호된 라우트
app.get('/api/todos', auth, (req, res) => {
  const todos = db.prepare('SELECT * FROM todos WHERE user_id = ?').all(req.userId)
  res.json(todos)
})
```

---

## 배포하기

### 풀스택 앱 배포

```
> 이 풀스택 앱을 배포하려고 해.
> 프론트엔드는 Vercel,
> 백엔드는 Railway에 배포하는 방법 알려줘.
```

### 환경변수 설정

```
> 프로덕션에서 필요한 환경변수들 정리해줘.
> - 데이터베이스 연결
> - JWT 시크릿
> - 프론트엔드 URL
```

### 배포 체크리스트

Claude에게 물어보세요:
```
> 배포 전에 체크해야 할 것들 알려줘.
> - 보안
> - 성능
> - 에러 처리
```

---

## 실습 과제

### 기본 과제

```
> 북마크 관리 앱을 만들어줘.
>
> 기능:
> - URL과 제목으로 북마크 추가
> - 북마크 목록 보기
> - 태그로 분류
> - 검색
>
> 기술: React + Express + SQLite
```

### 심화 과제

```
> 간단한 블로그 시스템을 만들어줘.
>
> - 글 작성/수정/삭제
> - 마크다운 지원
> - 카테고리
> - 간단한 관리자 인증
```

```
> 파일 공유 서비스를 만들어줘.
>
> - 파일 업로드
> - 공유 링크 생성
> - 다운로드 카운터
> - 만료 시간 설정
```

---

## API 설계 팁

### RESTful 설계

```
> RESTful API 설계 원칙 알려줘.
> - 리소스 네이밍
> - HTTP 메서드
> - 상태 코드
```

| 메서드 | 용도 | 예시 |
|--------|------|------|
| GET | 조회 | GET /api/todos |
| POST | 생성 | POST /api/todos |
| PUT | 전체 수정 | PUT /api/todos/1 |
| PATCH | 부분 수정 | PATCH /api/todos/1 |
| DELETE | 삭제 | DELETE /api/todos/1 |

### 에러 처리

```
> API 에러 응답 형식을 통일해줘.
> 프론트엔드에서 처리하기 쉽게.
```

```javascript
// 일관된 에러 형식
{
  "error": true,
  "message": "할 일을 찾을 수 없습니다",
  "code": "NOT_FOUND"
}
```

---

## 안 되면? 문제 해결 팁

### CORS 오류: "Access-Control-Allow-Origin"

```javascript
// 백엔드: cors 미들웨어가 있는지 확인
const cors = require('cors')
app.use(cors())  // 라우트보다 먼저 추가하세요!

// 여전히 문제가 있으면 더 구체적으로:
app.use(cors({
  origin: 'http://localhost:5173',  // 프론트엔드 URL
  credentials: true
}))
```

### "Network Error" 또는 "Failed to Fetch"

```bash
# 백엔드가 실제로 실행 중인지 확인
curl http://localhost:3001/api/todos

# 응답이 없으면 백엔드가 실행 중이 아님
# 올바른 디렉토리에 있는지 확인
cd backend && node index.js
```

### 서버 재시작하면 데이터가 사라짐

메모리 저장은 그것이 정상입니다. 데이터베이스를 사용하시기 바랍니다:

```javascript
// SQLite 사용 (영구 저장)
const Database = require('better-sqlite3')
const db = new Database('mydata.db')

// 재시작해도 데이터가 유지됩니다
```

### 새 항목 추가 후에도 프론트엔드에 이전 데이터가 보임

```javascript
// API 호출 후 상태를 업데이트하고 있는지 확인
const addTodo = async () => {
  const res = await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ text: newTodo })
  })
  const todo = await res.json()

  // 상태 업데이트 - 잊지 마시기 바랍니다
  setTodos([todo, ...todos])
}
```

### 포트가 이미 사용 중이라는 오류

```bash
# 해당 포트를 사용 중인 프로세스 찾기
lsof -i :3001

# 종료하기 (PID를 실제 프로세스 ID로 대체)
kill -9 <PID>

# 또는 다른 포트 사용
app.listen(3002, ...)
```

---

## 자주 하는 실수

### 1. JSON Body 파싱을 빼먹음

```javascript
// 틀림 - req.body가 undefined됨
app.post('/api/todos', (req, res) => {
  console.log(req.body)  // undefined!
})

// 맞음 - JSON 미들웨어 추가
app.use(express.json())  // 라우트보다 먼저 추가
app.post('/api/todos', (req, res) => {
  console.log(req.body)  // 이제 작동함!
})
```

### 2. HTTP 메서드 혼동

```javascript
// GET: 데이터 조회 (body 없음)
// POST: 새 데이터 생성
// PUT: 리소스 전체 교체
// PATCH: 리소스 일부 수정
// DELETE: 데이터 삭제

// 틀림 - GET으로 데이터 생성
app.get('/api/todos/create', ...)

// 맞음 - 생성은 POST 사용
app.post('/api/todos', ...)
```

### 3. 프론트엔드에서 에러 처리 안 함

```javascript
// 틀림 - 에러 시 조용히 실패
const data = await fetch(API_URL).then(r => r.json())

// 맞음 - 에러를 우아하게 처리
try {
  const res = await fetch(API_URL)
  if (!res.ok) throw new Error('API 오류')
  const data = await res.json()
  setTodos(data)
} catch (err) {
  console.error('가져오기 실패:', err)
  setError('할 일을 불러올 수 없습니다')
}
```

### 4. API URL 하드코딩

```javascript
// 틀림 - 배포하면 안 됨
const API_URL = 'http://localhost:3001/api'

// 맞음 - 환경변수 사용
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3001/api'
```

### 5. 백엔드 입력값 검증 안 함

```javascript
// 틀림 - 누구나 아무거나 보낼 수 있음
app.post('/api/todos', (req, res) => {
  const { text } = req.body
  db.prepare('INSERT INTO todos (text) VALUES (?)').run(text)
})

// 맞음 - 먼저 검증
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  if (!text || typeof text !== 'string' || text.length > 500) {
    return res.status(400).json({ error: '유효하지 않은 할 일 텍스트' })
  }

  db.prepare('INSERT INTO todos (text) VALUES (?)').run(text.trim())
})
```

### 6. 프론트엔드와 백엔드를 같은 포트에서 실행

```bash
# 백엔드는 3001
# 프론트엔드 개발 서버는 5173 (Vite 기본값)
# 반드시 다른 포트여야 합니다

# 백엔드
app.listen(3001, ...)

# 프론트엔드가 백엔드에 연결
const API_URL = 'http://localhost:3001/api'
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 프론트엔드와 백엔드 구조
- [x] REST API 설계
- [x] 데이터베이스 연동
- [x] 인증 구현
- [x] 풀스택 앱 배포

풀스택 개발의 핵심은 **프론트/백엔드 간의 데이터 흐름**입니다:
1. 프론트엔드에서 요청
2. 백엔드에서 처리
3. 데이터베이스에 저장/조회
4. 응답 반환
5. 프론트엔드에서 표시

Claude에게 요청할 때:
1. 프론트/백엔드 기술 스택 명시
2. 필요한 API 엔드포인트 설명
3. 데이터 구조 설명

이 세 가지를 명확히 하면 전체 앱을 만들 수 있습니다.

축하합니다! Part 4 (실전 II)를 완료했습니다.

다음 Part에서는 Claude Code의 고급 기능을 배웁니다.

[Chapter 18: 고급 기능](../Chapter18/README.ko.md)으로 넘어가세요.
