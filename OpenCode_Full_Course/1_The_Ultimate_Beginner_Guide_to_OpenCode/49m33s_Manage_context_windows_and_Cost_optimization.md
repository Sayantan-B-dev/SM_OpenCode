# Manage Context Windows and Cost Optimization

Context windows and cost management are essential skills for using OpenCode efficiently. This section explains how they work and how to optimize both.

## Understanding Context Windows

### What Is a Context Window?

The **context window** is the amount of text (tokens) the AI model can "see" at once. It includes:

- Your conversation history.
- Files you have attached.
- System instructions (agents.md, agent prompts).
- Tool outputs (search results, file contents, command results).

### Context Window Sizes by Model

| Model | Context Window | Effective Working Limit |
|-------|---------------|------------------------|
| GPT-4o | 128K tokens | ~100K tokens |
| GPT-4o-mini | 128K tokens | ~100K tokens |
| Claude 3.5 Sonnet | 200K tokens | ~180K tokens |
| Gemini 1.5 Pro | 1M tokens | ~800K tokens |
| Codellama (Ollama) | 16K tokens | ~12K tokens |
| DeepSeek Coder | 128K tokens | ~100K tokens |

### Why Context Matters

```text
More context = Better understanding but higher cost.
Less context = Cheaper and faster but may miss details.

The goal: Use only as much context as needed.
```

## Thinking Like a Tiered Workspace

Imagine your context window as a physical desk:

```text
┌────────────────────────────────────────────────────┐
│                  YOUR DESK                          │
│  (Context Window: 128K tokens)                      │
│                                                     │
│  ├── Priority Zone (current task) ─── 20% space    │
│  ├── Reference Zone (relevant files) ── 30% space  │
│  ├── History Zone (conversation) ──── 30% space    │
│  └── System Zone (instructions) ──── 20% space     │
│                                                     │
│  When desk gets full → /compact to organize         │
└────────────────────────────────────────────────────┘
```

### Tiered Model Strategy

Use different models for different tasks to optimize costs:

```text
┌────────────────────────────────────────────────────┐
│              TIERED MODEL STRATEGY                  │
├────────────┬──────────────────┬────────────────────┤
│   Tier     │     Model        │    Best For        │
├────────────┼──────────────────┼────────────────────┤
│ Tier 1     │ Claude 3.5       │ Complex reasoning, │
│ (Premium)  │ Sonnet / GPT-4o  │ architecture,      │
│            │                  │ security review    │
├────────────┼──────────────────┼────────────────────┤
│ Tier 2     │ GPT-4o           │ Daily coding,      │
│ (Standard) │                  │ refactoring,       │
│            │                  │ writing tests      │
├────────────┼──────────────────┼────────────────────┤
│ Tier 3     │ GPT-4o-mini      │ Simple tasks,      │
│ (Budget)   │                  │ formatting,        │
│            │                  │ quick Q&A          │
├────────────┼──────────────────┼────────────────────┤
│ Tier 4     │ Ollama (local)   │ Private code,      │
│ (Free)     │ models           │ offline work,      │
│            │                  │ simple automation  │
└────────────┴──────────────────┴────────────────────┘
```

## Cost Optimization Strategies

### 1. Use the Right Model for the Task

```text
❌ Using Claude Sonnet for: "Fix this typo"
   Cost: $0.015 per call (overkill)

✅ Using GPT-4o-mini for: "Fix this typo"
   Cost: $0.0003 per call (99% cheaper)

Savings: ~98% per call
```

### 2. Attach Only Relevant Files

```text
❌ Bad: "Review all files in the project"
   Loads whole codebase into context → $$$

✅ Good: "Review the auth flow in src/auth.ts and src/middleware/auth.ts"
   Only loads 2 files → $$
```

Use `@` for precise file attachment:

```text
> @src/auth.ts @src/middleware/auth.ts review the authentication flow
```

### 3. Use /compact Regularly

```text
> /compact
[System] Compacted 24 messages into a summary.
```

When to compact:

```text
- After every 10-15 messages in a session.
- When responses start getting slower.
- When you notice the agent "forgetting" earlier context.
- Before switching to a different task.
```

### 4. Use @scout Instead of @explore

```text
@explore reads many files (high cost, high context usage).
@scout does focused searches (low cost, low context usage).

Use @scout when you know what you are looking for.
Use @explore only when you need deep understanding.
```

### 5. Mention Filenames in Prompts

Instead of vague prompts, be specific:

```text
❌ "Fix the API bug"
   Agent has to search to find the right file → extra token cost

✅ "Fix the bug in src/routes/users.ts:42 where the query parameter is missing"
   Agent goes directly to the right place → minimal token cost
```

### 6. Only Use Needed MCP Servers

Each active MCP server adds tool definitions to the context:

```text
❌ 5 MCP servers enabled: Notion, GitHub, PostgreSQL, Slack, Jira
   All tool definitions consume context = $$$

✅ Enable only what you need for the current task.
   Disable unused MCP servers in opencode.json.
```

### 7. Start Fresh Sessions for Unrelated Tasks

```text
❌ One session for: "Fix auth bug, then build a new feature,
   then update documentation, then refactor the database"
   → Context window fills up, agent gets confused, costs rise

✅ Separate sessions for each task:
   Session 1: Fix auth bug
   Session 2: Build new feature
   Session 3: Update docs
   Session 4: Refactor database
   → Each session is clean and focused
```

## Cost Comparison Examples

| Scenario | Model | Tokens Used | Approx Cost |
|----------|-------|-------------|-------------|
| Fix typo in 1 file | GPT-4o-mini | 500 | $0.0003 |
| Refactor 5 files | GPT-4o | 10,000 | $0.03 |
| Architect a system | Claude 3.5 Sonnet | 15,000 | $0.06 |
| Full codebase review | GPT-4o | 100,000 | $0.30 |
| Session with large context | Claude 3.5 Sonnet | 500,000 | $3.00 |

## Optimization Checklist

```text
Before each prompt, ask yourself:

[ ] Can I use a cheaper model for this task?
[ ] Did I attach only the files I need?
[ ] Is it time to /compact the session?
[ ] Am I using @scout instead of @explore?
[ ] Did I mention specific filenames in my prompt?
[ ] Do I need all these MCP servers enabled?
[ ] Should I start a new session instead?
[ ] Am I in Plan mode (read-only) or Build mode?
```

## Pro Tips

```text
1. Set a default cheap model and switch up when needed:
   Default: gpt-4o-mini (cheap)
   Complex tasks: Switch to gpt-4o or Claude

2. Use temperature 0.0 for debugging:
   - Always gives the same result for the same input.
   - Saves tokens by being more direct.

3. Batch related changes in one session:
   - "Add error handling to all API routes" (one prompt)
   - Instead of one prompt per route

4. Monitor your usage:
   - Most providers have a dashboard.
   - Track which models cost you the most.
   - Optimize your most expensive patterns.

5. Set a budget alert:
   - OpenAI: Usage limits in the dashboard.
   - Anthropic: Billing alerts.
   - OpenRouter: Credit limits.
```
