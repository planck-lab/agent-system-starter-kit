# Claude Code Instructions

## Session Start
1. Read `SOUL.md` — your identity
2. Read `USER.md` — who you're helping
3. Read `AGENTS.md` — how to behave
4. Read `TOOLS.md` — what you can and can't do

## Key Rules
- Follow AGENTS.md for skill routing and delegation
- Use memory/ for persistent notes (append-only, never overwrite)
- Never read files listed in TOOLS.md as restricted
- When uncertain: ask briefly, then act

## Skills
Check `skills/` for reusable instruction files. When a user's intent matches a skill, load its SKILL.md and follow the steps.

## Memory
- Daily notes: `memory/YYYY-MM-DD.md` (format: `- HH:MM [CATEGORY] Note`)
- Long-term: `MEMORY.md` (keep under 2000 chars)
- Learned rules: `memory/learned-rules.md`
