# Cloud Alternatives: OpenRouter and Other Options

If your local machine is not powerful enough to run Gemma 4 (especially the 26B or 32B variants), or if you want to use it without installation, cloud alternatives are the answer. This section covers every cloud option for running Gemma 4 with OpenCode.

## Why Use Cloud Instead of Local?

| Situation | Best Choice |
|-----------|-------------|
| No GPU / low RAM | Cloud (OpenRouter) |
| Want to try before downloading | Cloud (OpenRouter) |
| Need the 32B variant without high-end hardware | Cloud (OpenRouter) |
| Occasional use only | Cloud (OpenRouter) |
| Privacy-sensitive code | Local (Ollama) |
| Frequent/heavy usage | Local (Ollama) - cheaper long-term |
| Offline work | Local (Ollama) |

## Option 1: OpenRouter (Recommended Cloud Option)

OpenRouter provides a unified API to access Gemma 4 and 100+ other models with a single API key.

### Why OpenRouter?

```text
✓ One API key for all models.
✓ Free tier available (rate-limited).
✓ No hardware requirements.
✓ Access the full 32B variant.
✓ Pay only for what you use.
✓ Works anywhere with internet.
```

### Step 1: Create an OpenRouter Account

```text
https://openrouter.ai
```

1. Sign up with email or GitHub.
2. Go to the API keys section.
3. Click "Create Key".
4. Copy your API key (starts with `sk-or-v1-`).

### Step 2: Find the Gemma 4 Models

OpenRouter hosts all four Gemma 4 variants:

| Model | OpenRouter ID | Free Tier |
|-------|---------------|:---------:|
| Gemma 4 E2B | `google/gemma-4-2b-it` | Yes |
| Gemma 4 E4B | `google/gemma-4-4b-it` | Yes |
| Gemma 4 26B | `google/gemma-4-26b-it` | Limited |
| Gemma 4 32B | `google/gemma-4-32b-it` | No (paid) |

### Step 3: Connect OpenCode to OpenRouter

#### Method A: Using /connect Command

```bash
# Inside OpenCode TUI
> /connect openrouter --api-key sk-or-v1-your-key-here
```

#### Method B: Using Environment Variable

```bash
# Windows PowerShell
$env:OPENROUTER_API_KEY = "sk-or-v1-your-key-here"

# macOS / Linux
export OPENROUTER_API_KEY="sk-or-v1-your-key-here"
```

#### Method C: Configure in opencode.json

```jsonc
{
  "provider": {
    "name": "openrouter",
    "apiKey": "$OPENROUTER_API_KEY",
    "baseUrl": "https://openrouter.ai/api/v1",
    "models": {
      "gemma-fast": {
        "model": "google/gemma-4-2b-it",
        "description": "Free tier, fast responses",
        "temperature": 0.3
      },
      "gemma-balanced": {
        "model": "google/gemma-4-4b-it",
        "description": "Free tier, balanced quality",
        "temperature": 0.3
      },
      "gemma-powerful": {
        "model": "google/gemma-4-26b-it",
        "description": "High quality, limited free tier",
        "temperature": 0.2
      },
      "gemma-max": {
        "model": "google/gemma-4-32b-it",
        "description": "Maximum capability, paid",
        "temperature": 0.2
      }
    }
  }
}
```

### Step 4: Use Gemma 4 via OpenRouter

```bash
# Quick test
opencode run "Hello, what model are you?" --model openrouter/google/gemma-4-4b-it

# Code generation
opencode run "Write a Python function to download a file from URL" --model openrouter/google/gemma-4-26b-it

# Complex task with the best model
opencode run "Design a microservices architecture for an e-commerce platform" --model openrouter/google/gemma-4-32b-it
```

### Step 5: Switching Models in Session

Inside OpenCode TUI:

```text
> /model gemma-fast
> /model gemma-balanced
> /model gemma-max
```

## Option 2: Google AI Studio (Gemini API)

If you prefer Google's official API:

```text
https://aistudio.google.com
```

```bash
# Get API key from Google AI Studio
# Then configure OpenCode
opencode init --provider google
$env:GOOGLE_API_KEY = "your-google-api-key"
```

**Note:** Google AI Studio provides access to Gemini models, not Gemma 4 directly. For Gemma 4 specifically, use OpenRouter.

## Option 3: Hugging Face Inference API

```text
https://huggingface.co/google/gemma-4
```

```bash
# Configure as custom OpenAI-compatible endpoint
opencode init --provider custom
```

Then configure:

```jsonc
{
  "provider": {
    "name": "custom",
    "model": "google/gemma-4-4b-it",
    "baseUrl": "https://api-inference.huggingface.co/models/",
    "apiKey": "$HF_API_KEY",
    "headers": {
      "Authorization": "Bearer $HF_API_KEY"
    }
  }
}
```

## Option 4: Your Own Cloud Server

If you have access to a cloud GPU, you can self-host Gemma 4:

```bash
# Deploy on a cloud VM (GCP, AWS, Azure, etc.)
# Install Ollama on the cloud VM
curl -fsSL https://ollama.ai/install.sh | sh
ollama pull gemma4:32b
ollama serve

# Then connect OpenCode to your remote Ollama
opencode run "Hello" --model ollama/gemma4:32b
```

Configure with the remote URL:

```jsonc
{
  "provider": {
    "name": "ollama",
    "model": "gemma4:32b",
    "baseUrl": "http://your-cloud-vm-ip:11434"
  }
}
```

## Hybrid Setup: Local + Cloud

The best of both worlds — use local for simple tasks, cloud for complex ones:

```jsonc
{
  "provider": {
    "primary": {
      "name": "ollama",
      "model": "gemma4:e4b",
      "description": "Local for fast, private tasks"
    },
    "fallback": [
      {
        "name": "openrouter",
        "model": "google/gemma-4-26b-it",
        "description": "Cloud for complex tasks when local is not enough"
      }
    ],
    "models": {
      "local-fast": {
        "name": "ollama",
        "model": "gemma4:e2b",
        "description": "Very fast local model"
      },
      "local-balanced": {
        "name": "ollama",
        "model": "gemma4:e4b",
        "description": "Daily driver"
      },
      "cloud-powerful": {
        "name": "openrouter",
        "model": "google/gemma-4-26b-it",
        "description": "For tough problems"
      },
      "cloud-max": {
        "name": "openrouter",
        "model": "google/gemma-4-32b-it",
        "description": "Maximum power"
      }
    }
  }
}
```

### Using the Hybrid Setup

```text
> /model local-fast     # Quick questions (free, instant)
> /model local-balanced  # Daily coding (free, good quality)
> /model cloud-powerful  # Complex refactoring (paid, high quality)
> /model cloud-max       # Hardest problems (paid, best quality)
```

## Cost Comparison

| Setup | Initial Cost | Running Cost | Best For |
|-------|:-----------:|:------------:|----------|
| Local Ollama E2B/E4B | Free (already have hardware) | $0 (electricity only) | Daily use |
| Local Ollama 26B | Free (already have hardware) | $0 | If you have the hardware |
| OpenRouter E2B/E4B (free tier) | $0 | $0 (rate limited) | Trying it out |
| OpenRouter 26B/32B (paid) | $0 | ~$0.50-2.00 per million tokens | Occasional complex tasks |
| Cloud VM + Ollama | Cloud VM cost | $0.05-0.50/hour + storage | Heavy production use |

## Rate Limits on Free Tier

OpenRouter's free tier has the following typical limits:

```text
Free models:     ~20 requests per minute, ~200 per day
Paid models:     No strict limits (pay per use)

If rate limited:
  - Wait a minute and try again.
  - Switch to a different free model temporarily.
  - Consider adding a small amount of credits.
```

## Complete Free Setup: OpenCode + OpenRouter + Gemma 4

This is the fastest way to get started with zero cost and zero installation:

```bash
# Step 1: Install OpenCode (if not already)
npm install -g @opencode/core

# Step 2: Set OpenRouter API key
$env:OPENROUTER_API_KEY = "sk-or-v1-your-key"

# Step 3: Start OpenCode with Gemma 4
opencode run "Hello, what model are you using?" --model openrouter/google/gemma-4-4b-it

# Step 4: Use it freely
opencode
```

Inside the TUI:

```text
> /model openrouter/google/gemma-4-4b-it
> Create a REST API with Express and TypeScript
```

## Conclusion: True Flexibility

```text
┌─────────────────────────────────────────────────────────┐
│              OpenCode + Gemma 4                         │
│                                                         │
│  Local (Ollama):                                        │
│    ├── E2B (2.6B) — Any machine, fast, free             │
│    ├── E4B (4B) — Most laptops, good quality, free      │
│    ├── 26B — Gaming PC, high quality, free               │
│    └── 32B — Workstation, best quality, free            │
│                                                         │
│  Cloud (OpenRouter):                                    │
│    ├── Free tier — No hardware needed, rate limited     │
│    └── Paid — Unlimited access, pay per use             │
│                                                         │
│  Hybrid:                                                │
│    ├── Local for daily tasks (free)                     │
│    └── Cloud for complex tasks (pay as needed)          │
│                                                         │
│  You are never locked in. Switch anytime.              │
└─────────────────────────────────────────────────────────┘
```

With OpenCode and Gemma 4, you have a truly free, open-source AI coding stack that works everywhere — no subscriptions, no vendor lock-in, no compromises.
