# Chapter 22: MCP Integration

**English** | [한국어](./README.ko.md)

## What You Will Learn

- MCP (Model Context Protocol) concepts
- Connecting external services
- Practical MCP server usage

---

## Why do you need this?

**Real-world scenario**: You're debugging an issue and need to check the database. You open a terminal, connect to PostgreSQL, write a query, copy the results, paste into Claude... Then Claude asks for more data. Repeat. This is exhausting!

MCP lets Claude connect directly to external services, so you just ask "show me the last 10 orders" and it happens.

### Simple Analogy: Giving Claude a Phone

Without MCP, Claude can only see what's in front of it (your files). It's like someone who can only see documents on their desk.

With MCP, you give Claude a "phone" to call external services:
- "Call the database and ask for user data"
- "Call GitHub and check the issues"
- "Call Slack and send a message"

MCP servers are like phone numbers in Claude's contact list.

---

## Your First MCP Setup (Step by Step)

Let's set up a simple MCP server. We'll use the filesystem server as it requires no external accounts.

### Step 1: Create the Config File

Create the MCP config file:

```bash
mkdir -p ~/.claude
touch ~/.claude/mcp_servers.json
```

### Step 2: Add a Simple Server

Open `~/.claude/mcp_servers.json` and add:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/tmp"]
    }
  }
}
```

This gives Claude access to files in `/tmp` via MCP.

### Step 3: Restart Claude Code

Exit and restart Claude Code for the MCP server to load.

### Step 4: Test It

```
> Using MCP, list the files in /tmp
```

If it works, you'll see the files! If not, check the troubleshooting section below.

---

## Understanding the JSON Config

The MCP config uses JSON. Here's what each part means:

```json
{
  "servers": {                    // List of all your MCP servers
    "serverName": {               // Name you choose (use in conversations)
      "command": "npx",           // Program to run
      "args": ["-y", "package"],  // Arguments for the command
      "env": {                    // Environment variables (optional)
        "API_KEY": "your-key"
      }
    }
  }
}
```

### Common Config Patterns

**Pattern 1: npx package**
```json
{
  "command": "npx",
  "args": ["-y", "@anthropic-ai/mcp-server-postgres"]
}
```

**Pattern 2: Environment variable**
```json
{
  "env": {
    "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
  }
}
```

**Pattern 3: Multiple servers**
```json
{
  "servers": {
    "server1": { ... },
    "server2": { ... }
  }
}
```

---

## Why Learn MCP?

Claude Code basically only handles files and terminal. With MCP connected:

- **Direct database connection**: Execute SQL queries
- **External API integration**: GitHub, Slack, JIRA, etc.
- **Custom tool additions**: Extend with needed features

---

## What is MCP?

MCP (Model Context Protocol) is a standard protocol for adding new capabilities to Claude.

### Basic Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                        MCP Architecture                          │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Claude Code  │ ◀──▶│  MCP Server  │ ◀──▶│ External     │
│              │stdio│              │     │ Service      │
│              │ or  │ (Bridge)     │     │ DB, API etc  │
│              │HTTP │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

MCP server acts as a translator between Claude and external services.

### Why Does This Matter?

**Eliminate copy-paste:**

Instead of copying database results and pasting to Claude:

```
> show me the 10 most recent users from users table
```

Claude queries the database directly.

---

## Setting Up MCP Servers

### Configuration File Location

```
~/.claude/mcp_servers.json
```

### Basic Structure

```json
{
  "servers": {
    "serverName": {
      "command": "command to run",
      "args": ["arg1", "arg2"],
      "env": {
        "ENV_VAR": "value"
      }
    }
  }
}
```

### Example: PostgreSQL Connection

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

---

## Commonly Used MCP Servers

### 1. Filesystem

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/allowed/path"]
    }
  }
}
```

Access files in the specified path.

### 2. PostgreSQL

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

```
> show me the users table structure
> get the 10 most recent orders
> analyze this user's purchase history
```

### 3. GitHub

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxx"
      }
    }
  }
}
```

```
> show me recent PRs in this repo
> check the contents of issue #42
> add a review comment to this PR
```

### 4. Slack

```json
{
  "servers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "xoxb-xxxx"
      }
    }
  }
}
```

```
> send deployment complete message to #dev channel
> summarize what was posted in #bugs channel today
```

---

## Practice: Database Integration

### 1. MCP Server Setup

```json
// ~/.claude/mcp_servers.json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/ecommerce"
      }
    }
  }
}
```

### 2. Actual Usage

```
> what tables are in the database?
```

Claude queries the schema directly.

```
> calculate this month's revenue from the orders table
```

Writes and executes SQL queries.

```
> analyze the top 10 best-selling products
```

Performs complex analysis directly.

### 3. Safe Usage

**Use read-only account:**

```
DATABASE_URL=postgresql://readonly_user:pass@localhost/mydb
```

**Query restrictions:**

Add rules to CLAUDE.md:

```markdown
# DB Access Rules
- SELECT only
- No DELETE, DROP, TRUNCATE
- Always confirm before bulk UPDATE
```

---

## Practice: GitHub Integration

### 1. Setup

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

### 2. Usage Examples

**Issue management:**

```
> show open issues with bug label
> summarize issue #123
> where should I fix to resolve this issue?
```

**PR review:**

```
> analyze the changes in this PR
> let me know if there are code quality issues
> write review comments
```

**Release notes:**

```
> analyze the last 10 commits and write release notes
```

---

## Combining Multiple MCP Servers

### Integrated Workflow

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/mydb"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxx"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "xoxb-xxxx"
      }
    }
  }
}
```

### Integrated Usage Example

```
> check GitHub issue #42, analyze related DB data,
> and share the results to #dev channel
```

Claude will:
1. Check issue contents from GitHub
2. Query related data from database
3. Send analysis results to Slack

---

## Creating Custom MCP Servers

### Simple MCP Server Structure

```javascript
// my-mcp-server.js
const { Server } = require('@modelcontextprotocol/sdk/server')

const server = new Server({
  name: 'my-custom-server',
  version: '1.0.0'
})

// Define tool
server.tool({
  name: 'my_custom_tool',
  description: 'My custom tool',
  inputSchema: {
    type: 'object',
    properties: {
      query: { type: 'string' }
    }
  },
  handler: async ({ query }) => {
    // Tool logic
    return { result: `Processed: ${query}` }
  }
})

server.listen()
```

### Add to Configuration

```json
{
  "servers": {
    "my-custom": {
      "command": "node",
      "args": ["/path/to/my-mcp-server.js"]
    }
  }
}
```

---

## Security Considerations

### 1. Token Management

```json
// Bad - hardcoded
{
  "env": {
    "GITHUB_TOKEN": "ghp_actual_token_here"
  }
}

// Good - environment variable reference
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

### 2. Minimize Permissions

- Use read-only tokens
- Allow access only to needed repos/channels
- Rotate tokens regularly

### 3. Protect Sensitive Data

Add rules to CLAUDE.md:

```markdown
# Security Rules
- Confirm purpose when querying personal data
- Never output passwords, tokens, or sensitive info
- Confirm before accessing bulk data
```

---

## Try it yourself

### Exercise 1: Set Up Filesystem MCP

Follow the step-by-step guide above to set up the filesystem MCP server. Test it by asking Claude to read a file from the allowed path.

### Exercise 2: Add GitHub MCP (if you have a token)

1. Get a GitHub personal access token
2. Add to your config:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token-here"
      }
    }
  }
}
```

3. Test: `> show me my recent GitHub repositories`

### Exercise 3: Combine Multiple Servers

Add both filesystem and GitHub to your config:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/tmp"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token"
      }
    }
  }
}
```

---

## If it doesn't work?

### Problem: MCP server not loading

**Possible causes:**
1. JSON syntax error in config
2. Wrong file location
3. Missing npm/npx

**Solutions:**
- Validate JSON: `cat ~/.claude/mcp_servers.json | jq .`
- Check location: must be `~/.claude/mcp_servers.json`
- Check npx: `which npx` (should show a path)

### Problem: "Permission denied" errors

**Possible causes:**
1. File/folder access restricted
2. API token missing or invalid
3. Network firewall blocking

**Solutions:**
- Check file permissions
- Verify your API tokens are valid
- Try with a simpler path like `/tmp`

### Problem: MCP server starts but Claude can't use it

**Possible causes:**
1. Server name mismatch
2. Server crashed after starting
3. Tool names different from expected

**Solutions:**
- Check server name in config matches what you're asking for
- Look at Claude Code logs for errors
- Restart Claude Code

### Problem: Slow or timing out

**Possible causes:**
1. Network issues
2. Server overloaded
3. Too much data requested

**Solutions:**
- Check your internet connection
- Start with smaller requests
- Use more specific queries

---

## Common mistakes

1. **Hardcoding secrets in config**
   ```json
   // BAD - token in plain text
   { "env": { "TOKEN": "ghp_abc123..." } }

   // BETTER - use environment variable
   { "env": { "TOKEN": "${GITHUB_TOKEN}" } }
   ```

2. **Wrong JSON format**
   - Remember: no comments, no trailing commas
   - Always validate with `jq`

3. **Too permissive access**
   - Don't give MCP access to your entire home folder
   - Use read-only tokens when possible

4. **Forgetting to restart**
   - After changing `mcp_servers.json`, restart Claude Code
   - Changes don't apply automatically

5. **Not checking logs**
   - When things fail, check Claude Code logs
   - MCP servers often print helpful error messages

---

## Summary

What you learned in this chapter:
- [x] MCP concepts and architecture
- [x] How to set up MCP servers
- [x] Commonly used MCP servers (DB, GitHub, Slack)
- [x] Combining multiple servers
- [x] Security considerations

**Key point**: MCP can infinitely extend Claude's capabilities.

In the next chapter, you'll learn how to build automation pipelines with CI/CD.

Proceed to [Chapter 23: CI/CD Automation](../Chapter23/README.md).
