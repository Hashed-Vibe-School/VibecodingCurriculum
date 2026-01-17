# Chapter 19: Backend Basics - Server and Database

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Previous Chapter Review

In [Chapter 18: Building Chatbots](../Chapter18-Chatbots/README.md), we built Discord and Slack bots. We learned event-driven programming and external API integration.

In this chapter, we'll learn how to build the **backend** of web services. In the next chapter, we'll connect it to a frontend to create a complete full-stack app!

---

## What You Will Learn

- Understanding the roles of frontend and backend
- Building a web server with Express
- REST API design and implementation
- Storing data with SQLite
- CRUD operations (Create, Read, Update, Delete)

---

## Why Do You Need a Backend?

Think about the apps we've built so far:
- Refresh the browser? -> Data disappears
- Access from a different device? -> No data
- Multiple people use it? -> Can't share data

**With a backend:**
- Data is permanently stored on the server
- Access the same data from any device
- Multiple users can have their own data
- Securely handle logic that requires security

> **Beginner Tip**: An app with only frontend is like "a house without a foundation." It looks nice, but it can't keep data for long. The backend is the foundation of that house!

---

## Simple Analogy: Understanding Through a Restaurant

Let's compare a web application to a restaurant:

```
+------------------------------------------------------------------+
|                   Restaurant = Web Application                     |
+------------------------------------------------------------------+
|                                                                    |
|  Dining Area (Frontend)              Kitchen (Backend)             |
|  ---------------------               ----------------              |
|  - Space customers see               - Where actual cooking happens|
|  - Menu, tables, decor               - Chefs, recipes, cooking     |
|  - Waiter takes orders               - Customers can't enter       |
|                                         (security)                 |
|                                                                    |
|  Order Slip = API Request            Pantry = Database             |
|  -------------------                 ----------------              |
|  "One tomato pasta please"           - All ingredients organized   |
|  -> POST /api/orders                 - Retrieved when needed       |
|     { menu: "pasta" }                - Inventory management        |
|                                                                    |
+------------------------------------------------------------------+
```

When ordering food:
1. **Customer (user)** -> Orders from waiter (frontend)
2. **Waiter (frontend)** -> Delivers order slip (API request) to kitchen (backend)
3. **Kitchen (backend)** -> Gets ingredients from pantry (database)
4. **Kitchen (backend)** -> Completes dish and hands to waiter (API response)
5. **Waiter (frontend)** -> Serves food to customer

> **Important**: Just as customers shouldn't go directly into the kitchen, the frontend should never access the database directly. Always go through the backend to maintain security!

---

## Frontend vs Backend Comparison

| Category | Frontend | Backend |
|----------|----------|---------|
| **Location** | Browser (user's computer) | Server (remote computer) |
| **Role** | Display UI, handle user input | Data processing, business logic |
| **Languages** | HTML, CSS, JavaScript | JavaScript (Node.js), Python, Java, etc. |
| **Security** | Code is exposed to users | Code is hidden from users |
| **Data** | Temporary storage (lost on refresh) | Permanent storage (database) |

```
+------------------------------------------------------------------+
|              Frontend Only vs With Backend Comparison              |
+------------------------------------------------------------------+
|                                                                    |
|  Frontend Only                       With Backend                  |
|  -------------                       ------------                  |
|                                                                    |
|  +--------------+                   +--------------+               |
|  |   Browser    |                   |   Browser    |               |
|  |              |                   |  (React etc) |               |
|  | localStorage |                   +--------------+               |
|  |  (temporary) |                         |                        |
|  +--------------+                         | API request/response   |
|        |                                  v                        |
|        | Refresh                   +--------------+                |
|        v = Partial data            |    Server    |                |
|        | Other device              |  (Express)   |                |
|        v = No data                 +--------------+                |
|                                          |                         |
|                                          v                         |
|                                   +--------------+                 |
|                                   |   Database   |                 |
|                                   |   (SQLite)   |                 |
|                                   +--------------+                 |
|                                   Accessible from anywhere         |
|                                                                    |
+------------------------------------------------------------------+
```

---

## Setting Up the Development Environment

Before starting backend development, let's install what we need.

### Step 1: Create Project Folder

```bash
# Create project folder
mkdir my-first-backend
cd my-first-backend

# Create package.json (initialize Node.js project)
npm init -y
```

> **What is npm init -y?** It automatically creates the configuration file (package.json) for a Node.js project with default values. The `-y` means answering "yes" to all questions.

### Step 2: Install Required Packages

```bash
npm install express better-sqlite3
```

Packages being installed:
- **express**: Framework for easily building web servers
- **better-sqlite3**: Library for using SQLite database

### Step 3: Folder Structure

```
my-first-backend/
├── package.json      # Project configuration
├── package-lock.json # Package version lock
├── node_modules/     # Installed packages
├── index.js          # Main server file (we'll create this)
└── database.db       # Database file (auto-generated)
```

---

## Part 1: Creating the Simplest Server

### Hello World Server

```javascript
// index.js - The simplest web server in the world

// 1. Import Express library
const express = require('express')

// 2. Create Express app
const app = express()

// 3. Respond "Hello World!" when accessing "/" path
app.get('/', (req, res) => {
  res.send('Hello World!')
})

// 4. Start server (listen on port 3001)
app.listen(3001, () => {
  console.log('Server running at http://localhost:3001!')
})
```

### Try Running It

```bash
node index.js
```

Access `http://localhost:3001` in your browser and you'll see "Hello World!"

> **What is a Port?** Think of it as a "door number" for your computer. Requests coming through door 3001 are handled by our server. We usually use numbers between 3000-9000 to avoid conflicts with other programs.

### Practice Problem 1-1: Greeting Server

Modify the code above to:
1. Respond "Hello!" when accessing `/hello` path
2. Respond "Goodbye!" when accessing `/bye` path

<details>
<summary>See Answer</summary>

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
  res.send('This is the main page')
})

app.get('/hello', (req, res) => {
  res.send('Hello!')
})

app.get('/bye', (req, res) => {
  res.send('Goodbye!')
})

app.listen(3001, () => {
  console.log('Server running at http://localhost:3001!')
})
```

</details>

---

## Part 2: Exchanging Data with JSON

Real APIs exchange data in **JSON** format, not plain text.

### What is JSON?

JSON (JavaScript Object Notation) is a format for representing data:

```json
{
  "name": "John Doe",
  "age": 25,
  "hobbies": ["reading", "gaming", "exercise"]
}
```

### Responding with JSON

```javascript
// index.js
const express = require('express')
const app = express()

// Respond with JSON
app.get('/api/user', (req, res) => {
  res.json({
    id: 1,
    name: 'John Doe',
    email: 'john@example.com'
  })
})

// Multiple data (array)
app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'John Doe' },
    { id: 2, name: 'Jane Smith' },
    { id: 3, name: 'Bob Wilson' }
  ])
})

app.listen(3001, () => {
  console.log('Server running: http://localhost:3001')
})
```

### Testing

Access `http://localhost:3001/api/users` in your browser to see JSON data.

Or test with curl in the terminal:

```bash
curl http://localhost:3001/api/users
```

> **res.send() vs res.json()**:
> - `res.send('text')`: Plain text response
> - `res.json(object)`: JSON format response (automatically sets Content-Type header)

---

## Part 3: Understanding HTTP Methods

APIs use **HTTP methods** to distinguish what operation to perform.

### CRUD and HTTP Methods

| Action | CRUD | HTTP Method | Example | Description |
|--------|------|-------------|---------|-------------|
| Create | Create | **POST** | `POST /api/todos` | Add new todo |
| Read | Read | **GET** | `GET /api/todos` | Get todo list |
| Update | Update | **PUT/PATCH** | `PATCH /api/todos/1` | Modify todo #1 |
| Delete | Delete | **DELETE** | `DELETE /api/todos/1` | Delete todo #1 |

### Understanding Through Analogy

```
+------------------------------------------------------------------+
|                  HTTP Methods = Library Operations                 |
+------------------------------------------------------------------+
|                                                                    |
|  GET = "Please show me this book"                                  |
|     -> Brings and shows the book (no data change)                  |
|                                                                    |
|  POST = "Please register a new book"                               |
|     -> Adds a new book to the library                              |
|                                                                    |
|  PATCH = "Please change this book's status to 'borrowed'"          |
|     -> Modifies only part of the book's information                |
|                                                                    |
|  DELETE = "Please dispose of this book"                            |
|     -> Removes the book from the library                           |
|                                                                    |
+------------------------------------------------------------------+
```

### Using All Methods

```javascript
const express = require('express')
const app = express()

// Middleware for parsing JSON request body
app.use(express.json())

// Temporary data storage (will replace with database later)
let todos = [
  { id: 1, text: 'Go shopping', completed: false },
  { id: 2, text: 'Exercise', completed: true }
]
let nextId = 3

// ===================================================================
// GET: Retrieve all todos
// ===================================================================
app.get('/api/todos', (req, res) => {
  res.json(todos)
})

// ===================================================================
// GET: Retrieve specific todo (using URL parameter)
// ===================================================================
app.get('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)  // Extract id from URL
  const todo = todos.find(t => t.id === id)

  if (!todo) {
    return res.status(404).json({ error: 'Todo not found' })
  }

  res.json(todo)
})

// ===================================================================
// POST: Add new todo
// ===================================================================
app.post('/api/todos', (req, res) => {
  const { text } = req.body  // Extract text from request body

  if (!text) {
    return res.status(400).json({ error: 'Please enter todo content' })
  }

  const newTodo = {
    id: nextId++,
    text: text,
    completed: false
  }

  todos.push(newTodo)
  res.status(201).json(newTodo)  // 201: Created
})

// ===================================================================
// PATCH: Update todo (partial)
// ===================================================================
app.patch('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const todo = todos.find(t => t.id === id)

  if (!todo) {
    return res.status(404).json({ error: 'Todo not found' })
  }

  // Update only fields present in request body
  if (req.body.text !== undefined) {
    todo.text = req.body.text
  }
  if (req.body.completed !== undefined) {
    todo.completed = req.body.completed
  }

  res.json(todo)
})

// ===================================================================
// DELETE: Delete todo
// ===================================================================
app.delete('/api/todos/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const index = todos.findIndex(t => t.id === id)

  if (index === -1) {
    return res.status(404).json({ error: 'Todo not found' })
  }

  todos.splice(index, 1)  // Remove from array
  res.json({ success: true, message: 'Deleted' })
})

app.listen(3001, () => {
  console.log('Server running: http://localhost:3001')
})
```

### Testing with curl

```bash
# GET: Retrieve all todos
curl http://localhost:3001/api/todos

# GET: Retrieve todo #1
curl http://localhost:3001/api/todos/1

# POST: Add new todo
curl -X POST http://localhost:3001/api/todos \
  -H "Content-Type: application/json" \
  -d '{"text": "Study"}'

# PATCH: Mark todo #1 as completed
curl -X PATCH http://localhost:3001/api/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"completed": true}'

# DELETE: Delete todo #2
curl -X DELETE http://localhost:3001/api/todos/2
```

> **req.params vs req.body**:
> - `req.params`: Values in URL (e.g., the 1 in `/api/todos/1`)
> - `req.body`: JSON data in request body

### Practice Problem 3-1: Memo API

Using the todos API as reference, create a memos API:
- `GET /api/memos` - Get all memos
- `POST /api/memos` - Add new memo (title, content fields)
- `DELETE /api/memos/:id` - Delete memo

<details>
<summary>See Answer</summary>

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
    return res.status(400).json({ error: 'Please enter a title' })
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
    return res.status(404).json({ error: 'Memo not found' })
  }

  memos.splice(index, 1)
  res.json({ success: true })
})

app.listen(3001, () => {
  console.log('Server running!')
})
```

</details>

---

## Part 4: Connecting to a Database

Until now, we've stored data in variables (memory). If you restart the server, the data disappears. Now let's use a **database** for permanent storage.

### What is SQLite?

SQLite is a lightweight file-based database:
- No separate installation needed (works with just one file)
- Suitable for small projects
- Great for learning

```
+------------------------------------------------------------------+
|                    Database = Excel File                          |
+------------------------------------------------------------------+
|                                                                    |
|  Store data in "table" format like Excel                          |
|                                                                    |
|  +----------------------------------------------------------+     |
|  | todos table                                               |     |
|  +------+----------------+------------+---------------------+|     |
|  | id   | text           | completed  | created_at          ||     |
|  +------+----------------+------------+---------------------+|     |
|  | 1    | Go shopping    | 0          | 2025-01-17 10:00    ||     |
|  | 2    | Exercise       | 1          | 2025-01-17 11:00    ||     |
|  | 3    | Study          | 0          | 2025-01-17 12:00    ||     |
|  +------+----------------+------------+---------------------+|     |
|                                                                    |
|  - id: Unique number identifying each row (auto-increment)        |
|  - text: Todo content                                              |
|  - completed: Completion status (0=incomplete, 1=complete)        |
|  - created_at: Creation time                                       |
|                                                                    |
+------------------------------------------------------------------+
```

### Connecting to SQLite

```javascript
// index.js
const express = require('express')
const Database = require('better-sqlite3')

const app = express()
app.use(express.json())

// ===================================================================
// Database connection
// ===================================================================
const db = new Database('todos.db')  // Create/connect to todos.db file

// ===================================================================
// Create table (create if doesn't exist)
// ===================================================================
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

console.log('Database connected!')
```

> **SQL Syntax Explanation**:
> - `CREATE TABLE IF NOT EXISTS`: Only create table if it doesn't exist
> - `INTEGER PRIMARY KEY AUTOINCREMENT`: Auto-incrementing unique number
> - `TEXT NOT NULL`: String, required
> - `DEFAULT 0`: Use default value 0 if not provided

### CRUD with Database

Now let's modify the API to use database instead of memory:

```javascript
// index.js - Complete code
const express = require('express')
const Database = require('better-sqlite3')

const app = express()
app.use(express.json())

// Database connection
const db = new Database('todos.db')

// Create table
db.exec(`
  CREATE TABLE IF NOT EXISTS todos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL,
    completed INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP
  )
`)

// ===================================================================
// GET: Retrieve all todos
// ===================================================================
app.get('/api/todos', (req, res) => {
  try {
    // SQL query: Get all rows sorted by newest first
    const todos = db.prepare(
      'SELECT * FROM todos ORDER BY created_at DESC'
    ).all()

    res.json(todos)
  } catch (error) {
    console.error('Query error:', error)
    res.status(500).json({ error: 'Failed to load data' })
  }
})

// ===================================================================
// GET: Retrieve specific todo
// ===================================================================
app.get('/api/todos/:id', (req, res) => {
  try {
    const id = req.params.id

    // SQL query: Get one row matching id
    const todo = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    if (!todo) {
      return res.status(404).json({ error: 'Todo not found' })
    }

    res.json(todo)
  } catch (error) {
    console.error('Query error:', error)
    res.status(500).json({ error: 'Failed to load data' })
  }
})

// ===================================================================
// POST: Add new todo
// ===================================================================
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  // Input validation
  if (!text || typeof text !== 'string') {
    return res.status(400).json({ error: 'Please enter todo content' })
  }

  if (text.trim().length === 0) {
    return res.status(400).json({ error: 'Empty content cannot be added' })
  }

  if (text.length > 500) {
    return res.status(400).json({ error: 'Please enter within 500 characters' })
  }

  try {
    // SQL query: Insert new row
    const result = db.prepare(
      'INSERT INTO todos (text) VALUES (?)'
    ).run(text.trim())

    // Query and return the just-added data
    const newTodo = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(result.lastInsertRowid)

    res.status(201).json(newTodo)
  } catch (error) {
    console.error('Add error:', error)
    res.status(500).json({ error: 'Failed to add todo' })
  }
})

// ===================================================================
// PATCH: Update todo
// ===================================================================
app.patch('/api/todos/:id', (req, res) => {
  const id = req.params.id
  const { text, completed } = req.body

  try {
    // First check if the todo exists
    const existing = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    if (!existing) {
      return res.status(404).json({ error: 'Todo not found' })
    }

    // Determine fields to update
    const newText = text !== undefined ? text.trim() : existing.text
    const newCompleted = completed !== undefined ? (completed ? 1 : 0) : existing.completed

    // SQL query: Update row
    db.prepare(
      'UPDATE todos SET text = ?, completed = ? WHERE id = ?'
    ).run(newText, newCompleted, id)

    // Return updated data
    const updated = db.prepare(
      'SELECT * FROM todos WHERE id = ?'
    ).get(id)

    res.json(updated)
  } catch (error) {
    console.error('Update error:', error)
    res.status(500).json({ error: 'Failed to update' })
  }
})

// ===================================================================
// DELETE: Delete todo
// ===================================================================
app.delete('/api/todos/:id', (req, res) => {
  const id = req.params.id

  try {
    // SQL query: Delete row
    const result = db.prepare(
      'DELETE FROM todos WHERE id = ?'
    ).run(id)

    // 404 if no rows deleted
    if (result.changes === 0) {
      return res.status(404).json({ error: 'Todo not found' })
    }

    res.json({ success: true, message: 'Deleted' })
  } catch (error) {
    console.error('Delete error:', error)
    res.status(500).json({ error: 'Failed to delete' })
  }
})

// ===================================================================
// Start server
// ===================================================================
const PORT = 3001

app.listen(PORT, () => {
  console.log('=========================================')
  console.log(`Server running at http://localhost:${PORT}!`)
  console.log('=========================================')
  console.log('')
  console.log('API Endpoints:')
  console.log('   GET    /api/todos     - Get all todos')
  console.log('   GET    /api/todos/:id - Get specific todo')
  console.log('   POST   /api/todos     - Add new todo')
  console.log('   PATCH  /api/todos/:id - Update todo')
  console.log('   DELETE /api/todos/:id - Delete todo')
  console.log('')
})
```

### Key SQL Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `SELECT` | Query data | `SELECT * FROM todos` |
| `INSERT` | Add data | `INSERT INTO todos (text) VALUES ('Go shopping')` |
| `UPDATE` | Modify data | `UPDATE todos SET completed = 1 WHERE id = 1` |
| `DELETE` | Delete data | `DELETE FROM todos WHERE id = 1` |

> **`?` Placeholder**: In SQL, `?` marks where a value will be inserted later. This prevents SQL Injection attacks!

### Practice Problem 4-1: Add Statistics API

Add the following API:
- `GET /api/todos/stats` - Return statistics
  - Total count
  - Completed count
  - Incomplete count

<details>
<summary>Hint</summary>

Use SQL's `COUNT` function:
```sql
SELECT COUNT(*) as total FROM todos
SELECT COUNT(*) as completed FROM todos WHERE completed = 1
```

</details>

<details>
<summary>See Answer</summary>

```javascript
// Note: This route must be defined before /api/todos/:id!
// Otherwise 'stats' will be interpreted as :id.

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
    console.error('Stats query error:', error)
    res.status(500).json({ error: 'Failed to load statistics' })
  }
})
```

</details>

---

## Part 5: Error Handling and Validation

**Error handling** is important for building stable APIs.

### HTTP Status Codes

| Code | Meaning | When to Use |
|------|---------|-------------|
| 200 | OK | Success |
| 201 | Created | New resource created successfully |
| 400 | Bad Request | Invalid request (input error) |
| 404 | Not Found | Resource not found |
| 500 | Internal Server Error | Server internal error |

### Input Validation Example

```javascript
app.post('/api/todos', (req, res) => {
  const { text } = req.body

  // 1. Check required value
  if (!text) {
    return res.status(400).json({
      error: 'Please enter todo content',
      field: 'text'
    })
  }

  // 2. Check type
  if (typeof text !== 'string') {
    return res.status(400).json({
      error: 'Todo content must be a string',
      field: 'text'
    })
  }

  // 3. Check length
  const trimmed = text.trim()
  if (trimmed.length === 0) {
    return res.status(400).json({
      error: 'Empty content cannot be added',
      field: 'text'
    })
  }

  if (trimmed.length > 500) {
    return res.status(400).json({
      error: 'Please enter within 500 characters',
      field: 'text',
      maxLength: 500,
      currentLength: trimmed.length
    })
  }

  // All validation passed -> Save data
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
    console.error('DB error:', error)
    res.status(500).json({ error: 'Server error occurred' })
  }
})
```

### Catching Errors with try-catch

```javascript
app.get('/api/todos', (req, res) => {
  try {
    // Code that runs normally
    const todos = db.prepare('SELECT * FROM todos').all()
    res.json(todos)
  } catch (error) {
    // If error occurs, comes here
    console.error('Error occurred:', error)
    res.status(500).json({ error: 'Server error occurred' })
  }
})
```

> **Important**: Without `try-catch`, the server could crash when errors occur! Add error handling to all APIs.

---

## Part 6: Understanding Middleware

Middleware is a **function that executes between request and response**.

```
+------------------------------------------------------------------+
|                    Middleware Flow                                 |
+------------------------------------------------------------------+
|                                                                    |
|  Request -> [Middleware1] -> [Middleware2] -> [Route Handler] -> Response |
|                                                                    |
|  Example:                                                          |
|  POST /api/todos                                                   |
|       |                                                            |
|       v                                                            |
|  [express.json()]  <- JSON parsing                                 |
|       |                                                            |
|       v                                                            |
|  [logRequest()]    <- Log recording                                |
|       |                                                            |
|       v                                                            |
|  [app.post handler] <- Actual processing                           |
|       |                                                            |
|       v                                                            |
|  Send response                                                     |
|                                                                    |
+------------------------------------------------------------------+
```

### Commonly Used Middleware

```javascript
const express = require('express')
const app = express()

// ===================================================================
// 1. JSON parsing middleware (built-in)
// ===================================================================
app.use(express.json())
// -> Automatically parses JSON in request body and puts it in req.body

// ===================================================================
// 2. Request logging middleware (custom)
// ===================================================================
app.use((req, res, next) => {
  const timestamp = new Date().toISOString()
  console.log(`[${timestamp}] ${req.method} ${req.url}`)
  next()  // Pass to next middleware
})

// ===================================================================
// 3. Error handling middleware (4 parameters)
// ===================================================================
app.use((err, req, res, next) => {
  console.error('Error occurred:', err)
  res.status(500).json({ error: 'Server error occurred' })
})
```

> **What is next()?** A function that passes control to the next middleware. If you don't call `next()`, the request stops there!

---

## Mini Quiz

### Quiz 1: HTTP Methods

Which HTTP method is used when adding new data?

A) GET
B) POST
C) PATCH
D) DELETE

<details>
<summary>See Answer</summary>

**Answer: B) POST**

- GET: Query data
- POST: Create new data
- PATCH: Modify part of data
- DELETE: Delete data

</details>

### Quiz 2: Status Codes

What status code should be returned when accessing a non-existent resource?

A) 200
B) 201
C) 400
D) 404

<details>
<summary>See Answer</summary>

**Answer: D) 404**

- 200: OK (success)
- 201: Created (creation success)
- 400: Bad Request (invalid request)
- 404: Not Found

</details>

### Quiz 3: SQL Commands

Which SQL command is used to modify data?

A) SELECT
B) INSERT
C) UPDATE
D) DELETE

<details>
<summary>See Answer</summary>

**Answer: C) UPDATE**

- SELECT: Query
- INSERT: Add
- UPDATE: Modify
- DELETE: Delete

</details>

### Quiz 4: Middleware

In the following code, what is the role of `next()`?

```javascript
app.use((req, res, next) => {
  console.log('log')
  next()
})
```

A) Sends response
B) Passes control to next middleware
C) Throws an error
D) Cancels request

<details>
<summary>See Answer</summary>

**Answer: B) Passes control to next middleware**

If `next()` is not called, the request stops at that middleware and no response is sent.

</details>

---

## Practice Assignments

### Basic Assignment: Complete Memo API

Build a memo API with the following features:

**Database Schema:**
```sql
CREATE TABLE memos (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  content TEXT,
  created_at TEXT DEFAULT CURRENT_TIMESTAMP,
  updated_at TEXT DEFAULT CURRENT_TIMESTAMP
)
```

**API Endpoints:**
- `GET /api/memos` - Get all memos
- `GET /api/memos/:id` - Get specific memo
- `POST /api/memos` - Add new memo (title required, content optional)
- `PATCH /api/memos/:id` - Update memo
- `DELETE /api/memos/:id` - Delete memo

**Additional Features (Challenge):**
- `GET /api/memos/search?q=keyword` - Search by title or content

### Advanced Assignment: Bookmark API

Build an API for saving URL bookmarks:

**Schema:**
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
- Basic CRUD + search functionality

---

## Glossary

| Term | Description |
|------|-------------|
| **API** | Method/rules for programs to communicate with each other |
| **REST API** | API design style using URLs and HTTP methods |
| **Express** | Node.js web server framework |
| **SQLite** | Lightweight file-based database |
| **CRUD** | Acronym for Create, Read, Update, Delete |
| **Middleware** | Function that executes between request and response |
| **HTTP Method** | Request types like GET, POST, PUT, PATCH, DELETE |
| **Status Code** | Numbers indicating response result like 200, 404, 500 |
| **JSON** | Format for representing data |
| **Query** | Command/question sent to database |

---

## Next Chapter Preview

In [Chapter 20: Building Full-Stack Apps - Frontend Integration and Authentication](../Chapter20-Full-Stack-Apps/README.md), we'll:
- Connect React frontend with backend
- Configure CORS
- Implement authentication system (JWT)
- Prepare for deployment

We'll connect the backend API created in this chapter to an actual interface and build a complete full-stack app!

---

## Summary

1. **Backend** handles data storage and business logic
2. **Express** makes it easy to build web servers
3. **HTTP methods** distinguish CRUD operations: GET (read), POST (create), PATCH (update), DELETE (delete)
4. **SQLite** provides permanent data storage
5. **Error handling** and **input validation** are essential!
6. **Middleware** handles common logic

Now that you've learned backend basics, let's connect it to a frontend in the next chapter!

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [Express.js Official Guide](https://expressjs.com/) - Express framework documentation
- [MDN HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) - Understanding HTTP protocol
- [SQLite Documentation](https://www.sqlite.org/docs.html) - SQLite official documentation

**Video Resources:**
- [Express.js Tutorial (YouTube)](https://www.youtube.com/results?search_query=express.js+tutorial+beginner)
- [REST API Design (YouTube)](https://www.youtube.com/results?search_query=rest+api+design+tutorial)
- [SQL Basics (YouTube)](https://www.youtube.com/results?search_query=sql+tutorial+beginner)

**Reading Materials:**
- [REST API Design Guide](https://restfulapi.net/) - RESTful API design principles
- [HTTP Status Code Reference](https://httpstatuses.com/) - Complete status code guide
- [API Security Best Practices](https://owasp.org/www-project-api-security/) - OWASP API security guide

**Related Tools:**
- [Postman](https://www.postman.com/) - API testing tool
- [Insomnia](https://insomnia.rest/) - REST client
- [DB Browser for SQLite](https://sqlitebrowser.org/) - SQLite GUI tool
- [Thunder Client](https://www.thunderclient.com/) - VS Code API testing extension

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
