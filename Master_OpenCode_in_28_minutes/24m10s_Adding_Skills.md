# Adding Skills (24:10 - 25:14)

## What Are Skills?

Skills are **reusable instruction sets** that OpenCode agents can load on-demand. Think of them as specialized knowledge packages — when an agent encounters a task that matches a skill, it can load that skill to get detailed, context-specific instructions.

### Skills vs MCPs

| Feature | Skills | MCPs |
|---------|--------|------|
| What they provide | Instructions/knowledge | External tools/APIs |
| How they work | Loaded as text into prompt | Exposed as callable tools |
| When they load | On-demand when relevant | On startup (if enabled) |
| Context cost | Only when loaded | Always (if enabled) |

### Skills vs Rules

| Feature | Skills | Rules |
|---------|--------|-------|
| Scope | Specific, situational | General, always active |
| Loading | On-demand | Always in context |
| Use case | "How to release a package" | "Always use TypeScript strict mode" |

## Skill Structure

Each skill is defined in a `SKILL.md` file with YAML frontmatter.

### File Location

Place skills in one of these directories:
```
# Project-level
.opencode/skills/<skill-name>/SKILL.md
.claude/skills/<skill-name>/SKILL.md
.agents/skills/<skill-name>/SKILL.md

# Global (available to all projects)
~/.config/opencode/skills/<skill-name>/SKILL.md
~/.claude/skills/<skill-name>/SKILL.md
~/.agents/skills/<skill-name>/SKILL.md
```

### SKILL.md Structure

```markdown
---
name: git-release
description: Create consistent releases and changelogs
license: MIT
compatibility: opencode
metadata:
  audience: maintainers
  workflow: github
---

## What I Do
- Draft release notes from merged PRs
- Propose a version bump
- Provide a copy-pasteable `gh release create` command

## When to Use Me
Use this when you are preparing a tagged release.
Ask clarifying questions if the target versioning scheme is unclear.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Skill name (1-64 chars, lowercase alphanumeric with hyphens) |
| `description` | Yes | Description (1-1024 chars, shown in agent tool list) |
| `license` | No | License identifier |
| `compatibility` | No | Compatible platforms |
| `metadata` | No | Custom string-to-string key/value pairs |

### Name Validation Rules

Skill names must:
- Be 1–64 characters
- Use only lowercase alphanumeric characters and hyphens
- Not start or end with a hyphen
- Not contain consecutive hyphens
- Match the directory name containing `SKILL.md`

Regex: `^[a-z0-9]+(-[a-z0-9]+)*$`

## Adding Skills via CLI

```bash
# List available skills
opencode skill list

# Install a skill from a URL or registry
opencode skill install <name>

# Create a new skill (opens an editor)
opencode skill create <name>

# Remove a skill
opencode skill remove <name>
```

## How Agents Use Skills

When an agent is working, it sees available skills through the `skill` tool:

```
<available_skills>
  <skill>
    <name>git-release</name>
    <description>Create consistent releases and changelogs</description>
  </skill>
  <skill>
    <name>nextjs-portfolio</name>
    <description>Create Next.js portfolio with best practices</description>
  </skill>
</available_skills>
```

The agent calls the skill tool when it decides a skill is relevant:
```
skill({ name: "git-release" })
```

This loads the full `SKILL.md` content into the agent's context.

## Skill Permissions

Control which skills agents can access:

```json
{
  "permission": {
    "skill": {
      "*": "allow",
      "pr-review": "allow",
      "internal-*": "deny",
      "experimental-*": "ask"
    }
  }
}
```

| Permission | Behavior |
|------------|----------|
| `allow` | Skill loads immediately |
| `deny` | Skill hidden from agent, access rejected |
| `ask` | User prompted for approval before loading |

## Per-Agent Skill Overrides

Custom agents can override global skill permissions:

```markdown
---
permission:
  skill:
    "documents-*": "allow"
---
```

Built-in agents configured in `opencode.json`:

```json
{
  "agent": {
    "plan": {
      "permission": {
        "skill": {
          "internal-*": "allow"
        }
      }
    }
  }
}
```

## Disabling Skills

Completely disable the skill tool for agents that shouldn't use it:

```json
{
  "agent": {
    "build": {
      "tools": {
        "skill": false
      }
    }
  }
}
```

## Troubleshooting

If a skill doesn't show up:
1. Verify the filename is `SKILL.md` (case-sensitive, all caps)
2. Check frontmatter includes both `name` and `description`
3. Ensure skill names are unique across all locations
4. Check permissions — skills with `deny` are hidden from agents
5. Verify the directory name matches the skill name
