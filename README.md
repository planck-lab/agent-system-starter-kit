# 🏗️ Agent System Starter Kit

**A reference architecture for personal AI agent systems.**
Study it. Understand it. Then decide if you want to build your own.

> This is not a product. It's an anatomy lesson — a real, working agent system, anonymized and documented so you can understand how the pieces fit together.

---

## What This Is

This repository shows the architecture of a **real personal AI agent system** that has been running in production for months. It handles daily workflows: research, note-taking, system monitoring, content creation, and multi-agent collaboration.

The code and configurations here are **anonymized** — no API keys, no personal data, no secrets. What remains is the structure, the patterns, and the decisions behind them.

## What This Is NOT

- ❌ Not a framework (use LangChain, CrewAI, or OpenClaw for that)
- ❌ Not a tutorial (the [Agent Literacy course](https://planck-lab.github.io/agent-literacy/) explains the concepts)
- ❌ Not production-ready out of the box (you'll need your own API keys and infrastructure)

## Architecture Overview

```
┌─────────────────────────────────────────────┐
│                  YOU (Human)                 │
│         Telegram / Signal / Discord          │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│              GATEWAY (Orchestrator)           │
│  Routes messages, manages sessions,          │
│  enforces policies, handles approvals        │
└──────┬───────────┬───────────┬──────────────┘
       │           │           │
       ▼           ▼           ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Agent   │ │  Agent   │ │  Agent   │
│  (Main)  │ │(Sub-Agent)│ │(Sub-Agent)│
│          │ │ Research  │ │ Review   │
└──────────┘ └──────────┘ └──────────┘
       │           │           │
       ▼           ▼           ▼
┌─────────────────────────────────────────────┐
│                WORKSPACE                     │
│  SOUL.md │ AGENTS.md │ memory/ │ skills/    │
│  Identity │ Behavior  │ Context │ Capabilities│
└─────────────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────┐
│              EXTERNAL TOOLS                  │
│  Shell │ Web Search │ Vault │ Calendar │ Git │
└─────────────────────────────────────────────┘
```

## The Four Layers

| Layer | What it does | Key files |
|-------|-------------|-----------|
| **Identity** | Who is this agent? Personality, language, boundaries | `SOUL.md`, `IDENTITY.md` |
| **Behavior** | How does it act? Rules, routing, delegation | `AGENTS.md`, `TOOLS.md` |
| **Memory** | What does it remember? Short-term, long-term, learned rules | `memory/`, `MEMORY.md` |
| **Skills** | What can it do? Reusable capability definitions | `skills/` |

## Key Patterns

### 1. The Agent Loop
Every interaction follows: **Perceive → Think → Act → Reflect**

The agent receives a message, decides what to do (route to a skill? ask a clarifying question? delegate to a sub-agent?), acts, and optionally reflects on the result.

### 2. Human-in-the-Loop (HITL)
Not a feature — a spectrum:

| Level | Metaphor | What happens |
|-------|----------|-------------|
| 🟢 Full Control | Driving instructor | Agent asks before every action |
| 🟡 Supervised | Co-pilot | Agent acts, human reviews |
| 🟠 Autonomous | Solo driver | Agent acts independently, reports results |
| 🔴 Monitored | Dashcam | Agent runs on schedule, human checks logs |

This system runs mostly at 🟡/🟠 — the human reviews research outputs and approves destructive actions, but routine tasks run autonomously.

### 3. Skill Routing
The agent reads `AGENTS.md` and routes messages to the right skill based on intent:

```
User: "Recherchiere aktuelle Trends in Agent Systems"
  → Intent: Research
  → Skill: deep-research/
  → Action: Read skill instructions, execute multi-step research
```

No hardcoded commands. The agent infers intent from natural language.

### 4. Multi-Agent Collaboration (Agent Exchange)
Two agents on different machines collaborate via a shared Git repository:

```
Agent A (Mac mini)          Agent B (MacBook)
     │                           │
     ├── writes signal ──────────►│
     │   signals/request.md      │
     │                           ├── reads signal
     │                           ├── writes response
     │◄──────────── response ────┤
     │   responses/answer.md     │
     │                           │
     └── reads response          │
```

Asynchronous, auditable, version-controlled. No real-time connection needed.

### 5. Memory Architecture
Three levels of memory:

| Type | Lifespan | Example |
|------|----------|---------|
| **Working Memory** | Current conversation | "User asked about X" |
| **Daily Notes** | `memory/YYYY-MM-DD.md` | Ideas, decisions, learnings |
| **Long-term Memory** | `MEMORY.md` + Vault | User preferences, project context |

## File Structure

```
agent-system-starter-kit/
├── README.md                 # You are here
├── SOUL.md                   # Agent identity & personality
├── IDENTITY.md               # Name, creature type, vibe
├── AGENTS.md                 # Behavior rules, skill routing, delegation
├── TOOLS.md                  # Tool permissions & restrictions
├── MEMORY.md                 # Long-term memory (compact)
├── USER.md                   # User preferences
├── HEARTBEAT.md              # Periodic health check instructions
├── memory/                   # Daily notes (append-only)
│   └── example-day.md
├── skills/                   # Reusable skill definitions
│   ├── deep-research/
│   │   └── SKILL.md
│   ├── quality-gate/
│   │   └── SKILL.md
│   └── pre-build-check/
│       └── SKILL.md
├── exchange/                 # Multi-agent communication
│   ├── signals/
│   ├── questions/
│   └── responses/
└── docs/
    ├── ARCHITECTURE.md       # Detailed architecture explanation
    ├── DELEGATION.md         # When to delegate, when not to
    ├── PATTERNS.md           # Common patterns & anti-patterns
    └── FAILURES.md           # Real failures and what we learned
```

## The Delegation Decision

The core framework for working with agents:

|                    | Agent CAN do it     | Agent CANNOT do it  |
|--------------------|--------------------|--------------------|
| **Low risk**       | Delegate ✅         | Human does it       |
| **High risk**      | Delegate with oversight ⚠️ | Don't delegate ❌ |

Every task should pass through this matrix before you hand it to an agent.

## What We Learned (The Hard Way)

See [`docs/FAILURES.md`](docs/FAILURES.md) for detailed stories. Highlights:

- **A batch edit removed closing HTML tags in 13 files.** The nav buttons disappeared. Lesson: Always verify HTML balance after batch operations.
- **An agent cited a paper with the wrong authors.** It was confident and wrong. Lesson: Always verify citations independently.
- **API costs for automated cron jobs are invisible.** You don't notice them until the monthly bill. Lesson: Set spending limits BEFORE you automate.

## Who This Is For

- **Studying agent architecture** — See how a real system is structured
- **Evaluating whether to build your own** — Understand the complexity before committing
- **Teaching/learning** — Use as reference material for the [Agent Literacy course](https://planck-lab.github.io/agent-literacy/)

## Related

- 📚 [Agent Literacy Course](https://planck-lab.github.io/agent-literacy/) — Understand agent systems (no code required)
- 🌐 [AI Learning Ecosystem](https://janrummel.github.io/ai-learning/) — More courses on AI, GitHub, Obsidian

## License

Content: [CC BY-SA 4.0](LICENSE) · Code: [MIT](LICENSE-CODE)

---

*This starter kit is based on a real system that has been running since early 2026. It was anonymized by the system itself — an agent documenting its own architecture. 🤖*
