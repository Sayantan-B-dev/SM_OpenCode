# Scaffolding a Next.js Project with OpenCode

OpenCode can scaffold an entire Next.js project from scratch — including routing, layouts, components, and styling — just by prompting.

## Key Commands for Session Management

| Command | What It Does | Example |
|---------|-------------|---------|
| `/session` | Shows current session info (provider, model, context usage) | `/session` → shows model, tokens used |
| `/new` | Starts a **fresh session** (clears conversation history) | `/new` → "Starting new session..." |
| `/exit` | Exits OpenCode | `/exit` → back to terminal |

## How to Prompt for a New Project

Be specific about:
- **Framework** (Next.js, React, Vite, etc.)
- **Language** (TypeScript recommended)
- **Styling** (Tailwind, CSS Modules, styled-components)
- **Features** (auth, database, API routes)
- **Structure** (App Router vs Pages Router)

## Real Example: Scaffolding a Next.js App

```text
> /new
Starting fresh session.

> Create a Next.js 14 project with App Router and TypeScript.
> It should have:
>   - A landing page with a hero section
>   - A /about page
>   - Tailwind CSS for styling
>   - A shared Header and Footer component in /components
>   - Dark mode toggle using next-themes
```

OpenCode will ask clarifying questions, then create all files:

```text
✓ Created next.config.js
✓ Created tailwind.config.ts
✓ Created app/layout.tsx
✓ Created app/page.tsx
✓ Created app/about/page.tsx
✓ Created components/Header.tsx
✓ Created components/Footer.tsx
✓ Created components/ThemeToggle.tsx
✓ Created package.json
```

## Using /session to Track Progress

```text
> /session

Current Session:
  Provider: OpenCode Zen
  Model: gpt-4o-mini
  Messages: 12
  Tokens Used: 3,421 / 128,000

> /new
Starting fresh... (tokens reset)
```

## Pro Tip: Use PROMPT.md for Large Scaffolds

Create a `PROMPT.md` file with your full requirements. Then reference it:

```text
> Read PROMPT.md and scaffold this entire project
```

OpenCode will use the file as instructions — saves you retyping everything.
