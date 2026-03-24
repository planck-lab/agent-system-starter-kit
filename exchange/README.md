# Agent Exchange

## How It Works

This directory enables asynchronous communication between multiple agents running on different machines.

### Flow

```
Agent A                          Agent B
  │                                │
  ├── git push ──────────────────► │
  │   signals/task-request.md      │
  │                                ├── git pull
  │                                ├── reads signal
  │                                ├── works on task
  │                                ├── git push
  │ ◄──────────────────────────────┤
  │   responses/task-response.md   │
  ├── git pull                     │
  ├── reads response               │
  └── continues work               │
```

### Directory Structure

```
exchange/
├── signals/      # Requests from any agent to any other
├── questions/    # Questions that need human or agent input
└── responses/    # Answers to signals or questions
```

### File Format

Each exchange file uses YAML frontmatter:

```yaml
---
from: agent-a
to: agent-b
type: signal | question | response
status: open | in-progress | closed
date: 2026-03-24
ref: signal-001  # (for responses: reference to original signal)
---

[Content in markdown]
```

### Rules

1. **Never modify another agent's files** — only add new files
2. **Always include `from` and `to`** — so agents know what's for them
3. **Close signals when done** — update status to `closed`
4. **Keep files small** — one topic per file
5. **Pull before push** — avoid conflicts

### Why Git?

- **Auditable:** Full history of all agent communication
- **Resilient:** Works offline, syncs when connected
- **Simple:** No protocol negotiation, just markdown files
- **Familiar:** Every developer knows Git
- **Reversible:** Bad exchanges can be reverted
