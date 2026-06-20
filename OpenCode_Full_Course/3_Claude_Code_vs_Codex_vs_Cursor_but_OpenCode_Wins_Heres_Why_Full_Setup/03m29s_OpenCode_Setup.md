# OpenCode Setup: Complete Configuration Guide

This section covers the complete setup of OpenCode — from installation to advanced configuration — including the full project structure, configuration options, MCP setup, and best practices for session management.

## Quick Start Installation

```bash
# Install OpenCode globally
npm install -g @opencode/core

# Initialize
opencode init

# Start using it
opencode
```

For detailed installation options (npm, choco, brew, docker), see the beginner guide.

## The .opencode Project Structure

A well-organized OpenCode project uses the `.opencode/` directory. Here is the complete structure:

```
.opencode/
  ├── agents/
  │   ├── frontend-coach.md
  │   ├── security-auditor.md
  │   └── migration-expert.md
  ├── commands/
  │   ├── test.md
  │   ├── deploy.md
  │   ├── component.md
  │   └── review.md
  ├── skills/
  │   ├── react-testing/
  │   │   └── SKILL.md
  │   ├── tailwind/
  │   │   └── SKILL.md
  │   └── api-design/
  │       └── SKILL.md
  └── rules/
      └── custom-rules.md
```

### Root-Level Files

These files sit at your project root and are automatically detected:

```
project-root/
  ├── .opencode/           # OpenCode configuration directory
  ├── opencode.json        # Main configuration file (or opencode.jsonc)
  ├── opencode.jsonc       # JSON with comments (alternative)
  ├── agents.md            # Project-wide agent instructions
  ├── design.md            # Design rules and conventions
  ├── CONTEXT.md           # Additional context for agents
  └── ... your project files
```

#### agents.md

This file provides persistent instructions to all agents. Example:

```markdown
# Project: MyApp

## Stack
- Next.js 14 (App Router)
- TypeScript strict mode
- Tailwind CSS
- Prisma ORM
- PostgreSQL

## Conventions
- Use named exports for all components
- Follow existing folder structure
- Write tests for every new feature
- Use async/await over promises
```

#### design.md

Design rules and UI/UX conventions:

```markdown
# Design System

## Components
- Use Shadcn UI components as base
- Customize via tailwind.config.ts
- All components must be accessible (WCAG AA)

## Layout
- Mobile-first responsive design
- Max content width: 1280px
- Use CSS Grid for page layouts
```

### agents/ Directory

Custom agent definitions. Each file defines one agent.

**File: `.opencode/agents/frontend-coach.md`**

```markdown
# Frontend Coach

## Description
Senior frontend developer specializing in React, TypeScript, and Tailwind CSS.

## System Prompt
You are a senior frontend developer. Follow accessibility best practices (WCAG AA).
Use the project's design system components. Write clean, maintainable code.

## Tools
- read
- write
- edit
- grep
- glob
- bash

## Model
gpt-4o

## Temperature
0.3
```

### commands/ Directory

Custom command definitions. Each file defines one slash command.

**File: `.opencode/commands/test.md`**

```markdown
# test

## Description
Run the test suite and fix any failures.

## Prompt
Run the test suite. If tests fail, analyze the failures and fix them.
Run tests again to confirm. $ARGUMENTS
```

### skills/ Directory

Reusable domain expertise for agents.

**File: `.opencode/skills/react-testing/SKILL.md`**

```markdown
# React Testing Skill

## Instructions
- Use Vitest as test runner
- Use React Testing Library
- Prefer getByRole over test IDs
- Test user behavior, not implementation
- Mock APIs with MSW
- Aim for 80%+ coverage
```

### rules/ Directory

Additional rule sets:

```markdown
# Custom Rules

## Security
- Never commit API keys
- Always validate user input
- Use parameterized SQL queries

## Performance
- Lazy load images and components
- Use React.memo only when profiling shows benefit
```

## Complete opencode.json Configuration

Here is a comprehensive configuration with all possible options:

```jsonc
{
  "$schema": "https://opencode.ai/schemas/opencode.json",

  // Provider Configuration
  "provider": {
    "name": "openai",
    "model": "gpt-4o",
    "temperature": 0.2,
    "maxTokens": 4096,
    "apiKey": "$OPENAI_API_KEY",
    "baseUrl": "https://api.openai.com/v1",
    "fallback": [
      { "name": "anthropic", "model": "claude-3.5-sonnet" },
      { "name": "ollama", "model": "codellama" }
    ]
  },

  // Multi-Model Setup
  "models": {
    "default": {
      "name": "openai",
      "model": "gpt-4o",
      "temperature": 0.2
    },
    "cheap": {
      "name": "openai",
      "model": "gpt-4o-mini",
      "temperature": 0.3
    },
    "powerful": {
      "name": "anthropic",
      "model": "claude-3.5-sonnet",
      "temperature": 0.2
    },
    "local": {
      "name": "ollama",
      "model": "codellama",
      "baseUrl": "http://localhost:11434"
    },
    "free-cloud": {
      "name": "openrouter",
      "model": "google/gemma-4-4b-it",
      "apiKey": "$OPENROUTER_API_KEY",
      "baseUrl": "https://openrouter.ai/api/v1"
    }
  },

  // Custom Commands
  "customCommands": [
    {
      "name": "test",
      "prompt": "Run tests. Fix failures. $ARGUMENTS",
      "description": "Run tests with optional filter"
    },
    {
      "name": "component",
      "prompt": "Create React component $ARGUMENTS with TS, tests, stories",
      "description": "Generate a component"
    },
    {
      "name": "deploy",
      "prompt": "Deploy to $ARGUMENTS after tests pass",
      "description": "Deploy to environment"
    },
    {
      "name": "review",
      "prompt": "Review all changes in current branch for bugs and security",
      "description": "Code review"
    },
    {
      "name": "refactor",
      "prompt": "Refactor $ARGUMENTS for better code quality",
      "description": "Refactor code"
    },
    {
      "name": "doc",
      "prompt": "Generate documentation for $ARGUMENTS",
      "description": "Document code"
    },
    {
      "name": "fix",
      "prompt": "Fix issue: $ARGUMENTS",
      "description": "Fix a bug"
    }
  ],

  // Custom Agents
  "agents": {
    "frontend-coach": {
      "fromFile": ".opencode/agents/frontend-coach.md"
    },
    "security-auditor": {
      "description": "Security code reviewer",
      "systemPrompt": "You are a security expert. Find vulnerabilities.",
      "tools": ["read", "grep", "glob"],
      "model": "anthropic/claude-3.5-sonnet",
      "mode": "plan",
      "permissions": {
        "write": "deny",
        "edit": "deny",
        "bash": "deny"
      }
    },
    "migration-expert": {
      "fromFile": ".opencode/agents/migration-expert.md",
      "permissions": {
        "skills": {
          "database": "allow"
        }
      }
    }
  },

  // MCP Servers
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
    },
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/data/shared"]
    }
  },

  // Permissions
  "permissions": {
    "allowedCommands": ["npm", "git", "node", "npx"],
    "blockedCommands": ["rm -rf", "sudo", "curl"],
    "allowedPaths": ["src/", "tests/"],
    "blockedPaths": ["node_modules/", ".git/"],
    "autoApprove": false
  },

  // Tool Settings
  "tools": {
    "read": { "enabled": true, "maxSize": 50000 },
    "edit": { "enabled": true, "requireConfirmation": true },
    "write": { "enabled": true, "requireConfirmation": true },
    "bash": { "enabled": true, "timeout": 120000, "requireConfirmation": true },
    "grep": { "enabled": true },
    "glob": { "enabled": true },
    "webfetch": { "enabled": true, "timeout": 30000 },
    "websearch": { "enabled": true }
  },

  // Interface
  "interface": {
    "theme": "dark",
    "language": "en",
    "undoLimit": 100,
    "autoApplyPatches": false,
    "streamResponses": true
  },

  // Rules
  "rules": {
    "alwaysIncludeTests": true,
    "followAgentsMd": true,
    "requireLintPass": true,
    "preferNamedExports": true
  }
}
```

## Switching Models While Keeping the Same Context

One of OpenCode's best features is the ability to switch models mid-session:

```text
> /model cheap
[System] Switched to gpt-4o-mini
> Format this file according to our prettier config

> /model default
[System] Switched to gpt-4o
> Now refactor the authentication logic

> /model powerful
[System] Switched to claude-3.5-sonnet
> Design the architecture for the new payment system
```

**The context is preserved across model switches.** The conversation history and attached files remain. Only the AI model changes.

## Efficient Session Management

### Best Practices

```text
1. Name your sessions meaningfully:
   > /sessions rename auth-refactor

2. Use separate sessions for different features:
   Session "auth-refactor": Authentication changes
   Session "payment-ui": Payment page UI
   Session "db-migration": Database changes

3. Compact long sessions:
   > /compact (every 10-15 messages)

4. Share sessions for collaboration:
   > /share

5. Review before accepting:
   Use Plan mode for review, then Build to implement.
```

### Session Workflow Example

```text
# Start a named session
opencode
> /sessions rename api-rate-limiting

# Plan first (Plan mode)
> [Tab] Plan rate limiting for our Express API

# Review and implement (Build mode)
> [Tab] Implement the plan

# Test
> /test

# Compact if needed
> /compact

# Share for review
> /share

# Done
> /exit
```

## Efficient Ways to Use OpenCode

### Pattern 1: Plan → Build → Test

```text
[Plan] Plan the feature
[Build] Implement it
[Test] Run tests and fix
```

### Pattern 2: Explore → Implement

```text
> @explore How is auth currently implemented?
> @general Add SSO support following the same patterns
```

### Pattern 3: Quick Iteration

```text
> /component UserProfile
Review the generated code.
> /fix Add loading state to UserProfile
> /test Run tests
```

### Pattern 4: Multi-Model Optimization

```text
> /model cheap
Quick formatting and linting.

> /model default
Write the main feature code.

> /model powerful
Review and optimize the complex parts.
```

## Summary

```text
Complete Setup Checklist:
  [ ] Install OpenCode
  [ ] Run opencode init
  [ ] Create .opencode/ directory structure
  [ ] Write agents.md with project rules
  [ ] Configure opencode.json
  [ ] Set up custom agents
  [ ] Define custom commands
  [ ] Configure MCP servers if needed
  [ ] Set up multi-model preferences
  [ ] Start building!
```
