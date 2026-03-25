# ⚡ Quickstart

**From clone to working agent in 15 minutes.**

---

## Step 1: Clone & Personalize (5 min)

```bash
git clone https://github.com/planck-lab/agent-system-starter-kit.git my-agent
cd my-agent
rm -rf .git && git init
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
```bash
# Claude Code reads CLAUDE.md automatically
cp AGENTS.md CLAUDE.md
# Or create a CLAUDE.md that references your files:
echo "Read SOUL.md, USER.md, and AGENTS.md at session start." > CLAUDE.md
```
Then run `claude` in the directory. Done.

### Option B: OpenClaw (recommended for always-on agents)
Point your OpenClaw workspace to this directory. The gateway reads SOUL.md, AGENTS.md, etc. automatically.

### Option C: Any LLM with system prompts
Concatenate `SOUL.md` + `AGENTS.md` + `TOOLS.md` into your system prompt. The agent will follow the behavior rules regardless of the underlying model.

### Option D: Study only (no platform needed)
Just read the files. The architecture is the lesson.

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

*Estimated time: 15 minutes from clone to working agent.*
*No code required. No framework lock-in. Just markdown files and clear thinking.*
