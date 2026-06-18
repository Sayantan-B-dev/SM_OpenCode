# Free AI Coding Models Walkthrough

You don't need to spend money to use AI coding assistance. OpenCode gives you access to several **free models**.

## Free Models Available via OpenCode Zen

| Model | Provider | Quality | Best For |
|-------|----------|---------|----------|
| `gpt-4o-mini` | OpenAI (via Zen) | ⭐⭐⭐⭐ | General coding, quick answers |
| `gemini-2.0-flash` | Google | ⭐⭐⭐⭐ | Large context, fast responses |
| `claude-3-haiku` | Anthropic (via Zen) | ⭐⭐⭐⭐ | Fast, concise code reviews |
| `llama-3.1-8b` | Meta / Ollama | ⭐⭐⭐ | Simple tasks, local/offline |
| `deepseek-coder` | DeepSeek | ⭐⭐⭐⭐ | Code-specific tasks |

## How to Access Free Models

### Method 1: OpenCode Zen (No Setup)

```bash
opencode
# → Select "OpenCode Zen"
# → Select "gpt-4o-mini" (free tier)
```

### Method 2: Local Models via Ollama (Fully Offline)

```bash
# Install Ollama from https://ollama.com
ollama pull llama3.1

# In opencode.json:
{
  "provider": "ollama",
  "model": "llama3.1"
}
```

### Method 3: Google Gemini Free Tier

```bash
# Get a free API key from Google AI Studio
export GEMINI_API_KEY="your-free-key"
```

Then select `gemini-2.0-flash` in OpenCode.

## Real Example: Using a Free Model

```text
> /provider zen
> /model gpt-4o-mini
> write a Python function to reverse a linked list

Certainly! Here's a simple implementation:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_list(head: ListNode) -> ListNode:
    prev = None
    curr = head
    while curr:
        next_temp = curr.next
        curr.next = prev
        prev = curr
        curr = next_temp
    return prev
```
```

## Pros & Cons of Free Models

| Pros | Cons |
|------|------|
| ✅ No cost | ❌ Rate-limited (slower) |
| ✅ No API key needed (Zen) | ❌ Smaller context windows |
| ✅ Good for learning & small tasks | ❌ Less reliable for complex code |
| ✅ Works offline (Ollama) | ❌ Requires good hardware (local) |

## Recommendation

Start with **OpenCode Zen → gpt-4o-mini** for everyday tasks. Switch to a paid model only when you need higher quality for production code.
