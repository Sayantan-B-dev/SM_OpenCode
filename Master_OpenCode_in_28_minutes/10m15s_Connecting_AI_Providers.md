# Connecting AI Providers (10:15 - 13:02)

## Connecting External AI Providers

OpenCode supports **75+ LLM providers** through Models.dev, giving you unprecedented flexibility in choosing and combining AI models.

### How to Connect a Provider

The simplest way is through the TUI:

```bash
# Start OpenCode in your project
opencode

# Then type in the TUI:
/connect
```

This opens an interactive menu where you can:
1. Search for your provider (e.g., "DeepSeek", "Perplexity", "OpenAI")
2. Choose authentication method (OAuth or API key)
3. Enter credentials when prompted
4. Select available models via `/models`

### Provider Categories

#### Major Cloud Providers
- **OpenAI** — GPT-4o, GPT-5, o1, o3 series (supports ChatGPT Plus/Pro login)
- **Anthropic** — Claude Opus 4.5, Sonnet 4.5, Haiku 4.5 (supports Claude Pro/Max)
- **Google** — Gemini 2.5 Pro, Gemini 2.5 Flash via Vertex AI
- **GitHub Copilot** — Use your Copilot subscription directly
- **GitLab Duo** — Premium/Ultimate subscription models

#### Chinese Providers
- **DeepSeek** — DeepSeek V4 Pro, DeepSeek Coder (extremely cost-effective)
- **Moonshot AI** — Kimi K2, Kimi K2 Instruct
- **MiniMax** — MiniMax M2.1, other models
- **01.AI** — Yi series models

#### Local & Open Source
- **Ollama** — Run local models (Llama, Qwen, Mistral, etc.)
- **LM Studio** — GUI for local model management
- **llama.cpp** — Lightweight local inference
- **Atomic Chat** — Desktop local model runner

#### Aggregators & Gateways
- **OpenRouter** — Access many models through one API
- **Cloudflare AI Gateway** — Unified billing across providers
- **Helicone** — LLM observability + gateway
- **LLM Gateway** — Multi-provider access

### Security: Don't Hardcode API Keys

**Never put API keys directly in your config files.** OpenCode supports several secure alternatives:

#### 1. Environment Variables
```bash
# Set before running opencode
export ANTHROPIC_API_KEY=sk-ant-xxx
export OPENAI_API_KEY=sk-xxx
opencode
```

#### 2. opencode.json with env references
```json
{
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  }
}
```

#### 3. Using /connect (recommended)
The `/connect` command stores credentials securely in:
```
~/.local/share/opencode/auth.json
```

#### 4. OAuth Authentication
Many providers support OAuth flow — OpenCode opens your browser, you authorize, and tokens are stored securely without ever typing a key.

### OpenCode Zen (Recommended for Beginners)

If you're new, start with **OpenCode Zen**:
- Curated list of tested, verified models
- Optimized for OpenCode's agent workflows
- Simple billing — one account, many models
- Run `/connect` → select "OpenCode Zen" → visit opencode.ai/auth
