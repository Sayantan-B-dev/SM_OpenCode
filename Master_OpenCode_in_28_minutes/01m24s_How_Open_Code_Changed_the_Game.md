# How OpenCode Changed the Game (1:24 - 1:41)

## Multiple Sessions with Multiple Models

The game-changing insight of OpenCode is simple but profound: **you don't have to choose one model, one task, or one conversation**.

### Before OpenCode

Developers had to pick a single AI tool and stick with it:
- "Should I use Claude for this or GPT-4?"
- "I need to refactor the backend, write tests, and update docs — but I can only do one at a time"
- "I wish I could have the planning model talk to the coding model"

### After OpenCode

OpenCode removes all these limitations:

#### Parallel Sessions
Start **multiple independent sessions** running in parallel, all on the same project:
```
Session 1 (Claude Opus):   Architecture planning for new feature
Session 2 (GPT-4o):        Implementation of REST endpoints
Session 3 (Claude Haiku):  Writing unit tests
Session 4 (DeepSeek):      Documentation and API references
```

#### Model Specialization
Different models excel at different tasks. OpenCode lets you match the model to the job:

| Task | Best Model Type | Why |
|------|----------------|-----|
| Architecture & planning | Powerful reasoning (Claude Opus, GPT-5) | Deep thinking, complex trade-offs |
| Implementation | Balanced (GPT-4o, Claude Sonnet) | Good code quality, reasonable speed |
| Testing & refactoring | Fast/cheap (Claude Haiku, DeepSeek) | High volume, lower risk |
| Bulk changes | Cheap models | Thousands of lines, low complexity |

#### Orchestration Without Overhead
The root terminal session acts as your **command center**:
- Launch new agent sessions with specific instructions
- Monitor progress across all sessions
- Review results as they come in
- Keep the big picture without losing details

This is the paradigm shift: from **sequential single-model coding** to **parallel multi-model orchestration**.
