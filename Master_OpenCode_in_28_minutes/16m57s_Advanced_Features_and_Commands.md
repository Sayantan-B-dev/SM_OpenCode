# Advanced Features and Commands (16:57 - 18:50)

## Built-in Commands

OpenCode comes with several powerful built-in commands accessible from the TUI.

### `/init` — Project Initialization

The most important command when starting with a new project.

```bash
/init
```

**What it does:**
- Scans your entire project directory structure
- Analyzes package.json, tsconfig, and other config files
- Identifies coding patterns, frameworks, and conventions
- Creates an `AGENTS.md` file in the project root
- Sets up `.opencode/` directory for project-specific config

**Why it matters:**
Without `/init`, every session starts with zero context about your project. With `/init`, every agent immediately understands your project's architecture, dependencies, and coding patterns.

**Best practice:** Commit `AGENTS.md` to git so your team benefits from accumulated context.

### `/review` — Code Review Command

```bash
/review
```

**What it does:**
- Reviews the current state of your code
- Identifies potential issues, bugs, and improvements
- Suggests specific changes with reasoning

**Usage patterns:**
```bash
# Review recent changes
/review

# Review a specific file
"Review @src/lib/api.ts for security vulnerabilities"

# Review with specific focus
"Review the recent changes to authentication flow"
```

### Other Built-in Commands

| Command | Description |
|---------|-------------|
| `/undo` | Revert the last AI-generated changes (can be used multiple times) |
| `/redo` | Re-apply undone changes |
| `/share` | Generate a shareable URL for the current session |
| `/help` | Show available commands and keyboard shortcuts |
| `/models` | List and select available AI models |
| `/connect` | Add or change AI provider configuration |
| `/agent` | Manage agents (create, switch, configure) |

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Tab` | Toggle between Plan mode and Build mode |
| `Ctrl+T` | Open file/tool picker |
| `Ctrl+Shift+N` | Create new session |
| `@` | Fuzzy search files in the project |
| `Ctrl+N` / `Ctrl+W` | Navigate sessions (configurable) |

### Plan Mode vs Build Mode

OpenCode operates in two modes:

**Plan Mode** (press Tab):
- AI can only discuss and analyze
- Cannot make any file changes
- Use this to get architecture reviews, design discussions, implementation plans
- Safe to use — no unintended modifications

**Build Mode** (press Tab again):
- AI has full read/write access to files
- Can create, edit, and delete code
- Use this when you're ready to implement
- Changes can be `/undo`-ed

### Session Management

Advanced users can leverage:
- **Multiple parallel sessions**: Each with its own model, agent, and context
- **Session switching**: Quick keyboard navigation between sessions
- **Root orchestration**: One main session tracking all sub-sessions
- **Session sharing**: `/share` creates a URL for collaboration

### CLI Equivalents

Everything in the TUI can also be done via CLI:

```bash
# Run OpenCode in a project
opencode [directory]

# Direct commands
opencode --init
opencode --review

# Run specific agent
opencode --agent plan

# Show version
opencode --version
```
