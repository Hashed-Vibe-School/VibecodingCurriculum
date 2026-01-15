# Chapter 13: Data Storage

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Why data storage is necessary
- Storing data in the browser with localStorage
- Understanding CRUD concepts

---

## Why Do You Need This?

Ever filled out a form online and accidentally closed the tab? That panic when everything you typed is gone? That's what happens without data storage.

**Real-world scenarios where you need data storage:**

- **Todo apps**: Your tasks should survive a page refresh
- **User preferences**: Dark mode setting shouldn't reset every visit
- **Shopping carts**: Items should stay in cart while you browse
- **Game progress**: High scores and unlocked levels should persist
- **Form drafts**: Half-written posts shouldn't disappear

> Without data storage, your app has the memory of a goldfish - it forgets everything the moment you look away.

### Simple Analogy: Notebook vs Whiteboard

A whiteboard is great for quick notes, but when you erase it or leave the room, everything's gone. That's your app without data storage.

A notebook keeps your notes even after you close it. That's your app WITH data storage. localStorage is your app's notebook.

---

## Try It Yourself: Save and Load One Thing

Before building a full app, let's see data storage in action with the simplest possible example.

```
> Create an HTML page with one input field and a "Save" button.
> When I click Save, store the input value in localStorage.
> When the page loads, show the saved value in the input field.
```

Type something, click Save, refresh the page. See how your text is still there? That's the magic of localStorage!

---

## Why Data Storage is Necessary

The websites built so far are "static." When you refresh the page, the data you entered disappears.

However, real apps need data to persist:
- Todo list → remains after refresh
- Notes app → content is still there when reopened
- Settings → remembers user preferences

For this, you need **data storage**.

---

## Data Storage Methods

There are several ways to store data:

| Method | Features | Use Case |
|--------|----------|----------|
| **localStorage** | Stored in browser, simple | Personal apps, learning |
| Supabase/Firebase | Stored on server, requires signup | Multi-user apps |
| Files | Direct file storage | Desktop apps |

In this chapter, we use **localStorage**. No signup required, and you can use it immediately.

---

## What is localStorage?

localStorage is a storage space provided by the browser.

### Features

- **Free**: No external service signup required
- **Simple**: Just a few lines of JavaScript
- **Persistent**: Data remains even after closing the browser
- **Capacity**: About 5MB (sufficient for most apps)

### How does it work?

```javascript
// Save
localStorage.setItem('key', 'value')

// Read
localStorage.getItem('key')

// Delete
localStorage.removeItem('key')
```

**Data storage request tips:**

```
> Save the todo list to localStorage
> Make the data persist after refresh
```

Be clear about what data you want to save for appropriate code.

---

## CRUD: The 4 Basic Data Operations

CRUD is the fundamental pattern for handling data:

- **C**reate: Make new data
- **R**ead: Get data
- **U**pdate: Modify data
- **D**elete: Remove data

Almost every app is a combination of these 4 operations. Todo apps, notes apps, social media—they're all CRUD at their core.

---

## Practice: Build a Todo App

Let's build a real todo app using localStorage.

### Step 1: Create the Basic Structure

```
> Create a todo list app.
> Using HTML, CSS, JavaScript.
> Include add, view list, and delete features.
> Save data with localStorage.
```

### Step 2: Create - Add a Todo

```
> Implement the add todo feature
```

Code Claude creates:

```javascript
function addTodo(text) {
  // Get existing todos
  const todos = JSON.parse(localStorage.getItem('todos')) || []

  // Add new todo
  const newTodo = {
    id: Date.now(),
    text: text,
    done: false
  }
  todos.push(newTodo)

  // Save
  localStorage.setItem('todos', JSON.stringify(todos))
}
```

**What is JSON.stringify/parse?**

localStorage can only store strings. To store arrays or objects, you need to convert them to strings. `JSON.stringify` converts object→string, and `JSON.parse` converts string→object.

### Step 3: Read - Load Todos

```
> Create a function to load saved todos
```

```javascript
function getTodos() {
  return JSON.parse(localStorage.getItem('todos')) || []
}

// Run on page load
window.onload = function() {
  const todos = getTodos()
  todos.forEach(todo => displayTodo(todo))
}
```

### Step 4: Update - Mark as Complete

```
> Add a completion checkbox feature
```

```javascript
function toggleTodo(id) {
  const todos = getTodos()
  const todo = todos.find(t => t.id === id)
  todo.done = !todo.done
  localStorage.setItem('todos', JSON.stringify(todos))
}
```

### Step 5: Delete - Remove

```
> Add a delete feature
```

```javascript
function deleteTodo(id) {
  const todos = getTodos()
  const filtered = todos.filter(t => t.id !== id)
  localStorage.setItem('todos', JSON.stringify(filtered))
}
```

---

## Improving with Feedback

Once the basic features work, refine with feedback:

```
> Show completed todos in gray

> Show "No todos" message when the list is empty

> Add a button to delete all completed items at once
```

---

## Checking with Developer Tools

To see the data stored in localStorage:

1. Press F12 in the browser (Developer Tools)
2. Select the Application tab
3. Click Local Storage in the left menu

You can directly view the stored data.

---

## When to Use localStorage vs a Database

### localStorage is Good For

- Personal apps (only you use it)
- Storing settings
- Simple notes, todo lists
- Learning projects

### A Database is Needed When

- Multiple people will use it together
- Access from different devices is needed
- Data exceeds 5MB
- User authentication is required

When you need a database, use services like Supabase or Firebase. The CRUD concepts apply exactly the same way.

---

## Mini Projects

### Project 1: Notes App

```
> Create a notes app.
> - Create, edit, delete notes
> - Save to localStorage
> - Search by title
```

### Project 2: Save Settings

```
> Create a dark mode toggle.
> Save the user's selected theme to localStorage
> so it persists on the next visit.
```

### Project 3: Habit Tracker

```
> Create a daily habit check app.
> - Manage habit list
> - Check completed habits for today
> - Save completion records to localStorage
```

### Advanced Challenges (For Experts)

```
> Add export/import localStorage data as JSON file.
> For backup and restore purposes.
```

---

## If It Doesn't Work?

Data storage can be tricky. Here are common issues and fixes:

### Data disappears after refresh
- Did you actually call `localStorage.setItem()`?
- Check browser console for errors (F12 > Console)
```
> The data isn't saving. Check my localStorage code.
```

### JSON.parse error
This happens when the stored data isn't valid JSON:
```
> Getting "Unexpected token" error when parsing localStorage data.
> How do I fix it?
```
Quick fix: Clear localStorage and start fresh:
```javascript
localStorage.clear()
```

### Data shows as "[object Object]"
You forgot to stringify before saving:
```javascript
// Wrong
localStorage.setItem('data', myObject)

// Correct
localStorage.setItem('data', JSON.stringify(myObject))
```

### localStorage is full
localStorage has a 5MB limit. If you're storing a lot:
```
> My localStorage is full. Help me check what's using space
> and clean up unnecessary data.
```

### Data works locally but not after deployment
localStorage is browser-specific. Data on your computer won't appear on someone else's browser - that's expected!

### Can't see localStorage in DevTools
- Make sure you're on the right domain
- Try Application tab > Local Storage > your site's URL

---

## Common Mistakes

Avoid these pitfalls!

### Mistake 1: Forgetting to parse when reading
```javascript
// Wrong - this is a string, not an array!
const todos = localStorage.getItem('todos')
todos.push(newTodo)  // Error!

// Correct
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
```

### Mistake 2: Not handling null/empty cases
First time user has no data:
```javascript
// Will crash if nothing stored
const todos = JSON.parse(localStorage.getItem('todos'))

// Safe way - default to empty array
const todos = JSON.parse(localStorage.getItem('todos')) || []
```

### Mistake 3: Using the wrong key name
Keys are case-sensitive and exact:
```javascript
localStorage.setItem('todos', data)
localStorage.getItem('Todos')  // Returns null! (capital T)
```

### Mistake 4: Not saving after changes
After modifying data, you must save it back:
```javascript
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
// Forgot to save! Changes are lost on refresh

// Don't forget this:
localStorage.setItem('todos', JSON.stringify(todos))
```

### Mistake 5: Storing sensitive data
localStorage is NOT secure. Never store:
- Passwords
- API keys
- Personal information

Anyone can open DevTools and see your localStorage!

---

## Summary

What you learned in this chapter:
- [x] Why data storage is needed
- [x] How to use localStorage
- [x] CRUD concepts
- [x] Building a todo app

CRUD is the foundation of almost every app. Once you learn the concept with localStorage, you can apply the same patterns when learning databases later.

The next chapter covers creating fun mini games.

Proceed to [Chapter 14: Mini Games](../Chapter14/README.md).
