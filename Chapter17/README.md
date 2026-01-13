# Chapter 17: Building Full-Stack Apps

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Building frontend and backend together
- API design and implementation
- Data storage and retrieval

---

## Why Full-Stack?

Most projects we've built so far had only frontend. But real services need to:
- Store data from multiple users
- Persist data permanently
- Have secure logic

You need a backend to handle these things.

**Full-stack request tips:**

```
> Create a bookmark saving app.
> Frontend: React,
> Backend: Express,
> Database: SQLite.
>
> Features:
> - Add bookmark (URL, title)
> - View bookmark list
> - Delete bookmark
```

Specify frontend/backend tech stack and required features to get the full structure.

---

## Project: Todo Management App

Let's build a simple but complete full-stack app.

### Step 1: Start the Project

```
> Create a full-stack todo app.
>
> Structure:
> - frontend/ (React + Vite)
> - backend/ (Express + SQLite)
>
> Just set up the folder structure first.
```

Structure Claude creates:
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

### Step 2: Create Backend API

```
> Create a todo API in the backend.
>
> Endpoints:
> GET /api/todos - Get all todos
> POST /api/todos - Add new todo
> PATCH /api/todos/:id - Toggle completion status
> DELETE /api/todos/:id - Delete todo
>
> Store data in SQLite.
```

API Claude creates:

```javascript
// backend/index.js
const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')

const app = express()
const db = new Database('todos.db')

app.use(cors())
app.use(express.json())

// Create table
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// Get all
app.get('/api/todos', (req, res) => {
  const todos = db.prepare('SELECT * FROM todos ORDER BY created_at DESC').all()
  res.json(todos)
})

// Add
app.post('/api/todos', (req, res) => {
  const { text } = req.body
  const result = db.prepare('INSERT INTO todos (text) VALUES (?)').run(text)
  res.json({ id: result.lastInsertRowid, text, completed: 0 })
})

// Toggle
app.patch('/api/todos/:id', (req, res) => {
  const { id } = req.params
  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)
  res.json({ success: true })
})

// Delete
app.delete('/api/todos/:id', (req, res) => {
  const { id } = req.params
  db.prepare('DELETE FROM todos WHERE id = ?').run(id)
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('Server running on http://localhost:3001')
})
```

### Step 3: Connect Frontend

```
> Create code in the frontend that calls the backend API.
>
> - Display todo list
> - Form to add new todo
> - Checkbox to toggle completion
> - Delete button
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
      <h1>Todo List</h1>
      <input
        value={newTodo}
        onChange={e => setNewTodo(e.target.value)}
        placeholder="Enter todo"
      />
      <button onClick={addTodo}>Add</button>

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
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  )
}

export default App
```

### Step 4: Run

```bash
# Terminal 1: Backend
cd backend && npm install && node index.js

# Terminal 2: Frontend
cd frontend && npm install && npm run dev
```

---

## Extending Features

### Add Categories

```
> Add category feature to todos.
> - Create/delete categories
> - Assign category to todo
> - Filter by category
```

### Due Dates

```
> Add ability to set due dates for todos.
> - Date picker UI
> - Sort by due date
> - Highlight items due today
```

### Search Feature

```
> Add todo search feature.
> - Search by text
> - Highlight search terms
```

---

## Second Project: Notes App

Let's build a notes app with markdown support.

### Basic Setup

```
> Create a markdown notes app.
>
> Frontend: React
> Backend: Express + SQLite
>
> Features:
> - Write notes (markdown support)
> - Notes list (sidebar)
> - Real-time preview
> - Delete notes
```

### Markdown Rendering

```
> Use react-markdown library for markdown rendering.
> Apply syntax highlighting to code blocks too.
```

### Auto Save

```
> Auto-save while writing notes.
> Save 1 second after typing stops.
> Show saving status indicator.
```

---

## Adding Authentication

Real services need login.

### Simple Auth

```
> Add simple login feature.
> - Signup (email, password)
> - Login
> - Use JWT tokens
> - Logged in users only see their own todos
```

### Backend Auth

```javascript
// Login API
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body
  const user = db.prepare('SELECT * FROM users WHERE email = ?').get(email)

  if (!user || !await bcrypt.compare(password, user.password)) {
    return res.status(401).json({ error: 'Login failed' })
  }

  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET)
  res.json({ token })
})

// Auth middleware
function auth(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1]
  if (!token) return res.status(401).json({ error: 'Auth required' })

  try {
    const { userId } = jwt.verify(token, process.env.JWT_SECRET)
    req.userId = userId
    next()
  } catch {
    res.status(401).json({ error: 'Invalid token' })
  }
}

// Protected route
app.get('/api/todos', auth, (req, res) => {
  const todos = db.prepare('SELECT * FROM todos WHERE user_id = ?').all(req.userId)
  res.json(todos)
})
```

---

## Deployment

### Deploying Full-Stack Apps

```
> I want to deploy this full-stack app.
> Show me how to deploy frontend to Vercel,
> backend to Railway.
```

### Environment Variables

```
> List the environment variables needed for production.
> - Database connection
> - JWT secret
> - Frontend URL
```

### Deployment Checklist

Ask Claude:
```
> Tell me what to check before deployment.
> - Security
> - Performance
> - Error handling
```

---

## Practice

### Basic Task

```
> Create a bookmark management app.
>
> Features:
> - Add bookmark with URL and title
> - View bookmark list
> - Categorize with tags
> - Search
>
> Tech: React + Express + SQLite
```

### Advanced Challenges

```
> Create a simple blog system.
>
> - Create/edit/delete posts
> - Markdown support
> - Categories
> - Simple admin auth
```

```
> Create a file sharing service.
>
> - File upload
> - Generate share link
> - Download counter
> - Set expiration time
```

---

## API Design Tips

### RESTful Design

```
> Explain RESTful API design principles.
> - Resource naming
> - HTTP methods
> - Status codes
```

| Method | Purpose | Example |
|--------|---------|---------|
| GET | Read | GET /api/todos |
| POST | Create | POST /api/todos |
| PUT | Full update | PUT /api/todos/1 |
| PATCH | Partial update | PATCH /api/todos/1 |
| DELETE | Delete | DELETE /api/todos/1 |

### Error Handling

```
> Standardize the API error response format.
> Make it easy for frontend to handle.
```

```javascript
// Consistent error format
{
  "error": true,
  "message": "Todo not found",
  "code": "NOT_FOUND"
}
```

---

## Summary

What you learned in this chapter:
- [x] Frontend and backend structure
- [x] REST API design
- [x] Database integration
- [x] Authentication implementation
- [x] Full-stack app deployment

The core of full-stack development is **data flow between frontend and backend**:
1. Request from frontend
2. Process in backend
3. Save/retrieve from database
4. Return response
5. Display in frontend

When requesting from Claude:
1. Specify frontend/backend tech stack
2. Describe required API endpoints
3. Describe data structure

Clarify these three things and you can build the entire app.

Congratulations! You've completed Part 4 (Practical II).

In the next Part, you'll learn advanced Claude Code features.

Proceed to [Chapter 18: Advanced Features](../Chapter18/README.md).
