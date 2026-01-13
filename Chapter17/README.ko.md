# Chapter 17: 풀스택 앱 만들기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 프론트엔드와 백엔드를 함께 만드는 방법
- API 설계와 구현
- 데이터 저장과 조회

---

## 왜 풀스택인가?

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
