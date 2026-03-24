# Architecture

## System Overview

This agent system follows a **layered architecture** with clear separation of concerns:

```
Layer 4: Skills (WHAT the agent can do)
Layer 3: Memory (WHAT the agent knows)
Layer 2: Behavior (HOW the agent acts)
Layer 1: Identity (WHO the agent is)
```

Each layer builds on the one below. You can change skills without touching identity. You can adjust behavior without rewriting skills.

## Layer 1: Identity (SOUL.md + IDENTITY.md)

The foundation. Defines:
- Name, personality, communication style
- Language preferences
- Hard boundaries (what the agent will NEVER do)
- Team context (who else is involved)

**Why it matters:** Without a clear identity, the agent's behavior is unpredictable across sessions. The identity layer ensures consistency.

**Anti-pattern:** Putting behavior rules in SOUL.md. Identity is about WHO, not HOW.

## Layer 2: Behavior (AGENTS.md + TOOLS.md)

The operating system. Defines:
- How to process incoming messages
- Skill routing (intent → skill file)
- Delegation rules (when to spawn sub-agents)
- Security constraints (locked paths, protected files)
- Tool permissions (what shell commands are allowed)
- Feedback loop (how to learn from corrections)

**Why it matters:** This is where most customization happens. A well-configured AGENTS.md can turn a generic LLM into a specialized teammate.

**Anti-pattern:** Making AGENTS.md too long (>300 lines). If it's too complex, the agent will ignore parts of it.

## Layer 3: Memory (MEMORY.md + memory/)

The context. Three tiers:

### Working Memory (conversation context)
- Managed by the gateway/orchestrator
- Includes recent messages, tool outputs, system context
- Lost when the session ends

### Daily Notes (memory/YYYY-MM-DD.md)
- Append-only daily log
- Categories: IDEA, DECISION, LEARNING, TODO, FACT, PROJECT, NOTE
- Searchable via grep

### Long-term Memory (MEMORY.md)
- Compact summary of persistent facts
- User preferences, active projects, system config
- Updated rarely, kept under 2000 characters

**Why it matters:** Without memory, every conversation starts from zero. With memory, the agent builds context over time.

**Anti-pattern:** Storing everything in MEMORY.md. It should be a summary, not a diary.

## Layer 4: Skills (skills/)

The capabilities. Each skill is:
- A `SKILL.md` file with instructions
- Optional helper scripts
- A trigger pattern (how the agent knows to use this skill)

Skills are **composable** — the agent can chain multiple skills in one task.

**Why it matters:** Skills make the agent extensible without changing core configuration.

**Anti-pattern:** Skills that modify SOUL.md or AGENTS.md. Skills should be sandboxed.

## The Gateway

The gateway is the orchestrator that ties everything together:
- Receives messages from messaging platforms
- Routes to the right agent/model
- Manages conversation sessions
- Enforces approval policies (HITL)
- Handles sub-agent spawning

In this system, [OpenClaw](https://github.com/openclaw/openclaw) serves as the gateway. But the architecture works with any orchestrator that can:
1. Route messages to an LLM
2. Inject workspace files as context
3. Execute tool calls
4. Manage sessions

## Multi-Agent Communication

See `exchange/` for the Git-based pattern. The key insight:

**Agents don't need real-time connections.** Asynchronous communication via files in a shared repository is:
- Auditable (git log)
- Resilient (no connection = no problem, just delayed)
- Simple (no protocol negotiation, just markdown files)
- Version-controlled (you can revert bad exchanges)

## Design Decisions

| Decision | Why |
|----------|-----|
| Markdown for everything | Human-readable, version-controllable, no special tooling |
| Append-only daily memory | Prevents accidental overwrites, natural chronology |
| Skills as files, not code | Can be reviewed, version-controlled, shared |
| Git for multi-agent communication | Async, auditable, works offline |
| Strict security boundaries | Prevents accidental exposure of secrets |
