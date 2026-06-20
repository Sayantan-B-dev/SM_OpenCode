# Setup Environment: Free Go and Zen Setup

OpenCode provides two free options to get started without paying anything: **OpenCode Go** and **OpenCode Zen**. This section explains both and how to set them up.

## What Are OpenCode Go and Zen?

OpenCode offers two curated provider services:

| Service | Description | Cost |
|---------|-------------|------|
| **OpenCode Go** | Basic free tier for getting started | Free (limited credits) |
| **OpenCode Zen** | Enhanced tier with more models and features | Free tier + paid plans |

## Setting Up OpenCode Go

### Step 1: Find Available Models

Visit the official documentation to see all available Go models:

```text
https://opencode.ai/docs/go/
```

This page lists:
- Available free models.
- Rate limits and quotas.
- Supported capabilities per model.

### Step 2: Authentication

```text
https://opencode.ai/auth
```

1. Go to the auth page.
2. Create an account or sign in.
3. Navigate to the API keys section.
4. Generate a new API key.

### Step 3: Enable Billing (for Zen)

For Zen, you may need to enable billing even for the free tier:

1. Go to opencode.ai/auth.
2. Select Zen as your provider.
3. Enable billing and add payment method (some free tiers still require card).
4. Copy your API key.

### Step 4: Configure in Terminal

Use `/connect` to set up the provider:

```bash
# Connect to Go
/connect go --api-key your-go-api-key

# Connect to Zen
/connect zen --api-key your-zen-api-key
```

Or set during initialization:

```bash
opencode init --provider zen
```

### Step 5: Select a Model

```bash
# List available models
/models

# Select a model
/model zen/gpt-4o-mini
```

## Difference Between Go and Zen

| Feature | Go | Zen |
|---------|-----|-----|
| Pricing | Free | Free tier + paid plans |
| Model Selection | Limited | Extensive |
| Rate Limits | Lower | Higher |
| Speed | Slower | Faster |
| Data Collection | Collects data for training | Opt-in data sharing |
| Support | Community | Priority support |
| Best For | Learning, experimentation | Production use |

## Important: Data Privacy Warning

> **Warning:** Free models (especially Go) may collect and store your prompts and code for model training. Do not submit sensitive, proprietary, or personal data when using free tiers.

```text
Safe practice:
  - Use free tiers for learning and experimenting.
  - Use paid tiers (Zen, OpenAI, Anthropic) for production code.
  - Use local models (Ollama) for sensitive/proprietary code.
```

## Quick Setup Checklist

```text
[ ] Go to https://opencode.ai/auth
[ ] Create an account
[ ] Generate an API key
[ ] Run: opencode init --provider zen
[ ] Run: /models to verify connection
[ ] Start coding!
```

## Next Steps

Once your free provider is set up, the next section covers models in detail — how to switch between them and configure them for different tasks.
