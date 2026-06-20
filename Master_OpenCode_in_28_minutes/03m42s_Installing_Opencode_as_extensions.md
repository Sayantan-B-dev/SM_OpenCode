# Installing OpenCode as Extensions (3:42 - 4:31)

## IDE Extensions: Integrating OpenCode into Your Editor

OpenCode isn't just a standalone tool — it integrates directly into your existing development environment through IDE extensions.

### Installing via Desktop App (Built-in)

The OpenCode desktop app comes with:
- A built-in terminal with full TUI
- File explorer for project navigation
- Session management with visual tabs
- Integrated settings panel
- Automatic updates

**Pros**: All-in-one solution, no setup required beyond installation
**Cons**: Separate application to manage alongside your editor

### Installing IDE Extensions

OpenCode offers extensions for popular editors:

#### VS Code Extension
- Install from the VS Code marketplace
- Adds OpenCode commands directly to the command palette
- Opens OpenCode sessions within VS Code's integrated terminal
- Syncs with your project's `.opencode/` configuration

#### Other Editors
- JetBrains IDEs (IntelliJ, WebStorm, PyCharm, etc.)
- Neovim (via plugin)
- Terminal emulator integration (WezTerm, Alacritty, Kitty, Ghostty)

#### How to Install via Extension

1. Open your editor's extension marketplace
2. Search for "OpenCode"
3. Click Install
4. OpenCode appears as a new panel or terminal option

### CLI vs GUI: Trade-offs

| Aspect | CLI (Terminal) | GUI (Desktop App) |
|--------|---------------|-------------------|
| **Learning curve** | Steeper — commands and shortcuts | Gentler — visual menus |
| **Speed** | Faster — keyboard-driven | Slightly slower — mouse navigation |
| **Flexibility** | More — pipe, script, automate | Less — constrained to UI options |
| **Multi-session** | Root terminal orchestration | Visual session browser |
| **Resource footprint** | Minimal | Moderate |
| **Remote usage** | Native (SSH) | Limited |
| **Customization** | Maximum (config files) | Good (settings UI + config) |

### Hybrid Approach

Many experienced OpenCode users run **both**: the desktop app for visual session management and the CLI for quick commands, scripting, and remote access.
