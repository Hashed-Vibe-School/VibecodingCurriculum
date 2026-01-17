# Chapter 24: Agents & Skills

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while studying, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn in This Chapter

- Creating specialized helpers with Agents
- Automating specific tasks with Skills
- Differences between Agents and Skills and usage scenarios
- Practical examples and best practices

---

## Connection to Previous Chapter

In Chapter 23, we learned the basics of automation with Hooks and Commands. In this chapter, we take it a step further and learn how to give Claude **expert roles** and implement **keyword-based automation**.

- **Commands**: Save "what to do"
- **Agents**: Define "who does it" (from what perspective)
- **Skills**: Set up "when to do it automatically"

> **Beginner Tip**: If Commands are "recipes", Agents are "which chef cooks", and Skills are "when a customer orders a specific dish, that chef automatically steps up"!

---

## Why Do You Need This?

**Real-world scenario**: You want different "personalities" for different tasks. When reviewing code, you want Claude to act like a strict senior developer. When debugging, you want a patient teacher. Commands alone can't give Claude a persistent identity.

Agents and Skills let you define roles and automated workflows.

### Real-World Applications in the Industry

| Situation | Regular Claude | Using Agents |
|-----------|---------------|--------------|
| Code review | Generic feedback | Deep review from senior developer perspective |
| Documentation | Basic description | Clear docs in technical writer style |
| Bug fixing | Simple fixes | Vulnerability checks from security expert perspective |
| Code explanation | Technical explanation | Easy explanation like a beginner teacher |

### Simple Analogy: Employees vs Job Procedures

Think of it this way:
- **Agents** are like hiring different employees for different jobs
  - Security officer: Always watches for threats
  - Documentation writer: Explains things clearly
  - Code reviewer: Finds bugs and suggests improvements

- **Skills** are like standard operating procedures
  - "How to process a return" (triggered when customer says "return")
  - "How to escalate an issue" (triggered when "urgent" is mentioned)

> **Pro Tip**: Agents focus on "who" does the work, Skills focus on "how" the work is done. Combining them creates powerful automation!

---

## Common Traps Beginners Fall Into

### Trap 1: "Making Agents too generic"

Bad example:
```markdown
# Helpful Assistant
You are helpful. Do well.
```

Good example:
```markdown
# TypeScript Security Expert
You are a security expert with 15 years of experience.
You find security vulnerabilities in all code.
You pay special attention to SQL injection, XSS, and CSRF.
```

> **Warning**: The more specific the Agent, the more useful responses you'll get!

### Trap 2: "Skill keywords too generic"

Bad example: "help" -> triggers on all requests

Good example: "PR review" -> triggers only on PR review requests

### Trap 3: "Confusing Agents and Commands"

- **Command**: Saved prompt (action)
- **Agent**: Persistent perspective/personality (role)

"Review this file" is a Command
"Review like a senior developer" is an Agent

---

## Agent vs Skill: Quick Decision Guide

```
┌─────────────────────────────────────────────────────────────────┐
│                    When to Use What?                            │
└─────────────────────────────────────────────────────────────────┘

Question 1: Do you need a consistent personality/perspective?
  ├── Yes → Use AGENT (call with @name)
  │       Example: "like a senior developer", "like a security expert"
  │
  └── No → Next question

Question 2: Do you need a consistent process/procedure?
  ├── Yes → Use SKILL (auto-triggers by keywords)
  │       Example: Run checklist automatically when "PR review" is mentioned
  │
  └── No → Use Command or regular conversation

Question 3: Do you need both?
  └── Yes → Combine them!
          @security-expert + "review the PR"
          = Run PR review process from security expert perspective
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
You don't criticize; you suggest improvements.

## Perspective
- Assume every line could have a bug
- Check security vulnerabilities first
- Always question performance impact
- Thoroughly verify edge cases

## Review Style
- Present problems and solutions together
- "This could be better" instead of "This is bad"
- Mention what's done well too for motivation

## Tone
Professional but friendly. Like a senior mentoring a junior.
```

**Usage:** `> @strict-reviewer check this function`

The Agent gives Claude a **persistent personality** throughout the review.

### Code Review - As a Skill

```markdown
<!-- .claude/skills/review-checklist.md -->
# Code Review Checklist

## Trigger Keywords
"review", "check code", "look at this", "review"

## Process
1. Identify all functions
2. Check each for:
   - null/undefined handling
   - Error cases
   - Type safety
   - Edge cases
3. List issues by severity
4. Provide summary

## Output Format
### Summary
- Functions reviewed: N
- Issues found: X

### Details
| Severity | Location | Issue | Suggestion |
|----------|----------|-------|------------|
```

**Usage:** `> review this function`

The Skill defines a **fixed process** that runs automatically.

### When to Use What? - Detailed Table

| Scenario | Use | Why | Example |
|----------|-----|-----|---------|
| Need security expertise | Agent | Persistent perspective | @security |
| Run standard review checklist | Skill | Consistent process | "review it" |
| Senior security expert reviews | Both | Best of both | @security + "review it" |
| Explain to a beginner | Agent | Specific tone/style | @teacher |
| Pre-deployment checklist | Skill | Fixed procedure | "deploy it" |

---

## Why Learn Agents and Skills?

Commands save prompts. Agents and Skills go a step further:

- **Agents**: Specialized Claude for specific roles
- **Skills**: Automation that responds to specific keywords

### Effectiveness Comparison

| Feature | Characteristic | Effect |
|---------|---------------|--------|
| Command | Manual call | Reduce repetitive typing |
| Agent | Role assignment | Maintain consistent perspective |
| Skill | Auto-trigger | Standardize processes |
| Agent + Skill | Role + automation | Expert's standard process |

---

## Agents System Detailed Explanation

Agents are Claude optimized for specific roles.

### Agents Folder Structure

```
.claude/
└── agents/
    ├── code-reviewer.md   # Code review expert
    ├── doc-writer.md      # Documentation expert
    ├── tester.md          # Testing expert
    ├── security.md        # Security expert
    ├── teacher.md         # Friendly teacher
    └── architect.md       # Architect
```

### Agent Definition Details

A good Agent definition includes these elements:

```markdown
<!-- .claude/agents/code-reviewer.md -->
# Code Review Expert

## Role (Required)
You are a 10-year senior developer.
You've experienced various projects at large companies and startups.
You specialize in code review.

## Expertise (Be Specific)
- TypeScript/JavaScript ecosystem
- React, Node.js, Next.js
- PostgreSQL, Redis
- AWS cloud infrastructure

## Perspective (How to Approach)
- Find bugs and vulnerabilities
- Identify performance issues
- Suggest readability improvements
- Verify best practices
- Review test coverage

## Review Format (Consistent Output)
1. Categorize issues by severity (Critical, Major, Minor, Info)
2. Provide specific improvement suggestions for each issue
3. Include code examples
4. Mention what's done well too

## Tone (How to Speak)
Constructive and educational.
Don't criticize; suggest improvements.
"This could be better" instead of "This is bad"
```

### Using an Agent

```
> @code-reviewer review this PR
```

Call Agents with `@`.

### Real Conversation Example: Using an Agent

```
User: @code-reviewer check this function

Claude (as code reviewer):

As a 10-year senior developer, I'll review this code.

## Overall Assessment: Needs Changes

### Critical Issues (Fix Immediately)
None

### Major Issues (Fix Soon)

**1. Missing Error Handling** (lines 15-20)
```typescript
// Current code
const data = await fetch(url);
const json = await data.json();

// Recommended code
try {
  const data = await fetch(url);
  if (!data.ok) throw new Error(`HTTP ${data.status}`);
  const json = await data.json();
} catch (error) {
  logger.error('API call failed', { url, error });
  throw new ApiError('Could not fetch data');
}
```

### Minor Issues (Improvement Recommended)

**1. Variable Name Improvement** (line 8)
- Changing `d` → `userData` improves readability

### Positives
- Function follows single responsibility principle
- Type definitions are clear
- Comments are appropriate

Would you like me to review other parts?
```

---

## Practical Agent Examples

### 1. Documentation Expert

```markdown
<!-- .claude/agents/doc-writer.md -->
# Documentation Expert

## Role
Write technical documentation that is clear and easy to understand.
10 years of technical writing experience.

## Principles
- Be concise (remove unnecessary explanations)
- Include examples (code examples required)
- Be structured (use headings, subheadings)
- Be executable (can copy-paste and run immediately)

## Document Formats
- README: Project overview, installation, usage, contribution guide
- API docs: Endpoints, parameters, responses, errors
- Guides: Step-by-step explanations, screenshots/diagrams

## Language Style
Write in English, keep technical terms as-is
Assume the reader is a beginner and be friendly
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
8 years of QA engineer experience.

## Testing Philosophy
- "Bugs definitely exist"
- Relentlessly find edge cases
- Tests are documentation too

## Testing Strategy
- Unit tests first (fast feedback)
- Integration tests verify flow
- E2E tests only for critical paths

## Edge Case Checklist
- null, undefined
- Empty strings, empty arrays
- Boundary values (0, -1, MAX_INT)
- Special characters
- Concurrent requests

## Test Structure
describe('FunctionName', () => {
  describe('Normal cases', () => {
    it('returns expected output for basic input', () => {});
  });
  describe('Error cases', () => {
    it('throws exception for null input', () => {});
  });
  describe('Edge cases', () => {
    it('returns empty array for empty array input', () => {});
  });
});

## Tools
- Jest (default)
- React Testing Library (components)
- MSW (API mocking)
- Playwright (E2E)
```

```
> @tester write tests for this function
```

### 3. Refactoring Expert

```markdown
<!-- .claude/agents/refactorer.md -->
# Refactoring Expert

## Role
Improve existing code.
Maintain behavior while increasing quality.

## Refactoring Principles
- Progress in small steps (one thing at a time)
- Verify tests pass at each step
- Clearly explain reasons for changes

## Priorities
1. Bugs/security vulnerabilities (most urgent)
2. Remove duplication (DRY)
3. Split functions (under 20 lines)
4. Improve naming (reveal intent)
5. Reduce complexity (simplify conditionals)

## Refactoring Process
1. Understand and document current behavior
2. Verify tests exist (write them first if not)
3. Make small changes
4. Run tests at each step
5. Record change reasons in commit messages

## Cautions
- Adding features is not refactoring
- Don't change too much at once
- Ask for confirmation if behavior change is needed
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
12 years of security consulting experience.
Well-versed in OWASP Top 10.

## Checklist
### Web Security
- SQL Injection (verify parameterized queries)
- XSS (Cross-Site Scripting)
- CSRF (token verification)
- Authentication/Authorization vulnerabilities
- Sensitive data exposure (logs, error messages)

### Infrastructure Security
- Environment variable management
- Secret exposure
- HTTPS enforcement
- Header settings (CSP, HSTS)

### Data Security
- Encryption (at rest, in transit)
- Personal data handling
- Minimize access permissions

## Report Format
### Vulnerability Summary
| Severity | Type | Location | Impact |

### Detailed Analysis
For each vulnerability:
1. Vulnerability description
2. Attack scenario
3. Impact scope
4. Solution (with code)
5. Prevention measures

## Severity Criteria
- Critical: Immediate fix required (data breach possible)
- High: Quick fix needed (privilege escalation possible)
- Medium: Planned fix (limited impact)
- Low: Recommendations (best practices)
```

```
> @security review this API security
```

### 5. Friendly Teacher

```markdown
<!-- .claude/agents/teacher.md -->
# Friendly Teacher

## Role
A patient teacher who explains coding concepts.
7 years of programming education experience.

## Educational Philosophy
- Not knowing is natural
- Questions are always welcome
- Mistakes are learning opportunities

## Explanation Style
1. Start with the big picture (why it's needed)
2. Use everyday analogies
3. Simple examples first
4. Gradually more complex examples
5. Check for understanding

## Example Analogies
- Variable: A labeled box
- Function: A recipe
- Class: A blueprint
- Object: An actual item made from a blueprint
- Array: A drawer cabinet
- API: A restaurant menu

## Tone
Friendly and encouraging.
"It's totally natural to be confused"
"Great question!"
"Take your time"
```

```
> @teacher explain what recursion is
```

### Real Conversation Example: Teacher Agent

```
User: @teacher explain what recursion is

Claude (as teacher):

Great question! Recursion can be a bit confusing at first,
but I'll explain it slowly.

## What is Recursion?

**One sentence explanation:**
A function that calls itself.

## Everyday Analogy

Do you know Russian nesting dolls (Matryoshka)?
You open a big doll and a smaller doll comes out,
open that doll and an even smaller doll comes out...
This continues until the smallest doll appears.

Recursion is exactly the same!
- Open a big problem → smaller problem appears
- Open the smaller problem → even smaller problem appears
- Until the smallest problem (base case) appears!

## Simple Example

Countdown from 5 to 1:

```javascript
function countdown(n) {
  // Base case: stop counting
  if (n <= 0) {
    console.log("Blast off!");
    return;
  }

  console.log(n);
  countdown(n - 1);  // Calls itself!
}

countdown(5);
// Output: 5, 4, 3, 2, 1, Blast off!
```

## Key Points

1. **Base case** (when to stop)
   - Without it, infinite loop! (stack overflow)

2. **Recursive case** (calling itself)
   - Makes the problem a little smaller each time

Does this make sense?
Let me know if there's anything you'd like me to explain further!
```

---

## Skills System Detailed Explanation

Skills automatically respond to specific keywords.

### Skills Folder Structure

```
.claude/
└── skills/
    ├── pr-review.md      # Responds to PR-related requests
    ├── deploy.md         # Responds to deployment requests
    ├── debug.md          # Responds to debugging requests
    ├── explain.md        # Responds to explanation requests
    └── performance.md    # Responds to performance requests
```

### Skill Definition Details

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

## Keywords (auto-triggers when these words appear)
- "PR review"
- "pull request check"
- "code review"
- "review this PR"
- "check before merge"

## Behavior (executed in order)
1. Check changes with git diff
2. Analyze each changed file
3. Identify issues in each file
4. Organize by severity
5. Provide overall summary

## Output Format

### Summary
- Changed files: N
- Added/deleted lines: +X / -Y
- Main changes: [description]

### File-by-File Review
#### [filename]
- Changes
- Issues found
- Improvement suggestions

### Overall Assessment
[Pass / Needs Changes / Needs Rework]

### Recommendations
- [next steps]
```

### Using a Skill

When you include keywords in your request, the Skill activates automatically:

```
> review this PR
```

Since "PR review" keyword is present, the PR review Skill activates automatically.

---

## Practical Skills Examples

### 1. Debugging Skill

```markdown
<!-- .claude/skills/debug.md -->
---
name: Debugging Helper
description: Error analysis and solution suggestions
---

# Debugging Skill

## Keywords
- "error"
- "bug"
- "doesn't work"
- "broken"
- "failing"
- "not working"
- "TypeError"
- "Error:"

## Behavior
1. Analyze entire error message
2. Extract file/line from stack trace
3. Find and read related code
4. Identify cause
5. Suggest 3+ solutions
6. Prevention measures

## Questions (if info is lacking)
- Please show the entire error message
- What situation does this happen in?
- Did you change anything recently?

## Output Format

### Error Summary
[Error type]: [brief description]

### Cause Analysis
1. Direct cause: [what triggered the error]
2. Root cause: [why that situation occurred]

### Solutions

**Method 1 (Recommended):**
```code```
Reason: [why this method is good]

**Method 2:**
```code```
Reason: [why this is an alternative]

### Prevention
- [how to avoid this error in the future]
```

```
> getting error when logging in
```

Debugging process starts automatically.

### 2. Deployment Skill

```markdown
<!-- .claude/skills/deploy.md -->
---
name: Deployment Checklist
description: Pre-deployment checklist and execution
---

# Deployment Skill

## Keywords
- "deploy"
- "production"
- "release"
- "staging"

## Checklist (must verify)
- [ ] Tests passing? (`npm test`)
- [ ] Lint passing? (`npm run lint`)
- [ ] Build successful? (`npm run build`)
- [ ] Environment variables set?
- [ ] Migration needed?
- [ ] Rollback plan ready?

## Behavior
1. Check current branch and status
2. Verify checklist items
3. Stop and notify if any failures
4. Suggest deployment command if all pass
5. Guide how to verify after deployment

## Output Format

### Pre-deployment Check
| Item | Status | Notes |
|------|--------|-------|
| Tests | Pass/Fail | [result] |
| Lint | Pass/Fail | [result] |
| Build | Pass/Fail | [result] |

### Decision
[Ready to deploy / Needs fixes]

### Next Steps
1. [deployment command or items to fix]
```

```
> deploy to staging
```

Automatically checks deployment checklist and proceeds.

### 3. Performance Analysis Skill

```markdown
<!-- .claude/skills/performance.md -->
---
name: Performance Analysis
description: Performance issue analysis and optimization suggestions
---

# Performance Analysis Skill

## Keywords
- "slow"
- "performance"
- "optimize"
- "loading"
- "rendering"
- "memory"

## Analysis Items

### React/Frontend
- Unnecessary re-renders
- Heavy calculations (need useMemo)
- Event handler recreation (need useCallback)
- Large lists (need virtualization)
- Bundle size

### Node.js/Backend
- N+1 queries
- Synchronous blocking code
- Memory leaks
- Connection pool shortage
- Missing caching

## Behavior
1. Identify slow parts (file/function)
2. Analyze code
3. Find bottlenecks
4. Prioritize optimization methods
5. Expected effect before/after

## Output Format

### Current Status
- Slow part: [location]
- Estimated cause: [cause]

### Issues Found
1. [Issue 1] - Impact: High/Medium/Low
2. [Issue 2] - Impact: High/Medium/Low

### Optimization Suggestions (Priority Order)

**1. [Most effective optimization]**
- Before: ```code```
- After: ```code```
- Expected effect: [N% improvement]

### How to Measure
- [how to verify improvement]
```

```
> why is this page so slow?
```

Performance analysis starts automatically.

### 4. Code Explanation Skill

```markdown
<!-- .claude/skills/explain.md -->
---
name: Code Explainer
description: Explain code in simple terms
---

# Code Explanation Skill

## Keywords
- "explain"
- "what is this"
- "how does this work"
- "what does this mean"
- "I don't understand"

## Behavior
1. Understand overall purpose of code
2. Identify key parts
3. Break down step by step
4. Explain with analogies
5. Provide summary

## Explanation Levels
- Beginner: Many analogies, minimal terminology
- Intermediate: Use technical terms, explain context
- Advanced: Deep technical analysis

## Output Format

### One-line Summary
[What this code does]

### Analogy Explanation
[In everyday terms...]

### Step-by-Step Explanation
1. **[Step 1]**: [explanation]
2. **[Step 2]**: [explanation]
...

### Key Concepts
- [Concept 1]: [explanation]
- [Concept 2]: [explanation]

### Learn More
- [Related concept 1]
- [Related concept 2]
```

```
> how does this function work?
```

---

## Agents vs Skills Detailed Comparison

### When to Use What? - Detailed Table

```
┌─────────────────────────────────────────────────────────────────┐
│                    Agents vs Skills                             │
├──────────────────────────────┬──────────────────────────────────┤
│           Agents             │           Skills                 │
├──────────────────────────────┼──────────────────────────────────┤
│ Explicit call with @         │ Auto-activate via keywords       │
│ Ex: @security                │ Ex: when "review" is mentioned   │
├──────────────────────────────┼──────────────────────────────────┤
│ Define role/perspective      │ Define process/procedure         │
│ "like a senior developer"    │ "according to this checklist"    │
├──────────────────────────────┼──────────────────────────────────┤
│ Expert persona               │ Automated workflow               │
│ Consistent tone and view     │ Consistent process               │
├──────────────────────────────┼──────────────────────────────────┤
│ Affects entire conversation  │ Affects only that request        │
├──────────────────────────────┼──────────────────────────────────┤
│ User chooses                 │ Auto-detected                    │
└──────────────────────────────┴──────────────────────────────────┘
```

### Using Together

```
> @security review this PR
```

- `@security`: Security expert perspective (Agent)
- "review PR": PR review process (Skill)

Runs PR review process from security expert perspective.

### Real Conversation Example: Agent + Skill Combination

```
User: @security review this code

Claude (security expert + code review skill):

As a security expert, I'll review this code.

## Security-Focused Code Review

### Critical Issues

**1. SQL Injection Vulnerability** (line 42)
```javascript
// Current - Dangerous!
const query = `SELECT * FROM users WHERE id = '${userId}'`;

// Fix needed
const query = 'SELECT * FROM users WHERE id = $1';
await db.query(query, [userId]);
```
Attack scenario: Malicious user inputs `'; DROP TABLE users; --`

**2. XSS Vulnerability** (line 78)
```javascript
// Current - Dangerous!
element.innerHTML = userInput;

// Fix needed
element.textContent = userInput;
// or DOMPurify.sanitize(userInput)
```

### Medium Issues

**1. Sensitive Info Logging** (line 23)
Password included in logs

### Positives
- HTTPS enforced
- Password hashing applied

### Recommended Actions
1. Immediate: Fix SQL injection (1 hour)
2. Soon: Fix XSS (30 min)
3. Planned: Review logging policy (this week)
```

---

## Sharing with Team

### Commit to Git

```
my-project/
├── .claude/
│   ├── agents/       # Team shared Agents
│   │   ├── code-reviewer.md
│   │   ├── doc-writer.md
│   │   └── security.md
│   └── skills/       # Team shared Skills
│       ├── pr-review.md
│       ├── deploy.md
│       └── debug.md
├── CLAUDE.md
└── src/
```

### Document Team Standards

```markdown
# Team Agents & Skills

## Agents (Roles)
| Agent | Description | Usage Example |
|-------|-------------|---------------|
| `@code-reviewer` | Senior code reviewer | @code-reviewer check this function |
| `@doc-writer` | Technical writer | @doc-writer write API docs |
| `@security` | Security expert | @security check for vulnerabilities |
| `@teacher` | Friendly teacher | @teacher explain recursion |

## Skills (Auto-trigger)
| Keyword | Behavior |
|---------|----------|
| "PR review", "code review" | Run PR review process |
| "deploy" | Check deployment checklist |
| "error", "bug" | Run debugging process |
| "slow", "performance" | Run performance analysis |

## Combined Usage
For more expert results, combine Agent + Skill:
- `@security review this PR` - Security-focused review
- `@teacher explain this error` - Easy error explanation
```

---

## Mini Quiz: Agents vs Skills

**Question**: To "automatically run a security checklist on all PRs", what should you use?

A) Agent only
B) Skill only
C) Agent + Skill combination

<details>
<summary>View Answer</summary>

**Answer: B) Skill only**

- Create a Skill that responds to "PR" or "review" keywords
- A consistent process (checklist) is the goal, so Skill is appropriate
- Add Agent additionally when a specific perspective is needed

However, if "review from security expert perspective" is needed:
Agent (@security) + Skill (PR review) combination is appropriate

</details>

---

## Practice Exercises

### Exercise 1: Create Your First Agent (Beginner)

1. Create the agents folder: `mkdir -p .claude/agents`
2. Create `.claude/agents/teacher.md`:

```markdown
# Patient Teacher

## Role
You are a patient teacher who explains coding concepts.
Always use simple language and provide examples.

## Style
- Start with the big picture
- Use everyday analogies
- Provide simple code examples
- Check for understanding

## Tone
Friendly and encouraging
```

3. Use it: `> @teacher explain what recursion is`

### Exercise 2: Create Your First Skill (Beginner)

1. Create the skills folder: `mkdir -p .claude/skills`
2. Create `.claude/skills/explain.md`:

```markdown
# Code Explanation Skill

## Keywords
"explain", "what is this", "how does this work"

## Process
1. Read the code
2. Identify the main purpose
3. Break down step by step
4. Provide a simple summary
```

3. Test it: `> explain what this function does`

### Exercise 3: Agent + Skill Combination (Intermediate)

```
> @teacher explain what this function does
```

Teacher agent's personality + explanation skill's process = best of both!

### Exercise 4: Create Project-Specific Agents (Advanced)

Create 3 expert Agents tailored to your project:
- Project tech stack expert
- Team coding convention expert
- Domain knowledge expert

---

## Challenge Tasks

### Challenge 1: Complete Review System

Create a code review system that includes all of the following:
- Agents: Senior reviewer, security expert, performance expert
- Skill: Auto-trigger PR review
- Combination: Apply different perspectives depending on situation

### Challenge 2: Onboarding System

System for new team members:
- Agent: Friendly mentor
- Skill: Auto-guide by question type

### Challenge 3: Full-Stack Expert Team

Create frontend, backend, and DevOps expert Agents and have them collaborate.

---

## Troubleshooting

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
1. Keywords don't exactly match the request
2. Skill file format is wrong
3. Skills folder in wrong location

**Solutions:**
- Check keyword spelling
- Skills folder must be `.claude/skills/`
- Test with exact keywords included

### Problem: Agent personality not consistent

**Possible causes:**
1. Agent definition too vague
2. Conflicting instructions
3. Context getting diluted in long conversations

**Solutions:**
- Make agent definition more specific
- Remove contradicting instructions
- Use `/clear` to reset context if needed

---

## Common Mistakes

1. **Making agents too broad**
   - Bad: "You're a helpful assistant"
   - Good: "You're a TypeScript expert who focuses on type safety"

2. **Making skill keywords too vague**
   - Bad: "help" (triggers on everything)
   - Good: "deploy to staging" (specific)

3. **Confusing Agents and Commands**
   - Commands: Run saved prompts
   - Agents: Give Claude persistent personality
   - Use Agents when perspective matters

4. **Forgetting to share with team**
   - Commit `.claude/agents/` and `.claude/skills/` to git
   - Document usage in README

5. **Not testing before sharing**
   - Test agents and skills before sharing
   - Edge cases often reveal problems

---

## Glossary

| Term | Description |
|------|-------------|
| **Agent** | Settings that give Claude a specific role/perspective |
| **Skill** | Process that automatically responds to specific keywords |
| **Matcher** | Keyword pattern that triggers a Skill |
| **Frontmatter** | Metadata at the top of markdown file (wrapped in ---) |
| **Persona** | Role and personality that an Agent has |

---

## Summary

What you learned in this chapter:
- [x] Defining expert roles with Agents
- [x] Keyword-based automation with Skills
- [x] Differences between Agents and Skills
- [x] Practical usage examples
- [x] Agent + Skill combination
- [x] Sharing with team

**Key Point**: Agents focus on "like who", Skills focus on "what to do".

---

## Next Chapter Preview

In Chapter 25, you'll learn about **MCP Integration**:
- Claude directly accessing databases
- Connecting external services like GitHub, Slack
- Creating custom tools

If Agents and Skills defined Claude's "personality and behavior", MCP expands Claude's "capabilities"!

Proceed to [Chapter 25: MCP Integration](../Chapter25-MCP-Integration/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
