# Subscriptions vs API Keys — Which Should You Use?

OpenCode supports two ways to access paid AI models: **subscriptions** and **API keys**. Here's what you need to know.

## API Keys (Pay-as-You-Go)

You get an API key from the provider (e.g., OpenAI, Anthropic) and configure it in OpenCode.

```json
{
  "provider": "openai",
  "model": "gpt-4o",
  "apiKey": "sk-..."
}
```

| Pros | Cons |
|------|------|
| ✅ Pay only for what you use | ❌ Can get expensive with heavy use |
| ✅ Full control over model choice | ❌ Need to manage keys securely |
| ✅ No monthly commitment | ❌ Rate limits per key |
| ✅ Cheaper for light/medium use | ❌ Billing can be unpredictable |

## Subscription (ChatGPT Plus / Claude Pro)

You subscribe to the provider's service and use the included model access.

| Pros | Cons |
|------|------|
| ✅ Fixed monthly cost | ❌ More expensive for light users |
| ✅ No surprise bills | ❌ Limited to the subscription model |
| ✅ Usually includes web UI access | ❌ Can't use the subscription API key directly |

## Recommendation: Why Free First?

**Start with free options before paying.** Here's why:

| Scenario | Recommended | Why |
|----------|------------|-----|
| Learning / experimenting | OpenCode Zen (free) | No cost, no setup |
| Small personal projects | Free models + API key | gpt-4o-mini is fast and cheap |
| Production / team use | API key (gpt-4o, Claude) | Best quality, pay-per-use |
| Heavy daily use (>1M tokens/day) | OpenAI API key with tier | Cheaper than subscription at scale |

## Real Example: Cost Comparison

```text
Assumptions: 500K tokens/month (code generation + review)

Option 1: ChatGPT Plus ($20/month)
  → Tokens: unlimited (within fair use)
  → Model: gpt-4o
  → Extra: Web UI access

Option 2: OpenAI API Key
  → gpt-4o-mini: ~$0.15/1M input → ~$0.08/month
  → gpt-4o: ~$2.50/1M input → ~$1.25/month
  → Total: ~$1.33/month (way cheaper!)

Option 3: OpenCode Zen
  → $0/month (rate-limited but functional)
```

## Verdict

| Your Use Case | Best Choice |
|---------------|-------------|
| "I'm just learning" | OpenCode Zen (free) |
| "I code a few hours a week" | Free models + API key for heavy tasks |
| "I use AI all day, every day" | API key (gpt-4o or Claude) |
| "I want zero setup, unlimited use" | ChatGPT Plus + use OpenCode with API key |
