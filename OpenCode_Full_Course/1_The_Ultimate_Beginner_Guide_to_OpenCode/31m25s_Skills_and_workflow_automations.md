# Skills and Workflow Automations

Skills are reusable, domain-specific instruction sets that teach OpenCode how to handle particular tasks. They are like plugins for your agents — giving them specialized knowledge for specific domains.

## What Is a Skill?

A **skill** is a packaged set of instructions, rules, and context that an agent can load to become an expert in a specific domain.

Think of skills as:

```text
Agent without skills:  "I know general coding."
Agent with skill:      "I am now an expert in React testing
                       with Vitest and Testing Library."
```

## Creating a Skill

### Directory Structure

Skills live in the `.opencode/skills/` directory:

```
.opencode/
  skills/
    react-testing/
      SKILL.md
    tailwind/
      SKILL.md
    security/
      SKILL.md
```

### SKILL.md Structure

Each skill is defined by a `SKILL.md` file:

**File: `.opencode/skills/react-testing/SKILL.md`**

```markdown
# React Testing Skill

## Description
Expert knowledge of React testing with Vitest and React Testing Library.

## Instructions
- Use Vitest as the test runner.
- Use React Testing Library for component tests.
- Prefer `getByRole` and `findByText` over test IDs.
- Always test user behavior, not implementation details.
- Mock external API calls with MSW (Mock Service Worker).
- Aim for 80%+ coverage on new components.
- Test error states and loading states, not just the happy path.

## Example Patterns

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect } from 'vitest';

describe('LoginForm', () => {
  it('shows error on invalid email', async () => {
    render(<LoginForm />);
    await userEvent.type(screen.getByLabelText('Email'), 'invalid');
    await userEvent.click(screen.getByRole('button', { name: /submit/i }));
    expect(screen.getByText(/invalid email/i)).toBeInTheDocument();
  });
});
```
```

## Configuring Skills in opencode.jsonc

### Associating Skills with Agents

```jsonc
{
  "agents": {
    "frontend-coach": {
      "description": "Frontend development expert",
      "systemPrompt": "You are a senior frontend developer.",
      "permissions": {
        "skills": {
          "react-testing": "allow",
          "tailwind": "allow"
        }
      }
    },
    "security-auditor": {
      "description": "Security code reviewer",
      "systemPrompt": "You review code for vulnerabilities.",
      "permissions": {
        "skills": {
          "security": "allow",
          "owasp": "allow"
        }
      }
    }
  }
}
```

### Skill Permission Levels

| Permission | Effect |
|------------|--------|
| `"allow"` | Agent can load this skill automatically |
| `"ask"` | Agent asks for permission before loading |
| `"deny"` | Agent cannot load this skill |

### Using Skills Inline

You can also load skills manually during a session:

```text
> @general [skill:react-testing] Write tests for the UserProfile component
> @general [skill:security] Audit the authentication flow
```

## Preventing Environment and Data Leaks

When creating skills, follow these security best practices:

### DO NOT Include in Skills

```markdown
<!-- BAD: Never include secrets in skills -->
- API keys
- Database passwords
- Internal URLs with credentials
- Personal access tokens
- Environment variable values
```

### DO This Instead

```markdown
<!-- GOOD: Reference environment variables -->
- Use `process.env.API_KEY` for authentication
- Database URL comes from `process.env.DATABASE_URL`
- Never hardcode secrets — always use env variables
```

### Skill Safety Checklist

```text
[ ] No hardcoded API keys
[ ] No hardcoded passwords
[ ] No internal URLs with tokens
[ ] No Personally Identifiable Information (PII)
[ ] Uses $ENV_VAR references instead of values
[ ] Includes warning about data sensitivity
```

## Advanced Skill Techniques

### Skill Chaining

Skills can reference and load other skills:

```markdown
# Full Stack Skill

## Depends On
- react-testing
- tailwind
- api-design
- database

## Combined Instructions
Use all loaded skills together. When writing a feature:
1. Design the API endpoint (api-design skill).
2. Create the database migration (database skill).
3. Build the React component (react-testing + tailwind skills).
```

### Conditional Skill Loading

Skills can have conditions:

```markdown
# Next.js Skill

## Condition
Only load this skill if the project uses Next.js (check package.json).

## Instructions
- Use App Router for new pages.
- Prefer Server Components by default.
- Use client components only when interactivity is needed.
```

### Skill Variables

Skills can use variables for flexibility:

```markdown
# Component Generator Skill

## Variables
- $COMPONENT_NAME: Name of the component to create
- $TYPE: "function" or "class" component

## Instructions
Create a $TYPE component named $COMPONENT_NAME.
Include TypeScript types, tests, and storybook stories.
```

## Workflow Automations

Combine skills with custom commands for powerful automations:

```jsonc
{
  "customCommands": [
    {
      "name": "new-component",
      "prompt": "[skill:react-testing][skill:tailwind] Create a new React component named $ARGUMENTS with tests and styling.",
      "description": "Generate a new component with tests"
    },
    {
      "name": "security-review",
      "prompt": "[skill:security] Perform a security audit on $ARGUMENTS. Report all findings.",
      "description": "Security audit"
    }
  ]
}
```

Now running:

```text
> /new-component UserProfile
```

...will automatically load both the react-testing and tailwind skills before generating the component.

## Built-in Skills

OpenCode comes with some built-in skills:

| Skill Name | Description |
|------------|-------------|
| `customize-opencode` | Configuring OpenCode itself |
| `general` | General coding knowledge |
| `web-development` | Web dev best practices |

You can override or extend these with your own.

## Summary

```text
Skills = Domain expertise for your agents.

Structure:
  .opencode/skills/<skill-name>/SKILL.md

Steps to create:
  1. Create the directory: .opencode/skills/my-skill/
  2. Create SKILL.md with instructions and examples.
  3. Reference in opencode.jsonc agent permissions.
  4. Load inline with [skill:my-skill].

Best practice:
  - Keep skills focused on one domain.
  - Never hardcode secrets.
  - Use skills for team-wide standards.
  - Combine skills with custom commands.
```
