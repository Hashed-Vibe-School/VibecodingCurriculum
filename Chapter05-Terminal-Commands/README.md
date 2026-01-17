# Chapter 05: Terminal Commands

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Learn to have Claude execute commands
- Know frequently used terminal commands
- Understand the npm ecosystem
- Understand server concepts and local development environment
- Run and test projects

---

## ⏱️ Estimated Time

- Reading: **30 minutes**
- Practice: **30 minutes**

---

## 📋 Prerequisites

- Claude Code installed and logged in
- Basic understanding of file handling (Chapter 04)

---

## 🔗 Previous Chapter Review

In [Chapter 04](../Chapter04-Working-with-Files/README.md), we learned to reference files with `@` and create/modify/delete files. Now it's time to learn terminal commands that "bring files to life."

---

## Why Do You Need This?

Files alone don't make software work. You need to run commands to install tools, start servers, and test your code. Think of commands as the actions that bring your files to life.

**Real-world scenarios:**
- You created a website, now you want to see it in your browser
- You need to install a package to add new features
- You want to test if your code works properly
- Your code has an error and you need to debug it

### Simple Analogy: Kitchen Appliances

If files are ingredients, then terminal commands are your kitchen appliances.

```
┌────────────────────────────────────────────┐
│           Kitchen Appliance Analogy        │
├────────────────────────────────────────────┤
│                                            │
│  npm install  = Going shopping (getting    │
│                 ingredients)               │
│  npm run dev  = Turning on the stove       │
│                 (starting to cook)         │
│  npm test     = Taste-testing (checking    │
│                 if it's good)              │
│  npm run build= Packaging (preparing for   │
│                 delivery)                  │
│                                            │
│  You don't need to know how the stove      │
│  works internally. You just need to know   │
│  "turn it on to cook."                     │
│                                            │
└────────────────────────────────────────────┘
```

You don't need to know how the gas stove works internally. You just need to know "turn it on to cook." Same with commands—Claude knows the details, you just say what you want to happen.

---

## History and Importance of the Terminal

### Why is the terminal still important?

Why do we still use the terminal when we have GUI (graphical interfaces)?

| Aspect | GUI (Graphical) | CLI (Terminal) |
|--------|-----------------|----------------|
| **Speed** | Mouse movement, clicking | Fast typing |
| **Automation** | Difficult | Easy with scripts |
| **Remote work** | Requires screen sharing | Works with text only |
| **Precision** | Limited | Fine-grained control |
| **Server management** | Mostly impossible | Essential |

### Why developers use the terminal

1. **Efficiency**: Same task completed faster
2. **Automation**: Repetitive tasks via scripts
3. **Server access**: Cloud servers have no GUI
4. **Tool integration**: Most dev tools support CLI
5. **Standard**: Developers worldwide use the same approach

> 💡 **Beginner Tip**
>
> Don't be afraid of the terminal!
> With Claude Code, you don't need to memorize commands.
> Just say "start the server" and that's it.

### Brief Terminal History

```
1960s: Teletype (physical typewriter)
    ↓
1970s: Terminal screens appear
    ↓
1980s: Personal computers, DOS
    ↓
1990s: Windows GUI, but developers keep using terminals
    ↓
2000s~: Web servers, cloud → terminals become more important
    ↓
Present: Execute terminal commands in natural language with AI
```

---

## Having Claude Execute Commands

Claude Code can execute terminal commands on your behalf.

### Requesting in Natural Language

```
> Show me the files in this folder
```

Claude runs the `ls` command and displays the result.

```
> Check the node version
```

Claude runs `node --version`.

```
> Show everything including hidden files
```

Claude runs `ls -la`.

### Approval Process

Claude requests confirmation before running commands:

```
Claude wants to run: ls -la
[Allow] [Deny]
```

Select `Allow` to execute.

> 💡 **Beginner Tip**
>
> You don't need to memorize terminal commands!
> Say "show me the file list" and Claude runs `ls` for you.
> Just speak in natural language.

### Immediate Execution with ! Prefix

Adding `!` runs the command immediately and shows Claude the result.

```
> !npm run build
```

Runs the build; Claude sees the result (success/failure).

```
> !git status
```

Checks git status; Claude recognizes the content.

---

## 20 Essential Terminal Commands

You don't need to memorize these. However, knowing basic commands helps you make more precise requests.

### File/Folder Commands (7)

| Command | Function | Natural Language Request | Example |
|---------|----------|--------------------------|---------|
| `ls` | List files | "Show me the file list" | `ls -la` |
| `cd` | Change directory | "Move to the src folder" | `cd src` |
| `mkdir` | Create folder | "Create a folder called new-folder" | `mkdir new-folder` |
| `rm` | Delete file | "Delete this file" | `rm file.txt` |
| `cp` | Copy file | "Copy this file" | `cp a.txt b.txt` |
| `mv` | Move/rename file | "Rename this file" | `mv old.txt new.txt` |
| `pwd` | Check current location | "Where am I now?" | `pwd` |

### Project Commands (7)

| Command | Function | Natural Language Request | Example |
|---------|----------|--------------------------|---------|
| `npm install` | Install packages | "Install the packages" | `npm install express` |
| `npm run dev` | Run dev server | "Start the server" | `npm run dev` |
| `npm run build` | Build | "Build it" | `npm run build` |
| `npm test` | Run tests | "Run the tests" | `npm test` |
| `npm init` | Initialize project | "Initialize npm" | `npm init -y` |
| `npx` | Execute package | "Run create-react-app" | `npx create-react-app my-app` |
| `npm uninstall` | Remove package | "Remove lodash" | `npm uninstall lodash` |

### Git Commands (4)

| Command | Function | Natural Language Request | Example |
|---------|----------|--------------------------|---------|
| `git status` | Check status | "Show git status" | `git status` |
| `git add` | Stage changes | "Add the changes" | `git add .` |
| `git commit` | Commit | "Commit" | `git commit -m "message"` |
| `git push` | Push | "Push" | `git push origin main` |

### Other Useful Commands (2)

| Command | Function | Natural Language Request | Example |
|---------|----------|--------------------------|---------|
| `cat` | View file contents | "Show me the file contents" | `cat config.json` |
| `grep` | Search text | "Find error" | `grep -r "error" src/` |

> 🔥 **Pro Tip**
>
> Don't try to memorize commands.
> Just ask Claude "tell me common npm commands."
> Learn naturally by asking when needed.

### Understanding Command Options

The `-` or `--` after commands are options.

```bash
ls -l        # Detailed view
ls -a        # Include hidden files
ls -la       # Both
npm install --save-dev  # Install as dev dependency
```

> 💡 **Beginner Tip**
>
> If options confuse you, ask Claude.
> "What does --save-dev mean in npm install?"

---

## Understanding the npm Ecosystem

### What is npm?

npm (Node Package Manager) is a JavaScript package manager.

```
┌────────────────────────────────────────────┐
│           npm Ecosystem                    │
├────────────────────────────────────────────┤
│                                            │
│  📦 npm = LEGO Block Store                 │
│                                            │
│  - Use code (packages) that others made    │
│  - No need to build everything from scratch│
│  - 2+ million packages shared by developers│
│    worldwide                               │
│                                            │
│  Examples:                                 │
│  - express: Create web servers             │
│  - react: Build UIs                        │
│  - lodash: Utility functions               │
│  - axios: HTTP requests                    │
│                                            │
└────────────────────────────────────────────┘
```

### Simple Analogy: LEGO Blocks

```
Building from scratch:
Clay → Bricks → House (takes long)

Using npm:
Get LEGO blocks → Assemble → Done (fast)
```

### Understanding package.json

The core file of every Node.js project.

```json
{
  "name": "my-project",          // Project name
  "version": "1.0.0",            // Version
  "scripts": {                    // Runnable commands
    "dev": "vite",                // npm run dev
    "build": "vite build",        // npm run build
    "test": "vitest"              // npm test
  },
  "dependencies": {               // Packages needed to run
    "react": "^18.2.0",
    "express": "^4.18.0"
  },
  "devDependencies": {            // Packages needed only for development
    "typescript": "^5.0.0",
    "eslint": "^8.0.0"
  }
}
```

> 💡 **Beginner Tip**
>
> package.json is the "project description."
> - What packages are needed
> - What commands to run
> Everything is written here.

### Frequently Used npm Commands

| Command | Meaning | When to Use |
|---------|---------|-------------|
| `npm install` | Install all dependencies | When you first get a project |
| `npm install <package>` | Install specific package | When adding new features |
| `npm run <script>` | Run script | Starting server, building, etc. |
| `npm init -y` | Create new project | When starting a new project |
| `npm update` | Update packages | When updating to latest versions |

### dependencies vs devDependencies

| Category | dependencies | devDependencies |
|----------|--------------|-----------------|
| Meaning | Needed to run | Needed only for development |
| Examples | react, express | typescript, eslint |
| Install | `npm install package` | `npm install -D package` |
| In production | Included | Excluded |

> 🔥 **Pro Tip**
>
> "Should this package go in dependencies or devDependencies?"
> If you're confused, ask Claude!

---

## Server Concepts and Local Development

### What is a Server?

```
┌────────────────────────────────────────────┐
│           Server Concept                   │
├────────────────────────────────────────────┤
│                                            │
│  Server = A program that receives requests │
│           and sends responses              │
│                                            │
│  [User] → Request → [Server] → Response →  │
│  [User]                                    │
│                                            │
│  Examples:                                 │
│  - Access Google → Google server sends     │
│    search page                             │
│  - Click video on YouTube → Server sends   │
│    the video                               │
│                                            │
└────────────────────────────────────────────┘
```

### Local Server vs Production Server

| Category | Local Server | Production Server |
|----------|--------------|-------------------|
| Location | My computer | Cloud (AWS, GCP, etc.) |
| Access | localhost:3000 | www.example.com |
| Purpose | Development, testing | Actual service |
| Security | Only I can access | Worldwide access |

### What is localhost?

```
localhost = My own computer

localhost:3000 = Port 3000 on my computer
```

Think of port numbers as "door numbers":
- Door 3000: Development server
- Door 8080: Another server
- Door 80: Regular web (http)
- Door 443: Secure web (https)

> 💡 **Beginner Tip**
>
> Accessing `localhost:3000` is like
> "knocking on door 3000 of your computer."
> If the server is running, you get a response.
> If it's off, you can't connect.

### Running a Development Server

```bash
# Start dev server (defined in package.json scripts)
npm run dev

# When server starts, you typically see:
#   ➜  Local:   http://localhost:3000/
#   ➜  Network: http://192.168.1.100:3000/
```

**Stopping the server:**
- Press `Ctrl + C`
- Closing the terminal also stops the server

---

## 🔨 Follow Along

### Exercise 1: Check Your Environment

```
> What version of node do I have?
```

**Expected result:** Claude runs `node --version` and shows the version

```
> Is npm installed?
```

**Expected result:** Claude checks and tells you

### Exercise 2: Create and Run a Simple Project

```
> Create a folder called test-server
```

```
> Initialize npm in that folder
```

```
> Install express
```

```
> Create index.js. Make it a simple web server that shows "Hello World".
```

```
> Run the server
```

**Expected result:** Server runs, accessible at `http://localhost:3000` in browser

### Exercise 3: Handle Errors

If you get an error:
```
> I just got an error. What went wrong?
```

**Expected result:** Claude analyzes the error cause and suggests solutions

### Exercise 4: Basic Git Usage

```
> Create a git repository
```

```
> Commit the current changes
```

**Expected result:** git init, git add, git commit run in sequence

### Exercise 5: Explore Packages

```
> Recommend a good package for handling dates
```

```
> Install dayjs
```

```
> Show me a usage example for dayjs
```

---

## 🖼️ Expected Screens

### Command Execution Approval

```
Claude wants to run command:
npm install express

Allow this action? [y/n/a]
```

### Server Running Result

```
> Run the server

Running: node index.js

Server running at http://localhost:3000

The server has started! Open http://localhost:3000 in your browser.
```

### package.json Creation

```
> Initialize npm

Running: npm init -y

Wrote to /Users/user/test-server/package.json:

{
  "name": "test-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

package.json has been created.
```

---

## 🎯 Mini Quiz

### Q1. What does npm stand for?
<details>
<summary>Show Answer</summary>

Node Package Manager.
It's a tool for managing JavaScript packages.
</details>

### Q2. What's the difference between `npm install` and `npm run dev`?
<details>
<summary>Show Answer</summary>

- `npm install`: Installs packages
- `npm run dev`: Runs the dev command defined in package.json scripts
</details>

### Q3. What does localhost:3000 mean?
<details>
<summary>Show Answer</summary>

Port 3000 on my computer (localhost).
It's the address for accessing the development server.
</details>

### Q4. What does Ctrl+C do in the terminal?
<details>
<summary>Show Answer</summary>

Stops the currently running process (e.g., server).
</details>

### Q5. What's the difference between dependencies and devDependencies?
<details>
<summary>Show Answer</summary>

- dependencies: Packages needed to run (e.g., react, express)
- devDependencies: Packages needed only for development (e.g., eslint, typescript)
</details>

---

## 📝 Practice Exercises

### Difficulty 1: Basic (Required)

1. Check `node --version` in terminal
2. Create a new folder and run `npm init -y`
3. Check the contents of `package.json`

### Difficulty 2: Intermediate

1. Install Express
2. Create a simple web server
3. Check it in the browser
4. Stop the server

### Difficulty 3: Challenge

1. Create an API server with multiple routes
2. Add test scripts
3. Manage versions with git

---

## 🏆 Challenge Tasks

1. **Package exploration**: Find and install 5 useful packages from npm
2. **Script writing**: Add custom scripts to package.json
3. **Deploy preparation**: Create a production build with `npm run build`

---

## Mini Project: Your Own API Server ⭐

Extend the Hello World server to create a simple API.

### 🟢 Easy: Basic API

```
> Add an API that responds with { "message": "Hello!" } when accessing GET /api/hello
```

### 🟡 Medium: Multiple APIs

```
> Add an API that returns the current time for GET /api/time
> Add an API that returns a random number between 1-100 for GET /api/random
```

### 🔴 Challenge: Parameter Handling

```
> Make GET /api/greeting?name=John respond with "Hello, John!"
```

---

## 🚨 Common Errors and Solutions

### Error 1: "command not found"

**What you see:**
```
bash: node: command not found
```

**Cause:** The program is not installed

**Solution:**
```
> Install node
```
Or install directly from [nodejs.org](https://nodejs.org)

### Error 2: "permission denied"

**What you see:**
```
Error: EACCES: permission denied
```

**Cause:** Permission issue

**Solution (Mac/Linux):**
```
> Run it again with sudo
```

**Solution (Windows):**
```
Run PowerShell as administrator
```

### Error 3: "ENOENT: no such file or directory"

**What you see:**
```
Error: ENOENT: no such file or directory
```

**Cause:** File or folder doesn't exist

**Solution:**
```
> Where am I right now?
> What's in this folder?
```

### Error 4: "port already in use"

**What you see:**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**Cause:** Another program is using the same port

**Solution:**
```
> Find and kill the process using port 3000
```

Or use a different port:
```
> Start the server on port 8080
```

### Error 5: "Cannot find module"

**What you see:**
```
Error: Cannot find module 'express'
```

**Cause:** Package is not installed

**Solution:**
```
> Run npm install
```

Or:
```
> Install express
```

### Error 6: "npm ERR! code E404"

**What you see:**
```
npm ERR! code E404
npm ERR! 404 Not Found
```

**Cause:** Package name is wrong

**Solution:**
```
> Check the package name. Is it react?
```

### Error 7: "EPERM: operation not permitted"

**What you see:**
```
npm ERR! code EPERM
npm ERR! syscall unlink
```

**Cause:** File is being used by another program

**Solution:**
- Close editors like VS Code
- Delete node_modules folder and reinstall:
```
> Delete node_modules and run npm install again
```

### Error 8: "npm WARN deprecated"

**What you see:**
```
npm WARN deprecated package@1.0.0: this package is deprecated
```

**Meaning:** Just a warning, not an error

**Solution:**
- Can usually be ignored
- If there's a newer alternative:
```
> What should I use instead of this deprecated package?
```

### Error 9: "SyntaxError"

**What you see:**
```
SyntaxError: Unexpected token '}'
```

**Cause:** Code syntax error

**Solution:**
```
> Fix this error. It says SyntaxError.
```

### Error 10: "npm run: command not found"

**What you see:**
```
npm ERR! missing script: start
```

**Cause:** That script doesn't exist in package.json

**Solution:**
```
> What scripts are in package.json?
```

Then use the correct script name

---

## ❌ Common Mistakes

### 1. Running Commands in the Wrong Folder
```
> Where am I now?
```
Always check your location.

> ⚠️ **Warning**
>
> `npm install` must be run in the folder with package.json.
> Running it in a different folder creates a new package.json.

### 2. Not Waiting for Commands to Finish
Commands like `npm install` take time. Wait until you see the completion message.

```
In progress:
[##########........] 50%

Complete:
added 52 packages in 3s
```

### 3. Forgetting to Install Dependencies
When you first open a project:
```
> npm install
```
must be run first.

> 💡 **Beginner Tip**
>
> Always run `npm install` first when you get a new project!
> Otherwise you'll get "module not found" errors.

### 4. Closing Terminal While Server is Running
The server needs the terminal to stay open to keep running.

```
Wrong way:
Start server → Close terminal → Server stops

Right way:
Start server → Keep terminal open → Work in another terminal tab
```

### 5. Ignoring Error Messages
Copy the error message and show it to Claude. It contains useful information.

```
> Look at this error message:
> Error: Cannot find module 'lodash'
```

### 6. Overusing sudo
```
> sudo npm install  (not recommended)
```
→ If you have permission issues, find other solutions first.

### 7. Committing node_modules
```
Add node_modules to .gitignore!
```
→ It's large and can be regenerated with npm install.

---

## 🆘 If It Doesn't Work?

| Symptom | Solution |
|---------|----------|
| "command not found" | Program needs to be installed |
| "permission denied" | Use `sudo` or check permissions |
| "ENOENT" | Check file/folder path |
| "port already in use" | Kill other process or change port |
| Server won't start | Check error message, verify dependencies installed |
| Not showing in browser | Check if server is running, try Ctrl+F5 to refresh |
| npm install fails | Check internet connection, delete node_modules and retry |

---

## 💡 How It's Used in the Real World

### Starting a Project

```bash
# 1. Clone repository
git clone https://github.com/example/project.git

# 2. Change directory
cd project

# 3. Install dependencies
npm install

# 4. Start dev server
npm run dev
```

### Daily Development Flow

```bash
# Morning: Get latest code
git pull

# Start dev server
npm run dev

# Develop new features...

# Test
npm test

# Commit
git add .
git commit -m "Add new feature"
git push
```

### Pre-deployment Checks

```bash
# Lint check
npm run lint

# Test
npm test

# Build
npm run build

# Preview build result
npm run preview
```

---

## ✅ Checklist

Check before finishing your learning:

- [ ] I can ask Claude to execute commands
- [ ] I know what the `!` prefix does
- [ ] I know the difference between npm install and npm run
- [ ] I know what package.json is
- [ ] I can create and run a simple server
- [ ] I can ask Claude for help when I get errors
- [ ] I know what localhost is

---

## 🔑 Key Terms

| Term | Description |
|------|-------------|
| **npm** | Node Package Manager. A package management tool |
| **Package** | A reusable bundle of code |
| **package.json** | Project configuration file |
| **dependencies** | List of packages needed to run |
| **devDependencies** | List of packages needed only for development |
| **Server** | A program that receives requests and sends responses |
| **localhost** | Address that refers to your own computer |
| **Port** | Door number for network communication |

Also refer to the [full glossary](../GLOSSARY.md).

---

## 📚 Learn More

**Official Documentation:**
- [npm Official Documentation](https://docs.npmjs.com/) - Complete npm guide
- [Node.js Official Documentation](https://nodejs.org/docs/latest/api/) - Node.js API documentation

**Video Resources:**
- [npm Tutorial for Beginners (YouTube)](https://www.youtube.com/results?search_query=npm+tutorial+beginners) - npm basics tutorial
- [Express.js Crash Course (YouTube)](https://www.youtube.com/results?search_query=express+js+crash+course) - Express quick start
- [Git Tutorial (YouTube)](https://www.youtube.com/results?search_query=git+tutorial+beginners) - Git basics

**Reading Materials:**
- [JavaScript.info](https://javascript.info/) - JavaScript tutorial
- [Express.js Official Guide](https://expressjs.com/) - Getting started with Express

**Useful Tools:**
- [npm Package Search](https://www.npmjs.com/) - Find npm packages
- [npx Guide](https://www.npmjs.com/package/npx) - Run packages directly

---

## 🎉 Part 1 Complete!

Congratulations! You've completed Part 1 (Getting Started).

**What you've learned:**
- AI coding and vibecoding concepts
- Claude Code installation and login
- Permission modes and basic conversation
- File creation, modification, and reference
- Terminal command execution
- npm ecosystem and server concepts

**What you can do now:**
- Create files while chatting with Claude
- Run simple web servers
- Request error resolution
- Install and use packages

In the next Part, we'll learn Claude Code's core features in depth!

---

## ➡️ Next Steps

In the next chapter, you'll learn how to understand and analyze project structure.

**Next chapter preview:**
- Analyzing project structure
- Understanding dependency relationships
- Codebase exploration strategies

Proceed to [Chapter 06: Understanding Project Structure](../Chapter06-Project-Structure/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
