# Using OpenCode Features and Tips (6:25 - 10:15)

## App Settings and Customization

### Key Settings in the GUI

When you open the OpenCode desktop app, you'll find settings for:
- **Build**: Configure which version/build of OpenCode you're running
- **Model**: Select the default AI model for new sessions
- **Provider**: Choose which AI provider to use (OpenAI, Anthropic, etc.)
- **Theme**: Pick from built-in themes or create custom ones
- **Keybinds**: Customize keyboard shortcuts

### Equivalent CLI Configuration

Everything configurable in the GUI can also be done via the CLI through config files:

```json
// opencode.json (project-level) or ~/.config/opencode/opencode.json (global)
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "models": {
      "planner": "anthropic/claude-opus-4-5",
      "coder": "anthropic/claude-sonnet-4-5",
      "fast": "anthropic/claude-haiku-4-5"
    }
  },
  "theme": "catppuccin-mocha",
  "keybind": {
    "session:new": "ctrl+n",
    "session:close": "ctrl+w"
  }
}
```

## Model Strategy (The Key to Efficiency)

A well-designed model strategy is the **single most impactful optimization** you can make in OpenCode. The idea: match model capabilities to task requirements.

### Recommended 3-Tier Strategy

| Tier | Model | Cost | Best For |
|------|-------|------|----------|
| **Planning & Thinking** | Claude Opus, GPT-5, Gemini 2.5 Pro | $$$ | Architecture, complex logic, debugging |
| **Execution & Refactoring** | Claude Sonnet, GPT-4o, DeepSeek V4 | $$ | Feature implementation, code generation |
| **Bulk / Low-Risk Tasks** | Claude Haiku, GPT-4o Mini, Qwen | $ | Tests, docs, simple changes, linting |

### How to Implement Model Strategy

```json
{
  "agent": {
    "plan": {
      "model": "anthropic/claude-opus-4-5"
    },
    "build": {
      "model": "anthropic/claude-sonnet-4-5"
    },
    "fast": {
      "model": "anthropic/claude-haiku-4-5"
    }
  }
}
```

### Tips for Saving Money

1. **Use parallel sessions with cheap models**: Start 3-4 Haiku/Qwen sessions on different tasks instead of one expensive model doing everything sequentially
2. **Plan with expensive models, execute with cheap ones**: Let Opus design the architecture, then have Haiku implement each piece
3. **Tier your tasks**: Low-risk tasks (formatting, comments, simple refactors) → cheap models. High-risk tasks (API design, security, complex logic) → expensive models

## Splitting New Sessions in Parallel

### The Root Terminal Pattern

```
┌────────────────────────────────────────┐
│  Root Terminal (your main session)     │
│  - Tracks all sub-sessions             │
│  - Maintains global context            │
│  - Orchestrates parallel agents        │
├────────────────────────────────────────┤
│                                        │
│  Sub-session 1: Refactor auth module   │
│  Sub-session 2: Write API tests        │
│  Sub-session 3: Update documentation   │
│  Sub-session 4: Review PR #142         │
│                                        │
└────────────────────────────────────────┘
```

### How to Do This in the Terminal

1. **Start in a project directory**: `cd /path/to/project`
2. **Launch root session**: `opencode`
3. **Spawn sub-agents**: Use `/agent` commands or keyboard shortcuts
4. **Monitor progress**: Switch between sessions with keyboard shortcuts
5. **Review and merge**: Each session produces results independently

### CLI Flow for Parallel Sessions

```bash
# Terminal 1: Root orchestrator
cd ~/projects/my-app
opencode

# In OpenCode's TUI, spawn sub-agents:
# Ctrl+Shift+N to create new session
# Tab to switch between sessions
# /agent plan to launch a planning sub-agent
```

## General Tips

- **Use @ to reference files**: `@src/auth/login.tsx` — OpenCode fuzzy-searches for files
- **Drag and drop images**: Add screenshots to your prompts for visual context
- **Use Plan mode first**: Press Tab to switch to Plan mode, review the plan, then switch back to Build mode
- **Commit AGENTS.md**: This file stores project context — commit it to git
- **Use /undo freely**: You can undo multiple steps if the AI goes in the wrong direction
