---
name: elicit-requirements
description: "Use after discover-and-define to break a vision/scope into concrete user stories with acceptance criteria. Use when user says 'what should we build', 'break this down', 'create stories', or 'what are the requirements'. Produces implementation-ready specifications."
---

# Elicit Requirements

## Overview

Transform a vision document into concrete, testable, prioritized user stories with acceptance criteria. This is where "what to build" becomes "exactly what to implement."

**Core principle:** Every requirement must be traceable to a business objective, testable by a developer, and understood by the user.

<HARD-GATE>
Do NOT write requirements without a vision/scope document or at minimum a clear problem statement. If none exists, invoke `claude-woow:discover-and-define` first.
</HARD-GATE>

## When to Use

- After `discover-and-define` produces a vision document
- When you have a scope but no implementation-ready stories
- When requirements are vague: "it should be fast", "users can manage things"
- When user says "break this down into tasks"

## The Iron Law

```
NO REQUIREMENT WITHOUT ACCEPTANCE CRITERIA
```

If you can't write a test for it, it's not a requirement — it's a wish.

## Checklist

Complete in order:

1. **Load context** — read vision doc, understand scope and objectives
2. **Map user flows** — identify core tasks per user class
3. **Write user stories** — one capability per story, INVEST criteria
4. **Define acceptance criteria** — specific, testable conditions per story
5. **Identify business rules** — extract policies, calculations, constraints
6. **Specify quality requirements** — performance, security, usability with targets
7. **Prioritize** — Value/Cost/Risk analysis
8. **Validate** — review stories against vision, check for gaps
9. **Save requirements doc** — get user approval

## The Process

### Step 1: Load Context

Read the vision document. Extract and confirm:
- Business objectives and success metrics
- User classes (who uses this?)
- In-scope features
- Known constraints

Ask: "Before we break this down — has anything changed since the vision doc? Any new ideas or concerns?"

### Step 2: Map User Flows

For each in-scope feature, ask:

"Walk me through what [user class] does step by step. Start from when they first need this feature to when they're done."

Capture as a simple flow:
```
1. User opens [screen/page]
2. User sees [information]
3. User does [action]
4. System responds with [result]
5. User confirms [outcome]
```

**Probe for the non-obvious:**
- "What happens if something goes wrong at step 3?"
- "What if the data isn't there yet?"
- "Is there a shortcut for experienced users?"
- "How often does this happen? Once a day? Once a month?"

**Listen for business rules:** "Oh, but only if the order is over $50" — that's a rule, capture it separately.

### Step 3: Write User Stories

**Format:**
```
As a [specific user class],
I want to [concrete action],
so that [measurable benefit tied to business objective].
```

**INVEST criteria — every story must be:**

| Criteria | Question | Fix if No |
|----------|----------|-----------|
| **I**ndependent | Can this be built without other stories? | Split dependencies |
| **N**egotiable | Is there room to discuss HOW? | Remove implementation details |
| **V**aluable | Does user get value from this alone? | Merge with another story |
| **E**stimable | Can a developer estimate effort? | Add detail or split |
| **S**mall | Can this ship in one iteration? | Split into smaller stories |
| **T**estable | Can you write a pass/fail test? | Add acceptance criteria |

**Good stories:**
```
As a home cook,
I want to search recipes by ingredients I have,
so that I reduce food waste by using what's in my fridge.
```

**Bad stories:**
```
As a user, I want the system to be good.
(Who? What action? What measurable benefit? All wrong.)
```

**Story decomposition:** If a story is too big (epic), split by:
- User workflow steps (search → select → save)
- Data variations (text input → file upload → API import)
- Business rule complexity (basic case → edge cases → error handling)
- User class (admin version → regular user version)

### Step 4: Define Acceptance Criteria

For EACH story, write 3-7 acceptance criteria in Given/When/Then format:

```
GIVEN [precondition — system state before action]
WHEN [action — what the user does]
THEN [observable result — what should happen]
```

**Example:**
```
Story: As a home cook, I want to search recipes by ingredients I have.

AC1: GIVEN I have entered "tomato, basil, mozzarella"
     WHEN I press Search
     THEN I see recipes containing ALL three ingredients, sorted by match percentage

AC2: GIVEN I have entered an ingredient with no matching recipes
     WHEN I press Search
     THEN I see "No recipes found" with suggestions for similar ingredients

AC3: GIVEN I have entered nothing
     WHEN I press Search
     THEN the Search button is disabled

AC4: GIVEN I have entered ingredients
     WHEN results appear
     THEN each result shows: recipe name, match %, missing ingredients, cook time
```

**Rules for good acceptance criteria:**
- Each criterion tests ONE thing
- Observable from the outside (no "system internally does X")
- Specific values, not vague ("shows results" → "shows up to 20 results sorted by match %")
- Cover happy path AND edge cases AND error cases
- A developer can write an automated test from this

**Anti-pattern:** "System works correctly" is not an acceptance criterion. Neither is "User is satisfied."

### Step 5: Identify Business Rules

Business rules are policies, calculations, or constraints that exist INDEPENDENTLY of any feature. Extract them into a separate section.

**Types:**

| Type | Example | How to Document |
|------|---------|-----------------|
| **Computation** | "Discount = 10% if order > $100" | Formula with variables |
| **Constraint** | "Password must be 8+ chars with number" | Explicit rule statement |
| **Policy** | "Free shipping over $50" | If/then statement |
| **Fact** | "Each user has exactly one primary address" | Declarative statement |

**Format:** `BR-001: [Rule statement]. Source: [where this comes from].`

**Why separate?** Rules change independently of features. A rule like "tax rate is 7%" affects multiple stories. Change it once, trace to all affected stories.

**Link rules to stories:** Each story references which business rules apply. Each rule lists which stories it affects.

### Step 6: Specify Quality Requirements

For each quality attribute that matters, write a measurable requirement using Planguage:

```
QR-001: Search Response Time
  Scale: Seconds from button press to results displayed
  Target: < 2 seconds for 95% of queries
  Minimum: < 5 seconds for 99% of queries
  Measurement: Automated performance test with 100 concurrent users
```

**Key quality attributes to consider:**

| Attribute | Question to Ask | Example Target |
|-----------|----------------|----------------|
| **Performance** | "How fast should X respond?" | < 2s response |
| **Availability** | "When must this be accessible?" | 99.5% uptime |
| **Security** | "Who should NOT access this?" | Auth required, data encrypted |
| **Usability** | "How quickly should a new user learn this?" | Complete core task in < 3 min |
| **Scalability** | "How much data/users over time?" | 10K users by year 2 |

**Rule:** Vague quality requirements are worthless. "Fast" means nothing. "< 2 seconds for 95th percentile" means everything.

For personal projects, keep it simple: 2-3 quality requirements for the things that matter most.

### Step 7: Prioritize

Use Value/Cost/Risk scoring for each story:

| Story | Benefit (1-9) | Penalty if Missing (1-9) | Cost (1-9) | Risk (1-9) | Priority Score |
|-------|--------------|-------------------------|------------|------------|----------------|
| Search by ingredient | 9 | 8 | 3 | 2 | **High** |
| Share recipe | 4 | 1 | 5 | 3 | **Low** |

**Formula:** `Priority = (Benefit + Penalty) / (Cost + Risk)`

Higher score = implement first.

**Simplified MoSCoW for personal projects:**
- **Must** — product is useless without this
- **Should** — important but product still works without it
- **Could** — nice to have, do if time permits
- **Won't (v1)** — explicitly deferred

**Rule:** If everything is "Must," you haven't prioritized. Force yourself: max 40% of stories are Must.

### Step 8: Validate

Before finalizing, run this checklist:

**Completeness:**
- [ ] Every in-scope feature has at least one story
- [ ] Every story has acceptance criteria
- [ ] Error cases and edge cases are covered
- [ ] Quality requirements are specified for critical attributes

**Traceability:**
- [ ] Every story traces back to a business objective
- [ ] Every business rule links to affected stories
- [ ] No story exists without a business justification

**Quality:**
- [ ] Each story is INVEST-compliant
- [ ] Acceptance criteria are testable (Given/When/Then)
- [ ] No ambiguous words: "fast," "easy," "intuitive," "user-friendly," "flexible"
- [ ] No implementation details in stories (HOW leaked into WHAT)

**Gaps:**
- [ ] What happens on first use? (empty states)
- [ ] What happens when things go wrong? (error handling)
- [ ] What data is needed? (data requirements)
- [ ] Who can do what? (permissions/access)

Ask the user: "Looking at these stories, does anything feel missing? Any scenario we haven't covered?"

### Step 9: Save Requirements Document

Save to `docs/requirements.md`:

```markdown
# [Product Name] — Requirements

**Vision:** [one-line from vision doc]
**Date:** [date]

## User Stories

### Must Have
#### US-001: [Story title]
**As a** [user], **I want to** [action], **so that** [benefit].
**Priority:** Must | **Estimate:** S/M/L
**Business Rules:** BR-001, BR-003

**Acceptance Criteria:**
1. GIVEN ... WHEN ... THEN ...
2. GIVEN ... WHEN ... THEN ...

[repeat for each story]

### Should Have
[stories...]

### Could Have
[stories...]

## Business Rules
- BR-001: [rule]
- BR-002: [rule]

## Quality Requirements
- QR-001: [requirement with target]

## Traceability
| Story | Business Objective | Business Rules |
|-------|-------------------|----------------|
| US-001 | Reduce waste by 50% | BR-001, BR-003 |
```

Present to user for review. Revise until approved.

## After Requirements

**Transition to implementation:**
- Requirements feed directly into `superpowers:brainstorming` for technical design
- Or into `superpowers:writing-plans` if technical approach is already clear
- Each user story with acceptance criteria becomes a task in the implementation plan

## Key Principles

- **One story, one capability** — split stories that say "and"
- **Acceptance criteria are tests** — if a developer can't automate it, rewrite it
- **Business rules live separately** — they outlive individual stories
- **Trace everything** — every story justifies its existence via business objective
- **Prioritize ruthlessly** — 40% Must, 30% Should, 30% Could
- **Challenge vagueness** — ban "fast," "easy," "user-friendly," "flexible," "robust"

## Red Flags — STOP

- Story has no acceptance criteria → write them before moving on
- Acceptance criterion says "works correctly" → make it specific
- All stories are "Must" → force ranking
- Story contains implementation details ("use React", "store in PostgreSQL") → remove
- No error/edge cases → add them
- Can't trace story to business objective → question if it's needed
- User says "just make it work" → ask "how will you know it works?"

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Acceptance criteria slow us down" | Vague requirements slow you down 10x more during implementation. |
| "I'll know it when I see it" | That means unlimited revisions. Define "done" now. |
| "We can add details later" | Later never comes. Or it comes during debugging. |
| "Every story is critical" | If everything is critical, nothing is. Prioritize. |
| "Business rules are obvious" | Obvious rules have the most edge cases. Write them down. |
| "Quality requirements are overkill for my project" | "It should be fast" during review means rework. "< 2 seconds" means done. |
