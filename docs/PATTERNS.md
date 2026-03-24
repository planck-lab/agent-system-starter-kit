# Patterns & Anti-Patterns

## Patterns That Work

### 1. Workspace as Context
**Pattern:** All agent configuration lives in the workspace as plain text files.
**Why:** The LLM reads these files at the start of every session. Changes take effect immediately. No redeployment needed.
**Example:** Change SOUL.md → agent's personality changes in the next message.

### 2. Append-Only Memory
**Pattern:** Daily memory files are append-only. Never overwrite, only add.
**Why:** Prevents accidental data loss. Creates a natural chronology. Easy to search with grep.
**Example:** `memory/2026-03-24.md` gets new entries throughout the day.

### 3. Skill Files as Instructions
**Pattern:** Each skill is a markdown file with trigger, method, and output format.
**Why:** Human-readable, version-controlled, shareable. The agent follows the instructions like a recipe.
**Example:** `skills/deep-research/SKILL.md` defines how research should be conducted.

### 4. Graduated Autonomy
**Pattern:** Start with full approval for everything. Gradually reduce oversight as trust builds.
**Why:** Catches configuration errors early. Prevents the "I trusted the agent and it deleted my files" scenario.
**Example:** Week 1 = approve every shell command. Month 3 = only approve destructive commands.

### 5. Sub-Agent Delegation
**Pattern:** Long-running tasks spawn a sub-agent. Main agent stays responsive.
**Why:** The user shouldn't wait 5 minutes for a research result. Async is better.
**Example:** "Research X" → spawns sub-agent → main agent confirms "Started, will report back" → sub-agent delivers result later.

### 6. Git-Based Agent Exchange
**Pattern:** Multiple agents communicate via files in a shared Git repository.
**Why:** Asynchronous, auditable, resilient. No need for real-time connections.
**Example:** Agent A writes `signals/review-request.md` → Agent B reads it, writes `responses/review-done.md`.

---

## Anti-Patterns to Avoid

### 1. The God Agent
**Anti-pattern:** One agent that does everything — research, coding, email, calendar, file management.
**Why it fails:** Context window fills up. Agent loses focus. Quality drops across all tasks.
**Fix:** Specialize. Main agent for conversation, sub-agents for heavy tasks.

### 2. Configuration Sprawl
**Anti-pattern:** AGENTS.md grows to 500+ lines with every new rule.
**Why it fails:** LLMs have limited attention. Long configs get partially ignored. Contradictions creep in.
**Fix:** Keep AGENTS.md under 300 lines. Move detailed instructions to skill files.

### 3. Blind Automation
**Anti-pattern:** Cron jobs that run agent tasks without any human review.
**Why it fails:** Errors accumulate silently. Costs grow invisibly. Quality degrades unnoticed.
**Fix:** Every automated task should have a verification mechanism — even if it's just a daily digest of what ran.

### 4. Memory Hoarding
**Anti-pattern:** Saving everything to MEMORY.md. "The agent should remember all context."
**Why it fails:** MEMORY.md becomes too long, agent ignores most of it. Important facts get buried.
**Fix:** MEMORY.md = compact summary (<2000 chars). Daily notes = detailed log. Vault = long-term archive.

### 5. Trusting Agent-Generated Facts
**Anti-pattern:** Agent says "Paper X was published in Nature" → you believe it.
**Why it fails:** LLMs hallucinate citations, dates, and authors with high confidence.
**Fix:** Every factual claim should be verified with a tool (web search, database). Agent memory is a hint, not a source.

### 6. Skills That Modify Core Config
**Anti-pattern:** A skill that rewrites SOUL.md or AGENTS.md as part of its workflow.
**Why it fails:** The agent can change its own behavior in unexpected ways. Self-modification without human oversight is a recipe for drift.
**Fix:** Core config files are protected. Only the human modifies them.
