# Building the AI Project Planner App

This is the main hands-on project in the tutorial — building an **AI-powered Project Planner** that helps you break down ideas into actionable tasks.

## The Approach: PROMPT.md First

Instead of explaining everything in chat, create a `PROMPT.md` file with your full requirements. The agent reads it and builds accordingly.

### Example PROMPT.md

```markdown
# PROMPT.md — AI Project Planner

Build a Next.js app with these features:

## Requirements
- Landing page with a text input for project ideas
- AI generates a structured project plan (tasks, milestones, timeline)
- Plan is displayed as a kanban board (drag & drop)
- Each task can be expanded for details
- Dark mode by default
- Responsive design (mobile + desktop)

## Tech Stack
- Next.js 14 App Router
- TypeScript
- Tailwind CSS
- shadcn/ui components
- @hello-pangea/dnd for drag & drop

## Pages
1. `/` — Landing page with input
2. `/planner/[id]` — Individual plan view
```

Then reference it:

```text
> Read PROMPT.md and build this project
```

## Plan Mode vs Build Mode

OpenCode has two interaction modes:

| Mode | How to Activate | Use Case |
|------|----------------|----------|
| **Build Mode** (default) | Just type | Agent builds immediately |
| **Plan Mode** | `Shift+Tab` | Agent asks questions first, plans before coding |

### Real Example: Plan Mode

Press `Shift+Tab`, then:

```text
Plan: Build a project planner with AI plan generation

The agent will ask:
  - What AI provider should generate the plans?
  - Should plans be saved to localStorage or a database?
  - What's the preferred state management approach?
  - Any specific UI libraries you prefer?

Once you answer, it produces a plan document, and only then starts coding.
```

This prevents the agent from going down the wrong path.

## Using the shadcn Skill

Before building, install the shadcn skill so the agent knows the component patterns:

```bash
npx skills sh shadcn
```

Now when you say "use a dialog for editing tasks", the agent generates:

```tsx
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog"
```

Following shadcn conventions automatically.

## Real Example: Building a Feature

```text
> Add a drag-and-drop kanban board to the planner page

The agent (with shadcn skill + Tailwind skill) will:
1. Install @hello-pangea/dnd
2. Create a KanbanBoard component
3. Create column components (To Do, In Progress, Done)
4. Add drag-and-drop handlers
5. Style everything consistent with your DESIGN.md
6. Add state management for column transitions
```

## Summary Workflow

1. Write `PROMPT.md` with requirements
2. Install relevant skills (`npx skills sh nextjs shadcn tailwind`)
3. Start OpenCode in the project folder
4. Use `Shift+Tab` for plan mode on complex features
5. Reference `PROMPT.md` → agent builds everything
