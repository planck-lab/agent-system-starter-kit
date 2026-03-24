# Failures & Learnings

Real failures from running this agent system in production. Not theoretical risks — things that actually happened.

---

## 1. The Cascading HTML Disaster

**What happened:** A batch edit to remove dropdown wrappers from 15 HTML files accidentally removed closing `</details>` tags in 13 of them. The navigation buttons at the bottom of each page disappeared — they were trapped inside collapsed dropdown elements.

**Impact:** Users couldn't navigate between pages. The agent reported "all files updated successfully" because the edit itself worked — it just had an unintended side effect.

**Root cause:** The removal pattern `</details>\n<details>...\n</details>` was too greedy. It removed the second `</details>` which belonged to a different element.

**Fix:** Manual verification of HTML tag balance after every batch edit.

**Lesson:** ⚠️ **Batch edits need structural verification, not just content verification.** The agent should have run `grep -c '<details' file && grep -c '</details>' file` after every edit.

---

## 2. The Confidently Wrong Citation

**What happened:** The agent cited "Dang et al. (2024)" as the authors of a paper. The actual first author was "Takerngsaksiri et al." The agent had either hallucinated the author name or confused it with a different paper.

**Impact:** Incorrect academic citation in a published course. Discovered during a multi-expert audit.

**Root cause:** LLMs have unreliable memory for paper metadata (authors, years, venues). The agent generated the citation from memory instead of verifying it.

**Fix:** Web search verification for every citation. Never trust LLM memory for paper metadata.

**Lesson:** ⚠️ **Never trust an agent's memory for facts that can be verified.** Always verify with a tool (web search, database lookup).

---

## 3. The Year That Wasn't

**What happened:** Anthropic's "Building Effective Agents" was cited as "(2025)" across 7 module files. It was actually published in December 2024.

**Impact:** Incorrect dates in 7 files. Small error, but repeated across the entire course.

**Root cause:** The agent's training data likely included early 2025 references to the paper, creating a false association.

**Fix:** Batch search-and-replace, followed by adding "Stand: März 2026" notes to time-sensitive content.

**Lesson:** ⚠️ **Dates are especially unreliable in LLM outputs.** Always verify publication dates against the source.

---

## 4. The Invisible Cost

**What happened:** Automated cron jobs (daily digest, weekly review) ran for 3 weeks before anyone checked the API costs. The combined cost was ~$45/month — 3x the expected budget.

**Impact:** Financial. No data loss, no security issue — just unexpected spending.

**Root cause:** Cron jobs run silently. Unlike manual API calls, you don't see the cost at the moment of use. The spending limit was set too high.

**Fix:** Reduced cron frequency, set granular spending alerts (80% of monthly budget), added cost tracking to the heartbeat check.

**Lesson:** ⚠️ **Set spending limits BEFORE you automate.** Monitor costs weekly, not monthly. Automated agents are silent spenders.

---

## 5. The Skill That Did Too Much

**What happened:** A research skill was configured to "save results to the vault automatically." It also had shell access for running verification scripts. A malformed research query caused the skill to write garbage data to 12 vault files before the human noticed.

**Impact:** 12 corrupted vault notes. Recoverable via Obsidian Sync history, but required 30 minutes of manual cleanup.

**Root cause:** The skill had write access to the vault without a confirmation step. The "save automatically" instruction bypassed the usual HITL checkpoint.

**Fix:** Added a verification step: skill writes to /tmp/ first, then the main agent reviews before vault write.

**Lesson:** ⚠️ **Auto-save is convenient until it auto-corrupts.** Every write to persistent storage should have at least one checkpoint.

---

## 6. The Multi-Agent Miscommunication

**What happened:** Agent A wrote a signal requesting "review the latest changes." Agent B interpreted "latest changes" as the last Git commit. Agent A meant the last 5 commits. Agent B reviewed one commit and reported "all good."

**Impact:** Incomplete review. 4 commits with issues went unreviewed.

**Root cause:** Ambiguous language in the signal file. "Latest" is not a precise term.

**Fix:** Signals now include explicit parameters: commit range, file list, or time window.

**Lesson:** ⚠️ **Agent-to-agent communication needs the same precision as API calls.** Natural language between agents creates the same ambiguity as between humans — but without the ability to read facial expressions.

---

## Meta-Lesson

Every failure above has the same root cause: **over-trusting the agent's competence.**

The agent is not stupid — it's confidently imprecise. It will execute tasks, report success, and move on. It won't notice that the HTML is broken, the citation is wrong, or the cost is too high.

**The human's job is not to do the work — it's to verify the work.** That's the core of Human-in-the-Loop: not a safety net, but a quality gate.
