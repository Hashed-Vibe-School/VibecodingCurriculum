# Chapter 26: CI/CD Automation

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Connection to Previous Chapter

In Chapter 25, you learned how Claude can communicate with external services through MCP. You can now directly execute database queries, GitHub issue management, Slack notifications, and more.

Now let's go one step further and learn about **automation**:

| Chapter | Content | Characteristic |
|---------|---------|----------------|
| Chapter 25 (MCP) | Claude accesses external services | Manual request → Auto execution |
| **Chapter 26 (CI/CD)** | **Auto execution on code changes** | **Push and everything runs automatically!** |

With MCP, Claude moved when you requested it, but with CI/CD, **just pushing code** automatically runs tests, builds, and deployment!

### CI/CD and Previous Chapters Relationship

```
┌─────────────────────────────────────────────────────────────────────┐
│                Concepts from Previous Chapters Used in CI/CD         │
└─────────────────────────────────────────────────────────────────────┘

Chapter 22 (Configuration)
    │ Define project rules with CLAUDE.md
    └──────────────────────────────┐
                                    │
Chapter 23 (Hooks & Commands)       │
    │ Automation scripts and        ├──▶ Integrated use in
    │ commands                      │     CI/CD pipelines!
    └──────────────────────────────┤
                                    │
Chapter 24 (Agents & Skills)        │
    │ Automated roles and           │
    │ workflows                     │
    └──────────────────────────────┤
                                    │
Chapter 25 (MCP)                   │
    │ External service integration  │
    └──────────────────────────────┘
```

---

## What You Will Learn in This Chapter

- CI/CD concepts and necessity
- Building automation with GitHub Actions
- YAML file writing
- Integrating Claude Code into pipelines
- Building production workflows
- Security management and cost optimization

---

## Why is CI/CD Important?

### Real Scenario: The Fear of Deployment

**Without CI/CD:**

```
Friday, 5 PM...

Developer: "I need to deploy a small bug fix"
       git push origin main

Developer: "Huh? There's an error in production..."

Developer: "I didn't run the tests..."

Developer: "The build was broken too..."

Team Lead: "Why is production down on Friday evening..."

Developer: 😱 (Weekend ruined)
```

**With CI/CD:**

```
Friday, 5 PM...

Developer: "I need to deploy a small bug fix"
       git push origin main

GitHub Actions:
  ✅ Lint passed
  ✅ Tests passed
  ✅ Build successful
  ✅ Auto-deployment complete
  📢 Slack: "Bug fix deployed!"

Developer: "Time to enjoy the weekend~" 😊
```

### Manual vs Automation Comparison

| Step | Manual | CI/CD |
|------|--------|-------|
| Code inspection | If I remember... | Auto-runs every time |
| Testing | If I have time... | Auto-runs every time |
| Build | Manually... | Auto-runs every time |
| Deployment | Manually... | Auto when tests pass |
| Notifications | If I remember... | Auto-sent |
| **When failure is discovered** | **In production** | **Right after code push** |

---

## Why Do You Need This?

**Real scenario**: You push code to GitHub. You forget to run tests. The code breaks production. Your team is frustrated. You wish there was a way to automatically check everything before it goes live.

CI/CD is exactly that - automatic checks and deployments that save you from yourself.

### Simple Analogy: Factory Assembly Line

Without CI/CD, you're hand-building each product:
- Build (hope it works)
- Test (if you remember)
- Ship (fingers crossed)

With CI/CD, you have a factory assembly line:
- Raw materials (code) go in
- Quality checks happen automatically
- Only good products (working code) ship out

### Another Analogy: Car Production Line

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Manual vs Automated Production Line               │
└─────────────────────────────────────────────────────────────────────┘

Manual Production (No CI/CD):
┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐
│Worker│ → │Worker│ → │Worker│ → │Worker│
│  1   │    │  2   │    │  3   │    │  4   │
│Code  │    │Test  │    │Build │    │Deploy│
└─────┘    └─────┘    └─────┘    └─────┘
    ↑          ↑          ↑          ↑
  Manual     Manual     Manual     Manual
(can err)  (can skip) (can fail) (risky!)

Automated Production Line (With CI/CD):
    Code Push
        │
        ▼
┌───────────────────────────────────────────────────┐
│                Automated Conveyor Belt             │
│  ┌────┐   ┌────┐   ┌────┐   ┌────┐   ┌────┐    │
│  │Lint│ → │Test│ → │Build│ → │Deploy│ → │Notify│ │
│  └────┘   └────┘   └────┘   └────┘   └────┘    │
│     ↓        ↓        ↓         ↓         ↓      │
│   Auto     Auto     Auto      Auto      Auto     │
│  100%     100%     100%     100%     100%    │
└───────────────────────────────────────────────────┘
```

---

## 💡 Tips for Non-Technical Readers

> **What does "CI/CD" mean?**
>
> - **CI (Continuous Integration)**: "Continuous Integration"
>   - Frequently merge code and automatically check each merge
>   - Analogy: Just like daily cleaning eliminates the need for deep cleaning, constant checking prevents big problems
>
> - **CD (Continuous Deployment)**: "Continuous Deployment"
>   - Automatically deploy when checks pass
>   - Analogy: Like products automatically shipping from a factory after passing quality inspection
>
> Simply put: **"A system that automatically checks and deploys when you push"**

---

## What is CI/CD?

### Basic Concept

```
┌─────────────────────────────────────────────────────────────────┐
│                        CI/CD Pipeline                            │
└─────────────────────────────────────────────────────────────────┘

     Code Push
         │
         ▼
┌──────────────┐
│     CI       │  ← Continuous Integration
│  (Auto Test) │
└──────┬───────┘
       │ Pass
       ▼
┌──────────────┐
│    Build     │
│              │
└──────┬───────┘
       │ Success
       ▼
┌──────────────┐
│     CD       │  ← Continuous Deployment
│ (Auto Deploy)│
└──────────────┘
```

- **CI**: Auto-test on code changes
- **CD**: Auto-deploy when tests pass

### Detailed CI/CD Pipeline Structure

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Complete CI/CD Pipeline Flow                      │
└─────────────────────────────────────────────────────────────────────┘

Developer pushes code
        │
        ▼
┌───────────────┐
│  1. Trigger   │  Detects code push/PR creation
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  2. Checkout  │  Download code
│               │
└───────┬───────┘
        │
        ├─────────────────┬─────────────────┐
        ▼                 ▼                 ▼
┌───────────────┐ ┌───────────────┐ ┌───────────────┐
│   3a. Lint    │ │  3b. Test     │ │ 3c. Security  │
│ (Code Check)  │ │  (Testing)    │ │   (Scan)      │
└───────┬───────┘ └───────┬───────┘ └───────┬───────┘
        │                 │                 │
        └─────────────────┴─────────────────┘
                          │
                          ▼
                    All passed?
                    /        \
                  Yes         No
                  │           │
                  ▼           ▼
        ┌───────────────┐  ┌───────────────┐
        │   4. Build    │  │    Notify     │
        │               │  │  (Failure)    │
        └───────┬───────┘  └───────────────┘
                │
                ▼
        ┌───────────────┐
        │  5. Deploy    │  main branch only
        │               │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │  6. Notify    │
        │               │
        └───────────────┘
```

---

## YAML Basics (For Beginners)

GitHub Actions use YAML format. Let's learn it quickly:

### What is YAML?

YAML is a human-readable format for configuration. It uses indentation (like Python) instead of brackets.

### JSON vs YAML Comparison

```json
// JSON
{
  "name": "John",
  "age": 30,
  "hobbies": ["reading", "coding"]
}
```

```yaml
# YAML - same data, cleaner look
name: John
age: 30
hobbies:
  - reading
  - coding
```

### Key YAML Rules

1. **Indentation matters!** (use 2 spaces, not tabs)
2. **Colons separate keys from values** `key: value`
3. **Dashes make lists** `- item`
4. **Comments start with #**

### Detailed YAML Syntax

```yaml
# ========== Basic Values ==========
string_value: "string"           # String
number_value: 42                 # Number
boolean_value: true              # Boolean (true/false)
null_value: null                 # null value

# ========== Lists ==========
# Method 1: Line break style
fruits:
  - apple
  - banana
  - orange

# Method 2: Inline style
fruits_inline: [apple, banana, orange]

# ========== Objects (Dictionaries) ==========
# Method 1: Line break style
person:
  name: John
  age: 30
  city: Seoul

# Method 2: Inline style
person_inline: {name: John, age: 30, city: Seoul}

# ========== Nested Structures ==========
company:
  name: TechCorp
  employees:
    - name: Alice
      role: Developer
    - name: Bob
      role: Designer
  location:
    country: Korea
    city: Seoul

# ========== Multi-line Strings ==========
# | : Preserve line breaks
description: |
  First line
  Second line
  Third line

# > : Convert line breaks to spaces
description_folded: >
  This sentence will be
  combined into one long line
```

### Common YAML Mistakes

```yaml
# ❌ BAD - inconsistent indentation
steps:
  - name: First
     run: echo "hi"  # <-- 3 spaces instead of 2!

# ✅ GOOD - consistent 2-space indentation
steps:
  - name: First
    run: echo "hi"

# ❌ BAD - missing space after colon
name:value

# ✅ GOOD - space after colon
name: value

# ❌ BAD - using tabs
jobs:
→test:  # tab character (invisible)

# ✅ GOOD - using spaces
jobs:
  test:  # 2 spaces
```

---

## 🔥 Pro Tip

> **Editor Settings for YAML**
>
> Useful VS Code settings when working with YAML:
>
> ```json
> // settings.json
> {
>   "[yaml]": {
>     "editor.tabSize": 2,
>     "editor.insertSpaces": true,
>     "editor.autoIndent": "full"
>   }
> }
> ```
>
> Recommended extensions:
> - **YAML** (Red Hat): Syntax checking, autocomplete
> - **GitHub Actions** (GitHub): Workflow-specific autocomplete

---

## Your First CI/CD (The Simplest Example)

Let's create the simplest possible CI workflow:

### Step 1: Create the Folder

```bash
mkdir -p .github/workflows
```

### Step 2: Create the Workflow File

Create `.github/workflows/hello.yml`:

```yaml
name: Hello CI

on: push

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, CI/CD!"
```

### Step 3: Push to GitHub

```bash
git add .github/workflows/hello.yml
git commit -m "Add first CI workflow"
git push
```

### Step 4: Check the Results

Go to your GitHub repository > Actions tab. You should see your workflow running!

That's it! You've created your first CI/CD pipeline. Now let's understand what each part means.

---

## Understanding the Workflow File

```yaml
name: Hello CI          # Name shown in GitHub UI

on: push                 # Trigger: when code is pushed

jobs:                    # List of jobs to run
  say-hello:             # Job name (you choose this)
    runs-on: ubuntu-latest  # What machine to run on
    steps:               # Steps in this job
      - name: Say Hello  # Step name (for logs)
        run: echo "Hello, CI/CD!"  # Command to run
```

### Detailed Workflow Components

```yaml
# ========== 1. Name ==========
name: My Workflow        # Name displayed in Actions tab

# ========== 2. Trigger (on) ==========
# Define when the workflow runs

# Single event
on: push

# Multiple events
on: [push, pull_request]

# Detailed configuration
on:
  push:
    branches:
      - main              # Only on main branch push
      - 'feature/*'       # Branches starting with feature/
    paths:
      - 'src/**'          # Only when src folder changes
      - '!src/**/*.md'    # Exclude md files
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * *'   # Run daily at midnight
  workflow_dispatch:       # Add manual run button

# ========== 3. Jobs ==========
jobs:
  # Job ID (lowercase, use hyphens/underscores)
  build-and-test:
    name: Build and Test  # Name displayed in UI
    runs-on: ubuntu-latest

    # Job order control
    needs: [lint]          # lint job must complete first

    # Conditional execution
    if: github.event_name == 'push'

    # Environment variables
    env:
      NODE_ENV: test

    # Steps
    steps:
      # Use an action
      - uses: actions/checkout@v4

      # Run a command
      - name: Run tests
        run: npm test

      # Multiple commands
      - name: Multiple commands
        run: |
          echo "First command"
          echo "Second command"
          npm install
          npm test

      # With environment variables
      - name: With env
        run: echo $MY_VAR
        env:
          MY_VAR: hello

# ========== 4. Environment Variables ==========
env:
  # Global environment variables
  CI: true

jobs:
  test:
    env:
      # Job-level environment variables
      NODE_ENV: test
    steps:
      - name: Step
        env:
          # Step-level environment variables
          API_KEY: ${{ secrets.API_KEY }}
```

---

## Commonly Used Trigger Patterns

| Trigger | Use Case | Example |
|---------|----------|---------|
| `push` | On code push | Run tests |
| `pull_request` | On PR creation/update | Automated code review |
| `schedule` | At scheduled times | Nightly builds, regular checks |
| `workflow_dispatch` | Manual execution | Deploy when needed |
| `release` | On release creation | Package deployment |

```yaml
# Combined trigger example
on:
  # When pushing to main or creating a PR
  push:
    branches: [main]
  pull_request:
    branches: [main]

  # Auto-run daily at 9 AM (KST)
  schedule:
    - cron: '0 0 * * *'  # UTC time

  # Manual execution option
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
```

---

## GitHub Actions Basics

### Workflow File Location

```
.github/
└── workflows/
    ├── ci.yml        # CI workflow
    ├── deploy.yml    # Deploy workflow
    └── review.yml    # Code review workflow
```

### Basic Structure

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

### Commonly Used Actions

| Action | Purpose | Example |
|--------|---------|---------|
| `actions/checkout@v4` | Code checkout | Required for almost all workflows |
| `actions/setup-node@v4` | Node.js setup | JavaScript/TypeScript projects |
| `actions/setup-python@v5` | Python setup | Python projects |
| `actions/cache@v4` | Save/restore cache | Speed up builds |
| `actions/upload-artifact@v4` | Upload files | Save build artifacts |
| `actions/download-artifact@v4` | Download files | Use artifacts from previous jobs |

---

## ⚠️ Caution

> **Security Notes for Workflow Writing**
>
> 1. **Avoid Exposing Secrets**
>    ```yaml
>    # ❌ Never do this!
>    - run: echo ${{ secrets.API_KEY }}  # Exposed in logs!
>
>    # ✅ Correct way
>    - run: my-command
>      env:
>        API_KEY: ${{ secrets.API_KEY }}  # Pass as environment variable
>    ```
>
> 2. **Be Careful with External Actions**
>    ```yaml
>    # ⚠️ Can be risky - auto-uses latest version
>    - uses: some-action@main
>
>    # ✅ Safe - pin to specific version
>    - uses: some-action@v2.1.0
>    ```
>
> 3. **Limit Secrets Access in PRs**
>    - Forked PRs cannot access Secrets by default
>    - Reason: Malicious code could steal Secrets

---

## Practical Workflow Examples

### 1. Basic CI Pipeline

```yaml
name: CI

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
```

### 2. Auto Deployment

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        run: |
          npm install -g vercel
          vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

### 3. PR Review Automation

```yaml
name: PR Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Review this PR for:
            - Potential bugs
            - Security vulnerabilities
            - Code quality
```

### 4. Testing in Multiple Environments (Matrix)

```yaml
name: Cross-platform Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
      fail-fast: false  # Continue other tests if one fails

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - run: npm ci
      - run: npm test
```

---

## Using Claude Code in CI

### Headless Mode

Run Claude in scripts with the `-p` flag:

```bash
# Basic usage
claude -p "Summarize this project"

# Allow specific tools only
claude -p "Analyze code" --allowedTools "Read,Glob,Grep"

# JSON output
claude -p "Extract function list" --output-format json
```

### Code Review in CI

```yaml
- name: Claude Code Review
  run: |
    # Get changed files
    CHANGED_FILES=$(git diff --name-only HEAD~1)

    # Review with Claude
    claude -p "Review these files: $CHANGED_FILES" \
      --allowedTools "Read,Glob,Grep"
```

### Auto Documentation

```yaml
- name: Generate Docs
  run: |
    claude -p "Generate documentation for functions in src/" \
      --allowedTools "Read,Write,Glob"

    git add docs/
    git commit -m "docs: auto-generated documentation"
    git push
```

### Claude Code CI Integration Workflow

```yaml
name: Claude Code Integration

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  claude-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Full history needed

      - name: Setup Claude Code
        run: |
          npm install -g @anthropic-ai/claude-code

      - name: Get changed files
        id: changed
        run: |
          FILES=$(git diff --name-only origin/main...HEAD)
          echo "files<<EOF" >> $GITHUB_OUTPUT
          echo "$FILES" >> $GITHUB_OUTPUT
          echo "EOF" >> $GITHUB_OUTPUT

      - name: Code Review with Claude
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Review the following changed files:
          ${{ steps.changed.outputs.files }}

          Review points:
          1. Potential bugs
          2. Performance issues
          3. Security vulnerabilities
          4. Code style" \
            --allowedTools "Read,Glob,Grep" \
            --output-format json > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));

            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: `## Claude Code Review\n\n${review.result}`
            });
```

---

## Building Production Pipelines

### Complete CI/CD Example

```yaml
name: Full Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'

jobs:
  # 1. Code Quality Check
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run lint
      - run: npm run type-check

  # 2. Testing
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm test -- --coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  # 3. Build
  build:
    needs: [quality, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run build

      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
          retention-days: 7

  # 4. Deploy (main branch only)
  deploy:
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    needs: build
    runs-on: ubuntu-latest
    environment: production  # Apply environment protection rules
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
          path: dist/

      - name: Deploy to Vercel
        run: |
          npm install -g vercel
          vercel deploy --prod --token=${{ secrets.VERCEL_TOKEN }}

  # 5. Notification
  notify:
    needs: deploy
    runs-on: ubuntu-latest
    if: always()  # Run regardless of success/failure
    steps:
      - name: Slack Notification
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ needs.deploy.result }}
          fields: repo,message,commit,author,action
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

---

## Real Conversation Examples with Claude

### Example 1: Creating CI Workflow

```
Me: "Create a CI pipeline for our Node.js project"

Claude: I'll create a CI pipeline for your Node.js project.

       [Analyzing project structure]

       Looking at package.json:
       - Using Node.js 20
       - Run tests with npm test
       - Run lint with npm run lint
       - Build with npm run build

       I've created the following workflow:

       📁 .github/workflows/ci.yml

       ```yaml
       name: CI

       on:
         push:
           branches: [main, develop]
         pull_request:
           branches: [main]

       jobs:
         lint:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm run lint

         test:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm test

         build:
           needs: [lint, test]
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm run build
       ```

       This workflow:
       1. Runs on push to main and develop branches
       2. Runs on PR creation
       3. lint and test run in parallel
       4. build runs after both pass
       5. npm caching speeds up builds

       Should I commit it?

Me: "Yes, commit it"

Claude: [Creating file and committing]

       ✅ Commit complete:

       "ci: Add GitHub Actions workflow for CI pipeline

        - Add lint, test, and build jobs
        - Enable npm caching for faster builds
        - Run on push to main/develop and PR to main"

       Now push to GitHub and you can see the results in the Actions tab.
```

### Example 2: Debugging Workflow

```
Me: "CI keeps failing. Can you see why?"

Claude: [Analyzing .github/workflows/ci.yml]

       I've checked the workflow file.
       I found several issues:

       ❌ Issue 1: Indentation error (line 15)
       ```yaml
       steps:
         - name: Install
          run: npm ci  # Missing 1 space!
       ```

       ❌ Issue 2: Missing checkout
       ```yaml
       jobs:
         test:
           runs-on: ubuntu-latest
           steps:
             # No checkout, can't access code!
             - run: npm test
       ```

       ❌ Issue 3: Node.js version mismatch
       ```yaml
       node-version: '18'  # But package.json requires 20
       ```

       Should I apply the fixes?

Me: "Yes, fix it"

Claude: [Editing file]

       Fixes complete:

       1. Fixed indentation
       2. Added checkout action
       3. Changed Node.js version to 20

       ```yaml
       jobs:
         test:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4  # Added
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'  # Fixed
             - name: Install
               run: npm ci  # Indentation fixed
             - run: npm test
       ```

       Should I commit and push?
```

### Example 3: Adding Deployment Automation

```
Me: "Set up auto-deployment to Vercel"

Claude: I'll add a Vercel auto-deployment workflow.

       First, I need to confirm:

       ⚠️ Required Secrets:
       - VERCEL_TOKEN: Vercel API token
       - VERCEL_ORG_ID: Organization ID
       - VERCEL_PROJECT_ID: Project ID

       How to set up Secrets:
       1. GitHub repository → Settings → Secrets → Actions
       2. Click "New repository secret"
       3. Add the three secrets above

       You can create a Vercel token at https://vercel.com/account/tokens

       Are the Secrets already set up?

Me: "Yes, all set up"

Claude: Great! Creating the deployment workflow.

       [Creating file: .github/workflows/deploy.yml]

       ```yaml
       name: Deploy

       on:
         push:
           branches: [main]

       jobs:
         deploy:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4

             - name: Setup Node.js
               uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'

             - name: Install dependencies
               run: npm ci

             - name: Build
               run: npm run build

             - name: Deploy to Vercel
               uses: amondnet/vercel-action@v25
               with:
                 vercel-token: ${{ secrets.VERCEL_TOKEN }}
                 vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
                 vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
                 vercel-args: '--prod'
       ```

       This workflow:
       - Only runs on push to main branch
       - Runs separately from CI workflow (lint, test)
       - Builds then auto-deploys to Vercel

       💡 Tip: Add `needs: [ci]` if you want
       deployment only after CI passes.

       Should I commit?
```

### Example 4: Error Scenario

```
Me: "Workflow runs but fails at npm ci"

Claude: Let me analyze the npm ci failure.

       [Requesting GitHub Actions logs]

       There are a few ways to check the logs:

       1. GitHub repository → Actions tab → Click failed workflow
       2. Check the failed step's logs

       Could you copy the error message?
       Or you can run `gh run view --log-failed`.

Me: "It says npm ERR! code ERESOLVE"

Claude: ERESOLVE error is a dependency conflict.

       🔍 Cause Analysis:
       - npm ci strictly follows package-lock.json
       - lock file not committed after npm install locally
       - Or lock file created with different npm version

       ✅ Solutions:

       Method 1: Regenerate package-lock.json (Recommended)
       ```bash
       rm package-lock.json
       npm install
       git add package-lock.json
       git commit -m "fix: Regenerate package-lock.json"
       git push
       ```

       Method 2: Use npm install instead of npm ci in CI (Not recommended)
       ```yaml
       # Instead of npm ci
       - run: npm install --legacy-peer-deps
       ```

       Should I do Method 1?

Me: "Do Method 1"

Claude: [Running commands]

       ```bash
       rm package-lock.json
       npm install
       ```

       ✅ package-lock.json has been regenerated.

       Changes:
       - Dependencies updated to latest
       - Conflicts resolved

       Should I commit and push?

Me: "Yes"

Claude: [Committing and pushing]

       ✅ Complete:

       "fix: Regenerate package-lock.json to resolve dependency conflicts"

       A new build will start in GitHub Actions.
       Check the Actions tab.

       Should I let you know the results in a few minutes?
```

---

## Security Management

### Secrets Setup

Set in GitHub Settings → Secrets and variables → Actions:

- `ANTHROPIC_API_KEY`: Claude API key
- `VERCEL_TOKEN`: Vercel deploy token
- `SLACK_WEBHOOK`: Slack notification URL

### Permission Restrictions

```yaml
- name: Read-only Claude
  run: |
    claude -p "Analyze code" \
      --allowedTools "Read,Glob,Grep"  # Exclude write tools
```

### Environment Protection Rules

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production  # Specify environment

    steps:
      - name: Deploy
        run: ./deploy.sh
```

Setting up environment protection rules in GitHub:
1. Settings → Environments → production
2. Add required reviewers (approval needed before deployment)
3. Set wait timer (e.g., 5 minute wait)

---

## Cost Optimization

### Process Only Changed Files

```yaml
- name: Get changed files
  id: changed
  run: |
    echo "files=$(git diff --name-only HEAD~1)" >> $GITHUB_OUTPUT

- name: Review only changed
  run: |
    claude -p "Review only these files: ${{ steps.changed.outputs.files }}"
```

### Use Caching

```yaml
# npm caching
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-${{ hashFiles('package-lock.json') }}
    restore-keys: npm-

# Built-in caching with setup-node
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'  # Auto caching!
```

### Conditional Execution

```yaml
jobs:
  # Run only when specific files change
  test-frontend:
    if: contains(github.event.head_commit.modified, 'frontend/')
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  # Deploy only on main branch
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh

  # Review only on PRs
  review:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - run: npm run review
```

### GitHub Actions Free Limits

| Account Type | Monthly Free Minutes |
|--------------|---------------------|
| Free | 2,000 minutes |
| Pro | 3,000 minutes |
| Team | 3,000 minutes |
| Enterprise | 50,000 minutes |

**Time-saving tips**:
- Use caching actively
- Disable unnecessary workflows
- Use parallel execution to reduce time
- Run only on needed branches

---

## Try It Yourself

### Exercise 1: Create Your First Workflow

1. Create the simplest workflow (see "Your First CI/CD" section above)
2. Push to GitHub
3. Watch it run in the Actions tab

### Exercise 2: Add Real Tests

Expand your workflow to run actual tests:

```yaml
name: Test

on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
```

### Exercise 3: Add Multiple Jobs

Create a workflow with lint and test running in parallel:

```yaml
name: CI

on: push

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test
```

---

## If Something Goes Wrong?

### Problem: Workflow not triggering

**Possible causes:**
1. YAML syntax error
2. Workflow file in wrong location
3. Branch name doesn't match

**Solutions:**
- Check YAML syntax with an online validator
- File must be in `.github/workflows/` folder
- Check the `on:` trigger matches your branch

### Problem: "Invalid workflow file"

**Possible causes:**
1. YAML indentation wrong
2. Missing required fields
3. Typo in action name

**Solutions:**
- Use exactly 2 spaces for indentation
- Every workflow needs: `name`, `on`, `jobs`
- Check action names are spelled correctly

### Problem: Tests pass locally but fail in CI

**Possible causes:**
1. Different Node/Python version
2. Missing environment variables
3. Different OS (your Mac vs Ubuntu)

**Solutions:**
- Match the version in your workflow to your local version
- Add environment variables to the workflow
- Test on the same OS as CI

### Problem: Secrets not working

**Possible causes:**
1. Secret name misspelled
2. Secret not added to repository
3. Wrong secret scope

**Solutions:**
- Check exact secret name in Settings > Secrets
- Add secret to the correct repository
- Use `${{ secrets.SECRET_NAME }}` format

### Troubleshooting Flowchart

```
Workflow not running
        │
        ▼
┌─────────────────┐
│ Check YAML      │ ──Has error──▶ Fix with online validator
│ syntax          │
└────────┬────────┘
         │ OK
         ▼
┌─────────────────┐
│ Check file      │ ──Wrong──▶ Move to .github/workflows/
│ location        │
└────────┬────────┘
         │ OK
         ▼
┌─────────────────┐
│ Check trigger   │ ──Mismatch──▶ Fix on: section
│ (branch name)   │
└────────┬────────┘
         │ Match
         ▼
┌─────────────────┐
│ Check logs in   │ ──Error──▶ Fix based on logs
│ Actions tab     │
└────────┬────────┘
         │ Success
         ▼
      Complete!
```

---

## Common Mistakes

### 1. Wrong Indentation

```yaml
# ❌ BAD - tabs instead of spaces
jobs:
→test:  # This is a tab!

# ✅ GOOD - 2 spaces
jobs:
  test:
```

### 2. Forgetting checkout

```yaml
# ❌ BAD - no checkout, can't access files
steps:
  - run: npm test

# ✅ GOOD - checkout first
steps:
  - uses: actions/checkout@v4
  - run: npm test
```

### 3. Exposing secrets in logs

```yaml
# ❌ BAD - prints secret to logs!
- run: echo ${{ secrets.API_KEY }}

# ✅ GOOD - use secret directly
- run: my-command
  env:
    API_KEY: ${{ secrets.API_KEY }}
```

### 4. Running on every push

```yaml
# ❌ BAD - runs on every branch
on: push

# ✅ BETTER - only main branch
on:
  push:
    branches: [main]
```

### 5. Not caching dependencies

```yaml
# ❌ SLOW - installs every time
steps:
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
  - run: npm ci

# ✅ FAST - uses caching
steps:
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
      cache: 'npm'  # Caching!
  - run: npm ci
```

### 6. Wrong needs order

```yaml
# ❌ BAD - depends on job that doesn't exist yet
jobs:
  build:
    needs: [test]  # test is defined below
    runs-on: ubuntu-latest
    steps: ...

  test:  # build waits for this but defined below
    runs-on: ubuntu-latest
    steps: ...

# ✅ GOOD - clear order
jobs:
  test:
    runs-on: ubuntu-latest
    steps: ...

  build:
    needs: [test]  # test is defined above
    runs-on: ubuntu-latest
    steps: ...
```

---

## 🎯 Mini Quiz

### Quiz 1: CI/CD Definition
What does CI stand for in CI/CD?

<details>
<summary>See Answer</summary>

**Answer**: Continuous Integration

**Explanation**: CI means frequently integrating code changes into the main branch and automatically running builds and tests each time. This allows problems to be discovered and resolved early.
</details>

### Quiz 2: Workflow File Location
Where should GitHub Actions workflow files be located?

<details>
<summary>See Answer</summary>

**Answer**: `.github/workflows/` folder

**Explanation**: GitHub Actions automatically recognizes `.yml` or `.yaml` files in the `.github/workflows/` folder of your repository. Workflows won't run if placed elsewhere.
</details>

### Quiz 3: Understanding Triggers
When will the following workflow run?

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

<details>
<summary>See Answer</summary>

**Answer**:
1. When pushing directly to main branch
2. When a PR targeting main branch is created or updated

**Explanation**: Both `push` and `pull_request` are set to `branches: [main]`, so it runs on both events related to the main branch.
</details>

### Quiz 4: Jobs Execution Order
When will the build job run in the following workflow?

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps: ...

  test:
    runs-on: ubuntu-latest
    steps: ...

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps: ...
```

<details>
<summary>See Answer</summary>

**Answer**: After both lint and test jobs complete successfully.

**Explanation**:
- `needs: [lint, test]` means build depends on lint and test
- lint and test run in parallel (no needs relationship between them)
- Both must succeed for build to run
- If either fails, build won't run
</details>

### Quiz 5: Using Secrets
Which is the correct way to use Secrets?

A) `run: echo ${{ secrets.API_KEY }}`
B) `run: curl -H "Auth: ${{ secrets.API_KEY }}"`
C) Pass as environment variable then use

<details>
<summary>See Answer</summary>

**Answer**: C) Pass as environment variable then use

**Explanation**:
- A exposes the secret directly in logs
- B also includes the secret in command line which can be logged
- C is safest:
  ```yaml
  - run: curl -H "Auth: $API_KEY"
    env:
      API_KEY: ${{ secrets.API_KEY }}
  ```
  GitHub automatically masks secrets values in logs, but passing as environment variables is the safest method.
</details>

---

## 📝 Practice Exercises

### Exercise 1: Basics - Create Hello CI [Difficulty: ★☆☆]

**Goal**: Write and run your first CI workflow

**Steps**:
1. Create `.github/workflows/hello.yml` file
2. Write a workflow that runs a simple echo command
3. Push to GitHub
4. Check results in Actions tab

**Template**:
```yaml
name: Hello CI

on: push

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello from CI!"
```

**Checklist**:
- [ ] Workflow runs automatically
- [ ] Green checkmark in Actions tab
- [ ] See "Hello from CI!" in logs

### Exercise 2: Intermediate - Test Automation [Difficulty: ★★☆]

**Goal**: Add test automation to a real project

**Steps**:
1. Add test workflow to Node.js project
2. Install dependencies with npm ci
3. Run tests with npm test
4. Add caching to improve speed

**Template**:
```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

**Checklist**:
- [ ] Tests run in CI
- [ ] Second run is faster due to caching
- [ ] Red X shows when tests fail

### Exercise 3: Advanced - Complete CI/CD Pipeline [Difficulty: ★★★]

**Goal**: Build lint → test → build → deploy pipeline

**Steps**:
1. Set up parallel lint and test jobs
2. Run build after both pass
3. Deploy only on main branch
4. Add Slack notification

**Template**:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist
      - name: Deploy
        run: echo "Deploying to production..."
        # Add actual deployment command
```

---

## 🏆 Challenge Tasks

### Challenge 1: Claude Code Review Automation

Create a workflow that automatically has Claude review code when a PR is created:

1. Trigger on PR creation
2. Extract list of changed files
3. Request review from Claude
4. Post results as PR comment

### Challenge 2: Matrix Build

Workflow that tests on multiple Node.js versions and OS:

1. Node.js 18, 20, 22
2. Ubuntu, Windows, macOS
3. Run tests on all combinations

### Challenge 3: Release Automation

Automatically on GitHub Release creation:

1. Generate release notes
2. Deploy npm package
3. Build and push Docker image
4. Send Slack notification

---

## Summary

What you learned in this chapter:
- [x] CI/CD concepts (auto-test, auto-deploy)
- [x] YAML syntax and writing
- [x] GitHub Actions basic structure
- [x] Practical workflow examples
- [x] Claude Code CI integration
- [x] Security and cost optimization

**Key point**: Automation provides continuous value once set up. CI/CD pipelines prevent mistakes and make deployment processes safe.

---

## Glossary

| Term | Description |
|------|-------------|
| **CI** | Continuous Integration. Auto build/test on code changes |
| **CD** | Continuous Deployment. Auto deploy when tests pass |
| **Pipeline** | Series of automated steps that code goes through |
| **Workflow** | File that defines automation process in GitHub Actions |
| **Job** | Individual work unit within a workflow |
| **Step** | Individual command or action within a job |
| **Trigger** | Event that starts a workflow |
| **Runner** | Server that executes the workflow (ubuntu-latest, etc.) |
| **Artifact** | Files generated by workflow (build outputs, etc.) |
| **Secret** | Variable that securely stores sensitive information |
| **Matrix** | Strategy to test across multiple environment combinations |
| **Cache** | Storing files to speed up builds |

---

## Next Chapter Preview

In Chapter 27, you'll learn about **Team Collaboration**. This is the final chapter of the curriculum!

- Claude Code utilization strategies for teams
- Sharing standardized configurations
- Setting up collaborative workflows
- Writing onboarding guides
- **Complete curriculum summary**
- **Next learning steps guide**

Now that you've learned automation with CI/CD, let's see how the entire team can effectively use Claude Code!

---

Proceed to [Chapter 27: Team Collaboration](../Chapter27-Team-Collaboration/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
