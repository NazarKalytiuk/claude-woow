---
name: ba-workflow
description: "Business analysis workflow with 5 stages: discover, elicit, prioritize, validate, change-impact. Pass stage name as argument. Use for taking a personal project or feature from vague idea through validated implementation-ready requirements."
---

# BA Workflow

Stage-based business analysis workflow. Single entry point for all BA work.

## Usage

Invoke with a stage argument:

- `discover` — vague idea → vision & scope document
- `elicit` — vision → implementation-ready user stories with acceptance criteria
- `prioritize` — feature list → ranked MoSCoW priority matrix
- `validate` — requirements → validation report before implementation
- `change-impact` — proposed mid-project change → impact analysis & recommendation

## How to Run

1. Read the `args` value passed when the skill was invoked. Extract the stage name (e.g. `discover`, `elicit`, `prioritize`, `validate`, `change-impact`).
2. If no stage was passed or the value is ambiguous, ask the user which stage they need. Match these triggers:
   - "I have an idea", "I want to build", "what should we build" → **discover**
   - "break this down", "create stories", "what are the requirements" → **elicit**
   - "what should we do first", "too many features", "help me prioritize" → **prioritize**
   - "are we ready to build", "review the requirements", "check if anything is missing" → **validate**
   - "what if we change", "can we add", "I want to modify" → **change-impact**
3. Read the corresponding stage file from `stages/<stage>.md` (relative to this SKILL.md location):
   - `discover` → `stages/discover.md`
   - `elicit` → `stages/elicit.md`
   - `prioritize` → `stages/prioritize.md`
   - `validate` → `stages/validate.md`
   - `change-impact` → `stages/change-impact.md`
4. Follow the instructions inside that stage file exactly. Each stage file is self-contained with its own checklist, process, principles, and red flags.

## Recommended Workflow Order

Most personal projects flow through stages in this order:

```
discover → elicit → prioritize → validate → [implementation]
                                              ↓
                                       change-impact (when changes appear)
```

After `validate` passes, hand off to `superpowers:brainstorming` or `superpowers:writing-plans` for technical design.

## Output Artifacts by Stage

| Stage | Output |
|-------|--------|
| discover | `docs/vision-and-scope.md` |
| elicit | `docs/requirements.md` |
| prioritize | `docs/priority-matrix.md` (or update requirements.md) |
| validate | Validation report (inline or `docs/validation-report.md`) |
| change-impact | Impact analysis + updates to existing docs |

## Core Principles Across All Stages

- **One question at a time** — never dump questionnaires on the user
- **Problems before solutions** — understand WHY before WHAT
- **Testable specifics** — ban "fast", "easy", "user-friendly", "robust", "flexible"
- **Trace everything** — every story to a business objective, every rule to affected stories
- **Document decisions** — future you needs to know why

## Hard Gates Between Stages

- No `elicit` without an approved vision document (or run `discover` first)
- No `validate` until requirements doc exists
- No implementation handoff until `validate` passes critical issues check
- No mid-project change implemented without `change-impact` analysis
