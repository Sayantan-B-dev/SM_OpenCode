# TUI, Terminal UI, Desktop, Web App, IDE

OpenCode offers multiple interfaces to fit different workflows. This section covers all of them.

## Starting the TUI

The Terminal User Interface (TUI) is OpenCode's primary interactive mode.

```bash
# Start the interactive TUI
opencode
```

### TUI Layout

```
+----------------------------------------------------------+
| OpenCode v2.x | Provider: gpt-4o | Mode: Build    |
+----------------------------------------------------------+
|                                                           |
|  [Agent Response Area]                                    |
|  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                           |
|  Showing AI responses, diffs, search results...            |
|                                                           |
|  [Status Bar]                                             |
|  Model: gpt-4o | Tokens: 1,234 | Session: my-project      |
|                                                           |
|  [Input Prompt] > _                                       |
|  /undo  /redo  /share  /help  Tab: Plan/Build            |
+----------------------------------------------------------+
```

### TUI Features

| Feature | Description |
|---------|-------------|
| Chat Interface | Natural language conversation with AI |
| File Tree | Browse and attach files visually |
| Diff Viewer | See changes before they are applied |
| History | Scroll through past messages |
| Tab Completion | Auto-complete commands and filenames |
| Status Bar | Shows current model, mode, token count |

## Changing the Theme

```text
> /theme dark
> /theme light
> /theme dracula
> /theme monokai
```

Available themes depend on your terminal's capabilities. The TUI adapts to popular color schemes.

## Desktop App

OpenCode offers a desktop application for a more native experience.

### Downloading the Desktop App

Visit [opencode.ai/download](https://opencode.ai/download) to get the desktop version for:

- Windows (`.exe` installer)
- macOS (`.dmg` or `.app`)
- Linux (`.AppImage` or `.deb`)

### Desktop App Features

- Works similarly to the TUI but in a standalone window.
- Better font rendering and color support.
- Persistent sessions across app restarts.
- System tray integration.
- Keyboard shortcuts consistent with the TUI.

### Using with VS Code

To use OpenCode alongside VS Code:

1. Open VS Code.
2. Open the integrated terminal (`Ctrl + ``).
3. Run `opencode` in the terminal.

Alternatively, install the OpenCode VS Code extension (if available) for tighter integration.

## Web App

OpenCode can run as a web application, accessible from any browser.

### Starting the Web App

```bash
# Start with default settings
opencode web

# Custom port and host
opencode web --port 4096 --hostname 0.0.0.0

# With password protection
opencode web --port 4096 --password my-secret-password

# With custom username
opencode web --port 4096 --username admin --password secure123
```

### Web App Options

| Flag | Description | Default |
|------|-------------|---------|
| `--port` | Port to listen on | `3000` |
| `--hostname` | Host to bind to | `localhost` |
| `--password` | Password for authentication | None (open) |
| `--username` | Username for authentication | `admin` |

### Why Use the Web App

- **Team Collaboration**: Share the URL with teammates.
- **Remote Development**: Run OpenCode on a server, access via browser.
- **Tablet/Phone**: Use OpenCode from mobile devices on the same network.
- **CI/CD Monitoring**: View session logs from the web dashboard.

### Security Considerations

```text
WARNING: Do not expose OpenCode web to the internet without a password.
Use --password flag or a reverse proxy with authentication.
```

## IDE Extensions

OpenCode can integrate with popular IDEs:

| IDE | Integration Method |
|-----|--------------------|
| VS Code | Extension from marketplace or integrated terminal |
| JetBrains (IntelliJ, WebStorm) | Terminal plugin |
| Vim/Neovim | Terminal within editor |
| Emacs | Terminal within editor |

### Best Practice: Side-by-Side

The recommended setup is:

```text
+-------------------------------------+
|   VS Code (Editor)   |  Terminal   |
|                      |             |
|  [Code editing]      | [OpenCode]  |
|  [File browsing]     | [AI Chat]   |
|  [Debugging]         | [Commands]  |
+-------------------------------------+
```

This gives you:
- Your familiar editor for manual changes.
- OpenCode in the terminal for AI-powered assistance.
- Both visible at the same time.

## Summary: Which Interface to Use

| Interface | Best For | When to Use |
|-----------|----------|-------------|
| TUI (Terminal) | Daily coding, quick tasks | Always available, no setup needed |
| Desktop App | Long sessions, better visuals | When you want a standalone window |
| Web App | Team collaboration, remote access | When you need to share or access remotely |
| IDE Terminal | Tight editor integration | When you want OpenCode next to your code |

## Pro Tip

```text
Start with the terminal TUI. It is the most feature-rich and
reliable interface. Move to the web app when you need to
share sessions with teammates.
```
