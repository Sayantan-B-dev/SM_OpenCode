# Sub-Agents & Context Window Protection

One of OpenCode's most powerful features is **sub-agents** — specialized mini-agents that handle specific tasks without polluting the main agent's context.

## The Context Window Problem

Every AI model has a **context window** (e.g., 128K tokens for GPT-4o). As you chat, the window fills up:

- Previous messages
- File contents read by the agent
- Code generated

When the window is full, the model starts **forgetting** earlier context. Sub-agents solve this.

## How Sub-Agents Help

| Without Sub-Agents | With Sub-Agents |
|-------------------|-----------------|
| One agent does everything | Main agent orchestrates, sub-agents execute |
| Context fills up fast | Each sub-agent has its own clean context |
| Agent forgets earlier instructions | Main agent stays focused on the big picture |
| Everything mixed in one conversation | Responsibilities are separated |

## Built-in Sub-Agents

OpenCode has a built-in sub-agent called `explore`:

```text
> explore the codebase for API route patterns
```

The explore sub-agent searches code independently and returns a summary — the main agent doesn't see the raw search output, saving context.

## Custom Sub-Agents

You can create specialized sub-agents. In `AGENTS.md`:

```markdown
## SUB-AGENTS

For testing:
→ use a sub-agent to write and run tests
→ keeps test logic out of main context

For code review:
→ use a sub-agent to review diffs
→ keeps review commentary separate
```

## Real Example: Context Protection in Action

```text
Main Agent Context (128K tokens):
├── AGENTS.md instructions           ~0.5K
├── Project structure overview       ~2K
├── Current task: "Add user auth"    ~1K
├── Main conversation history        ~20K
└── Generated auth code              ~10K
                                    ─────
                                    ~33.5K used (plenty of room)

Without sub-agents:
├── (everything above)               ~33.5K
├── Search results (all files)       ~50K
├── Test results (all tests)         ~30K
├── Git diff investigation           ~15K
└── More conversation                ~20K
                                    ─────
                                    ~148K OVERFLOW ❌
```

The main agent forgets what you asked it to do 10 messages ago.

## Configuring Sub-Agent Usage in AGENTS.md

```markdown
# AGENTS.md

## SUB-AGENT CONFIGURATION
- Use `explore` sub-agent for file searching and codebase exploration
- Use a dedicated sub-agent for running tests
- Use a dedicated sub-agent for refactoring tasks
- Main agent handles architecture and decision-making only
```

## Checking Context Usage

```text
> /session

Session: 8 messages | 12,341 tokens used
Sub-agents spawned: 3
  - explore (2,100 tokens)
  - test-runner (4,500 tokens)
  - code-review (1,800 tokens)
```

## Best Practices

- **Use sub-agents for any isolated task** — searching, testing, reviewing
- **Keep the main agent strategic** — let it decide WHAT to do, not HOW to search
- **Monitor `/session`** — if context is growing fast, you need more sub-agents
- **Name your sub-agents clearly** — `debug-helper`, `test-runner`, `code-formatter`
