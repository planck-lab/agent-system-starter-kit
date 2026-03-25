# Scientific Standards for Educational Content

This document defines how we research, write, cite, and verify content in this project. It applies to all pages, guides, and documentation — not just academic papers.

Our standard is derived from five established frameworks:
- [Wikipedia Verifiability Policy](https://en.wikipedia.org/wiki/Wikipedia:Verifiability) — burden of proof on the editor
- [CRAAP Test](https://en.wikipedia.org/wiki/CRAAP_test) (CSU Chico) — Currency, Relevance, Authority, Accuracy, Purpose
- [SIFT Method](https://hapgood.us/2019/06/19/sift-the-four-moves/) (Mike Caulfield) — Stop, Investigate, Find better, Trace to original
- [GRADE Framework](https://www.cochrane.org/learn/courses-and-resources/cochrane-methodology/grade) (Cochrane) — evidence quality tiers
- [Nature Formatting Guide](https://www.nature.com/nature/for-authors/formatting-guide) / [APA Style](https://apastyle.apa.org/) — citation standards

---

## 1. Research Before Prose

**Never write first, source later.** The workflow is:

```
1. Define what you want to say (outline)
2. Research: find sources for each claim
3. Evaluate sources (SIFT + CRAAP)
4. Write — building around the evidence
5. Verify — check every claim against its source
6. Cite — inline links + footer table
```

If you can't find a source for a claim, either:
- Mark it explicitly as estimate/opinion
- Remove the claim
- Find the evidence first

---

## 2. Source Tiers

| Tier | Definition | Examples | How to use |
|------|-----------|----------|------------|
| **A** | Peer-reviewed, official specifications, primary data | arXiv papers, Nature, IEEE, RFCs, official documentation (Anthropic, OpenAI, Apple), government data | Directly citable as fact |
| **B** | Reputable tech/industry with editorial oversight | DEV.to (data-backed), Ars Technica, Gartner, McKinsey, established tech blogs with named authors | Citable with context ("According to...") |
| **C** | Personal blogs, books, opinion pieces, tutorials | Medium posts, personal sites, self-published books | As perspective only, not as fact. Always note it's an opinion. |
| **❌** | Unknown, marketing, AI-generated without review | SEO content farms, product landing pages, press releases, anonymous sources | Do not use as evidence |

### Minimum per page
- Pages with factual claims: **≥3 sources** (at least 2× Tier A or B)
- Pages that are purely conceptual (no numbers): sources recommended but not required

---

## 3. SIFT Every Source

Before citing any source, run SIFT:

| Step | Question | Action |
|------|----------|--------|
| **Stop** | Am I about to accept this at face value? | Pause. Don't copy-paste claims without checking. |
| **Investigate** | Who wrote this? What's their expertise? Who published it? | Check the author, the publication, potential conflicts of interest. |
| **Find better** | Is there a more authoritative source for this claim? | Search for the primary source. A blog citing a paper → cite the paper. |
| **Trace** | Where did this claim originate? | Follow the citation chain back to the original data/study. |

**Red flags:**
- No author named
- No date
- Claims without their own sources
- Extraordinary claims ("X is 10× better") without data
- Domain is primarily a sales/marketing site

---

## 4. Citation Rules

### Every number needs a source

Any of these require an inline link or explicit "estimate" marker:
- Dollar amounts ($)
- Percentages (%)
- Quantities (million, billion, K)
- Benchmark scores (MMLU, HumanEval)
- Dates presented as facts ("launched in 2025")
- Comparative claims ("5× faster", "most popular")

### How to cite

**Inline (preferred for web content):**
```
Ollama reached [52 million monthly downloads](https://source-url) in Q1 2026.
```

**For HTML pages:**
```html
Ollama reached <a href="https://source-url">52 million monthly downloads</a> in Q1 2026.
```

**If no link available:**
```
Mac mini consumes approximately 15W idle (Apple Technical Specifications, March 2026).
```

### Estimates and opinions

When a claim is your own estimate or has no source:
```
✅ "approximately $5-15/month (estimate based on local electricity rates)"
✅ "in our experience, this takes about 30 minutes"
❌ "$5-15/month" (presented as fact without source)
❌ "this takes 15 minutes" (unsourced precision implies authority)
```

---

## 5. Source Footer

Every page with factual claims includes a source table at the bottom:

```markdown
## Sources

All prices and benchmarks as of March 2026 — verify before making decisions.

| # | Source | Type |
|---|--------|------|
| 1 | [Name — Title](URL) | What it supports |
| 2 | [Name — Title](URL) | What it supports |
```

For HTML pages, the same table in `<table>` format.

---

## 6. Expiration & Freshness

### Time-sensitive content
- **Prices:** Always include "As of [Month Year]" and "verify before purchasing"
- **Benchmarks:** Include model version and benchmark date
- **Market data:** Include the quarter/year of the data
- **Framework comparisons:** Note that the landscape changes rapidly

### Freshness rules
| Content type | Max age before review |
|-------------|----------------------|
| API prices | 3 months |
| Benchmark scores | 6 months |
| Framework comparisons | 6 months |
| Conceptual content (patterns, principles) | 12+ months |
| Historical facts | No expiration |

---

## 7. Verification Checklist

Before publishing any page, run this checklist:

- [ ] Every $ amount has a source or is marked as estimate
- [ ] Every % has a source or is marked as estimate
- [ ] Every "X million/billion" has a source
- [ ] Every benchmark score links to the evaluation
- [ ] Comparative claims ("better than", "faster than") have data
- [ ] Source footer exists with numbered entries
- [ ] "As of [date]" appears near time-sensitive data
- [ ] No claims from Tier ❌ sources
- [ ] Primary sources used where available (not blog-citing-blog)
- [ ] At least 3 sources per page with factual claims

---

## 8. Enforcement

### Automated (Gate 2 Pre-Push Hook)
- Scans `docs/*.md` and `*.html` for number-claims without nearby links
- Warning when >3 numbers but <2 source links

### Manual (before each publish)
- Run the verification checklist above
- Author self-review: "Could someone challenge this claim? If yes, is it sourced?"

### Learning loop
- When a factual error is found post-publish: document in `learnings/`
- Review and tighten standards quarterly

---

*This standard is a living document. It adapts as we learn. Last updated: March 2026.*

**Frameworks referenced:**
- Wikipedia. "Verifiability." [https://en.wikipedia.org/wiki/Wikipedia:Verifiability](https://en.wikipedia.org/wiki/Wikipedia:Verifiability)
- Blakeslee, Sarah. "The CRAAP Test." CSU Chico, 2004. [https://en.wikipedia.org/wiki/CRAAP_test](https://en.wikipedia.org/wiki/CRAAP_test)
- Caulfield, Mike. "SIFT (The Four Moves)." 2019. [https://hapgood.us/2019/06/19/sift-the-four-moves/](https://hapgood.us/2019/06/19/sift-the-four-moves/)
- Cochrane. "GRADE Approach." [https://www.cochrane.org/learn/courses-and-resources/cochrane-methodology/grade](https://www.cochrane.org/learn/courses-and-resources/cochrane-methodology/grade)
- Nature. "Formatting Guide." [https://www.nature.com/nature/for-authors/formatting-guide](https://www.nature.com/nature/for-authors/formatting-guide)
