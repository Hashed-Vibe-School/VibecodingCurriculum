# Chapter 13: Data Storage

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What a database is
- Using Supabase
- Saving and loading data

---

## Why Data Storage is Necessary

The websites built so far are "static." The content is fixed.

However, real applications have data that changes constantly:
- Sign up: save user information
- Write a post: save the content
- Like something: increase the count

For this, you need a **database**.

---

## What is a Database?

A database is similar to Excel. It stores data in tables.

### Example: Users Table

| id | name | email | joined_date |
|----|------|-------|-------------|
| 1 | John | john@email.com | 2024-01-15 |
| 2 | Jane | jane@email.com | 2024-01-16 |

### Example: Posts Table

| id | title | content | author_id | created_at |
|----|-------|---------|-----------|------------|
| 1 | First Post | Hello | 1 | 2024-01-15 |
| 2 | Second | Nice to meet you | 2 | 2024-01-16 |

---

## Supabase: The Easiest Database

Supabase is a free database service.

### Why Supabase?

- **Free**: Sufficient for personal projects
- **Simple**: Create tables like Excel
- **Fast**: Set up in a few clicks
- **Feature-rich**: Also handles login and file storage

### Other Options

| Service | Features | Difficulty |
|---------|----------|------------|
| **Supabase** | PostgreSQL based, nice UI | Easy |
| Firebase | Google product, realtime DB | Medium |
| PlanetScale | MySQL based | Medium |

Supabase is recommended for beginners.

---

## Getting Started with Supabase

### Step 1: Sign Up

1. Go to [supabase.com](https://supabase.com)
2. Log in with GitHub
3. Click "New Project"
4. Set project name and password
5. Select the region closest to you

### Step 2: Create a Table

In the dashboard:
1. Click "Table Editor"
2. Click "New Table"
3. Set table name and columns

### Using Claude

```
> Give me the SQL to create a todos table in Supabase
```

SQL that Claude provides:
```sql
create table todos (
  id bigint primary key generated always as identity,
  title text not null,
  is_done boolean default false,
  created_at timestamp default now()
);
```

Paste this in Supabase SQL Editor and run it.

---

## Connecting Supabase to Your Website

### Step 1: Get API Keys

In Supabase dashboard:
1. Settings → API
2. Copy `URL` and `anon key`

### Step 2: Add Connection Code

```
> Create Supabase connection code.
> URL: [your URL]
> API Key: [your key]
```

Code Claude creates:
```javascript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'YOUR_URL_HERE'
const supabaseKey = 'YOUR_KEY_HERE'

const supabase = createClient(supabaseUrl, supabaseKey)
```

---

## Data CRUD

CRUD is the basic data operations:
- **C**reate: Make new data
- **R**ead: Get data
- **U**pdate: Modify data
- **D**elete: Remove data

### Create: Add Data

```
> Create a function to add a todo
```

```javascript
async function addTodo(title) {
  const { data, error } = await supabase
    .from('todos')
    .insert({ title: title })

  if (error) console.error('Error:', error)
  return data
}

// Usage
addTodo('Learn Claude Code')
```

### Read: Get Data

```
> Create a function to get all todos
```

```javascript
async function getTodos() {
  const { data, error } = await supabase
    .from('todos')
    .select('*')

  if (error) console.error('Error:', error)
  return data
}

// Usage
const todos = await getTodos()
console.log(todos)
```

### Update: Modify Data

```
> Create a function to mark a todo as complete
```

```javascript
async function completeTodo(id) {
  const { data, error } = await supabase
    .from('todos')
    .update({ is_done: true })
    .eq('id', id)

  return data
}

// Usage
completeTodo(1)  // Complete todo with id 1
```

### Delete: Remove Data

```
> Create a function to delete a todo
```

```javascript
async function deleteTodo(id) {
  const { data, error } = await supabase
    .from('todos')
    .delete()
    .eq('id', id)

  return data
}

// Usage
deleteTodo(1)  // Delete todo with id 1
```

---

## Practice: Build a Todo App

### Goal

- Add todos
- View todo list
- Mark as complete
- Delete

### Step-by-Step

```
# 1. Project setup
> Create a todo-app project.
> Using HTML, CSS, JavaScript.
> Include Supabase connection.

# 2. Create table
> Give me the SQL to create a todos table

# 3. Create UI
> Create the todo input and list UI

# 4. Connect functionality
> Connect the add todo feature

# 5. Load list
> Load the todo list when page loads

# 6. Complete / Delete
> Add checkbox for completion, delete button
```

---

## Security Considerations

### Hide API Keys

Putting API keys directly in code is a security risk.

```
> Tell me how to manage API keys with environment variables
```

### Row Level Security (RLS)

Data security settings in Supabase:

1. Table Editor → Select table
2. Enable RLS
3. Add policies

```
> Tell me how to set up RLS policies
```

---

## Common Queries

### Filter by Condition

```javascript
// Only incomplete ones
const { data } = await supabase
  .from('todos')
  .select('*')
  .eq('is_done', false)
```

### Sort

```javascript
// Newest first
const { data } = await supabase
  .from('todos')
  .select('*')
  .order('created_at', { ascending: false })
```

### Limit Count

```javascript
// Only 10
const { data } = await supabase
  .from('todos')
  .select('*')
  .limit(10)
```

---

## Troubleshooting

### Connection Issues

```
> Getting Supabase connection error: [error message]
> What is wrong?
```

Common causes:
- Typo in URL or API key
- Network issue
- Access blocked due to RLS policy

### Data Not Showing

```
> Data adds but returns empty array when reading
```

Check the RLS policies.

---

## Mini Project: Database Apps

Build additional apps beyond the todo app.

### Project 1: Guestbook

```
> Create a guestbook app.
> - Name and message input
> - Sort newest first
> - Supabase integration
```

### Project 2: Notes App

```
> Create a notes app.
> - Create, edit, delete notes
> - Search functionality
> - Category/tag organization
```

### Project 3: Simple Forum

```
> Create a simple forum.
> - Post list and detail pages
> - Create, edit, delete posts
> - Pagination
```

### Advanced Challenges (For Experts)

```
> Add real-time chat using Supabase Realtime

> Add file upload using Supabase Storage

> Add user authentication using Supabase Auth
```

---

## Summary

What you learned in this chapter:
- [x] Database concepts
- [x] Setting up Supabase
- [x] Creating tables
- [x] CRUD operations
- [x] Connecting to website

You can now build real applications that save and load data.

The next chapter covers creating mini games.

Proceed to [Chapter 14: Mini Games](../Chapter14/README.md).
