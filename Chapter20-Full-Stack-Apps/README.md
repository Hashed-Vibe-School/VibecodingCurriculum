# Chapter 20: Building Full-Stack Apps - Frontend Integration and Authentication

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Previous Chapter Review

In [Chapter 19: Backend Basics](../Chapter19-Backend-Basics/README.md), we built REST APIs with Express and SQLite.

In this chapter, we'll connect a React frontend and add user authentication to create a **complete full-stack app**!

---

## What You Will Learn

- Connecting React with backend API
- Understanding CORS configuration
- User authentication system (JWT)
- Building a complete full-stack Todo app
- Deployment preparation

---

## Part 1: Understanding Frontend-Backend Connection

### Why We Need Two Servers

In full-stack development, **two servers** run separately:

```
+------------------------------------------------------------------+
|                     Full-Stack App Structure                       |
+------------------------------------------------------------------+
|                                                                    |
|  Terminal 1 (Frontend)              Terminal 2 (Backend)           |
|  --------------------               -----------------              |
|                                                                    |
|  +-----------------+                +-----------------+            |
|  | React Dev Server|    API        |  Express Server  |            |
|  | localhost:5173  | -----------> | localhost:3001   |            |
|  |                 |   Request     |                  |            |
|  | npm run dev     | <----------- | node index.js    |            |
|  |                 |   Response    |                  |            |
|  +-----------------+               +--------+---------+            |
|                                             |                      |
|                                      +------v------+               |
|                                      |   SQLite    |               |
|                                      |  todos.db   |               |
|                                      +-------------+               |
|                                                                    |
+------------------------------------------------------------------+
```

> **Beginner Tip**: The frontend server (5173) delivers React code to the browser, and the backend server (3001) processes data. It's like having "front of house staff" and "kitchen staff" at a restaurant!

### What is CORS?

Browsers **block requests from different origins (domains, ports)** by default. This is CORS (Cross-Origin Resource Sharing).

```
+------------------------------------------------------------------+
|                       CORS Problem Situation                       |
+------------------------------------------------------------------+
|                                                                    |
|  localhost:5173 (Frontend)         localhost:3001 (Backend)        |
|  ----------------------           -----------------------          |
|                                                                    |
|  fetch('/api/todos')                                               |
|        |                                                           |
|        v                                                           |
|  X Browser blocks!                                                 |
|    "Different ports not allowed"                                   |
|    (5173 != 3001)                                                  |
|                                                                    |
|  -----------------------------------------------------------      |
|                                                                    |
|  Solution: Add CORS permission on backend                          |
|                                                                    |
|  fetch('http://localhost:3001/api/todos')                          |
|        |                                                           |
|        v                                                           |
|  OK Backend responds "Requests from 5173 are OK"                   |
|     -> Browser allows it                                           |
|                                                                    |
+------------------------------------------------------------------+
```

### Configuring CORS

Install and configure the `cors` package on the backend:

```bash
npm install cors
```

```javascript
// backend/index.js
const express = require('express')
const cors = require('cors')  // Add this

const app = express()

// CORS configuration
app.use(cors())  // Allow all origins (for development)

// Or allow only specific origin (for production)
app.use(cors({
  origin: 'http://localhost:5173',  // Frontend URL
  credentials: true                  // Allow cookies
}))

app.use(express.json())

// ... rest of API code
```

> **Security Warning**: `app.use(cors())` allows all origins. It's convenient for development, but in production, allow only specific domains!

---

## Part 2: Creating Project Structure

### Step 1: Folder Structure

```
todo-fullstack/
├── backend/                  # Backend (created in previous chapter)
│   ├── index.js
│   ├── package.json
│   └── todos.db
│
├── frontend/                 # Frontend (we'll create this)
│   ├── src/
│   │   ├── App.jsx
│   │   ├── App.css
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
│
└── README.md
```

### Step 2: Create Frontend Project

```bash
# In project root folder
npm create vite@latest frontend -- --template react

# Move to frontend folder
cd frontend

# Install dependencies
npm install
```

### Step 3: Running Both Simultaneously

Open **two terminals**:

```bash
# Terminal 1: Backend
cd backend
node index.js
# -> "Server running at http://localhost:3001!"

# Terminal 2: Frontend
cd frontend
npm run dev
# -> "Local: http://localhost:5173"
```

---

## Part 3: Minimal Connection Test

Before building the full app, let's verify that frontend and backend can communicate properly.

### Backend: Add Simple API

```javascript
// Add to backend/index.js
app.get('/api/health', (req, res) => {
  res.json({
    status: 'ok',
    message: 'Backend is working properly!',
    timestamp: new Date().toISOString()
  })
})
```

### Frontend: Connection Test

```jsx
// frontend/src/App.jsx
import { useState, useEffect } from 'react'

function App() {
  const [status, setStatus] = useState('Checking connection...')
  const [error, setError] = useState(null)

  useEffect(() => {
    // Call backend API
    fetch('http://localhost:3001/api/health')
      .then(response => {
        if (!response.ok) {
          throw new Error('Server response error')
        }
        return response.json()
      })
      .then(data => {
        setStatus(`Connected! ${data.message}`)
      })
      .catch(err => {
        setError(`Connection failed: ${err.message}`)
        setStatus(null)
      })
  }, [])

  return (
    <div style={{ padding: '20px', fontFamily: 'sans-serif' }}>
      <h1>Backend Connection Test</h1>
      {status && <p style={{ color: 'green' }}>{status}</p>}
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  )
}

export default App
```

### Practice Problem 3-1: Connection Check

1. Run both backend and frontend
2. Access `http://localhost:5173` in your browser
3. If you see "Backend is working properly!", you're successful!

**If connection fails, check:**
- Is the backend running? (Check terminal)
- Did you configure CORS?
- Are port numbers correct? (Backend 3001, Frontend 5173)

---

## Part 4: Building Complete Todo App

Now let's build the real Todo app!

### Backend: Complete API

Just add CORS to the backend from the previous chapter:

```javascript
// backend/index.js
const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')  // Add this!

const app = express()

// Middleware
app.use(cors())  // Allow CORS
app.use(express.json())

// Database
const db = new Database('todos.db')
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// API endpoints (same code from previous chapter)
app.get('/api/todos', (req, res) => {
  const todos = db.prepare('SELECT * FROM todos ORDER BY created_at DESC').all()
  res.json(todos)
})

app.post('/api/todos', (req, res) => {
  const { text } = req.body
  if (!text || !text.trim()) {
    return res.status(400).json({ error: 'Please enter todo content' })
  }
  const result = db.prepare('INSERT INTO todos (text) VALUES (?)').run(text.trim())
  const newTodo = db.prepare('SELECT * FROM todos WHERE id = ?').get(result.lastInsertRowid)
  res.status(201).json(newTodo)
})

app.patch('/api/todos/:id', (req, res) => {
  const { id } = req.params
  const existing = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  if (!existing) {
    return res.status(404).json({ error: 'Todo not found' })
  }
  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)
  const updated = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  res.json(updated)
})

app.delete('/api/todos/:id', (req, res) => {
  const { id } = req.params
  const result = db.prepare('DELETE FROM todos WHERE id = ?').run(id)
  if (result.changes === 0) {
    return res.status(404).json({ error: 'Todo not found' })
  }
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('Backend server: http://localhost:3001')
})
```

### Frontend: React Todo App

```jsx
// frontend/src/App.jsx
import { useState, useEffect } from 'react'
import './App.css'

// API base URL
const API_URL = 'http://localhost:3001/api/todos'

function App() {
  // ===================================================================
  // State definitions
  // ===================================================================
  const [todos, setTodos] = useState([])       // Todo list
  const [newTodo, setNewTodo] = useState('')   // New todo input
  const [loading, setLoading] = useState(true) // Loading state
  const [error, setError] = useState(null)     // Error message

  // ===================================================================
  // Fetch todo list on page load
  // ===================================================================
  useEffect(() => {
    fetchTodos()
  }, [])

  // ===================================================================
  // API call functions
  // ===================================================================

  // Fetch todo list
  const fetchTodos = async () => {
    try {
      setLoading(true)
      setError(null)

      const response = await fetch(API_URL)
      if (!response.ok) {
        throw new Error('Server response error')
      }

      const data = await response.json()
      setTodos(data)
    } catch (err) {
      setError('Failed to load todos. Check if backend is running.')
      console.error(err)
    } finally {
      setLoading(false)
    }
  }

  // Add new todo
  const addTodo = async (e) => {
    e.preventDefault()

    if (!newTodo.trim()) {
      setError('Please enter todo content')
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
        throw new Error(errorData.error || 'Add failed')
      }

      const todo = await response.json()
      setTodos([todo, ...todos])  // Add to front of list
      setNewTodo('')              // Clear input field
    } catch (err) {
      setError(err.message)
    }
  }

  // Toggle completion status
  const toggleTodo = async (id) => {
    try {
      setError(null)

      const response = await fetch(`${API_URL}/${id}`, {
        method: 'PATCH'
      })

      if (!response.ok) {
        throw new Error('Status change failed')
      }

      // Update local state
      setTodos(todos.map(todo =>
        todo.id === id
          ? { ...todo, completed: todo.completed ? 0 : 1 }
          : todo
      ))
    } catch (err) {
      setError(err.message)
    }
  }

  // Delete todo
  const deleteTodo = async (id) => {
    if (!window.confirm('Are you sure you want to delete?')) {
      return
    }

    try {
      setError(null)

      const response = await fetch(`${API_URL}/${id}`, {
        method: 'DELETE'
      })

      if (!response.ok) {
        throw new Error('Delete failed')
      }

      setTodos(todos.filter(todo => todo.id !== id))
    } catch (err) {
      setError(err.message)
    }
  }

  // ===================================================================
  // Render screen
  // ===================================================================
  return (
    <div className="app">
      <h1>Todo List</h1>

      {/* Error message */}
      {error && (
        <div className="error">
          Warning: {error}
          <button onClick={() => setError(null)}>X</button>
        </div>
      )}

      {/* Add todo form */}
      <form onSubmit={addTodo} className="add-form">
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="Enter a new todo..."
          maxLength={500}
        />
        <button type="submit" disabled={!newTodo.trim()}>
          Add
        </button>
      </form>

      {/* Loading indicator */}
      {loading ? (
        <div className="loading">Loading...</div>
      ) : (
        <>
          {/* Todo list */}
          <ul className="todo-list">
            {todos.length === 0 ? (
              <li className="empty">
                No todos. Add a new todo!
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
                    Delete
                  </button>
                </li>
              ))
            )}
          </ul>

          {/* Statistics */}
          {todos.length > 0 && (
            <div className="stats">
              Total: {todos.length} |
              Completed: {todos.filter(t => t.completed).length} |
              Remaining: {todos.filter(t => !t.completed).length}
            </div>
          )}
        </>
      )}
    </div>
  )
}

export default App
```

### CSS Styles

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

/* Error message */
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

/* Input form */
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

/* Loading */
.loading {
  text-align: center;
  padding: 40px;
  color: #666;
}

/* Todo list */
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
  font-size: 14px;
  cursor: pointer;
  opacity: 0.5;
  transition: opacity 0.2s;
  color: #c00;
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

/* Statistics */
.stats {
  text-align: center;
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #eee;
  color: #666;
  font-size: 14px;
}
```

### Practice Problem 4-1: Todo App Test

1. Run both backend and frontend
2. Add a new todo
3. Mark it as completed with the checkbox
4. Delete it
5. Refresh the browser and verify data persists!

---

## Part 5: Understanding async/await

We used `async/await` a lot in API call code. Let's understand what it is.

### Why Do We Need async/await?

When you send a request to a server, it **takes time** to get a response. If you don't wait and execute the next code, problems occur.

```javascript
// Wrong example - doesn't wait for response
function badExample() {
  const response = fetch('/api/todos')  // Start request
  console.log(response)  // Response hasn't arrived yet!
  // Result: Promise { <pending> }
}

// Correct example - using async/await
async function goodExample() {
  const response = await fetch('/api/todos')  // Wait for response
  const data = await response.json()          // Wait for JSON conversion too
  console.log(data)  // Actual data!
}
```

### async/await Rules

1. **await** can only be used **inside an async function**
2. **await** is placed before functions that return a Promise
3. The **await** line waits until the result arrives

```javascript
// async function declaration
async function fetchData() {
  // Wait with await
  const response = await fetch('/api/data')
  const data = await response.json()
  return data
}

// Arrow function version
const fetchData = async () => {
  const response = await fetch('/api/data')
  const data = await response.json()
  return data
}
```

### Error Handling: try-catch

```javascript
async function fetchWithErrorHandling() {
  try {
    const response = await fetch('/api/data')

    // Check for HTTP errors (404, 500, etc.)
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`)
    }

    const data = await response.json()
    return data

  } catch (error) {
    // Network error or error thrown above
    console.error('Error occurred:', error.message)
    throw error  // Re-throw so caller can handle it
  }
}
```

> **Beginner Tip**: `async/await` is syntax for "wait and get the result". Like sending a letter and waiting for a reply, you send a request to the server and wait for the response!

---

## Part 6: Adding User Authentication

Now let's add login functionality. Each user can have their own todo list.

### Why We Need Authentication

```
+------------------------------------------------------------------+
|                    No Auth vs With Auth                            |
+------------------------------------------------------------------+
|                                                                    |
|  No Auth                              With Auth                    |
|                                                                    |
|  +-------------+                     +-------------+               |
|  | All users   |                     |   Alice     | -> Login      |
|  | shared data |                     |   Bob       | -> Login      |
|  |             |                     |   Charlie   | -> Login      |
|  +-------------+                     +-------------+               |
|        |                                    |                      |
|        v                                    v                      |
|  +-------------+                     +-------------+               |
|  |   Todos:    |                     | Alice's     |               |
|  | - Shopping  |                     | Todos       |               |
|  | - Exercise  |                     | - Shopping  |               |
|  | - Study     |                     +-------------+               |
|  | (Whose?)    |                     +-------------+               |
|  +-------------+                     | Bob's Todos |               |
|                                      | - Exercise  |               |
|  "Who looked at                      +-------------+               |
|   my data?"                          "Only I can see               |
|                                       my data!"                    |
|                                                                    |
+------------------------------------------------------------------+
```

### JWT (JSON Web Token) Authentication

JWT is an encrypted string containing user information.

```
+------------------------------------------------------------------+
|                      JWT Authentication Flow                       |
+------------------------------------------------------------------+
|                                                                    |
|  1. Login Request                                                  |
|     +--------+    Email + Password     +--------+                  |
|     |  User  | ----------------------> | Server |                  |
|     +--------+                         +--------+                  |
|                                             |                      |
|  2. Token Issued                            |                      |
|     +--------+       JWT Token        +----v----+                  |
|     |  User  | <--------------------- | Server  |                  |
|     +--------+    "eyJhbGc..."        +---------+                  |
|         |                                                          |
|  3. Store Token (Browser)                                          |
|         v                                                          |
|     localStorage.setItem('token', 'eyJhbGc...')                    |
|                                                                    |
|  4. Include Token in Subsequent Requests                           |
|     +--------+  Request + "Bearer token"  +--------+               |
|     |  User  | -------------------------> | Server |               |
|     +--------+  Authorization header      +--------+               |
|                                                |                   |
|  5. Server Validates Token                     |                   |
|                                          +-----v-----+             |
|     Valid -> Process request             |Token Check|             |
|     Invalid -> 401 error                 +-----------+             |
|                                                                    |
+------------------------------------------------------------------+
```

### Backend: Add Authentication API

First install required packages:

```bash
cd backend
npm install bcrypt jsonwebtoken
```

```javascript
// backend/index.js - Adding authentication

const express = require('express')
const Database = require('better-sqlite3')
const cors = require('cors')
const bcrypt = require('bcrypt')      // Password encryption
const jwt = require('jsonwebtoken')    // JWT tokens

const app = express()
app.use(cors())
app.use(express.json())

// WARNING: In production, use environment variables!
const JWT_SECRET = 'your-super-secret-key-change-in-production'

const db = new Database('todos.db')

// Create tables
db.exec(`
  -- Users table
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    email TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    name TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  );

  -- Todos table (with user_id)
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    user_id INTEGER NOT NULL,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
  )
`)

// ===================================================================
// Auth Middleware - Used for protected routes
// ===================================================================
function authMiddleware(req, res, next) {
  // Extract token from Authorization header
  const authHeader = req.headers.authorization
  const token = authHeader?.split(' ')[1]  // "Bearer token" -> "token"

  if (!token) {
    return res.status(401).json({ error: 'Login required' })
  }

  try {
    // Verify token
    const decoded = jwt.verify(token, JWT_SECRET)
    req.userId = decoded.userId
    req.userEmail = decoded.email
    next()
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({ error: 'Session expired' })
    }
    return res.status(401).json({ error: 'Invalid token' })
  }
}

// ===================================================================
// Registration API
// ===================================================================
app.post('/api/register', async (req, res) => {
  const { email, password, name } = req.body

  // Input validation
  if (!email || !password) {
    return res.status(400).json({ error: 'Please enter email and password' })
  }

  if (password.length < 6) {
    return res.status(400).json({ error: 'Password must be at least 6 characters' })
  }

  // Check for duplicate email
  const existing = db.prepare('SELECT id FROM users WHERE email = ?').get(email)
  if (existing) {
    return res.status(409).json({ error: 'Email already registered' })
  }

  try {
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10)

    // Save user
    const result = db.prepare(
      'INSERT INTO users (email, password, name) VALUES (?, ?, ?)'
    ).run(email, hashedPassword, name || email.split('@')[0])

    res.status(201).json({
      message: 'Registration complete',
      userId: result.lastInsertRowid
    })
  } catch (error) {
    console.error('Registration error:', error)
    res.status(500).json({ error: 'Registration failed' })
  }
})

// ===================================================================
// Login API
// ===================================================================
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body

  if (!email || !password) {
    return res.status(400).json({ error: 'Please enter email and password' })
  }

  // Find user
  const user = db.prepare('SELECT * FROM users WHERE email = ?').get(email)

  // If user doesn't exist or password is wrong
  if (!user || !await bcrypt.compare(password, user.password)) {
    return res.status(401).json({ error: 'Email or password is incorrect' })
  }

  // Generate JWT token
  const token = jwt.sign(
    { userId: user.id, email: user.email },
    JWT_SECRET,
    { expiresIn: '7d' }  // Expires in 7 days
  )

  res.json({
    message: 'Login successful',
    token,
    user: {
      id: user.id,
      email: user.email,
      name: user.name
    }
  })
})

// ===================================================================
// Protected APIs - Require authentication
// ===================================================================

// Get own todo list
app.get('/api/todos', authMiddleware, (req, res) => {
  const todos = db.prepare(
    'SELECT * FROM todos WHERE user_id = ? ORDER BY created_at DESC'
  ).all(req.userId)

  res.json(todos)
})

// Add new todo (as own)
app.post('/api/todos', authMiddleware, (req, res) => {
  const { text } = req.body

  if (!text || !text.trim()) {
    return res.status(400).json({ error: 'Please enter todo content' })
  }

  const result = db.prepare(
    'INSERT INTO todos (text, user_id) VALUES (?, ?)'
  ).run(text.trim(), req.userId)

  const newTodo = db.prepare('SELECT * FROM todos WHERE id = ?')
    .get(result.lastInsertRowid)

  res.status(201).json(newTodo)
})

// Toggle completion (own only)
app.patch('/api/todos/:id', authMiddleware, (req, res) => {
  const { id } = req.params

  // Check if it's own todo
  const todo = db.prepare(
    'SELECT * FROM todos WHERE id = ? AND user_id = ?'
  ).get(id, req.userId)

  if (!todo) {
    return res.status(404).json({ error: 'Todo not found' })
  }

  db.prepare('UPDATE todos SET completed = NOT completed WHERE id = ?').run(id)

  const updated = db.prepare('SELECT * FROM todos WHERE id = ?').get(id)
  res.json(updated)
})

// Delete (own only)
app.delete('/api/todos/:id', authMiddleware, (req, res) => {
  const { id } = req.params

  const result = db.prepare(
    'DELETE FROM todos WHERE id = ? AND user_id = ?'
  ).run(id, req.userId)

  if (result.changes === 0) {
    return res.status(404).json({ error: 'Todo not found' })
  }

  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('Server running: http://localhost:3001')
})
```

> **What is bcrypt?** A library that converts passwords into "hashes". A hash is an encrypted string that can't be reversed to the original. Even if the database is hacked, the actual passwords remain protected!

### Frontend: Add Login Feature

```jsx
// frontend/src/App.jsx - With authentication
import { useState, useEffect } from 'react'
import './App.css'

const API_URL = 'http://localhost:3001/api'

function App() {
  // ===================================================================
  // State
  // ===================================================================
  const [user, setUser] = useState(null)           // Logged in user
  const [todos, setTodos] = useState([])           // Todo list
  const [newTodo, setNewTodo] = useState('')       // New todo
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  // Login/Registration form state
  const [isLoginMode, setIsLoginMode] = useState(true)
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [name, setName] = useState('')

  // ===================================================================
  // Auto-login if token exists
  // ===================================================================
  useEffect(() => {
    const token = localStorage.getItem('token')
    const savedUser = localStorage.getItem('user')

    if (token && savedUser) {
      setUser(JSON.parse(savedUser))
    }
    setLoading(false)
  }, [])

  // Load todo list after login
  useEffect(() => {
    if (user) {
      fetchTodos()
    }
  }, [user])

  // ===================================================================
  // Authenticated API call helper
  // ===================================================================
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

    // Logout on token expiry
    if (response.status === 401) {
      logout()
      throw new Error('Session expired. Please login again.')
    }

    return response
  }

  // ===================================================================
  // Auth functions
  // ===================================================================

  // Register
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

      // Registration success -> Switch to login mode
      setIsLoginMode(true)
      setError(null)
      alert('Registration complete. Please login!')
    } catch (err) {
      setError(err.message)
    }
  }

  // Login
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

      // Save token and user info
      localStorage.setItem('token', data.token)
      localStorage.setItem('user', JSON.stringify(data.user))
      setUser(data.user)

      // Clear form
      setEmail('')
      setPassword('')
    } catch (err) {
      setError(err.message)
    }
  }

  // Logout
  const logout = () => {
    localStorage.removeItem('token')
    localStorage.removeItem('user')
    setUser(null)
    setTodos([])
  }

  // ===================================================================
  // Todo API functions
  // ===================================================================

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

      if (!response.ok) throw new Error('Add failed')

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
    if (!window.confirm('Delete this todo?')) return

    try {
      await authFetch(`${API_URL}/todos/${id}`, { method: 'DELETE' })
      setTodos(todos.filter(t => t.id !== id))
    } catch (err) {
      setError(err.message)
    }
  }

  // ===================================================================
  // Loading
  // ===================================================================
  if (loading) {
    return <div className="app"><div className="loading">Loading...</div></div>
  }

  // ===================================================================
  // Not logged in -> Login/Register form
  // ===================================================================
  if (!user) {
    return (
      <div className="app">
        <h1>Todo List</h1>

        {error && (
          <div className="error">
            Warning: {error}
            <button onClick={() => setError(null)}>X</button>
          </div>
        )}

        <form onSubmit={isLoginMode ? login : register} className="auth-form">
          <h2>{isLoginMode ? 'Login' : 'Register'}</h2>

          {!isLoginMode && (
            <input
              type="text"
              placeholder="Name (optional)"
              value={name}
              onChange={(e) => setName(e.target.value)}
            />
          )}

          <input
            type="email"
            placeholder="Email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            required
          />

          <input
            type="password"
            placeholder="Password (6+ characters)"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
            minLength={6}
          />

          <button type="submit">
            {isLoginMode ? 'Login' : 'Register'}
          </button>

          <p className="switch-mode">
            {isLoginMode ? "Don't have an account? " : 'Already have an account? '}
            <button
              type="button"
              onClick={() => {
                setIsLoginMode(!isLoginMode)
                setError(null)
              }}
            >
              {isLoginMode ? 'Register' : 'Login'}
            </button>
          </p>
        </form>
      </div>
    )
  }

  // ===================================================================
  // Logged in -> Todo app
  // ===================================================================
  return (
    <div className="app">
      <header className="header">
        <h1>{user.name || user.email}'s Todos</h1>
        <button onClick={logout} className="logout-btn">Logout</button>
      </header>

      {error && (
        <div className="error">
          Warning: {error}
          <button onClick={() => setError(null)}>X</button>
        </div>
      )}

      <form onSubmit={addTodo} className="add-form">
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="Enter a new todo..."
        />
        <button type="submit" disabled={!newTodo.trim()}>Add</button>
      </form>

      <ul className="todo-list">
        {todos.length === 0 ? (
          <li className="empty">No todos</li>
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
                Delete
              </button>
            </li>
          ))
        )}
      </ul>

      {todos.length > 0 && (
        <div className="stats">
          Total: {todos.length} | Completed: {todos.filter(t => t.completed).length}
        </div>
      )}
    </div>
  )
}

export default App
```

### Additional CSS

```css
/* Add to frontend/src/App.css */

/* Header */
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

/* Auth form */
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

### Practice Problem 6-1: Authentication Test

1. Register a new account
2. Login
3. Add some todos
4. Logout and login with a different account
5. Verify that the previous account's todos are not visible!

---

## Part 7: Deployment Preparation

The app is complete, let's prepare for deployment.

### Environment Variables

Secrets and configuration values should not be written directly in code!

```bash
# Create backend/.env file
JWT_SECRET=your-super-secret-key-at-least-32-characters-long
PORT=3001
```

```javascript
// Modify backend/index.js
require('dotenv').config()  // Add at the top

const JWT_SECRET = process.env.JWT_SECRET || 'fallback-dev-key'
const PORT = process.env.PORT || 3001
```

```bash
# Install dotenv package
npm install dotenv
```

> **Important**: Add the `.env` file to `.gitignore` so it doesn't get uploaded to Git!

### Recommended Deployment Platforms

| Service | Use Case | Free |
|---------|----------|------|
| **Vercel** | Frontend | Free |
| **Netlify** | Frontend | Free |
| **Railway** | Backend | $5/month credit |
| **Render** | Backend | Free (limited) |
| **Fly.io** | Backend | Free (limited) |

### Pre-Deployment Checklist

- [ ] Environment variables configured (.env)
- [ ] Sensitive files added to .gitignore
- [ ] CORS configured (allow only frontend domain)
- [ ] Frontend build test (`npm run build`)
- [ ] Backend error logging configured

---

## Mini Quiz

### Quiz 1: CORS

Why does a CORS error occur?

A) Server is off
B) URL is wrong
C) Frontend and backend have different origins (ports)
D) JSON format is wrong

<details>
<summary>See Answer</summary>

**Answer: C) Frontend and backend have different origins (ports)**

Browsers block requests from different origins (domains, ports) by default for security.
When frontend (5173) and backend (3001) have different ports, CORS errors occur.

</details>

### Quiz 2: JWT

Where is the JWT token stored?

A) Database
B) Server memory
C) Browser localStorage
D) URL parameters

<details>
<summary>See Answer</summary>

**Answer: C) Browser localStorage**

JWT tokens are stored in the browser's localStorage after login.
They are included in the Authorization header for subsequent API requests.

</details>

### Quiz 3: API Calls

What's wrong with this code?

```javascript
const response = await fetch('/api/todos', {
  method: 'POST',
  body: JSON.stringify({ text: 'Hello' })
})
```

A) fetch usage is wrong
B) await is used incorrectly
C) Content-Type header is missing
D) No problem

<details>
<summary>See Answer</summary>

**Answer: C) Content-Type header is missing**

When sending JSON, you must add the `'Content-Type': 'application/json'` header.
Without this header, the server's `req.body` will be undefined.

</details>

---

## Practice Assignments

### Basic Assignment: Extend to Memo App

Extend the Todo app into a memo app:
- Memos with title and content
- Memo editing feature
- Memo search feature

### Advanced Assignment: Additional Features

Add the following features:
- Category/tag classification
- Due date setting
- Priority marking (stars)

---

## Glossary

| Term | Description |
|------|-------------|
| **Full-stack** | Developing both frontend and backend |
| **CORS** | Security mechanism that controls resource sharing from different origins |
| **JWT** | JSON Web Token, encrypted token containing user authentication info |
| **bcrypt** | Library for securely hashing passwords |
| **async/await** | Syntax that allows writing asynchronous code synchronously |
| **Authorization header** | HTTP header that contains authentication information |
| **localStorage** | Browser storage for persisting data |

---

## Next Chapter Preview

In [Chapter 21: Understanding Architecture](../Chapter21-Architecture/README.md), we'll learn:
- How Claude Code works internally
- Tools and sub-agents
- Context management

Now that you've mastered full-stack development, let's understand Claude Code more deeply!

---

## Summary

1. **Full-stack apps** communicate via API between frontend and backend
2. **CORS** configuration allows requests from different origins
3. **async/await** handles asynchronous API calls
4. **JWT** implements user authentication
5. **bcrypt** safely stores passwords
6. **Environment variables** manage secret keys before deployment

Congratulations! You can now build complete full-stack apps!

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [React Official Docs](https://react.dev/) - React official documentation
- [Vite Guide](https://vitejs.dev/guide/) - Vite build tool
- [JWT Introduction](https://jwt.io/introduction) - Understanding JWT tokens

**Video Resources:**
- [Full-Stack App Tutorial (YouTube)](https://www.youtube.com/results?search_query=fullstack+react+express+tutorial)
- [React + Express Integration (YouTube)](https://www.youtube.com/results?search_query=react+express+integration+tutorial)
- [JWT Authentication Tutorial (YouTube)](https://www.youtube.com/results?search_query=jwt+authentication+nodejs+react+tutorial)

**Reading Materials:**
- [Complete CORS Guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) - MDN CORS documentation
- [JWT Best Practices](https://auth0.com/blog/jwt-authentication-best-practices/) - Auth0 JWT guide
- [Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/) - useEffect deep dive

**Deployment Platforms:**
- [Vercel](https://vercel.com/) - Frontend deployment (free)
- [Netlify](https://www.netlify.com/) - Frontend deployment (free)
- [Railway](https://railway.app/) - Full-stack deployment
- [Render](https://render.com/) - Backend deployment (free tier)
- [Fly.io](https://fly.io/) - Global app deployment

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
