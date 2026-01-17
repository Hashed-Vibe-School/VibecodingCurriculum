# Chapter 17: Building CLI Tools

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have questions while learning, ask in our Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn

- Structure and operation of Node.js CLI tools
- Handling user input (arguments, options)
- File system operations
- Creating interactive prompts
- Publishing to npm
- Building automation tools you actually use

---

## Connection to Previous Chapter

In Chapter 16, we built games that run in the browser. Now we'll make **tools that run in the terminal**. The goal is to automate tasks with a single command without needing a web browser.

---

## Why Do You Need This?

**Real-world scenarios where CLI tools shine:**

- **Your downloads folder is a mess** - Hundreds of files mixed together. A CLI tool can sort them automatically in seconds
- **You create the same project structure repeatedly** - Every new project needs the same folders and files. Automate it!
- **You need to process many files at once** - Rename 100 files, resize images, convert formats... CLI tools handle bulk operations easily
- **You want to share your automation with others** - Publish to npm and anyone can install your tool with one command

Think of CLI tools as **your personal robot assistant** that never gets tired of repetitive tasks.

> **Beginner Tip**: The tools we use daily like `git`, `npm`, `npx` are all CLI tools that someone built to save time. You can build them too!

---

## Simple Analogy: CLI Tools are Like Kitchen Appliances

Imagine you're making juice:
- **Without a blender**: Manually squeeze each fruit, takes forever
- **With a blender**: Put fruits in, press button, done!

CLI tools work the same way:
- **Without CLI tool**: Manually move files, create folders, type commands repeatedly
- **With CLI tool**: Run one command, everything happens automatically

---

## What is CLI?

### CLI vs GUI Comparison

| CLI (Command Line Interface) | GUI (Graphical User Interface) |
|------------------------------|-------------------------------|
| Operated by text commands | Operated by mouse clicks |
| Runs in terminal/command prompt | Programs with windows and buttons |
| Suitable for automation | Intuitive but disadvantageous for repetitive tasks |
| Example: `git commit -m "message"` | Example: GitHub Desktop app |

### Advantages of CLI

```
1. Speed: One command line > multiple clicks
2. Automation: Handle repetitive tasks with scripts
3. Remote work: Can use on servers via SSH
4. Reproducibility: Same command = same result
5. Composition: Connect multiple tools with pipes (|)
```

> **Pro Tip**: As you become a developer, you'll spend a lot of time in the terminal. Knowing how to build CLI tools lets you create your own workflows.

---

## Try It Yourself: Minimal Working Example

Before building the full file organizer, let's make a super simple CLI tool to understand the basics:

**1. Create a single file called `hello-cli.js`:**

```javascript
#!/usr/bin/env node
// ↑ Shebang: tells the system to run this file with Node.js

// Get command line arguments
// process.argv is an array: [node path, script path, arg1, arg2, ...]
// Skip the first two
const args = process.argv.slice(2)

// Use first argument as name (default 'World')
const name = args[0] || 'World'

console.log(`Hello, ${name}!`)
console.log('You just made your first CLI tool!')

// Show all arguments if there are multiple
if (args.length > 1) {
    console.log(`All arguments received: ${args.join(', ')}`)
}
```

**2. Make it executable and run:**

```bash
# On Windows, no permission setup needed
# On Mac/Linux, make it executable
chmod +x hello-cli.js

# Run it!
node hello-cli.js
# Output: Hello, World!

node hello-cli.js Alice
# Output: Hello, Alice!

node hello-cli.js Alice Bob Charlie
# Output: Hello, Alice!
#         All arguments received: Alice, Bob, Charlie
```

**3. Add it to package.json to use as a command:**

```json
{
  "name": "my-first-cli",
  "version": "1.0.0",
  "bin": {
    "greet": "./hello-cli.js"
  }
}
```

```bash
# Link the current project globally
npm link

# Now usable from anywhere!
greet Bob
# Output: Hello, Bob!
```

> **Beginner Tip**: `npm link` makes your CLI tool in the current folder available system-wide. Useful for testing during development.

---

## Core Concepts of CLI Tools

### 1. Shebang (#!/usr/bin/env node)

```javascript
#!/usr/bin/env node
// ↑ This line is the "Shebang"
```

| Operating System | Shebang Required | Description |
|---------|-----------------|------|
| Mac/Linux | Required | Tells the system which program to use when running the file directly |
| Windows | Optional | npm handles it automatically, but good to include for compatibility |

### 2. process.argv - Command Line Arguments

```javascript
// If you run: node myscript.js hello world 123
console.log(process.argv)
// [
//   '/usr/local/bin/node',     // [0] Node.js executable path
//   '/path/to/myscript.js',    // [1] Script path
//   'hello',                   // [2] First argument
//   'world',                   // [3] Second argument
//   '123'                      // [4] Third argument
// ]

// Get only the actual arguments
const args = process.argv.slice(2)  // ['hello', 'world', '123']
```

### 3. process.exit() - Exit Codes

```javascript
// Exit successfully
process.exit(0)

// Exit with error (other scripts can check this result)
process.exit(1)

// Practical usage example
if (!inputFile) {
    console.error('Please specify an input file')
    process.exit(1)
}
```

> **Note**: Exit code 0 means "success", 1 or above means "error". Following this convention ensures compatibility with other scripts.

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

### Why This Project?

| What You Learn | Real-world Application |
|----------|---------|
| File system operations | Foundation of all CLI tools |
| Argument handling | Receiving user input |
| Error handling | Building stable tools |
| Option handling | Flexible behavior settings |

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
├── package.json           # Project configuration
├── bin/
│   └── organize.js        # CLI entry point (file the user runs)
└── src/
    ├── organizer.js       # Core logic (file organization)
    ├── categories.js      # File classification rules
    └── utils.js           # Utility functions
```

> **Beginner Tip**: The `bin/` folder holds executable files, `src/` folder holds actual logic. This separation of concerns makes maintenance easier.

### Step 2: Create CLI Entry Point

```javascript
#!/usr/bin/env node
// bin/organize.js

const { program } = require('commander')
const { organizeFiles } = require('../src/organizer')
const chalk = require('chalk')
const packageJson = require('../package.json')

// Program basic information
program
  .name('organize')
  .description('Automatically organize files by type')
  .version(packageJson.version)  // Read version from package.json

// Main command definition
program
  .argument('<directory>', 'Directory to organize')
  .option('-d, --dry-run', 'Preview without actually moving')
  .option('-v, --verbose', 'Show detailed logs')
  .option('-c, --config <path>', 'Config file path')
  .option('--undo', 'Undo last organization')
  .action((directory, options) => {
    console.log(chalk.cyan('📁 Starting file organization...\n'))

    try {
      organizeFiles(directory, options)
    } catch (error) {
      console.error(chalk.red(`❌ Error: ${error.message}`))
      process.exit(1)
    }
  })

// Additional command: view stats
program
  .command('stats <directory>')
  .description('View folder file statistics')
  .action((directory) => {
    // Statistics display logic
  })

// Start argument parsing
program.parse()
```

**Key Concept: Commander Library**

| Method | Purpose | Example |
|-------|------|------|
| `.argument()` | Define required arguments | `<directory>` |
| `.option()` | Define optional options | `-d, --dry-run` |
| `.action()` | Action when command executes | Pass a function |
| `.command()` | Add subcommands | `stats`, `undo` |

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

// File classification rules definition
const CATEGORIES = {
  images: {
    extensions: ['.jpg', '.jpeg', '.png', '.gif', '.webp', '.svg', '.ico', '.bmp'],
    icon: '🖼️'
  },
  docs: {
    extensions: ['.pdf', '.doc', '.docx', '.txt', '.md', '.xlsx', '.xls', '.pptx', '.ppt'],
    icon: '📄'
  },
  videos: {
    extensions: ['.mp4', '.mov', '.avi', '.mkv', '.webm', '.flv'],
    icon: '🎬'
  },
  audio: {
    extensions: ['.mp3', '.wav', '.flac', '.aac', '.ogg', '.m4a'],
    icon: '🎵'
  },
  archives: {
    extensions: ['.zip', '.rar', '.7z', '.tar', '.gz', '.bz2'],
    icon: '📦'
  },
  code: {
    extensions: ['.js', '.ts', '.py', '.java', '.cpp', '.c', '.html', '.css', '.json'],
    icon: '💻'
  }
}

/**
 * Find category by file extension
 * @param {string} filename - File name
 * @returns {string} - Category name
 */
function getCategory(filename) {
  const ext = path.extname(filename).toLowerCase()

  for (const [category, config] of Object.entries(CATEGORIES)) {
    if (config.extensions.includes(ext)) {
      return category
    }
  }

  return 'others'  // Uncategorized files
}

/**
 * Main organization function
 * @param {string} directory - Directory path to organize
 * @param {object} options - Options (dryRun, verbose, etc.)
 */
function organizeFiles(directory, options = {}) {
  const fullPath = path.resolve(directory)

  // 1. Check if directory exists
  if (!fs.existsSync(fullPath)) {
    throw new Error(`Directory not found: ${fullPath}`)
  }

  // 2. Verify it's a directory
  const stats = fs.statSync(fullPath)
  if (!stats.isDirectory()) {
    throw new Error(`Not a directory: ${fullPath}`)
  }

  // 3. Read file list
  const files = fs.readdirSync(fullPath)
  const results = { moved: 0, skipped: 0, errors: [] }
  const history = []  // For undo feature

  console.log(chalk.gray(`Checking ${files.length} items...\n`))

  // 4. Process each file
  files.forEach(file => {
    const filePath = path.join(fullPath, file)
    const fileStats = fs.statSync(filePath)

    // Skip directories
    if (fileStats.isDirectory()) {
      if (options.verbose) {
        console.log(chalk.gray(`⏭️  Skipped (folder): ${file}`))
      }
      results.skipped++
      return
    }

    // Skip hidden files (optional)
    if (file.startsWith('.')) {
      if (options.verbose) {
        console.log(chalk.gray(`⏭️  Skipped (hidden): ${file}`))
      }
      results.skipped++
      return
    }

    // Determine category
    const category = getCategory(file)
    const categoryConfig = CATEGORIES[category] || { icon: '📁' }
    const targetDir = path.join(fullPath, category)
    const targetPath = path.join(targetDir, file)

    // 5. dry-run mode: preview only
    if (options.dryRun) {
      console.log(chalk.yellow(
        `${categoryConfig.icon} [Preview] ${file} → ${category}/`
      ))
      return
    }

    // 6. Actually move
    try {
      // Create target folder if it doesn't exist
      if (!fs.existsSync(targetDir)) {
        fs.mkdirSync(targetDir, { recursive: true })
        if (options.verbose) {
          console.log(chalk.blue(`📁 Created folder: ${category}/`))
        }
      }

      // Rename if same name file exists
      let finalPath = targetPath
      if (fs.existsSync(targetPath)) {
        finalPath = getUniqueFilename(targetPath)
        console.log(chalk.yellow(`⚠️  Name conflict: ${file} → ${path.basename(finalPath)}`))
      }

      // Move file
      fs.renameSync(filePath, finalPath)

      // Save history (for undo)
      history.push({ from: filePath, to: finalPath })

      if (options.verbose) {
        console.log(chalk.green(
          `${categoryConfig.icon} ${file} → ${category}/`
        ))
      }

      results.moved++

    } catch (error) {
      console.log(chalk.red(`❌ Failed: ${file} - ${error.message}`))
      results.errors.push({ file, error: error.message })
    }
  })

  // 7. Save history
  if (!options.dryRun && history.length > 0) {
    saveHistory(fullPath, history)
  }

  // 8. Summary
  console.log('')
  console.log(chalk.cyan('─'.repeat(40)))
  console.log(chalk.cyan(`Organization complete!`))
  console.log(chalk.green(`  ✓ Moved: ${results.moved}`))
  console.log(chalk.gray(`  ⏭️  Skipped: ${results.skipped}`))
  if (results.errors.length > 0) {
    console.log(chalk.red(`  ❌ Failed: ${results.errors.length}`))
  }

  if (options.dryRun) {
    console.log(chalk.yellow('\n💡 This is preview mode. Remove --dry-run to actually move files.'))
  }

  return results
}

/**
 * Generate unique filename when duplicate exists
 * example.txt → example (1).txt → example (2).txt
 */
function getUniqueFilename(filePath) {
  const dir = path.dirname(filePath)
  const ext = path.extname(filePath)
  const base = path.basename(filePath, ext)

  let counter = 1
  let newPath = filePath

  while (fs.existsSync(newPath)) {
    newPath = path.join(dir, `${base} (${counter})${ext}`)
    counter++
  }

  return newPath
}

/**
 * Save history for undo
 */
function saveHistory(directory, history) {
  const historyPath = path.join(directory, '.organize-history.json')
  fs.writeFileSync(historyPath, JSON.stringify(history, null, 2))
}

/**
 * Undo last organization
 */
function undoLastOrganize(directory) {
  const historyPath = path.join(directory, '.organize-history.json')

  if (!fs.existsSync(historyPath)) {
    throw new Error('No history to undo')
  }

  const history = JSON.parse(fs.readFileSync(historyPath, 'utf-8'))
  let restored = 0

  history.forEach(({ from, to }) => {
    if (fs.existsSync(to)) {
      fs.renameSync(to, from)
      restored++
      console.log(chalk.yellow(`↩️  ${path.basename(to)} → original location`))
    }
  })

  // Delete history file
  fs.unlinkSync(historyPath)

  console.log(chalk.green(`\n✓ Restored ${restored} files`))
}

module.exports = { organizeFiles, undoLastOrganize, getCategory }
```

> **Pro Tip**: Always create the `--dry-run` option first. Tools that handle files can be hard to undo if mistakes are made.

### Step 4: Test Locally

```bash
# Link the current project globally
npm link

# Test with preview mode (safe!)
organize ./downloads --dry-run

# Run with detailed logs
organize ./downloads --verbose

# Actually organize
organize ./downloads

# Undo
organize ./downloads --undo
```

### Mini Quiz

1. What's the difference between `path.resolve()` and `path.join()`?
2. What's the difference between `fs.existsSync()` vs `fs.exists()`?
3. Why is the `--dry-run` option important?

<details>
<summary>See Answers</summary>

1. `path.resolve()` returns an absolute path, `path.join()` just concatenates paths
2. `existsSync` is synchronous, `exists` is asynchronous (deprecated)
3. Allows preview before actual file changes - prevents mistakes!

</details>

---

## Extending Features

### Configuration File Support

```
> Read a .organizerc config file for custom rules.
> Let users define their own categories and extension mappings.
```

```json
// .organizerc (create in project folder)
{
  "categories": {
    "code": [".js", ".ts", ".py", ".go", ".rs"],
    "design": [".psd", ".ai", ".sketch", ".figma", ".xd"],
    "3d": [".blend", ".obj", ".fbx", ".stl"]
  },
  "ignore": ["node_modules", ".git", "*.tmp"],
  "defaultCategory": "misc"
}
```

```javascript
// Config file loading function
function loadConfig(directory) {
  const configPaths = [
    path.join(directory, '.organizerc'),
    path.join(directory, '.organizerc.json'),
    path.join(process.env.HOME, '.organizerc')  // Global config
  ]

  for (const configPath of configPaths) {
    if (fs.existsSync(configPath)) {
      console.log(chalk.gray(`📋 Config loaded: ${configPath}`))
      return JSON.parse(fs.readFileSync(configPath, 'utf-8'))
    }
  }

  return null  // Use default config
}
```

### Interactive Mode

```
> Add interactive mode using inquirer.
> Option to ask where to move each file.
```

```javascript
const inquirer = require('inquirer')

async function interactiveMode(files, fullPath) {
  console.log(chalk.cyan('\n🔄 Interactive Mode\n'))

  for (const file of files) {
    const filePath = path.join(fullPath, file)

    // Show file info
    const stats = fs.statSync(filePath)
    const size = formatFileSize(stats.size)

    const { action } = await inquirer.prompt([
      {
        type: 'list',
        name: 'action',
        message: `${file} (${size})`,
        choices: [
          { name: '🖼️  images/', value: 'images' },
          { name: '📄 docs/', value: 'docs' },
          { name: '🎬 videos/', value: 'videos' },
          { name: '🎵 audio/', value: 'audio' },
          { name: '📦 archives/', value: 'archives' },
          { name: '⏭️  Skip', value: 'skip' },
          { name: '🗑️  Delete', value: 'delete' }
        ]
      }
    ])

    // Handle based on selection
    if (action === 'skip') continue
    if (action === 'delete') {
      const { confirm } = await inquirer.prompt([
        {
          type: 'confirm',
          name: 'confirm',
          message: `Really delete ${file}?`,
          default: false
        }
      ])
      if (confirm) fs.unlinkSync(filePath)
    } else {
      // Move file
      moveToCategory(filePath, action)
    }
  }
}

function formatFileSize(bytes) {
  if (bytes < 1024) return bytes + ' B'
  if (bytes < 1024 * 1024) return (bytes / 1024).toFixed(1) + ' KB'
  return (bytes / (1024 * 1024)).toFixed(1) + ' MB'
}
```

> **Beginner Tip**: Using inquirer, you can create nice menus you navigate with arrow keys. Makes the user experience much better!

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

Next steps:
  cd my-project
  npm install
  npm run dev
```

### Why Do You Need Such a Tool?

```
Every time you start a new project:
1. Create folder
2. npm init
3. Install required packages
4. Create config files (tsconfig, eslint, prettier...)
5. Create folder structure
6. Write basic files

→ Takes 30 minutes to 1 hour

With a CLI tool:
1. Run command
2. Answer questions

→ Takes 1 minute!
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
// bin/create-my-app.js

const inquirer = require('inquirer')
const fs = require('fs-extra')
const path = require('path')
const chalk = require('chalk')
const { execSync } = require('child_process')
const ora = require('ora')  // Loading spinner

// Template definitions by project type
const TEMPLATES = {
  react: {
    name: 'React App',
    dependencies: ['react', 'react-dom'],
    devDependencies: ['vite', '@vitejs/plugin-react']
  },
  'node-api': {
    name: 'Node.js API',
    dependencies: ['express', 'cors'],
    devDependencies: ['nodemon']
  },
  'cli-tool': {
    name: 'CLI Tool',
    dependencies: ['commander', 'chalk', 'inquirer'],
    devDependencies: []
  }
}

async function main() {
  // 1. Welcome message
  console.log(chalk.cyan(`
╔════════════════════════════════════════╗
║                                        ║
║   🚀 Create My App                     ║
║   Quickly start your project!          ║
║                                        ║
╚════════════════════════════════════════╝
`))

  // 2. Get user input
  const answers = await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: 'Project name:',
      default: 'my-app',
      validate: (input) => {
        if (/^[a-z0-9-]+$/.test(input)) return true
        return 'Only lowercase letters, numbers, and hyphens allowed'
      }
    },
    {
      type: 'list',
      name: 'template',
      message: 'Project type:',
      choices: Object.entries(TEMPLATES).map(([key, val]) => ({
        name: val.name,
        value: key
      }))
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
      choices: [
        { name: 'Jest', value: 'jest' },
        { name: 'Vitest', value: 'vitest' },
        { name: 'None', value: 'none' }
      ]
    },
    {
      type: 'confirm',
      name: 'git',
      message: 'Initialize Git repository?',
      default: true
    }
  ])

  const projectPath = path.join(process.cwd(), answers.name)

  // 3. Check if folder exists
  if (fs.existsSync(projectPath)) {
    const { overwrite } = await inquirer.prompt([{
      type: 'confirm',
      name: 'overwrite',
      message: `${answers.name} folder already exists. Overwrite?`,
      default: false
    }])

    if (!overwrite) {
      console.log(chalk.yellow('Cancelled.'))
      process.exit(0)
    }

    fs.removeSync(projectPath)
  }

  // 4. Start project creation
  console.log(chalk.cyan('\n📁 Creating project...\n'))
  const spinner = ora()

  try {
    // 4-1. Create folders
    spinner.start('Creating folder structure...')
    fs.ensureDirSync(projectPath)
    fs.ensureDirSync(path.join(projectPath, 'src'))

    if (answers.template === 'react') {
      fs.ensureDirSync(path.join(projectPath, 'src', 'components'))
      fs.ensureDirSync(path.join(projectPath, 'public'))
    } else if (answers.template === 'node-api') {
      fs.ensureDirSync(path.join(projectPath, 'src', 'routes'))
      fs.ensureDirSync(path.join(projectPath, 'src', 'middleware'))
    } else if (answers.template === 'cli-tool') {
      fs.ensureDirSync(path.join(projectPath, 'bin'))
    }
    spinner.succeed('Folder structure created')

    // 4-2. Create package.json
    spinner.start('Creating package.json...')
    const pkg = createPackageJson(answers)
    fs.writeJsonSync(path.join(projectPath, 'package.json'), pkg, { spaces: 2 })
    spinner.succeed('package.json created')

    // 4-3. Create template files
    spinner.start('Creating template files...')
    createTemplateFiles(projectPath, answers)
    spinner.succeed('Template files created')

    // 4-4. TypeScript configuration
    if (answers.typescript) {
      spinner.start('Configuring TypeScript...')
      createTsConfig(projectPath, answers.template)
      spinner.succeed('TypeScript configured')
    }

    // 4-5. Git initialization
    if (answers.git) {
      spinner.start('Initializing Git...')
      execSync('git init', { cwd: projectPath, stdio: 'ignore' })
      createGitignore(projectPath)
      spinner.succeed('Git initialized')
    }

    // 4-6. Install dependencies
    spinner.start('Installing packages... (please wait)')
    execSync('npm install', { cwd: projectPath, stdio: 'ignore' })
    spinner.succeed('Packages installed')

    // 5. Completion message
    console.log(chalk.green(`
╔════════════════════════════════════════╗
║                                        ║
║   ✅ ${answers.name} project created!
║                                        ║
╚════════════════════════════════════════╝

Next steps:

  ${chalk.cyan(`cd ${answers.name}`)}
  ${chalk.cyan('npm run dev')}

Happy coding! 🎉
`))

  } catch (error) {
    spinner.fail('Error occurred')
    console.error(chalk.red(error.message))
    process.exit(1)
  }
}

function createPackageJson(answers) {
  const template = TEMPLATES[answers.template]
  const ext = answers.typescript ? 'ts' : 'js'

  const pkg = {
    name: answers.name,
    version: '1.0.0',
    description: '',
    main: answers.template === 'cli-tool' ? `bin/index.${ext}` : `src/index.${ext}`,
    scripts: {},
    dependencies: {},
    devDependencies: {}
  }

  // Template-specific scripts
  if (answers.template === 'react') {
    pkg.scripts = {
      dev: 'vite',
      build: 'vite build',
      preview: 'vite preview'
    }
  } else if (answers.template === 'node-api') {
    pkg.scripts = {
      dev: 'nodemon src/index.js',
      start: 'node src/index.js'
    }
  } else if (answers.template === 'cli-tool') {
    pkg.scripts = {
      dev: 'node bin/index.js',
      link: 'npm link'
    }
    pkg.bin = {
      [answers.name]: `./bin/index.${ext}`
    }
  }

  // Test script
  if (answers.testFramework !== 'none') {
    pkg.scripts.test = answers.testFramework
  }

  return pkg
}

function createTemplateFiles(projectPath, answers) {
  // Create template-specific basic files
  // (In practice, copying from templates/ folder is better)
}

function createTsConfig(projectPath, template) {
  const config = {
    compilerOptions: {
      target: 'ES2020',
      module: 'commonjs',
      strict: true,
      esModuleInterop: true,
      skipLibCheck: true,
      outDir: './dist'
    },
    include: ['src/**/*']
  }

  fs.writeJsonSync(
    path.join(projectPath, 'tsconfig.json'),
    config,
    { spaces: 2 }
  )
}

function createGitignore(projectPath) {
  const content = `
node_modules/
dist/
.env
.DS_Store
*.log
`
  fs.writeFileSync(path.join(projectPath, '.gitignore'), content.trim())
}

main().catch(console.error)
```

> **Beginner Tip**: Using the `ora` library, you can show a loading spinner. Lets users know that something is in progress.

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
  "files": [
    "bin/",
    "src/"
  ],
  "keywords": ["cli", "file", "organizer", "automation"],
  "author": "your-name",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-name/file-organizer-cli"
  }
}
```

The **bin field** is key. The `organize` command runs `./bin/organize.js`.

### Pre-publish Checklist

```markdown
- [ ] Write README.md (usage, examples)
- [ ] Check package.json info (name, version, description)
- [ ] Set up .npmignore or files field
- [ ] Verify tests pass
- [ ] Confirm login with npm whoami
```

### Publishing

```bash
# Log in to npm
npm login

# Check if package name is available
npm search file-organizer-cli

# Publish
npm publish

# Update version and republish
npm version patch  # 1.0.0 → 1.0.1
npm publish

# Now anyone can install!
npm install -g file-organizer-cli
```

> **Note**: npm package names must be unique. If the name already exists, you can't use it. Use `@username/package-name` format with a scope to avoid conflicts.

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
console.log(chalk.gray('Example: organize ./downloads'))
process.exit(1)
```

### 2. Auto-generated --help

commander handles this automatically:

```bash
$ organize --help
Usage: organize [options] <directory>

Automatically organize files by type

Options:
  -V, --version       output the version number
  -d, --dry-run       Preview without actually moving
  -v, --verbose       Show detailed logs
  -c, --config <path> Config file path
  --undo              Undo last organization
  -h, --help          display help for command
```

### 3. Progress Indicators

```
> Show a progress bar when processing many files. Use cli-progress.
```

```javascript
const cliProgress = require('cli-progress')

function processFilesWithProgress(files) {
  const bar = new cliProgress.SingleBar({
    format: 'Progress |{bar}| {percentage}% | {value}/{total} files',
    barCompleteChar: '█',
    barIncompleteChar: '░'
  }, cliProgress.Presets.shades_classic)

  bar.start(files.length, 0)

  files.forEach((file, index) => {
    // Process file...
    bar.update(index + 1)
  })

  bar.stop()
}
```

### 4. Colored Output

```javascript
const chalk = require('chalk')

console.log(chalk.green('✓ Success'))
console.log(chalk.red('✗ Failed'))
console.log(chalk.yellow('⚠ Warning'))
console.log(chalk.blue('ℹ Info'))
console.log(chalk.gray('  Details...'))
console.log(chalk.bold('Important content'))
console.log(chalk.cyan.underline('https://link'))
```

---

## Useful Libraries

| Library | Purpose | Example |
|-----------|------|------|
| `commander` | Argument/option parsing | `program.option('-v, --verbose')` |
| `inquirer` | Interactive prompts | Selection menus, confirm questions |
| `chalk` | Colored output | `chalk.red('error')` |
| `ora` | Spinner animations | Loading indicator |
| `cli-progress` | Progress bars | File processing progress |
| `fs-extra` | Enhanced file system | `fs.copySync`, `fs.ensureDir` |
| `glob` | File pattern matching | `**/*.js` |
| `boxen` | Box UI | Welcome messages, results display |
| `figlet` | ASCII art text | Large title display |
| `update-notifier` | Update notifications | New version alerts |

---

## Practice

### Level 1: Basic (Beginner)

Build the file organizer tool following along.

**Checklist:**
- [ ] Does `organize ./foldername` run?
- [ ] Does the `--dry-run` option work?
- [ ] Are error messages clear?

### Level 2: Applied (Intermediate)

```
> Create a CLI tool that analyzes git commit messages and shows statistics.
> - Count by commit type (feat, fix, docs, etc.)
> - Most active day/time
> - Commits per contributor
```

**Checklist:**
- [ ] Does it read git logs?
- [ ] Are statistics displayed as tables or graphs?
- [ ] Is there a date range filter?

### Level 3: Challenge (Advanced)

```
> Tool that merges markdown files into PDF or HTML documentation

> Tool that analyzes package.json across multiple projects
> to find common dependencies and version conflicts

> Tool that batch resizes/compresses images
```

---

## Challenge Tasks

### Challenge 1: Plugin System

Allow users to add their own classification rules:

```javascript
// ~/.organize/plugins/my-rule.js
module.exports = {
  name: 'screenshots',
  match: (filename) => filename.startsWith('Screenshot'),
  category: 'screenshots'
}
```

### Challenge 2: Cross-platform

Make it work on Windows, Mac, and Linux:

```javascript
const os = require('os')
const homeDir = os.homedir()
const pathSep = path.sep  // Windows: \, Unix: /
```

### Challenge 3: Auto-completion

Generate Bash/Zsh auto-completion scripts:

```bash
# organize <TAB> shows folder list
```

---

## If It Doesn't Work? Troubleshooting Tips

### "command not found" after npm link

```bash
# Check if npm bin is in your PATH
npm bin -g
# The output path should be in $PATH

# Try running with npx
npx organize ./downloads

# Or run with absolute path
$(npm bin -g)/organize ./downloads
```

### "Permission denied" execution permission error

```bash
# Check if file has execute permission
ls -la bin/organize.js
# Should be -rwxr-xr-x

# Grant permission
chmod +x bin/organize.js

# Check if shebang line is correct (first line should be):
#!/usr/bin/env node
```

### Files not moving / nothing happens

```bash
# Check what's happening with verbose flag
organize ./downloads --verbose

# Check if directory exists
ls ./downloads

# Try with absolute path
organize /Users/yourname/downloads

# Check permissions
ls -la ./downloads
```

### "Cannot find module 'commander'" error

```bash
# Check if you installed dependencies
cd your-project
npm install

# Or install specifically
npm install commander chalk

# Check dependencies in package.json
cat package.json | grep -A 10 "dependencies"
```

### Korean characters garbled on Windows

```javascript
// Specify encoding when reading files
fs.readFileSync(path, { encoding: 'utf8' })

// Or set terminal encoding
// PowerShell: chcp 65001
```

---

## Common Mistakes

### 1. Forgetting the Shebang Line

```javascript
// ❌ Wrong - won't run directly
const { program } = require('commander')

// ✅ Correct - needs shebang as first line
#!/usr/bin/env node
const { program } = require('commander')
```

### 2. Not Handling Missing Arguments

```javascript
// ❌ Wrong - crashes if no directory given
const dir = process.argv[2]
fs.readdirSync(dir)  // Error if dir is undefined!

// ✅ Correct - validate first
const dir = process.argv[2]
if (!dir) {
  console.log('Please provide a directory path')
  console.log('Usage: organize <directory>')
  process.exit(1)
}
```

### 3. Hardcoding Paths

```javascript
// ❌ Wrong - only works on your machine
const targetDir = '/Users/yourname/downloads'

// ✅ Correct - use relative or configurable paths
const targetDir = process.argv[2] || './downloads'

// Or base on home directory
const os = require('os')
const defaultDir = path.join(os.homedir(), 'Downloads')
```

### 4. Not Using --dry-run for Testing

Always add a dry-run option when your CLI modifies files! You don't want to accidentally delete or move important files while testing.

```bash
# Always test with dry-run first!
organize ./important-folder --dry-run

# After checking results, actually run
organize ./important-folder
```

### 5. Parsing Options Incorrectly

```javascript
// Note - commander converts kebab-case to camelCase
.option('-d, --dry-run')

// --dry-run becomes options.dryRun (hyphen removed, camelCase)
if (options.dryRun) {  // NOT options['dry-run']!
  // ...
}
```

### 6. Incorrect Async Handling

```javascript
// ❌ Wrong - might exit before file reading completes
files.forEach(async (file) => {
  await processFile(file)
})
console.log('Done!')  // Actually not done yet

// ✅ Correct - wait for all operations to complete
for (const file of files) {
  await processFile(file)
}
console.log('Done!')

// Or use Promise.all (parallel processing)
await Promise.all(files.map(file => processFile(file)))
console.log('Done!')
```

---

## Terminology

| Term | Meaning |
|------|------|
| CLI | Command Line Interface |
| Shebang | `#!/usr/bin/env node` - specifies script executor |
| process.argv | Command line argument array |
| bin field | Specifies executable files in package.json |
| npm link | Install local package globally (for development) |
| Option/Flag | Settings switches like `-v`, `--verbose` |
| Argument | Values passed to commands |
| stdout/stderr | Standard output/standard error |

---

## Next Chapter Preview

Now that we've built CLI tools, let's build **chatbots**!

- **Chapter 18**: Building Discord/Slack Bots
  - Auto-respond to messages
  - Execute functions with commands
  - 24/7 running automation

What CLI and chatbots have in common: Both are **text-based interfaces**. Command processing and argument parsing you learned with CLI apply directly to chatbots!

---

## Summary

What you learned in this chapter:
- [x] Basic structure and operation of CLI tools
- [x] Handling arguments with process.argv
- [x] Option parsing with commander
- [x] File system (fs) operations
- [x] Interactive prompts with inquirer
- [x] Nice output with chalk, ora
- [x] npm publishing

CLI tools greatly boost developer productivity. If you have repetitive tasks, automate them with a CLI!

Proceed to [Chapter 18: Building Chatbots](../Chapter18-Chatbots/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
