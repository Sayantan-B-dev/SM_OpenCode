# Building an Application

This section walks through a complete end-to-end demo: building a full application using OpenCode from start to finish. Follow along to see how all the pieces fit together.

## The Project: Task Management API

We will build a Task Management API with:

- Node.js + Express
- TypeScript
- SQLite database (via better-sqlite3)
- Zod validation
- RESTful endpoints
- Unit tests with Vitest

## Step 1: Initialize the Project

```bash
# Create project directory
mkdir task-manager-api
cd task-manager-api

# Initialize OpenCode
opencode init --provider openai --model gpt-4o

# Start the TUI
opencode
```

## Step 2: Plan the Application (Plan Mode)

Press `Tab` to switch to Plan mode. Then:

```text
> Plan a Task Management API with the following requirements:
  - Node.js + Express + TypeScript
  - SQLite database with better-sqlite3
  - CRUD endpoints for tasks (Create, Read, Update, Delete)
  - Zod validation for all inputs
  - Error handling middleware
  - Unit tests with Vitest
  - Each task has: id, title, description, status (todo/in-progress/done), createdAt, updatedAt
```

### Expected Plan Output

The agent will produce a plan like:

```text
## Proposed Structure

task-manager-api/
  src/
    index.ts           - Entry point, server setup
    app.ts             - Express app configuration
    db.ts              - Database setup and queries
    routes/
      tasks.ts         - Task route handlers
    middleware/
      errorHandler.ts  - Global error handler
      validation.ts    - Zod validation middleware
    schemas/
      task.ts          - Zod schemas for tasks
  tests/
    tasks.test.ts      - Integration tests
  package.json
  tsconfig.json
  vitest.config.ts

## Implementation Order
1. Project setup (package.json, tsconfig.json)
2. Database setup (db.ts)
3. Zod schemas (schemas/task.ts)
4. Routes (routes/tasks.ts)
5. Middleware (middleware/)
6. App and entry point (app.ts, index.ts)
7. Tests (tests/tasks.test.ts)
```

Review the plan. If it looks good, proceed.

## Step 3: Implement the Project (Build Mode)

Press `Tab` to switch to Build mode. Then:

```text
> Implement the Task Management API as planned. Start with the project setup.
```

### What the Agent Will Do

The agent will create all files step by step:

```typescript
// src/db.ts
import Database from 'better-sqlite3';
import path from 'path';

const db = new Database(path.join(__dirname, '../data/tasks.db'));

db.exec(`
  CREATE TABLE IF NOT EXISTS tasks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    description TEXT DEFAULT '',
    status TEXT DEFAULT 'todo' CHECK(status IN ('todo', 'in-progress', 'done')),
    createdAt TEXT DEFAULT (datetime('now')),
    updatedAt TEXT DEFAULT (datetime('now'))
  )
`);

export default db;
```

```typescript
// src/schemas/task.ts
import { z } from 'zod';

export const createTaskSchema = z.object({
  title: z.string().min(1).max(200),
  description: z.string().max(1000).optional().default(''),
});

export const updateTaskSchema = z.object({
  title: z.string().min(1).max(200).optional(),
  description: z.string().max(1000).optional(),
  status: z.enum(['todo', 'in-progress', 'done']).optional(),
});

export type CreateTaskInput = z.infer<typeof createTaskSchema>;
export type UpdateTaskInput = z.infer<typeof updateTaskSchema>;
```

```typescript
// src/routes/tasks.ts
import { Router, Request, Response } from 'express';
import db from '../db';
import { createTaskSchema, updateTaskSchema } from '../schemas/task';

const router = Router();

// GET /api/tasks
router.get('/', (_req: Request, res: Response) => {
  const tasks = db.prepare('SELECT * FROM tasks ORDER BY createdAt DESC').all();
  res.json(tasks);
});

// GET /api/tasks/:id
router.get('/:id', (req: Request, res: Response) => {
  const task = db.prepare('SELECT * FROM tasks WHERE id = ?').get(req.params.id);
  if (!task) return res.status(404).json({ error: 'Task not found' });
  res.json(task);
});

// POST /api/tasks
router.post('/', (req: Request, res: Response) => {
  const parsed = createTaskSchema.parse(req.body);
  const result = db.prepare(
    'INSERT INTO tasks (title, description) VALUES (?, ?)'
  ).run(parsed.title, parsed.description);
  const task = db.prepare('SELECT * FROM tasks WHERE id = ?').get(result.lastInsertRowid);
  res.status(201).json(task);
});

// PUT /api/tasks/:id
router.put('/:id', (req: Request, res: Response) => {
  const parsed = updateTaskSchema.parse(req.body);
  const existing = db.prepare('SELECT * FROM tasks WHERE id = ?').get(req.params.id);
  if (!existing) return res.status(404).json({ error: 'Task not found' });

  const updated = { ...existing, ...parsed, updatedAt: new Date().toISOString() };
  db.prepare(`
    UPDATE tasks SET title=?, description=?, status=?, updatedAt=? WHERE id=?
  `).run(updated.title, updated.description, updated.status, updated.updatedAt, req.params.id);
  res.json(updated);
});

// DELETE /api/tasks/:id
router.delete('/:id', (req: Request, res: Response) => {
  const result = db.prepare('DELETE FROM tasks WHERE id = ?').run(req.params.id);
  if (result.changes === 0) return res.status(404).json({ error: 'Task not found' });
  res.status(204).send();
});

export default router;
```

## Step 4: Install Dependencies

```text
> Install the project dependencies and set up TypeScript
```

The agent runs:

```bash
npm init -y
npm install express better-sqlite3 zod
npm install -D typescript @types/express @types/better-sqlite3 vitest tsx
```

## Step 5: Run Tests

```text
> Run the tests and fix any failures
```

The agent will run `vitest run`, analyze any failures, and fix them.

## Step 6: Verify the API

```text
> Start the development server and test the endpoints
```

The agent starts the server and runs curl commands to verify:

```bash
# Start server
npx tsx src/index.ts

# Test endpoints
curl http://localhost:3000/api/tasks
curl -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "My first task"}'
```

## Step 7: Review What Was Created

```text
> /review-recent
```

This shows all files created and modified during the session.

## Step 8: Add a Feature

```text
> Add pagination to the GET /api/tasks endpoint.
  Support ?page=1&limit=10 query parameters.
```

The agent modifies the route handler to support pagination.

## Step 9: Undo If Needed

```text
> /undo
```

If the pagination was not what you wanted, undo it.

## Complete Workflow Summary

```text
Step 1:  opencode init
Step 2:  opencode (start TUI)
Step 3:  [Plan mode] Plan the application
Step 4:  Review the plan
Step 5:  [Build mode] Implement the application
Step 6:  Install dependencies
Step 7:  Run tests and fix failures
Step 8:  Verify the API works
Step 9:  Add features iteratively
Step 10: Use /undo if needed
```

## Key Takeaways

```text
- Always plan before building (Plan mode).
- Review the plan carefully before implementing.
- Let the agent implement step by step.
- Run tests after each major change.
- Use /undo freely — every action is reversible.
- Build features incrementally, not all at once.
```
