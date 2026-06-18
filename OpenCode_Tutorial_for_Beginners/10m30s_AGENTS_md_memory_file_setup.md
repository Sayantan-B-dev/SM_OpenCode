# AGENTS.md — The Memory File

`AGENTS.md` is a special file in your project root that acts as **long-term memory** for OpenCode's agent. Every time the agent starts a session, it reads this file to understand your project's rules, preferences, and conventions.

## Why Use AGENTS.md?

Without it, every session starts from scratch. The agent doesn't know your preferences:

- Code style (tabs vs spaces, semicolons, etc.)
- Architecture patterns
- Testing requirements
- Communication style (concise vs detailed)

With `AGENTS.md`, the agent **always knows the rules**.

## How It Works

1. Place `AGENTS.md` in your project root
2. Write your instructions in markdown
3. OpenCode's agent reads it automatically at the start of every session

## What to Put in AGENTS.md

| Section | What to Include | Example |
|---------|----------------|---------|
| **Core Principles** | Your development philosophy | "Think before acting" |
| **Default Behavior** | How the agent should approach tasks | "Read relevant code before making changes" |
| **Implementation Rules** | Code quality standards | "Smallest correct solution wins" |
| **Code Changes** | Change protocol | "Check edge cases, remove dead code" |
| **Tech Stack** | Your project's stack | "React + TypeScript + Tailwind" |
| **Anti-Patterns** | What to avoid | "Over-engineering, premature abstractions" |

## Real Example: Minimal AGENTS.md

```markdown
# AGENTS.md

## TECH STACK
- Next.js 14 (App Router)
- TypeScript
- Tailwind CSS
- Prisma ORM
- PostgreSQL

## RULES
- Always use `async/await`, never `.then()`
- Use named exports, never default exports
- Write unit tests for all utility functions
- Keep components under 200 lines
- Use `shadcn/ui` components when possible

## ANTI-PATTERNS
- No prop drilling — use context or composition
- No `any` types in TypeScript
- No inline styles — use Tailwind classes
- No server actions for simple form submissions

## COMMUNICATION
- Be concise
- Explain reasoning only when the approach is non-obvious
- Show code changes as diffs
```

## Real Example: Full AGENTS.md

```markdown
# AGENTS.md

## CORE PRINCIPLES
- Think before acting.
- Gather context before proposing solutions.
- Optimize for correctness over speed, speed over complexity.
- Keep outputs concise unless depth is requested.
- Prefer evidence, not assumptions.

## DEFAULT BEHAVIOR
- Read relevant code before making changes.
- Understand existing patterns before introducing new ones.
- Ask questions only when missing information blocks accuracy.

## IMPLEMENTATION RULES
- Smallest correct solution wins.
- Reuse existing abstractions before creating new ones.
- Prefer clarity over cleverness.
- Minimize dependencies.

## REASONING PROCESS
1. Understand the goal
2. Identify constraints
3. Inspect existing implementation
4. Consider alternatives
5. Choose the simplest correct approach
6. Verify edge cases
7. Execute

## QUALITY BAR
- Correct
- Maintainable
- Testable
- Consistent

## ERROR HANDLING
- Fail loudly during development.
- Fail gracefully in production.
- Never silently swallow errors.
```

## How the Agent Uses AGENTS.md

When you prompt:

```text
> Add a new API route for user profiles
```

The agent checks `AGENTS.md` → sees you use Next.js App Router, TypeScript, and Prisma → generates code matching your conventions automatically.

## Tips

- **Be specific** — "Use tabs" is better than "indent properly"
- **Update as your project evolves** — add new dependencies and patterns
- **Keep it concise** — the agent reads it every session, so don't pad with fluff
- **Use negative rules** — state what NOT to do (e.g., "Don't use emojis")
