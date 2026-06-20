# Introducing OpenCode (0:13 - 0:31)

## How OpenCode Solves the Attention Problem

OpenCode is purpose-built to solve the bottleneck of attention through its **multi-agent, multi-session architecture**. Here's how each feature directly addresses the limitations of traditional AI coding tools.

### Core Problem-Solving Features

| Traditional Problem | OpenCode Solution |
|-------------------|-------------------|
| One conversation at a time | Multiple parallel agent sessions |
| Lost context between sessions | Shared `AGENTS.md` and memory files |
| Single model limitation | Any model from 75+ providers |
| No project awareness | LSP integration and project initialization |
| Manual context switching | Root session orchestration |

### Key Capabilities

- **Multi-session parallelism**: Start multiple agents simultaneously, each working on independent parts of your project
- **Shared memory system**: Every session writes to `AGENTS.md` and `.agent/` files, creating a persistent project brain
- **Flexible model strategy**: Use Claude for planning, GPT-4 for coding, and cheaper models for bulk tasks — all within the same project
- **LSP-aware coding**: OpenCode automatically detects and loads the right Language Server Protocols, giving the AI deep understanding of your code's types, references, and definitions
- **Undo/redo at conversation level**: Every change is tracked, so you can `/undo` multiple steps if something goes wrong
- **Session sharing**: Share any conversation with your team via a URL using `/share`

### The Architecture

OpenCode operates on a simple but powerful loop:
1. **Setup access** → Connect your preferred AI providers
2. **Models & providers** → Configure which models handle which types of work
3. **Customization & power** → Add MCPs, skills, custom commands, and rules
4. **Advanced workflow** → Run multiple agents in parallel with orchestration

This architecture makes OpenCode not just another AI coding tool, but a **complete AI-powered development platform**.
