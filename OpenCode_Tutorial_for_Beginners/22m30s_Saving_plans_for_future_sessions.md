# Saving Plans for Future Sessions

When OpenCode creates a plan (in plan mode), you can **save it for later**. This is useful for complex features you'll build over multiple sessions.

## The `.agents/plans/` Folder

Plans are saved as markdown files in `.agents/plans/`.

### Structure

```
my-project/
└── .agents/
    └── plans/
        ├── auth-system.md
        ├── dashboard-redesign.md
        └── api-integration.md
```

## How to Save a Plan

After the agent creates a plan (in plan mode), tell it:

```text
> Save this plan to .agents/plans/kanban-feature.md
```

The agent writes the plan as a markdown file.

## How to Load a Saved Plan

In a new session:

```text
> Read .agents/plans/kanban-feature.md and implement it
```

Or drag-and-drop the plan file into your OpenCode session (if your terminal supports it).

## The `/agents` Command

The `/agents` command shows everything in your `.agents` folder:

```text
> /agents

.agents/
├── skills/
│   ├── nextjs.md
│   └── tailwind.md
└── plans/
    ├── auth-system.md
    └── kanban-feature.md
```

## Real Example: Multi-Session Workflow

### Session 1: Planning

```text
> Shift+Tab (Plan Mode)
> Plan: Build a full authentication system

Agent creates a detailed plan with:
  - Database schema (Users, Sessions tables)
  - API routes (register, login, logout)
  - Middleware for protected routes
  - UI components (LoginForm, RegisterForm)
  - Testing strategy

> Save this to .agents/plans/auth-system.md
```

### Session 2: Implementation

```bash
# Start a fresh session
opencode
```

```text
> /new
> Read .agents/plans/auth-system.md
> Implement step 1: database schema + Prisma setup

# Agent implements just that step. Next session, implement step 2, etc.
```

## Benefits

- **Break large features into sessions** — don't exhaust the context window
- **Consistent plans** — reuse the same plan across sessions
- **Share plans** — send `.agents/plans/` to teammates
- **Iterate** — update the plan as you learn more

## Pro Tips

- Name plans descriptively: `feat-auth-system.md` not `plan1.md`
- Keep plans concise — just bullet points and architecture decisions
- Reference AGENTS.md from plans: `"Follow the patterns in AGENTS.md"`
- Use `/new` between plan-reading and implementation to keep context clean
