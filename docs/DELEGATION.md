# The Delegation Decision

## The Core Framework

Before delegating any task to an agent, run it through this matrix:

|                    | Agent CAN do it     | Agent CANNOT do it  |
|--------------------|--------------------|--------------------|
| **Low risk**       | ✅ Delegate         | Human does it       |
| **High risk**      | ⚠️ Delegate + review | ❌ Don't delegate   |

## How to Assess "Can the Agent Do It?"

Ask these questions:
1. **Is the task well-defined?** Vague tasks → vague results
2. **Can the output be verified?** If you can't check it, you can't trust it
3. **Does it require real-world knowledge after the training cutoff?** If yes, the agent needs web search
4. **Does it require judgment about YOUR specific context?** Agents don't know your organization

## How to Assess Risk

| Risk Level | Examples |
|-----------|---------|
| **Low** | Summarize a document, format text, search the web, draft an email |
| **Medium** | Write code, create content for publication, make calculations |
| **High** | Delete files, send emails, modify configuration, financial decisions |
| **Critical** | Anything irreversible, anything involving personal data, anything public-facing |

## Delegation Patterns

### Pattern 1: Fire and Forget ✅
- Low risk, agent is competent
- Example: "Summarize this article"
- Human involvement: None (read the result when convenient)

### Pattern 2: Draft and Review ⚠️
- Medium risk or uncertain quality
- Example: "Write a blog post about X"
- Human involvement: Review before publishing

### Pattern 3: Plan and Approve
- High risk, but agent can plan
- Example: "Reorganize the file structure"
- Human involvement: Agent proposes plan → human approves → agent executes

### Pattern 4: Supervised Execution
- High risk, step by step
- Example: "Deploy the new version"
- Human involvement: Approve each step individually

### Pattern 5: Human Only ❌
- Critical risk or requires human judgment
- Example: "Should we hire this person?"
- Human involvement: Agent can gather info, but human decides

## Common Mistakes

### Over-delegation
"Let the agent handle everything" → Invisible errors accumulate.
**Fix:** Start with Pattern 2 for everything. Move to Pattern 1 only after proving reliability.

### Under-delegation
"I'll do everything myself, the agent just makes mistakes" → You're paying for an agent you don't use.
**Fix:** Start with low-risk tasks. Build trust gradually.

### Delegation Without Verification
"The agent said it's done, so it's done" → The biggest source of failures.
**Fix:** Every high-risk task needs a verification step. Not "did the agent run?" but "is the result correct?"

## The Trust Ladder

```
Week 1:  🟢 Everything needs approval (driving instructor)
Week 2:  🟢 Routine tasks auto-approved (familiar roads)
Week 4:  🟡 Research runs autonomously (highway driving)
Week 8:  🟠 Complex workflows delegated (city driving)
Month 3: 🟠 Sub-agents run scheduled tasks (solo driving)
Month 6: 🔴 System monitors itself, human checks dashboards (dashcam)
```

Don't skip levels. Each level builds on trust earned at the previous level.
