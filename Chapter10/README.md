# Chapter 10: CI/CD Integration

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, ensure you:
- [ ] Have completed Chapter 00-09
- [ ] Understand CI/CD concepts and pipelines
- [ ] Have a GitHub account with Actions enabled

---

## Introduction

The ultimate expression of Vibecoding is automation—having Claude work for you even when you're not at your desk. This chapter covers how to integrate Claude Code into CI/CD pipelines for automated code review, testing, and even code generation.

### Why CI/CD Integration?

- **Automated Code Review**: Every PR gets AI-reviewed
- **Continuous Quality**: Catch issues before they merge
- **Documentation**: Auto-generate docs on changes
- **Testing**: AI-assisted test generation and validation

---

## Topics

### 1. Headless Mode

Claude Code can run without human interaction using `-p` flag:

```bash
# Basic headless command
claude -p "Summarize this project"

# With specific tools
claude -p "Fix the bug in auth.ts" --allowedTools "Read,Edit,Bash"

# JSON output
claude -p "List all exported functions" --output-format json
```

#### Output Formats

| Format | Description |
|--------|-------------|
| `text` | Human-readable (default) |
| `json` | Structured JSON output |
| `stream-json` | Streaming JSON |

### 2. Claude Code Remote

Run Claude on the web and resume locally:

```bash
# Append "&" to run on web in background
> Refactor the authentication module &
# Claude runs on claude.ai/code in the background

# Resume the session locally with teleport
claude --teleport <session_id>
```

This is perfect for:
- Long-running tasks while you step away
- Switching between devices
- Offloading heavy computation to the cloud

### 3. Structured Output

Get predictable JSON output:

```bash
claude -p "Extract function names from utils.ts" \
  --output-format json \
  --json-schema '{
    "type": "object",
    "properties": {
      "functions": {
        "type": "array",
        "items": {"type": "string"}
      }
    }
  }'
```

### 4. GitHub Actions Integration

#### Using Official Action

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Review this PR for:
            1. Security vulnerabilities
            2. Performance issues
            3. Code style violations

            Provide actionable feedback.
```

#### Custom Headless Workflow

```yaml
# .github/workflows/claude-custom.yml
name: Claude Custom Tasks

on:
  push:
    branches: [main]
  pull_request:

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Analysis
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Analyze this codebase for potential bugs" \
            --allowedTools "Read,Grep,Glob" \
            --output-format json > analysis.json

      - name: Upload Results
        uses: actions/upload-artifact@v4
        with:
          name: claude-analysis
          path: analysis.json
```

### 5. Conversation Continuation

Continue previous sessions in CI:

```bash
# First run - save session
claude -p "Find all TODO comments" \
  --output-format json > step1.json

# Continue the conversation
claude -p "Now fix the most critical one" \
  --continue \
  --allowedTools "Read,Edit,Bash"
```

### 6. Tool Permissions in CI

Control what Claude can do:

```bash
# Read-only analysis
claude -p "Review code" --allowedTools "Read,Grep,Glob"

# With editing
claude -p "Fix issues" --allowedTools "Read,Edit,Write,Bash"

# Full access (use carefully)
claude -p "Complete this task" --dangerouslySkipPermissions
```

### 7. Common CI Patterns

#### Auto-Fix Linting Issues
```yaml
- name: Auto-fix with Claude
  run: |
    bun run lint 2>&1 || true > lint-errors.txt
    claude -p "Fix the linting errors in lint-errors.txt" \
      --allowedTools "Read,Edit"
```

#### Generate Release Notes
```yaml
- name: Generate Changelog
  run: |
    CHANGES=$(git log --oneline ${{ github.event.before }}..${{ github.sha }})
    claude -p "Generate release notes from these commits: $CHANGES" \
      --output-format json > release-notes.json
```

#### Security Scan
```yaml
- name: Security Review
  run: |
    claude -p "Scan for security vulnerabilities in src/" \
      --allowedTools "Read,Grep,Glob" \
      --output-format json > security-report.json
```

### 8. Error Handling

```yaml
- name: Claude Task with Error Handling
  continue-on-error: true
  run: |
    claude -p "Fix failing tests" \
      --allowedTools "Read,Edit,Bash" \
      --timeout 300000 2>&1 | tee claude-output.txt

    if grep -q "error" claude-output.txt; then
      echo "::warning::Claude encountered issues"
    fi
```

### 9. Best Practices for CI

1. **Limit Permissions**: Only grant necessary tools
2. **Set Timeouts**: Prevent runaway processes
3. **Cache Results**: Store outputs for review
4. **Review Changes**: Don't auto-merge AI changes
5. **Monitor Costs**: Track API usage

---

## Resources

- [Claude Code CI/CD Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Anthropic API Pricing](https://www.anthropic.com/pricing)

---

## Checklist

Answer these questions as if in an interview:

1. **What is headless mode and when would you use it?**
   <details>
   <summary>Hint</summary>
   Non-interactive mode with -p flag, used in CI/CD and automation
   </details>

2. **How do you control what tools Claude can use in CI?**
   <details>
   <summary>Hint</summary>
   --allowedTools flag with comma-separated list
   </details>

3. **What's the difference between text and json output formats?**
   <details>
   <summary>Hint</summary>
   Text: human-readable. JSON: structured, parseable by scripts
   </details>

4. **How can you continue a Claude conversation in CI?**
   <details>
   <summary>Hint</summary>
   --continue flag to continue previous session
   </details>

5. **What security considerations are important for CI integration?**
   <details>
   <summary>Hint</summary>
   Limit tools, protect API keys, review AI changes, set timeouts
   </details>

---

## Mini Project: Automated Development Pipeline

### Project Goals

Build a complete CI/CD pipeline with Claude Code automation by completing:

- [ ] Create PR review workflow that automatically reviews all PRs
- [ ] Create auto-fix pipeline that attempts to fix failed CI issues
- [ ] Create documentation generator that updates docs on API changes
- [ ] Create release notes workflow that generates changelog from commits
- [ ] Add 2+ optional workflows (test generator, security auditor, etc.)
- [ ] Configure proper secrets and error handling
- [ ] Test that all workflows execute successfully

### Ideas to Try

- Build a custom GitHub Action that wraps Claude Code
- Create GitLab CI or Jenkins equivalents of your workflows
- Implement cost optimization with caching strategies
- Build a dashboard to monitor CI Claude usage and costs

---

## Advanced

- [ ] Build a custom GitHub Action that wraps Claude Code
- [ ] Create GitLab CI/Jenkins equivalents
- [ ] Implement cost optimization with caching
- [ ] Build a dashboard for CI Claude usage
