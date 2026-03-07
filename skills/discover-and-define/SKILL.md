---
name: discover-and-define
description: "Use BEFORE any project or feature work when requirements are unclear, when starting a new personal project, or when the user says 'I have an idea' or 'I want to build something'. Guides structured discovery from vague idea to clear vision, scope, and success metrics."
---

# Discover and Define

## Overview

Turn a vague idea into a clear product vision with defined scope, success metrics, and identified risks. This is the BA equivalent of brainstorming — but focused on WHAT to build and WHY, not HOW.

**Core principle:** No building until you know what problem you're solving and for whom.

<HARD-GATE>
Do NOT proceed to requirements elicitation, design, or implementation until you have a written vision document that the user has approved. Even "obvious" projects need this — that's where the most dangerous assumptions hide.
</HARD-GATE>

## When to Use

**Always before:**
- Starting a new personal project
- Adding a major feature
- "I have an idea for..."
- "I want to build..."
- "What if we..."

**Especially when:**
- The user is excited and wants to jump straight to code
- Scope feels fuzzy or open-ended
- There's no Jira, no PM, no spec — just you and an idea

## The Iron Law

```
NO REQUIREMENTS WITHOUT A CLEAR PROBLEM STATEMENT FIRST
```

If you can't state the problem in one sentence, you don't understand it yet.

## Checklist

Complete these in order:

1. **Understand the problem** — what pain exists today? who has it?
2. **Define the vision** — one-sentence product vision
3. **Identify users** — who are the 2-3 user types?
4. **Set business objectives** — measurable goals (not "make it good")
5. **Define success metrics** — how will you know it worked?
6. **Draw the boundary** — what's IN scope, what's OUT
7. **Identify risks** — what could go wrong?
8. **Write the vision doc** — save and get user approval

## The Process

### Step 1: Understand the Problem

Ask ONE question at a time. Do not dump a questionnaire.

**Start with:** "What problem are you trying to solve? Who has this problem today, and what do they do about it?"

Wait for the answer. Then probe deeper:

- "What happens if this problem stays unsolved?"
- "How are people dealing with this today? What's the workaround?"
- "What triggered this idea? What made you think about it now?"

**Listen for:**
- The REAL problem vs. the stated problem (users often describe solutions, not problems)
- Assumed constraints that may not exist
- Emotional drivers ("I'm frustrated that..." = strong signal)

**Anti-pattern:** User says "I want to build a todo app." Don't ask about features. Ask "What's wrong with existing todo apps for you? What specific situation makes you want your own?"

### Step 2: Define the Vision

Use this template to draft a one-sentence vision:

```
For [target user] who [has this problem],
[product name] is a [product category]
that [key benefit].
Unlike [current alternative],
our product [primary differentiator].
```

Present it to the user. Refine until they say "yes, that's it."

**Quality check:** If the vision statement could apply to 10 different products, it's too vague. Sharpen it.

### Step 3: Identify User Classes

Ask: "Who are the different types of people who will use this?"

For each user class, establish:
- **Name** — specific label (not "user")
- **Goal** — what they want to accomplish
- **Frequency** — how often they'll use it
- **Skill level** — technical sophistication
- **Priority** — which user class matters most?

Minimum: identify the PRIMARY user class. Most personal projects have 1-2.

**Favored user class:** When user classes conflict, whose needs win? Decide now, not during implementation.

### Step 4: Set Business Objectives

Ask: "If this project is wildly successful, what does that look like? How would you measure it?"

**Good objectives are SMART:**
- Specific: "Reduce time to X" not "be faster"
- Measurable: has a number or clear yes/no
- Attainable: realistic given constraints
- Relevant: tied to the actual problem
- Time-bound: "within 3 months of launch"

**Examples:**
- "Process 50 items per day instead of 10" (specific, measurable)
- "Reduce manual data entry by 80%" (measurable)
- "Ship MVP in 2 weeks" (time-bound)

**Anti-pattern:** "Make it user-friendly" is not an objective. "New users complete core task without help in under 3 minutes" IS an objective.

For personal projects, objectives can be simpler: "I want to use this daily for my own workflow" is valid — but make it concrete.

### Step 5: Define Success Metrics

For each objective, define HOW you'll measure it:

| Objective | Metric | Target | How to Measure |
|-----------|--------|--------|----------------|
| Faster processing | Items per day | 50 | Count in app |
| Less manual work | Time on task | <2 min | Stopwatch test |
| Daily usage | Active days/week | 5+ | Usage log |

**Rule:** If you can't measure it, you can't verify it. Rewrite the objective until it's measurable.

### Step 6: Draw the Boundary

This is the most critical step for personal projects where scope creep kills momentum.

**Create two lists:**

**IN scope (v1):**
- List 3-7 core features that solve the primary problem
- Each feature stated as user capability: "User can [do what]"
- Ruthlessly cut anything that isn't essential for the core problem

**OUT of scope (explicitly):**
- List features you're tempted to add but won't
- "Nice to have" features go here, not in v1
- "Future consideration" is fine — just not now

**Decision rule:** For each feature ask: "Can the product solve the core problem WITHOUT this?" If yes, it's out of v1.

**Anti-pattern:** "It would be cool to also..." — STOP. Write it in OUT of scope. You can always add it later.

### Step 7: Identify Risks and Assumptions

Ask: "What could prevent this from succeeding? What are we assuming that might not be true?"

**Common risk categories:**
- **Technical:** Can we actually build this? Any unknown tech?
- **User adoption:** Will people actually use it? How do we know?
- **Scope:** Is this bigger than it looks?
- **Dependencies:** Does this depend on external services/APIs/data?
- **Time:** Can we build this in the time we have?

For each risk: state the risk, assess likelihood (high/medium/low), and note a mitigation strategy.

**Assumptions to challenge:**
- "Users will want this" — how do you know?
- "This API exists and is free" — have you checked?
- "This will take a weekend" — have you estimated honestly?

### Step 8: Write the Vision Document

Save to `docs/vision-and-scope.md` with this structure:

```markdown
# [Product Name] — Vision and Scope

## Problem Statement
[One paragraph: what problem, who has it, why it matters]

## Vision
[One-sentence vision statement from Step 2]

## User Classes
[Table: Name | Goal | Priority]

## Business Objectives and Success Metrics
[Table: Objective | Metric | Target | How to Measure]

## Scope

### In Scope (v1)
- [Feature as user capability]
- ...

### Out of Scope
- [Feature explicitly deferred]
- ...

## Risks and Assumptions
[Table: Risk | Likelihood | Mitigation]

## Constraints
[Any known limitations: time, tech, budget]
```

Present to user for approval. Revise if needed.

## After the Vision Document

**Transition to requirements:**
- Invoke `claude-woow:elicit-requirements` skill to break scope into user stories with acceptance criteria
- The vision doc becomes your north star — every requirement must trace back to it

## Key Principles

- **One question at a time** — don't overwhelm with questionnaires
- **Problems before solutions** — understand WHY before WHAT
- **Explicit scope** — OUT of scope is as important as IN scope
- **Measurable objectives** — if you can't measure it, rewrite it
- **Challenge assumptions** — "obvious" things are where projects fail
- **Small v1** — cut scope aggressively, you can always add later

## Red Flags — STOP

- User describing solution before problem → redirect to problem
- "Let's just start coding and figure it out" → this IS figuring it out
- Everything marked "must have" → force prioritization
- No way to measure success → rewrite objectives
- Scope keeps growing → enforce the OUT list
- "It's simple, we don't need this" → simple projects fail most from unexamined assumptions

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "I already know what I want" | You know what you THINK you want. Discovery reveals what you actually need. |
| "This is a small project" | Small projects have the highest assumption-to-code ratio. |
| "I'll figure out scope as I go" | That's called scope creep. Define boundaries now. |
| "Success metrics are overkill" | Without metrics, you'll never know when you're done. |
| "I'm the only user, I know what I need" | You're the worst judge of your own needs. Write it down and challenge it. |
| "Vision docs are enterprise BS" | A one-page doc takes 20 minutes. Building the wrong thing takes weeks. |
