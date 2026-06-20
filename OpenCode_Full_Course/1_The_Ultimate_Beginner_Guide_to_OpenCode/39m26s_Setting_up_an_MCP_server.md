# Setting Up an MCP Server

MCP (Model Context Protocol) is an open standard that lets AI agents connect to external tools, databases, and services. This section covers how to set up and use MCP servers with OpenCode.

## What Is MCP?

MCP is like "USB-C for AI" — a universal protocol that lets any AI assistant interact with any MCP-compatible server.

```text
Without MCP:    Agent can only see your code files.
With MCP:       Agent can query databases, call APIs,
                read Notion pages, manage GitHub issues,
                and interact with any connected service.
```

## How MCP Works in OpenCode

```
OpenCode Agent
      │
      ├── Built-in Tools (read, write, bash, grep, ...)
      │
      └── MCP Servers (configured in opencode.json)
              │
              ├── Notion MCP Server  ──►  Notion API
              ├── Database MCP Server ──►  PostgreSQL
              ├── GitHub MCP Server  ──►  GitHub API
              └── Filesystem MCP Server ──►  Remote files
```

## Configuring MCP Servers in opencode.json

MCP servers are configured under the `mcpServers` key.

### Basic Structure

```jsonc
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",        // or "sse"
      "command": "npx",       // command to run
      "args": ["package-name", "--flag"],
      "env": {
        "API_KEY": "$ENV_VARIABLE"
      }
    }
  }
}
```

### Two Types of MCP Servers

| Type | Description | Use Case |
|------|-------------|----------|
| `stdio` | Local process spawned by OpenCode | Databases, local tools |
| `sse` | Remote server via Server-Sent Events | Cloud services, APIs |

## Example 1: Notion MCP Server

Connect OpenCode to your Notion workspace.

### Step 1: Get a Notion API Key

1. Go to https://www.notion.so/my-integrations.
2. Create a new integration.
3. Copy the "Internal Integration Secret" (API key).
4. Share the integration with your Notion databases.

### Step 2: Configure in opencode.json

```jsonc
{
  "mcpServers": {
    "notion": {
      "type": "stdio",
      "command": "npx",
      "args": ["@notionhq/opencode-mcp-server"],
      "env": {
        "NOTION_API_KEY": "$NOTION_API_KEY"
      }
    }
  }
}
```

### Step 3: Set Environment Variable

```bash
# Windows PowerShell
$env:NOTION_API_KEY = "ntn_your_api_key_here"

# macOS/Linux
export NOTION_API_KEY="ntn_your_api_key_here"
```

### Step 4: Use It

```text
> @general Find all tasks in the "Sprint 25" Notion database
> @general Create a new page in the "Bug Reports" database with this error details
> @general Update the status of "Fix login bug" to "In Progress"
```

## Example 2: PostgreSQL MCP Server

Connect OpenCode to your database.

### Step 1: Configure in opencode.json

```jsonc
{
  "mcpServers": {
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-postgres",
        "postgresql://localhost:5432/mydb"
      ],
      "env": {
        "PGPASSWORD": "$PGPASSWORD"
      }
    }
  }
}
```

### Step 2: Use It

```text
> @scout Find all users who have not logged in for 30 days
> @general Show me the schema of the orders table
> @general Run a query to find the top 10 products by revenue
```

## Example 3: GitHub MCP Server

Connect OpenCode to GitHub.

### Step 1: Configure

```jsonc
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "$GITHUB_TOKEN"
      }
    }
  }
}
```

### Step 2: Use It

```text
> @general List all open issues labeled "bug"
> @general Create a PR for the current branch
> @general Review the last 5 commits on the main branch
```

## Example 4: Filesystem MCP Server

Access files outside your project directory.

```jsonc
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-filesystem",
        "/data/shared",
        "/backups"
      ],
      "env": {}
    }
  }
}
```

## Example 5: Connecting Two MCP Servers

You can use multiple MCP servers at the same time:

```jsonc
{
  "mcpServers": {
    "notion": {
      "type": "stdio",
      "command": "npx",
      "args": ["@notionhq/opencode-mcp-server"],
      "env": {
        "NOTION_API_KEY": "$NOTION_API_KEY"
      }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-postgres",
        "postgresql://localhost:5432/mydb"
      ],
      "env": {
        "PGPASSWORD": "$PGPASSWORD"
      }
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "$GITHUB_TOKEN"
      }
    }
  }
}
```

Now you can combine them:

```text
> @general Find all high-priority bugs in Notion, check the database for affected users, and create GitHub issues
```

## Security: Use Environment Variables

**NEVER** put API keys directly in `opencode.json`:

```jsonc
// BAD — Never do this
{
  "mcpServers": {
    "notion": {
      "env": {
        "NOTION_API_KEY": "ntn_secret_abc123"  // Hardcoded!
      }
    }
  }
}
```

```jsonc
// GOOD — Use env variable references
{
  "mcpServers": {
    "notion": {
      "env": {
        "NOTION_API_KEY": "$NOTION_API_KEY"  // Reference env var
      }
    }
  }
}
```

## Warning: Token Usage

> **Important:** MCP servers can significantly increase token usage. Every tool call from an MCP server consumes tokens. Be mindful of this, especially when querying large databases or making many API calls.

### Cost Mitigation Tips

```text
1. Only enable MCP servers you actively need.
2. Use @scout instead of @explore for MCP queries.
3. Be specific in your prompts (not "show everything").
4. Disable MCP servers when not in use.
5. Monitor token usage with /compact when needed.
```

## Finding MCP Servers

| Source | How to Find |
|--------|-------------|
| Official | `@modelcontextprotocol/server-*` on npm |
| Notion | `@notionhq/opencode-mcp-server` |
| Community | Search npm/GitHub for "mcp-server" |
| Custom | Build your own using the MCP SDK |

## Authentication Steps Summary

```text
1. Sign up / log in to the service (Notion, GitHub, etc.).
2. Generate an API key or token.
3. Store the key in an environment variable.
4. Reference the env variable in opencode.json.
5. Start OpenCode and verify the connection.
6. Use @general or @scout with MCP-related prompts.
```

## Quick Reference

```jsonc
{
  "mcpServers": {
    "my-server": {
      "type": "stdio",                    // or "sse"
      "command": "command-to-run",
      "args": ["arg1", "arg2"],
      "env": {
        "API_KEY": "$ENV_VARIABLE"       // Never hardcode keys
      }
    }
  }
}
```
