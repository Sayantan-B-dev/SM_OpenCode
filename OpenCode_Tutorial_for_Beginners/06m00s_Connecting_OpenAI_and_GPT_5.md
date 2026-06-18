# Connecting OpenAI & GPT-5

This guide walks through connecting your **OpenAI API key** to OpenCode and using **GPT-5** (or the latest OpenAI model).

## Step-by-Step: Connecting OpenAI

### Step 1: Get an API Key

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Click **"Create new secret key"**
3. Copy the key (starts with `sk-...`)

### Step 2: Configure in OpenCode

**Option A — Environment variable (recommended):**

```bash
# Windows (PowerShell)
$env:OPENAI_API_KEY="sk-..."

# macOS / Linux
export OPENAI_API_KEY="sk-..."
```

**Option B — opencode.json (project-level):**

```json
{
  "provider": "openai",
  "model": "gpt-4o",
  "apiKey": "sk-..."
}
```

> ⚠️ **Never commit** `opencode.json` to git if it contains your API key. Add it to `.gitignore`.

### Step 3: Select the Model

Inside OpenCode:

```text
> /provider openai
> /model gpt-4o
```

Or set a default in `opencode.json`:

```json
{
  "provider": "openai",
  "model": "gpt-4o"
}
```

## Using `/variants` to Compare Models

The `/variants` command shows all available models from your current provider:

```text
> /provider openai
> /variants

Available models:
  gpt-4o          (recommended)
  gpt-4o-mini     (fast, cheap)
  gpt-4-turbo     (legacy)
  o1              (reasoning)
  o3-mini         (light reasoning)
  gpt-4.1         (latest) ← GPT-5 equivalent
```

Switch between them instantly:

```text
> /model gpt-4.1
Switched to gpt-4.1

> /model gpt-4o-mini
Switched to gpt-4o-mini (saves tokens for simple tasks)
```

## Real Example: Using GPT-5 for Complex Reasoning

```text
> /model gpt-4.1
> Write a complete authentication system with Next.js App Router,
  including middleware, JWT handling, and protected routes.
```

GPT-5 (gpt-4.1) will handle complex multi-file changes with better reasoning than smaller models.

## Cost Awareness

| Model | Cost (Input) | Cost (Output) | Best For |
|-------|-------------|--------------|----------|
| `gpt-4o-mini` | ~$0.15/M tokens | ~$0.60/M tokens | Quick edits, simple tasks |
| `gpt-4o` | ~$2.50/M tokens | ~$10/M tokens | General development |
| `gpt-4.1` (GPT-5) | ~$2.50/M tokens | ~$10/M tokens | Complex reasoning, architecture |
| `o1` | ~$15/M tokens | ~$60/M tokens | Hardest problems |

**Tip:** Use `gpt-4o-mini` for routine tasks and switch to `gpt-4.1` only when you need deeper reasoning.
