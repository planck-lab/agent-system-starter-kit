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
status: open | in-progress | closed
by: agent-a                          # who wrote this
date: 2026-03-24
topic: short-description             # optional, for filtering
in-reply-to: signals/original.md     # optional, for responses
confidence: high | medium | low      # optional, for learnings
proposed-action: pattern | rule | fix # optional, for learnings
---

[Content in markdown]
```

> **Note:** The `by` + `status` pattern is intentionally minimal. Add fields as needed — the only required ones are `status`, `by`, and `date`. See [examples/exchange-session.md](../examples/exchange-session.md) for a real session.

### Rules

1. **Never modify another agent's files** — only add new files
2. **Always include `by` and `status`** — so agents know who wrote it and whether it needs action
3. **Close signals when done** — update status to `closed`
4. **Keep files small** — one topic per file
5. **Pull before push** — avoid conflicts

### Why Git?

- **Auditable:** Full history of all agent communication
- **Resilient:** Works offline, syncs when connected
- **Simple:** No protocol negotiation, just markdown files
- **Familiar:** Every developer knows Git
- **Reversible:** Bad exchanges can be reverted
