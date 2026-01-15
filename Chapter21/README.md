# Chapter 21: Agents & Skills

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Creating specialized helpers with Agents
- Automating specific tasks with Skills
- Practical usage examples

---

## Why do you need this?

**Real-world scenario**: You want different "personalities" for different tasks. When reviewing code, you want Claude to act like a strict senior developer. When debugging, you want a patient teacher. Commands alone can't give Claude a persistent identity.

Agents and Skills solve this by letting you define roles and automated workflows.

### Simple Analogy: Employees vs Job Procedures

Think of it this way:
- **Agents** are like hiring different employees for different jobs
  - Security guard: Always watches for threats
  - Documentation writer: Explains things clearly
  - Code reviewer: Finds bugs and suggests improvements

- **Skills** are like standard operating procedures
  - "How to process a return" (triggered when customer says "return")
  - "How to escalate an issue" (triggered when "urgent" is mentioned)

### Agent vs Skill: Quick Decision Guide

```
Do you need a consistent PERSONALITY/PERSPECTIVE?
  --> Use an AGENT (call with @name)

Do you need a consistent PROCESS/PROCEDURE?
  --> Use a SKILL (auto-triggered by keywords)

Do you need both?
  --> Combine them! @agent-name with skill keywords
```

---

## Concrete Comparison: Agent vs Skill

Let's see the same task handled both ways:

### Code Review - As an Agent

```markdown
<!-- .claude/agents/strict-reviewer.md -->
# Strict Code Reviewer

## Identity
You are a 15-year senior developer who has seen every bug.
You're thorough but constructive.

## Perspective
- Assume every line could have a bug
- Check security vulnerabilities
- Question performance implications
- Verify edge cases
```

**Usage:** `> @strict-reviewer check this function`

The Agent gives Claude a **persistent personality** throughout the review.

### Code Review - As a Skill

```markdown
<!-- .claude/skills/review-checklist.md -->
# Code Review Checklist

## Trigger Keywords
"review", "check code", "look at this"

## Process
1. Identify all functions
2. Check each for: null handling, error cases, types
3. List issues by severity
4. Provide summary
```

**Usage:** `> review this function`

The Skill defines a **fixed process** that runs automatically.

### When to Use Which?

| Scenario | Use | Why |
|----------|-----|-----|
| "I need security expertise" | Agent | Persistent perspective |
| "Run our standard review checklist" | Skill | Consistent process |
| "Senior security expert doing our review" | Both | Best of both worlds |

---

## Why Learn Agents and Skills?

Commands save prompts. Agents and Skills go a step further:

- **Agents**: Specialized Claude for specific roles
- **Skills**: Automation that responds to specific keywords

---

## Agents System

Agents are Claude optimized for specific roles.

### Agents Folder Structure

```
.claude/
└── agents/
    ├── code-reviewer.md   # Code review expert
    ├── doc-writer.md      # Documentation expert
    └── tester.md          # Testing expert
```

### Defining an Agent

```markdown
<!-- .claude/agents/code-reviewer.md -->
# Code Review Expert

## Role
You are a senior developer specializing in code review.

## Perspective
- Find bugs and vulnerabilities
- Identify performance issues
- Suggest readability improvements
- Verify best practices

## Review Format
1. Categorize issues by severity (Critical, Major, Minor)
2. Provide specific improvement suggestions for each issue
3. Mention what's done well too

## Tone
Constructive and educational. Don't criticize, suggest improvements.
```

### Using an Agent

```
> @code-reviewer review this PR
```

Call Agents with `@`.

### Why Does This Matter?

**Consistent review quality:**

Reviews from the same perspective every time, nothing gets missed.

**Role-appropriate responses:**

Unlike general Claude, answers only from a specific perspective.

---

## Practical Agents Examples

### 1. Documentation Expert

```markdown
<!-- .claude/agents/doc-writer.md -->
# Documentation Expert

## Role
Write technical documentation that is clear and easy to understand.

## Principles
- Be concise (remove unnecessary explanations)
- Include examples (code examples required)
- Be structured (use headings, subheadings)

## Document Formats
- README: Project overview, installation, usage
- API docs: Endpoints, parameters, responses
- Guides: Step-by-step explanations

## Language
Use English, keep technical terms as-is
```

```
> @doc-writer write documentation for this API
```

### 2. Testing Expert

```markdown
<!-- .claude/agents/tester.md -->
# Testing Expert

## Role
Write robust test code.

## Testing Strategy
- Unit tests first
- Always include edge cases
- Minimize mocking

## Test Structure
```
describe('functionName', () => {
  it('normal case', () => {})
  it('error case', () => {})
  it('edge case', () => {})
})
```

## Tools
- Jest (default)
- React Testing Library (components)
- MSW (API mocking)
```

```
> @tester write tests for this function
```

### 3. Refactoring Expert

```markdown
<!-- .claude/agents/refactorer.md -->
# Refactoring Expert

## Role
Improve existing code. Maintain behavior while increasing quality.

## Refactoring Principles
- Progress in small steps
- Verify tests pass at each step
- Explain reasons for changes

## Priorities
1. Remove duplication
2. Split functions (under 20 lines)
3. Improve naming
4. Reduce complexity

## Caution
- Adding features is not refactoring
- Don't change too much at once
```

```
> @refactorer refactor this component
```

### 4. Security Expert

```markdown
<!-- .claude/agents/security.md -->
# Security Expert

## Role
Find security vulnerabilities in code and suggest improvements.

## Checklist
- SQL Injection
- XSS (Cross-Site Scripting)
- CSRF
- Authentication/Authorization vulnerabilities
- Sensitive data exposure

## Report Format
1. Vulnerability description
2. Attack scenario
3. Solution
4. Prevention code

## Severity
- Critical: Immediate fix required
- High: Quick fix needed
- Medium: Planned fix
- Low: Recommendations
```

```
> @security review this API security
```

---

## Skills System

Skills automatically respond to specific keywords.

### Skills Folder Structure

```
.claude/
└── skills/
    ├── pr-review.md      # Responds to PR-related requests
    ├── deploy.md         # Responds to deployment requests
    └── debug.md          # Responds to debugging requests
```

### Defining a Skill

Skills can be configured more precisely using frontmatter (metadata):

```markdown
<!-- .claude/skills/pr-review.md -->
---
name: PR Review
description: Pull Request review and code analysis
allowed-tools: [Read, Glob, Grep, Bash]
model: sonnet
---

# PR Review Skill

## Keywords
- "PR review"
- "pull request check"
- "code review"
- "review this PR"

## Behavior
1. Check changes with git diff
2. Analyze each changed file
3. Identify issues and suggestions
4. Provide overall summary

## Output Format
### Summary
- Number of changed files
- Lines added/deleted
- Main changes

### Detailed Review
[Review by file]

### Overall Assessment
[Pass/Needs Changes]
```

### Using a Skill

When you include keywords in your request, the Skill activates automatically:

```
> review this PR
```

Since "PR review" keyword is present, the PR review Skill activates automatically.

### Why Does This Matter?

**Natural automation:**

Just include keywords and appropriate actions happen automatically.

**Consistent process:**

Same keyword always triggers the same process.

---

## Practical Skills Examples

### 1. Debugging Skill

```markdown
<!-- .claude/skills/debug.md -->
# Debugging Skill

## Keywords
- "error"
- "bug"
- "doesn't work"
- "broken"
- "failing"

## Behavior
1. Analyze error message
2. Find related code
3. Identify cause
4. Suggest solution

## Process
- Read error message literally
- Analyze stack trace
- Check related files
- Try step-by-step fixes
```

```
> getting error when logging in
```

Debugging process starts automatically.

### 2. Deployment Skill

```markdown
<!-- .claude/skills/deploy.md -->
# Deployment Skill

## Keywords
- "deploy"
- "production"
- "release"
- "ship"

## Checklist
- [ ] Tests passing?
- [ ] Build successful?
- [ ] Environment variables set?
- [ ] Migration needed?

## Behavior
1. Check current state
2. Verify checklist
3. Execute deployment
4. Verify results
```

```
> deploy to staging
```

Automatically checks deployment checklist and proceeds.

### 3. Performance Analysis Skill

```markdown
<!-- .claude/skills/performance.md -->
# Performance Analysis Skill

## Keywords
- "slow"
- "performance"
- "optimize"
- "laggy"

## Analysis Items
- Render count (React)
- Unnecessary recalculations
- Memory leaks
- API call optimization
- Bundle size

## Behavior
1. Identify slow parts
2. Analyze cause
3. Suggest optimizations
4. Compare after applying
```

```
> why is this page so slow?
```

Performance analysis starts automatically.

---

## Agents vs Skills

### When to Use What?

```
┌─────────────────────────────────────────────────────────────────┐
│                    Agents vs Skills                              │
├──────────────────────────────┬──────────────────────────────────┤
│           Agents             │           Skills                  │
├──────────────────────────────┼──────────────────────────────────┤
│ Explicit call with @         │ Auto-activate via keywords        │
│ Define role/perspective      │ Define process/procedure          │
│ Expert persona               │ Automated workflow                │
│ Ex: @code-reviewer           │ Ex: Auto-run on "PR review"       │
└──────────────────────────────┴──────────────────────────────────┘
```

### Using Together

```
> @security review this PR
```

- `@security`: Security expert perspective (Agent)
- "review this PR": PR review process (Skill)

Runs PR review process from security expert perspective.

---

## Sharing with Team

### Commit to Git

```
my-project/
├── .claude/
│   ├── agents/       # Team shared Agents
│   │   ├── code-reviewer.md
│   │   └── doc-writer.md
│   └── skills/       # Team shared Skills
│       ├── pr-review.md
│       └── deploy.md
├── CLAUDE.md
└── src/
```

### Document Team Standards

```markdown
# Team Agents & Skills

## Agents
- `@code-reviewer` - Code review expert
- `@doc-writer` - Documentation expert
- `@security` - Security expert

## Skills (Auto-activate)
- "PR review" - PR review process
- "deploy" - Deployment checklist
- "error" - Debugging process
```

---

## Try it yourself

### Exercise 1: Create Your First Agent

1. Create the agents folder: `mkdir -p .claude/agents`
2. Create `.claude/agents/teacher.md`:

```markdown
# Patient Teacher

## Role
You are a patient teacher who explains coding concepts.
Always use simple language and provide examples.

## Style
- Start with the big picture
- Use analogies
- Provide code examples
- Check for understanding
```

3. Use it: `> @teacher explain what recursion is`

### Exercise 2: Create Your First Skill

1. Create the skills folder: `mkdir -p .claude/skills`
2. Create `.claude/skills/explain.md`:

```markdown
# Code Explanation Skill

## Keywords
"explain", "what does this do", "how does this work"

## Process
1. Read the code
2. Identify the main purpose
3. Break down step by step
4. Provide a simple summary
```

3. Test it: `> explain what this function does` (skill triggers automatically!)

### Exercise 3: Combine Agent + Skill

```
> @teacher explain what this function does
```

The teacher agent's personality + the explanation skill's process = best of both!

---

## If it doesn't work?

### Problem: Agent not found

**Possible causes:**
1. File not in `.claude/agents/` folder
2. File doesn't end with `.md`
3. Using wrong name (filename without .md is the name)

**Solutions:**
- Check folder: `ls -la .claude/agents/`
- Agent `@teacher` needs file `teacher.md`
- Case matters: `@Teacher` is different from `@teacher`

### Problem: Skill not triggering

**Possible causes:**
1. Keywords don't match your request
2. Skill file is malformed
3. Skills folder in wrong location

**Solutions:**
- Check your keywords exactly match what you typed
- Skills folder should be `.claude/skills/`
- Test with exact keyword from the skill file

### Problem: Agent personality not consistent

**Possible causes:**
1. Agent definition too vague
2. Conflicting instructions
3. Context getting diluted in long conversations

**Solutions:**
- Be specific in agent personality description
- Avoid contradictory instructions
- Use `/clear` to reset context if needed

---

## Common mistakes

1. **Making agents too broad**
   - Bad: "You're a helpful assistant" (too generic)
   - Good: "You're a TypeScript expert who focuses on type safety" (specific)

2. **Making skills too vague**
   - Bad: Keywords like "help" (triggers on everything)
   - Good: Specific keywords like "deploy to staging"

3. **Confusing agents and commands**
   - Commands: Just run a saved prompt
   - Agents: Give Claude a persistent personality
   - Use agents when perspective matters

4. **Forgetting to share with team**
   - Commit `.claude/agents/` and `.claude/skills/` to git
   - Document in README what's available

5. **Not testing before sharing**
   - Test your agents and skills before sharing
   - Edge cases often reveal problems

---

## Summary

What you learned in this chapter:
- [x] Defining expert roles with Agents
- [x] Keyword-based automation with Skills
- [x] Difference between Agents and Skills
- [x] Practical usage examples
- [x] Sharing with team

**Key point**: Agents focus on "like who", Skills focus on "what to do".

In the next chapter, you'll learn how to connect with external services using MCP.

Proceed to [Chapter 22: MCP Integration](../Chapter22/README.md).
