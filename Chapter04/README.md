# Chapter 04: Working with Files

**English** | [한국어](./README.ko.md)

## What You'll Learn

- Referencing files with Claude (@-mention)
- File creation and modification
- Processing multiple files simultaneously

---

## Why Do You Need This?

Files are everything in coding. Every website, app, or tool you build is made of files. Learning to work with files is like learning to use ingredients in cooking--you can't make anything without them.

**Real-world scenarios:**
- You want to create a website (need to make HTML, CSS files)
- You want to update an existing document
- You want to organize messy project files
- You want Claude to understand your project structure

### Simple Analogy: Filing Cabinet

Think of your computer as a giant filing cabinet. Each drawer is a folder, and each paper inside is a file.

When you say `@report.txt`, you're pointing to a specific paper and saying "look at this one." When you say `@documents/`, you're pointing to a whole drawer and saying "check everything in here."

The `@` symbol is your finger pointing at what you want Claude to see.

---

## Referencing Files with @

In Claude Code, `@` references files.

### Basic Usage

```
> @index.html Explain this file
```

```
> @src/app.js Find bugs in this code
```

```
> @package.json List the libraries used here
```

### Auto-complete

Typing `@` followed by a filename displays a list automatically. Use arrows to select and press Enter.

### Folder References

```
> @src/ Explain this folder structure
```

```
> @components/ What do these files do?
```

---

## File Creation

Request file creation from Claude.

### Examples

```
> Create a file called hello.txt. Write "Hello World" inside.
```

```
> Create an index.html file. Make it a simple web page.
```

### Approval Process

In Normal mode, Claude requests confirmation before creating files:

```
Claude wants to create hello.txt
---
Hello World
---
[Allow] [Deny]
```

Selecting `Allow` creates the file.

### Creating with Folders

```
> Create src/components/Button.js file. Make it a React button component.
```

If the folder does not exist, Claude creates it as well.

---

## File Modification

Existing files can be modified.

### Examples

```
> In @index.html, change the title to "My Website"
```

```
> Add a console.log to @app.js
```

### Reviewing Changes

Claude displays changes before editing:

```diff
- <h1>Hello World</h1>
+ <h1>My Website</h1>
```

Red (-) indicates removal, green (+) indicates addition.

---

## Viewing File Contents

```
> @config.json Show me the contents
```

```
> Read @README.md
```

Claude reads and displays file contents.

**For long files:**

```
> @README.md Summarize the key points
> @config.json Find only the port setting
```

Be specific about what you need to find it faster.

---

## Processing Multiple Files

### Referencing Multiple Files

```
> Look at @index.html and @style.css and check if styles are applied correctly
```

```
> Compare @src/api.js and @src/utils.js and find duplicate code
```

### Finding Files by Pattern

```
> Find all .js files in the src folder
```

```
> Show me all test files
```

---

## Special Prefixes

Beyond `@`, other useful prefixes exist.

| Prefix | Function | Example |
|--------|----------|---------|
| `@` | Reference file/folder | `@src/app.js` |
| `!` | Run command and show result | `!ls` |
| `#` | Save to Claude's memory | `# Always respond in English` |

### ! Examples

```
> !ls
```
Displays the current folder's file list; Claude recognizes the content.

```
> !cat package.json
```
Outputs file contents to terminal; Claude recognizes the content.

### # Examples

```
> # This project uses TypeScript
```
Claude remembers this rule and asks where to save it.

---

## Practice

### 1. File Creation

```
> Create my-first-page.html.
> Title should be "My First Web Page", put a self-introduction in the body.
```

### 2. File Modification

```
> @my-first-page.html Change the background color to sky blue
```

### 3. Multiple File Creation

```
> Create a my-project folder and make index.html, style.css, script.js inside.
> Make it a simple "Hello World" web page.
```

### 4. File Comparison

```
> Compare the structure of @index.html and @about.html
```

---

## Common Mistakes

### File Not Found

```
> @nonexistent.txt Show me
```
Verify the filename is correct. Using auto-complete reduces errors.

### Incorrect Path

```
> @app.js Show me
```
The file may be in another folder. Try including the path: `@src/app.js`.

### Approval Not Given

If a file was not created, verify `Allow` was selected on the approval request.

---

## Mini Project: Personal Introduction Page

Create a personal introduction page using concepts from this chapter.

### Goals

- Learn HTML/CSS basics
- Practice file creation and modification

### Creation

```
> Create a personal introduction page.
> - Name and profile picture placeholder
> - Short introduction paragraph
> - List of favorite things
> - Applied styling
```

### Improvement

```
> Change the background to pastel colors
```
Practice CSS color systems (`hex`, `rgb`, `hsl`)

```
> Make the fonts more elegant
```
Understand web fonts and `font-family` stacks

```
> Add hover effects that change colors
```
CSS pseudo-classes for interaction

### Challenge

```
> Make a dark mode version too
```
CSS variable-based theming - essential for modern web

```
> Split into multiple pages (index.html, about.html, contact.html)
```
Multi-page architecture and navigation design

---

## Try It Yourself

Let's practice file operations step by step:

### Exercise 1: Create Your First File
```
> Create a file called my-notes.txt with "Learning Claude Code is fun!" inside
```
After Claude creates it, verify by asking:
```
> @my-notes.txt Show me what's inside
```

### Exercise 2: Modify a File
```
> @my-notes.txt Add a new line that says "I created this with AI!"
```
Watch how Claude shows you the changes before applying them.

### Exercise 3: Create Multiple Files at Once
```
> Create a folder called my-website with index.html, style.css, and script.js inside.
> Make it a simple page that says "Welcome to My Site"
```

### Exercise 4: Explore Files
```
> What files are in this folder?
> @my-website/ Explain the structure
```

---

## If It Doesn't Work...

**"File not found" error?**
- Check if you spelled the filename correctly
- Use the autocomplete feature (type `@` and start typing)
- The file might be in a different folder - try including the path: `@folder/file.txt`

**File created in wrong location?**
- Specify the full path: "Create `my-folder/file.txt`"
- Check your current working directory: "Where am I?"

**Changes not showing up?**
- Make sure you approved the change (clicked Allow)
- Ask Claude to show the file contents again

**Claude can't see a file you know exists?**
- The file might be in a hidden folder (starting with `.`)
- Try using the `!ls -la` command to see all files

**Created the wrong file?**
- Ask Claude to delete it: "Delete the file you just created"
- Or manually delete it from your file explorer

---

## Summary

Covered in this chapter:
- [x] Referencing files with `@`
- [x] File creation
- [x] File modification
- [x] Processing multiple files
- [x] Special prefixes (`@`, `!`, `#`)

The next chapter covers terminal commands.

Proceed to [Chapter 05: Terminal Commands](../Chapter05/README.md).
