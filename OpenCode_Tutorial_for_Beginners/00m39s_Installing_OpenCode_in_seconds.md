# Installing OpenCode in Seconds

OpenCode is distributed as an npm package. You install it globally and run it from any project folder.

## Prerequisites

- **Node.js** v18 or later (LTS recommended)
- **npm** (comes with Node.js) or **pnpm** / **yarn**

## Installation

```bash
# Install globally via npm
npm install -g @opencode/core

# Or via pnpm
pnpm add -g @opencode/core

# Or via yarn
yarn global add @opencode/core
```

## Verify Installation

```bash
opencode --version
```

You should see something like: `v0.x.x`

## Using OpenCode in a Project

```bash
# Navigate to your project
cd my-project

# Start OpenCode
opencode
```

This launches OpenCode in your terminal. The first time you run it, you'll be prompted to **select an AI provider** (see the next lesson).

## Quick Start (Step-by-Step)

1. Create a new folder and enter it:
   ```bash
   mkdir my-app && cd my-app
   ```
2. Start OpenCode:
   ```bash
   opencode
   ```
3. Follow the on-screen setup:
   - Choose a provider (OpenCode Zen is free, no signup)
   - Pick a model (e.g., GPT-4o, Claude, or a free model)
   - Start prompting!

## Updating OpenCode

```bash
npm update -g @opencode/core
```

## Real Example

```text
$ opencode
✔ Provider › OpenCode Zen (Free)
✔ Model › gpt-4o-mini (free)

  ◇  OpenCode is ready!
  ◇  Type /help for commands
  ◇  Type /new to start a fresh session

> create a new React component called Button
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `command not found: opencode` | Ensure global npm bin is in your PATH |
| Version too old | Run `npm update -g @opencode/core` |
| Permission errors (macOS/Linux) | Try `sudo npm install -g @opencode/core` |
