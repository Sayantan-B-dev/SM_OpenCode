# Agents and Subagents

Agents are the core of OpenCode's architecture. Instead of one monolithic AI, OpenCode uses specialized agents for different tasks. This section covers everything about agents and subagents.

## Primary Agents: Build and Plan

OpenCode has two primary agents that control how the AI interacts with your code.

### Build Agent

| Aspect | Description |
|--------|-------------|
| **Purpose** | Implement changes directly |
| **Tool Access** | Full — read, write, edit, bash, grep, etc. |
| **Scope** | Can modify files and run commands |
| **Best For** | Adding features, fixing bugs, refactoring, writing code |
| **Key Behavior** | Analyzes, implements, and verifies |

**Example usage:**

```text
[Build mode]
> Add input validation to the registration form
> Create a REST API endpoint for user profiles
> Refactor the payment service to use async/await
```

### Plan Agent

| Aspect | Description |
|--------|-------------|
| **Purpose** | Analyze and plan without making changes |
| **Tool Access** | Read-only — read, grep, glob, search, LSP |
| **Scope** | Cannot write, edit, or run destructive commands |
| **Best For** | Code review, architecture planning, onboarding |
| **Key Behavior** | Reads files, analyzes patterns, produces reports |

**Example usage:**

```text
[Plan mode]
> Plan the implementation of a caching layer
> Review the authentication flow for security issues
> Map out the database schema from the models
```

### Switching Between Agents

Press `Tab` to toggle between Build and Plan mode in the TUI.

```text
Tab  ──►  Build Mode  ──►  Plan Mode  ──►  Build Mode ...
```

The current mode is shown in the TUI header.

## Subagents

Subagents are specialized helpers you invoke inline with `@` mention syntax. They extend your primary agent's capabilities.

### @general — General Purpose Agent

The all-rounder for any coding task.

```text
> @general Write unit tests for the auth service
> @general Add error handling to the API client
> @general Create a Dockerfile for the project
```

| Aspect | Description |
|--------|-------------|
| **Role** | General-purpose coding assistant |
| **Tool Access** | Full (same as Build mode) |
| **Best For** | Most coding tasks |
| **Strengths** | Broad knowledge, handles ambiguity |

### @explore — Deep Exploration Agent

Thorough codebase exploration and understanding.

```text
> @explore How does authentication work end-to-end?
> @explore Find all places where we handle file uploads
> @explore What is the data flow for the checkout process?
```

| Aspect | Description |
|--------|-------------|
| **Role** | Deep codebase exploration |
| **Tool Access** | Read-only (read, grep, glob, LSP) |
| **Best For** | Onboarding, understanding architecture |
| **Strengths** | Reads many files, follows import chains |
| **Weaknesses** | Slower, higher token usage |

### @scout — Fast Search Agent

Quick, focused searches across the codebase.

```text
> @scout Find all API route definitions
> @scout Where is the database connection configured?
> @scout List all TODO comments in the project
```

| Aspect | Description |
|--------|-------------|
| **Role** | Fast targeted searches |
| **Tool Access** | Read-only (lighter than explore) |
| **Best For** | Quick lookups, pattern matching |
| **Strengths** | Fast, low token usage |
| **Weaknesses** | Shallow, does not follow chains |

### Subagent Comparison

| Aspect | @general | @explore | @scout |
|--------|----------|----------|--------|
| Speed | Medium | Slow | Fast |
| Depth | Medium | Deep | Shallow |
| Token Usage | Medium | High | Low |
| Can Modify Files | Yes | No | No |
| Best For | Implementation | Understanding | Finding |

## Customizing Agents in opencode.jsonc

You can configure agents with specific models, temperatures, and permissions.

### Basic Agent Configuration

```jsonc
{
  "agents": {
    "default": {
      "model": "gpt-4o",
      "temperature": 0.2
    },
    "plan": {
      "model": "claude-3.5-sonnet",
      "temperature": 0.1,
      "description": "Planning mode uses high-quality model"
    },
    "build": {
      "model": "gpt-4o",
      "temperature": 0.3
    }
  }
}
```

### Creating a Custom Agent

You can define entirely new agents with specific roles:

```jsonc
{
  "agents": {
    "documenter": {
      "description": "Specializes in writing documentation",
      "systemPrompt": "You are a documentation expert. Write clear, concise, and comprehensive documentation. Use JSDoc format for code comments.",
      "tools": ["read", "write", "edit", "grep", "glob"],
      "model": "gpt-4o",
      "temperature": 0.3
    },
    "security-auditor": {
      "description": "Security-focused code reviewer",
      "systemPrompt": "You are a security expert. Analyze code for vulnerabilities, insecure patterns, and compliance issues. Be thorough and specific.",
      "tools": ["read", "grep", "glob", "lsp"],
      "model": "anthropic/claude-3.5-sonnet",
      "temperature": 0.1,
      "mode": "plan"
    },
    "frontend-coach": {
      "description": "Frontend development expert",
      "systemPrompt": "You are a senior frontend developer specializing in React, TypeScript, and Tailwind CSS. Follow accessibility best practices and modern patterns.",
      "tools": ["read", "write", "edit", "bash", "grep", "glob"],
      "model": "gpt-4o",
      "temperature": 0.3,
      "permissions": {
        "skills": {
          "react": "allow",
          "tailwind": "allow"
        }
      }
    }
  }
}
```

### Disabling Subagents

You can disable specific subagents if needed:

```jsonc
{
  "agents": {
    "scout": {
      "disabled": true
    }
  }
}
```

### Agent Permissions

Control what each agent can do:

```jsonc
{
  "agents": {
    "security-auditor": {
      "description": "Read-only security review agent",
      "tools": ["read", "grep", "glob"],
      "permissions": {
        "write": "deny",
        "edit": "deny",
        "bash": "deny"
      }
    }
  }
}
```

## Using Subagents Together

The most powerful workflow combines multiple subagents:

```text
Step 1: Explore to understand
> @explore How are API routes currently structured?

Step 2: General to implement
> @general Add a new /api/health endpoint following the same patterns

Step 3: Scout to verify
> @scout Find all the new files created
```

## Agent Mode, Model, and Temperature Settings

Each agent can have its own:

| Setting | Description | Example |
|---------|-------------|---------|
| `model` | Which AI model to use | `gpt-4o`, `claude-3.5-sonnet` |
| `temperature` | Creativity level (0-1) | `0.1` (precise), `0.8` (creative) |
| `mode` | Plan or Build | `plan` (read-only), `build` (full) |
| `tools` | Allowed tools | `["read", "write", "edit"]` |
| `description` | What the agent does | Shown in help |
| `systemPrompt` | Custom instructions | Guides agent behavior |

## Summary

```
Primary Agents:
  ├── Build  ─── Writes code, modifies files, runs commands
  └── Plan  ─── Reads code, analyzes, plans without changes

Subagents (inline with @):
  ├── @general  ─── General purpose coding assistant
  ├── @explore  ─── Deep codebase exploration
  └── @scout  ─── Fast targeted searches

Custom Agents (define in opencode.jsonc):
  └── Your own agents with custom prompts, models, tools
```
