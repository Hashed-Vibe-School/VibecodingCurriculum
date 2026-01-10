# Chapter 20: Mastery

**English** | [한국어](./README.ko.md)

## Congratulations

You have completed all 20 chapters.

Now you can create various projects using Claude Code.

---

## What You Have Learned

### Part 1: Getting Started (Chapter 01-05)

- AI coding and vibecoding concepts
- Installing Claude Code and first conversation
- Reading and writing files
- Terminal commands

### Part 2: Core Features (Chapter 06-10)

- Effective prompting
- Exploring code (Glob, Grep)
- Editing code
- Git basics
- Project memory (CLAUDE.md)

### Part 3: Practical Projects I (Chapter 11-14)

- Website development
- Deployment (Vercel, Railway)
- Data storage (Supabase)
- Making mini games

### Part 4: Practical Projects II (Chapter 15-17)

- Building AI tools
- Data processing
- API integration

### Part 5: Advanced & Mastery (Chapter 18-20)

- Advanced features (Hooks, Skills)
- Automation (MCP, CI/CD)
- Mastery (now!)

---

## Core Principles Review

### 1. Be Specific

```
# Bad example
> Fix the bug

# Good example
> There's a bug in @src/login.js where email validation doesn't work.
> Invalid emails like "test@" pass through.
> Fix it to check that domain has a dot (.)
```

### 2. Work Step by Step

```
# Step 1: Understand
> Explain this project structure

# Step 2: Plan
> How should I build the login feature? Don't write code yet.

# Step 3: Execute
> Good, build it according to that plan

# Step 4: Verify
> Run the tests
```

### 3. Improve with Feedback

The first result does not need to be perfect.

```
> Button is too small. Make it bigger.
> Don't like the color. Change it to blue.
> Add hover effect.
```

### 4. Use CLAUDE.md

Create a CLAUDE.md for each project.

```markdown
# My Project

## Tech Stack
- React, TypeScript, Tailwind

## Rules
- Use functional components
- English comments
```

### 5. Stay Safe with Git

Always commit before big changes:

```
> Commit current state. Backup before refactoring.
```

---

## Common Workflows

### Starting a New Project

```
1. > Create project folder
2. > Initialize npm/git
3. > Create CLAUDE.md
4. > Create basic file structure
```

### Bug Fixing

```
1. > Why am I getting this error? [error message]
2. > Find the cause
3. > Fix it
4. > Test it
```

### Adding New Features

```
1. > How should I build this feature?
2. > Build it according to that plan
3. > Add tests
4. > Commit
```

### Deployment

```
1. > Build
2. > All tests passing?
3. > Push to GitHub
4. > (Auto-deploy on Vercel)
```

---

## Tips for Improving

### 1. Start Small

```
# What to build today
- [ ] Button component
- [ ] Simple form
- [ ] API call
```

### 2. Practice Daily

30 minutes a day consistently is best.

### 3. Read Others' Code

Analyze open source projects on GitHub with Claude:

```
> Explain this project structure
> What does this function do?
```

### 4. Failure is Part of Learning

```
> Esc Esc  # Undo
> Retry with a different approach...
```

---

## Next Steps

### If You Want to Learn More

1. **Official docs**: [code.claude.com/docs](https://code.claude.com/docs)
2. **Example projects**: Search for Claude Code examples on GitHub
3. **Community**: Learn from how others use it

### Projects to Challenge

- [ ] Personal blog
- [ ] Todo app (Full-stack)
- [ ] Real-time chat app
- [ ] AI chatbot service
- [ ] Automation dashboard

### Get Involved

- **Feedback**: Help improve this curriculum
- **Share**: Share projects you've made
- **Help**: Help other beginners

---

## Final Practice: Comprehensive Project

Use everything you've learned to complete one project.

### Recommended Project: Personal Dashboard

```
> Create a personal dashboard.
> - Todo list
> - Today's weather
> - Recent GitHub activity
> - Notepad

> Store data with Supabase
> Deploy with Vercel
> Write good CLAUDE.md
> Version control with Git
```

### Project Checklist

- [ ] Planning: Define features
- [ ] Development: Implement one by one
- [ ] Testing: Verify it works
- [ ] Deployment: Publish to internet
- [ ] Sharing: Show to others

---

## Thank You

You have completed this curriculum.

With Claude Code, you can turn any idea into reality.

**Remember:**
- You do not need to be perfect from the start
- Keep improving with feedback
- Claude is always ready to help

---

## Appendix: Useful Links

### Official Resources

- [Claude Code Official Docs](https://code.claude.com/docs)
- [Anthropic Blog](https://www.anthropic.com/blog)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)

### Reference Materials

- [MDN Web Docs](https://developer.mozilla.org/) - Web development
- [JavaScript.info](https://javascript.info/) - JavaScript tutorial
- [React Official Docs](https://react.dev/) - React

### Tools

- [Vercel](https://vercel.com) - Deployment
- [Supabase](https://supabase.com) - Database
- [GitHub](https://github.com) - Code repository

---

**The End. Now go build.**
