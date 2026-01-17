# Chapter 27: Team Collaboration

**English** | [한국어](./README.ko.md)

---

## 💬 Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Connection to Previous Chapters

Congratulations! This is the **final chapter** of the Vibecoding Curriculum!

Looking back at everything you've learned:

| Part | Chapters | Content |
|------|----------|---------|
| **Basics** | 1-10 | Installation, basic usage, first project |
| **Practical** | 11-20 | Website, app development, deployment |
| **Advanced** | 22-26 | Configuration, Hooks, Agents, MCP, CI/CD |
| **Collaboration** | **27** | **Effective team utilization** |

All the features learned in previous chapters:
- **CLAUDE.md** (Chapter 22): Define project rules
- **Hooks & Commands** (Chapter 23): Automation workflows
- **Agents & Skills** (Chapter 24): Define roles and skills
- **MCP** (Chapter 25): External service integration
- **CI/CD** (Chapter 26): Automation pipelines

Let's learn how to standardize all of this so the **entire team can utilize it**!

---

## What You Will Learn in This Chapter

- Standardizing Claude Code in teams
- Sharing settings and prompts
- Building collaboration workflows
- Automating new member onboarding
- Documenting team rules
- **Complete curriculum summary**
- **Next learning steps guide**

---

## Why is Team Collaboration Important?

### Real Scenario: Solo vs Team

**When using alone:**
```
Me: "Review this code"
Claude: (Reviews in each person's own style)
```

**When using in a team:**
```
Team member A: "/review" → Standardized review process
Team member B: "/review" → Same review process
Team member C: "/review" → Same review process

Result: Consistent code quality!
```

### Problems Without Standardization

```
┌─────────────────────────────────────────────────────────────────────┐
│                     Chaos in a Non-Standardized Team                 │
└─────────────────────────────────────────────────────────────────────┘

Team member A: "Claude, review this code"
        → Uses their own prompt
        → Gets feedback in their own style

Team member B: "Is this code okay?"
        → Uses a different prompt
        → Gets feedback in different style

Team member C: "@reviewer please review"
        → Yet another prompt
        → Yet another style

New member: "...what am I supposed to do? 😰"

Results:
- Inconsistent code quality
- Difficult onboarding for new members
- Different experiences per team member
- Failed knowledge sharing
```

### What Changes with Standardization

```
┌─────────────────────────────────────────────────────────────────────┐
│                     Collaboration in a Standardized Team             │
└─────────────────────────────────────────────────────────────────────┘

Team member A: "/review"
Team member B: "/review"
Team member C: "/review"
New member: "/review" (Can use from day one!)

All get the same:
✓ Review perspective
✓ Feedback style
✓ Quality standards
✓ Documentation approach

Results:
- Consistent code quality
- Fast onboarding
- Improved team-wide productivity
- Automatic knowledge sharing
```

---

## Why Do You Need This?

**Real scenario**: Your team loves Claude Code, but everyone uses it differently. One person uses certain commands, another has different settings. New team members are confused. Code reviews are inconsistent.

Standardizing Claude Code for your team means everyone works the same way, and it's easier to onboard new members.

### Simple Analogy: Team Playbook

Think of team configuration like a sports team playbook:
- Everyone knows the same plays (commands)
- Everyone uses the same terminology (agents)
- New players can learn quickly (onboarding)

### Another Analogy: McDonald's Manual

The reason you can eat a Big Mac with the same taste at any McDonald's in the world:
- **Standardized recipes** = CLAUDE.md (project rules)
- **Standardized procedures** = Commands (work processes)
- **Role definitions** = Agents (expert roles)
- **Training manuals** = Onboarding documents

If you standardize your team's Claude Code usage the same way, **anyone who uses it gets consistent results**!

---

## 💡 Tips for Non-Technical Readers

> **Why is "standardization" important?**
>
> Using cooking as an example:
>
> **Without standardization:**
> - Chef A: A pinch of salt
> - Chef B: Two tablespoons of salt
> - Chef C: Salt... did I add it?
> → Different taste every time!
>
> **With standardization:**
> - All chefs: 5g of salt per recipe
> → Same taste every time!
>
> Code is the same way. When the entire team follows the same "recipe":
> - Consistent code style
> - Uniform review quality
> - Easy adaptation for new members

---

## Why Learn Team Collaboration?

Using alone vs in a team is different:

- **Consistency**: Different approaches per member → confusion
- **Onboarding**: New members should start immediately
- **Quality**: Improve code quality across the team

### Individual Use vs Team Use Comparison

| Aspect | Individual Use | Team Use |
|--------|---------------|----------|
| Settings | Personal preference | Team standards needed |
| Commands | Only I need to know | Everyone must be able to use |
| Agents | Freely defined | Roles clearly defined |
| Documentation | Optional | Essential! |
| Onboarding | None | Very important |

---

## Team Standardization

### What to Share

```
my-project/
├── .claude/
│   ├── settings.json    # Team settings
│   ├── commands/        # Team Commands
│   │   ├── pr.md
│   │   ├── review.md
│   │   ├── onboarding.md
│   │   └── issue-start.md
│   ├── agents/          # Team Agents
│   │   ├── senior-reviewer.md
│   │   ├── junior-helper.md
│   │   └── security-expert.md
│   └── skills/          # Team Skills
│       ├── deploy.md
│       └── debug.md
├── CLAUDE.md            # Project rules
├── .github/
│   └── workflows/       # CI/CD (Chapter 26)
└── src/
```

Commit these folders to Git and the whole team works in the same environment.

### Team Settings File Example

```json
// .claude/settings.json
{
  "project": {
    "name": "Team Project",
    "team": "Frontend Team"
  },
  "preferences": {
    "language": "en",
    "codeStyle": "team-standard"
  },
  "permissions": {
    "autoApprove": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test)",
      "Bash(npm run lint)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force)"
    ]
  },
  "hooks": {
    "preCommit": "npm run lint && npm test"
  }
}
```

### CLAUDE.md Team Version

```markdown
# Project: My Team App

## Team Info
- Team: Frontend Team
- Contact: #frontend-dev (Slack)
- Team Lead: @team-lead

## Tech Stack
- React 18 + TypeScript
- Tailwind CSS
- Jest + React Testing Library
- GitHub Actions

## Coding Conventions

### Naming Rules
| Target | Rule | Example |
|--------|------|---------|
| Components | PascalCase | `UserProfile.tsx` |
| Hooks | camelCase, use prefix | `useAuth.ts` |
| Files | kebab-case | `user-profile.tsx` |
| Tests | *.test.tsx | `user-profile.test.tsx` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |

### Code Style
- Functional components only
- Define Props with interface
- Error handling required (try-catch)
- Comments in English

## Git Rules

### Branch Strategy
- `main`: Production deployment
- `develop`: Development integration
- `feature/*`: Feature development
- `fix/*`: Bug fixes
- `hotfix/*`: Emergency fixes

### Commit Messages
Use Conventional Commits:
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `refactor:` Refactoring
- `test:` Tests
- `chore:` Other

Example: `feat(auth): Add social login support`

### PR Rules
- PR required: No direct push to main
- Reviewers: Minimum 1
- CI must pass

## Common Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Dev server |
| `npm test` | Tests |
| `npm run lint` | Lint |
| `npm run build` | Build |
| `npm run typecheck` | Type check |

## Important Folders

| Folder | Description |
|--------|-------------|
| `src/components/` | Reusable components |
| `src/features/` | Feature modules |
| `src/hooks/` | Custom hooks |
| `src/utils/` | Utility functions |
| `src/types/` | TypeScript types |
| `src/api/` | API calls |

## Claude Code Usage Rules

### Required Checks
- Run tests before code changes
- Call @security-expert when modifying security code
- Team discussion before major refactoring

### Prohibited
- Direct manipulation of production DB
- Modifying environment variable files (.env)
- Arbitrary package.json modifications
```

---

## 🔥 Pro Tip

> **Team CLAUDE.md Maintenance Tips**
>
> 1. **Designate a single maintainer**: One person manages CLAUDE.md
> 2. **Regular updates**: Review and update each sprint
> 3. **Version control**: Record change history
> 4. **Collect feedback**: Incorporate team improvement suggestions
>
> ```markdown
> ## Change History
> | Date | Content | Author |
> |------|---------|--------|
> | 2024-01-15 | Initial creation | @kim |
> | 2024-02-01 | Added Git rules | @lee |
> | 2024-02-15 | Updated test rules | @park |
> ```

---

## Team Commands

### Standard Workflow

```markdown
<!-- .claude/commands/pr.md -->
# PR Creation Workflow

## Pre-checks
Automatically verify the following:
- [ ] Tests passing: $(npm test)
- [ ] Lint passing: $(npm run lint)
- [ ] Type check: $(npm run typecheck)
- [ ] Build successful: $(npm run build)

## PR Title Format
```
<type>(<scope>): <description>

Examples:
feat(auth): Add social login
fix(cart): Fix quantity update bug
```

## PR Body Template
```markdown
### Changes
- What was changed

### Reason for Change
- Why this change was needed

### How to Test
1. Step-by-step test instructions

### Screenshots
(Attach for UI changes)

### Checklist
- [ ] Tests added/modified
- [ ] Documentation updated
- [ ] Migration needed check
```

## How to Run
```
> /pr
```
```

### Code Review Command

```markdown
<!-- .claude/commands/review.md -->
# Code Review

## Review Scope
Changed files:
$(git diff --name-only HEAD~1)

## Review Perspectives

### 1. Functionality
- Does it meet requirements?
- Are edge cases handled?

### 2. Code Quality
- Does it follow team conventions?
- Is there duplicate code?
- Are function/variable names clear?

### 3. Performance
- Are there unnecessary operations?
- Is there potential memory leak?

### 4. Security
- Is user input validated?
- No sensitive information exposure?

### 5. Testing
- Are tests sufficient?
- Is test coverage adequate?

## Feedback Format
```
[Severity] file:line - Description

Examples:
[Required] auth.ts:45 - Password validation logic needed
[Recommended] user.ts:23 - Make variable name clearer
[Note] api.ts:89 - There's also this approach
```

## How to Run
```
> /review
```
```

### Onboarding Helper

```markdown
<!-- .claude/commands/onboarding.md -->
# New Member Onboarding

Welcome! You must be new here.

## 1. Environment Setup
```bash
# Clone repository
git clone [repo-url]
cd [project-name]

# Install dependencies
npm install

# Set environment variables
cp .env.example .env.local
# Edit .env.local file (ask team)

# Run dev server
npm run dev
```

## 2. Project Structure
$(tree -L 2 src/)

## 3. Required Reading
- [ ] CLAUDE.md: Project rules
- [ ] README.md: Project introduction
- [ ] .claude/commands/: Available commands

## 4. Available Commands
| Command | Description |
|---------|-------------|
| /pr | Create PR |
| /review | Code review |
| /issue-start <number> | Start issue |
| /debug | Debugging help |

## 5. Available Agents
| Agent | Description |
|-------|-------------|
| @senior-reviewer | Senior perspective review |
| @junior-helper | Junior helper |
| @security-expert | Security expert |

## 6. First Task
1. Select an issue with `good-first-issue` label
2. Start with `/issue-start <number>`
3. Create PR with `/pr` after implementation

## 7. Ask Questions
- Slack: #frontend-dev
- Team meeting: Every Monday 10am

Ask anytime if you have questions!
```

```
> /onboarding
```

### Issue Start Command

```markdown
<!-- .claude/commands/issue-start.md -->
# Start Issue

Issue number: $ARGUMENTS

## 1. Check Issue
$(gh issue view $ARGUMENTS)

## 2. Create Branch
```bash
git checkout develop
git pull origin develop
git checkout -b feature/$ARGUMENTS
```

## 3. Work Plan
Analyze the issue content and create a work plan:

### Analysis
- Summarize requirements
- Identify impact scope

### Tasks
- [ ] Task 1
- [ ] Task 2
- [ ] Write tests
- [ ] Update documentation

### Estimated Time
- Estimate: X hours

## 4. Start
Begin implementation once plan is confirmed.

Use `/pr` to create a PR when complete.
```

```
> /issue-start 42
```

---

## ⚠️ Caution

> **Notes When Sharing Team Settings**
>
> 1. **Exclude sensitive information**: Add API keys, passwords, etc. to .gitignore
>    ```
>    # .gitignore
>    .env
>    .env.local
>    *.secret
>    ```
>
> 2. **Separate personal settings**: Distinguish team and personal settings
>    - Team settings: `.claude/` (commit)
>    - Personal settings: `~/.claude/` (don't commit)
>
> 3. **Watch for version conflicts**: Conflicts possible when multiple people edit simultaneously
>    - Have one person manage, or
>    - Manage changes via PR

---

## Team Agents

### Senior Reviewer

```markdown
<!-- .claude/agents/senior-reviewer.md -->
# Senior Developer

## Role
You are a 10-year senior developer.
Do thorough code reviews, but give constructive feedback.

## Review Perspective

### Architecture
- Is the design appropriate
- Is it scalable
- Is dependency management proper

### Performance
- Any unnecessary operations
- Any N+1 query issues
- Is memory usage appropriate

### Maintainability
- Is it easy for others to understand
- Is there code duplication
- Is the structure easy to test

### Testing
- Is it sufficiently tested
- Are edge cases covered
- Are tests independent

## Feedback Style
- Present problems with improvement directions
- "This could be better" instead of "This is bad"
- Mention what's done well too
- Attach learning resource links

## Usage Examples
```
@senior-reviewer check this PR
@senior-reviewer review this architecture design
```
```

### Junior Helper

```markdown
<!-- .claude/agents/junior-helper.md -->
# Junior Helper

## Role
Help junior developers.
Explain difficult concepts simply and guide step by step.

## Principles
- It's okay not to know
- Use many examples
- Explain the why
- Encourage questions
- Include term explanations

## Explanation Style
1. Core concept first (in one sentence)
2. Simple analogy
3. Simple example code
4. Apply to actual project
5. Suggest practice problems

## Prohibited
- Just listing difficult technical terms
- Expressions like "This is basic..."
- Giving only answers (without explaining process)

## Usage Examples
```
@junior-helper What is React's useEffect?
@junior-helper What does this error message mean?
@junior-helper Teach me how to write tests
```
```

### Security Expert

```markdown
<!-- .claude/agents/security-expert.md -->
# Security Expert

## Role
Review code from a security perspective.
Find potential vulnerabilities and suggest solutions.

## Review Items

### Authentication/Authorization
- Is authentication logic secure
- Is permission checking appropriate
- Is session management secure

### Input Validation
- Is user input validated
- SQL injection prevention
- XSS prevention
- CSRF prevention

### Data Protection
- Is sensitive data encrypted
- Is sensitive info exposed in logs
- Is internal info in error messages

### Dependencies
- Any packages with vulnerabilities
- Are latest security patches applied

## Report Format
```
[Severity: High/Medium/Low]
Location: file:line
Issue: Description
Impact: What could happen
Solution: How to fix
Reference: OWASP etc.
```

## Usage Examples
```
@security-expert review this auth code
@security-expert check this API security
```
```

---

## Collaboration Workflow

### Issue → Implement → Review → Merge

```
┌─────────────────────────────────────────────────────────────────┐
│                        Team Workflow                              │
└─────────────────────────────────────────────────────────────────┘

 1. Issue Assigned
      │
      ▼
┌──────────────┐
│ /issue-start │  ← Create branch, analyze issue
│   #42        │
└──────┬───────┘
       │
       ▼
 2. Implementation
      │ Develop while talking with Claude
      │
      ▼
┌──────────────┐
│ /pr          │  ← Create PR, auto test
└──────┬───────┘
       │
       ▼
 3. Review
      │
      ▼
┌──────────────┐
│@senior-      │  ← Code review
│ reviewer     │
└──────┬───────┘
       │ Apply feedback
       │
       ▼
 4. Approval & Merge
      │
      ▼
┌──────────────┐
│ CI/CD        │  ← Auto test, auto deploy
│ Pipeline     │
└──────┬───────┘
       │
       ▼
 5. Complete!
      │
      ▼
┌──────────────┐
│ Slack Alert  │  ← "PR #123 has been merged"
└──────────────┘
```

### Detailed Workflow Example

```
1. Start Issue
   > /issue-start 42
   Claude: Analyzing issue #42...
           Creating branch feature/42...
           Suggesting work plan...

2. During Development
   > Create a login form component
   Claude: [Writes code]

   > @junior-helper explain this part
   Claude: [Friendly explanation]

3. Code Review
   > @senior-reviewer check this code
   Claude: [Detailed review]
           - Good points: ...
           - Improvements: ...

   > @security-expert do a security review
   Claude: [Security review]
           - Issues found: ...
           - Solutions: ...

4. Create PR
   > /pr
   Claude: [Run auto tests]
           [Generate PR body]
           [PR creation complete]

5. After Merge
   CI/CD: Auto deploy
   Slack: "Issue #42 deployment complete!"
```

---

## Team Onboarding Automation

### New Member Checklist

```markdown
<!-- .claude/commands/new-member.md -->
# New Member Checklist

Welcome! Follow the steps below in order.

## Day 1: Environment Setup

### Required Installations
- [ ] Node.js 20.x
- [ ] Git
- [ ] VS Code
- [ ] Claude Code CLI

### Repository Setup
- [ ] Clone Git repository
- [ ] Set up dev environment (`npm install`)
- [ ] Configure environment variables (`.env.local`)
- [ ] Verify local run (`npm run dev`)

### Read Documents
- [ ] Read CLAUDE.md
- [ ] Read README.md
- [ ] Check .claude/commands/ folder

### Join Team Channels
- [ ] Slack #frontend-dev
- [ ] GitHub team addition

## Day 2: First Task

### Understand Project
- [ ] Understand project structure
- [ ] Understand main feature flow
- [ ] Read existing code

### First Contribution
- [ ] Get assigned a `good-first-issue`
- [ ] Start with `/issue-start <number>`
- [ ] Submit PR after small fix

## Week 1: Join Team

### Team Activities
- [ ] Attend team meeting (Monday 10am)
- [ ] Pair programming 1 time
- [ ] Try reviewing another team member's PR

### Learning
- [ ] Analyze main feature code
- [ ] Read architecture documentation
- [ ] Practice writing test code

## Ask for Help
- Technical questions: @senior-developer
- Process questions: @team-lead
- General questions: #frontend-dev
- Claude help: @junior-helper

Ask anytime if you have questions!
```

### Environment Setup Automation

```markdown
<!-- .claude/commands/setup.md -->
# Dev Environment Setup

## 1. Check Required Programs
```bash
# Check Node.js version (20.x required)
node --version

# Check npm version
npm --version

# Check Git version
git --version
```

## 2. Install Dependencies
```bash
npm install
```

## 3. Set Environment Variables
```bash
cp .env.example .env.local
```

Set the following in `.env.local`:

| Variable | Description | How to Get |
|----------|-------------|------------|
| DATABASE_URL | Local DB address | Ask team |
| API_KEY | Dev API key | Ask team |
| NEXT_PUBLIC_API_URL | API server address | Usually localhost:3001 |

## 4. Database Setup
```bash
# Run migrations
npm run db:migrate

# Add seed data
npm run db:seed
```

## 5. Run Dev Server
```bash
npm run dev
```

## 6. Verify
- Access http://localhost:3000
- Login test: test@example.com / test1234

## Having Problems?
- Ask in #frontend-dev with the error message
- Or ask Claude with `@junior-helper what's this error?`
```

---

## Team Rules Documentation

### Claude Usage in README

```markdown
# Project README

## Claude Code Usage

This project increases development productivity with Claude Code.

### Getting Started
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Run in project folder
claude
```

### Available Commands

| Command | Description | When to Use |
|---------|-------------|-------------|
| `/pr` | Create PR | After completing feature |
| `/review` | Code review | Before commit |
| `/issue-start <number>` | Start issue | When starting work |
| `/onboarding` | Onboarding guide | New members |
| `/setup` | Environment setup | Initial setup |
| `/debug` | Debugging help | When errors occur |

### Available Agents

| Agent | Description | When to Use |
|-------|-------------|-------------|
| `@senior-reviewer` | Senior perspective review | Before PR |
| `@junior-helper` | Friendly explanations | Learning concepts |
| `@security-expert` | Security review | Security code |

### Team Rules

#### Code Changes
- All code changes via PR
- Self-review with `/review` before PR
- Merge after reviewer approval

#### Commits
- Conventional Commits format
- Verify tests pass before commit
- Split large changes into small commits

#### Reviews
- Respond to reviews within 24 hours
- Give constructive feedback
- Mention good points too

### Example Workflow

```bash
# 1. Start issue
> /issue-start 42

# 2. Develop (talk with Claude)
> Create a login form

# 3. Self-review
> /review

# 4. Security review (if needed)
> @security-expert check this code

# 5. Create PR
> /pr

# 6. Wait after assigning reviewer
```
```

---

## Real Conversation Examples with Claude

### Example 1: New Member Onboarding

```
New member: "/onboarding"

Claude: Hello! Welcome to the Frontend Team!

        Starting onboarding.

        📋 Day 1 Checklist:

        1. ✅ Environment setup
           - Node.js 20.x installed
           - npm 9.x installed

        2. ⏳ Repository setup
           Please run these commands:
           ```bash
           npm install
           cp .env.example .env.local
           ```

        3. ⏳ Environment variable setup
           You need to set the following in .env.local:
           - DATABASE_URL
           - API_KEY

           These values are in a pinned message in #frontend-dev.

        Let me know when environment variables are set!

New member: "Setup complete!"

Claude: Great! Let's start the dev server.

        ```bash
        npm run dev
        ```

        ✅ Server started!
        Check at http://localhost:3000.

        📚 Next steps:
        1. Read the CLAUDE.md file (project rules)
        2. Check issues with `good-first-issue` label

        Use `/issue-start <issue number>` to start your first task!

New member: "@junior-helper What is React's useEffect?"

Claude: [Junior helper mode]

        Great question!

        🎯 One-line summary:
        useEffect defines "things to do after the component renders."

        🎭 Simple analogy:
        Like a movie theater:
        - After the movie (render) starts
        - Do additional tasks like "turn on subtitles", "dim lights"

        💻 Simple example:
        ```tsx
        function Profile({ userId }) {
          const [user, setUser] = useState(null);

          // Fetch user info whenever userId changes
          useEffect(() => {
            fetchUser(userId).then(data => setUser(data));
          }, [userId]);  // Dependency array

          return <div>{user?.name}</div>;
        }
        ```

        📝 Key points:
        1. First argument: Function to execute
        2. Second argument: Dependency array (runs when these values change)

        Want to see examples in our project?
```

### Example 2: Team PR Workflow

```
Developer: "/issue-start 42"

Claude: Analyzing issue #42.

        📌 Issue: "Add password visibility toggle to login form"

        Status: Open
        Assignee: @developer
        Labels: enhancement, good-first-issue

        🌿 Creating branch:
        ```bash
        git checkout develop
        git pull origin develop
        git checkout -b feature/42-password-toggle
        ```

        ✅ Branch created!

        📋 Work plan:
        1. Modify PasswordInput component
        2. Add eye icon (heroicons)
        3. Manage toggle state
        4. Write tests

        Estimated time: 2 hours

        Should we start?

Developer: "Yes, let's start"

Claude: [Writing code...]

        Modified PasswordInput component:

        ```tsx
        // src/components/PasswordInput.tsx
        export function PasswordInput({ ... }) {
          const [showPassword, setShowPassword] = useState(false);

          return (
            <div className="relative">
              <input
                type={showPassword ? "text" : "password"}
                ...
              />
              <button
                type="button"
                onClick={() => setShowPassword(!showPassword)}
                aria-label={showPassword ? "Hide password" : "Show password"}
              >
                {showPassword ? <EyeOffIcon /> : <EyeIcon />}
              </button>
            </div>
          );
        }
        ```

        Should I write tests too?

Developer: "Yes, and /review please"

Claude: [After writing tests, running review]

        📝 Code Review Results:

        ✅ Good points:
        - Accessibility considered (aria-label)
        - Clean state management
        - Good test coverage

        ⚠️ Improvement suggestions:
        1. [Recommended] Add minimum password length guidance
        2. [Note] Match icon size with design system

        📊 Automatic checks:
        - Lint: ✅ Passed
        - Test: ✅ Passed (3/3)
        - TypeCheck: ✅ Passed

        Should I create the PR?

Developer: "/pr"

Claude: Creating PR.

        📝 PR Info:

        Title: feat(auth): Add password visibility toggle

        Body:
        ```markdown
        ## Changes
        - Added show/hide password toggle to login form
        - Added aria-label for accessibility
        - Added test cases

        ## Related Issue
        Closes #42

        ## How to Test
        1. Go to login page
        2. Enter password
        3. Click eye icon to verify toggle

        ## Screenshots
        [Attached]
        ```

        ✅ PR #123 created!
        https://github.com/team/project/pull/123

        Added @senior-dev as reviewer.
```

### Example 3: Security Review

```
Developer: "@security-expert review this auth code"

Claude: [Security expert mode]

        🔒 Security Review Results

        Reviewed file: src/api/auth.ts

        ---

        [Severity: High]
        📍 Location: auth.ts:23
        🔴 Issue: SQL Injection vulnerability
        ```typescript
        // Current code
        const query = `SELECT * FROM users WHERE email = '${email}'`;
        ```

        💥 Impact: Attacker can execute arbitrary SQL

        ✅ Solution:
        ```typescript
        // Fixed code
        const query = 'SELECT * FROM users WHERE email = $1';
        const result = await db.query(query, [email]);
        ```

        📚 Reference: OWASP SQL Injection

        ---

        [Severity: Medium]
        📍 Location: auth.ts:45
        🟡 Issue: Internal info exposed in error message
        ```typescript
        catch (error) {
          throw new Error(`DB connection failed: ${error.message}`);
        }
        ```

        ✅ Solution:
        ```typescript
        catch (error) {
          console.error('DB connection failed:', error);  // Log only
          throw new Error('Authentication failed');  // To user
        }
        ```

        ---

        📊 Summary:
        - High: 1 issue (immediate fix required)
        - Medium: 1 issue (quick fix recommended)
        - Low: 0 issues

        Should I apply the fixed code?
```

---

## 🎯 Mini Quiz

### Quiz 1: Benefits of Team Standardization
Which is NOT a benefit of standardizing Claude Code in a team?

A) Consistent code quality
B) Easier onboarding for new members
C) Ability to configure per personal preference
D) Automated knowledge sharing

<details>
<summary>See Answer</summary>

**Answer**: C) Ability to configure per personal preference

**Explanation**: The core of team standardization is prioritizing team-wide consistency over personal preferences. Of course, you can add personal settings on top of team standards, but the base should follow team standards.
</details>

### Quiz 2: Files to Share
Which file should NOT be committed to Git and shared with the team?

A) CLAUDE.md
B) .claude/commands/
C) .env.local
D) .claude/agents/

<details>
<summary>See Answer</summary>

**Answer**: C) .env.local

**Explanation**: .env.local contains sensitive information like API keys and passwords, so it should not be committed to Git. It should be added to .gitignore.
</details>

### Quiz 3: Commands Usage
What Command should a new team member use when starting the project?

<details>
<summary>See Answer</summary>

**Answer**: `/onboarding` or `/setup`

**Explanation**:
- `/onboarding`: Full onboarding process guide
- `/setup`: Development environment setup guide
- `/new-member`: Checklist provision

New members can quickly adapt to the project through these Commands.
</details>

### Quiz 4: Agents Roles
Which Agent should you call when you want a security review?

<details>
<summary>See Answer</summary>

**Answer**: `@security-expert`

**Explanation**: Each Agent has a specialized role:
- `@senior-reviewer`: General code review, architecture
- `@junior-helper`: Concept explanation, learning help
- `@security-expert`: Security review, vulnerability analysis
</details>

### Quiz 5: Workflow Order
What is the correct team workflow order?

<details>
<summary>See Answer</summary>

**Answer**: Start issue → Implement → Review → Create PR → Merge

**Detailed order**:
1. `/issue-start <number>`: Create branch, work plan
2. Implement: Develop while talking with Claude
3. `/review`: Self-review
4. `/pr`: Create PR
5. Merge after reviewer approval
6. CI/CD auto deploy
</details>

---

## 📝 Practice Exercises

### Exercise 1: Basics - Write Team CLAUDE.md [Difficulty: ★☆☆]

**Goal**: Write a team CLAUDE.md file

**Steps**:
1. Create CLAUDE.md in project root
2. Include these sections:
   - Team info
   - Tech stack
   - Coding conventions
   - Git rules
   - Common commands
3. Get review from team member

**Checklist**:
- [ ] New member can read and start working immediately
- [ ] Team rules clearly defined
- [ ] Contact/question methods included

### Exercise 2: Intermediate - Create Team Commands [Difficulty: ★★☆]

**Goal**: Write 3 Commands for team workflow

**Required Commands**:
1. `/pr`: PR creation workflow
2. `/review`: Code review checklist
3. `/onboarding`: New member onboarding

**Steps**:
1. Create `.claude/commands/` folder
2. Write each Command file
3. Test with team members
4. Incorporate feedback

**Checklist**:
- [ ] All team members can use Commands
- [ ] Consistent output results
- [ ] Usage documented in README

### Exercise 3: Advanced - Build Complete Team Setup [Difficulty: ★★★]

**Goal**: Build complete team standard environment

**Components**:
1. CLAUDE.md (project rules)
2. Commands (5 or more)
3. Agents (3 or more)
4. settings.json (team settings)
5. README documentation

**Steps**:
1. Discuss standards with team
2. Write each component
3. Test and get feedback
4. Document and train
5. Establish regular update process

**Checklist**:
- [ ] New members productive from day one
- [ ] Improved code review quality
- [ ] Team-wide consistency secured

---

## 🏆 Challenge Tasks

### Challenge 1: Onboarding Automation

Fully automate a new member's first week:

1. Day 1 environment setup automation
2. Day 2 first task guide
3. Week 1 checklist
4. Auto-track progress

### Challenge 2: Quality Gate Setup

Automated quality check system before PR:

1. Auto lint check
2. Auto tests
3. Security scan
4. Code coverage check
5. Auto review request

### Challenge 3: Team Metrics Dashboard

Claude Code usage dashboard:

1. Commands usage statistics
2. Productivity metrics
3. Code quality trends
4. Usage patterns per team member

---

## Summary

What you learned in this chapter:
- [x] Team settings standardization (.claude/ folder)
- [x] Team Commands and Agents
- [x] Building collaboration workflows
- [x] Onboarding automation
- [x] Team rules documentation

**Key point**: Good team settings let all members work consistently. Once well established, it continuously provides value.

---

## Glossary

| Term | Description |
|------|-------------|
| **Standardization** | Establishing rules so the entire team works the same way |
| **Onboarding** | Process of new members adapting to the project |
| **Commands** | Pre-defined workflows executed in `/command` format |
| **Agents** | AI personas with specific roles called with `@name` format |
| **CLAUDE.md** | File that defines project rules and context |
| **Playbook** | Document organizing team's standard work methods |
| **Workflow** | Series of steps to perform work |
| **Code Review** | Process of reviewing others' code |

---

## Quick Tips for Success

1. **Start small** - Don't create 20 commands at once. Start with 2-3 essential ones.

2. **Document everything** - If it's not in the README, it doesn't exist for new members.

3. **Iterate** - Your first version won't be perfect. Improve based on feedback.

4. **Lead by example** - Use the team tools yourself consistently.

5. **Celebrate wins** - When team tools save time, share that success!

---

# Complete Curriculum Summary

## Vibecoding Curriculum Complete!

Congratulations! You've completed all 27 chapters!

### Looking Back at Your Learning Journey

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Vibecoding Curriculum Full Structure              │
└─────────────────────────────────────────────────────────────────────┘

Part 1: Basics (Chapter 1-10)
├── Chapter 1-3: Installation and first start
├── Chapter 4-6: Master basic usage
└── Chapter 7-10: Complete first project

Part 2: Practical (Chapter 11-20)
├── Chapter 11-13: Website development
├── Chapter 14-16: App development
└── Chapter 17-20: Deployment and operations

Part 3: Advanced (Chapter 22-27)
├── Chapter 22: Advanced configuration
├── Chapter 23: Hooks & Commands
├── Chapter 24: Agents & Skills
├── Chapter 25: MCP integration
├── Chapter 26: CI/CD automation
└── Chapter 27: Team collaboration ← You are here!
```

### Complete Summary of Core Concepts Learned

| Chapter Range | Core Concepts | Practical Application |
|---------------|---------------|----------------------|
| 1-10 | Claude Code basics | Complete first project |
| 11-15 | Web development | Build websites/apps |
| 16-20 | Deployment/operations | Run real services |
| **22** | **CLAUDE.md, settings.json** | **Define project rules** |
| **23** | **Hooks, Commands** | **Automation workflows** |
| **24** | **Agents, Skills** | **Role-based work** |
| **25** | **MCP** | **External service integration** |
| **26** | **CI/CD** | **Auto test/deploy** |
| **27** | **Team collaboration** | **Team standardization** |

### What You Can Do Now

#### Basic Skills
- Install and configure Claude Code
- Request coding in natural language
- Create, modify, delete files
- Understand project structure

#### Development Skills
- Build websites/apps
- API integration
- Database work
- Write tests

#### Operations Skills
- Deployment automation
- CI/CD pipelines
- Monitoring setup
- Problem solving

#### Advanced Skills
- Custom configuration (CLAUDE.md)
- Automation Hooks
- Reusable Commands
- Agent personas
- MCP service integration

#### Collaboration Skills
- Build team standards
- Onboarding automation
- Code review process
- Knowledge sharing system

---

## Next Learning Steps

### Level 1: Deep Dive

#### Claude Code Official Documentation
- [Claude Code Official Docs](https://docs.anthropic.com/claude-code)
- Check for latest feature updates
- Advanced configuration options

#### MCP Server Development
- Build custom MCP servers
- Connect company internal APIs
- Develop specialized tools

### Level 2: Real Projects

#### Recommended Projects
1. **Personal Portfolio Site**
   - Static site generation
   - Responsive design
   - Deployment automation

2. **Team Productivity Tool**
   - Issue tracker
   - Document management system
   - Dashboard

3. **Automation Bot**
   - Slack bot
   - GitHub bot
   - Monitoring bot

### Level 3: Community Participation

#### Ways to Contribute
1. **Bug Reports**
   - Register issues when problems found
   - Describe reproduction steps in detail

2. **Feature Suggestions**
   - Share new ideas
   - Explain use cases

3. **Documentation Contribution**
   - Fix typos/errors
   - Add examples
   - Participate in translation

4. **Share Experiences**
   - Write blog posts
   - Create tutorials
   - Answer community questions

---

## Recommended Learning Resources

### Official Resources

| Resource | Link | Description |
|----------|------|-------------|
| Claude Code Docs | docs.anthropic.com | Official documentation |
| Anthropic Blog | anthropic.com/blog | Latest news |
| GitHub Examples | github.com/anthropic | Example code |

### Community Resources

| Resource | Description |
|----------|-------------|
| Discord | Real-time Q&A |
| Reddit | Experience sharing, tips |
| YouTube | Tutorial videos |
| Dev.to | Blog posts |

### Related Technology Learning

| Technology | Recommended Resource |
|------------|---------------------|
| Git/GitHub | GitHub Docs |
| JavaScript/TypeScript | MDN Web Docs |
| React | React Official Docs |
| Node.js | Node.js Official Docs |
| GitHub Actions | GitHub Actions Docs |

---

## Community Participation

### Discord Community

[![Discord](https://img.shields.io/badge/Discord-Vibecoding_Community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

**Channel Introduction**:
- `#questions`: Questions during learning
- `#project-share`: Show off projects you made
- `#tips-share`: Share useful tips
- `#job-career`: Job info exchange

### GitHub Repository

- Issue registration: Bugs, feature suggestions
- PR: Document improvements, error fixes
- Star: Project support

### Share Your Experience

Please share your learning experience:
- Write blog posts
- Create tutorial videos
- Share on social media (#vibecoding)

---

## Congratulations

You've completed all 27 chapters of the Vibecoding Curriculum!

Now you can use Claude Code to:
- Build and deploy websites
- Create apps and automation tools
- Configure advanced workflows (Hooks, Skills, MCP)
- Build CI/CD pipelines
- Collaborate effectively with your team

### Remember

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Final Advice                                 │
└─────────────────────────────────────────────────────────────────────┘

🎯 You don't need to be perfect from the start
   Start with small projects.
   It's okay to fail. It's part of learning.

🔄 Keep improving with feedback
   Code, settings, workflows - all of them
   don't need to be perfect on the first version.

🤝 Claude is always ready to help
   When you're stuck, when you don't know, ask anytime.
   "What is this?", "How do I do this?" All questions welcome!

💪 Now go out and build amazing things
   Your ideas become reality.
   Enjoy the fun of coding!
```

---

**Thank you!**

Thank you for being with the Vibecoding Curriculum.

We support your development journey!

---

[![Discord](https://img.shields.io/badge/Discord-Join_Community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)
[![GitHub](https://img.shields.io/badge/GitHub-Star_Project-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/your-repo)

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
