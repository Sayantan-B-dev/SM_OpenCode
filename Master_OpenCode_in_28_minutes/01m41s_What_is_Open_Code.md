# What is OpenCode (1:41 - 3:19)

## Sequential vs. Parallel Coding

### The Old Way: Sequential Coding

```
Task A ──> Wait ──> Task B ──> Wait ──> Task C ──> Wait ──> Done
```

Each step blocks on the previous one. Your attention is a bottleneck.

### The OpenCode Way: Parallel Coding

```
         ┌── Agent A (Planning) ──┐
Root ────┼── Agent B (Coding) ────┼──> Review ──> Merge
         ├── Agent C (Testing) ───┤
         └── Agent D (Docs) ──────┘
```

All agents work simultaneously, coordinated by the root session.

## The OpenCode Workflow Cycle

OpenCode operates on a continuous loop with five stages:

```
Setup Access <──> Models & Providers <──> Customization & Power <──> Advanced Workflow
                                                                          │
                                                                          └──> Back to Setup (iteration)
```

### Stage 1: Setup Access
- Connect AI providers (OpenAI, Anthropic, local models, etc.)
- Configure API keys via `/connect` command or environment variables
- Use OpenCode Zen for tested, verified models

### Stage 2: Models & Providers
- Choose from 75+ providers via Models.dev
- Configure model strategy (which models for which tasks)
- Set up local models via Ollama, LM Studio, or llama.cpp

### Stage 3: Customization & Power
- Add **MCP servers** for external tool integration
- Create **skills** for reusable instructions
- Define **custom commands** for repetitive tasks
- Configure **rules** and **permissions**
- Set up **LSP servers** for deep code understanding

### Stage 4: Advanced Workflow
- Run multiple parallel agent sessions
- Use `/init` to create project context
- Orchestrate sub-agents from the root terminal
- Share sessions with `/share`

## Center: OpenCode

At the center of everything is **OpenCode itself** — the open-source AI coding agent that ties it all together. Available as:
- **Terminal (TUI/CLI)**: Full-featured terminal interface
- **Desktop App**: Native GUI for macOS, Windows, Linux (beta)
- **IDE Extensions**: VS Code and other editor integrations
- **Web Interface**: Browser-based access
- **Mobile**: Via SSH connection

OpenCode is free, open source (160K+ GitHub stars), used by 7.5M+ developers monthly, and built for privacy — it does not store your code or context data.
