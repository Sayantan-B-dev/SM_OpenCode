# Comparing Multiple Models

One of OpenCode's superpowers is the ability to compare different AI models side by side. This lets you choose the best model for each task and optimize for quality, speed, and cost.

## Why Compare Models?

Different models excel at different things:

| Model | Strength | Weakness | Cost |
|-------|----------|----------|------|
| GPT-4o | Balanced, fast, good at everything | Not the cheapest | Medium |
| Claude 3.5 Sonnet | Excellent at complex code, great reasoning | Slower, costlier | High |
| GPT-4o-mini | Fast, cheap, good for simple tasks | Less capable at complex reasoning | Low |
| Gemini 1.5 Pro | Large context window, good at structured tasks | Inconsistent on very complex code | Medium |
| Llama 3 (local) | Free, private, always available | Less capable than paid models | Free |
| DeepSeek Coder | Specialized for code, good quality | Smaller ecosystem | Low |

## Method 1: Sequential Comparison

Run the same prompt against different models and compare results:

```bash
# Same prompt, different models
opencode run "Write a function to merge two sorted arrays" --model openai/gpt-4o
opencode run "Write a function to merge two sorted arrays" --model anthropic/claude-3.5-sonnet
opencode run "Write a function to merge two sorted arrays" --model openai/gpt-4o-mini
opencode run "Write a function to merge two sorted arrays" --model ollama/codellama
```

### What to Compare

| Criterion | What to Look For |
|-----------|------------------|
| **Correctness** | Does the code compile and run? |
| **Quality** | Is the code idiomatic and well-structured? |
| **Completeness** | Does it handle edge cases? |
| **Style** | Does it follow your project conventions? |
| **Speed** | How fast does the response come? |
| **Cost** | How many tokens were used? |

## Method 2: In-Session Switching

Switch models during an active session to get different perspectives:

```text
# Start with a powerful model for planning
> /model openai/gpt-4o

> Plan a real-time chat application with WebSockets

# Switch to a cheaper model for implementation
> /model openai/gpt-4o-mini

> Implement the basic WebSocket server

# Switch back for complex parts
> /model anthropic/claude-3.5-sonnet

> Add authentication and message persistence
```

## Method 3: Config-Based Multi-Model Setup

Define multiple model configurations and switch between them:

```jsonc
{
  "provider": {
    "models": {
      "powerful": {
        "name": "anthropic",
        "model": "claude-3.5-sonnet",
        "temperature": 0.2,
        "maxTokens": 8192
      },
      "balanced": {
        "name": "openai",
        "model": "gpt-4o",
        "temperature": 0.3,
        "maxTokens": 4096
      },
      "cheap": {
        "name": "openai",
        "model": "gpt-4o-mini",
        "temperature": 0.3,
        "maxTokens": 2048
      },
      "local": {
        "name": "ollama",
        "model": "codellama",
        "temperature": 0.3,
        "maxTokens": 2048
      }
    }
  }
}
```

Then switch with simple names:

```text
> /model powerful
> /model cheap
> /model local
```

## Method 4: Task-Based Model Selection

Create a mental framework for choosing the right model:

```text
┌────────────────────────────────────────────────────┐
│                  TASK COMPLEXITY                    │
│                                                     │
│   Simple (gpt-4o-mini)   Medium (gpt-4o)  Complex  │
│   ┌─────────────────┐  ┌──────────────┐  (Claude)  │
│   │  • Formatting   │  │  • Refactoring│  ┌──────┐ │
│   │  • Quick Q&A    │  │  • Adding     │  │• Arch│ │
│   │  • Typo fixes   │  │    features   │  │• Sec │ │
│   │  • Simple grep  │  │  • Writing    │  │• Mig │ │
│   └─────────────────┘  │    tests      │  │• Opt │ │
│                         └──────────────┘  └──────┘ │
└────────────────────────────────────────────────────┘
```

### Decision Matrix

| Task Type | Recommended Model | Rationale |
|-----------|-------------------|-----------|
| Fix a typo | GPT-4o-mini | Cheap, fast, good enough |
| Refactor a class | Claude 3.5 Sonnet | Best at complex changes |
| Write unit tests | GPT-4o | Good balance |
| Design architecture | Claude 3.5 Sonnet | Superior reasoning |
| Format code | GPT-4o-mini | Trivial task |
| Debug a crash | GPT-4o (temp 0.0) | Deterministic |
| Generate boilerplate | GPT-4o-mini | Repetitive, low complexity |
| Security audit | Claude 3.5 Sonnet | Thorough analysis |
| Quick search/grep | Any model | Tool-dependent, not model-dependent |

## Method 5: Side-by-Side with Custom Commands

Create a custom command that runs the same prompt on multiple models:

```jsonc
{
  "customCommands": [
    {
      "name": "compare",
      "prompt": "Run the following request on multiple models and compare the results:\n\n$ARGUMENTS\n\nFirst run on gpt-4o, then on claude-3.5-sonnet. Compare the approaches and suggest which is better.",
      "description": "Compare responses from different models"
    }
  ]
}
```

Usage:

```text
> /compare Write a rate limiter middleware for Express
```

## Cost Comparison Table

| Model | Cost per 1M Input Tokens | Cost per 1M Output Tokens | Relative Cost |
|-------|-------------------------|--------------------------|---------------|
| GPT-4o-mini | $0.15 | $0.60 | Very Low |
| GPT-4o | $2.50 | $10.00 | Medium |
| Claude 3.5 Sonnet | $3.00 | $15.00 | High |
| DeepSeek Coder | $0.14 | $0.28 | Very Low |
| Gemini 1.5 Pro | $1.25 | $5.00 | Medium |
| Local (Ollama) | $0.00 | $0.00 | Free |
| OpenRouter (varies) | Depends on model | Depends on model | Low-Medium |

## Practical Comparison Workflow

```text
1. Start with your default model (gpt-4o).
2. If the task is simple, switch to gpt-4o-mini.
3. If the model struggles, switch to claude-3.5-sonnet.
4. If working offline, use Ollama.
5. Track which models perform best for which tasks.
6. Create model aliases in config for quick switching.
```

## Pro Tips

```text
1. Do not use the most expensive model for every task.
2. Use temperature 0.0 for debugging (deterministic output).
3. Use temperature 0.7+ for brainstorming and creative tasks.
4. Local models are great for private code but less capable.
5. OpenRouter gives you access to many models with one API key.
6. Switch models mid-session when you hit a complexity wall.
7. Create model aliases in opencode.jsonc for quick access.
```
