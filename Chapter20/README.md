# Chapter 20: Hooks & Commands

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Creating automation triggers with Hooks
- Saving frequently used prompts with Commands
- Practical automation examples

---

## Why do you need this?

**Real-world scenario**: Every time you ask Claude to edit a file, you manually run `npm run lint` to check formatting. Every time. That's tedious!

Hooks and Commands automate the repetitive parts so you can focus on the creative work.

### Simple Analogy: Smart Home Automation

Think of Hooks like smart home rules:
- **"When I leave home"** (trigger) --> **"Turn off lights"** (action)
- **"When I open the door"** (trigger) --> **"Turn on AC"** (action)

In Claude Code:
- **"When Claude edits a file"** (trigger) --> **"Run linter"** (action)
- **"When I press enter"** (trigger) --> **"Add context"** (action)

Commands are like voice shortcuts: "Hey Siri, start my morning routine" = `/commit`

---

## Your First Hook (Start Here!)

Before diving into all the options, let's create one working hook:

### Step 1: Add to settings.json

Add this to your `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "echo 'File edited!' >> ~/.claude/hook-log.txt"
      }
    ]
  }
}
```

### Step 2: Test It

Ask Claude to edit any file:

```
> Add a comment to this file: test.js
```

### Step 3: Check the Log

```bash
cat ~/.claude/hook-log.txt
```

You should see "File edited!" - your hook worked!

### Step 4: Make It Useful

Now replace the echo with something practical:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      }
    ]
  }
}
```

Now every edited file gets auto-formatted!

---

## Why Learn Hooks and Commands?

Configuration alone has limits. Understanding Hooks and Commands gives you:

- **Eliminate repetition**: No need to type the same request every time
- **Workflow automation**: Auto-process before/after specific actions
- **Team standardization**: Whole team works the same way

---

## Hooks System

Hooks are code that automatically runs when specific events occur.

### Hook Types

```
┌─────────────────────────────────────────────────────────────────┐
│                      Hook Execution Points                       │
└─────────────────────────────────────────────────────────────────┘

 User Input
      │
      ▼
┌──────────────────┐
│ UserPromptSubmit │ ← When user presses enter
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   PreToolUse     │ ← Just before tool execution
└────────┬─────────┘
         │
         ▼
    [Tool Executes]
         │
         ▼
┌──────────────────┐
│   PostToolUse    │ ← Just after tool execution
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│      Stop        │ ← When response completes
└──────────────────┘
```

### Hook Configuration File

```json
// ~/.claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "echo 'Starting file edit: $FILE_PATH'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo 'Command execution complete'"
      }
    ]
  }
}
```

### Why Does This Matter?

**Auto lint check:**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm run lint -- --fix $FILE_PATH"
      }
    ]
  }
}
```

Lint runs automatically every time a file is edited.

**Auto testing:**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm test -- --related $FILE_PATH"
      }
    ]
  }
}
```

Only tests related to the modified file run automatically.

---

## Practical Hook Examples

### 1. File Backup

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "cp $FILE_PATH $FILE_PATH.backup"
      }
    ]
  }
}
```

Automatically creates backup before editing files.

### 2. Change Notification

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo 'New file created: $FILE_PATH' | tee -a ~/.claude/log.txt"
      }
    ]
  }
}
```

Logs file creation.

### 3. Security Check

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -q 'rm -rf'; then exit 1; fi"
      }
    ]
  }
}
```

Blocks dangerous commands before execution.

### 4. Auto Formatting

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      }
    ]
  }
}
```

Automatically formats files after editing.

---

## Commands System

Commands save frequently used prompts for reuse.

### Commands Folder Structure

```
.claude/
└── commands/
    ├── commit.md       # /commit command
    ├── review.md       # /review command
    └── test.md         # /test command
```

### Simple Command Example

```markdown
<!-- .claude/commands/commit.md -->
Analyze current changes and write a meaningful commit message.

- Check changes with git diff
- Identify change type (feat, fix, refactor, etc.)
- Write commit message
- Execute git commit
```

Usage:
```
> /commit
```

### Command with Variables

```markdown
<!-- .claude/commands/explain.md -->
Analyze the $ARGUMENTS code.

1. What this code does
2. Important logic
3. Improvements
```

Usage:
```
> /explain src/auth/login.ts
```

### Including Dynamic Information

```markdown
<!-- .claude/commands/status.md -->
Show the current project status.

Current branch: $(git branch --show-current)
Changed files: $(git status --short)

Based on this info:
1. Summarize current work status
2. Suggest next steps
```

Commands inside `$()` are executed and results are inserted.

---

## Practical Command Examples

### 1. Code Review Request

```markdown
<!-- .claude/commands/review.md -->
Review the $ARGUMENTS file.

Check for:
- [ ] Potential bugs
- [ ] Security vulnerabilities
- [ ] Performance issues
- [ ] Code style

Provide specific improvement suggestions.
```

```
> /review src/api/users.ts
```

### 2. Write Tests

```markdown
<!-- .claude/commands/test.md -->
Write tests for $ARGUMENTS.

Requirements:
- Use Jest
- Unit tests first
- Include edge cases
- Test file: *.test.ts
```

```
> /test src/utils/validation.ts
```

### 3. Documentation

```markdown
<!-- .claude/commands/docs.md -->
Write documentation for $ARGUMENTS.

Include:
- Function/class description
- Parameter descriptions
- Return values
- Usage examples

Add to code in JSDoc format.
```

```
> /docs src/services/auth.ts
```

### 4. Refactoring

```markdown
<!-- .claude/commands/refactor.md -->
Refactor $ARGUMENTS.

Principles:
- Improve readability
- Remove duplication
- Split functions (under 20 lines)
- Clear variable names

Show the plan before making changes.
```

```
> /refactor src/components/Dashboard.tsx
```

### 5. Issue → Implementation Workflow

A common pattern in real work. From issue to implementation in one go:

```markdown
<!-- .claude/commands/ticket.md -->
Handle issue #$ARGUMENTS.

## 1. Check Issue
$(gh issue view $ARGUMENTS)

## 2. Create Branch
Create a feature/$ARGUMENTS branch.

## 3. Implementation Plan
Analyze the issue and create an implementation plan.

## 4. Implement
Implement according to the plan.

## 5. Create PR
Create a PR with the changes.
```

```
> /ticket 42
```

One command handles: check issue → create branch → implement → create PR.

---

## Project-Specific Commands

### Frontend Project

```
.claude/
└── commands/
    ├── component.md   # Create component
    ├── hook.md        # Create custom hook
    ├── story.md       # Create Storybook story
    └── style.md       # Add styles
```

```markdown
<!-- .claude/commands/component.md -->
Create a React component named $ARGUMENTS.

Rules:
- Functional component
- TypeScript
- Tailwind CSS
- Define props types

File: src/components/$ARGUMENTS/$ARGUMENTS.tsx
```

### Backend Project

```
.claude/
└── commands/
    ├── endpoint.md    # Create API endpoint
    ├── migration.md   # DB migration
    ├── seed.md        # Seed data
    └── validate.md    # Add input validation
```

```markdown
<!-- .claude/commands/endpoint.md -->
Create CRUD API for $ARGUMENTS resource.

Structure:
- GET /$ARGUMENTS - List
- GET /$ARGUMENTS/:id - Detail
- POST /$ARGUMENTS - Create
- PATCH /$ARGUMENTS/:id - Update
- DELETE /$ARGUMENTS/:id - Delete

Also create the Prisma model.
```

---

## Combining Hooks + Commands

### Commit Workflow Automation

```json
// settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -q 'git commit'; then npm test; fi"
      }
    ]
  }
}
```

```markdown
<!-- .claude/commands/commit.md -->
Commit the changes.

1. Check changes with git diff
2. Run tests (auto-runs via Hook)
3. Write commit message
4. Execute commit
```

This way:
1. Run `/commit`
2. Hook auto-runs tests before commit
3. Commit proceeds if tests pass

### File Creation Workflow

```json
// settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "npx prettier --write $FILE_PATH && npx eslint --fix $FILE_PATH"
      }
    ]
  }
}
```

```markdown
<!-- .claude/commands/feature.md -->
Create the $ARGUMENTS feature.

After file creation, Hook automatically:
- Prettier formatting
- ESLint fixes

Trust this automation and write code.
```

---

## Sharing Commands with Team

### Commit to Git

```
my-project/
├── .claude/
│   └── commands/     # Team shared Commands
│       ├── commit.md
│       ├── review.md
│       └── deploy.md
├── CLAUDE.md
└── src/
```

Committing `.claude/commands/` to git lets the whole team use the same Commands.

### Document in README

```markdown
# Team Commands

## Available Commands

- `/commit` - Commit changes
- `/review <file>` - Code review
- `/deploy` - Deploy to staging
- `/hotfix <issue>` - Emergency fix
```

---

## Try it yourself

### Exercise 1: Create a Simple Hook

Create a hook that logs every time Claude runs a Bash command:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo 'Bash command executed' >> ~/.claude/bash-log.txt"
      }
    ]
  }
}
```

### Exercise 2: Create Your First Command

1. Create the commands folder: `mkdir -p .claude/commands`
2. Create a file `.claude/commands/hello.md`:

```markdown
Say hello and tell me what project I'm working on.
Look at the folder structure and give me a brief summary.
```

3. Use it: `> /hello`

### Exercise 3: Command with Variables

Create `.claude/commands/explain.md`:

```markdown
Explain the $ARGUMENTS file in simple terms.
What does it do? What are the key functions?
```

Use it: `> /explain src/index.ts`

---

## If it doesn't work?

### Problem: Hook not triggering

**Possible causes:**
1. Wrong matcher name (case-sensitive)
2. JSON syntax error
3. Hook added to wrong section

**Solutions:**
- Check matcher names: `Edit`, `Write`, `Bash`, `Read`, etc.
- Validate JSON: `cat ~/.claude/settings.json | jq .`
- Make sure hooks are inside `"hooks": { }` section

### Problem: Hook command fails silently

**Possible causes:**
1. Command not found
2. Permission denied
3. Wrong file path

**Solutions:**
- Test command manually in terminal first
- Check if command exists: `which npm`, `which npx`
- Use absolute paths when possible

### Problem: Command not found

**Possible causes:**
1. File not in `.claude/commands/` folder
2. File extension is not `.md`
3. Folder is in wrong location

**Solutions:**
- Check folder exists: `ls -la .claude/commands/`
- Make sure file ends with `.md`
- Commands folder should be in project root or `~/.claude/`

### Problem: $ARGUMENTS not working

**Possible causes:**
1. Using wrong variable name
2. Not providing arguments when calling

**Solutions:**
- Use exactly `$ARGUMENTS` (case-sensitive)
- Provide arguments: `/explain src/file.ts` not just `/explain`

---

## Common mistakes

1. **Wrong hook timing**
   - `PreToolUse`: Before the tool runs (can block it)
   - `PostToolUse`: After the tool runs (for follow-up actions)
   - Mixing these up causes unexpected behavior

2. **Forgetting to escape special characters**
   ```json
   // BAD - unescaped quotes
   "command": "echo "hello""

   // GOOD - use single quotes
   "command": "echo 'hello'"
   ```

3. **Commands that hang**
   - If your hook command waits for input, Claude Code will hang
   - Always use non-interactive commands

4. **Overcomplicating hooks**
   - Start simple, add complexity gradually
   - One hook doing one thing is easier to debug

5. **Not testing commands manually first**
   - Always run your command in terminal first
   - If it doesn't work there, it won't work in a hook

---

## Summary

What you learned in this chapter:
- [x] Hooks system (Pre/Post ToolUse, UserPromptSubmit, Stop)
- [x] Reusing prompts with Commands
- [x] Using variables and dynamic information
- [x] Combining Hooks + Commands
- [x] Sharing Commands with team

**Key point**: Automate repetitive work with Hooks and Commands.

In the next chapter, you'll learn more powerful extensions with Agents and Skills.

Proceed to [Chapter 21: Agents & Skills](../Chapter21/README.md).
