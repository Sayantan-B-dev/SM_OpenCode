# OpenCode Crash Course: The Open Source Alternative to Codex and Claude Code

## Table of Contents

1. [What is OpenCode?](#1-what-is-opencode)
2. [Core Architecture & Inner Workings](#2-core-architecture--inner-workings)
3. [Key Characteristics](#3-key-characteristics)
4. [Installation Methods](#4-installation-methods)
5. [Configuration & Provider Setup](#5-configuration--provider-setup)
6. [Using the TUI (Terminal User Interface)](#6-using-the-tui-terminal-user-interface)
7. [Generating agents.md](#7-generating-agentsmd)
8. [Agent Modes: Build and Plan](#8-agent-modes-build-and-plan)
9. [Core Built-in Tools for Daily Usage](#9-core-built-in-tools-for-daily-usage)
10. [Everyday Commands](#10-everyday-commands)
11. [Subagents: @general, @explore, @scout](#11-subagents-general-explore-scout)
12. [opencode.json: Complete Configuration Reference](#12-opencodejson-complete-configuration-reference)
13. [Configuration Priority Chain](#13-configuration-priority-chain)
14. [Agent Types in Depth](#14-agent-types-in-depth)
15. [Creating Custom Agents](#15-creating-custom-agents)
16. [Custom Commands](#16-custom-commands)
17. [Rules & Instructions from agents.md](#17-rules--instructions-from-agentsmd)
18. [MCP (Model Context Protocol)](#18-mcp-model-context-protocol)
19. [Multi-Interface: CLI, TUI, Web, API](#19-multi-interface-cli-tui-web-api)
20. [Web Interface](#20-web-interface)
21. [GitHub Integration & CI/CD](#21-github-integration--cicd)
22. [AI SDK / OpenAI-Compatible](#22-ai-sdk--openai-compatible)
23. [Pro Tips & Best Practices](#23-pro-tips--best-practices)

---

## 1. What is OpenCode?

OpenCode is an **open-source, provider-agnostic AI coding assistant** built as a direct alternative to GitHub Copilot's Codex and Anthropic's Claude Code. It is designed to help developers write, refactor, debug, and understand code directly from the terminal using natural language commands.

### What It Does

- **Code Generation & Editing**: Generates new code, modifies existing files, applies patches, and refactors entire codebases based on natural language instructions.
- **Code Understanding**: Explains complex code, answers questions about codebases, and provides architectural insights.
- **Search & Navigation**: Searches through files, greps for patterns, and navigates large codebases quickly.
- **Task Automation**: Automates repetitive development tasks like running tests, generating components, setting up CI/CD, and more.
- **Multi-Provider AI Access**: Connects to any AI provider (OpenAI, Anthropic, Google, Mistral, Ollama, Zen, etc.) and lets you switch between them freely.
- **Agent-Based Workflows**: Uses specialized agents (general, explore, scout) for different types of tasks, from broad exploration to focused implementation.

### How It Does It

OpenCode operates through an **agent-based architecture** where each agent has access to a set of built-in tools (file operations, search, web, etc.). When you issue a command:

1. The **orchestrator** receives your request and determines which agent(s) to invoke.
2. The **agent** interprets the request, breaks it down into steps, and calls the appropriate tools.
3. Tools execute operations (reading/writing files, searching, running bash commands, fetching web content).
4. Results flow back to the agent, which refines its approach or presents the outcome to you.

The entire loop is transparent, undoable, and logged, giving you full control over every action taken.

---

## 2. Core Architecture & Inner Workings

### High-Level Architecture

```
User Input (CLI/TUI/API)
        |
        v
  Orchestrator Layer
        |
        v
  Session Manager (tracks history, undo/redo)
        |
        v
  Agent Router (routes to appropriate agent)
        |
        +--- General Agent (general-purpose tasks)
        +--- Explore Agent (codebase exploration)
        +--- Scout Agent (targeted searches)
        +--- Custom Agents (user-defined)
        |
        v
  Tool Execution Layer
        |
        +--- File Tools (read, write, edit, apply patch)
        +--- Search Tools (grep, glob, LSP)
        +--- Execution Tools (bash, webfetch, websearch)
        +--- Agent Tools (todowrite, question, skill)
        +--- MCP Tools (external MCP servers)
        |
        v
  LLM Provider Layer (OpenAI, Anthropic, Zen, Ollama, etc.)
```

### Session Management

Every OpenCode session maintains a **conversation history** with full undo/redo capability. Each action taken by an agent is recorded, and you can step backward or forward through the action log. This is critical for safety when AI agents are making changes to your filesystem.

### Agent Loop

The core loop works as follows:

1. **Prompt**: User or system provides a prompt.
2. **Think**: The agent analyzes the prompt and determines what tools to call.
3. **Act**: The agent calls one or more tools with specific parameters.
4. **Observe**: Results from tools are returned to the agent.
5. **Repeat**: The agent continues the loop until the task is complete or user intervention occurs.
6. **Present**: Final results are shown to the user.

### Inner Workings: Key Components

- **Tool Registry**: A central registry of all available tools with their schemas, descriptions, and permission rules. Each tool defines its input/output contract using TypeScript interfaces.
- **Provider Adapter**: An abstraction layer that normalizes requests/responses across different LLM providers. It handles token counting, retry logic, streaming, and error mapping.
- **Config Manager**: Loads and merges configuration from multiple sources (managed, project, custom, global, remote) with defined priority rules.
- **File Watcher**: Monitors project files for changes and updates the agent's context accordingly.
- **Permission System**: Controls which tools can be invoked, with what arguments, and in which contexts. Admins can enforce rules through managed config.

### Data Flow (Example: "Explain how closures work in JS")

```
User: opencode run "Explain how closures work in JS"
  |
  v
Orchestrator receives request
  |
  v
Session created, history initialized
  |
  v
Request sent to LLM via configured provider
  |
  v
LLM generates explanation (no tools needed for pure Q&A)
  |
  v
Response streamed back to user
```

### Data Flow (Example: "Refactor this function to use async/await")

```
User: opencode run "Refactor this function to use async/await"
  |
  v
Orchestrator routes to General Agent
  |
  v
Agent decides: need to read the file first
  |
  v
Tool: read("src/functions.js") -> returns file content
  |
  v
Agent analyzes content, formulates refactored version
  |
  v
Tool: edit("src/functions.js", oldCode, newCode)
  |
  v
File modified, result shown to user
  |
  v
User can undo with /undo if needed
```

---

## 3. Key Characteristics

### 3.1 Open Source

OpenCode is fully open source under the Apache 2.0 license. This means:
- You can inspect every line of code for security and correctness.
- You can fork, modify, and distribute your own versions.
- The community contributes plugins, tools, and improvements.
- No vendor lock-in: you own your workflow entirely.

### 3.2 Provider Agnostic

Unlike Copilot (locked to OpenAI) or Claude Code (locked to Anthropic), OpenCode supports **any LLM provider**:

| Provider | Type | Notes |
|----------|------|-------|
| OpenAI | Remote | GPT-4o, GPT-4, GPT-3.5 |
| Anthropic | Remote | Claude 3.5 Sonnet, Claude 3 Opus |
| Google | Remote | Gemini 1.5 Pro, Gemini Ultra |
| Mistral | Remote | Mistral Large, Mistral Medium |
| Groq | Remote | Fast inference on open models |
| Zen | Remote | OpenCode's curated provider |
| Ollama | Local | Llama 3, CodeLlama, Mistral (local) |
| Azure OpenAI | Remote | Enterprise OpenAI via Azure |
| Any OpenAI-compatible | Remote | vLLM, Together, Fireworks, etc. |

You can switch providers on the fly per command, or set defaults in your configuration.

### 3.3 Multi-Interface

OpenCode offers four interfaces to suit different workflows:

| Interface | Command | Use Case |
|-----------|---------|----------|
| CLI (one-shot) | `opencode run "..."` | Quick questions, CI/CD, scripting |
| TUI (interactive) | `opencode` (no args) | Extended coding sessions |
| Web UI | `opencode web` | Visual interface, sharing |
| API | Programmatic | Custom tooling, integrations |

### 3.4 Agent-Based Architecture

OpenCode uses specialized agents instead of a single monolithic AI:

| Agent | Purpose |
|-------|---------|
| **General** | Broad tasks: coding, debugging, Q&A, refactoring |
| **Explore** | Codebase exploration, finding relevant files |
| **Scout** | Focused, targeted searches (faster than Explore) |
| **Build** | Implementation mode - writes code, creates files |
| **Plan** | Planning mode - analyzes, researches, plans before coding |

You can invoke subagents inline with `@general`, `@explore`, or `@scout` followed by your query.

### 3.5 Undo/Redo

Every action in OpenCode is undoable. The session tracks:
- File modifications (before and after states)
- Tool invocations with all parameters
- Agent decisions and reasoning

Commands: `/undo` to revert the last action, `/redo` to reapply it.

### 3.6 Model Context Protocol (MCP)

MCP is an open standard for connecting AI agents with external tools and data sources. OpenCode fully supports MCP, allowing you to connect to:
- Databases (PostgreSQL, SQLite, MySQL)
- APIs (Notion, Jira, GitHub, Slack)
- File systems (cloud storage, remote servers)
- Custom services (internal tools, proprietary data)

Configure MCP servers in `opencode.json` under the `mcpServers` key.

### 3.7 Plugin, Tools, Agents, Commands

OpenCode's extensibility model has four layers:

| Layer | Description | Example |
|-------|-------------|---------|
| **Plugins** | Extend OpenCode's core capabilities | Custom provider adapters |
| **Tools** | Functions agents can call | `read`, `edit`, `bash`, `grep` |
| **Agents** | Specialized AI configurations | Custom agents with tailored instructions |
| **Commands** | User-defined shortcuts | `test`, `component`, `deploy` |

---

## 4. Installation Methods

OpenCode can be installed via multiple package managers. Choose the method that best fits your environment:

### 4.1 Using NPM (Node.js)

```bash
# Install globally
npm install -g @opencode/core

# Verify installation
opencode --version
```

### 4.2 Using Curl (Cross-Platform)

```bash
# Linux/macOS
curl -fsSL https://opencode.ai/install.sh | sh

# Windows (PowerShell)
iwr -useb https://opencode.ai/install.ps1 | iex
```

### 4.3 Using Homebrew (macOS/Linux)

```bash
brew install opencode/tap/opencode
```

### 4.4 Using Chocolatey (Windows)

```powershell
choco install opencode
```

### 4.5 Using NVM (Node Version Manager)

```bash
# First ensure you have Node.js 18+
nvm install 20
nvm use 20

# Then install OpenCode
npm install -g @opencode/core
```

### 4.6 Using Docker

```bash
# Pull the official image
docker pull opencode/opencode

# Run interactively (mount current directory)
docker run -it --rm \
  -v "$(pwd):/workspace" \
  -v "$HOME/.config/opencode:/root/.config/opencode" \
  opencode/opencode
```

### 4.7 From Source

```bash
git clone https://github.com/anomalyco/opencode.git
cd opencode
npm install
npm run build
npm link
```

### 4.8 Verify Installation

```bash
# Check version
opencode --version

# Check available providers
opencode --help

# Quick test
opencode run "Hello, what can you do?"
```

---

## 5. Configuration & Provider Setup

### 5.1 Initial Setup

On first run, OpenCode will prompt you to configure a provider. You can also run the setup wizard:

```bash
opencode init
```

This walks you through:
1. Selecting a provider (Zen, OpenAI, Anthropic, etc.)
2. Entering your API key
3. Setting default model parameters
4. Configuring project-level settings

### 5.2 Provider Configuration

#### Option A: Using Zen (Recommended for Beginners)

Zen is OpenCode's curated provider. It offers:
- Free tier with limited credits
- No API key required for basic usage
- Optimized for coding tasks

```bash
opencode init --provider zen
```

#### Option B: Using OpenAI

```bash
opencode init --provider openai
# Set your API key when prompted
```

Or set environment variables:

```bash
# Windows PowerShell
$env:OPENAI_API_KEY = "sk-..."

# Linux/macOS
export OPENAI_API_KEY="sk-..."
```

#### Option C: Using Anthropic Claude

```bash
opencode init --provider anthropic
$env:ANTHROPIC_API_KEY = "sk-ant-..."
```

#### Option D: Using Ollama (Local)

```bash
# Start Ollama with a model
ollama pull codellama
ollama run codellama

# Configure OpenCode to use it
opencode init --provider ollama --model codellama
```

### 5.3 Global Config File

The global configuration lives at:

- **Windows**: `%USERPROFILE%\.config\opencode\opencode.json`
- **Linux/macOS**: `~/.config/opencode/opencode.json`

Example global config:

```json
{
  "provider": {
    "name": "openai",
    "model": "gpt-4o",
    "temperature": 0.2,
    "maxTokens": 4096
  },
  "theme": "dark",
  "undoLimit": 100,
  "autoApply": false
}
```

### 5.4 Switching Providers at Runtime

You can override the provider for a single command:

```bash
opencode run "Refactor this file" --model openai/gpt-4o
opencode run "Debug this error" --model anthropic/claude-3.5-sonnet
opencode run "Explain this code" --model ollama/codellama
```

The format is: `provider/model-name`

### 5.5 Provider Fallback

Configure a fallback chain in config:

```json
{
  "provider": {
    "primary": {
      "name": "anthropic",
      "model": "claude-3.5-sonnet"
    },
    "fallback": [
      { "name": "openai", "model": "gpt-4o" },
      { "name": "groq", "model": "llama3-70b" }
    ]
  }
}
```

---

## 6. Using the TUI (Terminal User Interface)

The TUI is OpenCode's primary interactive interface. Launch it with no arguments:

```bash
opencode
```

### 6.1 TUI Layout

```
+----------------------------------------------------------+
|  OpenCode v2.x | Provider: gpt-4o | Mode: Build   |
+----------------------------------------------------------+
|                                                           |
|  [Agent Response Area]                                    |
|  Showing AI responses, file diffs, search results...       |
|                                                           |
|  [File Tree / Search Results Panel]                       |
|                                                           |
|  [Input Prompt] > _                                       |
|  /undo  /redo  /models  /share  /help                    |
+----------------------------------------------------------+
```

### 6.2 Keybindings

| Key | Action |
|-----|--------|
| `Tab` | Toggle between Plan and Build modes |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+L` | Clear screen |
| `Up/Down` | Navigate command history |
| `Ctrl+P` / `Ctrl+N` | Previous/Next in history |
| `Ctrl+D` | Exit OpenCode |
| `Ctrl+R` | Search command history |

### 6.3 Input Features

- **Natural Language**: Just type what you want, e.g., "Add error handling to the login function"
- **File Attachments**: Use `@` to fuzzy-search and attach files: `@src/utils/auth.ts`
- **Image Drag & Drop**: Drag images into the terminal (if supported) for multimodal analysis
- **Multi-line Input**: Use `Shift+Enter` for multi-line prompts
- **Command Prefix**: Start with `/` for built-in commands

---

## 7. Generating agents.md

The `agents.md` file defines behavioral rules and custom instructions for OpenCode agents in your project. It acts as a system prompt that shapes how agents behave.

### 7.1 Automatic Generation

```bash
opencode init
```

During initialization, OpenCode can generate a project-specific `agents.md` with rules derived from:
- Your project's `package.json`, `tsconfig.json`, and other config files
- Detected frameworks (React, Vue, Django, etc.)
- Your coding preferences (TypeScript vs JavaScript, tabs vs spaces, etc.)

### 7.2 Manual Generation

```bash
opencode run "Create an agents.md file for this project"
```

### 7.3 Example agents.md

```markdown
# Project: MyApp

## Coding Standards
- Use TypeScript strict mode
- Follow the existing component patterns
- Import order: React, third-party, then local
- Use named exports over default exports

## Testing
- Run `npm test` before submitting changes
- Aim for 80%+ coverage on new code
- Use React Testing Library for component tests

## Architecture
- Components go in src/components/
- Pages go in src/pages/
- Utils go in src/lib/
- Hooks go in src/hooks/

## Conventions
- Use async/await over .then()
- Prefer functional components with hooks
- Use Tailwind CSS for styling
- Use Zod for validation
```

### 7.4 How agents.md Works

1. OpenCode reads `agents.md` at session start.
2. The content is injected into the system prompt of every agent.
3. Agents follow these rules when generating code, making suggestions, or answering questions.
4. You can update `agents.md` at any time; changes take effect on the next query.

---

## 8. Agent Modes: Build and Plan

OpenCode has two primary interaction modes accessible via the `Tab` key.

### 8.1 Build Mode

**Purpose**: Execute changes directly. The agent reads, writes, edits, and applies patches to your codebase.

**When to use**:
- You know what needs to be done and want it implemented.
- You are refactoring, adding features, or fixing bugs.
- You want immediate results with minimal back-and-forth.

**Behavior**:
- Agent has full tool access (read, write, edit, bash, etc.)
- Changes are applied directly to files.
- Each action is logged and undoable.

**Example flow**:
```
User: "Add a rate limiter to the API routes"
Agent: Reads the route files, understands the structure,
       implements the rate limiter, writes tests, runs them.
```

### 8.2 Plan Mode

**Purpose**: Analyze, research, and plan without making changes. The agent reads files but does NOT write or edit them.

**When to use**:
- You are exploring an unfamiliar codebase.
- You want architectural advice before implementing.
- You need to understand the impact of a change before making it.
- You are in code review and want analysis only.

**Behavior**:
- Agent has restricted tool access (read-only: read, grep, glob, search).
- No write, edit, apply, or destructive commands.
- Output is analysis, suggestions, and plans.

**Example flow**:
```
User: "Plan how to add WebSocket support to our Express app"
Agent: Reads current server setup, middleware, and route structure.
       Analyzes dependencies and potential impact.
       Presents a detailed implementation plan without changing any files.
```

### 8.3 Switching Between Modes

Press `Tab` to toggle between Build and Plan modes. The current mode is displayed in the TUI header.

You can also set the mode explicitly:

```bash
# Start in Plan mode
opencode --mode plan

# Start in Build mode (default)
opencode --mode build
```

### 8.4 Best Practices

- **Start in Plan mode** for unfamiliar codebases or complex changes.
- **Switch to Build mode** once you have a clear plan.
- **Use subagents** within a mode: `@explore find all places where we handle authentication` then `Build the SSO integration`.

---

## 9. Core Built-in Tools for Daily Usage

OpenCode agents have access to a rich set of built-in tools. Here is every tool explained in detail:

### 9.1 File Operation Tools

#### `read`
Reads file contents or lists directory structure.

```markdown
read("src/components/Button.tsx")
```
- Accepts: file path, optional line range (offset/limit)
- Returns: file content with line numbers
- Use case: Understanding existing code, reviewing files

#### `edit`
Performs exact string replacements in a file.

```markdown
edit("src/utils/helpers.ts", "oldFunctionName", "newFunctionName")
```
- Accepts: file path, old string, new string
- Replaces first occurrence by default, or all with `replaceAll`
- Use case: Targeted edits, renaming, fixing bugs
- **Safe**: Fails if old string not found or ambiguous

#### `write`
Writes content to a file (creates or overwrites).

```markdown
write("src/components/NewFeature.tsx", "import React from 'react'\n\nexport const NewFeature = () => { ... }")
```
- Accepts: file path, content string
- Use case: Creating new files, completely rewriting files
- **Warning**: Overwrites existing files without confirmation in auto mode

#### `applyPatch`
Applies a unified diff/patch to a file.

```markdown
applyPatch("src/server.js", patchContent)
```
- Accepts: file path, patch content in unified diff format
- Use case: Applying generated diffs, integrating with other tools
- More precise than `edit` for complex multi-line changes

### 9.2 Search Tools

#### `grep`
Searches file contents using regular expressions.

```markdown
grep("function\\s+\\w+", include="*.ts")
```
- Accepts: regex pattern, optional file pattern, path
- Returns: file paths and line numbers with matching lines
- Use case: Finding function definitions, usage patterns, debugging references

#### `glob`
Finds files by glob patterns.

```markdown
glob("src/**/*.tsx")
```
- Accepts: glob pattern, optional path root
- Returns: matching file paths
- Use case: Listing components, finding config files, exploring project structure

#### `lsp` (Language Server Protocol)
Queries the language server for code intelligence.

```markdown
lsp.getDefinition("src/app.ts:42")
lsp.getReferences("src/utils/helpers.ts:15")
lsp.getHover("src/components/Button.tsx:23")
```
- Accepts: location in file:line format
- Returns: definitions, references, type information, hover docs
- Use case: Navigation, refactoring impact analysis, understanding types

### 9.3 Execution and Web Tools

#### `bash`
Executes shell commands.

```markdown
bash("npm test")
bash("git status")
bash("docker-compose up -d")
```
- Accepts: command string, optional working directory, timeout
- Returns: stdout, stderr, exit code
- Use case: Running tests, git operations, builds, deployments
- **Caution**: Can run arbitrary commands with your permissions

#### `webfetch`
Fetches content from a URL.

```markdown
webfetch("https://api.github.com/repos/opencode/opencode")
```
- Accepts: URL, optional format (markdown, text, html)
- Returns: fetched content converted to markdown
- Use case: Reading documentation, fetching API data, checking web resources

#### `websearch`
Performs a web search.

```markdown
websearch("React 19 new features 2025")
```
- Accepts: query, number of results
- Returns: search results with snippets
- Use case: Researching libraries, finding solutions, staying updated

### 9.4 Agent Utility Tools

#### `todowrite`
Creates and manages a structured task list within the session.

```markdown
todowrite([
  { content: "Set up authentication", status: "pending", priority: "high" },
  { content: "Create login page", status: "in_progress", priority: "high" },
  { content: "Write tests", status: "pending", priority: "medium" }
])
```
- Accepts: array of task objects with content, status, priority
- Use case: Breaking down complex tasks, tracking progress, staying organized

#### `question`
Asks the user for input or clarification during a task.

```markdown
question({
  header: "Component Type",
  question: "Should this be a class or functional component?",
  options: [
    { label: "Functional", description: "Use hooks, recommended for new code" },
    { label: "Class", description: "Use lifecycle methods" }
  ]
})
```
- Accepts: question object with header, question text, options array
- Returns: user's selected answer
- Use case: Getting user preferences, clarifying requirements, making decisions

#### `skill`
Loads a specialized skill for domain-specific tasks.

```markdown
skill("customize-opencode")
```
- Accepts: skill name
- Returns: skill instructions and resources injected into context
- Use case: Loading specialized workflows (testing, deployment, configuration)

---

## 10. Everyday Commands

### 10.1 Session Management

#### `/undo`
Reverts the last action taken by the agent.

```
> /undo
[System] Reverted: edit("src/auth.js")
```

Use case: When an edit wasn't what you wanted, step back.

#### `/redo`
Reapplies the last undone action.

```
> /redo
[System] Reapplied: edit("src/auth.js")
```

Use case: When you undid too far and want to go forward again.

#### `/compact`
Compacts the conversation history to save context window space.

```
> /compact
[System] Compacted 24 messages into summary. Context window freed.
```

Use case: Long sessions where context is getting full. Summarizes past exchanges.

### 10.2 File and Model Commands

#### `/attach`
Attaches a file to the current context.

```
> /attach src/utils/helpers.ts
[System] Attached: src/utils/helpers.ts (245 lines)
```

You can also use `@` fuzzy search directly in the prompt:
```
> @auth.ts explain the authentication flow
```

#### `/models`
Lists available models for the current provider.

```
> /models
Available models:
- gpt-4o (default)
- gpt-4-turbo
- gpt-3.5-turbo
- o1-preview
```

#### `/model`
Switches the active model during a session.

```
> /model gpt-4-turbo
[System] Switched to: gpt-4-turbo
```

### 10.3 Sharing and Collaboration

#### `/share`
Generates a shareable link of the current session.

```
> /share
[System] Session shared: https://opencode.ai/share/abc123
```

Use case: Sharing context with teammates, asking for help, code reviews.

#### `/help`
Shows help information.

```
> /help
Available commands:
/undo     - Undo last action
/redo     - Redo last undone action
/compact  - Compact conversation history
/models   - List available models
/model    - Switch model
/attach   - Attach a file
/share    - Share session
/connect  - Connect to remote agent
/init     - Initialize project config
```

#### `/connect`
Connects to a remote OpenCode agent or session.

```
> /connect my-server:4096
```

#### `/init`
Re-initializes the project configuration.

```
> /init
[System] Starting configuration wizard...
```

---

## 11. Subagents: @general, @explore, @scout

Subagents are specialized agents that can be invoked inline within any prompt using the `@` mention syntax.

### 11.1 @general (General Agent)

**Purpose**: General-purpose coding assistant. Handles a wide range of tasks.

```
> @general Add pagination to the users table component
```

**Strengths**:
- Broad knowledge across languages and frameworks
- Can use any tool in the toolkit
- Good for ambiguous or multi-step tasks

**When to use**:
- Default choice for most tasks
- When you're not sure which agent to use
- Complex, multi-step implementations

### 11.2 @explore (Explore Agent)

**Purpose**: Codebase exploration and understanding. Designed to be thorough.

```
> @explore How is authentication currently implemented in this project?
```

**Strengths**:
- Reads extensively to build comprehensive understanding
- Follows import chains and references
- Produces detailed reports and analysis
- Uses `grep`, `glob`, `lsp`, and `read` tools extensively

**When to use**:
- Onboarding to a new codebase
- Understanding complex features
- Finding all places where a pattern is used
- Before making architectural changes

**Behavior**:
- Will read multiple files to trace logic
- Reports back with file paths and line numbers
- Does NOT make changes (even in Build mode, it's focused on exploration)

### 11.3 @scout (Scout Agent)

**Purpose**: Fast, focused targeted searches. Lighter and quicker than Explore.

```
> @scout Find all API endpoint definitions
```

**Strengths**:
- Faster than Explore (reads fewer files)
- Focused on specific patterns or questions
- Lower token usage

**When to use**:
- Quick lookups
- When you know exactly what you're looking for
- Pattern matching across the codebase
- Before a targeted edit

**Comparison**:

| Aspect | @explore | @scout |
|--------|----------|--------|
| Speed | Slower (thorough) | Faster (focused) |
| Depth | Deep - follows chains | Shallow - surface level |
| Read files | Many | Few |
| Best for | Onboarding, architecture | Quick findings, grep-like |
| Token usage | High | Low |

### 11.4 Chaining Subagents

You can combine multiple subagents in one session:

```
> @explore Find all places that handle user authentication
... [explore returns results]
> @general Now add SSO support to all those places
... [general implements based on explore's findings]
```

This pattern is powerful: **explore first, then implement**.

---

## 12. opencode.json: Complete Configuration Reference

The `opencode.json` file is the central configuration file for OpenCode. It can exist at the project level, global level, or be specified via environment variable.

### 12.1 Full Schema

```json
{
  "$schema": "https://opencode.ai/schemas/opencode.json",

  "provider": {
    "name": "openai",
    "model": "gpt-4o",
    "temperature": 0.2,
    "maxTokens": 4096,
    "topP": 1,
    "frequencyPenalty": 0,
    "presencePenalty": 0,
    "stop": ["```"],
    "apiKey": "$OPENAI_API_KEY",
    "baseUrl": "https://api.openai.com/v1",
    "primary": {
      "name": "anthropic",
      "model": "claude-3.5-sonnet"
    },
    "fallback": [
      { "name": "openai", "model": "gpt-4o" }
    ]
  },

  "customCommands": [
    {
      "name": "test",
      "prompt": "Run the test suite and report results. Fix any failures using $ARGUMENTS",
      "description": "Run tests with optional filter"
    },
    {
      "name": "component",
      "prompt": "Create a new React component named $ARGUMENTS with TypeScript, tests, and stories",
      "description": "Generate a new component"
    }
  ],

  "agents": {
    "my-custom-agent": {
      "description": "Custom agent for database migrations",
      "systemPrompt": "You are a database migration expert. Always create down migrations alongside up migrations.",
      "tools": ["read", "write", "edit", "bash", "grep", "glob"],
      "model": "gpt-4o"
    }
  },

  "mcpServers": {
    "notion": {
      "type": "stdio",
      "command": "npx",
      "args": ["@notionhq/opencode-mcp-server"],
      "env": {
        "NOTION_API_KEY": "$NOTION_API_KEY"
      }
    },
    "database": {
      "type": "sse",
      "url": "https://mcp.internal.company.com/sse"
    },
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/data"]
    }
  },

  "permissions": {
    "allowedCommands": ["npm", "git", "docker", "node"],
      "blockedCommands": ["rm -rf", "sudo"],
    "allowedPaths": ["src/", "tests/"],
    "blockedPaths": ["node_modules/", ".git/"],
    "autoApprove": false
  },

  "rules": {
    "alwaysIncludeTests": true,
    "followAgentsMd": true,
    "requireLintPass": true,
    "maxLineLength": 100,
    "preferNamedExports": true
  },

  "interface": {
    "theme": "dark",
    "language": "en",
    "undoLimit": 100,
    "autoApplyPatches": false,
    "showDiffs": true,
    "streamResponses": true,
    "compactOnStart": false,
    "tabCompletion": true
  },

  "tools": {
    "read": { "enabled": true, "maxSize": 50000 },
    "edit": { "enabled": true, "requireConfirmation": true },
    "write": { "enabled": true, "requireConfirmation": true },
    "bash": { "enabled": true, "timeout": 120000, "requireConfirmation": true },
    "grep": { "enabled": true },
    "glob": { "enabled": true },
    "webfetch": { "enabled": true, "timeout": 30000 },
    "websearch": { "enabled": true }
  },

  "agentSettings": {
    "maxToolCallsPerTurn": 25,
    "maxTokensPerResponse": 4096,
    "contextWindow": 128000,
    "planMode": {
      "blockWriteTools": true
    }
  },

  "experimental": {
    "agentMemory": false,
    "parallelToolCalls": true,
    "visionSupport": true
  }
}
```

### 12.2 Section Breakdown

#### provider
Configures the LLM provider and model. Supports primary/fallback chains.

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Provider name: openai, anthropic, google, mistral, groq, zen, ollama |
| `model` | string | Model identifier, e.g. gpt-4o, claude-3.5-sonnet |
| `temperature` | number | Creativity/randomness (0-1). Lower = more deterministic |
| `maxTokens` | number | Maximum tokens per response |
| `apiKey` | string | API key or env var reference like `$ENV_VAR_NAME` |
| `baseUrl` | string | Custom API endpoint URL |
| `fallback` | array | Ordered list of backup providers |

#### customCommands
Defines reusable commands accessible via `/` in the TUI.

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Command name (used as `/name`) |
| `prompt` | string | Prompt template. Use `$ARGUMENTS` for user input |
| `description` | string | Shown in help and tab completion |

#### agents
Defines custom agents beyond the built-in ones.

| Field | Type | Description |
|-------|------|-------------|
| `description` | string | What the agent does |
| `systemPrompt` | string | Custom system instructions |
| `tools` | array | Which tools the agent can access |
| `model` | string | Optional model override for this agent |

#### mcpServers
Configures MCP (Model Context Protocol) servers.

| Field | Type | Description |
|-------|------|-------------|
| `type` | string | `stdio` for local processes, `sse` for remote HTTP servers |
| `command` | string | Command to run (for stdio) |
| `args` | array | Arguments for the command |
| `url` | string | SSE endpoint URL (for sse) |
| `env` | object | Environment variables for the process |

#### permissions
Security controls for what agents can do.

| Field | Type | Description |
|-------|------|-------------|
| `allowedCommands` | array | Bash commands that are permitted |
| `blockedCommands` | array | Bash commands that are forbidden |
| `allowedPaths` | array | File paths that agents can modify |
| `blockedPaths` | array | File paths that are off-limits |
| `autoApprove` | boolean | Whether to auto-approve tool calls |

#### tools
Granular control over individual tools.

| Field | Type | Description |
|-------|------|-------------|
| `enabled` | boolean | Whether the tool is available |
| `timeout` | number | Max execution time in ms |
| `requireConfirmation` | boolean | Whether user must approve each call |
| `maxSize` | number | Max file size for read operations |

---

## 13. Configuration Priority Chain

OpenCode loads configuration from multiple sources with a defined priority order. Higher priority overrides lower priority.

### Priority Levels

```
HIGHEST PRIORITY
    |
    |  5. Managed Config (admin-enforced, non-overridable)
    |  4. Project Config (opencode.json in project root)
    |  3. Custom Config (opencode_config env var)
    |  2. Global Config (~/.config/opencode/opencode.json)
    |  1. Remote Org Defaults (lowest priority)
    |
LOWEST PRIORITY
```

### Level 5: Managed Config (Highest Priority)

**Purpose**: Enforced by organization administrators. Cannot be overridden by users.

**Location**: Managed by admin tooling, typically pushed centrally.

**Characteristics**:
- Non-overridable by any lower level
- Used for security policies, compliance rules
- Enforces provider restrictions, blocked commands, required tool permissions
- Example use case: Block all non-approved AI providers, enforce audit logging

**Example** (pushed by admin):
```json
{
  "permissions": {
    "allowedCommands": ["npm", "git", "node"],
    "blockedCommands": ["rm -rf", "sudo", "curl"],
    "autoApprove": false
  },
  "provider": {
    "name": "openai",
    "model": "gpt-4o"
  }
}
```

### Level 4: Project Config

**Purpose**: Project-specific settings that travel with the codebase.

**Location**: `./opencode.json` in the project root directory.

**Characteristics**:
- Should be committed to version control
- Defines project conventions, custom commands, agents
- Team members share the same project config
- Overrides global and remote defaults

**Example**:
```json
{
  "customCommands": [
    { "name": "test", "prompt": "Run npm test", "description": "Run tests" }
  ],
  "rules": {
    "alwaysIncludeTests": true,
    "preferNamedExports": true
  }
}
```

### Level 3: Custom Config (Environment Variable)

**Purpose**: User-specific overrides for a particular session or workflow.

**Mechanism**: Set the `opencode_config` environment variable to point to a config file.

```bash
# PowerShell
$env:opencode_config = "C:\Users\me\.config\opencode\work-config.json"

# Linux/macOS
export opencode_config="$HOME/.config/opencode/work-config.json"
```

**Characteristics**:
- Overrides global config but not project or managed config
- Useful for switching between work/personal configurations
- Can be set per terminal session
- Allows testing different configurations without modifying global or project files

### Level 2: Global Config

**Purpose**: User-wide defaults for all OpenCode sessions.

**Location**:
- **Windows**: `%USERPROFILE%\.config\opencode\opencode.json`
- **Linux/macOS**: `~/.config/opencode/opencode.json`

**Characteristics**:
- Set once, applies everywhere
- Contains user preferences (theme, default provider, API keys)
- Created during `opencode init`
- Overridden by project, custom, and managed configs

### Level 1: Remote Org Defaults (Lowest Priority)

**Purpose**: Organization-wide defaults served from a remote endpoint.

**Mechanism**: Configured via DNS or a well-known URL that OpenCode fetches on startup.

**Characteristics**:
- Lowest priority - easily overridden
- Used for baseline settings in organizations
- Can set default provider, policies, and tool availability
- Users can override any setting at higher levels

### Merging Behavior

Configs are **deep-merged** according to priority:
- Objects are merged recursively (lower priority fields are preserved if not overridden)
- Arrays are replaced, not merged (to prevent duplicates)
- Primitive values from higher priority override lower priority

**Example**: If global config sets `theme: "dark"` and project config sets `theme: "light"`, the project config wins.

---

## 14. Agent Types in Depth

### 14.1 Build Agent

The Build agent is the default operational agent. It executes tasks by reading, writing, and modifying code.

**Characteristics**:
- Full tool access (read, write, edit, bash, grep, glob, etc.)
- Applies changes directly to files
- Operates iteratively: analyze, implement, verify
- Best for: implementing features, fixing bugs, refactoring

**Workflow**:

```
1. User: "Add input validation to the registration form"
2. Build Agent reads the form component
3. Reads any existing validation logic
4. Implements validation using the project's pattern (e.g., Zod)
5. Runs tests to verify
6. Reports results with a diff summary
```

### 14.2 Plan Agent

The Plan agent is the analytical counterpart. It researches and plans without modifying files.

**Characteristics**:
- Read-only tool access (read, grep, glob, search, LSP)
- Cannot write, edit, or execute destructive commands
- Produces detailed analysis and implementation plans
- Best for: code review, architecture planning, onboarding

**Workflow**:

```
1. User: "Plan the migration from REST to GraphQL"
2. Plan Agent reads all API route definitions
3. Analyzes data models and resolvers needed
4. Maps REST endpoints to GraphQL queries/mutations
5. Presents a step-by-step migration plan
6. Highlights potential breaking changes and risks
```

### 14.3 General Agent (Subagent)

Invoked via `@general`. A capable all-rounder for any coding task.

**Capabilities**:
- Same tool access as Build mode
- Broad knowledge across languages and frameworks
- Handles ambiguous requirements well
- Can chain multiple tools to solve complex problems

### 14.4 Explore Agent (Subagent)

Invoked via `@explore`. Specialized for deep codebase exploration.

**Capabilities**:
- Reads extensively (multiple files, follows import chains)
- Uses `grep`, `glob`, and LSP to trace references
- Produces comprehensive reports
- Does not make changes

**Ideal for**:
- "How does the payment system work end-to-end?"
- "Find all places where user data is accessed"
- "What's the current state of our test coverage?"

### 14.5 Scout Agent (Subagent)

Invoked via `@scout`. Fast, focused search agent.

**Capabilities**:
- Quick pattern matching across files
- Light token usage
- Returns concise results
- Does not make changes

**Ideal for**:
- "Where is the database connection configured?"
- "Find all TODO comments"
- "List all API endpoints"

---

## 15. Creating Custom Agents

Custom agents allow you to define specialized AI personas with tailored instructions and tool access.

### 15.1 Method A: Define in opencode.json

**Step 1**: Add agent to `opencode.json`:

```json
{
  "agents": {
    "migration-expert": {
      "description": "Database migration specialist",
      "systemPrompt": "You are a database migration expert. You specialize in PostgreSQL and Prisma ORM. Always generate both up and down migrations. Validate migrations before applying. Follow the project's existing migration naming convention.",
      "tools": ["read", "write", "edit", "bash", "grep", "glob"],
      "model": "gpt-4o",
      "temperature": 0.1
    },
    "ui-designer": {
      "description": "UI/UX component designer",
      "systemPrompt": "You are a UI developer specializing in React, Tailwind CSS, and Framer Motion. You follow accessibility best practices (WCAG AA). You use the project's existing design system components. Generate pixel-perfect implementations.",
      "tools": ["read", "write", "edit", "glob", "grep"],
      "model": "anthropic/claude-3.5-sonnet",
      "temperature": 0.3
    }
  }
}
```

**Step 2**: Use your custom agent:

```
> @migration-expert Create a migration to add a subscriptions table
> @ui-designer Redesign the settings page with the new design tokens
```

### 15.2 Method B: Agent Markdown Files

Create agent definitions as markdown files in `.opencode/agents/`.

**File**: `.opencode/agents/migration-expert.md`

```markdown
# migration-expert

## Description
Database migration specialist for PostgreSQL and Prisma ORM.

## System Prompt
You are a database migration expert. You specialize in PostgreSQL and Prisma ORM.
Always generate both up and down migrations. Validate migrations before applying.
Follow the project's existing migration naming convention.

## Tools
- read
- write
- edit
- bash
- grep
- glob

## Model
gpt-4o

## Temperature
0.1
```

Then register in `opencode.json`:

```json
{
  "agents": {
    "migration-expert": {
      "fromFile": ".opencode/agents/migration-expert.md"
    }
  }
}
```

### 15.3 Method C: Using opencode agent create

```bash
# Interactive creation
opencode agent create

# You'll be prompted for:
# 1. Agent name
# 2. Description
# 3. System prompt / instructions
# 4. Allowed tools
# 5. Model (optional override)
```

### 15.4 Naming and Organization

```
.opencode/
  agents/
    migration-expert.md
    ui-designer.md
    security-auditor.md
```

The `.opencode/` directory should be committed to version control so all team members have access to the same agents.

---

## 16. Custom Commands

Custom commands are reusable shortcuts that trigger specific prompts. They are accessible via `/command-name` in the TUI.

### 16.1 Define in opencode.json

```json
{
  "customCommands": [
    {
      "name": "test",
      "prompt": "Run the test suite. If tests fail, analyze the failures and fix them. Run tests again to confirm. $ARGUMENTS",
      "description": "Run tests and fix failures"
    },
    {
      "name": "component",
      "prompt": "Create a new React component named $ARGUMENTS. Create the component file, a test file, a stories file, and an index file. Follow the project conventions.",
      "description": "Generate a new React component"
    },
    {
      "name": "deploy",
      "prompt": "Deploy the current branch to staging environment. Run tests first, then build, then deploy using the project's deployment script. $ARGUMENTS",
      "description": "Deploy to staging"
    },
    {
      "name": "review",
      "prompt": "Review the changes in the current branch compared to main. Analyze all changed files for bugs, security issues, and code quality. Provide a detailed report.",
      "description": "Code review for current branch"
    },
    {
      "name": "docs",
      "prompt": "Generate documentation for $ARGUMENTS. Read the file(s), understand the code, and produce comprehensive documentation in the project's documentation format.",
      "description": "Generate documentation"
    }
  ]
}
```

### 16.2 Using $ARGUMENTS

The `$ARGUMENTS` placeholder is replaced with whatever the user types after the command name.

**Examples**:

```
> /test
Expands to: "Run the test suite. If tests fail, analyze the failures and fix them. Run tests again to confirm."

> /component UserProfile
Expands to: "Create a new React component named UserProfile. Create the component file, a test file, a stories file, and an index file. Follow the project conventions."

> /test --watch
Expands to: "Run the test suite. If tests fail, analyze the failures and fix them. Run tests again to confirm. --watch"

> /deploy --env production
Expands to: "Deploy the current branch to staging environment. Run tests first, then build, then deploy using the project's deployment script. --env production"
```

### 16.3 Creating Custom Commands as Markdown Files

Create command definitions in `.opencode/commands/`.

**File**: `.opencode/commands/test.md`

```markdown
# test

## Description
Run the test suite and fix any failures.

## Prompt
Run the test suite. If tests fail, analyze the failures and fix them.
Run tests again to confirm the fixes work.
$ARGUMENTS
```

**File**: `.opencode/commands/component.md`

```markdown
# component

## Description
Generate a new React component with tests and stories.

## Prompt
Create a new React component named $ARGUMENTS.
Create the component file, a test file, a stories file, and an index file.
Follow the project conventions for file structure and code style.
Use TypeScript, add proper JSDoc comments, and ensure accessibility.
```

These are auto-discovered and don't need separate registration in `opencode.json`.

### 16.4 Benefits of Custom Commands

- **Standardization**: Every team member runs the same command with the same prompt.
- **Speed**: `/component UserProfile` is much faster than typing the full prompt each time.
- **Consistency**: Enforces project conventions and best practices.
- **Discoverability**: Tab completion shows available commands with descriptions.

---

## 17. Rules & Instructions from agents.md

The `agents.md` file serves as a persistent instruction set for all agents in the project.

### 17.1 How It Works

1. OpenCode reads `agents.md` from the project root (if it exists).
2. The content is injected into the system prompt for every agent invocation.
3. Agents treat these as hard rules - they guide all code generation, suggestions, and decisions.

### 17.2 Example Structure

```markdown
# Project: E-Commerce Platform

## Technology Stack
- Frontend: Next.js 14 (App Router), TypeScript, Tailwind CSS, Shadcn UI
- Backend: Next.js API routes, Prisma ORM, PostgreSQL
- Testing: Vitest, Playwright
- Package Manager: pnpm

## Code Conventions
- Use TypeScript strict mode. No `any` types.
- Use named exports for all components and utilities.
- Follow the existing folder structure:
  - src/app/ - Next.js App Router pages and layouts
  - src/components/ - Reusable React components
  - src/lib/ - Utility functions and helpers
  - src/server/ - Server-side logic, API routes
  - src/validations/ - Zod schemas
- Import order: React/Next.js, third-party libraries, local modules (alphabetical).
- Use async/await over raw promises or callbacks.

## Styling
- Use Tailwind CSS utility classes exclusively. No CSS modules or styled-components.
- Follow mobile-first responsive design.
- Use the project's design tokens (colors, spacing) from tailwind.config.ts.

## State Management
- Use React Server Components by default. Only add 'use client' when necessary.
- Use Zustand for client-side global state.
- Use React Query (TanStack Query) for server state.

## Testing Requirements
- Every new component must have a Vitest test file.
- Every API endpoint must have integration tests.
- Tests must use Testing Library queries (getByRole, findByText) over test IDs.
- Mock external services with MSW (Mock Service Worker).

## Performance
- All images must use next/image with proper sizing.
- Implement streaming where possible for data-heavy pages.
- Use React.memo and useMemo only when profiling shows a bottleneck.

## Security
- Validate all API inputs with Zod schemas.
- Sanitize user-generated content before rendering.
- Use server actions for form submissions, not client-side fetch.
```

### 17.3 Auto-Generation Based on Project

OpenCode can generate `agents.md` from your project's existing configuration:

```bash
opencode run "Generate an agents.md file based on our project configuration"
```

It will analyze:
- `package.json` for dependencies and scripts
- `tsconfig.json` for TypeScript settings
- Framework-specific configs (next.config.js, tailwind.config.ts, etc.)
- Existing code patterns (import style, naming conventions, etc.)

### 17.4 Best Practices for agents.md

- **Keep it current**: Update as your stack evolves.
- **Be specific**: "Use Tailwind CSS" is better than "Use good styling."
- **Include examples**: Show preferred patterns for common tasks.
- **Cover testing**: Specify testing framework and conventions.
- **Document exceptions**: Note when rules don't apply.

---

## 18. MCP (Model Context Protocol)

### 18.1 What Is MCP?

The **Model Context Protocol (MCP)** is an open standard developed by Anthropic that allows AI models to connect with external tools, data sources, and services. Think of it as a "USB-C for AI" - a universal protocol that lets any AI assistant interact with any MCP-compatible server.

### 18.2 How MCP Connects to OpenCode

OpenCode has native MCP support. This means:
- You can connect your AI coding assistant to live databases, APIs, and file systems.
- Agents can read from and write to external services through MCP servers.
- MCP tools appear alongside built-in tools in the agent's toolkit.

### 18.3 Configuring MCP in opencode.json

MCP servers are configured under the `mcpServers` key:

```json
{
  "mcpServers": {
    "notion": {
      "type": "stdio",
      "command": "npx",
      "args": ["@notionhq/opencode-mcp-server"],
      "env": {
        "NOTION_API_KEY": "$NOTION_API_KEY"
      }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres", "postgresql://localhost:5432/mydb"],
      "env": {
        "PGPASSWORD": "$PGPASSWORD"
      }
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "$GITHUB_TOKEN"
      }
    },
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/data/shared"]
    },
    "api-gateway": {
      "type": "sse",
      "url": "https://mcp.internal.company.com/sse"
    }
  }
}
```

### 18.4 MCP Server Types

| Type | Description | Use Case |
|------|-------------|----------|
| `stdio` | Local process spawned by OpenCode | Databases, local tools, filesystem |
| `sse` | Remote server via Server-Sent Events | Cloud services, internal APIs |

### 18.5 Example: Notion Integration

With the Notion MCP server configured, you can:

```
> @general Find my tasks in the "Sprint 24" Notion database and list them
> @general Create a new Notion page in the "Bug Reports" database with the details of this issue
> @general Update the status of task "API rate limiting" to "In Progress" in Notion
```

The agent will:
1. Connect to the Notion MCP server.
2. Query Notion databases using the server's tools.
3. Read, create, or update pages as instructed.

### 18.6 Example: Database Integration

With the PostgreSQL MCP server:

```
> @scout Find all users who haven't logged in for 30 days from the database
> @general Run a migration to add a `lastLoginAt` column to the users table
```

The agent can run SQL queries, read schemas, and even generate migrations based on the live database structure.

### 18.7 MCP vs Built-in Tools

| Aspect | Built-in Tools | MCP Tools |
|--------|---------------|-----------|
| Scope | File operations, search, execution | External services, databases, APIs |
| Availability | Always available | Require server configuration |
| Security | Controlled by permissions | Key-based access per server |
| Latency | Local (fast) | Network-dependent |

### 18.8 Finding MCP Servers

- Official servers: `@modelcontextprotocol/server-*` on npm
- Community servers: Search npm/GitHub for "mcp-server"
- Custom servers: Build your own using the MCP SDK

---

## 19. Multi-Interface: CLI, TUI, Web, API

OpenCode can be used through multiple interfaces depending on your workflow.

### 19.1 CLI (Command Line Interface) - One-Shot Mode

Use `opencode run` for single queries without entering the interactive TUI.

```bash
# Quick question
opencode run "Explain how closures work in JavaScript"

# With specific model
opencode run "Refactor this function" --model openai/gpt-4o

# With file context
opencode run "Review the changes in auth.ts" --file src/auth.ts

# Continue from previous session
opencode --continue

# Pipe content in
cat error.log | opencode run "Analyze these errors and suggest fixes"

# Use in scripts/CI
opencode run "Bump version to 2.1.0 and update changelog" --no-interactive
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--model` | Override provider/model |
| `--file` | Attach a file to the prompt |
| `--continue` | Continue from last session |
| `--no-interactive` | Non-interactive mode (no confirmations) |
| `--mode` | Start in plan or build mode |
| `--provider` | Specify provider |

### 19.2 TUI (Terminal User Interface) - Interactive Mode

```bash
# Start interactive session
opencode

# With specific working directory
opencode --workdir /path/to/project

# With initial prompt
opencode --prompt "Set up authentication"
```

### 19.3 Web Interface

```bash
# Start web server
opencode web --port 4096 --hostname 0.0.0.0
```

Opens a web-based UI accessible from any browser. Useful for:
- Team collaboration
- Remote development
- Visual diff viewing
- Sharing sessions

### 19.4 API Mode

OpenCode can be used programmatically via its API.

```bash
# Start API server
opencode serve --port 8080
```

```javascript
// Example: Using the API from a script
const response = await fetch('http://localhost:8080/api/run', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    prompt: 'Add error handling to the login function',
    model: 'gpt-4o',
    files: ['src/auth/login.ts']
  })
});
```

### 19.5 Comparison

| Feature | CLI | TUI | Web | API |
|---------|-----|-----|-----|-----|
| Start | `opencode run "..."` | `opencode` | `opencode web` | `opencode serve` |
| Best for | Quick queries, CI/CD | Extended sessions | Visual, sharing | Custom tooling |
| File editing | Yes | Yes | Yes | Yes |
| Session history | One-shot | Full | Full | Configurable |
| Undo/Redo | No | Yes | Yes | Yes |
| Image support | No | Yes | Yes | Yes |
| Multi-user | No | No | Yes | Yes |

---

## 20. Web Interface

### 20.1 Starting the Web Server

```bash
# Basic start
opencode web

# Custom port and host
opencode web --port 4096 --hostname 0.0.0.0

# With authentication
opencode web --port 4096 --auth-token my-secret-token
```

### 20.2 Features

The web interface provides:
- Full chat interface similar to ChatGPT/Claude web.
- File tree viewer for navigating the project.
- Diff viewer for reviewing changes before accepting.
- Session management (multiple sessions).
- Shareable session links for collaboration.
- Provider and model switching from the UI.

### 20.3 Use Cases

- **Pair Programming**: Share the web URL with a teammate for real-time collaboration.
- **Remote Development**: Run OpenCode on a server, access via browser.
- **Non-Developer Stakeholders**: Product managers can ask questions about the codebase.
- **Code Review**: Review diffs in a visual interface.

---

## 21. GitHub Integration & CI/CD

### 21.1 GitHub App Installation

```bash
opencode github install
```

This command:
1. Authenticates with GitHub.
2. Installs the OpenCode GitHub App in your account/organization.
3. Configures webhooks and permissions.

### 21.2 Setting Up GitHub Actions

**Step 1**: Configure provider and model:

```bash
opencode github install --provider openai --model gpt-4o
```

**Step 2**: This creates two workflow files:

#### `.github/workflows/opencode.yml`

```yaml
name: OpenCode PR Review

on:
  pull_request:
    types: [opened, synchronize, labeled]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run OpenCode PR Review
        uses: opencode/actions/pr-review@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          opencode-api-key: ${{ secrets.OPENCODE_ZEN_API_KEY }}
          model: gpt-4o
          mode: plan
```

#### `.github/workflows/opencode-pr-check.yml`

```yaml
name: OpenCode PR Check

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: OpenCode PR Check
        uses: opencode/actions/pr-check@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          opencode-api-key: ${{ secrets.OPENCODE_ZEN_API_KEY }}
          check-type: |
            - Verify no security vulnerabilities
            - Check code style compliance
            - Ensure test coverage
            - Validate documentation updates
```

### 21.3 GitHub Secrets & Variables

Set these in your GitHub repository:

```bash
# Navigate to: Settings > Secrets and variables > Actions
# Add repository secret:
OPENCODE_ZEN_API_KEY: your-api-key-here
```

### 21.4 Using OpenCode Run in CI/CD

```bash
# Example: Automated version bump
opencode run "Bump version from 1.0.0 to 1.1.0, update changelog, and create a git tag" --no-interactive

# Example: Generate release notes
opencode run "Generate release notes from commits since last tag" --no-interactive

# Example: Automated code cleanup
opencode run "Remove all console.log statements from src/ and add proper error logging" --no-interactive
```

### 21.5 PR Review Workflow

1. Developer opens a PR.
2. GitHub Actions triggers OpenCode PR Check.
3. OpenCode analyzes the changes (in Plan mode - no modifications).
4. Comments are posted on the PR with findings.
5. Labels are added (e.g., `needs-changes`, `approved`, `security-concern`).

### 21.6 Security Best Practices

- Store API keys in GitHub Secrets, never in code.
- Use `OPENCODE_ZEN_API_KEY` as the secret name convention.
- Limit OpenCode's permissions to read-only in CI (Plan mode).
- Review OpenCode's suggestions before accepting them in automated workflows.

---

## 22. AI SDK / OpenAI-Compatible

### 22.1 OpenAI-Compatible API

OpenCode uses the OpenAI-compatible API format, meaning any service that offers an OpenAI-compatible endpoint can be used as a provider.

```json
{
  "provider": {
    "name": "custom",
    "model": "my-model",
    "baseUrl": "https://api.my-custom-service.com/v1",
    "apiKey": "$CUSTOM_API_KEY"
  }
}
```

### 22.2 Supported Providers via OpenAI Compatibility

| Provider | Base URL | Notes |
|----------|----------|-------|
| OpenAI | `https://api.openai.com/v1` | Native |
| Azure OpenAI | `https://{resource}.openai.azure.com` | Enterprise |
| Together AI | `https://api.together.xyz/v1` | Open models |
| Fireworks AI | `https://api.fireworks.ai/inference/v1` | Fast inference |
| vLLM | `http://localhost:8000/v1` | Self-hosted |
| Ollama | `http://localhost:11434/v1` | Local models |
| Groq | `https://api.groq.com/openai/v1` | Fast, free tier |
| Mistral | `https://api.mistral.ai/v1` | European provider |
| DeepSeek | `https://api.deepseek.com` | Cost-effective |
| Together | `https://api.together.xyz/v1` | Open-source models |

### 22.3 Custom Provider Configuration

```json
{
  "provider": {
    "name": "custom",
    "displayName": "My Internal Model",
    "model": "gpt-4o-mini",
    "baseUrl": "https://internal-ai.company.com/api/v1",
    "apiKey": "$INTERNAL_AI_KEY",
    "headers": {
      "X-Organization-ID": "org-123",
      "X-Source": "opencode"
    },
    "models": {
      "list": "/models",
      "completions": "/chat/completions"
    }
  }
}
```

### 22.4 Model Selection Strategies

```bash
# Use specific model
opencode run "..." --model openai/gpt-4o

# Use cheapest model for simple tasks
opencode run "Format this file" --model openai/gpt-4o-mini

# Use fastest model
opencode run "Quick grep" --model groq/llama3-70b

# Use local model (offline)
opencode run "..." --model ollama/codellama

# Use preview/advanced model
opencode run "..." --model openai/o1-preview
```

---

## 23. Pro Tips & Best Practices

### 23.1 Fuzzy File Search with @

Instead of typing full file paths, use `@` for fuzzy search:

```
> @auth.ts add JWT token refresh logic
> @user-profile.tsx fix the layout on mobile
> @utils/api-client.ts add retry logic
```

This is faster than manual typing and handles file location changes gracefully.

### 23.2 Drag and Drop Images

Drag images directly into the TUI for multimodal analysis:

```
> [Drag image of error message] What does this error mean and how do I fix it?
> [Drag UI mockup] Implement this design using our component library
> [Drag screenshot of bug] Fix this UI bug
```

### 23.3 Plan Then Build

The most powerful workflow: plan first, then implement.

```
Step 1: Press Tab to switch to Plan mode
> Plan the implementation of a caching layer for our API

Step 2: Review the plan
Step 3: Press Tab to switch to Build mode
> Implement the caching layer as planned
```

This prevents costly mistakes and gives you full understanding of what will change before it changes.

### 23.4 Use OpenCode Run in CI/CD

Automate repetitive tasks in your CI pipeline:

```bash
# In your CI script
opencode run "Auto-format all TypeScript files" --no-interactive
opencode run "Check for deprecated API usage" --no-interactive --mode plan
opencode run "Generate API documentation" --no-interactive
```

### 23.5 Custom Commands for Daily Workflows

Create commands for your most frequent tasks:

```json
{
  "customCommands": [
    { "name": "fix", "prompt": "Fix the following issue: $ARGUMENTS", "description": "Fix a bug or issue" },
    { "name": "refactor", "prompt": "Refactor $ARGUMENTS to improve code quality", "description": "Refactor code" },
    { "name": "doc", "prompt": "Add JSDoc comments to $ARGUMENTS", "description": "Add documentation" },
    { "name": "type", "prompt": "Add TypeScript types to $ARGUMENTS", "description": "Add TypeScript types" },
    { "name": "test", "prompt": "Write tests for $ARGUMENTS", "description": "Generate tests" }
  ]
}
```

### 23.6 Context Window Management

- Use `/compact` regularly in long sessions to summarize history.
- Attach only relevant files with `/attach` rather than whole directories.
- Start new sessions for unrelated tasks.
- Use `@scout` for quick lookups instead of `@explore` to save tokens.

### 23.7 Provider Switching

Switch providers based on task:

```
# Cheap and fast for simple tasks
> /model openai/gpt-4o-mini

# Powerful for complex refactoring
> /model anthropic/claude-3.5-sonnet

# Local for offline work
> /model ollama/codellama
```

### 23.8 Collaborative Workflows

```
1. Developer A: "Create a shareable session"
   > /share
   [Session URL: https://opencode.ai/share/abc123]

2. Developer A shares URL with Developer B.

3. Developer B: Reviews the session, adds comments.
```

### 23.9 Safety and Permissions

- Always review diffs before accepting in auto-approve mode.
- Use `blockedCommands` to prevent dangerous operations.
- Use `allowedPaths` to restrict what agents can modify.
- Start with Plan mode for unfamiliar tasks.
- Use `agents.md` to enforce project-specific safety rules.

### 23.10 Performance Optimization

```json
{
  "experimental": {
    "parallelToolCalls": true
  }
}
```

Enable parallel tool calls to speed up multi-step operations. The agent can read multiple files, run multiple searches, or execute multiple bash commands simultaneously.

### 23.11 Keyboard Shortcuts Mastery

| Shortcut | Action |
|----------|--------|
| `Tab` | Toggle Plan/Build mode |
| `Ctrl+P` | Previous command |
| `Ctrl+N` | Next command |
| `Ctrl+R` | Search history |
| `Ctrl+C` | Cancel |
| `Ctrl+L` | Clear screen |
| `Shift+Enter` | Multi-line input |
| `Ctrl+D` | Exit |

### 23.12 Quick Reference Card

```bash
# Start
opencode                          # Start interactive TUI
opencode run "..."                # One-shot query
opencode web                      # Start web UI

# Configuration
opencode init                     # Setup wizard
opencode agent create             # Create custom agent

# Session
/undo                             # Undo last action
/redo                             # Redo last undone
/compact                          # Compact history
/share                            # Share session

# Files
@filename                         # Attach file via fuzzy search
/attach path/to/file              # Attach file explicitly

# Models
/models                           # List models
/model provider/model-name        # Switch model

# Subagents
@general Write a test             # Use general agent
@explore How does X work          # Explore codebase
@scout Find Y patterns            # Quick search

# Custom
/test                             # Run custom test command
/component MyComponent            # Generate component
```

---

## Appendix A: Troubleshooting

### Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Provider not responding | Check API key, network, and base URL |
| Token limit exceeded | Use `/compact`, reduce context, or switch to larger context model |
| Tool permission denied | Check `permissions` in opencode.json |
| Config not loading | Check file location and JSON validity |
| Agent not following instructions | Update `agents.md` with clearer rules |
| Web interface not accessible | Check `--hostname 0.0.0.0` for external access |

### Debugging

```bash
# Verbose output
opencode --verbose run "..."

# Check config
opencode --version --verbose

# Validate config JSON
opencode config validate
```

---

## Appendix B: Example End-to-End Workflows

### Workflow 1: Adding a New Feature

```bash
# 1. Explore the codebase
opencode
> @explore How are API routes currently structured?
> @explore Find existing middleware patterns

# 2. Plan the implementation
> [Tab -> Plan mode]
> Plan adding a file upload endpoint with S3 storage

# 3. Implement
> [Tab -> Build mode]
> Implement the file upload endpoint as planned

# 4. Write tests
> /test-file upload

# 5. Review changes
> git diff
```

### Workflow 2: Debugging a Production Issue

```bash
# 1. Quick search for the problematic code
opencode run "Find all places where we handle user authentication" --mode plan

# 2. Analyze the issue
opencode
> @scout Find error handling in the auth flow

# 3. Fix with undo protection
> Add proper error logging and a fallback auth mechanism

# 4. Verify
> npm test
```

### Workflow 3: Code Review

```bash
# Using OpenCode in Plan mode for PR review
opencode run "Review the changes in the current branch. Focus on security and performance." --mode plan

# Generate review comments
opencode run "Generate a PR review summary with specific file-by-file feedback" --mode plan
```

---

## Appendix C: Glossary

| Term | Definition |
|------|------------|
| **Agent** | An AI-powered entity with tool access that performs tasks |
| **Build Mode** | Mode where agents can read and write files |
| **Plan Mode** | Read-only mode for analysis and planning |
| **TUI** | Terminal User Interface - the interactive text-based UI |
| **MCP** | Model Context Protocol - standard for connecting AI to external tools |
| **Subagent** | A specialized agent invoked inline with @mention |
| **Tool** | A function an agent can call (read, write, bash, etc.) |
| **Provider** | An LLM service (OpenAI, Anthropic, etc.) |
| **Config** | Configuration file (opencode.json) |
| **Session** | A continuous interaction with undo/redo history |
| **agents.md** | Project-level instructions injected into all agent prompts |
| **Custom Command** | A user-defined shortcut for common tasks |
| **SSE** | Server-Sent Events - transport for remote MCP servers |
| **LLM** | Large Language Model - the AI behind the agent |

---

## Appendix D: Version History

| Version | Date | Key Changes |
|---------|------|-------------|
| 1.0.0 | Q1 2025 | Initial release |
| 1.5.0 | Q2 2025 | Added MCP support, custom agents |
| 2.0.0 | Q3 2025 | Web interface, GitHub integration, Plan/Build modes |
| 2.1.0 | Q4 2025 | Subagents (@general, @explore, @scout), custom commands as markdown files |

---

*This documentation covers OpenCode's features comprehensively. For the latest updates, visit [opencode.ai](https://opencode.ai) or the [GitHub repository](https://github.com/anomalyco/opencode).*
