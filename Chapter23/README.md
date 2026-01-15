# Chapter 23: CI/CD Automation

**English** | [한국어](./README.ko.md)

## What You Will Learn

- CI/CD concepts and why they matter
- Building automation with GitHub Actions
- Integrating Claude Code into pipelines

---

## Why do you need this?

**Real-world scenario**: You push code to GitHub. You forget to run tests. The code breaks production. Your team is frustrated. You wish there was a way to automatically check everything before it goes live.

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

---

## YAML Basics (For Beginners)

GitHub Actions use YAML format. Here's a crash course:

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

### Common YAML Mistakes

```yaml
# BAD - inconsistent indentation
steps:
  - name: First
     run: echo "hi"  # <-- 3 spaces instead of 2!

# GOOD - consistent 2-space indentation
steps:
  - name: First
    run: echo "hi"

# BAD - missing space after colon
name:value

# GOOD - space after colon
name: value
```

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

---

## Why Learn CI/CD?

Manual work causes mistakes and wasted time:

```
Manual:
1. Modify code
2. Run tests (forgot)
3. Build (error occurs)
4. Fix again
5. Deploy (missing config)
...

Automated:
1. Modify code
2. Everything else is automatic!
```

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

jobs:
  # 1. Code Quality Check
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
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
          node-version: '20'
      - run: npm ci
      - run: npm test -- --coverage

  # 3. Build
  build:
    needs: [quality, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/

  # 4. Deploy (main branch only)
  deploy:
    if: github.ref == 'refs/heads/main'
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
      - run: vercel --prod --token=${{ secrets.VERCEL_TOKEN }}

  # 5. Notification
  notify:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Slack Notification
        run: |
          curl -X POST ${{ secrets.SLACK_WEBHOOK }} \
            -H 'Content-type: application/json' \
            -d '{"text": "Deployment complete!"}'
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
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: node_modules
    key: npm-${{ hashFiles('package-lock.json') }}
```

---

## Try it yourself

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

## If it doesn't work?

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

---

## Common mistakes

1. **Wrong indentation**
   ```yaml
   # BAD - tabs instead of spaces
   jobs:
   	test:  # This is a tab!

   # GOOD - 2 spaces
   jobs:
     test:
   ```

2. **Forgetting checkout**
   ```yaml
   # BAD - no checkout, can't access files
   steps:
     - run: npm test

   # GOOD - checkout first
   steps:
     - uses: actions/checkout@v4
     - run: npm test
   ```

3. **Exposing secrets in logs**
   ```yaml
   # BAD - prints secret to logs!
   - run: echo ${{ secrets.API_KEY }}

   # GOOD - use secret directly
   - run: my-command
     env:
       API_KEY: ${{ secrets.API_KEY }}
   ```

4. **Running on every push**
   ```yaml
   # BAD - runs on every branch
   on: push

   # BETTER - only main branch
   on:
     push:
       branches: [main]
   ```

5. **Not caching dependencies**
   - Without caching, `npm install` runs every time
   - Add caching to speed up workflows significantly

---

## Summary

What you learned in this chapter:
- [x] CI/CD concepts (auto-test, auto-deploy)
- [x] GitHub Actions basic structure
- [x] Practical workflow examples
- [x] Claude Code CI integration
- [x] Security and cost optimization

**Key point**: Automation provides value continuously once set up.

In the next chapter, you'll learn how to effectively use Claude Code in teams.

Proceed to [Chapter 24: Team Collaboration](../Chapter24/README.md).
