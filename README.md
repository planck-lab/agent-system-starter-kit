# 🏗️ Agent System Starter Kit

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Code License: MIT](https://img.shields.io/badge/Code-MIT-green.svg)](LICENSE-CODE)
[![GitHub stars](https://img.shields.io/github/stars/planck-lab/agent-system-starter-kit?style=social)](https://github.com/planck-lab/agent-system-starter-kit/stargazers)

**Understand before you build.** A minimal, framework-agnostic starter kit for personal AI agent systems.

> Based on a real agent system that's been running in production since early 2026. Anonymized, documented, and ready to use.

📚 **New to agent systems?** Start with the [Agent Literacy Course](https://planck-lab.github.io/agent-literacy/) — no code required.

### ⚡ [Quickstart Guide →](QUICKSTART.md)

---

## Which kit do you need?

```
Want to understand how agent systems work?
  → You're here. This kit.

Want to build a production system with Claude Code?
  → Orchestrator Kit (github.com/janrummel/claude-orchestrator-starter)

Want both agents to collaborate?
  → Clone both + connect via Agent Exchange (see below)
```

## Three Ways to Use This

| Path | Time | For whom |
|------|------|----------|
| **📖 Study** | 30 min | Read the architecture docs, understand the patterns |
| **🚀 Build** | 30 min | Clone, personalize, connect to Claude Code / OpenClaw / any LLM |
| **🎓 Learn** | 2 hours | Follow the [Agent Literacy Course](https://planck-lab.github.io/agent-literacy/) with this as companion |

---

## What This Is

A **scaffold template** for personal AI agent systems. It provides:
- Identity layer (who is this agent?)
- Behavior layer (how does it act?)
- Memory layer (what does it remember?)
- Skill layer (what can it do?)
- Multi-agent communication (agent exchange pattern)

Based on a real system that handles research, note-taking, system monitoring, and content creation daily. Anonymized — no API keys, no personal data.

## What This Is NOT

- ❌ Not a framework (use LangChain, CrewAI, or OpenClaw for that)
- ❌ Not an app you can `npm start` (it's markdown-driven, not code-driven)
- ❌ Not locked to any provider (works with Claude, GPT, Gemini, local models)

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
├── QUICKSTART.md             # ⚡ Start here — 15 min setup guide
├── CLAUDE.md                 # Claude Code auto-reads this
├── README.md                 # You are here
│
├── SOUL.md                   # Layer 1: Identity & personality
├── IDENTITY.md               # Layer 1: Name, creature type, vibe
├── AGENTS.md                 # Layer 2: Behavior rules, skill routing
├── TOOLS.md                  # Layer 2: Tool permissions & restrictions
├── MEMORY.md                 # Layer 3: Long-term memory (compact)
├── USER.md                   # Context: User preferences
├── HEARTBEAT.md              # Scheduled health check instructions
│
├── memory/                   # Layer 3: Daily notes (append-only)
│   └── example-day.md
├── skills/                   # Layer 4: Reusable skill definitions
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
├── examples/
│   └── exchange-session.md   # Real multi-agent exchange (anonymized)
└── docs/
    ├── ARCHITECTURE.md       # Detailed architecture explanation
    ├── RUNTIME.md            # Where agents live: hardware, platforms, costs
    ├── AGENT_EXCHANGE.md     # Multi-agent collaboration deep dive
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

- **Building your first agent system** — Clone, personalize, run ([Quickstart](QUICKSTART.md))
- **Studying agent architecture** — See how a real system is structured
- **Evaluating whether to build your own** — Understand the complexity before committing
- **Teaching/learning** — Use as companion to the [Agent Literacy course](https://planck-lab.github.io/agent-systems/)

## How This Compares

There are other starter kits. They're good — for different things.

|  | This Kit | [wshobson/agents](https://github.com/wshobson/agents) | [Decipherist](https://thedecipherist.github.io/claude-code-mastery-project-starter-kit/) |
|---|---|---|---|
| **Philosophy** | Understand first | Plugin catalog | Project scaffold |
| **Files** | ~15 markdown | 300+ | 50+ |
| **Dependencies** | None | None | None |
| **Multi-Agent** | ✅ Exchange pattern | ❌ | ❌ |
| **Provider lock-in** | None | Claude Code | Claude Code |
| **Skills included** | 3 examples | 146+ | 10+ |
| **Failure docs** | ✅ Real failures | ❌ | ❌ |
| **Best for** | First agent system | Power users | New projects |

**If you need 146 skills and 72 plugins →** [wshobson/agents](https://github.com/wshobson/agents)
**If you need MDD workflow and hooks →** [Decipherist Starter Kit](https://thedecipherist.github.io/claude-code-mastery-project-starter-kit/)
**If you want to understand how agent systems work →** You're here.

## Better Together

**Why two agents?** One agent can't do everything well. Splitting responsibilities — research vs. coding, day vs. night, build vs. audit — makes each agent better. The Exchange pattern connects them without a framework.

**Real use cases:**
- 🔬 **Research + Review:** Agent A researches, Agent B challenges findings
- 🌙 **Day + Night:** Cron agent works overnight, interactive agent picks up in the morning
- 🔍 **Build + Audit:** One agent writes code, another runs quality checks

This kit is one half of a duo:

| | This Kit | [Orchestrator Kit](https://github.com/janrummel/claude-orchestrator-starter) |
|---|---|---|
| **Role** | The thinker — understand the architecture | The builder — set up the system |
| **Approach** | Markdown-driven, any LLM | Shell + YAML, Claude Code |
| **Strength** | Architecture, patterns, failures | Memory, skills, scheduling, Telegram |

**Connect them** via the [Agent Exchange](exchange/) pattern — a shared Git repo where agents on different machines communicate asynchronously.

## Learning Path & Ecosystem

**Recommended learning journey:**

1. **📚 [Agent Literacy](https://planck-lab.github.io/agent-literacy/)** — Start here if you're new to agent systems. No code required, pure concept work. Understand what agents are, how they think, and when (not) to use them.

2. **🏗️ This Kit** — Once you understand the theory, use this starter kit to see how concepts translate into a real system. Study the architecture, clone it, personalize it.

3. **🌐 [AI Learning Hub](https://janrummel.github.io/ai-learning/)** — Explore the broader ecosystem: GitHub workflows, Obsidian knowledge management, AI reasoning patterns.

4. **🔬 [Agent Systems (Archive)](https://planck-lab.github.io/agent-systems/)** — Technical deep dive into production patterns, multi-agent orchestration, and system design. Advanced material.

**Philosophy:** Understand before you build → Build with structure → Refine with experience.

## License

Content: [CC BY-SA 4.0](LICENSE) · Code: [MIT](LICENSE-CODE)

---

*This starter kit is based on a real system that has been running since early 2026. It was anonymized by the system itself — an agent documenting its own architecture. 🤖*
