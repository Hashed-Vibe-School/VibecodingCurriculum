# Chapter 09: Subagents & Skills

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, ensure you:
- [ ] Have completed Chapter 00-08
- [ ] Understand slash commands and hooks
- [ ] Are comfortable creating markdown configuration files

---

## Introduction

Subagents and Skills are advanced features that let you create specialized AI assistants and teach Claude domain-specific knowledge. Subagents are independent workers for specific tasks, while Skills are knowledge modules that activate automatically.

### Subagents vs Skills

| Feature | Subagents | Skills |
|---------|-----------|--------|
| **Purpose** | Delegate specific tasks | Add domain knowledge |
| **Activation** | Explicit (you call them) | Automatic (context-based) |
| **Context** | Separate conversation | Shared conversation |
| **Tools** | Configurable per agent | Configurable per skill |

---

## Topics

### 1. Understanding Subagents

Subagents are specialized AI assistants that:
- Have their own conversation context
- Can be configured with specific tools
- Run independently and report back

**Built-in Subagents**:
| Name | Model | Purpose |
|------|-------|---------|
| `Explore` | Haiku | Fast codebase exploration |
| `Plan` | Sonnet | Implementation planning |
| `General-purpose` | Sonnet | Complex multi-step tasks |

### 2. Creating Custom Subagents

Create subagents in `.claude/agents/`:

```markdown
<!-- .claude/agents/code-reviewer.md -->
---
name: code-reviewer
description: Expert code review specialist. Use for thorough code reviews.
tools: Read, Grep, Glob
model: sonnet
---

# Code Reviewer Agent

## Role
You are an expert code reviewer focused on quality, security, and maintainability.

## Review Checklist
1. **Security**: Look for vulnerabilities
2. **Performance**: Identify bottlenecks
3. **Style**: Check consistency
4. **Logic**: Verify correctness

## Output Format
Provide structured feedback with:
- Severity (Critical/Major/Minor)
- Location (file:line)
- Issue description
- Suggested fix
```

### 3. Subagent Configuration

**Frontmatter Options**:
```yaml
---
name: agent-name           # Required: identifier (lowercase, hyphens)
description: Brief desc    # Required: when to use
tools: Read, Grep, Bash    # Optional: comma-separated tool list
model: sonnet              # Optional: sonnet, opus, haiku, inherit
permissionMode: default    # Optional: permission mode
skills: skill1, skill2     # Optional: skills to auto-load
---
```

**Tool Restrictions**:
- `Read, Grep, Glob` - Read-only agent
- `Read, Edit, Write` - Can modify files
- `Bash` - Can run commands
- Omit to inherit all tools

### 4. Using Subagents

```bash
# Explicitly invoke
> Have the code-reviewer agent review @src/auth/

# Claude may suggest using one
> I need a thorough code review
# Claude: I'll use the code-reviewer agent for this...

# List available agents
/agents
```

### 5. Understanding Skills

Skills are knowledge modules that Claude loads automatically when relevant:

```markdown
<!-- .claude/skills/react-patterns/SKILL.md -->
---
name: react-patterns
description: React best practices and patterns for this project
allowed-tools: Read, Grep
---

# React Patterns for This Project

## Component Structure
- Use functional components with hooks
- Place hooks at the top of components
- Extract logic into custom hooks

## State Management
- Use useState for local state
- Use React Query for server state
- Avoid Redux unless necessary

## File Naming
- Components: PascalCase.tsx
- Hooks: useHookName.ts
- Utils: camelCase.ts
```

### 6. Skill Structure

```
.claude/skills/
├── react-patterns/
│   ├── SKILL.md          # Main skill file (required)
│   ├── examples/         # Code examples
│   │   └── hooks.md
│   └── anti-patterns.md  # What to avoid
├── api-design/
│   └── SKILL.md
└── testing/
    └── SKILL.md
```

### 7. When Skills Activate

Skills activate automatically when Claude detects:
- Keywords in your prompt
- Related file types
- Context matching the skill description

```bash
> Create a new React component

# Claude automatically loads react-patterns skill
# and follows the defined conventions
```

### 8. Skills vs Other Features

| Feature | Auto-Activate | Scope | Best For |
|---------|--------------|-------|----------|
| **Skills** | Yes | Domain knowledge | Project conventions |
| **Commands** | No (explicit) | Reusable prompts | Repeated tasks |
| **CLAUDE.md** | Yes (always) | Project rules | Global guidelines |
| **Subagents** | No (delegated) | Separate context | Complex tasks |

---

## Resources

- [Claude Code Agents Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Skills Documentation](https://docs.anthropic.com/en/docs/claude-code)

---

## Checklist

Answer these questions as if in an interview:

1. **What's the difference between subagents and skills?**
   <details>
   <summary>Hint</summary>
   Subagents: delegated tasks, separate context. Skills: knowledge modules, auto-activate.
   </details>

2. **When would you use a subagent vs a skill?**
   <details>
   <summary>Hint</summary>
   Subagent: complex isolated task. Skill: domain knowledge that applies broadly.
   </details>

3. **How do you restrict what tools a subagent can use?**
   <details>
   <summary>Hint</summary>
   tools field in frontmatter: Read, Grep, Edit, etc.
   </details>

4. **How does Claude decide when to activate a skill?**
   <details>
   <summary>Hint</summary>
   Keywords, file types, context matching skill description
   </details>

---

## Mini Project: Specialized AI Team

### Project Goals

Build a team of specialized subagents and skills by completing:

- [ ] Create Security Auditor subagent with read-only tools for security reviews
- [ ] Create Test Generator subagent that can write test files
- [ ] Create Documentation Writer subagent for API docs and READMEs
- [ ] Create Project Architecture skill documenting your project structure
- [ ] Create Code Style skill defining coding conventions
- [ ] Create Testing Patterns skill explaining test organization
- [ ] Add 2+ optional subagents or skills (refactoring, performance, etc.)
- [ ] Test that agents and skills work together

### Ideas to Try

- Create agents that coordinate with each other on complex tasks
- Build skills that reference external documentation
- Make version-specific skills (e.g., React 18 vs React 17)
- Create a migration assistant subagent for framework upgrades

---

## Advanced

- [ ] Create agents that coordinate with each other
- [ ] Build skills that reference external documentation
- [ ] Implement version-specific skills (React 18 vs 17)
- [ ] Create a skill/agent sharing system for your team
