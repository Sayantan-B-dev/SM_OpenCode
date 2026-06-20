# Local Ollama and OpenRouter Setup

This section covers two important setups: running models locally with Ollama, and accessing many models through OpenRouter's unified API.

## Why Use Local Models?

Running models locally has several advantages:

| Benefit | Description |
|---------|-------------|
| **Privacy** | Your code never leaves your machine |
| **Free** | No API costs after initial setup |
| **Offline** | Works without internet |
| **No Limits** | No rate limits or quotas |
| **Low Latency** | No network round-trips |

### When to Use Local Models

```text
Use Local When:
  - You work with sensitive/proprietary code.
  - You have limited or no internet access.
  - You want to avoid API costs.
  - You are experimenting and iterating rapidly.

Use Cloud When:
  - You need the most capable models (GPT-4o, Claude).
  - Your local hardware is not powerful enough.
  - You need faster responses than your local GPU provides.
```

## Setting Up Ollama

### Step 1: Install Ollama

Download from the official website:

```text
https://ollama.ai/download
```

Or use the terminal:

```bash
# Windows
winget install Ollama.Ollama

# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.ai/install.sh | sh
```

### Step 2: Verify Installation

```bash
ollama --version
```

### Step 3: Pull a Model

```bash
# Pull a code-specialized model (recommended)
ollama pull codellama

# Other good options:
ollama pull llama3          # General purpose
ollama pull deepseek-coder  # Code specialized
ollama pull mistral         # Lightweight
ollama pull qwen2.5-coder   # Strong for coding

# List downloaded models
ollama list
```

### Step 4: Run a Model (Test)

```bash
ollama run codellama
```

This opens an interactive chat. Type `/bye` to exit.

## Configuring OpenCode to Use Ollama

### Step 1: Configure in opencode.json

```jsonc
{
  "provider": {
    "name": "ollama",
    "model": "codellama",
    "baseUrl": "http://localhost:11434",
    "options": {
      "temperature": 0.3,
      "numCtx": 4096    // Context window size
    }
  }
}
```

### Step 2: Verify Connection

```bash
# Check Ollama is running
ollama list

# Start OpenCode with Ollama
opencode run "Hello, what model are you using?" --model ollama/codellama
```

### Step 3: Customize Context Length

You can adjust the context length per model in Ollama:

```bash
# Create a custom model with larger context
ollama create my-codellama -f Modelfile
```

**Modelfile:**

```dockerfile
FROM codellama
PARAMETER num_ctx 8192
PARAMETER temperature 0.2
```

```bash
ollama create my-codellama -f Modelfile
```

Then update your config:

```jsonc
{
  "provider": {
    "name": "ollama",
    "model": "my-codellama",
    "baseUrl": "http://localhost:11434"
  }
}
```

### Step 4: Using Multiple Ollama Models

```jsonc
{
  "provider": {
    "name": "ollama",
    "model": "default",
    "baseUrl": "http://localhost:11434",
    "models": {
      "fast": {
        "model": "llama3",
        "temperature": 0.3
      },
      "code": {
        "model": "codellama",
        "temperature": 0.2,
        "numCtx": 8192
      },
      "powerful": {
        "model": "deepseek-coder",
        "temperature": 0.1
      }
    }
  }
}
```

Then switch between them:

```text
> /model code
> /model fast
> /model powerful
```

## Setting Up OpenRouter

OpenRouter provides a unified API to access many models from different providers with a single API key and billing.

### Why OpenRouter?

```text
One API key ──► 100+ models from all providers
Single billing ──► Unified invoice
Free tier ──► Some models available for free
```

### Step 1: Create an Account

```text
https://openrouter.ai
```

1. Sign up with email or GitHub.
2. Go to the API keys section.
3. Generate a new API key.
4. Copy the key (starts with `sk-or-v1-`).

### Step 2: Connect OpenCode to OpenRouter

```bash
# Using /connect
/connect openrouter --api-key sk-or-v1-your-key-here

# Or set environment variable
$env:OPENROUTER_API_KEY = "sk-or-v1-your-key-here"
```

### Step 3: Configure in opencode.json

```jsonc
{
  "provider": {
    "name": "openrouter",
    "model": "openai/gpt-4o",
    "apiKey": "$OPENROUTER_API_KEY",
    "baseUrl": "https://openrouter.ai/api/v1"
  }
}
```

### Step 4: Use OpenRouter Models

```bash
# List available models
/models

# Use specific models
opencode run "Hello" --model openrouter/openai/gpt-4o
opencode run "Hello" --model openrouter/anthropic/claude-3.5-sonnet
opencode run "Hello" --model openrouter/google/gemini-pro
opencode run "Hello" --model openrouter/mistral/mistral-large
```

### Free Models on OpenRouter

OpenRouter offers several free models:

| Model | Provider | Notes |
|-------|----------|-------|
| Llama 3 70B | Meta | Good general purpose |
| Mistral 7B | Mistral | Lightweight |
| DeepSeek Coder | DeepSeek | Good for code |
| Gemma 2 | Google | Fast |
| Qwen 2.5 | Alibaba | Strong coding |

```bash
opencode run "Explain this code" --model openrouter/meta-llama/llama-3-70b
```

## Combining Local and Cloud Models

The real power is using both together:

```jsonc
{
  "provider": {
    "primary": {
      "name": "openrouter",
      "model": "openai/gpt-4o"
    },
    "fallback": [
      { "name": "ollama", "model": "codellama" }
    ]
  }
}
```

Now OpenCode automatically falls back to your local model if the cloud provider is unavailable.

## Quick Model Selection

```text
┌─────────────────────────────────────────────────────┐
│                  SITUATION                          │
├──────────────────────┬──────────────────────────────┤
│  Internet Available  │  Offline / Private Code      │
│                      │                              │
│  ┌────────────────┐  │  ┌────────────────────────┐  │
│  │ OpenRouter     │  │  │ Ollama (Local)         │  │
│  │ • All models   │  │  │ • codellama            │  │
│  │ • Free tier    │  │  │ • deepseek-coder       │  │
│  │ • Pay-per-use  │  │  │ • llama3               │  │
│  └────────────────┘  │  │ • mistral              │  │
│                      │  └────────────────────────┘  │
└──────────────────────┴──────────────────────────────┘
```

## Summary

```text
Ollama:
  Install:  ollama.ai/download
  Pull:     ollama pull codellama
  Config:   provider.name = "ollama"
            provider.model = "codellama"

OpenRouter:
  Sign up:  openrouter.ai
  Config:   provider.name = "openrouter"
            provider.apiKey = "$OPENROUTER_API_KEY"

Best practice:
  Use OpenRouter for complex tasks (GPT-4o, Claude).
  Use Ollama for simple tasks and private code.
  Set Ollama as fallback for offline resilience.
```
