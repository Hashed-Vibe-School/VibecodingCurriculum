# Chapter 15: Building CLI Tools

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Structure of Node.js CLI tools
- Handling user input
- File system operations
- Publishing to npm

---

## Why CLI Tools?

CLI (Command Line Interface) tools are essential for developer productivity:
- Automating repetitive tasks
- Project scaffolding
- Batch file processing
- Improving development workflows

The tools we use daily—`git`, `npm`, `npx`—are all CLI tools.

**CLI tool request tips:**

```
> Create a CLI tool that automatically generates project folder structure.
> When the user enters a project name, create basic folders (src, tests, docs)
> and initialize package.json.
```

Clearly describe inputs (arguments, options) and outputs (what actions to perform).

---

## Project: Building a File Organizer

Is your downloads folder a mess? Let's build a CLI tool that automatically organizes files.

### Goal

```bash
# Run this command
$ organize ./downloads

# Files get automatically sorted
# downloads/
#   images/  → .jpg, .png, .gif
#   docs/    → .pdf, .docx, .txt
#   videos/  → .mp4, .mov
#   others/  → everything else
```

### Step 1: Start the Project

```
> Create a Node.js CLI project.
> Name it file-organizer.
> Use commander library for argument parsing,
> and chalk for colored output.
```

Structure Claude creates:

```
file-organizer/
├── package.json
├── bin/
│   └── organize.js    # CLI entry point
└── src/
    └── organizer.js   # Core logic
```

### Step 2: Create CLI Entry Point

```javascript
#!/usr/bin/env node
// bin/organize.js

const { program } = require('commander')
const { organizeFiles } = require('../src/organizer')

program
  .name('organize')
  .description('Automatically organize files by type')
  .argument('<directory>', 'Directory to organize')
  .option('-d, --dry-run', 'Preview without actually moving')
  .option('-v, --verbose', 'Show detailed logs')
  .action((directory, options) => {
    organizeFiles(directory, options)
  })

program.parse()
```

**Key Concept: Shebang**

`#!/usr/bin/env node` tells the system to run this file with Node.js. This allows direct execution with `./organize.js`.

### Step 3: Implement Core Logic

```
> Implement file organizing logic in organizer.js.
> Categorize by extension,
> create folders if they don't exist,
> and move files.
> If dry-run option is set, just show what would happen without moving.
```

```javascript
// src/organizer.js
const fs = require('fs')
const path = require('path')
const chalk = require('chalk')

const CATEGORIES = {
  images: ['.jpg', '.jpeg', '.png', '.gif', '.webp', '.svg'],
  docs: ['.pdf', '.doc', '.docx', '.txt', '.md', '.xlsx'],
  videos: ['.mp4', '.mov', '.avi', '.mkv'],
  audio: ['.mp3', '.wav', '.flac'],
  archives: ['.zip', '.rar', '.7z', '.tar', '.gz']
}

function getCategory(filename) {
  const ext = path.extname(filename).toLowerCase()
  for (const [category, extensions] of Object.entries(CATEGORIES)) {
    if (extensions.includes(ext)) return category
  }
  return 'others'
}

function organizeFiles(directory, options) {
  const fullPath = path.resolve(directory)

  if (!fs.existsSync(fullPath)) {
    console.log(chalk.red(`❌ Directory not found: ${fullPath}`))
    process.exit(1)
  }

  const files = fs.readdirSync(fullPath)
  const results = { moved: 0, skipped: 0 }

  files.forEach(file => {
    const filePath = path.join(fullPath, file)

    // Skip directories
    if (fs.statSync(filePath).isDirectory()) {
      results.skipped++
      return
    }

    const category = getCategory(file)
    const targetDir = path.join(fullPath, category)
    const targetPath = path.join(targetDir, file)

    if (options.dryRun) {
      console.log(chalk.yellow(`[Preview] ${file} → ${category}/`))
    } else {
      if (!fs.existsSync(targetDir)) {
        fs.mkdirSync(targetDir, { recursive: true })
      }
      fs.renameSync(filePath, targetPath)
      if (options.verbose) {
        console.log(chalk.green(`✓ ${file} → ${category}/`))
      }
      results.moved++
    }
  })

  console.log(chalk.cyan(`\nDone: ${results.moved} moved, ${results.skipped} skipped`))
}

module.exports = { organizeFiles }
```

### Step 4: Test Locally

```bash
# Link the project globally
npm link

# Now usable from anywhere
organize ./downloads --dry-run
organize ./downloads --verbose
```

---

## Extending Features

### Configuration File Support

```
> Read a .organizerc config file for custom rules.
> Let users define their own categories and extension mappings.
```

```json
// .organizerc
{
  "categories": {
    "code": [".js", ".ts", ".py", ".go"],
    "design": [".psd", ".ai", ".sketch", ".figma"]
  },
  "ignore": ["node_modules", ".git"]
}
```

### Undo Feature

```
> Record original locations before moving files,
> and allow organize --undo to revert.
```

```javascript
// Save move history
const history = []
history.push({ from: filePath, to: targetPath })
fs.writeFileSync('.organize-history.json', JSON.stringify(history))

// Undo
function undoLastOrganize() {
  const history = JSON.parse(fs.readFileSync('.organize-history.json'))
  history.forEach(({ from, to }) => {
    fs.renameSync(to, from)
  })
}
```

### Interactive Mode

```
> Add interactive mode using inquirer.
> Ask where to move each file.
```

```javascript
const inquirer = require('inquirer')

async function interactiveMode(files) {
  for (const file of files) {
    const { action } = await inquirer.prompt([
      {
        type: 'list',
        name: 'action',
        message: `Where should ${file} go?`,
        choices: ['images', 'docs', 'videos', 'skip', 'delete']
      }
    ])
    // Handle based on selection
  }
}
```

---

## Second Project: Project Generator

Let's build a tool that scaffolds projects, like create-react-app.

### Goal

```bash
$ create-my-app my-project
? Select project type: (React / Node API / CLI Tool)
? Use TypeScript? (Y/n)
? Test framework: (Jest / Vitest / None)

✓ Project created!
```

### Implementation

```
> Create a project scaffolding CLI called create-my-app.
> Get user input with inquirer,
> copy template files based on selection,
> and add necessary packages to package.json.
```

```javascript
#!/usr/bin/env node

const inquirer = require('inquirer')
const fs = require('fs-extra')
const path = require('path')
const chalk = require('chalk')
const { execSync } = require('child_process')

async function main() {
  const answers = await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: 'Project name:',
      default: 'my-app'
    },
    {
      type: 'list',
      name: 'template',
      message: 'Project type:',
      choices: ['React', 'Node API', 'CLI Tool']
    },
    {
      type: 'confirm',
      name: 'typescript',
      message: 'Use TypeScript?',
      default: true
    },
    {
      type: 'list',
      name: 'testFramework',
      message: 'Test framework:',
      choices: ['Jest', 'Vitest', 'None']
    }
  ])

  const projectPath = path.join(process.cwd(), answers.name)

  console.log(chalk.cyan('\n📁 Creating project...\n'))

  // 1. Create folder
  fs.ensureDirSync(projectPath)

  // 2. Copy template
  const templatePath = path.join(__dirname, '../templates', answers.template.toLowerCase())
  fs.copySync(templatePath, projectPath)

  // 3. Modify package.json
  const pkgPath = path.join(projectPath, 'package.json')
  const pkg = require(pkgPath)
  pkg.name = answers.name

  if (answers.typescript) {
    pkg.devDependencies.typescript = '^5.0.0'
  }

  fs.writeJsonSync(pkgPath, pkg, { spaces: 2 })

  // 4. Install dependencies
  console.log(chalk.yellow('📦 Installing packages...'))
  execSync('npm install', { cwd: projectPath, stdio: 'inherit' })

  console.log(chalk.green(`\n✅ ${answers.name} project created!`))
  console.log(chalk.gray(`\n  cd ${answers.name}\n  npm run dev\n`))
}

main().catch(console.error)
```

---

## Publishing to npm

Share your CLI tool with the world.

### package.json Setup

```json
{
  "name": "file-organizer-cli",
  "version": "1.0.0",
  "description": "CLI tool that automatically organizes files",
  "bin": {
    "organize": "./bin/organize.js"
  },
  "keywords": ["cli", "file", "organizer", "automation"],
  "author": "your-name",
  "license": "MIT"
}
```

The **bin field** is key. The `organize` command runs `./bin/organize.js`.

### Publishing

```bash
# Log in to npm
npm login

# Publish
npm publish

# Now anyone can install
npm install -g file-organizer-cli
```

---

## CLI Tool Design Principles

### 1. Clear Error Messages

```javascript
// ❌ Bad
console.log('Error')
process.exit(1)

// ✅ Good
console.log(chalk.red('❌ Directory not found: ' + path))
console.log(chalk.gray('Hint: Check your relative or absolute path'))
process.exit(1)
```

### 2. Auto-generated --help

commander handles this automatically:

```bash
$ organize --help
Usage: organize [options] <directory>

Automatically organize files by type

Options:
  -d, --dry-run  Preview without actually moving
  -v, --verbose  Show detailed logs
  -h, --help     display help for command
```

### 3. Progress Indicators

```
> Show a progress bar when processing many files. Use cli-progress.
```

```javascript
const cliProgress = require('cli-progress')

const bar = new cliProgress.SingleBar({}, cliProgress.Presets.shades_classic)
bar.start(files.length, 0)

files.forEach((file, index) => {
  // Process...
  bar.update(index + 1)
})

bar.stop()
```

---

## Useful Libraries

| Library | Purpose |
|---------|---------|
| `commander` | Argument/option parsing |
| `inquirer` | Interactive prompts |
| `chalk` | Colored output |
| `ora` | Spinner animations |
| `cli-progress` | Progress bars |
| `fs-extra` | Enhanced file system |
| `glob` | File pattern matching |
| `boxen` | Box UI |

---

## Practice

### Basic Task

```
> Create a CLI tool that analyzes git commit messages and shows statistics.
> - Count by commit type (feat, fix, docs, etc.)
> - Most active day/time
> - Commits per contributor
```

### Advanced Challenges

```
> Tool that merges markdown files into PDF or HTML documentation

> Tool that analyzes package.json across multiple projects
> to find common dependencies and version conflicts

> Tool that batch resizes/compresses images
```

---

## Summary

What you learned in this chapter:
- [x] Basic structure of CLI tools
- [x] Argument and option handling
- [x] File system operations
- [x] Interactive prompts
- [x] npm publishing

CLI tools greatly boost developer productivity. If you have repetitive tasks, automate them with a CLI.

In the next chapter, we'll build Discord/Slack bots.

Proceed to [Chapter 16: Building Chatbots](../Chapter16/README.md).
