# Adding MCP Servers (18:50 - 24:10)

## What is MCP?

**MCP (Model Context Protocol)** is a standard that allows OpenCode to connect to external tools and services. MCP servers add new capabilities to the AI agent — like searching documentation, querying databases, interacting with APIs, or accessing internal tools.

### Why Use MCP?

Think of MCP as **extending OpenCode's abilities** beyond file editing:
- Search external documentation (Context7)
- Query error tracking (Sentry)
- Search code snippets (Grep by Vercel)
- Access databases, APIs, or internal services

## Adding MCP Servers

There are three ways to add MCPs to OpenCode.

### Method 1: Via opencode.json Config

Edit your `opencode.json` file:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "my-mcp": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp-package"],
      "enabled": true,
      "environment": {
        "MY_API_KEY": "{env:MY_API_KEY}"
      }
    }
  }
}
```

### Method 2: Via CLI

```bash
# Add an MCP
opencode mcp add <name> --type local --command "npx -y my-mcp"

# List all MCPs
opencode mcp list

# Test an MCP connection
opencode mcp debug <name>

# Remove an MCP
opencode mcp remove <name>

# Authenticate an MCP (for OAuth servers)
opencode mcp auth <name>

# Logout from an MCP
opencode mcp logout <name>
```

### Method 3: Via GUI

In the desktop app, navigate to Settings → MCP Servers → Add Server, and fill in the details interactively.

## MCP Configuration Options

### Local MCP (runs on your machine)

```json
{
  "mcp": {
    "my-local-mcp": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-everything"],
      "cwd": "/path/to/working/dir",
      "environment": {
        "MY_VAR": "value"
      },
      "enabled": true,
      "timeout": 5000
    }
  }
}
```

### Remote MCP (runs on a server)

```json
{
  "mcp": {
    "my-remote-mcp": {
      "type": "remote",
      "url": "https://mcp.example.com/mcp",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer {env:API_KEY}"
      },
      "oauth": {}
    }
  }
}
```

## Real-World MCP Examples

### Context7 — Documentation Search

Context7 allows OpenCode to search through npm package docs, framework docs, and more.

```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

Usage in prompts:
```
"Configure a Cloudflare Worker to cache API responses. use context7"
```

### Sentry — Error Tracking

```json
{
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {}
    }
  }
}
```

### Grep by Vercel — Code Search

```json
{
  "mcp": {
    "gh_grep": {
      "type": "remote",
      "url": "https://mcp.grep.app"
    }
  }
}
```

## Managing MCPs

### Enable/Disable Per Agent

```json
{
  "mcp": {
    "my-mcp": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp"],
      "enabled": true
    }
  },
  "tools": {
    "my-mcp*": true
  },
  "agent": {
    "plan": {
      "tools": {
        "my-mcp*": false
      }
    }
  }
}
```

### Context Considerations

**Warning**: MCP servers add to your context window. Each tool registered by an MCP consumes tokens. Be selective about which MCP servers you enable, especially if using models with smaller context limits.

### TUI Keyboard Shortcuts

- `Ctrl+T` → Open file/tool picker (shows available MCP tools)
- `Tab` → Switch between agents (each can have different MCP access)
- `Ctrl` + model selector → Change active model

### CLI Commands for MCP Management

```bash
# List all configured MCP servers
opencode mcp list

# Authenticate with an OAuth MCP server
opencode mcp auth sentry

# Debug MCP connection issues
opencode mcp debug my-mcp

# View auth status
opencode mcp auth list

# Logout from an MCP
opencode mcp logout my-mcp
```
