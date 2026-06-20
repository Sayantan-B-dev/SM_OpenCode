# Live Demo: OpenCode in Action (13:02 - 16:57)

## Real-World Example: Building a Next.js Portfolio

This section walks through a complete example using OpenCode to build a Next.js + TypeScript portfolio project from scratch.

### Project Setup

```bash
# Create a new Next.js project with TypeScript
npx create-next-app@latest portfolio --typescript --tailwind

# Navigate into the project
cd portfolio

# Initialize OpenCode
opencode
```

### Step 1: Initialize Project Context

In OpenCode TUI, run:
```
/init
```

This command:
- Analyzes your entire project structure
- Creates an `AGENTS.md` file with project context
- Documents coding patterns, dependencies, and architecture
- Makes this context available to **every future session**

**Important**: Commit `AGENTS.md` to git — it's the shared brain for all your OpenCode sessions.

### Step 2: Multi-Agent Session Strategy

Instead of doing everything in one session, split the work:

```
Root Session (you)
├── Agent 1 (Plan mode): Design portfolio architecture & component tree
│   ├── Decides on layout, pages, data flow
│   └── Writes architecture to DESIGN.md
├── Agent 2 (Build): Implement Home page & navigation
├── Agent 3 (Build): Create Project showcase components
├── Agent 4 (Build): Build Contact form with validation
└── Agent 5 (Fast): Write tests for all components
```

### Step 3: Shared Memory System

The key to parallel sessions working together is **shared memory**:

#### AGENTS.md
Stores project-wide context that every agent reads:
```markdown
# Project: Portfolio
- Framework: Next.js 15 with App Router
- Styling: Tailwind CSS v4
- Language: TypeScript
- Components: Server components by default, 'use client' only when needed
- Data: Static markdown files for projects
```

#### DESIGN.md
Created by the planning agent, consumed by coding agents:
```markdown
# Architecture Decisions
- Use `app/` directory for routes
- Components in `@/components/` (with `index.ts` barrel exports)
- `@/lib/` for utility functions
- `@/content/` for markdown project data
```

#### .agent/ Folder
Each agent writes its progress here so others can pick up where it left off.

### Step 4: The Development Flow

1. **Plan first**: Switch to Plan mode (Tab key), describe the portfolio requirements
2. **Review the plan**: Ensure the architecture makes sense
3. **Spawn sub-agents**: Launch multiple build agents in parallel
4. **Monitor progress**: Check back on each agent's progress
5. **Test and build**: Use the `/init` test command or custom test command
6. **Iterate**: Run `/undo` if something needs rework

### Step 5: Installing Custom Skills

OpenCode supports **skills** — reusable instructions loaded on-demand:

```bash
# From the skills marketplace or community
opencode skill install --from <url>

# Or create manually
mkdir -p .opencode/skills/nextjs-portfolio
```

Example skill: `.opencode/skills/nextjs-portfolio/SKILL.md`
```markdown
---
name: nextjs-portfolio
description: Create Next.js portfolio with Tailwind CSS best practices
---

- Use `next/image` for optimized images
- Implement ISR for project pages with `revalidate`
- Use Geist font (default in Next.js 15)
- Add metadata API for SEO on every page
- Use `@/` path aliases for imports
```

### Step 6: Testing and Building

```bash
# In OpenCode session:
"Run the full test suite and report any failures"

# For building:
"Build the production bundle and check for errors"
```

The agent handles everything — running commands, parsing output, fixing issues, and re-running until clean.

### Key Takeaways from the Demo

1. **Multiple agents, one project**: Parallel execution dramatically speeds up development
2. **Shared memory is crucial**: AGENTS.md, DESIGN.md, and .agent/ create a persistent project brain
3. **Plan before building**: Use Plan mode to catch architecture issues early
4. **Custom skills accelerate**: Pre-built instructions for common patterns
5. **Every session contributes**: All agents write their progress to shared context
