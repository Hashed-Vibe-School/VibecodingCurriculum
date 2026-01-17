# Chapter 20: 풀스택 앱 완성하기 - 프론트엔드 연동과 인증

[English](./README.md) | **한국어**

---

## 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이전 챕터 복습

[Chapter 19: 백엔드 기초](../Chapter19-Backend-Basics/README.ko.md)에서 Express와 SQLite로 REST API를 만들었습니다.

이번 챕터에서는 React 프론트엔드를 연결하고, 사용자 인증까지 추가해서 **완전한 풀스택 앱**을 완성합니다!

---

## 이 챕터에서 배우는 것

- React와 백엔드 API 연결
- CORS 설정 이해
- 사용자 인증 시스템 (JWT)
- 완전한 풀스택 Todo 앱 만들기
- 배포 준비

---

## Part 1: 프론트엔드와 백엔드 연결 이해하기

### 두 개의 서버가 필요한 이유

풀스택 개발에서는 **두 개의 서버**가 각각 실행됩니다:

```
┌─────────────────────────────────────────────────────────────────┐
│                     풀스택 앱 구조                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  터미널 1 (프론트엔드)              터미널 2 (백엔드)             │
│  ━━━━━━━━━━━━━━━━━━                ━━━━━━━━━━━━━━━              │
│                                                                 │
│  ┌─────────────────┐              ┌─────────────────┐          │
│  │  React 개발서버  │    API      │   Express 서버   │          │
│  │  localhost:5173 │ ──────────▶ │  localhost:3001  │          │
│  │                 │    요청      │                 │          │
│  │  npm run dev    │ ◀────────── │  node index.js  │          │
│  │                 │    응답      │                 │          │
│  └─────────────────┘              └────────┬────────┘          │
│                                            │                    │
│                                     ┌──────▼──────┐            │
│                                     │   SQLite    │            │
│                                     │   todos.db  │            │
│                                     └─────────────┘            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **비전공자 팁**: 프론트엔드 서버(5173)는 React 코드를 브라우저에 전달하고, 백엔드 서버(3001)는 데이터를 처리합니다. 마치 식당에서 "홀 담당"과 "주방 담당"이 따로 있는 것처럼요!

### CORS란 무엇인가요?

브라우저는 기본적으로 **다른 출처(도메인, 포트)의 요청을 차단**합니다. 이것이 CORS(Cross-Origin Resource Sharing)입니다.

```
┌─────────────────────────────────────────────────────────────────┐
│                       CORS 문제 상황                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  localhost:5173 (프론트)         localhost:3001 (백엔드)         │
│  ━━━━━━━━━━━━━━━━━━━━━         ━━━━━━━━━━━━━━━━━━━━            │
│                                                                 │
│  fetch('/api/todos')                                            │
│        │                                                        │
│        ▼                                                        │
│  ❌ 브라우저 차단!                                               │
│     "포트가 달라서 안 돼요"                                       │
│     (5173 ≠ 3001)                                               │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  해결: 백엔드에 CORS 허용 설정                                    │
│                                                                 │
│  fetch('http://localhost:3001/api/todos')                       │
│        │                                                        │
│        ▼                                                        │
│  ✅ 백엔드가 "5173에서 온 요청 OK" 라고 응답                       │
│     → 브라우저가 허용                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### CORS 설정하기

백엔드에 `cors` 패키지를 설치하고 설정합니다:

```bash
npm install cors
```

```javascript
// backend/index.js
const express = require('express')
const cors = require('cors')  // 추가

const app = express()

// CORS 설정
app.use(cors())  // 모든 출처 허용 (개발용)

// 또는 특정 출처만 허용 (배포용)
app.use(cors({
  origin: 'http://localhost:5173',  // 프론트엔드 주소
  credentials: true                  // 쿠키 허용
}))

app.use(express.json())

// ... 나머지 API 코드
```

> ⚠️ **보안 주의**: `app.use(cors())`는 모든 출처를 허용합니다. 개발할 때는 편하지만, 배포할 때는 특정 도메인만 허용하세요!

---

## Part 2: 프로젝트 구조 만들기

### Step 1: 폴더 구조

```
todo-fullstack/
├── backend/                  # 백엔드 (이전 챕터에서 만든 것)
│   ├── index.js
│   ├── package.json
│   └── todos.db
│
├── frontend/                 # 프론트엔드 (새로 만들 것)
│   ├── src/
│   │   ├── App.jsx
│   │   ├── App.css
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
│
└── README.md
```

### Step 2: 프론트엔드 프로젝트 생성

```bash
# 프로젝트 루트 폴더에서
npm create vite@latest frontend -- --template react

# 프론트엔드 폴더로 이동
cd frontend

# 의존성 설치
npm install
```

### Step 3: 동시 실행 방법

**터미널 두 개**를 열어서:

```bash
# 터미널 1: 백엔드
cd backend
node index.js
# → "서버가 http://localhost:3001 에서 실행 중!"

# 터미널 2: 프론트엔드
cd frontend
npm run dev
# → "Local: http://localhost:5173"
```

---

## Part 3: 최소 연결 테스트

전체 앱을 만들기 전에, 프론트엔드와 백엔드가 제대로 통신하는지 확인해봅시다.

### 백엔드: 간단한 API 추가

```javascript
// backend/index.js에 추가
app.get('/api/health', (req, res) => {
  res.json({
    status: 'ok',
    message: '백엔드가 정상 작동 중입니다!',
    timestamp: new Date().toISOString()
  })
})
```

### 프론트엔드: 연결 테스트

```jsx
// frontend/src/App.jsx
import { useState, useEffect } from 'react'

function App() {
  const [status, setStatus] = useState('연결 확인 중...')
  const [error, setError] = useState(null)

  useEffect(() => {
    // 백엔드 API 호출
    fetch('http://localhost:3001/api/health')
      .then(response => {
        if (!response.ok) {
          throw new Error('서버 응답 오류')
        }
        return response.json()
      })
      .then(data => {
        setStatus(`✅ ${data.message}`)
      })
      .catch(err => {
        setError(`❌ 연결 실패: ${err.message}`)
        setStatus(null)
      })
  }, [])

  return (
    <div style={{ padding: '20px', fontFamily: 'sans-serif' }}>
      <h1>백엔드 연결 테스트</h1>
      {status && <p style={{ color: 'green' }}>{status}</p>}
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  )
}

export default App
```

### 🎯 실습 문제 3-1: 연결 확인

1. 백엔드와 프론트엔드를 모두 실행하세요
2. 브라우저에서 `http://localhost:5173`에 접속하세요
3. "백엔드가 정상 작동 중입니다!"가 보이면 성공!

**연결이 안 되면 확인할 것:**
- 백엔드가 실행 중인가? (터미널 확인)
- CORS 설정을 했는가?
- 포트 번호가 맞는가? (백엔드 3001, 프론트엔드 5173)

---

## Part 4: 완전한 Todo 앱 만들기

이제 진짜 Todo 앱을 만들어봅시다!

### 백엔드: 완전한 API

이전 챕터에서 만든 백엔드에 CORS만 추가하면 됩니다:

```javascript
// backend/index.js
const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')  // 추가!

const app = express()

// 미들웨어
app.use(cors())  // CORS 허용
app.use(express.json())

// 데이터베이스
const db = new Database('todos.db')
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// API 엔드포인트들 (이전 챕터 코드 그대로)
app.get('/api/todos', (req, res) => {
  const todos = db.prepare('SELECT * FROM todos ORDER BY created_at DESC').all()
  res.json(todos)
})

app.post('/api/todos', (req, res) => {
  const { text } = req.body
  if (!text || !text.trim()) {
    return res.status(400).json({ error: '할 일 내용을 입력해주세요' })
  }
  const result = db.prepare('INSERT INTO todos (text) VALUES (?)').run(text.trim())
  const newTodo = db.prepare('SELECT * FROM todos WHERE id = ?').get(result.lastInsertRowid)
  res.status(201).json(newTodo)
})

app.patch('/api/todos/:id', (req, res) => {
  const { id } = req.params
  const existing = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  if (!existing) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }
  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)
  const updated = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  res.json(updated)
})

app.delete('/api/todos/:id', (req, res) => {
  const { id } = req.params
  const result = db.prepare('DELETE FROM todos WHERE id = ?').run(id)
  if (result.changes === 0) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('🚀 백엔드 서버: http://localhost:3001')
})
```

### 프론트엔드: React Todo 앱

```jsx
// frontend/src/App.jsx
import { useState, useEffect } from 'react'
import './App.css'

// API 기본 URL
const API_URL = 'http://localhost:3001/api/todos'

function App() {
  // ═══════════════════════════════════════════════════════════════
  // 상태(State) 정의
  // ═══════════════════════════════════════════════════════════════
  const [todos, setTodos] = useState([])       // 할 일 목록
  const [newTodo, setNewTodo] = useState('')   // 새 할 일 입력값
  const [loading, setLoading] = useState(true) // 로딩 상태
  const [error, setError] = useState(null)     // 에러 메시지

  // ═══════════════════════════════════════════════════════════════
  // 페이지 로드 시 할 일 목록 가져오기
  // ═══════════════════════════════════════════════════════════════
  useEffect(() => {
    fetchTodos()
  }, [])

  // ═══════════════════════════════════════════════════════════════
  // API 호출 함수들
  // ═══════════════════════════════════════════════════════════════

  // 할 일 목록 조회
  const fetchTodos = async () => {
    try {
      setLoading(true)
      setError(null)

      const response = await fetch(API_URL)
      if (!response.ok) {
        throw new Error('서버 응답 오류')
      }

      const data = await response.json()
      setTodos(data)
    } catch (err) {
      setError('할 일을 불러오는데 실패했습니다. 백엔드가 실행 중인지 확인하세요.')
      console.error(err)
    } finally {
      setLoading(false)
    }
  }

  // 새 할 일 추가
  const addTodo = async (e) => {
    e.preventDefault()

    if (!newTodo.trim()) {
      setError('할 일 내용을 입력해주세요')
      return
    }

    try {
      setError(null)

      const response = await fetch(API_URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ text: newTodo })
      })

      if (!response.ok) {
        const errorData = await response.json()
        throw new Error(errorData.error || '추가 실패')
      }

      const todo = await response.json()
      setTodos([todo, ...todos])  // 목록 맨 앞에 추가
      setNewTodo('')              // 입력 필드 초기화
    } catch (err) {
      setError(err.message)
    }
  }

  // 완료 상태 토글
  const toggleTodo = async (id) => {
    try {
      setError(null)

      const response = await fetch(`${API_URL}/${id}`, {
        method: 'PATCH'
      })

      if (!response.ok) {
        throw new Error('상태 변경 실패')
      }

      // 로컬 상태 업데이트
      setTodos(todos.map(todo =>
        todo.id === id
          ? { ...todo, completed: todo.completed ? 0 : 1 }
          : todo
      ))
    } catch (err) {
      setError(err.message)
    }
  }

  // 할 일 삭제
  const deleteTodo = async (id) => {
    if (!window.confirm('정말 삭제하시겠습니까?')) {
      return
    }

    try {
      setError(null)

      const response = await fetch(`${API_URL}/${id}`, {
        method: 'DELETE'
      })

      if (!response.ok) {
        throw new Error('삭제 실패')
      }

      setTodos(todos.filter(todo => todo.id !== id))
    } catch (err) {
      setError(err.message)
    }
  }

  // ═══════════════════════════════════════════════════════════════
  // 화면 렌더링
  // ═══════════════════════════════════════════════════════════════
  return (
    <div className="app">
      <h1>📝 할 일 목록</h1>

      {/* 에러 메시지 */}
      {error && (
        <div className="error">
          ⚠️ {error}
          <button onClick={() => setError(null)}>✕</button>
        </div>
      )}

      {/* 할 일 추가 폼 */}
      <form onSubmit={addTodo} className="add-form">
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="새로운 할 일을 입력하세요..."
          maxLength={500}
        />
        <button type="submit" disabled={!newTodo.trim()}>
          추가
        </button>
      </form>

      {/* 로딩 표시 */}
      {loading ? (
        <div className="loading">불러오는 중...</div>
      ) : (
        <>
          {/* 할 일 목록 */}
          <ul className="todo-list">
            {todos.length === 0 ? (
              <li className="empty">
                할 일이 없습니다. 새로운 할 일을 추가해보세요!
              </li>
            ) : (
              todos.map(todo => (
                <li key={todo.id} className={todo.completed ? 'completed' : ''}>
                  <input
                    type="checkbox"
                    checked={!!todo.completed}
                    onChange={() => toggleTodo(todo.id)}
                  />
                  <span className="text">{todo.text}</span>
                  <button
                    className="delete"
                    onClick={() => deleteTodo(todo.id)}
                  >
                    🗑️
                  </button>
                </li>
              ))
            )}
          </ul>

          {/* 통계 */}
          {todos.length > 0 && (
            <div className="stats">
              전체: {todos.length}개 |
              완료: {todos.filter(t => t.completed).length}개 |
              미완료: {todos.filter(t => !t.completed).length}개
            </div>
          )}
        </>
      )}
    </div>
  )
}

export default App
```

### CSS 스타일

```css
/* frontend/src/App.css */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  padding: 20px;
}

.app {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

/* 에러 메시지 */
.error {
  background: #fee;
  border: 1px solid #fcc;
  color: #c00;
  padding: 10px 15px;
  border-radius: 8px;
  margin-bottom: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.error button {
  background: none;
  border: none;
  color: #c00;
  cursor: pointer;
  font-size: 18px;
}

/* 입력 폼 */
.add-form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-form input {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.add-form input:focus {
  outline: none;
  border-color: #667eea;
}

.add-form button {
  padding: 12px 24px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, opacity 0.2s;
}

.add-form button:hover:not(:disabled) {
  transform: translateY(-2px);
}

.add-form button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* 로딩 */
.loading {
  text-align: center;
  padding: 40px;
  color: #666;
}

/* 할 일 목록 */
.todo-list {
  list-style: none;
}

.todo-list li {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 15px;
  border-bottom: 1px solid #eee;
  transition: background-color 0.2s;
}

.todo-list li:hover {
  background-color: #f9f9f9;
}

.todo-list li:last-child {
  border-bottom: none;
}

.todo-list li.completed .text {
  text-decoration: line-through;
  color: #999;
}

.todo-list input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.todo-list .text {
  flex: 1;
  font-size: 16px;
}

.todo-list .delete {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  opacity: 0.5;
  transition: opacity 0.2s;
}

.todo-list .delete:hover {
  opacity: 1;
}

.empty {
  text-align: center;
  color: #999;
  padding: 40px;
  font-style: italic;
}

/* 통계 */
.stats {
  text-align: center;
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #eee;
  color: #666;
  font-size: 14px;
}
```

### 🎯 실습 문제 4-1: Todo 앱 테스트

1. 백엔드와 프론트엔드를 모두 실행하세요
2. 새 할 일을 추가해보세요
3. 체크박스로 완료 표시해보세요
4. 삭제해보세요
5. 브라우저를 새로고침해도 데이터가 유지되는지 확인하세요!

---

## Part 5: async/await 이해하기

API 호출 코드에서 `async/await`를 많이 사용했습니다. 이것이 무엇인지 알아봅시다.

### 왜 async/await가 필요한가요?

서버에 요청을 보내면 응답이 올 때까지 **시간이 걸립니다**. 이 시간 동안 기다리지 않고 다음 코드를 실행하면 문제가 생깁니다.

```javascript
// ❌ 잘못된 예시 - 응답을 기다리지 않음
function badExample() {
  const response = fetch('/api/todos')  // 요청 시작
  console.log(response)  // 아직 응답 안 옴!
  // 결과: Promise { <pending> }
}

// ✅ 올바른 예시 - async/await 사용
async function goodExample() {
  const response = await fetch('/api/todos')  // 응답 올 때까지 기다림
  const data = await response.json()          // JSON 변환도 기다림
  console.log(data)  // 실제 데이터!
}
```

### async/await 규칙

1. **await**는 **async 함수 안에서만** 사용 가능
2. **await**는 Promise를 반환하는 함수 앞에 붙임
3. **await** 줄에서 결과가 올 때까지 기다림

```javascript
// async 함수 선언
async function fetchData() {
  // await로 기다리기
  const response = await fetch('/api/data')
  const data = await response.json()
  return data
}

// 화살표 함수 버전
const fetchData = async () => {
  const response = await fetch('/api/data')
  const data = await response.json()
  return data
}
```

### 에러 처리: try-catch

```javascript
async function fetchWithErrorHandling() {
  try {
    const response = await fetch('/api/data')

    // HTTP 에러 체크 (404, 500 등)
    if (!response.ok) {
      throw new Error(`HTTP 에러: ${response.status}`)
    }

    const data = await response.json()
    return data

  } catch (error) {
    // 네트워크 에러 또는 위에서 throw한 에러
    console.error('에러 발생:', error.message)
    throw error  // 에러를 다시 던져서 호출자가 처리하게 함
  }
}
```

> 💡 **비전공자 팁**: `async/await`는 "기다렸다가 결과 받기" 문법이에요. 편지를 보내고 답장을 기다리는 것처럼, 서버에 요청을 보내고 응답을 기다리는 거예요!

---

## Part 6: 사용자 인증 추가하기

이제 로그인 기능을 추가해봅시다. 각 사용자가 자신만의 할 일 목록을 가질 수 있게 됩니다.

### 인증이 필요한 이유

```
┌─────────────────────────────────────────────────────────────────┐
│                   인증 없음 vs 인증 있음                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ❌ 인증 없음                        ✅ 인증 있음                 │
│                                                                 │
│  ┌─────────────┐                    ┌─────────────┐            │
│  │ 모든 사용자  │                    │   철수      │ → 로그인    │
│  │   공용 데이터│                    │   영희      │ → 로그인    │
│  │             │                    │   민수      │ → 로그인    │
│  └─────────────┘                    └─────────────┘            │
│        │                                   │                    │
│        ▼                                   ▼                    │
│  ┌─────────────┐                    ┌─────────────┐            │
│  │   할 일:    │                    │ 철수의 할 일│            │
│  │ - 장보기   │                     │ - 장보기   │            │
│  │ - 운동하기 │                     └─────────────┘            │
│  │ - 공부하기 │                     ┌─────────────┐            │
│  │ (누구 거?) │                     │ 영희의 할 일│            │
│  └─────────────┘                    │ - 운동하기 │            │
│                                      └─────────────┘            │
│  "내 데이터 누가 봤지?"               "내 데이터는 나만!"        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### JWT(JSON Web Token) 인증 방식

JWT는 사용자 정보를 담은 암호화된 문자열입니다.

```
┌─────────────────────────────────────────────────────────────────┐
│                      JWT 인증 흐름                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1️⃣ 로그인 요청                                                  │
│     ┌────────┐   이메일 + 비밀번호    ┌────────┐                │
│     │  사용자 │ ────────────────────▶│  서버   │                │
│     └────────┘                       └────────┘                │
│                                           │                     │
│  2️⃣ 토큰 발급                              │                     │
│     ┌────────┐    JWT 토큰           ┌────▼────┐               │
│     │  사용자 │ ◀───────────────────│  서버   │                │
│     └────────┘    "eyJhbGc..."       └─────────┘               │
│         │                                                       │
│  3️⃣ 토큰 저장 (브라우저)                                          │
│         ▼                                                       │
│     localStorage.setItem('token', 'eyJhbGc...')                 │
│                                                                 │
│  4️⃣ 이후 모든 요청에 토큰 포함                                     │
│     ┌────────┐  요청 + "Bearer 토큰"   ┌────────┐              │
│     │  사용자 │ ────────────────────▶│  서버   │               │
│     └────────┘  Authorization 헤더    └────────┘               │
│                                           │                     │
│  5️⃣ 서버가 토큰 검증                        │                     │
│                                      ┌────▼────┐               │
│     토큰 유효 → 요청 처리             │토큰 검증 │               │
│     토큰 무효 → 401 에러 반환         └─────────┘               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 백엔드: 인증 API 추가

먼저 필요한 패키지를 설치합니다:

```bash
cd backend
npm install bcrypt jsonwebtoken
```

```javascript
// backend/index.js - 인증 기능 추가

const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')
const bcrypt = require('bcrypt')      // 비밀번호 암호화
const jwt = require('jsonwebtoken')    // JWT 토큰

const app = express()
app.use(cors())
app.use(express.json())

// ⚠️ 실제 서비스에서는 환경변수로 관리하세요!
const JWT_SECRET = 'your-super-secret-key-change-in-production'

const db = new Database('todos.db')

// 테이블 생성
db.exec(`
  -- 사용자 테이블
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    email TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    name TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  );

  -- 할 일 테이블 (user_id 추가)
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    user_id INTEGER NOT NULL,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
  )
`)

// ═══════════════════════════════════════════════════════════════
// 인증 미들웨어 - 보호된 라우트에서 사용
// ═══════════════════════════════════════════════════════════════
function authMiddleware(req, res, next) {
  // Authorization 헤더에서 토큰 추출
  const authHeader = req.headers.authorization
  const token = authHeader?.split(' ')[1]  // "Bearer 토큰" → "토큰"

  if (!token) {
    return res.status(401).json({ error: '로그인이 필요합니다' })
  }

  try {
    // 토큰 검증
    const decoded = jwt.verify(token, JWT_SECRET)
    req.userId = decoded.userId
    req.userEmail = decoded.email
    next()
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({ error: '세션이 만료되었습니다' })
    }
    return res.status(401).json({ error: '유효하지 않은 토큰입니다' })
  }
}

// ═══════════════════════════════════════════════════════════════
// 회원가입 API
// ═══════════════════════════════════════════════════════════════
app.post('/api/register', async (req, res) => {
  const { email, password, name } = req.body

  // 입력값 검증
  if (!email || !password) {
    return res.status(400).json({ error: '이메일과 비밀번호를 입력해주세요' })
  }

  if (password.length < 6) {
    return res.status(400).json({ error: '비밀번호는 6자 이상이어야 합니다' })
  }

  // 이메일 중복 확인
  const existing = db.prepare('SELECT id FROM users WHERE email = ?').get(email)
  if (existing) {
    return res.status(409).json({ error: '이미 가입된 이메일입니다' })
  }

  try {
    // 비밀번호 암호화 (해시)
    const hashedPassword = await bcrypt.hash(password, 10)

    // 사용자 저장
    const result = db.prepare(
      'INSERT INTO users (email, password, name) VALUES (?, ?, ?)'
    ).run(email, hashedPassword, name || email.split('@')[0])

    res.status(201).json({
      message: '회원가입이 완료되었습니다',
      userId: result.lastInsertRowid
    })
  } catch (error) {
    console.error('회원가입 오류:', error)
    res.status(500).json({ error: '회원가입에 실패했습니다' })
  }
})

// ═══════════════════════════════════════════════════════════════
// 로그인 API
// ═══════════════════════════════════════════════════════════════
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body

  if (!email || !password) {
    return res.status(400).json({ error: '이메일과 비밀번호를 입력해주세요' })
  }

  // 사용자 조회
  const user = db.prepare('SELECT * FROM users WHERE email = ?').get(email)

  // 사용자가 없거나 비밀번호가 틀리면
  if (!user || !await bcrypt.compare(password, user.password)) {
    return res.status(401).json({ error: '이메일 또는 비밀번호가 올바르지 않습니다' })
  }

  // JWT 토큰 생성
  const token = jwt.sign(
    { userId: user.id, email: user.email },
    JWT_SECRET,
    { expiresIn: '7d' }  // 7일 후 만료
  )

  res.json({
    message: '로그인 성공',
    token,
    user: {
      id: user.id,
      email: user.email,
      name: user.name
    }
  })
})

// ═══════════════════════════════════════════════════════════════
// 보호된 API - 인증 필요
// ═══════════════════════════════════════════════════════════════

// 자신의 할 일 목록 조회
app.get('/api/todos', authMiddleware, (req, res) => {
  const todos = db.prepare(
    'SELECT * FROM todos WHERE user_id = ? ORDER BY created_at DESC'
  ).all(req.userId)

  res.json(todos)
})

// 새 할 일 추가 (자신의 것으로)
app.post('/api/todos', authMiddleware, (req, res) => {
  const { text } = req.body

  if (!text || !text.trim()) {
    return res.status(400).json({ error: '할 일 내용을 입력해주세요' })
  }

  const result = db.prepare(
    'INSERT INTO todos (text, user_id) VALUES (?, ?)'
  ).run(text.trim(), req.userId)

  const newTodo = db.prepare('SELECT * FROM todos WHERE id = ?')
    .get(result.lastInsertRowid)

  res.status(201).json(newTodo)
})

// 완료 토글 (자신의 것만)
app.patch('/api/todos/:id', authMiddleware, (req, res) => {
  const { id } = req.params

  // 자신의 할 일인지 확인
  const todo = db.prepare(
    'SELECT * FROM todos WHERE id = ? AND user_id = ?'
  ).get(id, req.userId)

  if (!todo) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }

  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)

  const updated = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  res.json(updated)
})

// 삭제 (자신의 것만)
app.delete('/api/todos/:id', authMiddleware, (req, res) => {
  const { id } = req.params

  const result = db.prepare(
    'DELETE FROM todos WHERE id = ? AND user_id = ?'
  ).run(id, req.userId)

  if (result.changes === 0) {
    return res.status(404).json({ error: '할 일을 찾을 수 없습니다' })
  }

  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('🚀 서버 실행 중: http://localhost:3001')
})
```

> 💡 **bcrypt란?** 비밀번호를 "해시"로 변환하는 라이브러리입니다. 해시는 원본을 알 수 없는 암호화된 문자열이에요. 데이터베이스가 해킹당해도 실제 비밀번호를 알 수 없게 보호합니다!

### 프론트엔드: 로그인 기능 추가

```jsx
// frontend/src/App.jsx - 인증 기능 추가 버전
import { useState, useEffect } from 'react'
import './App.css'

const API_URL = 'http://localhost:3001/api'

function App() {
  // ═══════════════════════════════════════════════════════════════
  // 상태
  // ═══════════════════════════════════════════════════════════════
  const [user, setUser] = useState(null)           // 로그인한 사용자
  const [todos, setTodos] = useState([])           // 할 일 목록
  const [newTodo, setNewTodo] = useState('')       // 새 할 일
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  // 로그인/회원가입 폼 상태
  const [isLoginMode, setIsLoginMode] = useState(true)
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [name, setName] = useState('')

  // ═══════════════════════════════════════════════════════════════
  // 토큰이 있으면 자동 로그인
  // ═══════════════════════════════════════════════════════════════
  useEffect(() => {
    const token = localStorage.getItem('token')
    const savedUser = localStorage.getItem('user')

    if (token && savedUser) {
      setUser(JSON.parse(savedUser))
    }
    setLoading(false)
  }, [])

  // 로그인 후 할 일 목록 불러오기
  useEffect(() => {
    if (user) {
      fetchTodos()
    }
  }, [user])

  // ═══════════════════════════════════════════════════════════════
  // 인증된 API 호출 헬퍼
  // ═══════════════════════════════════════════════════════════════
  const authFetch = async (url, options = {}) => {
    const token = localStorage.getItem('token')

    const response = await fetch(url, {
      ...options,
      headers: {
        ...options.headers,
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      }
    })

    // 토큰 만료 시 로그아웃
    if (response.status === 401) {
      logout()
      throw new Error('세션이 만료되었습니다. 다시 로그인해주세요.')
    }

    return response
  }

  // ═══════════════════════════════════════════════════════════════
  // 인증 함수들
  // ═══════════════════════════════════════════════════════════════

  // 회원가입
  const register = async (e) => {
    e.preventDefault()
    setError(null)

    try {
      const response = await fetch(`${API_URL}/register`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password, name })
      })

      const data = await response.json()

      if (!response.ok) {
        throw new Error(data.error)
      }

      // 회원가입 성공 → 로그인 모드로 전환
      setIsLoginMode(true)
      setError(null)
      alert('회원가입이 완료되었습니다. 로그인해주세요!')
    } catch (err) {
      setError(err.message)
    }
  }

  // 로그인
  const login = async (e) => {
    e.preventDefault()
    setError(null)

    try {
      const response = await fetch(`${API_URL}/login`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password })
      })

      const data = await response.json()

      if (!response.ok) {
        throw new Error(data.error)
      }

      // 토큰과 사용자 정보 저장
      localStorage.setItem('token', data.token)
      localStorage.setItem('user', JSON.stringify(data.user))
      setUser(data.user)

      // 폼 초기화
      setEmail('')
      setPassword('')
    } catch (err) {
      setError(err.message)
    }
  }

  // 로그아웃
  const logout = () => {
    localStorage.removeItem('token')
    localStorage.removeItem('user')
    setUser(null)
    setTodos([])
  }

  // ═══════════════════════════════════════════════════════════════
  // Todo API 함수들
  // ═══════════════════════════════════════════════════════════════

  const fetchTodos = async () => {
    try {
      const response = await authFetch(`${API_URL}/todos`)
      const data = await response.json()
      setTodos(data)
    } catch (err) {
      setError(err.message)
    }
  }

  const addTodo = async (e) => {
    e.preventDefault()
    if (!newTodo.trim()) return

    try {
      const response = await authFetch(`${API_URL}/todos`, {
        method: 'POST',
        body: JSON.stringify({ text: newTodo })
      })

      if (!response.ok) throw new Error('추가 실패')

      const todo = await response.json()
      setTodos([todo, ...todos])
      setNewTodo('')
    } catch (err) {
      setError(err.message)
    }
  }

  const toggleTodo = async (id) => {
    try {
      await authFetch(`${API_URL}/todos/${id}`, { method: 'PATCH' })
      setTodos(todos.map(t =>
        t.id === id ? { ...t, completed: t.completed ? 0 : 1 } : t
      ))
    } catch (err) {
      setError(err.message)
    }
  }

  const deleteTodo = async (id) => {
    if (!window.confirm('삭제하시겠습니까?')) return

    try {
      await authFetch(`${API_URL}/todos/${id}`, { method: 'DELETE' })
      setTodos(todos.filter(t => t.id !== id))
    } catch (err) {
      setError(err.message)
    }
  }

  // ═══════════════════════════════════════════════════════════════
  // 로딩 중
  // ═══════════════════════════════════════════════════════════════
  if (loading) {
    return <div className="app"><div className="loading">로딩 중...</div></div>
  }

  // ═══════════════════════════════════════════════════════════════
  // 로그인되지 않은 상태 → 로그인/회원가입 폼
  // ═══════════════════════════════════════════════════════════════
  if (!user) {
    return (
      <div className="app">
        <h1>📝 할 일 목록</h1>

        {error && (
          <div className="error">
            ⚠️ {error}
            <button onClick={() => setError(null)}>✕</button>
          </div>
        )}

        <form onSubmit={isLoginMode ? login : register} className="auth-form">
          <h2>{isLoginMode ? '로그인' : '회원가입'}</h2>

          {!isLoginMode && (
            <input
              type="text"
              placeholder="이름 (선택)"
              value={name}
              onChange={(e) => setName(e.target.value)}
            />
          )}

          <input
            type="email"
            placeholder="이메일"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            required
          />

          <input
            type="password"
            placeholder="비밀번호 (6자 이상)"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
            minLength={6}
          />

          <button type="submit">
            {isLoginMode ? '로그인' : '회원가입'}
          </button>

          <p className="switch-mode">
            {isLoginMode ? '계정이 없으신가요? ' : '이미 계정이 있으신가요? '}
            <button
              type="button"
              onClick={() => {
                setIsLoginMode(!isLoginMode)
                setError(null)
              }}
            >
              {isLoginMode ? '회원가입' : '로그인'}
            </button>
          </p>
        </form>
      </div>
    )
  }

  // ═══════════════════════════════════════════════════════════════
  // 로그인된 상태 → Todo 앱
  // ═══════════════════════════════════════════════════════════════
  return (
    <div className="app">
      <header className="header">
        <h1>📝 {user.name || user.email}님의 할 일</h1>
        <button onClick={logout} className="logout-btn">로그아웃</button>
      </header>

      {error && (
        <div className="error">
          ⚠️ {error}
          <button onClick={() => setError(null)}>✕</button>
        </div>
      )}

      <form onSubmit={addTodo} className="add-form">
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="새로운 할 일을 입력하세요..."
        />
        <button type="submit" disabled={!newTodo.trim()}>추가</button>
      </form>

      <ul className="todo-list">
        {todos.length === 0 ? (
          <li className="empty">할 일이 없습니다</li>
        ) : (
          todos.map(todo => (
            <li key={todo.id} className={todo.completed ? 'completed' : ''}>
              <input
                type="checkbox"
                checked={!!todo.completed}
                onChange={() => toggleTodo(todo.id)}
              />
              <span className="text">{todo.text}</span>
              <button className="delete" onClick={() => deleteTodo(todo.id)}>
                🗑️
              </button>
            </li>
          ))
        )}
      </ul>

      {todos.length > 0 && (
        <div className="stats">
          전체: {todos.length} | 완료: {todos.filter(t => t.completed).length}
        </div>
      )}
    </div>
  )
}

export default App
```

### 추가 CSS

```css
/* frontend/src/App.css에 추가 */

/* 헤더 */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.header h1 {
  margin: 0;
  font-size: 1.5rem;
}

.logout-btn {
  padding: 8px 16px;
  background: #f44336;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
}

.logout-btn:hover {
  background: #d32f2f;
}

/* 인증 폼 */
.auth-form {
  max-width: 300px;
  margin: 0 auto;
}

.auth-form h2 {
  text-align: center;
  margin-bottom: 20px;
}

.auth-form input {
  width: 100%;
  padding: 12px;
  margin-bottom: 12px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 16px;
}

.auth-form input:focus {
  outline: none;
  border-color: #667eea;
}

.auth-form button[type="submit"] {
  width: 100%;
  padding: 12px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
}

.switch-mode {
  text-align: center;
  margin-top: 15px;
  color: #666;
}

.switch-mode button {
  background: none;
  border: none;
  color: #667eea;
  cursor: pointer;
  font-weight: 600;
}
```

### 🎯 실습 문제 6-1: 인증 테스트

1. 회원가입을 해보세요
2. 로그인해보세요
3. 할 일을 추가해보세요
4. 로그아웃 후 다른 계정으로 로그인해보세요
5. 이전 계정의 할 일이 보이지 않는지 확인하세요!

---

## Part 7: 배포 준비

앱이 완성되었으니 배포 준비를 해봅시다.

### 환경변수 설정

비밀키나 설정값은 코드에 직접 쓰면 안 됩니다!

```bash
# backend/.env 파일 생성
JWT_SECRET=your-super-secret-key-at-least-32-characters-long
PORT=3001
```

```javascript
// backend/index.js 수정
require('dotenv').config()  // 맨 위에 추가

const JWT_SECRET = process.env.JWT_SECRET || 'fallback-dev-key'
const PORT = process.env.PORT || 3001
```

```bash
# dotenv 패키지 설치
npm install dotenv
```

> ⚠️ **중요**: `.env` 파일은 `.gitignore`에 추가해서 Git에 올라가지 않게 하세요!

### 배포 플랫폼 추천

| 서비스 | 용도 | 무료 여부 |
|--------|------|----------|
| **Vercel** | 프론트엔드 | 무료 |
| **Netlify** | 프론트엔드 | 무료 |
| **Railway** | 백엔드 | $5/월 크레딧 |
| **Render** | 백엔드 | 무료 (제한적) |
| **Fly.io** | 백엔드 | 무료 (제한적) |

### 배포 전 체크리스트

- [ ] 환경변수 설정 (.env)
- [ ] .gitignore에 민감한 파일 추가
- [ ] CORS 설정 (프론트엔드 도메인만 허용)
- [ ] 프론트엔드 빌드 테스트 (`npm run build`)
- [ ] 백엔드 에러 로깅 설정

---

## 🎯 미니 퀴즈

### 퀴즈 1: CORS

CORS 오류가 발생하는 이유는?

A) 서버가 꺼져 있어서
B) URL이 잘못되어서
C) 프론트엔드와 백엔드의 출처(포트)가 달라서
D) JSON 형식이 잘못되어서

<details>
<summary>정답 보기</summary>

**정답: C) 프론트엔드와 백엔드의 출처(포트)가 달라서**

브라우저는 보안상 다른 출처(도메인, 포트)의 요청을 기본적으로 차단합니다.
프론트엔드(5173)와 백엔드(3001)의 포트가 다르면 CORS 에러가 발생해요.

</details>

### 퀴즈 2: JWT

JWT 토큰을 어디에 저장하나요?

A) 데이터베이스
B) 서버 메모리
C) 브라우저 localStorage
D) URL 파라미터

<details>
<summary>정답 보기</summary>

**정답: C) 브라우저 localStorage**

JWT 토큰은 로그인 후 브라우저의 localStorage에 저장합니다.
이후 API 요청 시 Authorization 헤더에 포함해서 보내요.

</details>

### 퀴즈 3: API 호출

다음 코드의 문제점은?

```javascript
const response = await fetch('/api/todos', {
  method: 'POST',
  body: JSON.stringify({ text: 'Hello' })
})
```

A) fetch 사용법이 틀림
B) await를 잘못 사용함
C) Content-Type 헤더가 없음
D) 문제없음

<details>
<summary>정답 보기</summary>

**정답: C) Content-Type 헤더가 없음**

JSON을 보낼 때는 반드시 `'Content-Type': 'application/json'` 헤더를 추가해야 합니다.
이 헤더가 없으면 서버의 `req.body`가 undefined가 됩니다.

</details>

---

## 📝 실습 과제

### 기본 과제: 메모 앱으로 확장

Todo 앱을 메모 앱으로 확장해보세요:
- 제목과 내용이 있는 메모
- 메모 수정 기능
- 메모 검색 기능

### 심화 과제: 추가 기능

다음 기능을 추가해보세요:
- 카테고리/태그 분류
- 마감일 설정
- 중요도 표시 (별표)

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **풀스택** | 프론트엔드와 백엔드를 모두 개발하는 것 |
| **CORS** | 다른 출처의 리소스 공유를 제어하는 보안 메커니즘 |
| **JWT** | JSON Web Token, 사용자 인증 정보를 담은 암호화된 토큰 |
| **bcrypt** | 비밀번호를 안전하게 해시하는 라이브러리 |
| **async/await** | 비동기 코드를 동기적으로 작성할 수 있게 해주는 문법 |
| **Authorization 헤더** | HTTP 요청에서 인증 정보를 담는 헤더 |
| **localStorage** | 브라우저에 데이터를 영구 저장하는 공간 |

---

## 다음 챕터 미리보기

[Chapter 21: Claude Code 아키텍처](../Chapter21-Architecture/README.ko.md)에서는:
- Claude Code의 내부 동작 원리
- 도구와 서브에이전트
- 컨텍스트 관리

풀스택 개발을 마스터했으니, 이제 Claude Code를 더 깊이 이해해봅시다!

---

## 요약

1. **풀스택 앱**은 프론트엔드와 백엔드가 API로 통신
2. **CORS** 설정으로 다른 출처의 요청 허용
3. **async/await**로 비동기 API 호출 처리
4. **JWT**로 사용자 인증 구현
5. **bcrypt**로 비밀번호 안전하게 저장
6. 배포 전 **환경변수**로 비밀키 관리

축하합니다! 이제 완전한 풀스택 앱을 만들 수 있게 되었습니다! 🎉

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [React 공식 문서 (한글)](https://ko.react.dev/) - React 공식 한글 문서
- [Vite 가이드](https://vitejs.dev/guide/) - Vite 빌드 도구
- [JWT 소개](https://jwt.io/introduction) - JWT 토큰 이해하기

**영상 자료:**
- [풀스택 앱 만들기 (YouTube)](https://www.youtube.com/results?search_query=fullstack+react+express+tutorial)
- [React + Express 연동 (YouTube)](https://www.youtube.com/results?search_query=react+express+integration+tutorial)
- [JWT 인증 구현 (YouTube)](https://www.youtube.com/results?search_query=jwt+authentication+nodejs+react+tutorial)

**읽을거리:**
- [CORS 완벽 가이드](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS) - MDN CORS 문서
- [JWT 베스트 프랙티스](https://auth0.com/blog/jwt-authentication-best-practices/) - Auth0 JWT 가이드
- [React Hooks 완벽 가이드](https://overreacted.io/a-complete-guide-to-useeffect/) - useEffect 심화

**배포 플랫폼:**
- [Vercel](https://vercel.com/) - 프론트엔드 배포 (무료)
- [Netlify](https://www.netlify.com/) - 프론트엔드 배포 (무료)
- [Railway](https://railway.app/) - 풀스택 배포
- [Render](https://render.com/) - 백엔드 배포 (무료 티어)
- [Fly.io](https://fly.io/) - 글로벌 앱 배포

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
