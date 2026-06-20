# Installing OpenCode in Terminal (4:31 - 6:25)

## Terminal Installation Guide

### Quick Install (All Platforms)

The easiest way to install OpenCode in your terminal:

```bash
curl -fsSL https://opencode.ai/install | bash
```

This script auto-detects your OS and architecture, downloads the correct binary, and installs it.

### Package Manager Installation

#### macOS (Homebrew)
```bash
brew install anomalyco/tap/opencode
```

#### Linux
```bash
# Using the install script (recommended)
curl -fsSL https://opencode.ai/install | bash

# Using Homebrew on Linux
brew install anomalyco/tap/opencode

# Arch Linux
sudo pacman -S opencode           # Stable
paru -S opencode-bin              # Latest from AUR
```

#### Windows
```bash
# Using WSL (recommended for best experience)
# Install WSL first, then use Linux instructions

# Using Chocolatey
choco install opencode

# Using Scoop
scoop install opencode

# Using npm
npm install -g opencode-ai
```

#### Node.js Package Managers
```bash
npm install -g opencode-ai
bun install -g opencode-ai
pnpm install -g opencode-ai
yarn global add opencode-ai
```

#### Docker
```bash
docker run -it --rm ghcr.io/anomalyco/opencode
```

### Using OpenCode Web

OpenCode also offers a **web interface** at [https://opencode.ai](https://opencode.ai) — no installation needed:
- Browser-based access to OpenCode's features
- Useful for quick tasks or when you can't install software
- Share sessions publicly via generated links

### Warp.dev Integration

If you're using **[Warp](https://warp.dev)** (a modern Rust-based terminal):
- Warp has native OpenCode integration
- OpenCode commands work seamlessly within Warp's IDE-like terminal
- Features: AI command search, block-based editing, and OpenCode session management
- Install OpenCode via Warp's built-in package manager

### Post-Installation Verification

After installing, verify it works:
```bash
opencode --version
# Should output version (e.g., 1.9.0+...)

opencode --help
# Shows available commands and options
```

### Next Steps After Installation

1. Run `opencode` to start the TUI
2. Use `/connect` to add an AI provider
3. Navigate to a project and run `opencode` again
4. Use `/init` to initialize project context
5. Start coding with your first prompt
