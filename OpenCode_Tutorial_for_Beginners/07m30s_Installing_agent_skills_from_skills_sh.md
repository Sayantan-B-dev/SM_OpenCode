# Installing Agent Skills from skills.sh

**Skills** are pre-built instruction packs that teach OpenCode's agent how to work with specific frameworks, libraries, or patterns.

## What is skills.sh?

[skills.sh](https://skills.sh) is a **marketplace of community-contributed skills**. You pick a skill (e.g., Next.js, shadcn, Tailwind), install it, and OpenCode instantly knows the conventions, best practices, and common patterns for that technology.

## How to Install a Skill

### Step 1: Browse Skills

```text
Inside OpenCode, run:

> /skills

This shows all currently installed skills.
```

### Step 2: Find a Skill on skills.sh

Open [skills.sh](https://skills.sh) in your browser. Popular skills include:

| Skill | npx Command | What It Teaches |
|-------|-------------|-----------------|
| **Next.js** | `npx skills sh nextjs` | App Router, layouts, data fetching |
| **shadcn/ui** | `npx skills sh shadcn` | Component patterns, theming |
| **Vercel AI SDK** | `npx skills sh aisdk` | AI streaming, tool calling |
| **Tailwind CSS** | `npx skills sh tailwind` | Utility classes, responsive design |
| **React** | `npx skills sh react` | Hooks, state, performance |

### Step 3: Install the Skill

In a **separate terminal** (or a new terminal tab), run:

```bash
npx skills sh nextjs
```

This creates a `.agents` folder in your project with the skill files.

### Step 4: Verify Installation

Back in OpenCode:

```text
> /skills

Installed skills:
  nextjs     - Next.js App Router & best practices
  tailwind   - Tailwind CSS patterns
```

If nothing shows, run `/exit` and reopen OpenCode.

## The `.agents` Folder Structure

After installing skills, your project will have:

```
my-project/
├── .agents/
│   ├── skills/
│   │   ├── nextjs.md          # Next.js conventions
│   │   ├── tailwind.md        # Tailwind conventions
│   │   └── shadcn.md          # shadcn/ui patterns
│   └── plans/                 # Saved plans (optional)
```

OpenCode reads these files and the agent follows the instructions inside.

## Real Example: Using a Next.js Skill

After installing the Next.js skill, prompt:

```text
> Create a blog with dynamic routes using App Router
```

The agent already knows:

- File-based routing: `app/blog/[slug]/page.tsx`
- Layout nesting: `app/blog/layout.tsx`
- Data fetching patterns: `fetch()` in Server Components
- Loading states: `app/blog/loading.tsx`

Without the skill, you'd have to explain all of this manually.

## Pro Tips

- Install **multiple skills** — they stack
- Update skills with `npx skills update`
- Create your **own custom skills** — just write markdown in `.agents/skills/`
- Skills install to the **current working directory**, so run the npx command from your project root
