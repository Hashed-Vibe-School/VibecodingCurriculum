# Chapter 15: Data Storage

**English** | [한국어](./README.ko.md)

---

## Chapter Navigation

| Previous | Current | Next |
|----------|---------|------|
| [Chapter 14: Deployment](../Chapter14-Deployment/README.md) | **Chapter 15: Data Storage** | [Chapter 16: Mini Games](../Chapter16-Mini-Games/README.md) |

**Prerequisites**: You should understand the JavaScript basics from Chapter 13 (Website Development).

---

## Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn

- Why data storage is necessary
- Storing data in the browser with localStorage
- Understanding CRUD concepts
- Managing data in real apps
- Handling complex data with JSON
- Limitations of data storage and alternatives

---

## Why Do You Need This?

Have you ever filled out a form online and accidentally closed the tab? You know that panic when everything you typed is gone? That's exactly what happens without data storage.

**Real-world scenarios where you need data storage:**

- **Todo apps**: Your tasks should survive a page refresh
- **User settings**: Dark mode setting shouldn't reset every visit
- **Shopping carts**: Items should stay in cart while you browse
- **Game progress**: High scores and unlocked levels should persist
- **Draft saving**: Half-written posts shouldn't disappear

> Without data storage, your app has the memory of a goldfish - it forgets everything the moment you look away.

### Simple Analogy: Notebook vs Whiteboard

A whiteboard is great for quick notes, but when you erase it or leave the room, everything's gone. That's your app without data storage.

A notebook keeps your notes even after you close it. That's your app WITH data storage. localStorage is your app's notebook.

### Understanding the Data Lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│                    Data Lifecycle                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   [User Input] → [Memory (RAM)] → [Storage (localStorage)]  │
│        ↓              ↓                  ↓                  │
│    Keyboard       Volatile           Permanent              │
│    Mouse      (refresh→gone)    (persists after close)      │
│                                                             │
│   Memory is fast but temporary, storage is slow but lasting │
└─────────────────────────────────────────────────────────────┘
```

> **Beginner Tip**: Computer memory (RAM) is like a desk. It's wide and fast, but everything gets cleared when power goes off. Storage (hard disk, localStorage) is like a drawer. It takes time to retrieve things, but they stay stored for a long time.

---

## Try It Yourself: Save and Load One Thing

Before building a complete app, let's see how data storage works with the simplest example.

```
> Create an HTML page. Just one input field and a "Save" button.
> When I click Save, store the input value in localStorage.
> When the page loads, show the saved value in the input field.
```

Type something, click Save, refresh the page. See how your text is still there? That's the magic of localStorage!

### Practice: Try It Yourself

Open browser developer tools (F12) and test directly in the Console tab:

```javascript
// 1. Save
localStorage.setItem('myName', 'John Doe')

// 2. Check
console.log(localStorage.getItem('myName'))  // 'John Doe' output

// 3. Refresh and check again
// (Data remains even after page refresh)

// 4. Delete
localStorage.removeItem('myName')

// 5. Delete all
localStorage.clear()
```

> **Pro tip**: You can visually check and directly edit localStorage in the Application tab of developer tools. Very useful for debugging.

---

## Why Is Data Storage Necessary?

The websites we've built so far are "static." When you refresh the page, entered content disappears.

But real apps need data to persist:
- Todo list → remains after refresh
- Notes app → content still there when reopened
- Settings → remembers user preferences

For this, you need **data storage**.

### Data Storage in Real Services

| Service | Data Stored | Storage Location |
|---------|-------------|------------------|
| Gmail | Emails, attachments | Server (database) |
| WhatsApp | Messages, photos | Server + local |
| Notepad | Text files | Local (hard disk) |
| Web games | Scores, levels | localStorage |
| YouTube | Watch history | Server + cookies |

> **Beginner Tip**: Most apps combine multiple storage methods. For example, WhatsApp stores recent messages locally and full history on servers. This way you can see recent messages even offline.

---

## Data Storage Methods

There are several ways to store data:

| Method | Features | Use Case | Difficulty |
|--------|----------|----------|------------|
| **localStorage** | Stored in browser, simple | Personal apps, learning | Easy |
| **sessionStorage** | Deleted when tab closes | Temporary data | Easy |
| **IndexedDB** | Large capacity, complex | Offline apps | Hard |
| **Supabase/Firebase** | Stored on server | Multi-user apps | Medium |
| **Files** | Direct file storage | Desktop apps | Medium |

### Storage Method Selection Guide

```
┌─────────────────────────────────────────────────────────────┐
│                Storage Method Selection Guide               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Question 1: Do others need to see the data?                │
│     ├─ YES → Server storage needed (Supabase, Firebase)     │
│     └─ NO → Local storage possible                          │
│                                                             │
│  Question 2: Need access from other devices?                │
│     ├─ YES → Server storage needed                          │
│     └─ NO → localStorage is fine                            │
│                                                             │
│  Question 3: Need to store more than 5MB?                   │
│     ├─ YES → IndexedDB or server                            │
│     └─ NO → localStorage is enough                          │
│                                                             │
│  Question 4: Should it be deleted when browser closes?      │
│     ├─ YES → sessionStorage                                 │
│     └─ NO → localStorage                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

In this chapter, we use **localStorage**. No signup required, and you can use it immediately.

---

## What is localStorage?

localStorage is a storage space provided by the browser.

### Features

- **Free**: No external service signup required
- **Simple**: Just a few lines of JavaScript
- **Persistent**: Data remains even after closing the browser
- **Capacity**: About 5MB (sufficient for most apps)
- **Synchronous**: Save/read completes instantly
- **String only**: Objects need JSON conversion

### localStorage vs sessionStorage vs Cookies

| Property | localStorage | sessionStorage | Cookies |
|----------|--------------|----------------|---------|
| Capacity | 5-10MB | 5-10MB | 4KB |
| Expiration | Permanent | Deleted when tab closes | Configurable |
| Sent to server | No | No | Yes (every request) |
| Access | JavaScript | JavaScript | JavaScript + Server |

### How Does It Work?

```javascript
// Save - store as key-value pair
localStorage.setItem('name', 'value')

// Read - find value by key
localStorage.getItem('name')

// Delete - delete specific key
localStorage.removeItem('name')

// Delete all - full reset
localStorage.clear()

// Number of stored items
localStorage.length

// Get nth key name
localStorage.key(0)
```

> **Beginner Tip**: localStorage is like a locker. You put items (values) in and out by locker number (key). Every locker needs a name tag (key).

### More Conversation Examples

**Basic save request:**

```
> Save the username to localStorage
```

**Conditional save request:**

```
> Only save when the input field is not empty
```

**Save multiple data:**

```
> Save user settings (theme, language, font size) all at once and load them
```

**Save confirmation request:**

```
> Show "Saved successfully" alert when save succeeds
```

**Auto-save request:**

```
> Auto-save to localStorage every time text is entered.
> Add 1 second delay (apply debounce).
```

Be clear about what data you want to save for appropriate code.

---

## JSON: Storing Complex Data

localStorage can only store strings. But we usually need to store arrays or objects.

### The Problem

```javascript
// If you just store an object?
const user = { name: 'John Doe', age: 25 }
localStorage.setItem('user', user)

// When you read it...
console.log(localStorage.getItem('user'))
// Output: "[object Object]"  <- Not what we wanted!
```

### The Solution: JSON

```javascript
// JSON.stringify: object → string
const user = { name: 'John Doe', age: 25 }
const jsonString = JSON.stringify(user)
// Result: '{"name":"John Doe","age":25}'

localStorage.setItem('user', jsonString)

// JSON.parse: string → object
const savedString = localStorage.getItem('user')
const savedUser = JSON.parse(savedString)
// Result: { name: 'John Doe', age: 25 }
```

### What is JSON?

Stands for **J**ava**S**cript **O**bject **N**otation.

```
┌─────────────────────────────────────────────────────────────┐
│                    JSON Conversion Process                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   JavaScript Object         JSON String          localStorage│
│   { name: "John" }    ─→  '{"name":"John"}'    ─→   Save!   │
│                    stringify                                │
│                                                             │
│   JavaScript Object         JSON String          localStorage│
│   { name: "John" }    ←─  '{"name":"John"}'    ←─   Read!   │
│                    parse                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### JSON Conversion with Various Data Types

```javascript
// Arrays
const fruits = ['apple', 'banana', 'orange']
localStorage.setItem('fruits', JSON.stringify(fruits))
// Stored: '["apple","banana","orange"]'

// Complex objects
const settings = {
  theme: 'dark',
  language: 'en',
  notifications: {
    email: true,
    push: false
  },
  favorites: [1, 5, 12]
}
localStorage.setItem('settings', JSON.stringify(settings))

// When reading
const loadedSettings = JSON.parse(localStorage.getItem('settings'))
console.log(loadedSettings.theme)  // 'dark'
console.log(loadedSettings.notifications.email)  // true
```

> **Note**: Some things cannot be converted to JSON:
> - Functions
> - undefined
> - Symbol
> - Objects with circular references

> **Pro tip**: Date objects are converted to strings. You need to convert back with `new Date(string)` when reading.

---

## CRUD: The 4 Basic Data Operations

CRUD is the fundamental pattern for handling data:

- **C**reate: Make new data
- **R**ead: Get saved data
- **U**pdate: Modify existing data
- **D**elete: Remove data

Almost every app is a combination of these 4 operations. Todo apps, notes apps, social media - they're all CRUD at their core.

### CRUD in Real Life Examples

| App | Create | Read | Update | Delete |
|-----|--------|------|--------|--------|
| Notes | Write new note | View note list | Edit note | Delete note |
| Contacts | Add contact | Search contacts | Change phone number | Delete contact |
| Social Media | Create post | View feed | Edit post | Delete post |
| Email | Compose email | View inbox | Edit draft | Delete email |

### CRUD and localStorage Method Mapping

```javascript
// Create
localStorage.setItem('newItem', 'value')

// Read
const data = localStorage.getItem('newItem')

// Update - overwrite with same key
localStorage.setItem('newItem', 'new value')

// Delete
localStorage.removeItem('newItem')
```

> **Beginner Tip**: CRUD is a magic word. For any app you build, thinking "What's the CRUD for this app?" makes feature design easier.

---

## Mini Quiz

### Quiz 1: localStorage Basics
What is the result of localStorage.getItem('count') after running localStorage.setItem('count', 5)?

A) 5 (number)
B) "5" (string)
C) undefined
D) null

<details>
<summary>Show Answer</summary>

**Answer: B) "5" (string)**

localStorage stores everything as strings. Even if you store the number 5, it's converted to "5".
To use as a number, convert with `parseInt(localStorage.getItem('count'))` or `Number(localStorage.getItem('count'))`.

</details>

### Quiz 2: JSON Conversion
What's wrong with this code?

```javascript
const todos = localStorage.getItem('todos')
todos.push({ text: 'New todo' })
```

A) The 'todos' key might not exist in localStorage
B) todos is a string so push cannot be used
C) Cannot directly push an object
D) Both A and B

<details>
<summary>Show Answer</summary>

**Answer: D) Both A and B**

1. `localStorage.getItem('todos')` returns a string. To use the array method `push()`, you must first convert with `JSON.parse()`.
2. If the key doesn't exist, it returns `null`, and `null.push()` will error.

Correct code:
```javascript
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push({ text: 'New todo' })
localStorage.setItem('todos', JSON.stringify(todos))
```

</details>

### Quiz 3: Identifying CRUD Operations
"User changes their profile photo" - which CRUD operation is this?

A) Create
B) Read
C) Update
D) Delete

<details>
<summary>Show Answer</summary>

**Answer: C) Update**

Since you're "modifying" the existing profile photo to a new one, it's an Update operation.
If registering a profile photo for the first time, it would be Create; removing it would be Delete.

</details>

---

## Practice: Build a Todo List App

Let's build a real todo list app using localStorage.

### Overall Structure Preview

```
┌─────────────────────────────────────────────────────────────┐
│                     Todo App Structure                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   [HTML]              [JavaScript]           [localStorage] │
│   - Input field       - addTodo()            - 'todos' key  │
│   - Add button        - getTodos()           - JSON array   │
│   - Todo list         - toggleTodo()                        │
│   - Delete button     - deleteTodo()                        │
│                       - displayTodos()                      │
│                                                             │
│   User input → JavaScript processing → localStorage save    │
│                       ↓                                     │
│              Display list ← Read from localStorage          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Step 1: Create the Basic Structure

```
> Create a todo list app.
> Using HTML, CSS, JavaScript.
> Include add todo, view list, delete features.
> Save data with localStorage.
```

### Step 2: Create - Add a Todo

```
> Implement the add todo feature
```

Code Claude creates:

```javascript
function addTodo(text) {
  // Check for empty input
  if (!text.trim()) {
    alert('Please enter a todo!')
    return
  }

  // Get existing todo list
  // || [] means use empty array when nothing is saved
  const todos = JSON.parse(localStorage.getItem('todos')) || []

  // Create new todo object
  const newTodo = {
    id: Date.now(),       // Use current time as ID (guarantees uniqueness)
    text: text,           // Todo content
    done: false,          // Completion status
    createdAt: new Date().toISOString()  // Creation time
  }

  // Add to array
  todos.push(newTodo)

  // Save to localStorage
  localStorage.setItem('todos', JSON.stringify(todos))

  // Refresh display
  displayTodos()
}
```

> **Beginner Tip**: `Date.now()` returns milliseconds since January 1, 1970 until now. It gives a different number every moment, so it's great for unique IDs.

### Step 3: Read - Load Todos

```
> Create a function to load saved todos
```

```javascript
// Get todo list
function getTodos() {
  // Handle so JSON.parse doesn't error on null
  const saved = localStorage.getItem('todos')
  if (!saved) return []

  try {
    return JSON.parse(saved)
  } catch (error) {
    console.error('Todo list parsing error:', error)
    return []  // Return empty array on error
  }
}

// Display on screen
function displayTodos() {
  const todos = getTodos()
  const container = document.getElementById('todo-list')

  // Message when no todos
  if (todos.length === 0) {
    container.innerHTML = '<p class="empty-message">No todos. Add a new todo!</p>'
    return
  }

  // Generate todo list HTML
  container.innerHTML = todos.map(todo => `
    <div class="todo-item ${todo.done ? 'completed' : ''}" data-id="${todo.id}">
      <input type="checkbox" ${todo.done ? 'checked' : ''}
             onchange="toggleTodo(${todo.id})">
      <span class="todo-text">${todo.text}</span>
      <button onclick="deleteTodo(${todo.id})" class="delete-btn">Delete</button>
    </div>
  `).join('')
}

// Run on page load
window.onload = function() {
  displayTodos()
}
```

> **Note**: `JSON.parse()` throws an error on invalid JSON strings. Wrapping in try-catch is safer.

### Step 4: Update - Mark as Complete

```
> Add a completion checkbox feature
```

```javascript
function toggleTodo(id) {
  const todos = getTodos()

  // Find todo with that ID
  const todoIndex = todos.findIndex(t => t.id === id)

  if (todoIndex === -1) {
    console.error('Todo not found:', id)
    return
  }

  // Toggle completion status (true ↔ false)
  todos[todoIndex].done = !todos[todoIndex].done

  // Record completion time
  if (todos[todoIndex].done) {
    todos[todoIndex].completedAt = new Date().toISOString()
  } else {
    delete todos[todoIndex].completedAt
  }

  // Save
  localStorage.setItem('todos', JSON.stringify(todos))

  // Refresh display
  displayTodos()
}
```

> **Pro tip**: `find()` returns the first element matching the condition, `findIndex()` returns its index. For modifications, you need the index.

### Step 5: Delete - Remove

```
> Add a delete feature
```

```javascript
function deleteTodo(id) {
  // Confirm deletion
  if (!confirm('Are you sure you want to delete?')) {
    return
  }

  const todos = getTodos()

  // Create new array excluding that ID
  const filtered = todos.filter(t => t.id !== id)

  // Save
  localStorage.setItem('todos', JSON.stringify(filtered))

  // Refresh display
  displayTodos()
}

// Batch delete completed todos
function deleteCompletedTodos() {
  const todos = getTodos()
  const completedCount = todos.filter(t => t.done).length

  if (completedCount === 0) {
    alert('No completed todos.')
    return
  }

  if (!confirm(`Delete ${completedCount} completed todos?`)) {
    return
  }

  const remaining = todos.filter(t => !t.done)
  localStorage.setItem('todos', JSON.stringify(remaining))
  displayTodos()
}
```

> **Beginner Tip**: `filter()` creates a new array with only elements matching the condition. The original array is unchanged.

---

## More Conversation Examples

### Feature Addition Request

```
> Add priority (high/medium/low) to todos
```

**Claude's response example:**
```javascript
const newTodo = {
  id: Date.now(),
  text: text,
  done: false,
  priority: 'medium'  // 'high', 'medium', 'low'
}
```

### Sort Feature Request

```
> Sort by priority. High should be at the top.
```

**Claude's response example:**
```javascript
function sortByPriority(todos) {
  const order = { high: 0, medium: 1, low: 2 }
  return todos.sort((a, b) => order[a.priority] - order[b.priority])
}
```

### Search Feature Request

```
> Add todo search feature.
> Filter in real-time as you type.
```

### Due Date Request

```
> Add due date to each todo.
> Show overdue items in red.
> Show items due today in orange.
```

### Statistics Feature Request

```
> Show todo statistics.
> - Total count
> - Completed count
> - Incomplete count
> - Completion rate
```

### Error Resolution Conversation

```
> Todos aren't saving. No console errors.
```

**Claude's response:** "Let's check a few things..."

```
> Added todos disappear when I refresh
```

**Claude's response:** "Let's check if localStorage.setItem() is being called..."

---

## Improving with Feedback

Once basic features work, improve with feedback:

### Visual Improvements

```
> Show completed todos in gray with strikethrough

> Different colors by priority (high: red, medium: yellow, low: green)

> Add drag and drop to reorder
```

### UX Improvements

```
> Show "No todos" message when list is empty

> Add todo when pressing Enter key

> Auto-clear input field after adding todo

> Ask for confirmation before deleting
```

### Advanced Features

```
> Add button to delete all completed items at once

> Allow categorizing todos

> Support dark mode

> Add data export/import feature
```

---

## Checking with Developer Tools

To check data stored in localStorage:

1. Press F12 in browser (Developer Tools)
2. Select Application tab (Chrome) / Storage tab (Firefox)
3. Click Local Storage in left menu
4. Select site URL

You can directly view the stored data.

### What You Can Do in Developer Tools

| Action | Method |
|--------|--------|
| View value | Click key |
| Edit value | Double-click value to edit |
| Delete item | Right-click item > Delete |
| Delete all | Right-click > Clear |
| Add new item | Double-click empty space |

> **Pro tip**: If localStorage gets corrupted during development, clear with `localStorage.clear()` and start fresh.

### Direct Manipulation in Console

```javascript
// View stored data (pretty print)
console.log(JSON.parse(localStorage.getItem('todos')))

// View specific todo
const todos = JSON.parse(localStorage.getItem('todos'))
console.table(todos)  // View as table

// Force modify data (for testing)
const todos = JSON.parse(localStorage.getItem('todos'))
todos[0].text = 'Modified todo'
localStorage.setItem('todos', JSON.stringify(todos))
```

---

## When to Use localStorage vs a Database?

### localStorage is Good For

- Personal apps (only you use it)
- Storing settings (theme, language, font size, etc.)
- Simple notes, todo lists
- Learning projects
- When it needs to work offline
- Cache data (temporary API response storage)

### When You Need a Database

- Multiple people use it together (collaboration apps)
- Need access from different devices (sync)
- Large amounts of data (over 5MB)
- User authentication needed
- Need data analysis, statistics
- Backup/recovery is important

### Growth Path by Level

```
┌─────────────────────────────────────────────────────────────┐
│                  Data Storage Growth Path                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [Beginner]         [Intermediate]        [Advanced]        │
│  localStorage  →  Supabase/Firebase  →  Custom Server       │
│                                                             │
│  - Personal project  - User auth          - Full control    │
│  - Learning          - Real-time sync     - Complex queries │
│  - Prototype         - Multi-device       - Large data      │
│                      - Team collaboration - Custom optimize │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

When you need a database, use services like Supabase or Firebase. CRUD concepts apply exactly the same way.

```
> Change the todo app to save data with Supabase
```

> **Beginner Tip**: Start with localStorage, migrate to a database as your app grows. Knowing CRUD makes the transition easy.

---

## Practice Exercises

### Difficulty 1: Notes App

```
> Create a notes app.
> - Create, edit, delete notes
> - Save to localStorage
> - Search by title
```

**Success criteria:**
- [ ] Can create new notes
- [ ] Can edit existing notes
- [ ] Can delete notes
- [ ] Notes persist after refresh
- [ ] Can search notes by title

### Difficulty 2: Save Dark Mode Setting

```
> Create a dark mode toggle.
> Save the user's selected theme to localStorage
> so it persists on next visit.
> Also detect system settings.
```

**Success criteria:**
- [ ] Dark/light mode toggle button exists
- [ ] Selected theme saved to localStorage
- [ ] Selected theme persists after refresh
- [ ] Uses system theme as default
- [ ] Has smooth transition animation

### Difficulty 3: Habit Tracker

```
> Create a daily habit check app.
> - Manage habit list (add, edit, delete)
> - Check completed habits for today
> - Date-based completion records
> - Show weekly/monthly statistics
> - Display streak (consecutive days)
```

**Success criteria:**
- [ ] Can add, edit, delete habits
- [ ] Can check today's completion for each habit
- [ ] Can view past date records
- [ ] Can see weekly completion rate as graph
- [ ] Streak is displayed
- [ ] All data persists after refresh

---

## Challenge Exercises

### Challenge 1: Data Backup/Restore

```
> Add export/import localStorage data as JSON file feature.
> For backup and restore purposes.
```

**Hints:**
- File download: Use Blob and URL.createObjectURL
- File upload: Use input type="file" and FileReader

### Challenge 2: Data Sync Simulation

```
> When opening the same app in two browser tabs,
> make it so changes in one reflect in the other.
> (Use localStorage's storage event)
```

### Challenge 3: Capacity Management

```
> Show localStorage usage,
> warn when over 80% used.
> Add auto-cleanup of old data feature.
```

---

## Troubleshooting

Data storage can be tricky. Here are common issues and fixes.

### Data Disappears After Refresh

**Checklist:**
1. Did you actually call `localStorage.setItem()`?
2. Did you check for errors in browser console? (F12 > Console)
3. Did you verify with `localStorage.getItem()` after saving?

```
> Data isn't saving. Check my localStorage code.
```

**Debugging method:**
```javascript
// Add logs before and after saving
console.log('Before save:', localStorage.getItem('todos'))
localStorage.setItem('todos', JSON.stringify(todos))
console.log('After save:', localStorage.getItem('todos'))
```

### JSON.parse Error

Occurs when stored data isn't valid JSON:

```
Uncaught SyntaxError: Unexpected token 'u' at position 0
```

This error usually occurs when trying to parse `undefined`.

```
> Getting "Unexpected token" error when parsing localStorage data.
> How do I fix it?
```

**Solution:**
```javascript
// Safe parsing function
function safeGetItem(key, defaultValue = null) {
  try {
    const item = localStorage.getItem(key)
    return item ? JSON.parse(item) : defaultValue
  } catch (error) {
    console.error(`${key} parsing error:`, error)
    return defaultValue
  }
}
```

Quick fix: Clear localStorage and start fresh:
```javascript
localStorage.clear()
```

### Data Shows as "[object Object]"

Forgot to stringify before saving:

```javascript
// Wrong - object becomes [object Object] when converted to string
localStorage.setItem('data', myObject)

// Correct - convert to JSON string before saving
localStorage.setItem('data', JSON.stringify(myObject))
```

### localStorage is Full

localStorage has a 5MB limit.

```
> My localStorage is full. Help me check what's using space
> and clean up unnecessary data.
```

**How to check capacity:**
```javascript
// Check total usage
function getLocalStorageSize() {
  let total = 0
  for (let key in localStorage) {
    if (localStorage.hasOwnProperty(key)) {
      total += localStorage[key].length * 2  // x2 for UTF-16
    }
  }
  return (total / 1024 / 1024).toFixed(2) + ' MB'
}

console.log('localStorage usage:', getLocalStorageSize())

// Check size by key
for (let key in localStorage) {
  if (localStorage.hasOwnProperty(key)) {
    const size = (localStorage[key].length * 2 / 1024).toFixed(2)
    console.log(`${key}: ${size} KB`)
  }
}
```

### Works Locally But Not After Deployment

localStorage is separate per browser and per domain.

- localhost's localStorage ≠ mysite.com's localStorage
- Chrome's localStorage ≠ Firefox's localStorage
- It's normal that data from your computer doesn't appear in someone else's browser

### Can't See localStorage in DevTools

- Make sure you're on the right domain
- Try Application tab > Local Storage > select your site's URL
- localStorage might not work with "file://" protocol (use Live Server)

### Doesn't Work in Incognito Mode

In incognito (private) browsing mode, localStorage may be restricted:
- Saves work but deleted when window closes
- Completely blocked in some browsers

---

## Common Mistakes

Avoid these pitfalls.

### Mistake 1: Forgetting to Parse When Reading

```javascript
// Wrong - this is a string, not an array!
const todos = localStorage.getItem('todos')
todos.push(newTodo)  // Error! push is not a function

// Correct
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
```

### Mistake 2: Not Handling null/Empty Cases

First-time users have no data:

```javascript
// Will crash if nothing stored
const todos = JSON.parse(localStorage.getItem('todos'))
// Parsing null causes error!

// Safe way - default to empty array
const todos = JSON.parse(localStorage.getItem('todos')) || []
```

### Mistake 3: Wrong Key Name

Keys are case-sensitive and exact:

```javascript
localStorage.setItem('todos', data)
localStorage.getItem('Todos')  // Returns null! (capital T)
localStorage.getItem('todo')   // Returns null! (missing s)
```

> **Pro tip**: Define key names as constants to reduce mistakes.

```javascript
const STORAGE_KEYS = {
  TODOS: 'todos',
  SETTINGS: 'settings',
  USER: 'user'
}

localStorage.setItem(STORAGE_KEYS.TODOS, data)
localStorage.getItem(STORAGE_KEYS.TODOS)
```

### Mistake 4: Not Saving After Changes

After modifying data, you must save it back:

```javascript
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
// Forgot to save! Changes lost on refresh

// Don't forget this:
localStorage.setItem('todos', JSON.stringify(todos))
```

> **Beginner Tip**: Data from localStorage is a "copy." Modifying it doesn't change the original (localStorage). You must save it back.

### Mistake 5: Storing Sensitive Data

localStorage is NOT secure. Never store:
- Passwords
- API keys
- Credit card information
- Social security numbers

Anyone can open DevTools and see your localStorage!

```javascript
// Never do this!
localStorage.setItem('password', '1234')
localStorage.setItem('apiKey', 'sk-xxxxx')
```

### Mistake 6: Performance Issues from Synchronous Calls

localStorage operates synchronously. Frequent reads and writes of large data can slow your app.

```javascript
// Not great - reads and writes entire data every time
function updateTodoText(id, newText) {
  const todos = JSON.parse(localStorage.getItem('todos'))
  // modify
  localStorage.setItem('todos', JSON.stringify(todos))
}

// Improved - cache in memory, save only when needed
let todosCache = null

function getTodosFromCache() {
  if (!todosCache) {
    todosCache = JSON.parse(localStorage.getItem('todos')) || []
  }
  return todosCache
}

function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(todosCache))
}
```

---

## Practical Pattern: Reusable Storage Utility

A storage utility you can use right away in your projects.

```javascript
// storage.js - Reusable localStorage wrapper
const Storage = {
  // Save
  set(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value))
      return true
    } catch (error) {
      console.error('Save failed:', error)
      return false
    }
  },

  // Read
  get(key, defaultValue = null) {
    try {
      const item = localStorage.getItem(key)
      return item ? JSON.parse(item) : defaultValue
    } catch (error) {
      console.error('Read failed:', error)
      return defaultValue
    }
  },

  // Delete
  remove(key) {
    localStorage.removeItem(key)
  },

  // Delete all
  clear() {
    localStorage.clear()
  },

  // Check if key exists
  has(key) {
    return localStorage.getItem(key) !== null
  },

  // Check capacity (KB)
  getSize() {
    let total = 0
    for (let key in localStorage) {
      if (localStorage.hasOwnProperty(key)) {
        total += localStorage[key].length * 2
      }
    }
    return (total / 1024).toFixed(2)
  }
}

// Usage example
Storage.set('user', { name: 'John Doe', age: 25 })
const user = Storage.get('user', { name: 'Guest' })
console.log(user.name)  // 'John Doe'
```

---

## Summary

What you learned in this chapter:
- [x] Why data storage is needed
- [x] How to use localStorage
- [x] Storing complex data with JSON
- [x] CRUD concepts and implementation
- [x] Building a todo list app
- [x] Common mistakes and solutions

CRUD is the foundation of almost every app. Once you learn the concept with localStorage, you can apply the same patterns when learning databases later.

---

## Glossary

| Term | Description |
|------|-------------|
| **localStorage** | Browser-provided key-value storage. Permanent storage. |
| **sessionStorage** | Similar to localStorage but deleted when tab closes |
| **JSON** | JavaScript Object Notation. Data exchange format. |
| **JSON.stringify()** | Converts JavaScript object to JSON string |
| **JSON.parse()** | Converts JSON string to JavaScript object |
| **CRUD** | Acronym for Create, Read, Update, Delete |
| **Key** | Unique name that identifies data |
| **Value** | Actual data connected to a key |
| **Serialization** | Converting object to storable/transmittable form (stringify) |
| **Deserialization** | Converting stored/transmitted data back to object (parse) |
| **Synchronous** | Operations execute sequentially. Wait until complete. |
| **Cache** | Temporarily storing frequently used data for performance |

---

## Next Steps

In the next chapter, we'll create fun mini games. We'll also add features to save high scores with localStorage!

Proceed to [Chapter 16: Mini Games](../Chapter16-Mini-Games/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [MDN Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) - localStorage official documentation
- [JSON Official Site](https://www.json.org/json-en.html) - JSON format explanation

**Video Resources:**
- [localStorage Tutorial (YouTube)](https://www.youtube.com/results?search_query=javascript+localstorage+tutorial) - localStorage usage
- [CRUD App Tutorial (YouTube)](https://www.youtube.com/results?search_query=javascript+crud+tutorial) - CRUD concept practice

**Reading:**
- [JavaScript.info - localStorage](https://javascript.info/localstorage) - localStorage detailed explanation
- [IndexedDB Introduction](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) - Large capacity storage

**Related Tools:**
- [Supabase](https://supabase.com/) - Open source backend service
- [Firebase](https://firebase.google.com/) - Google backend platform
- [JSON Formatter](https://jsonformatter.org/) - JSON formatter/validator

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
