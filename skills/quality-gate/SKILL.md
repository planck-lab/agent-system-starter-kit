# Skill: Quality Gate

## Trigger
"Quality check", "Review this", "Is this accurate?"

## Method
Apply 5 lenses to any content:

### 1. Factual Accuracy
- Are claims verifiable?
- Are numbers/statistics sourced?
- Any hallucinations or fabrications?

### 2. Source Quality
- Tier A/B/C classification
- Are sources actually cited (not just mentioned)?
- Do the sources support the claims made?

### 3. Bias Check
- One-sided perspective?
- Missing counterarguments?
- Marketing language disguised as analysis?

### 4. Completeness
- Obvious gaps in coverage?
- Important caveats missing?
- Target audience considered?

### 5. Consistency
- Internal contradictions?
- Claims in one section that contradict another?
- Tone/style consistent throughout?

## Output Format
| Lens | Rating | Finding |
|------|--------|---------|
| Accuracy | ✅/⚠️/❌ | Details |
| Sources | ✅/⚠️/❌ | Details |
| Bias | ✅/⚠️/❌ | Details |
| Completeness | ✅/⚠️/❌ | Details |
| Consistency | ✅/⚠️/❌ | Details |

**Overall:** X/5 lenses passed. [Recommendation]

## Rules
- Be honest, not confirmatory
- "No issues found" is a valid result
- Flag severity: 🔴 Critical / 🟡 Important / 🟢 Minor
