# Playwright MCP Server Setup

**Playwright MCP** bridges OpenCode with a real browser. The agent can **see** your app, **click** buttons, **fill** forms, and **verify** visual output — all autonomously.

## What is MCP?

**MCP (Model Context Protocol)** is a standard that lets AI models interact with external tools. Playwright MCP gives OpenCode browser automation powers.

## Why Do You Need It?

Without MCP, the agent can only:
- Read your code
- Run terminal commands
- Make assumptions about UI behavior

With Playwright MCP, the agent can:
- Open your app in a real browser
- Navigate pages
- Click, type, scroll
- Take screenshots
- Verify visual output

## Installation

### Step 1: Find the Boilerplate

Go to the Playwright MCP GitHub:

```
https://github.com/microsoft/playwright-mcp
```

Read the README. Find the section for **OpenCode configuration**.

### Step 2: Create `opencode.json` in Your Project Root

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp", "--headless"]
    }
  }
}
```

### Step 3: Verify the Connection

Inside OpenCode:

```text
> /mcps

Connected MCP Servers:
  playwright (running) — Browser automation
```

If `playwright` shows as **(running)**, the connection works!

### Step 4: Manage MCP Servers

```text
> /mcps
Space bar → toggle server on/off
```

If you need to connect an MCP server mid-session:

```text
> /exit
# Reconfigure opencode.json
> opencode
# MCP server connects on startup
```

## Real Example: Browser Automation

```text
> Use Playwright to go to http://localhost:3000 and test the login flow

Agent (via Playwright MCP):
  → Navigate to http://localhost:3000
  → Screenshot taken ✓
  → Click "Sign In" button
  → Fill email field: "test@example.com"
  → Fill password field: "password123"
  → Click "Submit"
  → Screenshot taken ✓
  → Verify we're on the dashboard page
  → Report: "Login flow works correctly"
```

## Configuration Options

| Option | Values | Description |
|--------|--------|-------------|
| `--headless` | (default) | Run browser without UI |
| `--headed` | Add flag | Show the browser window |
| `--port` | Number | Custom debug port |
| Channel | `chrome`, `msedge` | Specific browser |

Example with headed mode (watch the agent work):

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp", "--headed"]
    }
  }
}
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `command not found: npx` | Install Node.js / npm |
| Playwright not installed | MCP auto-installs it |
| Browser not found | Run `npx playwright install chromium` |
| Connection refused | Make sure your dev server is running first |
