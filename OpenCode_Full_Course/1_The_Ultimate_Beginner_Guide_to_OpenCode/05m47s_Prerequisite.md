# Prerequisites

Before using OpenCode, there are a few things you should know. Do not worry — none of these require expert-level knowledge. Basic familiarity is enough.

## What You Need to Know

### 1. JSON Structure

OpenCode uses JSON files for configuration (`opencode.json`, `opencode.jsonc`). You need to understand:

```json
{
  "key": "value",
  "nested": {
    "property": true
  },
  "array": ["item1", "item2"]
}
```

**Key concepts:**
- Objects: `{ }` with key-value pairs.
- Arrays: `[ ]` with ordered items.
- Strings, numbers, booleans, null.
- JSONC (JSON with Comments) allows `// comments` and trailing commas.

### 2. JavaScript / TypeScript Basics

OpenCode excels at working with JS/TS projects. Know:

- Variables, functions, and objects.
- Async/await and promises.
- ES modules (import/export).
- Basic TypeScript types (optional but helpful).

```javascript
// Example you might ask OpenCode to write
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}
```

### 3. Components and State

If working with frontend frameworks (React, Vue, Svelte):

- What a component is.
- What state means.
- Props vs state.
- Lifecycle concepts.

### 4. React, Node.js, npm

| Technology | Why You Need It |
|------------|-----------------|
| React | Common target for AI-generated code |
| Node.js | Runtime for OpenCode itself |
| npm | Installing OpenCode and packages |

**Check your setup:**
```bash
node --version    # Should be 18+
npm --version     # Should be 8+
```

### 5. Visual Studio Code

While OpenCode runs in the terminal, VS Code is commonly used alongside it. You should:

- Know how to open a terminal in VS Code (`Ctrl + ``).
- Understand the VS Code file explorer.
- Be comfortable editing files.

### 6. Configure AI Models

You will need API keys from at least one provider:

| Provider | Sign Up | Cost |
|----------|---------|------|
| Zen (OpenCode) | opencode.ai/auth | Free tier available |
| OpenAI | platform.openai.com | Pay-per-use |
| Anthropic | console.anthropic.com | Pay-per-use |
| Google AI | makersuite.google.com | Free tier |
| Ollama | ollama.ai (download) | Free (local) |
| OpenRouter | openrouter.ai | Free tier + pay |

**You will need to:**
- Create an account.
- Generate an API key.
- Store it securely (env variables recommended).

### 7. Git Basics

OpenCode integrates with Git. Know:

```bash
git init          # Create a repo
git add .         # Stage files
git commit -m "msg"  # Commit changes
git push          # Push to remote
git pull          # Pull from remote
git status        # Check status
git diff          # View changes
```

### 8. Debugging

Basic debugging skills help you verify OpenCode's work:

- Reading error messages in the terminal.
- Using `console.log` or `console.error`.
- Checking network requests.
- Understanding stack traces.

## What You Do NOT Need

- **You do not need** to be an AI/ML expert.
- **You do not need** to know every OpenCode feature upfront.
- **You do not need** to have used Copilot or Claude Code before.
- **You do not need** to be a terminal expert — basic commands are enough.

## Quick Self-Check

Run these commands in your terminal:

```bash
# Check Node.js
node --version && npm --version

# Check Git
git --version

# Check if you have a terminal
echo "Ready to go!"
```

If all three work, you are ready for the next section.
