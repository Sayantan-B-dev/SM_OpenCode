# Models

Models are the AI brains behind OpenCode. This section covers how to configure, switch, and optimize models for different tasks.

## Ways to Set Models

There are three ways to specify which model OpenCode uses:

### 1. Command Line Flag

Override the model for a single command:

```bash
opencode run "Explain this code" --model openai/gpt-4o
opencode run "Quick format" --model openai/gpt-4o-mini
opencode run "Refactor this" --model anthropic/claude-3.5-sonnet
opencode run "Local task" --model ollama/codellama
```

**Format:** `provider/model-name`

### 2. Config File (opencode.jsonc)

Set defaults in your project or global config:

```jsonc
{
  "provider": {
    "name": "openai",
    "model": "gpt-4o",
    "temperature": 0.2,
    "maxTokens": 4096
  }
}
```

### 3. In-Session Switch

Change models during an active session:

```text
> /models
Available models:
- gpt-4o (default)
- gpt-4-turbo
- gpt-4o-mini
- o1-preview

> /model gpt-4-turbo
[System] Switched to: gpt-4-turbo
```

## Model Variants

Many providers offer variants of the same base model with different characteristics:

```jsonc
{
  "provider": {
    "name": "openai",
    "model": "gpt-4o",
    "variants": {
      "thinking": {
        "temperature": 0.1,
        "maxTokens": 8192,
        "description": "For complex reasoning tasks"
      },
      "creative": {
        "temperature": 0.8,
        "description": "For brainstorming and creative writing"
      },
      "fast": {
        "model": "gpt-4o-mini",
        "temperature": 0.3,
        "description": "For quick, simple tasks"
      },
      "debug": {
        "temperature": 0.0,
        "maxTokens": 2048,
        "description": "For debugging — deterministic output"
      },
      "minimal": {
        "temperature": 0.2,
        "maxTokens": 512,
        "description": "For short, concise answers"
      },
      "medium": {
        "temperature": 0.4,
        "maxTokens": 2048,
        "description": "Balanced default"
      },
      "high": {
        "model": "claude-3.5-sonnet",
        "temperature": 0.3,
        "maxTokens": 8192,
        "description": "Highest quality, higher cost"
      }
    }
  }
}
```

### Using Variants

Switch between variants easily:

```text
> /model thinking
> /model fast
> /model creative
```

## Model Selection Guide

| Task Type | Recommended Model | Why |
|-----------|-------------------|-----|
| Quick Q&A | gpt-4o-mini | Cheap, fast |
| Code Generation | gpt-4o or claude-3.5-sonnet | High quality |
| Debugging | gpt-4o (temp 0.0) | Deterministic |
| Brainstorming | claude-3.5-sonnet (temp 0.8) | Creative |
| Code Review | gpt-4o | Thorough |
| Refactoring | claude-3.5-sonnet | Great at complex changes |
| Documentation | gpt-4o-mini | Good enough, cheaper |
| Local/Offline | ollama/codellama | Free, private |

## Provider-Specific Model Naming

Different providers use different naming conventions:

| Provider | Example Model Names |
|----------|---------------------|
| OpenAI | gpt-4o, gpt-4-turbo, gpt-4o-mini, o1-preview, o1-mini |
| Anthropic | claude-3.5-sonnet, claude-3-opus, claude-3-haiku |
| Google | gemini-1.5-pro, gemini-1.5-flash |
| Mistral | mistral-large, mistral-medium, open-mistral-7b |
| Groq | llama3-70b, llama3-8b, mixtral-8x7b |
| Ollama | codellama, llama3, mistral, deepseek-coder |
| OpenRouter | openai/gpt-4o, anthropic/claude-3.5-sonnet, google/gemini-pro |

## Strict Model Enforcement

For teams and organizations, you can enforce which models are allowed:

```jsonc
{
  "provider": {
    "allowedModels": [
      "openai/gpt-4o",
      "openai/gpt-4o-mini",
      "anthropic/claude-3.5-sonnet"
    ],
    "blockedModels": [
      "openai/gpt-3.5-turbo",
      "ollama/*"
    ]
  }
}
```

This ensures:
- Team members only use approved models.
- Budget controls are enforced.
- Security/compliance requirements are met.

## Pro Tips

```text
1. Use cheap models for simple tasks (formatting, linting, short answers).
2. Use expensive models for complex tasks (refactoring, debugging, architecture).
3. Use temperature 0.0 for debugging (deterministic output).
4. Use temperature 0.7-0.9 for creative tasks.
5. Set model variants in config for quick switching.
6. Use /models to see what is available before starting a task.
```

## Quick Reference

```bash
# Switch model for one command
opencode run "..." --model provider/model

# Switch model in session
> /model provider/model

# Use a variant
> /model fast

# List available
> /models
```
