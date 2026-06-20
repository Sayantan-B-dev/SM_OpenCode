# Setting Up OpenCode

This section walks you through installing OpenCode on any platform, verifying the installation, and running your first session.

## Installation Methods

Choose the method that works best for your operating system:

### Option 1: npm (Recommended)

Works on all platforms. Requires Node.js 18+.

```bash
npm install -g @opencode/core
```

### Option 2: Chocolatey (Windows)

```powershell
choco install opencode
```

### Option 3: Homebrew (macOS / Linux)

```bash
brew install opencode/tap/opencode
```

### Option 4: Docker

Works on any platform with Docker installed.

```bash
docker pull opencode/opencode

docker run -it --rm \
  -v "$(pwd):/workspace" \
  -v "$HOME/.config/opencode:/root/.config/opencode" \
  opencode/opencode
```

### Option 5: Curl (Cross-Platform)

```bash
# Linux/macOS
curl -fsSL https://opencode.ai/install.sh | sh

# Windows PowerShell
iwr -useb https://opencode.ai/install.ps1 | iex
```

### Option 6: From Source

```bash
git clone https://github.com/anomalyco/opencode.git
cd opencode
npm install
npm run build
npm link
```

## Verify Installation

After installing, check that everything works:

```bash
# Check version
opencode --version

# Check help
opencode --help

# Quick test (non-interactive)
opencode run "Hello, what can you do?"
```

## First Run: The /init Command

Before using OpenCode, you must run the initialization command:

```bash
opencode init
```

This will:
1. Prompt you to select an AI provider (Zen, OpenAI, Anthropic, etc.).
2. Ask for your API key.
3. Create the global config file at `~/.config/opencode/opencode.json`.
4. Optionally generate a project-level `agents.md` file.

**Why /init is important:**
- It sets up your default provider and model.
- It creates the configuration structure OpenCode needs.
- Without it, OpenCode will not know which AI to talk to.

## Understanding Modes

OpenCode has two primary modes you can use:

| Mode | Command | Description |
|------|---------|-------------|
| Interactive TUI | `opencode` | Full terminal user interface |
| One-shot CLI | `opencode run "..."` | Single query, returns result |

### Interactive TUI

```bash
opencode
```

Launches the full TUI with:
- Chat interface
- File tree
- Command history
- Mode switching (Plan / Build)

### One-Shot CLI

```bash
opencode run "Explain how closures work in JavaScript"
opencode run "Create a todo list app with React" --model openai/gpt-4o
```

### Plan Mode vs Build Mode

Press `Tab` inside the TUI to switch between them:

| Mode | Can Read Files | Can Write Files | Best For |
|------|:-------------:|:---------------:|----------|
| Plan | Yes | No | Analysis, planning, code review |
| Build | Yes | Yes | Implementation, refactoring, fixes |

**Best practice:** Always start in Plan mode for unfamiliar tasks, then switch to Build mode to implement.

## Demo: Creating a Next.js App with Plan and Build Modes

### Step 1: Plan the App

```
> [Tab -> Plan mode]
> Plan a Next.js todo app with the following features:
  - User authentication
  - CRUD operations for todos
  - SQLite database
  - Tailwind CSS styling
```

The agent will analyze the requirements and present a plan without changing any files.

### Step 2: Review the Plan

Read the plan carefully. The agent will suggest:
- Project structure
- Dependencies
- Route design
- Database schema

### Step 3: Implement

```
> [Tab -> Build mode]
> Implement the todo app as planned
```

Now the agent creates files, installs dependencies, and sets up the project.

### Step 4: Verify

```
> Run the development server and check if everything works
```

## Checking Version and Configuration

```bash
# Current version
opencode --version

# Verbose info
opencode --verbose

# Config file location (Windows)
type %USERPROFILE%\.config\opencode\opencode.json

# Config file location (macOS/Linux)
cat ~/.config/opencode/opencode.json
```

## Next Steps

Now that OpenCode is installed, the next section covers all the core commands you will use every day.
