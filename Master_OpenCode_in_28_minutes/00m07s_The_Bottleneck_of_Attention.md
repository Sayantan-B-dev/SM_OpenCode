# The Bottleneck of Attention (0:07 - 0:13)

## Understanding the Attention Bottleneck

The **bottleneck of attention** refers to the fundamental limitation humans face when working with AI coding tools: you can only focus on one thing at a time. Traditional tools amplify this problem by forcing you to context-switch constantly.

### Why Attention Is the Bottleneck

1. **Limited working memory**: Developers can only hold ~4-7 chunks of information in working memory at once. Each context switch flushes this cache
2. **Context re-building overhead**: Switching between tasks requires re-loading the problem space, re-reading code, and re-explaining the situation to the AI
3. **Sequential waiting waste**: In traditional tools, you ask a question, wait for a response, review it, then ask the next question. This synchronous pattern wastes the AI's potential

### How OpenCode Overcomes the Attention Bottleneck

OpenCode tackles this with **parallel agent architecture**:

- **Divide and conquer**: Break a large task into smaller pieces and assign each to a separate agent session running in parallel
- **Root session orchestration**: One main terminal tracks all sub-sessions, giving you a dashboard view of progress across all agents
- **Shared context through files**: `AGENTS.md`, `.agent/` folders, and `DESIGN.md` act as shared memory — every agent writes its progress so others can read and understand
- **Model specialization**: Use different models for different types of work (e.g., a powerful reasoning model for planning, a fast cheap model for bulk refactoring)

### The Concept of "Attention as a Service"

By decoupling your attention from the AI's execution, OpenCode lets you:
- Start multiple agents on different tasks simultaneously
- Check in on progress periodically instead of waiting synchronously
- Have agents coordinate with each other through shared files and memory systems

This transforms coding from a **linear, sequential process** into a **parallel, orchestrated workflow** — overcoming the bottleneck of attention entirely.
