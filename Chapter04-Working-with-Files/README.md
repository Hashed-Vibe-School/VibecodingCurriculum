# Chapter 04: Reading and Writing Files

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Learn to reference files with `@`
- Create and modify files
- Handle multiple files at once
- Use special prefixes `@`, `!`, `#`
- Understand file system basics

---

## ⏱️ Estimated Time

- Reading: **25 minutes**
- Hands-on: **30 minutes**

---

## 📋 What You Need

- Claude Code installation and login complete (Chapter 02)
- Understanding of permission modes (Chapter 03)

---

## 🔗 Previous Chapter Review

In [Chapter 03](../Chapter03-First-Conversation/README.md), we learned about permission modes (Plan, Normal, Accept Edits) and effective conversation methods. Now it's time to learn how to actually work with files.

---

## Why Is This Necessary?

Files are everything in coding. Websites, apps, tools -- everything you create is made of files. Learning to work with files is like learning to handle ingredients in cooking -- you can't make anything without ingredients.

**Real situations:**
- Want to make a website (need HTML, CSS files)
- Want to modify an existing document
- Want to organize messy project files
- Want Claude to understand project structure

### Easy Analogy: File Cabinet

Think of your computer as a giant file cabinet. Each drawer is a folder, and each paper inside is a file.

When you say `@report.txt`, you're pointing at a specific paper saying "look at this." When you say `@documents/`, you're pointing at an entire drawer saying "check everything in here."

The `@` symbol is your finger pointing at what Claude should look at.

---

## File System Basics

Let's learn the basic concepts before working with files.

### Files and Folders

```
┌──────────────────────────────────────────┐
│           File System Structure           │
├──────────────────────────────────────────┤
│                                          │
│  📁 my-project/                          │
│  ├── 📄 index.html     ← file            │
│  ├── 📄 style.css      ← file            │
│  ├── 📁 src/           ← folder (subfolder) │
│  │   ├── 📄 app.js                       │
│  │   └── 📄 utils.js                     │
│  └── 📁 images/        ← folder          │
│      ├── 📄 logo.png                     │
│      └── 📄 banner.jpg                   │
│                                          │
└──────────────────────────────────────────┘
```

### Path

An address that indicates the location of a file.

| Path Type | Example | Description |
|-----------|---------|-------------|
| **Absolute path** | `/Users/johndoe/my-project/index.html` | Full path from root |
| **Relative path** | `src/app.js` | Path from current location |
| **Parent folder** | `../other-folder/file.txt` | `..` means parent folder |

> 💡 **Beginner Tip**
>
> A path is like a "home address."
> - Absolute path: "123 Main Street, New York, NY" (full address)
> - Relative path: "The building next door, 3rd floor" (relative to current location)

### File Extensions

The `.xxx` after a file name indicates the file type.

| Extension | Purpose | Example |
|-----------|---------|---------|
| `.html` | Webpage structure | `index.html` |
| `.css` | Webpage styling | `style.css` |
| `.js` | JavaScript code | `app.js` |
| `.json` | Data/configuration | `package.json` |
| `.md` | Markdown document | `README.md` |
| `.txt` | Plain text | `notes.txt` |
| `.py` | Python code | `main.py` |

> 🔥 **Pro Tip**
>
> Looking at extensions tells you the file's role.
> When you ask Claude "Explain this project," it looks at extensions to determine what technologies are used.

---

## Referencing Files with @

In Claude Code, you can reference files using `@`.

### Basic Usage

```
> @index.html Explain this file
```

```
> @src/app.js Find bugs in this code
```

```
> @package.json Tell me the library list used here
```

### Auto-complete

When you type `@` and start typing a file name, a list appears automatically. Select with arrow keys and press Enter.

```
> @in (auto-complete list appears)
  ├── index.html
  ├── index.js
  └── init.py
```

> 💡 **Beginner Tip**
>
> Type `@` and any letters to try. When the auto-complete list appears, just pick from there.
> No need to memorize file names!

### Folder Reference

```
> @src/ Explain this folder structure
```

```
> @components/ What do the files here do?
```

Adding `/` at the end of a folder path references the entire folder.

### Various @ Usage Examples

**File analysis:**
```
> @README.md Explain what this project does
```

**Code review:**
```
> @src/auth.js Review for security issues
```

**Error debugging:**
```
> @app.js Fix the error on line 23
```

**Comparison analysis:**
```
> @old-version.js and @new-version.js What's different?
```

**Configuration check:**
```
> @tsconfig.json Check if this configuration is correct
```

---

## Creating Files

Request Claude to create files.

### Basic Examples

```
> Create hello.txt file. Write "Hello" inside.
```

```
> Create index.html file. As a simple webpage.
```

### Approval Process

In Normal mode, Claude asks for confirmation before creating a file:

```
Claude wants to create hello.txt
───────────────────────────────────────
Hello
───────────────────────────────────────
[Allow] [Deny]
```

Select `Allow` and the file is created.

### Creating with Folders

```
> Create src/components/Button.js file. As a React button component.
```

If the folder doesn't exist, Claude creates the folder too.

**Expected conversation:**
```
> Create src/components/Button.jsx

Folder 'src/components' doesn't exist. Should I create it?

Creating folder: src/components/
Creating file: src/components/Button.jsx
───────────────────────────────────────
import React from 'react';

export default function Button({ children, onClick }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
───────────────────────────────────────

Allow this action? [y/n/a]
```

### Creating Multiple Files at Once

```
> Create my-website folder and inside it create index.html, style.css, script.js.
> A simple page that says "Welcome to my site".
```

**Expected result:**
```
✓ Created folder: my-website/
✓ Created file: my-website/index.html
✓ Created file: my-website/style.css
✓ Created file: my-website/script.js

Created 3 files.
Open index.html in your browser to check.
```

> 💡 **Beginner Tip**
>
> You can request complex structures all at once:
> "Create a basic React project structure"
> Claude will create all the necessary folders and files automatically.

---

## Modifying Files

You can modify existing files.

### Basic Examples

```
> @index.html Change the title to "My Website"
```

```
> @app.js Add a console.log
```

### Viewing Changes (Diff)

Claude shows you the changes before modifying:

```diff
- <h1>Hello World</h1>
+ <h1>My Website</h1>
```

Red (-) is the deleted part, green (+) is the added part.

### How to Read Diff

```
Editing: src/config.js

────────────────────────────────────────
 1 | const config = {
-2 |   port: 3000,
+2 |   port: 8080,
 3 |   host: 'localhost',
+4 |   debug: true,
 5 | };
────────────────────────────────────────

Allow this edit? [y/n/a]
```

| Symbol | Meaning | Color |
|--------|---------|-------|
| `-` | Line being deleted | Red |
| `+` | Line being added | Green |
| (none) | Unchanged line | White/Gray |

> 🔥 **Pro Tip**
>
> Get in the habit of reading diffs. Understanding what changes Claude is trying to make:
> 1. You can catch mistakes early
> 2. You gradually understand the code
> 3. Later you can make modifications yourself

### Partial Edit vs Complete Rewrite

**Partial edit (recommended):**
```
> @config.js Change port to 8080
```

**Complete rewrite:**
```
> @config.js Rewrite the entire file
```

> ⚠️ **Caution**
>
> "Complete rewrite" overwrites all existing content.
> Partial edits are safer and easier to track.

---

## Viewing File Contents

```
> @config.json Show me the contents
```

```
> @README.md Read this
```

Claude reads and displays file contents.

### Handling Long Files

**View entire file:**
```
> @long-file.js Show me the entire contents
```

**Request summary:**
```
> @README.md Summarize the key points only
```

**Only specific parts:**
```
> @config.json Find only the port setting
```

**Specify line range:**
```
> @app.js Show me lines 100-150
```

**Find function/class:**
```
> @utils.js Show me the formatDate function
```

> 💡 **Beginner Tip**
>
> Requesting specific information gets you faster results.
> "Find only the config part in this file" is more efficient than "Explain this entire file."

---

## Handling Multiple Files

### Referencing Multiple Files at Once

```
> @index.html and @style.css Check if styles are applied correctly
```

```
> @src/api.js and @src/utils.js Compare and find duplicate code
```

```
> @components/Header.js, @components/Footer.js, @components/Sidebar.js
> Modify these three files consistently
```

### Searching with File Patterns

```
> Find all .js files in src folder
```

```
> Show me the test files list
```

```
> Find CSS files that have color settings
```

### Analyzing Entire Project

```
> Analyze the entire project structure
```

```
> Tell me if there are any unused files
```

```
> Organize and show me the import relationships
```

---

## Special Prefixes

There are useful prefixes besides `@`.

| Prefix | Function | Example |
|--------|----------|---------|
| `@` | File/folder reference | `@src/app.js` |
| `!` | Execute command and show result | `!ls` |
| `#` | Save to Claude memory | `# Always respond in English` |

### @ - File Reference

We already learned this.

```
> @package.json Tell me the dependency list
```

### ! - Command Execution

```
> !ls
```
Shows the file list in the current folder, and Claude recognizes the contents.

```
> !npm run build
```
Runs the build command and shows Claude the result.

```
> !git status
```
Checks Git status and Claude analyzes it.

**Difference from normal request:**
| Method | Example | Difference |
|--------|---------|------------|
| Normal | "Show me file list" | Claude decides command, needs approval |
| `!` prefix | `!ls -la` | Runs specified command immediately, Claude recognizes result |

> 💡 **Beginner Tip**
>
> Use `!` when you know the command.
> If you don't know, just say "Show me the file list."
> Claude will run the appropriate command automatically.

### # - Save to Memory

```
> # This project uses TypeScript
```
Claude remembers this information and uses it in subsequent conversations.

```
> # Respond in English
```
All subsequent responses will be in English.

```
> # Add detailed comments to code
```
Code generated from now on will have detailed comments.

**# Usage examples:**
```
> # This project rules:
> # - Filenames in kebab-case (my-component.js)
> # - Function names in camelCase (myFunction)
> # - Component names in PascalCase (MyComponent)
> # - Comments in English

(Claude follows these rules from now on)
```

> 🔥 **Pro Tip**
>
> Telling project conventions with `#` generates consistent code.
> If you have team coding style, let Claude know at the start with `#`.

---

## Handling Large Files

### Strategies for Reading Large Files

**1. View table of contents first:**
```
> @large-file.js Show me only the file structure. Just the function list.
```

**2. Request only specific parts:**
```
> @large-file.js Show me only the UserService class
```

**3. Request summary:**
```
> @large-file.js Briefly summarize what this file does
```

### Strategies for Modifying Large Files

**1. Break into small units:**
```
> @app.js Modify only the first function
```

**2. Search then modify:**
```
> @app.js Find deprecated functions and replace with new versions
```

**3. Request refactoring:**
```
> @app.js This is too long. Split it into multiple files.
```

### File Splitting (Code Splitting)

```
> @utils.js is too big. Split by function.
> - Date related → date-utils.js
> - String related → string-utils.js
> - Array related → array-utils.js
```

**Expected result:**
```
✓ Created utils/date-utils.js
✓ Created utils/string-utils.js
✓ Created utils/array-utils.js
✓ Updated imports in 5 files
✓ Deleted utils.js

Refactoring complete. Updated all import paths too.
```

---

## Real Project File Management

### When Starting a Project

**1. Understand structure:**
```
> Explain this project structure
```

**2. Check key files:**
```
> Tell me the 5 most important files
```

**3. Understand config files:**
```
> @package.json How do I run this project?
```

### While Working

**1. Find related files:**
```
> Where are the login-related files?
```

**2. Track dependencies:**
```
> @Header.js What other files use this file?
```

**3. Understand impact scope:**
```
> @config.js If I change this file, what's affected?
```

### When Cleaning Up

**1. Find unused files:**
```
> Are there files that aren't imported anywhere?
```

**2. Find duplicate code:**
```
> Let me know if there's similar code in multiple files
```

**3. Improve structure:**
```
> Any improvements for this project folder structure?
```

---

## 🔨 Try It Yourself

### Exercise 1: Create First File

```
> Create my-notes.txt file. Write "Learning Claude Code is fun!" inside.
```

**Expected result:** File creation approval request → Select Allow → File created

Verify:
```
> @my-notes.txt Show me what's inside
```

### Exercise 2: Modify File

```
> @my-notes.txt Add a line saying "This was made with AI!"
```

**Expected result:** Changes displayed in diff format

### Exercise 3: Create Multiple Files at Once

```
> Create my-website folder and inside it create index.html, style.css, script.js.
> A simple page that says "Welcome to my site".
```

**Expected result:** Folder and 3 files created

### Exercise 4: Explore Files

```
> @my-website/ Explain the structure
```

**Expected result:** Explanation of files in folder and their roles

### Exercise 5: Using Special Prefixes

```
> !dir (Windows) or !ls (Mac/Linux)
```
Check current folder file list

```
> # Add English comments to code from now on
```
Code generated from now on will have English comments

---

## 🖼️ Expected Screens

### File Creation Approval Request

```
Claude wants to create file: my-notes.txt

────────────────────────────────────────
Learning Claude Code is fun!
────────────────────────────────────────

Allow this action? [y/n/a]
```

### File Modification Diff Display

```
Editing my-notes.txt:

  Learning Claude Code is fun!
+ This was made with AI!

Allow this edit? [y/n/a]
```

### Folder Structure Explanation

```
my-website/ folder structure:

├── index.html   # Main HTML file - defines page structure
├── style.css    # Stylesheet - handles design
└── script.js    # JavaScript - handles dynamic features

File relationships:
- index.html loads style.css and script.js
- Open index.html in browser to see the webpage
```

---

## 🎯 Mini Quiz

### Q1. What does @ symbol do?
<details>
<summary>Show Answer</summary>

Used to reference files or folders.
Example: `@index.html` points to the index.html file.
</details>

### Q2. What's the difference between ! and @?
<details>
<summary>Show Answer</summary>

- `@`: File/folder reference (e.g., `@src/app.js`)
- `!`: Terminal command execution (e.g., `!npm install`)
</details>

### Q3. What is # used for?
<details>
<summary>Show Answer</summary>

Saves information to Claude's memory.
Example: `# Respond in English` → Continues responding in English afterwards
</details>

### Q4. What do - and + mean in a diff?
<details>
<summary>Show Answer</summary>

- `-` (red): Line being deleted
- `+` (green): Line being added
</details>

### Q5. How do you reference a folder?
<details>
<summary>Show Answer</summary>

Add `/` at the end of the folder name.
Example: `@src/` → References entire src folder
</details>

---

## 📝 Practice Exercises

### Difficulty 1: Basic (Required)

1. Create `greeting.txt` file and write your name
2. Read file contents to verify
3. Add today's date to file contents

### Difficulty 2: Intermediate

1. Create `mini-project` folder
2. Create `index.html`, `style.css` inside
3. Link the CSS file in HTML
4. Check `@mini-project/` structure

### Difficulty 3: Challenge

1. Create 3+ related files
2. Set up import/link relationships between files
3. Modify one file and request automatic update of related files
4. Request analysis of entire project structure

---

## 🏆 Challenge Tasks

1. **Large-scale modification**: Try modifying 5+ files at once
2. **Refactoring**: Try splitting a large file into multiple smaller files
3. **Automation**: Set coding rules with `#` and generate consistent code

---

## Mini Project: Self-Introduction Webpage ⭐

Let's create a self-introduction page using what we learned in this chapter.

### 🟢 Easy: Basic Page

```
> Create a self-introduction page.
> - Name
> - One-line introduction
```

### 🟡 Medium: Add Style

```
> Create a self-introduction page.
> - Name and profile picture placeholder
> - Simple introduction paragraph
> - List of favorite things
> - Apply nice styling
```

### 🔴 Challenge: Dark Mode

```
> Add a dark mode toggle button to the self-introduction page
> When the button is pressed, change the color theme
```

---

## 🚨 Common Errors and Solutions

### Error 1: "File not found"

**Situation:** Said `@config.json show me` but it says doesn't exist

**Cause:** File is in a different folder, or name is different

**Solution:**
- Use auto-complete (type `@` then type)
- Include path: `@src/config.json`
- Ask "Where is the config file?"

**Conversation example:**
```
> @config.json Show me

File not found: config.json

> Where is the config file?

Found config-related files:
- src/config/app-config.json
- src/config/db-config.json

Which file would you like to see?
```

### Error 2: File Created in Wrong Place

**Situation:** Created a file but it's in unexpected location

**Solution:**
- Specify full path: `my-folder/file.txt create`
- Check current location: `Where am I now?`

```
> Where am I working right now?

Current working folder: /Users/johndoe/projects/my-app

> Create utils.js inside src folder

(src/utils.js created in correct location)
```

### Error 3: Changes Not Visible

**Situation:** Modified file but contents didn't change

**Cause:** Didn't approve (selected Deny or ignored)

**Solution:** Verify you selected `Allow` or `y`

### Error 4: Auto-complete Doesn't Work

**Situation:** Type `@` but file list doesn't appear

**Solution:**
- Wait a moment (might be loading)
- Type one more character
- Verify you started Claude Code in the project folder

### Error 5: File Too Large to Display

**Situation:** Requested large file contents but only shows partial

**Solution:**
```
> @large-file.js Show me only the first 100 lines
> @large-file.js Show me only the function list
> @large-file.js Summarize this
```

### Error 6: Wrong File Modified

**Situation:** A different file was modified

**Solution:**
```
> Cancel what you just did
```
Or undo with `Esc Esc`

---

## ❌ Common Mistakes

### 1. File Name Typos
```
> @indx.html show me  (indx instead of index)
```
→ Using auto-complete can reduce typos.

### 2. Omitting Path
```
> @app.js show me  (actually it's src/app.js)
```
→ Try including the folder path.

### 3. Not Approving
In Normal mode, you must approve file creation/modification.

> 💡 **Beginner Tip**
>
> If approval requests are annoying, switch to Accept Edits mode.
> However, Normal mode is recommended for important work.

### 4. Creating Wrong File
For accidentally created files:
```
> Delete the file I just made
```
Or delete directly in file explorer.

### 5. Modifying Without Backup
Before modifying important files:
```
> @config.js Back this up
```
Or using Git is recommended.

### 6. Requesting Entire Large Files
```
> @10000-line-file.js Show me everything  (takes a long time)
```
→ Request only the parts you need.

### 7. Missing Extension
```
> Create index  (what kind?)
> Create index.html  (clear)
```

---

## 🆘 If It's Not Working?

| Symptom | Solution |
|---------|----------|
| Can't find file | Use auto-complete, include path |
| File created in wrong place | Specify full path |
| Changes not visible | Verify Allow was selected |
| Can't find hidden files | Check with `!ls -la` command |
| File too large to open | Request partial or summary |
| Wrong modification | `Esc Esc` or "Cancel that" |

---

## 💡 How Professionals Use It

### Code Review Scenario

```
> @pull-request-files/ Review the files in this PR

Review results:

src/auth.js:
- ✅ Line 23: Proper error handling
- ⚠️ Line 45: Password logged in plain text - security risk
- 💡 Line 78: async/await pattern recommended

src/api.js:
- ✅ Generally clean
- ⚠️ Line 12: API key is hardcoded

Overall: Recommend fixing 2 security items before merge
```

### Legacy Code Analysis

```
> @old-system/ Analyze this legacy code. What needs to change to modernize it?

Analysis results:

1. jQuery → React/Vue migration recommended
2. var → let/const change needed (ES6+)
3. Callback hell → Promise/async-await conversion
4. Global variables → Introduce module system

Priority:
1. Security vulnerabilities (3 XSS risk spots)
2. Performance issues (unnecessary DOM manipulation)
3. Maintainability improvements
```

### Auto Documentation Generation

```
> @src/ Analyze everything and create README.md

README.md creation complete:

- Project introduction
- Installation instructions
- Usage examples
- API documentation
- Folder structure explanation
- Contribution guide
```

---

## ✅ Checklist

Before finishing your learning, confirm:

- [ ] I can reference files with `@`
- [ ] I can create files
- [ ] I can modify files
- [ ] I can reference multiple files at once
- [ ] I know the difference between special prefixes (`@`, `!`, `#`)
- [ ] I know how to read diffs
- [ ] I know what a path is

---

## 🔑 Key Terms

| Term | Description |
|------|-------------|
| **@ (at sign)** | Prefix for referencing files/folders |
| **! (exclamation)** | Prefix for executing commands |
| **# (hash)** | Prefix for saving to Claude memory |
| **Path** | Address indicating file location |
| **Diff** | Display comparing before/after file changes |
| **Extension** | Suffix indicating file type (.js, .html, etc.) |

Also refer to the [full glossary](../GLOSSARY.md).

---

## 📚 Learn More

**Official Documentation:**
- [File System Basics (MDN)](https://developer.mozilla.org/en/docs/Learn/Getting_started_with_the_web/Dealing_with_files) - Working with files and folders

**Video Resources:**
- [File System Basics (YouTube)](https://www.youtube.com/results?search_query=file+system+basics+tutorial) - File system basics
- [HTML CSS JS Basics (YouTube)](https://www.youtube.com/results?search_query=html+css+javascript+basics) - Web file basics

**Reading Materials:**
- [Git - The Basics](https://git-scm.com/book/en/v2) - Managing file versions with Git
- [VS Code File Navigation](https://code.visualstudio.com/docs/getstarted/tips-and-tricks) - Working with files in VS Code

**Related Tools:**
- [VS Code](https://code.visualstudio.com/) - Code editor
- [GitHub Desktop](https://desktop.github.com/) - Git GUI client

---

## ➡️ Next Steps

In the next chapter, we'll learn how to handle terminal commands.

**Next Chapter Preview:**
- History and importance of terminals
- Essential commands to know
- npm and package ecosystem
- Running a local server

Proceed to [Chapter 05: Terminal Commands](../Chapter05-Terminal-Commands/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
