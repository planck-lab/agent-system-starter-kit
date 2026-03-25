# Agent Exchange: Multi-Agent Collaboration via Git

The Agent Exchange is a pattern for asynchronous collaboration between AI agents using Git as the communication layer. No framework, no message queue, no API — just markdown files in a shared repository.

---

## Why Multi-Agent?

A single agent has limits:

| Problem | Example |
|---------|---------|
| **Cognitive load** | A research task takes 10 minutes. During that time, the agent can't answer chat messages. |
| **Specialization** | A coding agent needs different instructions than a research agent. Combining both dilutes quality. |
| **Availability** | An always-on server agent handles cron jobs at 3 AM. A laptop agent is available when you're working. |
| **Review bias** | An agent reviewing its own output is like proofreading your own essay — it sees what it expects to see. |

Multi-agent setups solve this by splitting responsibilities. The Exchange pattern connects them.

Sources: [Talebirad & Nadiri (2023), "Multi-Agent Collaboration"](https://arxiv.org/abs/2306.03314), [Google A2A Protocol](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)

---

## How It Works

### The Setup

Two (or more) agents share a Git repository:

```
agent-exchange/
├── signals/          # "Look at this" — requests for attention
│   ├── agent-a/      # Signals written by Agent A
│   └── agent-b/      # Signals written by Agent B
├── questions/        # "What do you think?" — open questions
│   ├── agent-a/
│   └── agent-b/
├── responses/        # "Here's my take" — answers and reviews
│   ├── agent-a/
│   └── agent-b/
├── learnings/        # "I noticed something" — observations
│   ├── agent-a/
│   └── agent-b/
└── reviews/          # "I reviewed your work" — structured feedback
    ├── agent-a/
    └── agent-b/
```

Each agent writes only in its own subfolder. Agents never modify each other's files.

### The Cycle

```
Agent A                              Agent B
  │                                    │
  ├─ writes signal or question         │
  ├─ git commit + push ────────────►   │
  │                                    ├─ git pull
  │                                    ├─ reads signal
  │                                    ├─ thinks / researches / acts
  │                                    ├─ writes response
  │   ◄──────────────── git push ──────┤
  ├─ git pull                          │
  ├─ reads response                    │
  └─ acts on it                        │
```

**Timing:** Agents don't need to be online simultaneously. Agent A pushes at 10:00, Agent B pulls and responds at 14:00, Agent A reads the response at 15:00. Fully asynchronous.

### File Format

Every exchange file uses YAML frontmatter:

```yaml
---
status: open           # open | in-progress | closed
by: agent-a            # who wrote this
date: 2026-03-25
topic: cost-analysis   # optional, for filtering
in-reply-to: signals/agent-a/original.md  # optional, for responses
---

# Title

Content in markdown. As long or short as needed.
```

**Required fields:** `status`, `by`, `date`
**Optional fields:** `topic`, `in-reply-to`, `confidence`, `proposed-action`

---

## Use Cases

### 1. Research + Review

The most common pattern. Agent A researches, Agent B challenges.

**Flow:**
1. Human asks Agent A: "Research the current state of MCP"
2. Agent A does the research, writes a briefing
3. Agent A writes a signal: `signals/agent-a/2026-03-25-mcp-research.md`
4. Agent B pulls, reads the briefing, writes a review: `reviews/agent-b/2026-03-25-mcp-review.md`
5. The review identifies: 2 claims without sources, 1 outdated price, 1 missing competitor
6. Agent A reads the review and improves the briefing

**Why it works:** Agent B has no investment in Agent A's output. It can be genuinely critical — unlike Agent A reviewing its own work.

### 2. Day + Night Shifts

Agent A (laptop) is interactive during the day. Agent B (server) runs automated tasks overnight.

**Flow:**
1. At 22:00, Agent A writes: `signals/agent-a/tonight-tasks.md` (list of overnight research tasks)
2. At 02:00, Agent B's cron picks up the signals, runs research, writes results to `responses/agent-b/`
3. At 08:00, Agent A pulls and presents the results

**Why it works:** No human needs to be awake. The exchange repo is the handoff point.

### 3. Build + Audit

One agent writes code or content. The other runs quality checks.

**Flow:**
1. Agent A builds a feature and pushes to the project repo
2. Agent A writes: `signals/agent-a/ready-for-review.md` (link to commit)
3. Agent B pulls, runs automated checks (HTML balance, broken links, security scan)
4. Agent B writes: `reviews/agent-b/feature-review.md` with findings

**Why it works:** The audit agent has different instructions (focused on finding problems, not building features). Separation of concerns.

### 4. Learning Loop

Agents teach each other by documenting observations.

**Flow:**
1. Agent A notices a pattern: "Rules erode proportionally to session length"
2. Agent A writes: `learnings/agent-a/2026-03-25-rules-erode.md`
3. Agent B reviews the learning: `accepted`, `returned`, or `rejected` with reasoning
4. Accepted learnings become design principles for both agents

**Why it works:** Distributed knowledge creation. Neither agent has the full picture, but together they build institutional knowledge.

---

## Limitations

Be honest about what the Exchange pattern does NOT do:

| Limitation | Detail |
|-----------|--------|
| **Not real-time** | Git push/pull has seconds-to-minutes latency. For real-time collaboration, use A2A or direct API calls. |
| **No streaming** | Agents can't watch each other think. They see finished outputs only. |
| **Merge conflicts** | If both agents push simultaneously, someone needs to resolve. In practice: rare, because agents write to their own subfolders. |
| **No built-in routing** | Agents must check for new files themselves (cron + `git pull`). There's no "inbox" notification. |
| **Scale ceiling** | Works well for 2-5 agents. At 10+ agents, the repo gets noisy. Consider topic-based sub-repos at that point. |
| **Trust is implicit** | Any agent with repo access can read everything. There's no per-file access control. |

### When NOT to use Exchange

- **Sub-second responses needed** → Use direct API calls or A2A protocol
- **10+ agents** → Use a proper message broker (RabbitMQ, Redis Streams)
- **Untrusted agents** → Exchange assumes all agents are on the same team
- **Binary data** → Git handles markdown well, not large binary files

---

## How Exchange Compares

| | Agent Exchange (Git) | A2A Protocol | Message Queue | Shared Database |
|---|---|---|---|---|
| **Setup** | `git init` | SDK + server | Broker + consumers | DB + schema |
| **Latency** | Seconds-minutes | Milliseconds | Milliseconds | Milliseconds |
| **Audit trail** | Full (git log) | Needs logging | Needs logging | Needs logging |
| **Offline support** | ✅ Works offline | ❌ Needs network | ❌ Needs broker | ❌ Needs DB |
| **Complexity** | Minimal | Medium | Medium-High | Medium |
| **Best for** | 2-5 agents, async | Real-time multi-agent | High-throughput | Structured data |

Sources: [Google A2A Specification](https://github.com/a2aproject/A2A), [Anthropic MCP](https://modelcontextprotocol.io/specification/2025-11-25)

---

## Getting Started

### Minimal setup (5 minutes)

```bash
# Create exchange repo
mkdir agent-exchange && cd agent-exchange
git init
mkdir -p signals/agent-a signals/agent-b
mkdir -p questions/agent-a questions/agent-b
mkdir -p responses/agent-a responses/agent-b

# Create a signal
cat > signals/agent-a/first-signal.md << 'EOF'
---
status: open
by: agent-a
date: 2026-03-25
---

# Hello from Agent A

This is my first signal. Agent B, please acknowledge.
EOF

git add . && git commit -m "first signal"
# Push to shared remote (GitHub, GitLab, self-hosted)
```

### Connect to this starter kit

The `exchange/` folder in this kit is already structured for Exchange. To connect two instances:

1. Create a shared repo on GitHub (private recommended)
2. Clone it on both machines
3. Configure each agent to check for new files periodically (`git pull` in a cron job)
4. Each agent writes to its own subfolders

See [examples/exchange-session.md](../examples/exchange-session.md) for a real session between two production agents.

---

*This pattern was developed through daily use between two agents running on different machines since March 2026. It's simple, and that's the point.*
