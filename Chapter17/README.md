# Chapter 17: Building Full-Stack Apps

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Building frontend and backend together
- API design and implementation
- Data storage and retrieval

---

## Why Do You Need This?

**Real-world scenarios where full-stack skills shine:**

- **Building a todo app that works on any device** - Your data syncs everywhere because it's stored in a database, not just your browser
- **Creating a service for multiple users** - Each person has their own account, their own data, securely separated
- **Running a real business** - Accept payments, send emails, handle user authentication - all require backend logic
- **Making your app work offline** - Store data locally but sync when online

Without a backend, your app is like a house with no foundation - it looks nice but won't last!

---

## Simple Analogy: Full-Stack is Like a Restaurant

Think of a restaurant:
- **Frontend (Dining Area)**: What customers see - the menu, tables, decor, waiters taking orders
- **Backend (Kitchen)**: Where the actual work happens - cooking, storing ingredients, managing recipes
- **Database (Pantry/Storage)**: Where all ingredients and supplies are kept organized

When you order food:
1. You tell the waiter (frontend) what you want
2. The waiter sends your order to the kitchen (backend)
3. The kitchen gets ingredients from storage (database)
4. Food is prepared and sent back through the waiter to you

A web app works exactly the same way!

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

## Try It Yourself: Minimal Working Example

Before building the full todo app, let's make sure frontend and backend can talk to each other:

**1. Create a minimal backend (`server.js`):**

```javascript
// server.js - Simplest possible Express server
const express = require('express')
const cors = require('cors')

const app = express()
app.use(cors())
app.use(express.json())

// In-memory data (just for testing)
let message = 'Hello from backend!'

// GET: Retrieve data
app.get('/api/message', (req, res) => {
  res.json({ message })
})

// POST: Update data
app.post('/api/message', (req, res) => {
  message = req.body.message
  res.json({ success: true, message })
})

app.listen(3001, () => {
  console.log('Backend running on http://localhost:3001')
})
```

**2. Test with curl (or browser):**

```bash
# Start the server
npm init -y && npm install express cors
node server.js

# In another terminal, test the API
curl http://localhost:3001/api/message
# {"message":"Hello from backend!"}

curl -X POST http://localhost:3001/api/message \
  -H "Content-Type: application/json" \
  -d '{"message":"Updated!"}'
# {"success":true,"message":"Updated!"}
```

**3. Connect from frontend:**

```javascript
// In your React component
const [message, setMessage] = useState('')

// Fetch on load
useEffect(() => {
  fetch('http://localhost:3001/api/message')
    .then(res => res.json())
    .then(data => setMessage(data.message))
}, [])
```

If you see "Hello from backend!" in your React app, the connection works! Now you can build the full todo app.

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

## If It Doesn't Work? Troubleshooting Tips

### CORS Error: "Access-Control-Allow-Origin"

```javascript
// Backend: Make sure you have cors middleware
const cors = require('cors')
app.use(cors())  // Add this BEFORE your routes!

// If still having issues, be more specific:
app.use(cors({
  origin: 'http://localhost:5173',  // Your frontend URL
  credentials: true
}))
```

### "Network Error" or "Failed to Fetch"

```bash
# Check if backend is actually running
curl http://localhost:3001/api/todos

# If no response, your backend isn't running
# Make sure you're in the right directory
cd backend && node index.js
```

### Data disappears when server restarts

That's normal with in-memory storage! Use a database:

```javascript
// With SQLite (persistent storage)
const Database = require('better-sqlite3')
const db = new Database('mydata.db')

// Data survives restarts!
```

### Frontend shows old data after adding new item

```javascript
// Make sure you're updating state after API call
const addTodo = async () => {
  const res = await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ text: newTodo })
  })
  const todo = await res.json()

  // UPDATE STATE - don't forget this!
  setTodos([todo, ...todos])
}
```

### Port already in use error

```bash
# Find what's using the port
lsof -i :3001

# Kill it (replace PID with actual process ID)
kill -9 <PID>

# Or just use a different port
app.listen(3002, ...)
```

---

## Common Mistakes

### 1. Forgetting to Parse JSON Body

```javascript
// WRONG - req.body will be undefined
app.post('/api/todos', (req, res) => {
  console.log(req.body)  // undefined!
})

// CORRECT - add JSON middleware
app.use(express.json())  // Add this BEFORE routes
app.post('/api/todos', (req, res) => {
  console.log(req.body)  // Now it works!
})
```

### 2. Mixing Up HTTP Methods

```javascript
// GET: Retrieve data (no body)
// POST: Create new data
// PUT: Replace entire resource
// PATCH: Update part of resource
// DELETE: Remove data

// WRONG - using GET to create data
app.get('/api/todos/create', ...)

// CORRECT - use POST for creation
app.post('/api/todos', ...)
```

### 3. Not Handling Errors in Frontend

```javascript
// WRONG - crashes silently on error
const data = await fetch(API_URL).then(r => r.json())

// CORRECT - handle errors gracefully
try {
  const res = await fetch(API_URL)
  if (!res.ok) throw new Error('API error')
  const data = await res.json()
  setTodos(data)
} catch (err) {
  console.error('Failed to fetch:', err)
  setError('Could not load todos')
}
```

### 4. Hardcoding API URLs

```javascript
// WRONG - breaks when you deploy
const API_URL = 'http://localhost:3001/api'

// CORRECT - use environment variables
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3001/api'
```

### 5. Not Validating Backend Input

```javascript
// WRONG - anyone can send anything
app.post('/api/todos', (req, res) => {
  const { text } = req.body
  db.prepare('INSERT INTO todos (text) VALUES (?)').run(text)
})

// CORRECT - validate first
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  if (!text || typeof text !== 'string' || text.length > 500) {
    return res.status(400).json({ error: 'Invalid todo text' })
  }

  db.prepare('INSERT INTO todos (text) VALUES (?)').run(text.trim())
})
```

### 6. Running Both Frontend and Backend on Same Port

```bash
# Backend is on 3001
# Frontend dev server is on 5173 (Vite default)
# They MUST be different ports!

# Backend
app.listen(3001, ...)

# Frontend connects to backend
const API_URL = 'http://localhost:3001/api'
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
