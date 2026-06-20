# Adding Custom Commands (25:14 - 25:57)

## What Are Custom Commands?

Custom commands let you define **reusable prompts** that you can trigger with a simple `/command-name` in the TUI. They're perfect for automating repetitive tasks like running tests, reviewing code, or generating components.

## Creating Custom Commands

There are two ways to define custom commands.

### Method 1: Markdown Files (Recommended)

Create `.md` files in the `commands/` directory:

```
# Project-level
.opencode/commands/test.md

# Global (available to all projects)
~/.config/opencode/commands/test.md
```

Example: `.opencode/commands/test.md`

```markdown
---
description: Run tests with coverage
agent: build
model: anthropic/claude-sonnet-4-5
---

Run the full test suite with coverage report and show any failures.
Focus on the failing tests and suggest fixes.
```

Usage: `/test`

### Method 2: opencode.json Config

```json
{
  "$schema": "https://opencode.ai/config.json",
  "command": {
    "test": {
      "template": "Run the full test suite with coverage report and show any failures.\nFocus on the failing tests and suggest fixes.",
      "description": "Run tests with coverage",
      "agent": "build",
      "model": "anthropic/claude-sonnet-4-5"
    }
  }
}
```

Usage: `/test`

## Advanced: Arguments

Pass arguments to custom commands using `$ARGUMENTS` or positional parameters.

### Using $ARGUMENTS

```markdown
---
description: Create a new component
---

Create a new React component named $ARGUMENTS with TypeScript support.
Include proper typing and basic structure.
```

Usage: `/component Button`

### Using Positional Parameters

```markdown
---
description: Create a new file
---

Create a file named $1 in the directory $2 with the following content:
$3
```

Usage: `/create-file config.json src "{ \"key\": \"value\" }"`

## Advanced: Shell Output

Inject command output into your prompt using `command`:

```markdown
---
description: Analyze test coverage
---

Here are the current test results:
!`npm test`

Based on these results, suggest improvements to increase coverage.
```

## Advanced: File References

Include files in your command using `@`:

```markdown
---
description: Review component
---

Review the component in @src/components/Button.tsx.
Check for performance issues and suggest improvements.
```

## Command Configuration Options

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `template` | String | Yes | The prompt sent to the LLM |
| `description` | String | No | Shown in TUI command palette |
| `agent` | String | No | Which agent to use (e.g., "plan", "build") |
| `subtask` | Boolean | No | Force subagent invocation (isolated context) |
| `model` | String | No | Override model for this command |

### Example with All Options

```json
{
  "command": {
    "analyze": {
      "template": "Analyze the performance of @src/lib/database.ts and suggest optimizations",
      "description": "Analyze database performance",
      "agent": "plan",
      "subtask": true,
      "model": "anthropic/claude-opus-4-5"
    }
  }
}
```

## Built-in vs Custom Commands

### Built-in Commands (always available)
- `/init` — Initialize project context
- `/undo` — Undo last change
- `/redo` — Redo undone change
- `/share` — Share session via URL
- `/help` — Show help
- `/models` — List/select models
- `/connect` — Configure providers

### Custom Commands (your definitions)
- Can override built-in commands with the same name
- Are listed in the TUI command palette
- Support arguments, shell output, and file references

### Tips for Effective Custom Commands

1. **Use descriptive names**: `/test`, `/lint`, `/deploy`, `/review`
2. **Set the right agent**: Use `plan` agent for analysis, `build` for execution
3. **Set the right model**: Use cheaper models for simple tasks
4. **Use `subtask: true`**: Keeps the command's context isolated from your main session
5. **Commit commands to the repo**: Share useful commands with your team via `.opencode/commands/`
