# Stage: Analyze Change Impact

## Overview

Every change mid-project has ripple effects. This skill prevents "just a small change" from silently breaking scope, schedule, and quality. Assess before you commit.

**Core principle:** Understand the full cost of a change before deciding. Uninformed "yes" is more dangerous than informed "no."

## When to Use

- New feature request appears mid-implementation
- Existing requirement needs modification
- User says "what if we also..." or "can we change..."
- External constraint changes (API deprecation, library update, new regulation)
- After discovering technical limitation that forces redesign

## The Iron Law

```
NO CHANGE IMPLEMENTED WITHOUT IMPACT ANALYSIS FIRST
```

"Quick change" is an oxymoron. Every change has consequences — find them before coding.

## The Process

### Step 1: Capture the Change Request

Before analysis, document exactly what's being asked:

```markdown
## Change Request
**What:** [precise description of the change]
**Why:** [business reason / trigger]
**Requested by:** [who wants this]
**Date:** [when requested]
```

Ask: "Let me make sure I understand — you want to [restate change]. What's driving this? What problem does it solve that the current plan doesn't?"

**Key:** Understand WHY before assessing WHAT. Sometimes the underlying need can be solved differently with less impact.

### Step 2: Trace the Impact

Work through this checklist systematically. For each item, answer: "Does this change affect [X]? If yes, how?"

**Requirements Impact:**
- [ ] Other user stories — do any existing stories need modification?
- [ ] Acceptance criteria — do any existing AC need to change?
- [ ] Business rules — are any rules affected or contradicted?
- [ ] Quality requirements — does this affect performance, security, usability targets?
- [ ] User flows — does the core user journey change?

**Architecture & Design Impact:**
- [ ] Data model — new fields, tables, relationships?
- [ ] API contracts — endpoints change, new params, breaking changes?
- [ ] Component boundaries — does this cross existing module lines?
- [ ] Third-party dependencies — new libraries, API calls, services?
- [ ] Security model — new permissions, access patterns?

**Implementation Impact:**
- [ ] Existing code — which files/modules need modification?
- [ ] Tests — which existing tests break? What new tests needed?
- [ ] Data migration — does existing data need transformation?
- [ ] Configuration — new settings, environment variables?
- [ ] Documentation — what docs become outdated?

**Project Impact:**
- [ ] Timeline — does this extend the ship date?
- [ ] Other features — does anything get displaced or deprioritized?
- [ ] Dependencies — does this block or unblock other work?

### Step 3: Estimate Effort

For each affected component, estimate:

```markdown
## Effort Estimate

| Component | What Changes | Effort | Confidence |
|-----------|-------------|--------|------------|
| User stories | Rewrite US-003, add US-012 | 1h | High |
| Data model | Add 2 fields to Recipe table | 30m | High |
| API | New endpoint + modify GET /recipes | 3h | Medium |
| Frontend | New form component + update search | 4h | Medium |
| Tests | 5 new tests + update 3 existing | 2h | High |
| Migration | Backfill existing records | 1h | Low |
| **Total** | | **~11.5h** | |
```

**Confidence levels:**
- **High:** Done this before, well-understood scope
- **Medium:** Reasonable estimate, some unknowns
- **Low:** Significant unknowns, could be 2-3x estimate

**Rule:** If total confidence is Low, add 50-100% buffer. Unknowns always take longer.

### Step 4: Assess Alternatives

Before recommending "do it" or "don't," explore alternatives:

"Is there a simpler way to achieve the same goal?"

Common alternatives:
- **Subset:** Implement 30% of the request that delivers 80% of the value
- **Defer:** Add to backlog for next release instead of disrupting current work
- **Workaround:** Manual process or configuration instead of code change
- **Different approach:** Solve the underlying need differently with less impact

Present 2-3 options with trade-offs.

### Step 5: Make Recommendation

Present the analysis clearly:

```markdown
## Impact Analysis Summary

**Change:** [one-line description]
**Total Effort:** [estimate] (confidence: [level])
**Timeline Impact:** [none / delays by X days / blocks feature Y]
**Risk Level:** Low / Medium / High

### Affected Components
[list from Step 2]

### Options
1. **Full implementation** — [effort], [timeline impact], [risk]
2. **Simplified version** — [effort], [timeline impact], [risk]
3. **Defer to next release** — no current impact, tracked in backlog

### Recommendation
[Your recommendation with rationale]
```

**Decision framework:**
- High value + Low impact → Recommend: implement now
- High value + High impact → Recommend: simplified version or defer
- Low value + Low impact → Recommend: implement if time permits
- Low value + High impact → Recommend: reject or defer

Ask: "Given this analysis, how would you like to proceed?"

### Step 6: Update Artifacts

If change is approved:
- [ ] Update requirements doc with new/modified stories
- [ ] Update priority matrix
- [ ] Update vision doc scope if boundary changed
- [ ] Note what was displaced (if anything moved out to accommodate this)
- [ ] Commit changes with clear message: "Scope change: [description] — approved [date]"

## Quick Impact Check (Lightweight Version)

For small changes that don't warrant full analysis:

```
1. WHAT changes? [one sentence]
2. WHAT ELSE is affected? [list files/components]
3. HOW LONG? [quick estimate]
4. WHAT BREAKS? [existing tests/features]
5. WORTH IT? [yes/no with one-line rationale]
```

Takes 5 minutes. Use for changes estimated under 2 hours of work.

## Key Principles

- **Trace before you code** — follow the change through all layers
- **Alternatives first** — the cheapest change is the one you don't make
- **Confidence matters** — low confidence = pad the estimate
- **No silent scope growth** — every addition displaces something
- **Document the decision** — future you needs to know why this changed

## Red Flags — STOP

- "Just a quick change" → quick changes have the most hidden impacts
- No business reason for the change → reject until justified
- Impact analysis skipped "to save time" → analysis IS saving time
- Change affects >50% of codebase → this isn't a change, it's a rewrite
- Third change this week → scope is unstable, freeze and reassess priorities
- "We'll figure out the impact as we go" → that's called discovering bugs in production

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It's tiny, no need to analyze" | Tiny changes cause half of all production bugs. |
| "We're already behind, just do it" | Doing it without analysis puts you further behind. |
| "The user really wants this" | Users also want a working product. Analyze first. |
| "I can see what needs to change" | You can see what's obvious. Tracing reveals what isn't. |
| "Impact analysis is bureaucracy" | 15 minutes of analysis vs. 3 hours of debugging hidden breakage. |
| "It won't affect anything else" | Famous last words. Trace it. |
