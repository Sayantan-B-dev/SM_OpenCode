# Creating Custom Sub-Agents

OpenCode lets you create **custom sub-agents** — specialized assistants that focus on specific tasks like testing, debugging, code review, or refactoring.

## Why Custom Sub-Agents?

| Built-in Agent | Custom Sub-Agent |
|---------------|-----------------|
| General-purpose | Specialized for one job |
| Has default behavior | Follows YOUR instructions |
| Fixed toolset | Controlled tool access |

## How to Create a Custom Sub-Agent

Use the `opencode create agent` command:

```bash
# In your terminal (not inside OpenCode)
opencode create agent test-runner
```

This creates a configuration in `.agents/agents/`:

```
.agents/
├── skills/
└── agents/
    └── test-runner.json
```

### Example: Test Runner Sub-Agent

```json
{
  "name": "test-runner",
  "description": "Runs tests and reports failures",
  "instructions": "Run tests with `npm test`. Parse the output. If tests fail, identify the failing test names and error messages. Do NOT fix code — only report issues.",
  "tools": ["bash"]
}
```

Now use it inside OpenCode:

```text
> @test-runner Run all tests and show me failures

@test-runner:
  → npm test
  → FAIL tests/auth.test.ts (5 failing)
  → FAIL tests/api.test.ts (2 failing)
  → Summary: 7/42 tests failing
```

## Viewing Sub-Agents During a Session

While a sub-agent is working:

```text
> Ctrl+X then Down Arrow

This shows active sub-agents and their status
```

You can see which sub-agent is running, what it's doing, and its progress.

## Communicating with Running Sub-Agents

While a sub-agent is building or processing, you can:

```text
> @test-runner Can you focus on auth tests only?

@test-runner:
  → Running: npm test -- --testPathPattern=auth
  → 3 tests failing in auth.test.ts
```

This lets you **steer** the sub-agent mid-task without restarting.

## Real Example: Multi-Agent Workflow

```text
> Build a user dashboard with user data

Main agent: "I'll use sub-agents for this."

@explorer:
  → Searches existing components and patterns
  → Reports: "Found user service at lib/user.ts"

@designer:
  → Reads DESIGN.md
  → Suggests: "Card-based layout with data table"

Main agent:
  → Creates the Dashboard component
  → Passes to @tester

@tester:
  → Writes tests for the dashboard
  → Reports: "All tests passing"
```

## Example: Code Review Sub-Agent

Create `reviewer`:

```bash
opencode create agent reviewer
```

```json
{
  "name": "reviewer",
  "description": "Reviews code changes for quality and bugs",
  "instructions": "Review code for: 1) Bugs, 2) TypeScript errors, 3) Missing edge cases, 4) Performance issues, 5) Deviation from AGENTS.md rules. Provide a numbered list of issues. Do NOT fix — only report.",
  "tools": ["read", "bash"]
}
```

Use:

```text
> @reviewer Review the changes in src/components/Button.tsx
```

## Available Tools for Sub-Agents

| Tool | What It Does |
|------|-------------|
| `read` | Read file contents |
| `write` | Write file contents |
| `edit` | Edit existing files |
| `bash` | Run shell commands |
| `glob` | Search files by pattern |
| `grep` | Search file contents |
| `webSearch` | Search the web |

Limit tools based on the agent's job — a reviewer should read, not write.

## Pro Tips

- **One job per sub-agent** — don't make a "super agent" that does everything
- **Restrict tools** — give only what's needed (security + focus)
- **Write clear instructions** — "Read only, never modify" prevents accidents
- **Use in AGENTS.md** — tell the main agent to delegate certain tasks automatically
