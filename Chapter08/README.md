# Chapter 08: MCP Servers

[한국어](./README.ko.md) | **English**

## Prerequisites

Before starting this chapter, ensure you:
- [ ] Have completed Chapter 00-07
- [ ] Understand API concepts and HTTP
- [ ] Have basic knowledge of JSON and environment variables

---

## Introduction

MCP (Model Context Protocol) servers extend Claude Code's capabilities by connecting it to external tools, databases, and services. Think of MCP as a plugin system that lets Claude interact with the world beyond your local files.

### Why MCP?

- **Database Access**: Query PostgreSQL, MongoDB, etc.
- **API Integration**: Connect to GitHub, Sentry, Jira
- **Custom Tools**: Build your own integrations
- **Team Sharing**: Share MCP configs across projects

---

## Topics

### 1. Understanding MCP

MCP provides a standardized way for Claude to:
- Discover available tools
- Call external services
- Receive structured responses

**Transport Types**:
| Type | Description | Use Case |
|------|-------------|----------|
| `stdio` | Local process communication | Local tools, scripts |
| `http` | HTTP/HTTPS connections | Cloud services |
| `sse` | Server-sent events (deprecated) | Legacy systems |

### 2. Adding MCP Servers

#### Using CLI Commands

```bash
# Add an HTTP MCP server
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# Add a local (stdio) MCP server
claude mcp add --transport stdio postgres -- npx @anthropic-ai/mcp-postgres

# Add with environment variables
claude mcp add --transport stdio github -- \
  npx @anthropic-ai/mcp-github \
  --env GITHUB_TOKEN=your_token

# List configured servers
claude mcp list

# Remove a server
claude mcp remove sentry
```

### 3. Configuration Files

**Project MCP** (`.mcp.json` - git tracked):
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["@anthropic-ai/mcp-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["@anthropic-ai/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**User MCP** (`~/.claude.json`):
```json
{
  "mcpServers": {
    "personal-api": {
      "type": "http",
      "url": "https://my-api.example.com/mcp"
    }
  }
}
```

### 4. Popular MCP Servers

| Server | Purpose | Installation |
|--------|---------|--------------|
| **PostgreSQL** | Database queries | `npx @anthropic-ai/mcp-postgres` |
| **GitHub** | Issues, PRs, repos | `npx @anthropic-ai/mcp-github` |
| **Sentry** | Error monitoring | HTTP: `mcp.sentry.dev` |
| **Slack** | Messaging | `npx @anthropic-ai/mcp-slack` |
| **Filesystem** | Extended file ops | `npx @anthropic-ai/mcp-filesystem` |

### 5. Using MCP Tools

Once configured, Claude can use MCP tools naturally:

```bash
# With PostgreSQL MCP
> Query the users table to find all premium subscribers

# With GitHub MCP
> List all open issues labeled "bug" in this repository

# With Sentry MCP
> Show me the most recent unresolved errors
```

### 6. Environment Variables

Secure handling of credentials:

```bash
# Set environment variables
export DATABASE_URL="postgresql://user:pass@localhost/db"
export GITHUB_TOKEN="ghp_xxxx"

# Reference in config with ${VAR_NAME}
```

**Best Practices**:
- Never commit credentials to git
- Use `.env` files for local development
- Reference variables in MCP config

### 7. Creating Custom MCP Servers

Basic MCP server structure (Node.js):

```javascript
// my-mcp-server.js
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({
  name: "my-custom-server",
  version: "1.0.0",
});

// Define tools
server.setRechapterHandler("tools/list", async () => ({
  tools: [
    {
      name: "get_weather",
      description: "Get weather for a location",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string" }
        }
      }
    }
  ]
}));

// Handle tool calls
server.setRechapterHandler("tools/call", async (rechapter) => {
  if (rechapter.params.name === "get_weather") {
    const { city } = rechapter.params.arguments;
    // Fetch weather data...
    return { result: `Weather in ${city}: Sunny, 72°F` };
  }
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);
```

### 8. Debugging MCP

```bash
# Check MCP status
claude mcp list

# Test MCP server manually
npx @anthropic-ai/mcp-postgres --help

# View MCP logs
claude --debug

# Verify environment variables
echo $DATABASE_URL
```

---

## Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [MCP Servers Directory](https://github.com/modelcontextprotocol/servers)
- [Building MCP Servers Guide](https://modelcontextprotocol.io/docs/building)

---

## Checklist

Answer these questions as if in an interview:

1. **What is MCP and why would you use it?**
   <details>
   <summary>Hint</summary>
   Protocol for connecting Claude to external tools/services: databases, APIs, custom tools
   </details>

2. **What are the different MCP transport types?**
   <details>
   <summary>Hint</summary>
   stdio (local), http (remote), sse (deprecated)
   </details>

3. **Where should MCP configurations be stored?**
   <details>
   <summary>Hint</summary>
   Project: .mcp.json (git tracked). User: ~/.claude.json (personal)
   </details>

4. **How do you securely handle credentials for MCP servers?**
   <details>
   <summary>Hint</summary>
   Environment variables, ${VAR} references, never commit to git
   </details>

---

## Mini Project: Connected Development Environment

### Project Goals

Build an MCP-powered development environment by completing:

- [ ] Configure database integration (PostgreSQL, MySQL, or MongoDB)
- [ ] Configure issue tracker integration (GitHub Issues, Jira, or Linear)
- [ ] Configure monitoring integration (Sentry, Datadog, or custom logging)
- [ ] Add 1+ optional integration (Slack, Notion, CI/CD, etc.)
- [ ] Create complete `.mcp.json` configuration file
- [ ] Handle credentials securely with environment variables
- [ ] Test each integration works end-to-end

### Ideas to Try

- Build a custom MCP server for your own API
- Create a workflow that connects monitoring to code fixes
- Set up notifications that send alerts to Slack/Discord
- Combine database queries with issue tracking for data investigations

---

## Advanced

- [ ] Build an MCP server that wraps your company's internal APIs
- [ ] Create an MCP aggregator that combines multiple services
- [ ] Implement caching in MCP servers for better performance
- [ ] Build MCP servers for domain-specific tools (CAD, music, etc.)
