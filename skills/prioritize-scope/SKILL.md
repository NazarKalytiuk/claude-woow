---
name: prioritize-scope
description: "Use when scope feels too large, priorities are unclear, features are competing, or you need to decide what to build first. Use when user says 'what should we do first', 'too many features', 'scope is too big', 'help me prioritize'. Produces a ranked feature list with clear rationale."
---

# Prioritize Scope

## Overview

Cut scope to what matters. Rank features by real value, not gut feeling. This skill prevents the #1 killer of personal projects: trying to build everything at once.

**Core principle:** Shipping half the features at full quality beats shipping all features at half quality.

## When to Use

- Feature list feels overwhelming
- "Everything is important"
- Scope keeps growing
- Limited time, need to pick what matters most
- Deciding between v1 features
- Mid-project: need to cut scope to ship

## The Iron Law

```
NO FEATURE SURVIVES WITHOUT A BUSINESS JUSTIFICATION
```

"It would be cool" is not a justification. "Users can't accomplish [objective] without this" IS.

## The Process

### Step 1: List All Candidate Features

Gather everything — stories, features, ideas, nice-to-haves. Put them ALL in one list. No filtering yet.

Ask: "What are ALL the things you've thought about including? Don't hold back — we'll cut later."

### Step 2: Score Each Feature

For each feature, score on four dimensions (1-9 scale):

| Dimension | Score 1 | Score 9 | Who Decides |
|-----------|---------|---------|-------------|
| **Benefit** | Marginal improvement | Core value proposition | User/stakeholder |
| **Penalty** | Nobody notices if missing | Product is useless without it | User/stakeholder |
| **Cost** | Trivial to build | Major effort, weeks of work | Developer (you) |
| **Risk** | Known tech, done it before | Unknown tech, uncertain feasibility | Developer (you) |

**Ask for each feature:**
- Benefit: "On a scale of 1-9, how much value does this give to your primary user?"
- Penalty: "On a scale of 1-9, what's the consequence if we DON'T build this?"
- Cost: Estimate yourself based on complexity
- Risk: Estimate yourself based on technical uncertainty

### Step 3: Calculate Priority

```
Total Value = (Benefit * weight_b) + (Penalty * weight_p)
Total Cost = (Cost * weight_c) + (Risk * weight_r)
Priority Score = Total Value / Total Cost
```

Default weights: benefit=2, penalty=1, cost=1, risk=0.5

Present the ranked list to the user. The math is a starting point for discussion, not the final answer.

**Format:**

| # | Feature | Benefit | Penalty | Cost | Risk | Score | Tier |
|---|---------|---------|---------|------|------|-------|------|
| 1 | Search by ingredient | 9 | 8 | 3 | 2 | 7.4 | Must |
| 2 | Save favorites | 7 | 5 | 2 | 1 | 7.6 | Must |
| 3 | Share recipe | 4 | 1 | 5 | 3 | 1.4 | Won't |

### Step 4: Apply MoSCoW

Based on scores AND discussion, assign tiers:

- **Must** (max 40% of features) — product fails without these
- **Should** (max 30%) — significant value, but product works without them
- **Could** (max 20%) — nice to have, build only if time permits
- **Won't** (remaining) — explicitly cut from this release

**Forcing function:** If user puts >40% in Must, push back:

"You have 8 features marked Must out of 12. That means almost everything is critical, which means nothing is. Let's look at each Must and ask: 'If we shipped without this ONE feature, would people still use the product?' If yes, it's Should, not Must."

### Step 5: Define the MVP Cut Line

Draw a clear line:

```
=== MVP (ship this) ===
[ ] Feature A (Must)
[ ] Feature B (Must)
[ ] Feature C (Must)

=== Next Release ===
[ ] Feature D (Should)
[ ] Feature E (Should)

=== Future ===
[ ] Feature F (Could)

=== Not Doing ===
[ ] Feature G (Won't)
```

Ask: "If we shipped ONLY the MVP features tomorrow, would it solve your core problem?" If no, something critical is in the wrong tier.

### Step 6: Validate the Cut

Run sanity checks:

- [ ] Can the primary user complete their core task with MVP features only?
- [ ] Does MVP address the #1 business objective?
- [ ] Is the MVP small enough to ship in a reasonable timeframe?
- [ ] Does each Must feature have a clear business justification?
- [ ] Are "Should" features genuinely deferrable?

If any check fails, re-prioritize.

### Step 7: Save and Commit

Update `docs/requirements.md` or create `docs/priority-matrix.md` with the ranked, tiered feature list and scoring rationale.

## Scope Creep Defense

When a new idea appears mid-project:

```
1. STOP — don't add it to the current work
2. SCORE — apply the same Benefit/Penalty/Cost/Risk analysis
3. COMPARE — where does it rank against existing features?
4. TRADE — if it goes IN, what comes OUT?
5. DECIDE — add to backlog, or reject with rationale
```

**Rule:** Every new feature IN means an existing feature OUT (or a later ship date). There are no free additions.

## Key Principles

- **Smaller is better** — an MVP that ships beats a "complete" product that doesn't
- **Score, don't vote** — gut feeling is biased; scoring creates productive disagreement
- **40% rule** — max 40% of features are Must. If more, you haven't prioritized
- **Trade-offs are explicit** — every IN means something OUT or later
- **Revisit regularly** — priorities change as you learn; re-score quarterly or after major discoveries

## Red Flags — STOP

- Everything is Must → force the 40% rule
- "We need ALL of this for launch" → no you don't. Challenge each feature
- New features keep appearing → enforce scope creep defense protocol
- No features are Won't → you haven't cut hard enough
- Cost/Risk ignored → features that are hard AND low-value should die first
- "Users expect this" → which users? How do you know? Evidence required

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "We can build it all if we work harder" | Overwork produces bugs, not features. Cut scope instead. |
| "Every feature is essential" | Essential to whom? Score it. If penalty < 5, it's not essential. |
| "Competitor has this feature" | Competitors also have years of development. You're building v1. |
| "It's just a small addition" | Small additions compound into large delays. Score it. |
| "Users will be disappointed" | Users prefer a working product with 5 features over a broken one with 15. |
| "We'll lose market opportunity" | You lose more by not shipping. MVP first, iterate second. |
