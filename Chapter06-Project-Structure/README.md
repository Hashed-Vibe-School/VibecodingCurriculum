# Chapter 06: Understanding Project Structure

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have any questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 Goals for This Chapter

- Understand the **philosophy** and **reasoning** behind project folder structures
- Know the roles of important files and **why they are placed where they are**
- Compare structures across different frameworks
- Learn advanced techniques for asking Claude to explain projects
- Distinguish between good and bad structures

---

## ⏱️ Estimated Time

- Reading: **30 minutes**
- Practice: **45 minutes**

---

## 📋 Prerequisites

- Claude Code installed and logged in
- Basic understanding of terminal commands (Chapter 05)

---

## 🔗 Previous Chapter Review

In Chapter 05, we learned terminal commands. With commands like `ls`, `cd`, and `mkdir`, you can now navigate folders. Now let's learn how to use this ability to understand project structures.

---

## Why Does Project Structure Matter?

Before writing code, you need to understand how a project is organized. It's like figuring out where everything is when you move into a new house.

**Real-world situations:**
- Downloaded a project from GitHub but have no idea what's what
- Don't know which file to modify
- Want to understand how the project works
- Just joined a team and need to quickly grasp the codebase

### Easy Analogy: Exploring a New House

When you move into a new house, you first figure out the layout:
- Where's the entrance (entry point)
- Where's the kitchen (core functionality)
- Where's the storage (auxiliary files)
- Which room serves which purpose (folder roles)

Projects are the same. When you know the roles of folders and files, you can quickly find what you need.

> 💡 **Beginner Tip**
>
> Project structure is a **convention among developers**.
> When there's an agreement like "components go in the components folder",
> anyone opening the project immediately knows where to find components.
> It's like how bathrooms are usually in similar locations in every house.

---

## The Philosophy of Folder Structure: Why Organize This Way?

Folder structure isn't just about organizing files. Good structure embodies a **philosophy**.

### 1. Separation of Concerns

Group code with similar responsibilities together.

```
my-project/
├── src/
│   ├── components/    # UI-related code only
│   ├── api/           # Server communication only
│   ├── styles/        # Design-related only
│   └── utils/         # Utility functions only
```

**Why do this?**
- When modifying button design, just look at the `components` folder
- When the API changes, only modify the `api` folder
- Changes in one place don't affect others

> 🔥 **Pro Tip**
>
> In practice, you'll often wonder "where should I put this file?"
> Key question: **"What is this file's main responsibility?"**
> - If it draws UI → `components`
> - If it fetches data → `api` or `services`
> - If it's a helper function used in multiple places → `utils`

### 2. Scalability

The structure shouldn't break down as the project grows.

```
# Bad example: All files in one place
src/
├── App.js
├── Button.js
├── Header.js
├── api.js
├── utils.js
├── Login.js
├── Dashboard.js
├── UserProfile.js
├── ... (100 files)

# Good example: Separated by role
src/
├── components/
│   ├── common/      # Common components
│   ├── auth/        # Auth-related
│   └── dashboard/   # Dashboard-related
├── pages/
├── api/
└── utils/
```

### 3. Discoverability

Files should be easy to find.

```
# Bad example: Names don't tell you anything
src/
├── stuff/
├── things/
├── misc/

# Good example: Names are self-explanatory
src/
├── authentication/
├── user-management/
├── payment-processing/
```

> ⚠️ **Warning**
>
> Common traps beginners fall into:
> 1. **Excessive nesting**: `src/components/ui/buttons/primary/index.js` (too deep!)
> 2. **Unclear names**: `helpers`, `misc`, `other` (can't tell what's inside)
> 3. **Inconsistency**: Some things are folders, some are files (no rules)

---

## Common Project Structure

Most web projects have a similar structure.

### Basic Web Project Structure

```
my-project/
├── src/                  # Source code (core) - code we write
│   ├── components/       # Reusable UI pieces (buttons, cards, modals, etc.)
│   ├── pages/            # Page-specific code (home, login, settings, etc.)
│   ├── styles/           # CSS style files
│   ├── utils/            # Utility functions (date conversion, formatting, etc.)
│   ├── hooks/            # React custom hooks (state management logic)
│   ├── api/              # API call functions
│   └── types/            # TypeScript type definitions
├── public/               # Static files (images, icons, favicon)
├── tests/                # Test files
├── node_modules/         # Installed packages (don't touch!)
├── package.json          # Project configuration file (ID card)
├── package-lock.json     # Exact package version records
├── README.md             # Project documentation
├── .gitignore            # List of files Git should ignore
├── .env                  # Environment variables (secrets)
└── tsconfig.json         # TypeScript configuration (if applicable)
```

> 💡 **Beginner Tip**
>
> You don't need to memorize this structure!
> Ask Claude "explain this project structure" and it will tell you.
> What's important is knowing **that structure exists**.
>
> Start by just remembering the `src` folder and `package.json`. Learn the rest when needed.

---

## Framework-Specific Structure Comparison

### Next.js Project (App Router)

```
nextjs-project/
├── app/                  # Pages and routing (folder = URL path)
│   ├── page.tsx          # Home page (/)
│   ├── layout.tsx        # Common layout
│   ├── about/
│   │   └── page.tsx      # /about page
│   └── api/
│       └── users/
│           └── route.ts  # /api/users API
├── components/           # UI components
├── lib/                  # Utility functions
├── public/               # Static files
└── package.json
```

**Feature**: Folder structure is URL structure! `app/about/page.tsx` = `/about` page

### React (Create React App)

```
react-project/
├── src/
│   ├── App.js            # Main app component
│   ├── index.js          # Entry point
│   ├── components/
│   ├── pages/
│   └── styles/
├── public/
│   └── index.html        # HTML template
└── package.json
```

**Feature**: `src/index.js` is the starting point, routing requires a separate library

### Vue.js Project

```
vue-project/
├── src/
│   ├── App.vue           # Main app component
│   ├── main.js           # Entry point
│   ├── components/
│   ├── views/            # Page components
│   ├── router/           # Routing configuration
│   └── store/            # State management (Vuex/Pinia)
└── package.json
```

**Feature**: Uses `.vue` extension, `views` folder serves as pages

### Python Django Project

```
django-project/
├── manage.py             # Django command execution
├── mysite/               # Project settings
│   ├── settings.py       # Overall settings
│   ├── urls.py           # URL routing
│   └── wsgi.py           # Web server settings
├── myapp/                # App (feature unit)
│   ├── models.py         # Database models
│   ├── views.py          # Request handling logic
│   ├── urls.py           # App URLs
│   └── templates/        # HTML templates
└── requirements.txt      # Package list (serves as package.json)
```

**Feature**: Separates functionality by `app` units, defines DB structure in `models.py`

> 🔥 **Pro Tip**
>
> Even with different frameworks, the **core concepts** are the same:
> - There's an entry point
> - There are components/views
> - There's routing (URL connection)
> - There are configuration files
>
> Once you learn one framework, you can quickly learn others!

---

## Deep Dive into Core Files

### package.json - The Project's ID Card

The most important file in every Node.js project. Contains all information about the project.

```json
{
  "name": "my-awesome-project",      // Project name
  "version": "1.0.0",                // Version number
  "description": "Todo management app", // Description
  "main": "index.js",                // Entry point file
  "scripts": {                       // Executable commands
    "dev": "next dev",               // Start dev server
    "build": "next build",           // Build for deployment
    "start": "next start",           // Start production server
    "test": "jest",                  // Run tests
    "lint": "eslint ."               // Code inspection
  },
  "dependencies": {                  // Packages needed to run
    "react": "^18.2.0",
    "next": "^14.0.0"
  },
  "devDependencies": {               // Packages needed only for development
    "typescript": "^5.0.0",
    "eslint": "^8.0.0"
  }
}
```

**What this file tells you:**
- `name`: Project name
- `scripts`: Executable commands (`npm run dev`, etc.)
- `dependencies`: Libraries essential for the app to work
- `devDependencies`: Tools only needed during development

> 💡 **Beginner Tip**
>
> Difference between `dependencies` and `devDependencies`:
> - `dependencies`: What's needed when the app is shown to users (React, Next.js, etc.)
> - `devDependencies`: What's only needed when developers write code (ESLint, TypeScript, etc.)
>
> Think of it like a restaurant:
> - `dependencies` = Cooking ingredients (served to customers)
> - `devDependencies` = Kitchen tools (only used by chefs)

### README.md - Project Documentation

A file that explains what the project is and how to run it.

**What a good README includes:**

```markdown
# Project Name

Brief description of the project

## Installation

npm install

## Running

npm run dev

## Main Features
- Feature 1
- Feature 2

## Tech Stack
- React 18
- Next.js 14
- TypeScript

## Folder Structure Explanation
(Optional but very useful)
```

A good project can be started just by reading the README.

### .gitignore - List of Files to Ignore

Specifies files that shouldn't be uploaded to Git.

```gitignore
# Dependencies folder (can be recreated with npm install)
node_modules/

# Environment variables (contains secrets!)
.env
.env.local

# Build output (can be recreated with build command)
.next/
build/
dist/

# OS-generated files
.DS_Store          # macOS
Thumbs.db          # Windows

# IDE settings (personal settings, no need to share)
.vscode/
.idea/

# Log files
*.log
npm-debug.log*
```

> ⚠️ **Warning**
>
> Never upload `.env` files to Git!
> They contain sensitive information like API keys and database passwords.
> Once uploaded, it remains in the history and is very hard to completely remove.

### .env - Environment Variables File

A file that stores secret information.

```env
# Never upload to Git!

# API keys
NEXT_PUBLIC_API_KEY=your-api-key-here
SECRET_API_KEY=super-secret-key

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/mydb

# External services
STRIPE_SECRET_KEY=sk_test_xxxxx
```

> 🔥 **Pro Tip**
>
> Variables with the `NEXT_PUBLIC_` prefix are accessible in the browser.
> Never add this prefix to secret keys!
>
> - `NEXT_PUBLIC_API_URL` = Can be public (API address)
> - `SECRET_KEY` = Private (server-side only)

---

## Detailed Folder Role Explanation

### src/ Folder

The core folder containing **source code**.

| Folder | Role | Analogy | Example Files |
|--------|------|---------|---------------|
| `components/` | Reusable UI pieces | Lego blocks | `Button.tsx`, `Modal.tsx` |
| `pages/` | Code for each page | Chapters of a book | `Home.tsx`, `Login.tsx` |
| `styles/` | Design, colors, layout | Interior design | `globals.css`, `theme.ts` |
| `utils/` | Helper functions | Toolbox | `formatDate.ts`, `validate.ts` |
| `api/` | Code for server communication | Telephone | `userApi.ts`, `authApi.ts` |
| `hooks/` | React custom hooks | Frequently used recipes | `useAuth.ts`, `useFetch.ts` |
| `types/` | TypeScript type definitions | Blueprints | `User.ts`, `Product.ts` |
| `constants/` | Constant values | Rulebook | `routes.ts`, `messages.ts` |
| `context/` | React Context | Shared bulletin board | `AuthContext.tsx` |

### components/ Folder Organization Patterns

As projects grow, components get subdivided.

```
components/
├── common/           # Common components (used everywhere)
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.styles.ts
│   │   └── index.ts
│   ├── Modal/
│   └── Card/
├── layout/           # Layout components
│   ├── Header/
│   ├── Footer/
│   └── Sidebar/
├── forms/            # Form-related components
│   ├── Input/
│   ├── Select/
│   └── Checkbox/
└── features/         # Feature-specific components
    ├── auth/
    │   ├── LoginForm/
    │   └── SignupForm/
    └── dashboard/
        ├── Stats/
        └── Chart/
```

> 💡 **Beginner Tip**
>
> You don't need this complexity at the start!
> If you have fewer than 10 files, just put them all in the `components/` folder.
> When files multiply and become hard to find, that's when you separate.
>
> This is called **progressive refactoring**.

### public/ Folder

Contains **static files**. Images, icons, fonts - files that don't change.

```
public/
├── images/           # Image files
│   ├── logo.png
│   └── hero-bg.jpg
├── fonts/            # Web font files
│   └── Pretendard.woff2
├── favicon.ico       # Browser tab icon
└── robots.txt        # Search engine crawler settings
```

**Feature**: Files in this folder are directly accessible via URL.
- `public/images/logo.png` → `http://localhost:3000/images/logo.png`

### node_modules/ Folder

**Never touch this!**

This folder is automatically created when you run `npm install`. It contains thousands of files, but you'll never need to modify them directly.

```
node_modules/
├── react/            # React library
├── next/             # Next.js framework
├── lodash/           # Utility library
└── ... (hundreds to thousands of folders)
```

> ⚠️ **Warning**
>
> - Never modify directly
> - Don't upload to Git (included in `.gitignore`)
> - Can be restored with `npm install` even if deleted
> - Very large (hundreds of MB) so shouldn't be uploaded to Git

---

## Good Structure vs Bad Structure

### Example of Bad Structure and Its Problems

```
bad-project/
├── app.js                # All logic in one file 😱
├── helpers.js            # Can't tell what's inside
├── stuff/                # Unclear name
│   ├── thing1.js
│   └── thing2.js
├── old_code/             # Don't know if it's used or not
├── test.js               # Test file in root
├── Button.js             # Component in root
├── styles.css            # Styles in root
└── misc/                 # All sorts of miscellaneous
```

**Problems:**
1. Hard to find where files are
2. Hard for new team members to understand
3. Confusion increases as files multiply
4. Similar files are scattered

### Example of Good Structure

```
good-project/
├── src/
│   ├── components/       # UI component collection
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   └── Button.styles.ts
│   │   └── Modal/
│   ├── pages/            # Page components
│   ├── hooks/            # Custom hooks
│   ├── utils/            # Utility functions
│   ├── api/              # API calls
│   ├── types/            # Type definitions
│   └── constants/        # Constants
├── tests/                # E2E tests
├── public/               # Static files
├── docs/                 # Documentation
└── package.json
```

**Advantages:**
1. Can find files immediately
2. Easy for new team members to understand
3. Easy to extend
4. Tests and styles are with components

### Structure Comparison Table

| Item | Bad Structure | Good Structure |
|------|---------------|----------------|
| File location | Unpredictable | Intuitive |
| Folder names | `stuff`, `misc` | `components`, `utils` |
| Related files | Scattered | Grouped together |
| Scalability | Confusion as files grow | Systematic expansion possible |
| Tests | Don't know where they are | Next to components |

---

## Large-Scale Project Structure

Let's look at large-scale project structures used in real companies.

### Feature-based Structure

```
large-project/
├── src/
│   ├── features/              # Separated by feature
│   │   ├── auth/              # Authentication feature
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── api/
│   │   │   ├── types/
│   │   │   └── index.ts
│   │   ├── dashboard/         # Dashboard feature
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   └── ...
│   │   ├── payment/           # Payment feature
│   │   └── user-profile/      # User profile
│   ├── shared/                # Shared code
│   │   ├── components/        # Common components
│   │   ├── hooks/             # Common hooks
│   │   └── utils/             # Common utilities
│   └── app/                   # App configuration
│       ├── routes/
│       └── providers/
```

**Advantages:**
- Features are independent units
- All code for one feature is in one place
- Teams can be assigned to specific features

> 🔥 **Pro Tip**
>
> Large-scale project structure isn't needed from the start.
>
> **Recommended progression:**
> 1. Start: Simple `src/components`, `src/pages` structure
> 2. Growth: When components exceed 30, separate into `common/`, `features/`
> 3. Expansion: Transition to feature-based structure when features get complex
>
> **YAGNI Principle**: "You Aren't Gonna Need It"
> Add it when you need it. Don't make it complex in advance.

### Monorepo Structure

A method of managing multiple projects in a single repository.

```
monorepo/
├── apps/                     # Applications
│   ├── web/                  # Web app
│   ├── mobile/               # Mobile app
│   └── admin/                # Admin app
├── packages/                 # Shared packages
│   ├── ui/                   # Shared UI components
│   ├── utils/                # Shared utilities
│   └── config/               # Shared configuration
└── package.json
```

**Use case:** When large companies like Naver or Kakao have multiple services sharing common components

---

## Asking Claude to Understand Projects

### Understanding Overall Structure

```
> Explain this project structure
```

Claude analyzes the folders and files and explains.

```
> What does this project do?
```

It analyzes README.md and code to tell you the purpose.

### Asking About Specific File Roles

```
> @package.json Explain this file
```

```
> @src/components/ What's in this folder?
```

### Asking How to Start

```
> How do I run this project?
```

Claude checks the scripts in package.json and guides you.

### Advanced Question Examples

```
# Understanding architecture
> Explain the data flow in this project

# Dependency analysis
> List the main libraries used in this project and their roles

# Finding entry points
> Where does this app start? What's the first code that runs?

# Understanding relationships
> Where is the UserProfile component being used?
```

---

## 🎯 Mini Quiz

Answer the following questions.

### Quiz 1
Which statement about `node_modules` is **incorrect**?

A) It's automatically created when running `npm install`
B) It's okay to modify it directly
C) It shouldn't be uploaded to Git
D) It can be reinstalled even if deleted

<details>
<summary>See Answer</summary>

**Answer: B**

You should never directly modify `node_modules`.
It gets overwritten on the next `npm install`.

</details>

### Quiz 2
Which statement about the `.env` file is **correct**?

A) You should upload it to Git to share with the team
B) It stores secret information like API keys
C) It can be directly accessed from the browser
D) It's used instead of package.json

<details>
<summary>See Answer</summary>

**Answer: B**

The `.env` file stores secret information and
**should never be uploaded to Git**.
It must be included in `.gitignore`.

</details>

### Quiz 3
What's the problem with this structure?

```
my-project/
├── stuff/
├── things/
├── misc/
└── other/
```

<details>
<summary>See Answer</summary>

**Problems:**
- Folder names are unclear so you can't tell what's inside
- Hard for new team members to find code
- Should use clear names like `components`, `utils`, `api`

</details>

---

## 📝 Practice Exercises

### 🟢 Beginner: Creating Project Structure

```
> Create a my-first-app folder, and inside it create src, public, and styles folders
> Also create components, pages, and utils folders inside src
```

**Expected Result:** Basic folder structure is created

### 🟡 Intermediate: Creating and Analyzing package.json

```
> Initialize npm in the my-first-app folder

> @package.json Add a scripts section to this file
> dev should be "echo starting dev server", build should be "echo building"
```

**Expected Result:** scripts are added to package.json

### 🔴 Advanced: Analyzing Existing Projects

Clone any project from GitHub:

```bash
git clone https://github.com/vercel/next.js.git
cd next.js
claude
```

```
> Analyze this project. What does it do?
> Tell me the 5 most important folders in this project
> What structural pattern does this project use?
```

---

## 🏆 Challenge Tasks

### Challenge 1: Framework Comparison Analysis

Clone 3 different framework projects and compare their structures.

```bash
# React
npx create-react-app react-sample
# Next.js
npx create-next-app nextjs-sample
# Vue
npm create vue@latest vue-sample
```

Ask Claude in each project:
```
> Analyze this project structure
> What's the entry point file?
> Where should I put components?
```

Summarize the similarities and differences.

### Challenge 2: Write Your Own Structure Guide

Create your own project structure guide.

```
> Create a folder structure guide for web development projects
> Include the role and example files for each folder
> Save it as STRUCTURE_GUIDE.md
```

---

## 🚨 Common Errors and Solutions

### Error 1: "node_modules is missing"

**Situation:** Downloaded a project but it won't run

**Cause:** Dependencies not installed

**Solution:**
```
> Run npm install
```

### Error 2: "Don't know which file to modify"

**Situation:** Want to change something but don't know where to look

**Solution:**
```
> Which file should I look at to change [what you want to change] in this project?
```

Example:
```
> Which file should I look at to change the main page title?
> Where do I modify to change the login button color?
```

### Error 3: "package.json is missing"

**Situation:** npm commands don't work

**Cause:** You're not in the project root, you're in a different folder

**Solution:**
```
> Where is package.json?
> Move to that folder
```

### Error 4: "Cannot find module" Error

**Situation:** `Cannot find module 'xxx'` error occurs

**Cause:**
- Package not installed, or
- Import path is wrong

**Solution:**
```
> Why is this error happening? [error message]
> Install the necessary packages
```

---

## ❌ Common Mistakes

### 1. Directly Modifying node_modules
This folder is automatically generated by `npm install`. Never modify it directly.

> ⚠️ **Why is this dangerous?**
>
> - Changes disappear on next `npm install`
> - Creates different environment from teammates
> - Makes debugging very difficult

### 2. Not Checking Project Root
Commands should be run from the project root (where package.json is).
```
> Where am I now?
```
Always check with this.

### 3. Not Reading README
Good projects have everything explained in the README. Before asking Claude:
```
> @README.md Read this for me
```

### 4. Trying to Understand Everything at Once
At first, just understand the parts you need. Don't try to understand everything.

> 💡 **Beginner Tip**
>
> The "as needed, as much as needed" strategy is effective.
>
> 1. First understand what the project does
> 2. Focus only on the part you need to modify
> 3. Only look at related files
> 4. The rest can wait until you need it

### 5. Uploading .env Files to Git

**Never do this!**

```
# Must add to .gitignore
.env
.env.local
.env.production
```

---

## 🖼️ Expected Screens

### Requesting Project Structure Explanation

```
> Explain this project structure

This project is a Next.js 14 based web application.

📁 Main folder structure:

src/
├── app/          # Next.js App Router (pages and routing)
│   ├── page.tsx  # Home page (/)
│   ├── layout.tsx # Common layout
│   └── api/      # API routes
├── components/   # Reusable UI components
│   ├── Header/
│   └── Button/
└── lib/          # Utility functions

📦 Main dependencies:
- React 18.2.0
- Next.js 14.0.0
- TypeScript 5.0.0

🚀 How to run:
npm run dev (development server)
npm run build (production build)
```

### Analyzing a Specific Folder

```
> @src/components/ Analyze this folder

The components folder contains 5 components:

1. Button/ - Button component with various styles
   - Button.tsx: Main component
   - Button.styles.ts: Style definitions
   - index.ts: Exports

2. Modal/ - Popup modal component
   - Includes open/close animations

3. Card/ - Card for displaying information
   - Supports image, title, description

4. Header/ - Top navigation for pages

5. Footer/ - Bottom information for pages

Pattern: Each component is in its own folder,
with related files (styles, tests) together.
```

---

## Mini Project: Project Explorer

Analyze any open-source project.

### 🟢 Easy: Basic Analysis

```
> What is this project?
> How do I run it?
> Tell me the 3 most important files
```

### 🟡 Medium: Understanding Structure

```
> Explain the folder structure
> Where is the main page in this project?
> What pattern are the components organized in?
> How is styling done?
```

### 🔴 Challenge: Deep Analysis

```
> List the libraries used in this project
> Where is the entry point of this project?
> Explain the data flow
> What is the architectural pattern of this project?
> Are there any structural issues that could be improved?
```

---

## 🆘 If It's Not Working?

| Symptom | Solution |
|---------|----------|
| Structure doesn't make sense | Add "explain like I'm a beginner" |
| Don't know what a file is | Ask Claude "@filename what is this?" |
| Too many folders | "Just tell me the 3 most important folders" |
| Project doesn't run | "Tell me why it's not running" |
| Error message doesn't make sense | Copy the error message and ask Claude |

---

## 📚 Glossary

| Term | Description |
|------|-------------|
| **Entry Point** | The first file where the app starts |
| **Dependency** | External libraries the project needs |
| **Component** | Reusable UI piece |
| **Routing** | Connecting URLs to pages |
| **Static Files** | Files that don't change (images, fonts, etc.) |
| **Environment Variables** | Variables that store configuration values |
| **Build** | Converting development code for deployment |
| **Monorepo** | Managing multiple projects in one repository |

---

## ✅ Checklist

Check before finishing your learning:

- [ ] I know the general project folder structure
- [ ] I know the role and main items of package.json
- [ ] I know the difference between src/, public/, and node_modules/ folders
- [ ] I know the importance of .gitignore and .env files
- [ ] I know the difference between good and bad structure
- [ ] I can ask Claude about project structure
- [ ] I understand the structure differences between various frameworks

---

## ➡️ Next Chapter Preview

In the next chapter, we'll learn how to utilize **Context and Memory**.

What you'll learn:
- How AI remembers conversations
- Setting project rules with CLAUDE.md file
- Effective conversation management strategies
- Overcoming memory limitations

Proceed to [Chapter 07: Context and Memory](../Chapter07-Context-and-Memory/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
