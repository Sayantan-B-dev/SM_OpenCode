# Auto-Testing the App with Browser MCP

With Playwright MCP connected, OpenCode can **automatically test your app** — clicking through flows, verifying UI, and reporting issues.

## How It Works

1. You start your dev server
2. OpenCode connects to it via Playwright MCP
3. You describe a test scenario
4. The agent launches a browser, follows your instructions, and reports results

## Basic Test Scenarios

### 1. Visual Verification

```text
> Open http://localhost:3000 and take a screenshot.
> Verify the hero section has a heading "Welcome" and a CTA button.
```

The agent will:
- Navigate to the URL
- Take a screenshot
- Check for the heading element
- Check for the button element
- Report pass/fail

### 2. Form Submission Testing

```text
> Test the contact form at /contact:
> 1. Fill in name: "John Doe"
> 2. Fill in email: "john@example.com"
> 3. Fill in message: "Hello, this is a test"
> 4. Click Submit
> 5. Verify success message appears
```

### 3. Responsive Design Check

```text
> Test the landing page at different viewports:
> - Desktop (1440x900)
> - Tablet (768x1024)
> - Mobile (375x812)
> Take screenshots at each size
> Confirm navigation menu adapts correctly (hamburger menu on mobile)
```

## Using Design References

You can give the agent a **visual reference** for what the UI should look like:

```text
> Here's a design mockup: [drag-and-drop image file]
> Compare the current app at http://localhost:3000 to this design
> Identify differences in:
> - Colors
> - Layout
> - Typography
> - Spacing
```

The agent opens the app, takes screenshots, and compares them against your reference.

## Real Example: Full Test Suite

```text
> Run a full test flow on the Project Planner app:

Test 1: Landing Page
  - Open http://localhost:3000
  - Verify hero section exists
  - Verify input field is present

Test 2: Generate a Plan
  - Type "Build a todo app" in the input
  - Click "Generate"
  - Wait for results
  - Verify a plan card appears

Test 3: Edit a Task
  - Click on a task in the plan
  - Verify edit dialog opens
  - Change the task title
  - Save
  - Verify the title updated

Test 4: Drag and Drop
  - Drag a task from "To Do" to "In Progress"
  - Refresh the page
  - Verify the task is still in "In Progress" (persistence)

Report all results in a table.
```

## Combining with MCP Scripts

Save common test flows as scripts:

```json
// opencode.json
{
  "mcpServers": {
    "playwright": { ... }
  },
  "scripts": {
    "test-login": "Open http://localhost:3000/login, fill credentials, submit, verify redirect to dashboard",
    "test-signup": "Open http://localhost:3000/signup, fill form, submit, verify success toast"
  }
}
```

Then run them with:

```text
> /script test-login
```

## Pro Tips

- **Start your dev server first** — OpenCode can't test a dead URL
- **Use `--headed` mode** — watch the agent navigate for debugging
- **Combine with skills** — the testing skill can define standard test patterns
- **Always check `/mcps`** — verify Playwright is connected before asking for browser actions
- **Be specific** — "Click the blue button labeled 'Submit'" works better than "Click the submit button"
