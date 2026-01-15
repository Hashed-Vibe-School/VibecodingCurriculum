# Chapter 05: Terminal Commands

**English** | [한국어](./README.ko.md)

## What You'll Learn

- Having Claude execute commands
- Frequently used terminal commands
- Project execution and testing

---

## Why Do You Need This?

Files alone don't make software work. You need to run commands to install tools, start servers, and test your code. Think of commands as the actions that bring your files to life.

**Real-world scenarios:**
- You created a website, now you want to see it in your browser
- You need to install a package to add new features
- You want to run tests to make sure your code works
- Your code has an error and you need to debug it

### Simple Analogy: Kitchen Appliances

If files are ingredients, then terminal commands are your kitchen appliances.

- `npm install` = Going shopping (getting the ingredients you need)
- `npm run dev` = Turning on the stove (starting your project)
- `npm test` = Taste-testing (checking if it's good)
- `npm run build` = Packaging for delivery (preparing for the real world)

You don't need to know how the stove works internally. You just need to know "turn it on to cook." Same with commands--Claude knows the details, you just say what you want to happen.

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

Memorization is not required. Say "show me the files" and Claude runs `ls`. However, knowing basic commands helps you communicate more precisely.

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

LLMs are particularly strong at error analysis. Their training data includes countless error messages and solutions, enabling them to recognize error patterns and suggest appropriate fixes.

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

## Try It Yourself

Let's practice running commands step by step:

### Exercise 1: Check Your Environment
```
> What version of node do I have?
```
Claude will run `node --version` and tell you.

```
> Is npm installed?
```
Claude will check and let you know.

### Exercise 2: Create and Run a Simple Project
Follow along to create a mini web server:

```
1. Create a folder called test-server
2. Initialize npm in that folder
3. Install express
4. Create a simple server file
5. Run it and open in browser
```

Tell Claude each step or ask it to guide you through the whole process:
```
> Help me create a simple Express server from scratch. Guide me step by step.
```

### Exercise 3: Deal with an Error
If something fails (and something probably will), practice error handling:
```
> That just errored. What went wrong?
> How do I fix it?
```

---

## Common Mistakes

### 1. Running Commands in the Wrong Folder
If you create a project in `my-project` folder but run commands from a different location, things won't work. Always check where you are:
```
> Where am I right now?
```

### 2. Not Waiting for Commands to Finish
Some commands (like `npm install`) take time. Don't start typing new requests until the command finishes.

### 3. Forgetting to Install Dependencies
If Claude creates a project with a `package.json`, you need to run `npm install` before running the project. Otherwise you'll get "module not found" errors.

### 4. Closing the Terminal While Server is Running
If you start a server with `npm run dev` and close the terminal, the server stops. Keep the terminal open while using the server.

### 5. Ignoring Error Messages
Error messages look scary but they contain useful information. Copy the error and ask Claude to explain it.

---

## If It Doesn't Work...

**"command not found" error?**
- The program isn't installed. Ask Claude to install it.
- Example: "Install node" or "Install npm"

**"permission denied" error?**
- You might need admin rights. Ask Claude about using `sudo` (use carefully).
- On Windows, try running the terminal as Administrator.

**"ENOENT: no such file or directory"?**
- You're trying to access a file that doesn't exist.
- Check your current folder location.
- Make sure you spelled the file/folder name correctly.

**Server won't start?**
- Another program might be using the same port.
- Ask Claude: "Port 3000 seems busy. How do I fix this?"

**Changes not showing in browser?**
- Try refreshing with `Ctrl + F5` (hard refresh)
- Check if the server is still running
- Make sure you saved the file

**Everything seems stuck?**
- Press `Ctrl + C` to stop the current operation
- Try restarting Claude Code with `/exit` and `claude`

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
