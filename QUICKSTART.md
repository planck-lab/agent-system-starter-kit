# ⚡ Quickstart

**From clone to working agent in 30 minutes.**
**Just want to study the architecture? Skip to [Option D](#option-d-study-only-no-platform-needed).**

---

## Step 1: Clone & Personalize (5 min)

```bash
git clone https://github.com/planck-lab/agent-system-starter-kit.git my-agent
cd my-agent
rm -rf .git && git init
mkdir -p memory    # Agent writes daily notes here
```

Now customize these three files:

### `IDENTITY.md` — Name your agent
```markdown
# IDENTITY
- **Name:** Nova          # ← Pick a name
- **Creature:** AI assistant
- **Vibe:** Concise, helpful
- **Emoji:** 🤖
```

### `USER.md` — Describe yourself
```markdown
# USER
- **Name:** Your Name
- **Timezone:** Europe/Berlin
- **Language:** English
```

### `SOUL.md` — Set personality & boundaries
Change the name, language, and boundaries. Keep the structure — it works.

**That's it.** Your agent has an identity.

---

## Step 2: Choose Your Platform (5 min)

This architecture works with any LLM platform that supports system prompts and file reading. Here's how to connect it:

### Option A: Claude Code (recommended for developers)
The kit already includes a `CLAUDE.md` that tells Claude Code to read your configuration files at session start. No extra setup needed.
```bash
# Just run Claude Code in the directory
claude
```
Claude Code auto-reads `CLAUDE.md` → which loads `SOUL.md`, `USER.md`, `AGENTS.md`, and `TOOLS.md`. Done.

### Option B: OpenClaw (recommended for always-on agents)
Point your OpenClaw workspace to this directory. The gateway reads SOUL.md, AGENTS.md, etc. automatically.

### Option C: Any LLM with system prompts
Concatenate `SOUL.md` + `AGENTS.md` + `TOOLS.md` into your system prompt. The agent will follow the behavior rules regardless of the underlying model.

### Option D: Study only (no platform needed)

No API key, no platform, no setup. Just read the files — the architecture is the lesson.

**Guided Tour — map of concepts to files:**

| Concept | File | What you'll learn |
|---------|------|-------------------|
| Agent identity & personality | `SOUL.md` | How to define WHO an agent is |
| Behavior rules & skill routing | `AGENTS.md` | How to define HOW an agent acts |
| Tool permissions & restrictions | `TOOLS.md` | How to set boundaries |
| Memory architecture | `memory/` + `MEMORY.md` | Short-term vs long-term memory |
| Reusable capabilities | `skills/` | How to teach an agent new things |
| Multi-agent communication | `exchange/` | How agents collaborate via Git |
| Real failures & lessons | `docs/FAILURES.md` | What goes wrong in practice |
| Architecture deep-dive | `docs/ARCHITECTURE.md` | The 4-layer model explained |

**Reading order:** SOUL.md → AGENTS.md → docs/ARCHITECTURE.md → docs/FAILURES.md → exchange/ + [examples/exchange-session.md](examples/exchange-session.md)

If you've taken the [Agent Literacy Course](https://planck-lab.github.io/agent-systems/), you'll recognize the concepts:
- **Module 1** (Agent Loop) → See it in `AGENTS.md` (skill routing, intent detection)
- **Module 2** (Trust & Delegation) → See it in `docs/DELEGATION.md` (the 5 levels)
- **Module 3** (Architecture) → See it in `docs/ARCHITECTURE.md` (4-layer model)
- **Module 4** (Risk & Security) → See it in `TOOLS.md` (restricted paths) + `docs/FAILURES.md`
- **Module 5** (Strategy) → See it in the [exchange example](examples/exchange-session.md) (real decision-making)

---

## Step 3: Test It (5 min)

Send these messages to verify your agent works:

| Test | Send this | Expected behavior |
|------|----------|------------------|
| **Identity** | "Who are you?" | Agent responds with its name and personality |
| **Memory** | "Remember: I prefer dark mode." | Agent writes to `memory/YYYY-MM-DD.md` |
| **Skill routing** | "Research the current state of MCP" | Agent loads `skills/deep-research/SKILL.md` and follows it |
| **Boundaries** | "Read my .env file" | Agent refuses (security rule) |
| **Delegation** | "This is a work topic" | Agent declines (privacy rule) |

If all five pass: your agent is working. 🎉

---

## What's Next?

### Add a skill
```
skills/
└── my-new-skill/
    └── SKILL.md    # Instructions the agent follows
```

Write a SKILL.md with clear steps. The agent will route to it automatically when it detects matching intent (see `AGENTS.md` → Skill Routing table).

### Add a second agent (Agent Exchange)
```
exchange/
├── signals/       # Agent A writes requests here
├── questions/     # Open questions between agents
└── responses/     # Agent B writes answers here
```

Two agents share a Git repo. They communicate asynchronously via markdown files. See `exchange/README.md`.

### Customize behavior
Edit `AGENTS.md` to add:
- New skill routes
- Changed delegation rules
- Different memory categories
- Custom feedback loops

### Set up scheduled tasks
Add cron jobs or LaunchAgents that trigger your agent periodically:
- Heartbeat checks (`HEARTBEAT.md`)
- Daily digests (skill + cron)
- Weekly reviews (skill + cron)

---

## The Delegation Decision

Before giving any task to your agent, ask:

|                    | Agent CAN do it     | Agent CANNOT do it  |
|--------------------|--------------------|--------------------|
| **Low risk**       | ✅ Delegate          | Human does it       |
| **High risk**      | ⚠️ Delegate + review | ❌ Don't delegate    |

Start at 🟢 (everything needs approval). Earn trust. Then expand.

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Putting everything in SOUL.md | SOUL = identity, AGENTS = behavior, TOOLS = permissions |
| No security rules | Always block `.env`, `.ssh/`, `*.key` reads |
| Skipping memory setup | Agent loses context between sessions without `memory/` |
| Starting at full autonomy | Start supervised, expand trust over weeks |
| No failure documentation | Keep `docs/FAILURES.md` — you'll need it |

---

*Estimated time: 30 minutes from clone to working agent. Or 30 minutes to study the architecture without building anything.*
*No code required. No framework lock-in. Just markdown files and clear thinking.*
