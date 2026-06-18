# Design System for Consistent UI

You can make OpenCode's agent follow a **design system** by creating a `DESIGN.md` file. This ensures every UI component it generates matches your visual style.

## What is DESIGN.md?

`DESIGN.md` is a reference file in your project root that contains all your design decisions. The agent reads it before creating any UI code.

## What to Include in DESIGN.md

| Section | Example Content |
|---------|----------------|
| **Colors** | Primary: `#2563eb`, Background: `#0f172a` |
| **Typography** | Font: Inter, Headings: 2rem/1.5rem/1.25rem |
| **Spacing** | 4px base unit, 8/16/24/32/48 spacing scale |
| **Border Radius** | `4px` inputs, `8px` cards, `12px` modals |
| **Shadows** | Card: `0 2px 8px rgba(0,0,0,0.1)` |
| **Component Patterns** | Button variants, input styles, layout grid |

## Real Example: Minimal DESIGN.md

```markdown
# DESIGN.md — Design System

## COLORS
| Token | Value | Usage |
|-------|-------|-------|
| `--primary` | `#2563eb` | Buttons, links, active states |
| `--primary-hover` | `#1d4ed8` | Button hover |
| `--bg` | `#0f172a` | Page background |
| `--bg-card` | `#1e293b` | Card / surface background |
| `--text` | `#f8fafc` | Primary text |
| `--text-muted` | `#94a3b8` | Secondary text |
| `--border` | `#334155` | Borders, dividers |
| `--error` | `#ef4444` | Error states |
| `--success` | `#22c55e` | Success states |

## TYPOGRAPHY
- Font family: `Inter`, system-ui, sans-serif
- Headings: Inter Bold
- Body: Inter Regular
- Scale: `12 / 14 / 16 / 20 / 24 / 32 / 48`

## SPACING
- 4px base unit
- Spacing scale: `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64`

## BORDER RADIUS
- `sm`: 4px (inputs, buttons)
- `md`: 8px (cards)
- `lg`: 12px (modals, dialogs)

## COMPONENTS

### Button
- Padding: `8px 16px`
- Border radius: `4px`
- Font: 14px Medium
- Variants: primary (blue), secondary (outline), ghost (transparent)

### Card
- Background: `--bg-card`
- Border: `1px solid --border`
- Radius: `8px`
- Padding: `24px`
- Shadow: `0 2px 8px rgba(0,0,0,0.2)`

### Input
- Background: `--bg`
- Border: `1px solid --border`
- Radius: `4px`
- Padding: `8px 12px`
- Font: 14px
```

## How to Use It

In `AGENTS.md`, reference the design file:

```markdown
## UI DESIGN
- Always follow DESIGN.md for colors, spacing, typography
- Use Tailwind CSS utility classes that match the design tokens
- All new components must match the existing design system
```

Then prompt:

```text
> Create a login page with email/password fields
```

The agent reads `DESIGN.md` and generates:

```tsx
// Agent knows to use --bg-card (#1e293b) for the form container
// Uses --primary (#2563eb) for the submit button
// Applies 8px border radius to inputs
// Uses correct spacing scale
export default function LoginPage() {
  return (
    <main className="min-h-screen bg-[#0f172a] flex items-center justify-center p-4">
      <div className="bg-[#1e293b] border border-[#334155] rounded-lg p-6 w-full max-w-md shadow-lg">
        <h1 className="text-2xl font-bold text-[#f8fafc] mb-6">Sign In</h1>
        <input className="w-full bg-[#0f172a] border border-[#334155] rounded px-3 py-2 text-sm text-[#f8fafc] mb-4" />
        <button className="w-full bg-[#2563eb] hover:bg-[#1d4ed8] text-white rounded px-4 py-2 text-sm font-medium">
          Sign In
        </button>
      </div>
    </main>
  )
}
```

## Benefits

- **Consistency** — every component looks like it belongs together
- **Speed** — no need to explain colors or spacing in every prompt
- **Scalability** — add new components that match existing ones automatically
- **Team alignment** — everyone (including AI) follows the same design language
