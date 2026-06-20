# Other Integrations: GitHub, LSP, and ACP

This section covers three important integrations that extend OpenCode's capabilities: GitHub Actions integration, Language Server Protocol (LSP) support, and ACP (Auto Commit and Push) workflows.

## GitHub Integration

OpenCode can integrate with GitHub to automate PR reviews, checks, and repository management.

### GitHub Actions Setup

#### Step 1: Install the GitHub Integration

```bash
opencode github install
```

This authenticates OpenCode with GitHub and sets up the necessary permissions.

#### Step 2: Configure with Provider

```bash
opencode github install --provider openai --model gpt-4o
```

#### Step 3: Add API Key to GitHub Secrets

Go to your repository: `Settings > Secrets and variables > Actions`

Add a new repository secret:

```text
Name: OPENCODE_ZEN_API_KEY
Value: your-opencode-api-key
```

#### Step 4: Create Workflow Files

OpenCode generates two workflow files:

**File: `.github/workflows/opencode.yml`**

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

**File: `.github/workflows/opencode-pr-check.yml`**

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
            - Ensure test coverage meets minimum
            - Validate documentation updates
```

### How the PR Check Works

```text
1. Developer opens a PR on GitHub.
2. GitHub Actions triggers the OpenCode workflow.
3. OpenCode analyzes the PR diff in Plan mode (read-only).
4. Results are posted as a PR comment.
5. Labels are added (approved, needs-changes, security-concern).
6. The check passes or fails based on findings.
```

### Using OpenCode in Custom GitHub Actions

You can also use OpenCode directly in your own workflows:

```yaml
name: Automated Code Quality

on: [push]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install OpenCode
        run: npm install -g @opencode/core
      - name: Run OpenCode checks
        run: |
          opencode run "Check for deprecated API usage" --no-interactive --mode plan
          opencode run "Verify all files follow our coding standards" --no-interactive --mode plan
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

## LSP (Language Server Protocol) Support

OpenCode uses LSP to provide IDE-like intelligence within the terminal.

### What LSP Provides

| Feature | Description | Example |
|---------|-------------|---------|
| Go to Definition | Jump to where a symbol is defined | `lsp.definition("auth.ts:42")` |
| Find References | Find all usages of a symbol | `lsp.references("auth.ts:15")` |
| Hover Info | Get type information and docs | `lsp.hover("Button.tsx:23")` |
| Code Completion | Suggest completions at a position | `lsp.completions("app.ts:10")` |
| Diagnostics | Show errors and warnings | `lsp.diagnostics("app.ts")` |

### Configuring LSP

OpenCode automatically detects and uses LSP servers for your project. To customize:

```jsonc
{
  "lsp": {
    "enabled": true,
    "servers": {
      "typescript": {
        "command": "typescript-language-server",
        "args": ["--stdio"]
      },
      "eslint": {
        "command": "vscode-eslint-language-server",
        "args": ["--stdio"]
      }
    }
  }
}
```

### Supported Languages

OpenCode supports LSP for many languages:

| Language | LSP Server |
|----------|------------|
| TypeScript / JavaScript | typescript-language-server |
| Python | pyright / pylsp |
| Rust | rust-analyzer |
| Go | gopls |
| Java | eclipse-jdtls |
| Ruby | solargraph |
| PHP | intelephense |
| HTML / CSS | vscode-html-languageserver / vscode-css-languageserver |
| JSON | vscode-json-languageserver |

### LSP in Action

When you ask OpenCode about code, it can use LSP behind the scenes:

```text
> @explore What is the return type of the getUsers function?
  Agent uses LSP to get hover info → returns the type.

> @scout Find all references to the UserModel class
  Agent uses LSP to find references → lists all locations.

> @general Rename the userId variable to authorId
  Agent uses LSP to find all references → renames them all.
```

### Custom LSP Configuration

For detailed LSP configuration options:

```text
https://opencode.ai/docs/lsp
```

## ACP (Auto Commit and Push)

ACP stands for **Auto Commit and Push** — a workflow where OpenCode automatically commits and pushes changes after making modifications.

### How ACP Works

```text
1. Agent makes changes to files.
2. Agent runs: git add .
3. Agent runs: git commit -m "description of changes"
4. Agent runs: git push
```

### Enabling ACP

ACP can be triggered with a custom command:

```jsonc
{
  "customCommands": [
    {
      "name": "acp",
      "prompt": "Make the following changes, then commit and push them with a descriptive message.\n\n$ARGUMENTS",
      "description": "Auto Commit and Push changes"
    }
  ]
}
```

### Using ACP

```text
> /acp Add rate limiting to the API and update the documentation
```

The agent will:

1. Implement the rate limiting.
2. Update the documentation.
3. Run `git add .`
4. Create a commit: "Add rate limiting to API and update docs"
5. Push to the remote repository.

### ACP Safety

```text
WARNING: ACP will push changes without review.
Always review the diff first or use Plan mode before ACP.
```

**Safer approach:** Review first, then ACP.

```text
> [Plan mode] Plan the implementation of rate limiting
Review the plan.
> [Build mode] Implement the plan
Review the diff.
> /acp Commit and push the rate limiting implementation
```

### Partial ACP

You can also ACP specific files:

```jsonc
{
  "customCommands": [
    {
      "name": "commit",
      "prompt": "Commit the changes to $ARGUMENTS with a descriptive message. Do not push.",
      "description": "Commit specific files"
    }
  ]
}
```

## Integration Summary

```text
GitHub:
  - Auto PR reviews with OpenCode checks.
  - Run in CI/CD pipelines.
  - Store API key in GitHub Secrets.

LSP:
  - IDE-like intelligence in terminal.
  - Go to definition, find references, hover info.
  - Language support for JS/TS, Python, Rust, Go, etc.

ACP:
  - Auto Commit and Push workflow.
  - Use with caution — review before pushing.
  - Customize commit messages.
```
