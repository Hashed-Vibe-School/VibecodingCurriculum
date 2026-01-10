# Chapter 05: Terminal Commands

**English** | [한국어](./README.ko.md)

## What You'll Learn

- Having Claude execute commands
- Frequently used terminal commands
- Project execution and testing

---

## Having Claude Execute Commands

Claude Code can execute terminal commands on your behalf.

### Examples

```
> Show me the files in this folder
```

Claude runs `ls` and displays the result.

```
> Check the node version
```

Claude runs `node --version`.

### Approval Process

Claude requests confirmation before running commands:

```
Claude wants to run: ls -la
[Allow] [Deny]
```

Select `Allow` to execute.

---

## Useful Terminal Commands

Memorization is not required. Claude handles these. However, familiarity improves communication.

### File/Folder Commands

| Command | Function | Example |
|---------|----------|---------|
| `ls` | List files | `ls`, `ls -la` |
| `cd` | Change directory | `cd src` |
| `mkdir` | Create folder | `mkdir new-folder` |
| `rm` | Delete file | `rm file.txt` |
| `cp` | Copy file | `cp a.txt b.txt` |
| `mv` | Move/rename file | `mv old.txt new.txt` |

### Project Commands

| Command | Function | Example |
|---------|----------|---------|
| `npm install` | Install packages | `npm install` |
| `npm run dev` | Run dev server | `npm run dev` |
| `npm run build` | Build project | `npm run build` |
| `npm test` | Run tests | `npm test` |

### System Commands

| Command | Function | Example |
|---------|----------|---------|
| `pwd` | Show current location | `pwd` |
| `cat` | View file contents | `cat package.json` |
| `which` | Find program location | `which node` |

---

## Project Execution

Request project execution from Claude.

### Examples

```
> How do I run this project?
```

Claude examines `package.json` and provides instructions.

```
> Start the dev server
```

Claude runs a command like `npm run dev`.

```
> Run the tests
```

Claude runs `npm test` and displays the results.

---

## Immediate Execution with !

The `!` prefix runs commands immediately and shows Claude the result.

```
> !npm run build
```

Runs the build; Claude sees the result (success/failure).

```
> !git status
```

Checks git status; Claude recognizes the content.

### Difference Between ! and Regular Requests

| Method | Example | Difference |
|--------|---------|------------|
| Regular | `Run npm install` | Claude decides the command, requires approval |
| `!` prefix | `!npm install` | Runs immediately, Claude sees result |

Use `!` when the exact command is known.

---

## Practice: Creating a Simple Project

### 1. Folder Creation

```
> Create a folder called my-first-project
```

### 2. Project Initialization

```
> Initialize an npm project in that folder
```

Claude runs `npm init -y`.

### 3. Package Installation

```
> Install express
```

Claude runs `npm install express`.

### 4. Code Creation

```
> Create index.js. Make it a simple web server.
> It should show "Hello World".
```

### 5. Execution

```
> Run the server
```

Claude runs `node index.js`.

### 6. Verification

Open `http://localhost:3000` in the browser.

---

## Error Handling

When a command fails, inform Claude.

```
> That just errored. Why?
```

```
> Fix the error
```

Claude analyzes the error message and suggests solutions.

### Common Errors

**"command not found"**
The program is not installed. Request Claude to install it.

**"permission denied"**
Permission issue. May require `sudo`.

**"ENOENT: no such file or directory"**
File or folder does not exist. Verify the path.

---

## Dangerous Commands

Claude warns before executing dangerous commands.

### Commands Requiring Caution

| Command | Risk Level | Reason |
|---------|------------|--------|
| `rm -rf` | High | Permanently deletes files |
| `sudo` | High | Runs with admin privileges |
| `chmod 777` | Medium | Changes security settings |

Verify carefully when Claude proposes these commands.

---

## Mini Project: Custom API Server

Extend the Hello World server to build a simple API.

### Goals

- Understand Express servers
- Create API endpoints

### Creation

```
> Add an API to the server we just made.
> GET /api/hello should return { "message": "Hello!" }
```

### Extension

```
> Add GET /api/time that returns the current time

> Add GET /api/random that returns a random number between 1-100

> Add GET /api/greeting?name=John that returns "Hello, John!"
```

### Challenge

```
> Create POST /api/todos to add todos
> and GET /api/todos to get the list
> (storing in memory is acceptable)
```

---

## Summary

Covered in this chapter:
- [x] Having Claude execute commands
- [x] Frequently used terminal commands
- [x] Immediate execution with `!`
- [x] Project execution
- [x] Error handling

Part 1 (Getting Started) is complete.

The next Part covers Claude Code's core features in depth.

Proceed to [Chapter 06: Effective Prompting](../Chapter06/README.md).
