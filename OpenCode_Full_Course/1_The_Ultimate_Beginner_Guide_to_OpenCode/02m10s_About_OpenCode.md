# About OpenCode

## What Is OpenCode?

OpenCode is an **open-source, provider-agnostic AI coding assistant** built as a direct alternative to GitHub Copilot Codex, Anthropic Claude Code, and Cursor. It helps developers write, refactor, debug, and understand code using natural language — all from the terminal.

Unlike proprietary tools, OpenCode gives you:

- **Full control** over which AI model you use.
- **Complete transparency** — every action is logged and undoable.
- **No vendor lock-in** — switch providers whenever you want.
- **Offline capability** — run local models with Ollama.

## Why OpenCode Matters

The AI coding tool landscape has been dominated by closed-source, provider-locked solutions. OpenCode changes this by being:

### 1. Truly Open Source

```text
Proprietary Tools:    Source code hidden, cannot audit, cannot modify.
OpenCode:             Apache 2.0 licensed, fully auditable, forkable.
```

### 2. Provider Agnostic

```text
Copilot:    Only works with OpenAI models.
Claude Code: Only works with Anthropic models.
Cursor:     Limited provider options.
OpenCode:   Works with ANY provider — OpenAI, Anthropic, Google,
            Mistral, Groq, Ollama, Zen, custom endpoints.
```

### 3. Agent-Based Architecture

Instead of a single AI brain, OpenCode uses **specialized agents**:

| Agent | Job |
|-------|-----|
| General | All-purpose coding assistant |
| Explore | Codebase exploration and analysis |
| Scout | Fast targeted searches |
| Build | Implementation mode (writes code) |
| Plan | Analysis mode (reads only) |

### 4. Cost Effective

With OpenCode, you can:
- Use cheap models for simple tasks (gpt-4o-mini, llama3).
- Use powerful models only for complex work (claude-3.5-sonnet, gpt-4o).
- Run completely free with local Ollama models.
- No monthly subscription fees — pay only for API usage.

## Why Is OpenCode Better Than Claude Code or Codex or Cursor?

| Feature | OpenCode | Claude Code | GitHub Copilot | Cursor |
|---------|----------|-------------|----------------|--------|
| Open Source | Yes (Apache 2.0) | No | No | No |
| Provider Agnostic | Yes | No (Anthropic only) | No (OpenAI only) | Limited |
| Local Models | Yes (Ollama) | No | No | No |
| Multi-Interface | CLI, TUI, Web, API | CLI only | IDE only | IDE only |
| MCP Support | Yes | Yes | No | No |
| Undo/Redo | Yes | Yes | No | Yes |
| Custom Agents | Yes | Limited | No | Limited |
| Cost Control | Full | Limited | Subscription | Subscription |
| Plugin System | Yes | No | No | Limited |

## Who Is OpenCode For?

- **Solo developers** who want a free, powerful AI coding assistant.
- **Teams** that need consistent, customizable AI workflows.
- **Enterprises** that require data privacy and local models.
- **Open source contributors** who value transparency and auditability.
- **Anyone** who wants to avoid vendor lock-in with AI tools.

## Core Philosophy

```
OpenCode believes:
  - AI coding tools should be open, not locked behind paywalls.
  - You should own your workflow and your data.
  - The best AI model depends on the task, not the tool.
  - Every action should be reversible and transparent.
```

Now that you understand what OpenCode is and why it matters, let us look at what you need to know before getting started.
