# OpenCode — Intro & Overview

**OpenCode** is an open-source AI coding assistant that runs entirely in your terminal. It helps you write, refactor, debug, and scaffold code using AI models — all without leaving the command line.

Unlike GUI-based tools (Cursor, Copilot Chat), OpenCode gives you direct control over **which AI model**, **which provider**, and **how much context** to use.

## Why OpenCode?

| Feature | OpenCode | Other Tools |
|---------|----------|-------------|
| **Fully open source** | ✅ MIT License | ❌ Often proprietary |
| **Model choice** | Any provider (OpenAI, Anthropic, Google, local models) | Locked to one provider |
| **Runs in terminal** | ✅ No GUI needed | ❌ IDE-dependent |
| **Context window control** | Manual / agent-based | Automatic / hidden |
| **Custom sub-agents** | ✅ Yes | ❌ Rarely supported |

## Real-World Example

Instead of switching to a browser to ask ChatGPT, you just run:

```bash
# Start OpenCode in your project folder
opencode
```

Then inside OpenCode:

```text
> explain this whole repo to me
> find and fix all TypeScript errors
> add a login page with Next.js Auth
```

OpenCode reads your files, understands your project structure, and edits code directly.

## Key Concepts

- **Session-based** — each `/new` starts a fresh context
- **Agent-driven** — OpenCode uses agents to explore, plan, and execute
- **Skill-based** — you install "skills" (e.g., Next.js, shadcn) to give the agent domain knowledge
- **Plan mode** — press `Shift+Tab` to switch to planning before implementing

## When to Use OpenCode

- You want a **free AI coding assistant** (using OpenCode Zen or local models)
- You need **fine-grained control** over your AI provider
- You prefer **terminal-first workflows**
- You want **custom agents** that follow your project's conventions
