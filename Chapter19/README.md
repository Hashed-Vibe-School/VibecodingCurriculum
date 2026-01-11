# Chapter 19: Automation

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Extending Claude with MCP
- Auto-deployment with CI/CD
- Using headless mode

---

## What is MCP?

MCP (Model Context Protocol) is a way to add new capabilities to Claude.

### What MCP Can Do

- Connect directly to databases
- Send Slack/Discord messages
- Extend file system access
- Integrate external APIs

### Basic Concept

```
Claude Code ←→ MCP Server ←→ External Services
```

The MCP server connects Claude to external services.

---

## Setting Up MCP Servers

### Configuration File Location

`~/.claude/mcp_servers.json`:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/path/to/allowed/files"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db"
      }
    }
  }
}
```

### Commonly Used MCP Servers

| Server | Function |
|--------|----------|
| `mcp-server-filesystem` | File system access |
| `mcp-server-postgres` | PostgreSQL connection |
| `mcp-server-github` | GitHub API |
| `mcp-server-slack` | Slack messaging |

### Adding MCP Servers

```
> Tell me how to set up the GitHub MCP server
```

---

## Practice: Connect Database with MCP

### 1. PostgreSQL MCP Setup

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/mydb"
      }
    }
  }
}
```

### 2. Using in Claude

```
> Get user list from database

> Add new user to users table
> Name: John Doe, Email: john@email.com
```

Claude can directly query and modify the database.

---

## What is CI/CD?

CI/CD is a system for automatically testing and deploying code.

- **CI** (Continuous Integration): Auto-test on code changes
- **CD** (Continuous Deployment): Auto-deploy when tests pass

### Why Do We Need It?

Manually:
1. Modify code
2. Run tests
3. Build
4. Deploy
5. Verify

With CI/CD:
1. Modify code
2. Everything else is automatic

---

## CI/CD with GitHub Actions

### Basic Workflow

`.github/workflows/deploy.yml`:

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
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        run: vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

### Creating Workflow with Claude

```
> Create a GitHub Actions workflow for this project.
> Test and deploy to Vercel when pushed to main.
```

---

## Using Claude Code in CI

### Headless Mode

Use the `-p` flag to run Claude in scripts:

```bash
# Basic usage
claude -p "Summarize this project"

# Allow only specific tools
claude -p "Fix the bug" --allowedTools "Read,Edit"

# JSON output
claude -p "Extract function list" --output-format json
```

### Automate Code Review in CI

```yaml
name: Claude Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Please review this PR for:
            - Potential bugs
            - Security vulnerabilities
            - Code quality
```

### Auto-fix Lint Errors

```yaml
- name: Auto-fix with Claude
  run: |
    npm run lint 2>&1 || true > lint-errors.txt
    claude -p "Fix the lint errors in lint-errors.txt" \
      --allowedTools "Read,Edit"
```

---

## Auto-Deploy Pipeline

### Full Automation Example

```yaml
name: Full Pipeline

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: vercel --prod

  notify:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Notify Slack
        run: |
          curl -X POST $SLACK_WEBHOOK \
            -d '{"text": "Deployment complete!"}'
```

---

## Security Considerations

### Managing Secrets

```yaml
# Bad example
env:
  API_KEY: "sk-1234567890"  # Never do this!

# Good example
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Manage in GitHub Settings → Secrets.

### Limiting Permissions

```yaml
- name: Claude with limited permissions
  run: |
    claude -p "Analyze this" \
      --allowedTools "Read,Glob,Grep"  # Read-only
```

---

## Cost Optimization

### Process Only Changed Files

```yaml
- name: Get changed files
  id: changed
  run: |
    echo "files=$(git diff --name-only HEAD~1)" >> $GITHUB_OUTPUT

- name: Review only changed files
  run: |
    claude -p "Review only these files: ${{ steps.changed.outputs.files }}"
```

### Use Caching

```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: node_modules
    key: ${{ hashFiles('package-lock.json') }}
```

---

## Practice: CI/CD Pipeline

### Basic Tasks

```
# 1. Set up GitHub Actions
> Create a CI/CD workflow for this project

# 2. Auto testing
> Run tests on every push

# 3. Auto deployment
> Auto-deploy to Vercel when merged to main
```

### Extra Challenges

```
> Add a workflow that auto-reviews code when PR is opened

> Send Slack notification on deploy success/failure

> Auto-check for dependency updates daily
```

---

## Mini Project: Automation Pipeline

Build a fully automated development pipeline.

### Goals

- Automate entire development cycle
- Production-level CI/CD experience

### Build It

```
> Create a complete CI/CD pipeline.
> - Auto-test when PR is opened
> - Claude code review
> - Auto-deploy when merged to main
> - Slack notification
```

### Additional Automation Ideas

```
> Daily automated dependency vulnerability scan

> Weekly code quality report generation

> Automatic release notes writing
```

### Advanced Challenges (For Experts)

```
> Set up canary deployment (only some traffic to new version)

> Set up automatic rollback (auto-rollback on high error rate)

> Multi-environment deployment (dev → staging → prod)

> Integrate feature flag system
```

---

## Advanced: Using AI as Part of Your System

So far, we have used Claude Code interactively. But the real power comes when it becomes **part of an automated system**.

### One-off Tasks vs Systems

| One-off | System |
|---------|--------|
| "Fix this bug" | Auto code review on every PR |
| "Write a commit message" | Meaningful messages generated on every commit |
| "Write tests" | Tests auto-supplemented on code changes |

Using headless mode (`-p` flag), you can integrate Claude into scripts and CI pipelines.

### Building Feedback Loops

The real value of automation is that **improvements accumulate**.

1. Claude performs code review
2. Discovers the same pattern of mistakes repeating
3. Add that rule to CLAUDE.md
4. Future reviews catch that mistake proactively

The system gets smarter over time. Quality improves just by refining prompts and settings, without code changes.

### Eliminating Repetitive Work with MCP

If you are constantly copying the same information and pasting it to Claude, you can automate it with MCP servers.

For example:
- Fetching task details from issue tracker
- Checking database schema
- Sharing results to Slack channel

When these tasks are automatically connected, you can focus only on the truly important decisions.

### Growing as a Developer

To use Claude Code effectively, you ultimately need **good development habits**:

- Defining requirements clearly
- Breaking work into appropriate sizes
- Verifying results and providing feedback
- Automating repeating patterns

AI tools let you execute these habits faster—they do not replace the habits themselves. As you work with AI and develop these capabilities, you grow alongside the tools as they evolve.

---

## Summary

What you learned in this chapter:
- [x] Extending Claude with MCP
- [x] CI/CD concepts and setup
- [x] Using GitHub Actions
- [x] Headless mode
- [x] Auto-deploy pipelines
- [x] Using AI as part of systems

In the next chapter, we bring together everything you have learned.

[Chapter 20: Mastery](../Chapter20/README.md)
