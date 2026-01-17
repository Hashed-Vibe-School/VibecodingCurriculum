# Chapter 25: MCP Integration

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions while studying, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## Connection to Previous Chapter

Remember Agents and Skills from Chapter 24?

- **Agents**: Give Claude specific roles and personalities
- **Skills**: Execute automated workflows with keywords

Now we go one step further:

| Concept | Analogy | Characteristic |
|---------|---------|----------------|
| Agents | Role-playing | Changes Claude's personality and behavior |
| Skills | Automated recipes | Auto-executes predefined tasks |
| **MCP** | **External phone connection** | **Claude directly talks to external services** |

Chapter 24's Agents and Skills were things happening **inside Claude**, but MCP **connects Claude to the outside world**. An Agent could play the role of a DB expert, but couldn't actually access the DB - with MCP connected, it can really query the database!

---

## What You Will Learn in This Chapter

- MCP (Model Context Protocol) concepts and how it works
- MCP server setup and connection methods
- Connecting external services (DB, GitHub, Slack, etc.)
- Practical MCP server usage
- Security and permission management
- Creating custom MCP servers

---

## Why is MCP Important?

### Real-World Scenario: Copy-Paste Hell

**Working without MCP:**

```
Me: "There's a problem with recent orders. Please check it."

Claude: "Sure, please query the order information from the database."

Me: (Open terminal, connect to PostgreSQL, write query...)
    SELECT * FROM orders WHERE status = 'error' LIMIT 10;
    (Copy results and paste into Claude)

Claude: "Orders from user_id 123 are problematic. Can I also see that user's information?"

Me: (Go back to terminal...)
    SELECT * FROM users WHERE id = 123;
    (Copy and paste again)

Claude: "It would be good to check payment information too."

Me: (Back to terminal...)
    SELECT * FROM payments WHERE order_id IN (...)
    (Copy and paste again)

... repeat this process ...
```

**With MCP connected:**

```
Me: "There's a problem with recent orders. Please check it."

Claude: [Directly queries DB]
       "I checked the orders table. There are 3 orders with 'error' status.

        - Order #1234: user_id 123, payment failed
        - Order #1235: user_id 123, out of stock
        - Order #1238: user_id 456, shipping address error

        user_id 123 has 2 issues - the most. Checking that user's info,
        this user's credit card is expired. Should I send a card
        renewal notification email?"

Me: "Great! Please do that"
```

**Result**: 5-minute task reduced to 30 seconds!

### Problems MCP Actually Solves

| Problem Situation | Without MCP | With MCP |
|-------------------|-------------|----------|
| DB query | Terminal -> Query -> Copy -> Paste | "Show me recent orders from DB" |
| Check GitHub issue | Open browser -> Find -> Copy -> Paste | "Check issue #42 contents" |
| Send Slack message | Open app -> Find channel -> Type directly | "Let #dev channel know deployment is complete" |
| API call | Write curl command -> Copy result -> Paste | "Check Seoul weather with weather API" |
| Document search | Open multiple files -> Search -> Copy -> Paste | "Find API spec in project docs" |

---

## Why Do You Need This?

**Real-world scenario**: You're debugging an issue and need to check the database. Open terminal, connect to PostgreSQL, write query, copy results, paste to Claude... Then Claude asks for more data. Repeat. This is exhausting!

MCP lets Claude connect directly to external services, so you just ask "show me the last 10 orders" and it happens.

### Simple Analogy: Giving Claude a Phone

Without MCP, Claude can only see what's in front of it (files). Like someone who can only see documents on their desk.

With MCP, you give Claude a "phone" to call external services:
- "Call the database and ask for user data"
- "Call GitHub and check the issues"
- "Call Slack and send a message"

MCP servers are like phone numbers in Claude's contact list.

### Another Analogy: Giving Your Assistant Access

Imagine you're a CEO and Claude is your capable assistant:

**Assistant without access (no MCP):**
> "Boss, I need to check customer data. Please contact the DB team, get the data, and pass it to me so I can analyze it."

**Assistant with access (with MCP):**
> "Boss, I checked the customer data. New signups increased 23% compared to last month, especially among women in their 20s. Should I share this analysis to the marketing team via Slack?"

MCP is like issuing an **employee badge** that gives your assistant access to company systems!

---

## Beginner Tip

> **What is an "MCP server"?**
>
> The word "server" might sound difficult, but an MCP server is just a **translator program**.
>
> - Claude speaks Korean (Claude language)
> - PostgreSQL speaks SQL (database language)
> - GitHub speaks REST API (web language)
>
> MCP server **translates** between them:
> ```
> Claude: "Show me user info"
>    ↓ MCP server translates
> PostgreSQL: "SELECT * FROM users"
>    ↓ MCP server translates
> Claude: "There are 100 users, of which 80 are active"
> ```
>
> You just hire the translator (MCP server). You don't need to know how translation works!

---

## How MCP Works

### Basic Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        MCP Architecture Detail                       │
└─────────────────────────────────────────────────────────────────────┘

                    Your Request
                         │
                         ▼
┌──────────────────────────────────────┐
│           Claude Code                 │
│  ┌────────────────────────────────┐  │
│  │     MCP Client (Built-in)      │  │
│  │  - Server connection management │  │
│  │  - Request/response handling    │  │
│  │  - Tool call coordination       │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
                         │
            stdio or HTTP protocol
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ MCP Server  │  │ MCP Server  │  │ MCP Server  │
│  (postgres) │  │  (github)   │  │  (slack)    │
│             │  │             │  │             │
│ Tools:      │  │ Tools:      │  │ Tools:      │
│ - query     │  │ - get_issue │  │ - send_msg  │
│ - schema    │  │ - create_pr │  │ - read_ch   │
│ - tables    │  │ - review    │  │ - list_ch   │
└─────────────┘  └─────────────┘  └─────────────┘
         │               │               │
         ▼               ▼               ▼
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ PostgreSQL  │  │ GitHub API  │  │ Slack API   │
│  Database   │  │             │  │             │
└─────────────┘  └─────────────┘  └─────────────┘
```

### MCP Communication Flow Detail

```
┌────────────────────────────────────────────────────────────────────┐
│                    MCP Request-Response Flow                        │
└────────────────────────────────────────────────────────────────────┘

1. User Request
   ┌─────────────────────────────────────────────────┐
   │ "Find and analyze orders with errors"            │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
2. Claude Analysis and Tool Selection
   ┌─────────────────────────────────────────────────┐
   │ Claude: "To handle this request, I need to      │
   │         use the postgres MCP query tool"        │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
3. MCP Server Call
   ┌─────────────────────────────────────────────────┐
   │ Claude → postgres MCP:                          │
   │ {                                               │
   │   "method": "tools/call",                       │
   │   "params": {                                   │
   │     "name": "query",                            │
   │     "arguments": {                              │
   │       "sql": "SELECT * FROM orders              │
   │               WHERE status = 'error'"           │
   │     }                                           │
   │   }                                             │
   │ }                                               │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
4. MCP Server Calls External Service
   ┌─────────────────────────────────────────────────┐
   │ postgres MCP → PostgreSQL DB:                   │
   │ SELECT * FROM orders WHERE status = 'error'     │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
5. Results Returned
   ┌─────────────────────────────────────────────────┐
   │ PostgreSQL → postgres MCP → Claude:             │
   │ [                                               │
   │   {id: 1234, user_id: 123, status: "error"},   │
   │   {id: 1235, user_id: 123, status: "error"},   │
   │   ...                                           │
   │ ]                                               │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
6. Response to User
   ┌─────────────────────────────────────────────────┐
   │ Claude: "There are 3 orders with error status.  │
   │ Analysis results..."                            │
   └─────────────────────────────────────────────────┘
```

---

## Your First MCP Setup (Step by Step)

Let's set up a simple MCP server. We'll use the filesystem server as it requires no external accounts.

### Step 1: Create the Config File

Create the MCP config file:

```bash
# macOS/Linux
mkdir -p ~/.claude
touch ~/.claude/mcp_servers.json

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
New-Item -ItemType File -Force -Path "$env:USERPROFILE\.claude\mcp_servers.json"
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

> **Windows Users**: Use `C:/Temp` or your preferred folder path instead of `/tmp`.

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
  "servers": {                    // List of all MCP servers
    "serverName": {               // Name you choose (used in conversations)
      "command": "npx",           // Program to run
      "args": ["-y", "package"],  // Arguments for the command
      "env": {                    // Environment variables (optional)
        "API_KEY": "your-key"
      }
    }
  }
}
```

### JSON Config Structure Detail

```json
{
  "servers": {
    "my-database": {
      // ========== Required Settings ==========

      "command": "npx",
      // Program to execute
      // - "npx": Run npm package (most common)
      // - "node": Run Node.js script directly
      // - "python": Run Python script
      // - path: "/usr/local/bin/my-server"

      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      // Arguments to pass to the program
      // - "-y": Auto-approve installation prompt in npx
      // - Package name or script path
      // - Additional options

      // ========== Optional Settings ==========

      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db",
        "LOG_LEVEL": "info"
      },
      // Environment variable settings
      // - Sensitive info (API keys, passwords)
      // - Config values (log level, timeout, etc.)

      "cwd": "/path/to/working/directory",
      // Working directory (optional)
      // - Base path where server runs

      "timeout": 30000
      // Timeout (milliseconds, optional)
      // - Server response wait time
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

**Pattern 2: Environment variables**
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

**Pattern 4: Local script execution**
```json
{
  "command": "node",
  "args": ["/home/user/my-mcp-servers/custom-server.js"],
  "cwd": "/home/user/my-mcp-servers"
}
```

**Pattern 5: Python MCP server**
```json
{
  "command": "python",
  "args": ["-m", "my_mcp_server"],
  "env": {
    "PYTHONPATH": "/path/to/modules"
  }
}
```

---

## Pro Tip

> **Managing Secrets with Environment Variables**
>
> Don't write API keys or passwords directly in config files!
>
> ```json
> // BAD - secret exposed
> {
>   "env": {
>     "GITHUB_TOKEN": "ghp_abc123xyz789..."
>   }
> }
>
> // GOOD - reference environment variable
> {
>   "env": {
>     "GITHUB_TOKEN": "${GITHUB_TOKEN}"
>   }
> }
> ```
>
> Then set environment variables in your shell config:
> ```bash
> # ~/.bashrc or ~/.zshrc
> export GITHUB_TOKEN="ghp_abc123xyz789..."
> ```
>
> This way:
> 1. Secrets aren't exposed if config file is committed to Git
> 2. Each team member can use different tokens
> 3. Easy to rotate tokens

---

## Why Learn MCP?

Claude Code basically only handles files and terminal. With MCP connected:

- **Direct database connection**: Execute SQL queries
- **External API integration**: GitHub, Slack, JIRA, etc.
- **Custom tool additions**: Extend with needed features

### Real-World MCP Usage

| Situation | Without MCP | With MCP |
|-----------|-------------|----------|
| Bug investigation | Manually copy/paste DB results | "Check DB records related to this error log" |
| Code review | Manually copy GitHub PR contents | "Review PR #123 and add comments" |
| Deployment notification | Open Slack and write message | "Notify #dev channel deployment is complete" |
| API testing | Copy curl results and request analysis | "Analyze this endpoint response" |
| Documentation update | Gather info from multiple services | "Check changes and update README" |

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
> Show me the 10 most recent signups from users table
```

Claude queries the database directly.

---

## Complete Guide to Common MCP Servers

### 1. Filesystem

**Purpose**: Access files in specific folders

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

**Usage Examples:**

```
> Show me all JSON files in /allowed/path
> Read the config.json file contents
> Create a new settings file
```

**Use Case Scenarios:**
- Managing config files outside the project
- Analyzing log files
- Accessing documentation files

### 2. PostgreSQL

**Purpose**: Directly query PostgreSQL database

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:password@localhost:5432/mydb"
      }
    }
  }
}
```

**Usage Examples:**

```
> What tables are in the database?
> Show me the users table structure
> Query the 10 most recent orders
> Analyze this user's purchase history
> Calculate this month's sales statistics
```

**MCP Provided Tools:**

| Tool Name | Description | Example |
|-----------|-------------|---------|
| `list_tables` | List tables | View all tables |
| `describe_table` | Check table structure | Columns, types, keys info |
| `query` | Execute SQL query | Run SELECT statements |

### 3. MySQL

**Purpose**: Directly query MySQL database

```json
{
  "servers": {
    "mysql": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-mysql"],
      "env": {
        "MYSQL_HOST": "localhost",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "root",
        "MYSQL_PASSWORD": "password",
        "MYSQL_DATABASE": "mydb"
      }
    }
  }
}
```

**Usage Examples:**

```
> Analyze the database schema
> Find products with low stock in the products table
> Tell me the number of new signups last week
```

### 4. GitHub

**Purpose**: Manage GitHub issues, PRs, repos

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Usage Examples:**

```
> Show me recent PRs in this repo
> Check issue #42 contents
> Show open issues with bug label
> Add a review comment to this PR
> Write release notes from the last 10 commits
```

**MCP Provided Tools:**

| Tool Name | Description | Example |
|-----------|-------------|---------|
| `list_issues` | List issues | View open issues |
| `get_issue` | Get issue details | Issue content, comments |
| `create_issue` | Create issue | New bug report |
| `list_prs` | List PRs | PRs awaiting review |
| `get_pr` | Get PR details | Check changes |
| `create_pr_review` | Write PR review | Add comments |

### 5. Slack

**Purpose**: Read/send Slack messages

```json
{
  "servers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "${SLACK_BOT_TOKEN}"
      }
    }
  }
}
```

**Getting Slack Token:**
1. Go to https://api.slack.com/apps
2. Click "Create New App"
3. Add required scopes in "OAuth & Permissions"
4. Install app to workspace
5. Copy "Bot User OAuth Token"

**Required Scopes:**
- `channels:read` - Read channel list
- `channels:history` - Read channel messages
- `chat:write` - Send messages

**Usage Examples:**

```
> Send "Deployment complete" message to #dev channel
> Summarize what was posted in #bugs channel today
> Show me the 10 most recent messages in #general channel
```

### 6. Notion

**Purpose**: Manage Notion pages/databases

```json
{
  "servers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-notion"],
      "env": {
        "NOTION_API_KEY": "${NOTION_API_KEY}"
      }
    }
  }
}
```

**Usage Examples:**

```
> Read the project documentation page contents
> Add a new item to the tasks database
> Organize completed tasks from this week
```

### 7. Linear

**Purpose**: Linear issue tracking

```json
{
  "servers": {
    "linear": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-linear"],
      "env": {
        "LINEAR_API_KEY": "${LINEAR_API_KEY}"
      }
    }
  }
}
```

**Usage Examples:**

```
> Show me my assigned issues
> Summarize this sprint's progress
> Create a new bug issue
```

---

## Warning

> **Security Considerations When Connecting MCP Servers**
>
> MCP is powerful, but misconfiguration can create security risks:
>
> 1. **Use read-only permissions**: Use DB accounts that only allow SELECT when possible
> 2. **Principle of least privilege**: GitHub tokens should only access necessary repos
> 3. **Be careful with sensitive data**: Be cautious when accessing tables with personal information
> 4. **Don't directly connect to production DB**: Recommend using dev/staging DB
>
> ```markdown
> # Add security rules to CLAUDE.md
>
> ## MCP Usage Rules
> - No DELETE, DROP, TRUNCATE queries
> - Confirm purpose when accessing personal info tables (users.email, users.phone)
> - Confirm before querying 1000+ records
> - No write operations in production environment
> ```

---

## Setting Up MCP Servers

### Config File Location

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
> What tables are in the database?
```

Claude queries the schema directly.

```
> Calculate this month's revenue from the orders table
```

Writes and executes SQL queries.

```
> Analyze the top 10 best-selling products
```

Performs complex analysis directly.

### 3. Safe Usage

**Use read-only account:**

```sql
-- Create read-only user in PostgreSQL
CREATE USER readonly_user WITH PASSWORD 'secure_password';
GRANT CONNECT ON DATABASE mydb TO readonly_user;
GRANT USAGE ON SCHEMA public TO readonly_user;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly_user;

-- Auto-apply to future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO readonly_user;
```

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

### 2. Getting GitHub Token

1. GitHub -> Settings -> Developer settings -> Personal access tokens
2. Click "Generate new token (classic)"
3. Select required permissions:
   - `repo` - Private repo access
   - `public_repo` - Public repos only
   - `read:org` - Read org info
4. Generate token and store safely

### 3. Usage Examples

**Issue management:**

```
> Show open issues with bug label
> Summarize issue #123
> Where should I fix to resolve this issue?
```

**PR review:**

```
> Analyze this PR's changes
> Let me know if there are code quality issues
> Write review comments
```

**Release notes:**

```
> Analyze the last 10 commits and write release notes
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
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "${SLACK_BOT_TOKEN}"
      }
    }
  }
}
```

### Integrated Usage Example

```
> Check GitHub issue #42, analyze related DB data,
> and share the results to #dev channel
```

Claude will:
1. Check issue contents from GitHub
2. Query related data from database
3. Send analysis results to Slack

---

## Real Conversation Examples with Claude

### Example 1: Database Analysis

```
Me: "This month's sales seem off. Analyze what's wrong."

Claude: [Uses postgres MCP to query orders table]

       I analyzed this month's sales data.

       Sales Status:
       - This month total: $45,230
       - Last month total: $52,180
       - Decrease: -13.3%

       Cause Analysis:
       1. Order count is similar (1,234 -> 1,189, -3.6%)
       2. Average order value decreased ($42.30 -> $38.00, -10.2%)

       The biggest change is "Premium" category sales
       decreased 35%.

       Want to see detailed analysis by premium category product?

Me: "Yes, and also find out why premium sales dropped"

Claude: [Runs additional queries]

       Premium category detailed analysis:

       Decreased Products:
       1. Premium Laptop A: -45% (out of stock for 5 days)
       2. Premium Tablet B: -28% (coincides with competitor launch)
       3. Premium Earbuds C: -15% (decreased after price increase)

       Recommended Actions:
       1. Laptop A inventory management needed
       2. Tablet B pricing/promotion review
       3. Earbuds C pricing strategy review

       Should I share this analysis to marketing team Slack channel?
```

### Example 2: Bug Investigation Workflow

```
Me: "Got CS reports that payment failures are high in production. Investigate."

Claude: [Uses postgres MCP to query payments table]

       I analyzed recent 24-hour payment data.

       Anomaly Found:
       - Total payment attempts: 523
       - Success: 412 (78.8%)
       - Failed: 111 (21.2%)

       Significantly increased from normal 5% failure rate.

       Failure Cause Analysis:
       - "timeout" error: 89 (80%)
       - "invalid_card": 15 (14%)
       - "insufficient_funds": 7 (6%)

       Mostly timeout errors.
       Looks like payment gateway API response time issue.

       Should I check if there are related issues on GitHub?

Me: "Yes, and check if this happened before"

Claude: [Uses github MCP to search issues]

       Found related GitHub issues:

       Related Issues:
       - #234 "Payment timeout increase" (2 weeks ago, closed)
         -> Fix: Increased timeout from 30s to 60s

       - #156 "Payment retry logic improvement" (1 month ago, closed)
         -> Added auto-retry on API timeout

       Similar issues happened before.
       Solution from #234 might be needed again.

       [Uses postgres MCP for additional query]

       Looking at timeout occurrence times,
       they're concentrated from today 2PM.
       High probability it's payment gateway side issue.

       Should I notify infra team via Slack?

Me: "Yes, share the situation to #infra channel"

Claude: [Uses slack MCP to send message]

       Shared the following to #infra channel:

       "Payment Timeout Increase Alert

       Status:
       - Last 24h payment failure rate 21.2% (normally 5%)
       - Main cause: timeout error (80%)
       - Start time: Today 14:00

       Possible cause:
       - Payment gateway API response delay

       Related issue: #234 (previous similar case)

       Please check."
```

### Example 3: PR Review Automation

```
Me: "Review PR #89"

Claude: [Uses github MCP to get PR contents]

       PR #89 "User auth logic refactoring" review.

       Change Summary:
       - Changed files: 5
       - Added: +234 lines
       - Deleted: -156 lines
       - Author: @kim-dev

       Positives:
       1. Auth logic well separated into AuthService
       2. Error handling is more detailed
       3. Test coverage increased (+15%)

       Improvement Suggestions:
       1. auth.service.js:45 - Token expiry time is hardcoded
          -> Recommend moving to environment variable

       2. auth.service.js:78 - SQL injection risk
          -> Need to use parameterized query

       3. auth.test.js:23 - Missing edge case test
          -> Recommend adding expired token test

       Should I write PR comments with this content?

Me: "Look more closely at the security issue part"

Claude: [Detailed PR diff analysis]

       Security-related detailed analysis:

       Severity: High
       Location: auth.service.js:78-82

       Current code:
       ```javascript
       const query = `SELECT * FROM users
                      WHERE email = '${email}'`;
       ```

       Problem:
       - User input (email) directly inserted into query
       - SQL injection attack possible

       Attack example:
       email = "'; DROP TABLE users; --"

       Recommended fix:
       ```javascript
       const query = 'SELECT * FROM users WHERE email = $1';
       const result = await db.query(query, [email]);
       ```

       Should I add a review comment about this security issue
       directly to the PR?

Me: "Yes, add it"

Claude: [Uses github MCP to write PR comment]

       Added review comment to PR #89:

       Added Comment:

       [auth.service.js:78]
       "Security Issue: SQL Injection Risk

       Current code has user input directly
       inserted into query. Please use parameterized query.

       Fix example:
       ```javascript
       const query = 'SELECT * FROM users WHERE email = $1';
       const result = await db.query(query, [email]);
       ```"

       Set review status to "Changes Requested".
```

### Example 4: Error Situation

```
Me: "Query sales data from DB"

Claude: [Attempts postgres MCP call]

       MCP server connection error occurred.

       Error message: "Connection refused: localhost:5432"

       Possible causes:
       1. PostgreSQL server not running
       2. DATABASE_URL setting is wrong
       3. Firewall blocking connection

       Things to check:
       ```bash
       # Check PostgreSQL running status
       pg_isready -h localhost -p 5432

       # Check service status (macOS)
       brew services list | grep postgresql

       # Start service
       brew services start postgresql
       ```

       Please try again after starting PostgreSQL server.

Me: "Started the server"

Claude: [Retries postgres MCP]

       Connection successful! Querying sales data.

       ...
```

---

## Creating Custom MCP Servers

### Why Do You Need Custom MCP Servers?

When default MCP servers don't cover your needs:
- Internal company API integration
- Special data source access
- Custom business logic processing

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

### Practical Custom MCP Example: Internal API Integration

```javascript
// internal-api-mcp.js
const { Server } = require('@modelcontextprotocol/sdk/server');
const axios = require('axios');

const server = new Server({
  name: 'internal-api',
  version: '1.0.0'
});

// Query employee info by ID
server.tool({
  name: 'get_employee',
  description: 'Query employee info by employee ID',
  inputSchema: {
    type: 'object',
    properties: {
      employeeId: {
        type: 'string',
        description: 'Employee ID (e.g., EMP001)'
      }
    },
    required: ['employeeId']
  },
  handler: async ({ employeeId }) => {
    try {
      const response = await axios.get(
        `${process.env.INTERNAL_API_URL}/employees/${employeeId}`,
        {
          headers: {
            'Authorization': `Bearer ${process.env.INTERNAL_API_TOKEN}`
          }
        }
      );
      return {
        success: true,
        employee: response.data
      };
    } catch (error) {
      return {
        success: false,
        error: error.message
      };
    }
  }
});

// Check meeting room availability
server.tool({
  name: 'get_room_availability',
  description: 'Check meeting room available times',
  inputSchema: {
    type: 'object',
    properties: {
      roomId: { type: 'string' },
      date: { type: 'string', format: 'date' }
    },
    required: ['roomId', 'date']
  },
  handler: async ({ roomId, date }) => {
    const response = await axios.get(
      `${process.env.INTERNAL_API_URL}/rooms/${roomId}/availability`,
      { params: { date } }
    );
    return response.data;
  }
});

// Request vacation
server.tool({
  name: 'request_vacation',
  description: 'Request vacation (self only)',
  inputSchema: {
    type: 'object',
    properties: {
      startDate: { type: 'string', format: 'date' },
      endDate: { type: 'string', format: 'date' },
      reason: { type: 'string' }
    },
    required: ['startDate', 'endDate']
  },
  handler: async ({ startDate, endDate, reason }) => {
    // In reality, use authenticated user info
    const response = await axios.post(
      `${process.env.INTERNAL_API_URL}/vacation/request`,
      { startDate, endDate, reason }
    );
    return response.data;
  }
});

server.listen();
```

**Configuration:**

```json
{
  "servers": {
    "internal": {
      "command": "node",
      "args": ["/path/to/internal-api-mcp.js"],
      "env": {
        "INTERNAL_API_URL": "https://api.company.internal",
        "INTERNAL_API_TOKEN": "${INTERNAL_API_TOKEN}"
      }
    }
  }
}
```

**Usage Examples:**

```
> Query employee Kim Cheolsu (EMP001) info
> Tell me available times for meeting room A tomorrow
> Request vacation from next Monday to Wednesday
```

---

## Security Considerations

### 1. Token Management

```json
// BAD - hardcoded
{
  "env": {
    "GITHUB_TOKEN": "ghp_actual_token_here"
  }
}

// GOOD - environment variable reference
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

### Security Checklist

| Item | Check |
|------|-------|
| Are API tokens managed via environment variables? | [ ] |
| Is DB account read-only? | [ ] |
| Are only minimum required permissions granted? | [ ] |
| Avoided direct production DB connection? | [ ] |
| Security rules specified in CLAUDE.md? | [ ] |
| Sensitive info table access restrictions set? | [ ] |

---

## Practice Exercises

### Exercise 1: Set Up Filesystem MCP

Follow the step-by-step guide above to set up the filesystem MCP server. Test by asking Claude to read a file from the allowed path.

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

3. Test: `> Show me my recent GitHub repositories`

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

## Troubleshooting

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

### Troubleshooting Flowchart

```
MCP not working
        │
        ▼
┌─────────────────┐
│ Check JSON syntax│ ──error──▶ Fix JSON
│ jq . < config   │
└────────┬────────┘
         │ OK
         ▼
┌─────────────────┐
│ Check file location │ ──wrong──▶ Move to ~/.claude/mcp_servers.json
│                 │
└────────┬────────┘
         │ OK
         ▼
┌─────────────────┐
│ Check npx installed │ ──missing──▶ Install Node.js/npm
│ which npx      │
└────────┬────────┘
         │ exists
         ▼
┌─────────────────┐
│ Did you restart  │
│ Claude Code?    │ ──no──▶ Restart
└────────┬────────┘
         │ yes
         ▼
┌─────────────────┐
│ Check env vars  │ ──missing──▶ Set environment variables
│ echo $TOKEN    │
└────────┬────────┘
         │ set
         ▼
┌─────────────────┐
│ Check network   │ ──problem──▶ Check VPN/firewall
│ ping server    │
└────────┬────────┘
         │ OK
         ▼
    Check detailed logs
```

---

## Common Mistakes

### 1. Hardcoding secrets in config

```json
// BAD - plaintext token
{ "env": { "TOKEN": "ghp_abc123..." } }

// BETTER - use environment variable
{ "env": { "TOKEN": "${GITHUB_TOKEN}" } }
```

### 2. Wrong JSON format

```json
// BAD - using comments
{
  "servers": {
    // This doesn't work
    "github": { ... }
  }
}

// BAD - trailing comma
{
  "servers": {
    "github": { ... },  // <- This comma doesn't work
  }
}

// GOOD
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"]
    }
  }
}
```

- Remember: no comments, no trailing commas
- Always validate with `jq`

### 3. Too permissive access

```json
// BAD - access to entire home folder
{
  "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "~"]
}

// GOOD - specific folder only
{
  "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "~/projects/myapp"]
}
```

- Don't give MCP access to your entire home folder
- Use read-only tokens when possible

### 4. Forgetting to restart

- After changing `mcp_servers.json`, restart Claude Code
- Changes don't apply automatically

### 5. Not checking logs

- When things fail, check Claude Code logs
- MCP servers often print helpful error messages

---

## Mini Quiz

### Quiz 1: Role of MCP
What does MCP do?

<details>
<summary>View Answer</summary>

**Answer**: MCP is a protocol that allows Claude to directly access external services (databases, APIs, etc.).

**Explanation**: MCP (Model Context Protocol) acts as a "translator" between Claude and external services. The MCP server transforms Claude's requests into a format the service understands and returns results back to Claude.
</details>

### Quiz 2: Config File Location
Where should the MCP config file be located?

<details>
<summary>View Answer</summary>

**Answer**: `~/.claude/mcp_servers.json`

**Explanation**: MCP config is stored in the `mcp_servers.json` file inside the `.claude` folder in your home directory. After modifying this file, you must restart Claude Code for changes to take effect.
</details>

### Quiz 3: Security Best Practice
Which is the safe way to manage API tokens in MCP config?

A) Enter token value directly in config file
B) Reference environment variable (`${GITHUB_TOKEN}`)
C) Record token as comment
D) Document token in README

<details>
<summary>View Answer</summary>

**Answer**: B) Reference environment variable (`${GITHUB_TOKEN}`)

**Explanation**:
- A is security risk (exposed if committed to Git)
- B is correct - managing via environment variables keeps sensitive info out of config file
- C won't work as JSON doesn't support comments
- D should never be done - token would be exposed
</details>

### Quiz 4: MCP Server Role
What does the PostgreSQL MCP server do?

<details>
<summary>View Answer</summary>

**Answer**: It transforms Claude's requests into SQL queries, executes them on the PostgreSQL database, and returns results in a format Claude can understand.

**Process**:
1. Claude: "Show me the 10 most recent orders"
2. MCP server: Transforms to `SELECT * FROM orders ORDER BY created_at DESC LIMIT 10`
3. PostgreSQL: Executes query, returns results
4. MCP server: Passes results to Claude
5. Claude: Responds in human-readable format
</details>

### Quiz 5: Troubleshooting
Changed MCP settings but it's not working. What should you do first?

<details>
<summary>View Answer</summary>

**Answer**: Restart Claude Code.

**Explanation**: MCP setting changes don't apply automatically. You must exit and restart Claude Code for new settings to load. If problems persist after restarting, check JSON syntax, file location, environment variables in order.
</details>

---

## Practice Exercises

### Exercise 1: Basic - Filesystem MCP Setup [Difficulty: 1/3]

**Goal**: First MCP server setup and test

**Steps**:
1. Create `~/.claude/mcp_servers.json` file
2. Add Filesystem MCP server config
3. Specify allowed folder path (e.g., `/tmp` or project folder)
4. Restart Claude Code
5. Test with "List files in [specified folder] using MCP"

**Checklist**:
- [ ] Config file is in correct location
- [ ] JSON syntax is correct
- [ ] Claude returns file list

### Exercise 2: Intermediate - GitHub MCP Integration [Difficulty: 2/3]

**Goal**: Connect GitHub MCP and manage issues/PRs

**Steps**:
1. Generate GitHub Personal Access Token
2. Set token as environment variable
3. Add GitHub server to MCP config
4. Test various GitHub operations:
   - List issues
   - View specific issue details
   - Check PR list

**Config Template**:
```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Test Commands**:
```
> Show open issues in this repo
> Check issue #1 contents
> Show the 5 most recent PRs
```

### Exercise 3: Advanced - Multiple MCP Server Integration [Difficulty: 3/3]

**Goal**: Build workflow combining multiple MCP servers

**Scenario**: Database + GitHub integration

**Steps**:
1. Set up both PostgreSQL MCP and GitHub MCP
2. Execute integrated workflow using both servers:
   - Check GitHub issue contents
   - Query related database data
   - Organize analysis results

**Config Template**:
```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Integration Test**:
```
> Check GitHub issue #42 and analyze related DB data together
```

---

## Challenge Tasks

### Challenge 1: Create a Custom MCP Server

Create your own MCP server:

1. Internal company API integration
2. Frequently used external service integration
3. Special data source access

**Hint**: Use `@modelcontextprotocol/sdk` package

### Challenge 2: Production-Level Security Configuration

Create production-level security config:

1. Create read-only DB account
2. Generate minimum permission API tokens
3. Write detailed security rules in CLAUDE.md
4. Separate configs by environment (dev/staging/production)

### Challenge 3: Team MCP Standard Configuration

Create MCP standard config for the whole team:

1. Define common MCP server list
2. Document config templates
3. Write environment variable guide
4. Write troubleshooting guide

---

## Summary

What you learned in this chapter:
- [x] MCP concepts and architecture
- [x] How to set up MCP servers
- [x] Commonly used MCP servers (DB, GitHub, Slack)
- [x] Combining multiple servers
- [x] Creating custom MCP servers
- [x] Security considerations

**Key Point**: MCP can infinitely extend Claude's capabilities. Directly connecting to external services like databases, GitHub, and Slack eliminates copy-paste work and enables efficient workflows.

---

## Glossary

| Term | Description |
|------|-------------|
| **MCP** | Model Context Protocol. Standard protocol for Claude to communicate with external services |
| **MCP Server** | Program that acts as translator between Claude and external services |
| **stdio** | Standard Input/Output. One way MCP servers communicate with Claude |
| **npx** | Tool to run npm packages without installing |
| **Environment Variable** | Variable set in system. Used to store sensitive info like API keys |
| **Token** | Authentication info for API access |
| **Read-only** | Permission that only allows SELECT, no modify/delete |
| **JSON** | JavaScript Object Notation. Config file format |

---

## Next Chapter Preview

In Chapter 26, you'll learn about **CI/CD Automation**:

- GitHub Actions basics
- Integrating Claude Code into CI/CD pipelines
- Setting up automatic code review
- Configuring automatic PR checks
- Deployment automation

You've learned how to connect to external services with MCP - now let's learn how to integrate this into automation pipelines!

---

Proceed to [Chapter 26: CI/CD Automation](../Chapter26-CI-CD-Automation/README.md).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
