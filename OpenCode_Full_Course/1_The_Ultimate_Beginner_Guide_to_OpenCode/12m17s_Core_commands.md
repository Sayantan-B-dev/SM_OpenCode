# Core Commands

This section covers every essential command you will use with OpenCode daily. Commands are organized by category for easy reference.

## File and Context Commands

### @ — Attach Files via Fuzzy Search

Use the `@` symbol to quickly attach files to your prompt. OpenCode will fuzzy-search your project.

```
> @auth.ts add JWT refresh logic
> @components/Button.tsx fix the hover style
> @utils/api.ts add retry logic
```

**How it works:**
- Type `@` followed by part of the filename.
- OpenCode searches the project tree.
- The matched file is attached to the current context.
- The agent can now read and modify that file.

### /connect — Connect to Custom Providers

Connect to a self-hosted or custom provider endpoint.

```
> /connect https://my-ai-server.com/v1 --api-key my-key
```

**Use cases:**
- Connect to your own fine-tuned model.
- Connect to a company-internal AI endpoint.
- Switch between different provider instances.

### /sessions — Manage Sessions

View and manage your OpenCode sessions.

```
> /sessions
```

| Command | Description |
|---------|-------------|
| `/sessions` | List all sessions |
| `/sessions rename my-session` | Rename current session |
| `/sessions delete old-session` | Delete a session |

Sessions persist your conversation history and undo/redo log.

### /share — Share Your Session

Share your current session with teammates or save it for later.

```
> /share
[System] Session shared: https://opencode.ai/share/abc123
```

| Command | Description |
|---------|-------------|
| `/share` | Generate a shareable link |
| `/unshare` | Remove the shared link |
| `/share --readonly` | Share in read-only mode |

**Use cases:**
- Pair programming sessions.
- Asking for help with a specific issue.
- Documenting a troubleshooting process.

### /review-recent — Review Recent Changes

Review recent modifications made by the agent.

```
> /review-recent
```

Shows a summary of:
- Files modified in the last N actions.
- What was changed (diff-like output).
- Status of each change (applied, undone).

## Undo and Redo Commands

### /undo — Revert Last Action

```text
> /undo
[System] Reverted: edit("src/auth.ts")
```

- Reverses the most recent agent action.
- Works for file edits, writes, and deletes.
- Each undo restores the file to its previous state.

### /redo — Reapply Last Undone Action

```text
> /redo
[System] Reapplied: edit("src/auth.ts")
```

- Reapplies the most recently undone action.
- Useful if you undo too far by accident.

## Custom Commands

Custom commands are shortcuts you define in `opencode.jsonc`. They let you create reusable prompts.

### Defining Custom Commands

Create them in your project's `opencode.jsonc`:

```jsonc
{
  "$schema": "https://opencode.ai/schemas/opencode.json",
  "customCommands": [
    {
      "name": "test",
      "prompt": "Run the test suite. If tests fail, fix them and run again. $ARGUMENTS",
      "description": "Run tests with optional filter"
    },
    {
      "name": "review",
      "prompt": "Review all changes in the current branch. Analyze for bugs, security issues, and code quality. $ARGUMENTS",
      "description": "Code review for current branch"
    },
    {
      "name": "deploy",
      "prompt": "Deploy the application to $ARGUMENTS environment. Run tests first, then build, then deploy.",
      "description": "Deploy to environment"
    },
    {
      "name": "component",
      "prompt": "Create a new React component named $ARGUMENTS with TypeScript, tests, and storybook stories.",
      "description": "Generate a new component"
    },
    {
      "name": "docs",
      "prompt": "Generate documentation for $ARGUMENTS. Read the file, understand the code, and produce docs.",
      "description": "Document a file or module"
    },
    {
      "name": "fix",
      "prompt": "Fix the following issue: $ARGUMENTS",
      "description": "Fix a bug or issue"
    }
  ]
}
```

### Using Custom Commands

Once defined, use them with `/` prefix:

```
> /test
> /test --watch
> /component UserProfile
> /deploy staging
> /docs src/utils/api.ts
> /fix The login page crashes when email is empty
```

### How $ARGUMENTS Works

The `$ARGUMENTS` placeholder is replaced with whatever you type after the command name.

```
Command: /deploy production
Expands to: "Deploy the application to production environment. Run tests first, then build, then deploy."
```

## Session Management Commands

### /exit — Exit OpenCode

```text
> /exit
```

Or use `Ctrl + D` to exit the TUI.

### /compact — Compact Conversation

When your session gets long, use `/compact` to summarize the history.

```text
> /compact
[System] Compacted 24 messages into a summary. Context window freed.
```

This helps manage token usage in long sessions.

## Quick Reference Table

| Command | Description | Example |
|---------|-------------|---------|
| `@filename` | Attach a file via fuzzy search | `@auth.ts fix the login` |
| `/undo` | Revert last action | `/undo` |
| `/redo` | Reapply last undone action | `/redo` |
| `/connect` | Connect to custom provider | `/connect https://ai.example.com` |
| `/sessions` | List/manage sessions | `/sessions rename my-work` |
| `/share` | Share session link | `/share` |
| `/unshare` | Remove shared link | `/unshare` |
| `/review-recent` | Review recent changes | `/review-recent` |
| `/compact` | Compact conversation history | `/compact` |
| `/exit` | Exit OpenCode | `/exit` |
| `/command` | Run custom command | `/test` |

## Important Notes

- Commands starting with `/` are OpenCode built-in commands.
- Commands without `/` are natural language prompts for the AI.
- Use `@` for file attachment anywhere in your prompt.
- Custom commands you define appear in tab completion.
- All actions are undoable with `/undo` — experiment freely.
