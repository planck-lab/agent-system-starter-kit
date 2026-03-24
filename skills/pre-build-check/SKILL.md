# Skill: Pre-Build Check

## Trigger
"Does this already exist?", "I want to build X", "Is it worth building?"

## Purpose
Before building anything: Check if someone else has already solved it.
The Fraunhofer Rule: "99% has already been done by someone — find the information."

## Method
1. **Search** for existing solutions (GitHub, Product Hunt, web search)
2. **Evaluate** each solution:
   - Does it solve the actual problem?
   - Is it maintained? (Last commit, stars, issues)
   - What's the license?
   - What's missing from existing solutions?
3. **Decide**: Build / Use existing / Combine / Don't build

## Output Format
```markdown
# Pre-Build Check: [Project Idea]

## Existing Solutions Found
| Solution | Solves Problem? | Maintained? | License | Gap |
|----------|----------------|-------------|---------|-----|
| X | Partially | Yes (2024) | MIT | Missing Y |

## Recommendation
[Build / Use X / Combine X+Y / Don't build — because...]

## If Building: Minimal Viable Scope
[What's the SMALLEST thing worth building?]
```

## Rules
- Always check before building
- "Just build it" is not a valid recommendation without checking
- If 3+ good solutions exist: the bar for building your own is HIGH
