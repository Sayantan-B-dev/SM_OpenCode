# Connecting Providers & OpenCode Zen

OpenCode supports **multiple AI providers**. The easiest way to start is with **OpenCode Zen** — the built-in free tier.

## What is OpenCode Zen?

OpenCode Zen is a **free, no-signup proxy** that gives you access to popular AI models without needing your own API keys. It's rate-limited but perfect for learning and small projects.

## Provider Options

| Provider | API Key Needed? | Cost | Best For |
|----------|----------------|------|----------|
| **OpenCode Zen** | ❌ No | Free | Beginners, quick tasks |
| **OpenAI** | ✅ Yes | Pay-per-token | High-quality code gen |
| **Anthropic (Claude)** | ✅ Yes | Pay-per-token | Complex reasoning |
| **Google Gemini** | ✅ Yes | Free tier available | Large context tasks |
| **Local Models (Ollama)** | ❌ No | Free (your hardware) | Privacy, offline |

## How to Connect a Provider

### Option 1: OpenCode Zen (Easiest)

Start OpenCode, select "OpenCode Zen" from the provider list, and pick a model. No setup required.

### Option 2: Bring Your Own Key

```bash
# Set your API key as an environment variable
export OPENAI_API_KEY="sk-..."

# Or create an opencode.json in your project root
```

Example `opencode.json`:

```json
{
  "provider": "openai",
  "model": "gpt-4o",
  "apiKey": "sk-..."
}
```

> **⚠️ Security Tip:** Never commit API keys to git. Use environment variables or `.env` files.

## Real Example: Switching Providers Mid-Session

```text
> /provider openai
> /model gpt-4o

Switched to OpenAI / gpt-4o

> /provider zen
> /model gpt-4o-mini

Switched to OpenCode Zen / gpt-4o-mini (free)
```

## Verifying Your Connection

```text
> /status

Provider: OpenCode Zen
Model: gpt-4o-mini
Rate Limit: 20 req/min
```

If you see your provider and model listed, you're connected!
