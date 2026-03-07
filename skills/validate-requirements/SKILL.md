---
name: validate-requirements
description: "Use before starting implementation to verify requirements are complete, correct, and testable. Use when user says 'are we ready to build', 'review the requirements', 'check if anything is missing', or before transitioning from planning to coding."
---

# Validate Requirements

## Overview

Catch requirement defects now, not during implementation. A defect found in requirements costs $1 to fix. The same defect found during testing costs $200. In production: $4,200.

**Core principle:** Requirements are code for humans. Review them with the same rigor you'd review a pull request.

## When to Use

- Before starting implementation
- After completing requirements elicitation
- Before handing off to `superpowers:brainstorming` for technical design
- When something "feels off" about the plan
- Periodically during long projects (every 2-3 weeks)

## The Iron Law

```
NO IMPLEMENTATION STARTS WITH UNVALIDATED REQUIREMENTS
```

Validated means: reviewed against checklist, gaps identified, ambiguities resolved. "Looks good to me" is not validation.

## The Process

### Phase 1: Individual Requirement Quality

Review EACH requirement/story against these criteria. Mark pass/fail for each:

**Is it COMPLETE?**
- [ ] Contains all information a developer needs to implement
- [ ] Happy path described
- [ ] Error/exception cases covered
- [ ] Edge cases identified (empty input, max values, concurrent access)
- [ ] No TBDs, placeholders, or "details to follow"
- [ ] Data requirements specified (what data, what format, what validation)

**Is it CORRECT?**
- [ ] Accurately describes what the user needs (not what they said — what they NEED)
- [ ] Consistent with business objectives in vision doc
- [ ] Doesn't contradict other requirements
- [ ] Business rules are accurate (verified, not assumed)

**Is it UNAMBIGUOUS?**

Scan for these dangerous words and replace them:

| Vague Word | Problem | Replace With |
|------------|---------|-------------|
| "fast" | How fast? | "< 2 seconds for 95th percentile" |
| "easy" | Easy for whom? | "Complete in < 3 steps without documentation" |
| "user-friendly" | Meaningless | Specific usability criteria |
| "intuitive" | Subjective | "First-time user completes task without help" |
| "flexible" | Infinite scope | List specific configuration options |
| "robust" | Vague | "Recovers from [specific failure] within [time]" |
| "efficient" | Compared to what? | "Processes N items in < X seconds" |
| "appropriate" | Who decides? | Specific criteria |
| "if possible" | Is it or isn't it? | Remove or make definitive |
| "etc." | Hiding unknowns | List all items explicitly |
| "support" | Do what exactly? | Specific integration actions |
| "handle" | How? | Describe exact behavior |
| "manage" | Vague action | List specific CRUD operations |
| "several" / "some" / "many" | How many? | Exact number or range |
| "minimize" / "maximize" | To what degree? | Specific target value |

**Is it TESTABLE?**
- [ ] Can write a pass/fail test from this requirement alone
- [ ] Acceptance criteria have specific expected values
- [ ] Quality requirements have measurable targets
- [ ] A tester who never spoke to the user could verify this

**Is it NECESSARY?**
- [ ] Traces back to a business objective
- [ ] Removing it would reduce user value
- [ ] Not a "nice to have" disguised as "must have"
- [ ] Not a solution masquerading as a requirement (HOW leaked into WHAT)

**Is it FEASIBLE?**
- [ ] Technically implementable with known technology
- [ ] Achievable within project constraints (time, budget)
- [ ] Doesn't require solving unsolved problems
- [ ] Dependencies are available and accessible

### Phase 2: Requirements Collection Quality

Review the FULL SET of requirements together:

**Completeness Check:**
- [ ] Every in-scope feature from vision doc has corresponding stories
- [ ] All user classes have stories addressing their primary tasks
- [ ] Error handling covered (what happens when things go wrong?)
- [ ] Empty states covered (first use, no data, cleared data)
- [ ] Permissions/access control defined (who can do what?)
- [ ] Data lifecycle covered (create, read, update, delete, archive)
- [ ] Quality attributes specified with targets (performance, security, usability)
- [ ] Business rules documented and linked to stories

**Consistency Check:**
- [ ] No contradicting requirements (Story A says X, Story B says not-X)
- [ ] Terminology is consistent (same thing isn't called "order" in one story and "purchase" in another)
- [ ] Priority assignments are consistent (dependencies have compatible priorities)
- [ ] Data definitions match across stories (same field, same type, same validation)

**Traceability Check:**
- [ ] Every story traces to a business objective
- [ ] Every business objective has at least one story
- [ ] Business rules link to affected stories
- [ ] Quality requirements link to relevant features

**Gap Detection — The Missing Requirements Checklist:**

These are the requirements MOST OFTEN forgotten. Check each:

- [ ] **Authentication:** How do users log in? Password rules? Session timeout?
- [ ] **Authorization:** Role-based access? What can each role do?
- [ ] **Audit/logging:** What actions are recorded? Who can view logs?
- [ ] **Data validation:** Input formats? Length limits? Allowed characters?
- [ ] **Error messages:** User-facing error text defined? Helpful or generic?
- [ ] **Loading states:** What does the user see while waiting?
- [ ] **Offline behavior:** What happens with no internet? (if applicable)
- [ ] **Data migration:** Existing data needs conversion? Import/export?
- [ ] **Notifications:** Email? Push? In-app? Triggers and templates?
- [ ] **Search/filter:** How do users find things? What's searchable?
- [ ] **Pagination:** What happens with 10,000 results?
- [ ] **Internationalization:** Multiple languages? Date/currency formats?
- [ ] **Accessibility:** Screen reader support? Keyboard navigation?
- [ ] **Backup/recovery:** What if data is lost? Recovery procedure?
- [ ] **Rate limiting:** Protection against abuse?

Not all apply to every project. But REVIEW each and explicitly mark N/A if not relevant — don't skip silently.

### Phase 3: Report Findings

Compile findings into a validation report:

```markdown
## Requirements Validation Report

**Date:** [date]
**Reviewed:** [number] stories, [number] business rules, [number] quality requirements

### Summary
- Critical issues: [count] (must fix before implementation)
- Warnings: [count] (should fix, but can start with caution)
- Suggestions: [count] (improvements, not blockers)

### Critical Issues
1. **[Story ID]:** [Description of issue]. Fix: [what to do]
2. ...

### Warnings
1. **[Story ID]:** [Description]. Recommendation: [what to do]
2. ...

### Missing Requirements
1. [Gap identified]. Recommendation: [add story / add AC / add rule]
2. ...

### Ambiguous Language Found
1. **[Story ID]:** "[vague word]" — replace with: [specific alternative]
2. ...

### Validation Passed
- [X] stories passed all quality checks
- [X] business rules verified
- [X] traceability complete
```

Present to user. Fix critical issues before proceeding.

## Quick Validation (Lightweight)

For small projects or single-story validation:

```
For each story, answer these 5 questions:
1. Can a developer implement this WITHOUT asking me questions? (Complete)
2. Can a tester write a pass/fail test from this alone? (Testable)
3. Does every reader get the same interpretation? (Unambiguous)
4. Does the user actually need this? (Necessary)
5. Can we actually build this? (Feasible)

Any "no" → fix before implementing.
```

## Key Principles

- **Review like code** — requirements deserve the same rigor as pull requests
- **Ban vague words** — every ambiguous word is a future bug
- **Check the gaps** — missing requirements cause more damage than wrong ones
- **Trace everything** — orphan requirements are scope creep in disguise
- **Fix before building** — $1 now vs $4,200 later is not a trade-off, it's a mandate

## Red Flags — STOP

- "Requirements are good enough" → "good enough" means "we'll fix it in production"
- Skipping validation "to save time" → you're borrowing time at 4200% interest
- No acceptance criteria on stories → they're wishes, not requirements
- Vague words survived review → you didn't actually review, you skimmed
- Can't trace story to objective → it shouldn't exist
- "The developer will figure it out" → the developer will guess wrong

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "We validated by discussing it" | Discussion without checklist misses 60% of defects. |
| "Requirements are clear to me" | You wrote them. Of course they're clear to YOU. |
| "We'll catch issues in testing" | Testing finds implementation bugs. Requirements bugs survive testing. |
| "This is a personal project, validation is overkill" | Personal projects have the least external feedback. Validation IS your feedback. |
| "We're agile, we'll iterate" | Iterating on wrong requirements wastes entire iterations. |
| "It takes too long" | Phase 1 takes 15 min per story. A wrong story takes days to rebuild. |
