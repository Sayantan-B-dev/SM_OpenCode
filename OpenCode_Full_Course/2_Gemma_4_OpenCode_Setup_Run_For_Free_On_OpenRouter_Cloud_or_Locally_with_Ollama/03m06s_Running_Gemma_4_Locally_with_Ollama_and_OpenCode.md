# Running Gemma 4 Locally with Ollama and OpenCode

This section provides a complete step-by-step guide to running Gemma 4 on your own machine using Ollama and connecting it to OpenCode.

## Prerequisites

Before starting, make sure you have:

- **OpenCode installed** (see the beginner guide if not).
- **At least 4 GB of free RAM** (more for larger variants).
- **Ollama** installed (instructions below).

## Step 1: Install Ollama

### On Windows

```powershell
# Option 1: Download from website
# Go to https://ollama.ai/download and run the installer.

# Option 2: Using winget
winget install Ollama.Ollama

# Option 3: Using PowerShell (irm)
irm https://ollama.ai/install.ps1 | iex
```

### On macOS

```bash
brew install ollama
```

### On Linux

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Verify Installation

```bash
ollama --version
```

## Step 2: Pull Gemma 4

Once Ollama is installed, pull the Gemma 4 variant you want to use.

```bash
# Pull the lightweight E2B variant (2.6B params)
ollama pull gemma4:e2b

# Pull the balanced E4B variant (4B params)
ollama pull gemma4:e4b

# Pull the 26B variant (requires ~16-20 GB RAM)
ollama pull gemma4:26b

# Pull the full 32B variant (requires ~20-24 GB RAM)
ollama pull gemma4:32b
```

### Choose Based on Your Hardware

| Your RAM | Recommended Variant | Pull Command |
|----------|-------------------|--------------|
| 4-8 GB | E2B (2.6B) | `ollama pull gemma4:e2b` |
| 8-16 GB | E4B (4B) | `ollama pull gemma4:e4b` |
| 16-24 GB | 26B (26B) | `ollama pull gemma4:26b` |
| 24-32 GB+ | 32B (32B) | `ollama pull gemma4:32b` |

### Verify the Download

```bash
ollama list
```

You should see your pulled model(s) listed:

```text
NAME              ID              SIZE
gemma4:e2b        xxxxxxxx        1.6 GB
gemma4:e4b        xxxxxxxx        2.5 GB
```

## Step 3: Test Gemma 4 Locally

Before connecting to OpenCode, verify the model works:

```bash
# Run interactively
ollama run gemma4:e2b

# Ask a question
>>> Write a JavaScript function to merge two sorted arrays
```

Type `/bye` or `Ctrl+D` to exit.

### Check Model Context Size

```bash
# While the model is running (in another terminal)
ollama ps
```

This shows:

```text
NAME              ID              SIZE   PROCESSOR    UNTIL
gemma4:e2b        xxxxxxxx        1.6 GB  100% CPU    4m from now
```

The context size is shown in Ollama's internal settings. For Gemma 4, the default context size is typically **8192 tokens**. You can customize this.

### Customize Context Size

Create a custom model with a larger context window:

```bash
# Create a Modelfile
echo "FROM gemma4:e2b
PARAMETER num_ctx 16384
PARAMETER temperature 0.3" > Modelfile

# Build the custom model
ollama create gemma4:e2b-ctx16k -f Modelfile

# Now use the custom model
ollama run gemma4:e2b-ctx16k
```

## Step 4: Run OpenCode with Gemma 4

### Method A: Using the Interactive TUI

```bash
# Start OpenCode
opencode
```

Then inside OpenCode:

```text
> /connect ollama
> /model gemma4:e2b
```

### Method B: Configure in opencode.json

Create or edit your project's `opencode.json`:

```jsonc
{
  "provider": {
    "name": "ollama",
    "model": "gemma4:e2b",
    "baseUrl": "http://localhost:11434",
    "options": {
      "temperature": 0.3,
      "numCtx": 8192
    }
  }
}
```

### Method C: One-Shot Commands

```bash
# Ask a quick question
opencode run "Explain this Python code" --model ollama/gemma4:e2b

# Generate code
opencode run "Write a React hook for debouncing" --model ollama/gemma4:e4b

# Complex task with larger model
opencode run "Design a database schema for a blog" --model ollama/gemma4:26b
```

## Step 5: Launch OpenCode with Ollama

Ollama can automatically manage the model for OpenCode:

```bash
# Start Ollama service (if not already running)
ollama serve

# In another terminal, start OpenCode
opencode
```

Inside OpenCode, the model will be loaded on demand when you send your first prompt.

### Running Both Together

For the smoothest experience, run Ollama in one terminal and OpenCode in another:

```text
Terminal 1:
> ollama serve
[Ollama] Listening on http://localhost:11434

Terminal 2:
> opencode
> /model gemma4:e4b
```

## Step 6: Performance Optimization

### Check Running Models

```bash
ollama ps
```

This shows currently loaded models and their resource usage.

### Monitor Resource Usage

```bash
# Windows
tasklist | findstr ollama

# macOS/Linux
ps aux | grep ollama
```

### Free Up Memory

```bash
# Stop a running model
ollama stop gemma4:e4b

# Or stop all models
ollama stop --all
```

### Use WSL for Better Performance (Windows)

OpenCode works better on WSL (Windows Subsystem for Linux) than on native Windows for local model execution:

```powershell
# Install WSL (if not already installed)
wsl --install

# Open WSL
wsl

# Inside WSL, install Ollama and OpenCode
curl -fsSL https://ollama.ai/install.sh | sh
npm install -g @opencode/core

# Pull Gemma 4
ollama pull gemma4:e4b

# Run OpenCode
opencode
```

## Complete Local Setup Example

Here is the full sequence from start to finish:

```bash
# 1. Install Ollama
# Windows: winget install Ollama.Ollama
# macOS: brew install ollama
# Linux: curl -fsSL https://ollama.ai/install.sh | sh

# 2. Pull Gemma 4
ollama pull gemma4:e4b

# 3. Start Ollama service
ollama serve

# 4. In a new terminal, create opencode.json
echo '{
  "provider": {
    "name": "ollama",
    "model": "gemma4:e4b",
    "baseUrl": "http://localhost:11434"
  }
}' > opencode.json

# 5. Run OpenCode
opencode

# 6. Inside OpenCode, try
> Write a function to check if a string is a palindrome
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Model not found | Run `ollama list` to verify it was pulled |
| Out of memory | Use a smaller variant (E2B instead of E4B) |
| Slow responses | Check CPU/GPU usage; use smaller context size |
| Connection refused | Ensure `ollama serve` is running |
| OpenCode cannot connect | Check `baseUrl` in config (default: `http://localhost:11434`) |
| WSL not working | Run `wsl --update` and restart |

## Summary

```bash
# Quick start (3 commands)
ollama pull gemma4:e4b
opencode init --provider ollama
opencode

# Inside OpenCode
> /model ollama/gemma4:e4b
> Create a todo list app with React
```

Running Gemma 4 locally gives you a completely free, private AI coding assistant with no internet required and no rate limits.
