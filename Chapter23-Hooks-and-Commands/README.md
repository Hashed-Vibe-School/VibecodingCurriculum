# Chapter 23: Hooks & Commands

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while studying, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn in This Chapter

- Creating automation triggers with Hooks
- Saving frequently used prompts with Commands
- Practical automation examples and best practices
- Building powerful workflows by combining Hooks + Commands

---

## Connection to Previous Chapter

In Chapter 22, we learned how to configure Claude's basic behavior with CLAUDE.md and settings.json. In this chapter, we take it a step further and enter the world of **automation**.

If configuration is "rules", then Hooks and Commands are "automatically executed actions".

> **Beginner Tip**: Hooks are "when ~ happens, automatically do ~" and Commands are "when I type /command, do ~". Both are magic tools that reduce repetitive work!

---

## Why Do You Need This?

**Real-world scenario**: Every time you ask Claude to edit a file, you manually run `npm run lint` to check formatting. Repeating this every time gets tiring.

Hooks and Commands automate the repetitive parts so you can focus on creative work.

### Real-World Applications in the Industry

| Situation | Without Hooks/Commands | With Them |
|-----------|------------------------|-----------|
| Lint after file edit | Manual execution every time | Auto execution |
| Writing commit messages | Remember format and type | Just `/commit` once |
| PR review request | Copy-paste checklist | Just `/review` once |
| Writing tests | Find and copy template | `/test filename` |

### Simple Analogy: Smart Home Automation

Think of Hooks like smart home rules:
- **"When I leave home"** (trigger) --> **"Turn off lights"** (action)
- **"When I open the door"** (trigger) --> **"Turn on AC"** (action)

In Claude Code:
- **"When Claude edits a file"** (trigger) --> **"Run linter"** (action)
- **"When I press enter"** (trigger) --> **"Add context"** (action)

Commands are like voice shortcuts: "Hey Siri, start my morning routine" = `/commit`

> **Pro Tip**: If you automate a task you do 10 times a day, you can eliminate 300 repetitions per month!

---

## Common Traps Beginners Fall Into

### Trap 1: "Automation is an advanced feature"

Many people postpone thinking "I'll do it later when I'm more familiar." But:
- One simple Hook saves 5 minutes daily
- One Command prevents typos and mistakes
- Setting it up from the start builds good habits

> **Warning**: Don't start with complex automation. Start with one simple task you do most frequently!

### Trap 2: "Too many Hooks, I don't know what's what"

If you create too many Hooks:
- Hard to track which Hook is executing
- Possible conflicts between Hooks
- Debugging becomes complex

### Trap 3: "Trying to put everything in one Command"

If you put too many features in a single Command:
- Less flexible
- Hard to modify
- Can't be reused

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

> **Beginner Tip**: `$FILE_PATH` is a "variable" that automatically inserts the path of the file Claude edited. It's like cell references in Excel!

---

## Why Learn Hooks and Commands?

Configuration alone has limits. Understanding Hooks and Commands gives you:

- **Eliminate repetition**: No need to type the same request every time
- **Workflow automation**: Auto-process before/after specific actions
- **Team standardization**: Whole team works the same way

### ROI of Automation (Return on Investment)

| Investment Time | Automation Target | Savings Effect |
|-----------------|-------------------|----------------|
| 5 min | Lint after file save | 20 times/day x 30 sec = 10 min/day |
| 10 min | Commit message writing | 5 times/day x 2 min = 10 min/day |
| 15 min | Test templates | 3 times/day x 5 min = 15 min/day |

---

## Hooks System Detailed Explanation

Hooks are code that automatically runs when specific events occur.

### Hook Types

Below is the execution flow of the most commonly used hooks:

```
┌─────────────────────────────────────────────────────────────────┐
│                   Main Hook Execution Points                    │
└─────────────────────────────────────────────────────────────────┘

 User Input
      │
      ▼
┌──────────────────┐
│ UserPromptSubmit │ ← When user presses enter
└────────┬─────────┘   (input preprocessing, context addition)
         │
         ▼
┌──────────────────┐
│   PreToolUse     │ ← Just before tool execution
└────────┬─────────┘   (validation, backup, logging)
         │
         ▼
    [Tool Executes]
         │
         ▼
┌──────────────────┐
│   PostToolUse    │ ← Just after tool execution
└────────┬─────────┘   (formatting, testing, notification)
         │
         ▼
┌──────────────────┐
│      Stop        │ ← When response completes
└──────────────────┘   (cleanup, summary, logging)
```

### Complete Hook Event List

Claude Code supports a total of 12 hook events:

| Hook Event | Description | Uses Matcher | Main Use |
|------------|-------------|--------------|----------|
| `PreToolUse` | Just before tool execution | Yes | Backup, validation, blocking |
| `PostToolUse` | Just after successful tool execution | Yes | Formatting, testing, logging |
| `PostToolUseFailure` | When tool execution fails | Yes | Error logging, notification |
| `PermissionRequest` | When permission dialog is shown | Yes | Auto-approval, logging |
| `UserPromptSubmit` | When user submits a prompt | No | Input transformation, context addition |
| `Notification` | When Claude Code sends a notification | No | External notification integration |
| `Stop` | When Claude Code response completes | No | Summary, cleanup, logging |
| `SessionStart` | When session starts or resumes | No | Initialization, environment setup |
| `SessionEnd` | When session ends | No | Cleanup, save, logging |
| `SubagentStart` | When subagent starts | No | Monitoring, logging |
| `SubagentStop` | When subagent completes | No | Result processing, logging |
| `PreCompact` | Just before context compaction | No | Save important information |

> **Note**: Matcher is a pattern for matching tool names. It is only used with `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, and `PermissionRequest`.

### Hook Configuration File Details

```json
// ~/.claude/settings.json
{
  "hooks": {
    // Pre-tool execution Hook
    "PreToolUse": [
      {
        "matcher": "Edit",           // Apply only to Edit tool
        "command": "cp $FILE_PATH $FILE_PATH.backup"  // Create backup
      },
      {
        "matcher": "Bash",           // Apply only to Bash tool
        "command": "echo 'Command executing: $COMMAND' >> ~/.claude/cmd-log.txt"
      }
    ],

    // Post-tool execution Hook
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH && npx eslint --fix $FILE_PATH"
      },
      {
        "matcher": "Write",
        "command": "echo 'New file created: $FILE_PATH' >> ~/.claude/file-log.txt"
      }
    ],

    // User input Hook
    "UserPromptSubmit": [
      {
        "command": "echo '$(date): New request started' >> ~/.claude/session-log.txt"
      }
    ],

    // Response completion Hook
    "Stop": [
      {
        "command": "echo '$(date): Response completed' >> ~/.claude/session-log.txt"
      }
    ]
  }
}
```

### Available Variables

| Variable | Description | Available in Hooks |
|----------|-------------|-------------------|
| `$FILE_PATH` | File path | Edit, Write, Read |
| `$COMMAND` | Executed command | Bash |
| `$TOOL_NAME` | Tool name | All tool-related Hooks |
| `$EXIT_CODE` | Exit code | PostToolUse, PostToolUseFailure |

> **Pro Tip**: You can use `$()` syntax in hook commands to insert shell command results. Examples: `$(date)`, `$(git branch --show-current)`

---

## Practical Hook Examples

### 1. File Backup (Safety Net)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "cp $FILE_PATH $FILE_PATH.backup.$(date +%Y%m%d_%H%M%S)"
      }
    ]
  }
}
```

Creates a backup with timestamp before editing files.

### Real Conversation Example: When Backup Helped

```
User: Refactor this function

Claude: I'll refactor the calculateTotal function.
[File edit executed - Hook automatically creates backup]

User: Ah, the original version was better. Revert it

Claude: There's a backup file. I'll restore it.
utils.ts.backup.20240115_143022 → utils.ts
Restore complete!
```

### 2. Auto Lint and Formatting

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      },
      {
        "matcher": "Edit",
        "command": "npx eslint --fix $FILE_PATH"
      }
    ]
  }
}
```

> **Beginner Tip**: Prettier prettifies code alignment, and ESLint catches syntax errors. Running them automatically keeps your code always clean!

### 3. Change Logging

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '$(date): New file created - $FILE_PATH' | tee -a ~/.claude/log.txt"
      },
      {
        "matcher": "Edit",
        "command": "echo '$(date): File edited - $FILE_PATH' | tee -a ~/.claude/log.txt"
      }
    ]
  }
}
```

Records all file changes to a log.

### 4. Security Check

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -qE 'rm -rf|DROP|DELETE FROM'; then echo 'BLOCKED: Dangerous command' && exit 1; fi"
      }
    ]
  }
}
```

Blocks dangerous commands before execution.

### 5. Auto Test Execution

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "if [[ $FILE_PATH == *.test.* ]]; then npm test -- --testPathPattern=$(basename $FILE_PATH); fi"
      }
    ]
  }
}
```

When a test file is modified, only that test runs automatically.

### 6. Slack Notification

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "curl -X POST $SLACK_WEBHOOK -H 'Content-type: application/json' -d '{\"text\": \"Claude task completed!\"}'"
      }
    ]
  }
}
```

Sends a Slack notification when a long task finishes.

---

## Mini Quiz: Understanding Hooks

**Question**: What is the correct use of a `PreToolUse` Hook?

A) Running formatting after file edit
B) Creating backup before file edit
C) Logging after response completion

<details>
<summary>View Answer</summary>

**Answer: B**

- `PreToolUse` runs **before** tool execution
- Used for backup, validation, blocking
- A is for `PostToolUse`, C is for `Stop` Hook

</details>

---

## Commands System Detailed Explanation

Commands save frequently used prompts for reuse.

### Commands Folder Structure

```
.claude/
└── commands/
    ├── commit.md       # /commit command
    ├── review.md       # /review command
    ├── test.md         # /test command
    ├── explain.md      # /explain command
    └── refactor.md     # /refactor command
```

> **Beginner Tip**: When you create a `.md` file in the `.claude/commands/` folder, the filename becomes the command. `review.md` → `/review`

### Simple Command Example

```markdown
<!-- .claude/commands/commit.md -->
Analyze current changes and write a meaningful commit message.

## Steps
1. Check changes with git diff
2. Identify change type (feat, fix, refactor, docs, test, etc.)
3. Write commit message in English
4. Execute git commit

## Commit Message Format
type: brief description

- Detailed change 1
- Detailed change 2
```

Usage:
```
> /commit
```

### Command with Variables

```markdown
<!-- .claude/commands/explain.md -->
Analyze the $ARGUMENTS code.

## Analysis Items
1. What this code does (one sentence summary)
2. Main functions/classes explanation
3. Data flow
4. Points that could be improved

## Output Format
### Summary
[One sentence explanation]

### Detailed Analysis
[Function-by-function explanation]

### Improvement Suggestions
[Suggestions if any]
```

Usage:
```
> /explain src/auth/login.ts
```

`$ARGUMENTS` gets replaced with `src/auth/login.ts`.

### Including Dynamic Information

```markdown
<!-- .claude/commands/status.md -->
Show the current project status.

## Current Information
- Branch: $(git branch --show-current)
- Changed files: $(git status --short)
- Recent commit: $(git log -1 --oneline)

## Requests
Based on the above information:
1. Summarize current work status
2. Identify incomplete work
3. Suggest next tasks
```

Commands inside `$()` are executed and results are inserted.

### Real Conversation Example: Using Commands

```
User: /status

Claude: I'll analyze the current project status.

## Current Status
- Branch: feature/user-auth
- Changed files: 3 (M src/auth/login.ts, A src/auth/signup.ts, M tests/auth.test.ts)
- Recent commit: abc1234 feat: Add login form UI

## Analysis
Currently developing user authentication features.
Login and signup features are in progress,
and test files are being modified together.

## Suggested Next Steps
1. Complete tests for current changes
2. Create PR for code review
3. Add signup validation
```

---

## Practical Command Examples

### 1. Code Review Request

```markdown
<!-- .claude/commands/review.md -->
Review the $ARGUMENTS file.

## Review Checklist
- [ ] Bug potential
- [ ] Security vulnerabilities
- [ ] Performance issues
- [ ] Code style (team conventions)
- [ ] Test coverage
- [ ] Error handling

## Output Format

### Summary
[Overall code quality assessment]

### Issues Found
| Severity | Location | Issue | Suggestion |
|----------|----------|-------|------------|
| ... | ... | ... | ... |

### Positives
[Things done well]

### Improvement Suggestions
[Refactoring ideas]
```

```
> /review src/api/users.ts
```

### 2. Writing Tests

```markdown
<!-- .claude/commands/test.md -->
Write tests for $ARGUMENTS.

## Test Requirements
- Use Jest
- Unit tests first
- Include edge cases (null, undefined, empty values, boundary values)
- Test file: *.test.ts

## Test Structure
```typescript
describe('Function/ClassName', () => {
  describe('methodName', () => {
    it('normal case description', () => {});
    it('error case description', () => {});
    it('edge case description', () => {});
  });
});
```

## Mocking Rules
- External APIs: Use MSW
- Database: in-memory mock
- Time: jest.useFakeTimers()
```

```
> /test src/utils/validation.ts
```

### 3. Documentation

```markdown
<!-- .claude/commands/docs.md -->
Write documentation for $ARGUMENTS.

## Documentation Contents
- Function/class description
- Parameter descriptions (type, default value, required)
- Return values
- Exception cases
- Usage examples (minimum 2)

## Format
Add directly to code in JSDoc or TSDoc format

## Example
/**
 * Retrieves user information.
 *
 * @param userId - ID of the user to retrieve
 * @returns User information object
 * @throws {NotFoundError} When user cannot be found
 *
 * @example
 * const user = await getUser('user123');
 * console.log(user.name);
 */
```

```
> /docs src/services/auth.ts
```

### 4. Refactoring

```markdown
<!-- .claude/commands/refactor.md -->
Refactor $ARGUMENTS.

## Refactoring Principles
- Keep behavior the same
- Improve readability
- Remove duplication
- Split functions (20 lines or less)
- Clear variable names

## Process
1. Analyze current code and identify problems
2. Present refactoring plan
3. Proceed after user approval
4. Verify tests pass at each step

## Cautions
- Don't change too much at once
- Adding features is not refactoring
- Explain reasons for changes
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

## 2. Work Plan
Analyze the issue content and create an implementation plan.
- Expected work time
- List of files to modify
- Test plan

## 3. Create Branch
Create a feature/$ARGUMENTS branch.

## 4. Implementation
Implement once the plan is approved.

## 5. Testing
Run related tests and verify they pass.

## 6. Create PR
Create a PR with the changes.
Link to issue: Closes #$ARGUMENTS
```

```
> /ticket 42
```

One command handles: check issue → create branch → implement → create PR.

### 6. Bug Debugging

```markdown
<!-- .claude/commands/debug.md -->
Debug the following error:

$ARGUMENTS

## Debugging Steps
1. Analyze error message
2. Check stack trace
3. Find related code
4. Identify cause
5. Present solution
6. Prevention measures

## Output Format
### Error Summary
[What error is it]

### Cause
[Why it occurred]

### Solution
[How to fix it - include code]

### Prevention
[How to prevent it in the future]
```

```
> /debug "TypeError: Cannot read property 'map' of undefined at UserList.tsx:15"
```

---

## Project-Specific Commands

### Frontend Project

```
.claude/
└── commands/
    ├── component.md   # Create component
    ├── hook.md        # Create custom hook
    ├── story.md       # Create Storybook story
    ├── style.md       # Add styles
    └── page.md        # Create page
```

```markdown
<!-- .claude/commands/component.md -->
Create a React component named $ARGUMENTS.

## Rules
- Functional component (arrow function)
- TypeScript strict mode
- Tailwind CSS
- Define props type with interface

## File Structure
src/components/$ARGUMENTS/
├── $ARGUMENTS.tsx        # Component
├── $ARGUMENTS.test.tsx   # Test
├── $ARGUMENTS.stories.tsx # Storybook
└── index.ts              # export

## Component Template
```tsx
interface ${ARGUMENTS}Props {
  // props definition
}

export const $ARGUMENTS = ({ }: ${ARGUMENTS}Props) => {
  return (
    <div>
      {/* content */}
    </div>
  );
};
```
```

### Backend Project

```
.claude/
└── commands/
    ├── endpoint.md    # Create API endpoint
    ├── migration.md   # DB migration
    ├── seed.md        # Seed data
    ├── validate.md    # Add input validation
    └── middleware.md  # Create middleware
```

```markdown
<!-- .claude/commands/endpoint.md -->
Create CRUD API for $ARGUMENTS resource.

## Endpoint Structure
- GET /$ARGUMENTS - List (with pagination)
- GET /$ARGUMENTS/:id - Detail
- POST /$ARGUMENTS - Create
- PATCH /$ARGUMENTS/:id - Update
- DELETE /$ARGUMENTS/:id - Delete

## Files to Create
- src/controllers/$ARGUMENTS.controller.ts
- src/services/$ARGUMENTS.service.ts
- src/repositories/$ARGUMENTS.repository.ts
- src/dto/$ARGUMENTS.dto.ts
- src/entities/$ARGUMENTS.entity.ts

## Requirements
- Input validation (zod)
- Error handling
- Pagination
- Also create Prisma model
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

## Steps
1. Check changes with git diff
2. Run tests (auto-runs via Hook)
3. Write commit message (Conventional Commits)
4. Execute commit

## Commit Message Format
type(scope): description

- type: feat, fix, docs, style, refactor, test, chore
- scope: optional, scope of change
- description: present tense, in English
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

## Process
1. Analyze and design the feature
2. Create necessary files (auto-formatted by Hook)
3. Write tests
4. Update documentation

## Automation
After file creation, Hook automatically:
- Prettier formatting
- ESLint fixes
```

### Real Conversation Example: Using Combination

```
User: /feature user-profile

Claude: I'll create the user-profile feature.

## Design
- User profile view/edit functionality
- Profile image upload
- Password change

## Files to Create
1. src/features/user-profile/
   ├── UserProfile.tsx
   ├── EditProfile.tsx
   ├── ChangePassword.tsx
   └── index.ts

Starting creation...

[File created: UserProfile.tsx]
[Hook: prettier execution complete]
[Hook: eslint execution complete]

[File created: EditProfile.tsx]
[Hook: prettier execution complete]
[Hook: eslint execution complete]

All files have been created and automatically formatted.
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
│       ├── deploy.md
│       └── hotfix.md
├── CLAUDE.md
└── src/
```

Committing `.claude/commands/` to git lets the whole team use the same Commands.

### Document in README

```markdown
# Team Commands

## Available Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/commit` | Commit changes | `/commit` |
| `/review <file>` | Code review | `/review src/api.ts` |
| `/test <file>` | Write tests | `/test src/utils.ts` |
| `/deploy` | Deploy to staging | `/deploy` |
| `/hotfix <issue>` | Emergency fix | `/hotfix 123` |

## Usage
Enter the above commands in Claude Code.

## Adding Custom Commands
1. Create a `.md` file in `.claude/commands/` folder
2. Write the prompt in the file content
3. Commit to Git
```

---

## Practice Exercises

### Exercise 1: Create a Simple Hook (Beginner)

Create a hook that logs every time Claude runs a Bash command:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo '$(date): Bash command executed' >> ~/.claude/bash-log.txt"
      }
    ]
  }
}
```

**Test**: Ask Claude to run `ls` command and check the log file

### Exercise 2: Create Your First Command (Beginner)

1. Create the commands folder: `mkdir -p .claude/commands`
2. Create a file `.claude/commands/hello.md`:

```markdown
Say hello and tell me what project I'm working on.
Look at the folder structure and give me a brief summary.
```

3. Use it: `> /hello`

### Exercise 3: Command with Variables (Intermediate)

Create `.claude/commands/explain.md`:

```markdown
Explain the $ARGUMENTS file in simple terms.

## Explanation Items
1. Purpose of this file
2. Main functions/classes
3. Relationship with other files
4. Points that could be improved
```

Use it: `> /explain src/index.ts`

### Exercise 4: Hook + Command Combination (Advanced)

1. Hook: Auto backup on file modification
2. Command: View backup list
3. Command: Restore to specific backup

---

## Challenge Tasks

### Challenge 1: Complete Development Workflow

Create a system that automates all of the following:
- Issue start (`/issue-start`)
- Backup during development (Hook)
- Auto test execution (Hook)
- PR creation (`/pr`)

### Challenge 2: Team Onboarding Command

Create a `/onboarding` command that teaches a new team member everything about the project.

### Challenge 3: Notification System

Implement a Hook that sends Slack/Discord notifications when long tasks complete.

---

## Troubleshooting

### Problem: Hook is not executing

**Possible causes:**
1. Wrong matcher name (case-sensitive)
2. JSON syntax error
3. Hook added to wrong section

**Solutions:**
- Check matcher names: `Edit`, `Write`, `Bash`, `Read`, etc. (exact case)
- Validate JSON: `cat ~/.claude/settings.json | jq .`
- Make sure it's inside the `"hooks": { }` section

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

## Common Mistakes

1. **Wrong hook timing**
   - `PreToolUse`: Before the tool runs (for blocking, backup)
   - `PostToolUse`: After the tool runs (for formatting, testing)
   - Mixing these up causes unexpected behavior

2. **Forgetting to escape special characters**
   ```json
   // BAD - unescaped quotes
   "command": "echo "hello""

   // GOOD - use single quotes
   "command": "echo 'hello'"

   // GOOD - escape quotes
   "command": "echo \"hello\""
   ```

3. **Commands that hang**
   - If your hook command waits for input, Claude Code will hang
   - Always use non-interactive commands (`-y`, `--yes` flags)

4. **Overcomplicating hooks**
   - Start simple, add complexity gradually
   - One hook doing one thing is easier to debug

5. **Not testing commands manually first**
   - Always run your command in terminal first
   - If it doesn't work there, it won't work in a hook

---

## Glossary

| Term | Description |
|------|-------------|
| **Hook** | Code that automatically runs when specific events occur |
| **Command** | Saved prompt executed with `/command` |
| **Matcher** | Pattern that specifies which tool the Hook responds to |
| **$ARGUMENTS** | Variable that receives arguments passed to Command |
| **$FILE_PATH** | Variable containing the path of the modified file |
| **PreToolUse** | Hook that runs before tool execution |
| **PostToolUse** | Hook that runs after tool execution |

---

## Summary

What you learned in this chapter:
- [x] Hooks system (Pre/Post ToolUse, UserPromptSubmit, Stop)
- [x] Reusing prompts with Commands
- [x] Using variables and dynamic information
- [x] Combining Hooks + Commands
- [x] Sharing Commands with team

**Key Point**: Automate repetitive work with Hooks and Commands.

---

## Next Chapter Preview

In Chapter 24, you'll learn about **Agents and Skills**:
- Agents: Giving Claude expert roles
- Skills: Keyword-based automatic workflows
- Creating specialized helpers like code reviewers, testers, documentation writers

If Hooks and Commands focus on "what to do", Agents and Skills focus on "who does it, and how"!

Proceed to [Chapter 24: Agents & Skills](../Chapter24-Agents-and-Skills/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
