# Plugin Authoring Standards

Personal standards for building Claude Code plugins (skills, agents, slash commands). Goal: keep auto-loaded context minimal while staying scalable as the plugin count grows.

## Core Principle: BMAD-Style

> **One entry point exposed to Claude. Everything else lives on disk and loads just-in-time via the Read tool.**

Whatever the plugin does, expose ONE skill, agent, or slash command as the public entry. All workflows, templates, checklists, references, and stage scripts live in subdirectories that the entry point reads when needed.

**Why:** Every skill/agent/command description sits in the main context window the moment the plugin is enabled. Hidden subfolders cost zero context until explicitly read.

## Repository Layouts

Two valid layouts. Pick based on scope.

### Single-plugin repo (default)

Use when the repo is one focused tool. Example: `claude-woow`.

```
repo-root/
├── .claude-plugin/
│   ├── marketplace.json        ← marketplace metadata (1 plugin)
│   └── plugin.json             ← plugin metadata
├── README.md
├── agents/
│   └── <name>.md               ← optional, 1 orchestrator agent
├── skills/
│   └── <skill-name>/
│       ├── SKILL.md            ← orchestrator (short frontmatter description)
│       └── <content-dir>/      ← stages/, references/, templates/, examples/
│           └── *.md            ← loaded only on demand
├── commands/                    ← optional, 1 slash shortcut
│   └── <name>.md
└── docs/
    └── STANDARDS.md             ← this file
```

### Multi-plugin marketplace repo

Use when grouping related but independent plugins under one domain. Example: `nazar-qa-tools` (contains `test-plan-generator` + `spec-compliance-verifier`).

```
repo-root/
├── .claude-plugin/
│   └── marketplace.json        ← lists multiple plugins, pluginRoot: "./plugins"
├── README.md
└── plugins/
    ├── <plugin-1>/
    │   ├── .claude-plugin/plugin.json
    │   └── skills/<name>/SKILL.md + content-dir/
    └── <plugin-2>/
        ├── .claude-plugin/plugin.json
        └── skills/<name>/SKILL.md + content-dir/
```

## Choosing the Entry Type

| Entry | When to use | Context cost (description) |
|-------|-------------|----------------------------|
| **Skill** | Auto-discovery via trigger phrases. Claude proposes it when user says matching things. | 150-300 chars |
| **Agent** | Delegated work in isolated context. Use when output is heavy or you don't want it polluting main context. | 500-1500 chars (with examples) |
| **Slash command** | Manual invocation only. User types `/name`. | 50-150 chars |

**Default to skill** for auto-discoverable workflows. **Use agent** when the work produces big intermediate output. **Use slash command** when you only invoke manually and want minimum context cost.

## Description Field Rules

The `description:` in frontmatter is the ONLY thing always in context. Treat it like ad copy with a strict word budget.

**DO:**
- Lead with WHEN to use ("Use when ...")
- Include 3-5 trigger phrases ("user says X, Y, Z")
- State the output ("Produces ...")
- 150-250 chars total

**DON'T:**
- Repeat the WHAT (the skill name tells you that)
- Include implementation details
- Add multi-sentence prose
- Bloat with rationale

**Good example (claude-woow:ba-workflow):**
> "Business analysis workflow with 5 stages: discover, elicit, prioritize, validate, change-impact. Pass stage name as argument. Use for taking a personal project or feature from vague idea through validated implementation-ready requirements."

## Content Subdirectory Conventions

Standard names for the on-disk content folder:

| Folder | Purpose |
|--------|---------|
| `stages/` | Sequential workflow steps (discover → elicit → ...) |
| `references/` | Concept docs, lookups, format specs |
| `templates/` | Output document templates |
| `examples/` | Worked examples to study |
| `schemas/` | JSON/YAML schemas |
| `scripts/` | Helper shell/python scripts |
| `checklists/` | Standalone QA checklists |

Pick whichever fits. Don't introduce a new folder name without reason — consistency across plugins matters.

## The Orchestrator Pattern

SKILL.md should be short. Its job is to **route**, not **contain**.

Template:

```markdown
---
name: <skill-name>
description: "<150-250 chars: when, triggers, output>"
---

# <Skill Name>

<2-3 sentences: what this skill does at the top level>

## How to Run

1. Read the `args` value passed when the skill was invoked. Extract <param>.
2. If <param> is missing, ask the user.
3. Read the corresponding file from `<content-dir>/<param>.md`.
4. Follow the instructions inside that file exactly.

## Available <Param Options>

| Option | When to Use | Output |
|--------|-------------|--------|
| ... | ... | ... |

## Core Principles (cross-cutting only)

<things that apply to ALL paths through the skill>
```

Keep SKILL.md under 100 lines. Every stage/reference document goes in its own subdirectory file.

## Naming Conventions

**Repos:**
- Domain plugins: `claude-<domain>` (e.g., `claude-woow` for BA) — owned by you
- Tool-first plugins (CLI + Claude integration): `<tool-name>` (e.g., `tarn`)
- Marketplace groupings: `<scope>-<category>` (e.g., `nazar-qa-tools`)

**Plugins (inside multi-plugin marketplaces):**
- `<tool-name>` — concise, no prefix needed since marketplace scopes it
- Example: `nazar-qa-tools:test-plan-generator`, not `nazar-qa-tools:nazar-test-plan-generator`

**Skills/agents/commands within a plugin:**
- Match the plugin name when there's only one (e.g., `tarn-api-testing` skill in `tarn` plugin)
- Use verb-noun for action skills (`elicit-requirements`, `validate-spec`)
- Use noun for orchestrators (`ba-workflow`, `test-plan-generator`)

## Versioning

- Use SemVer in `plugin.json` `version` field
- Bump MINOR when adding new stages/references that are backward compatible
- Bump MAJOR when changing the entry point's args or removing capabilities
- Tag releases in git (`git tag v1.2.0 && git push --tags`)

## Local Testing

Before pushing:

```bash
# Symlink your local plugin into Claude's marketplace cache,
# or add it as a local marketplace in ~/.claude/settings.json:
#   "marketplaces": [{"name": "dev", "source": "/path/to/your/repo"}]
# Then enable the plugin in /config → Plugins.
```

## Publishing Checklist

- [ ] `README.md` explains purpose, install instructions, basic usage
- [ ] `description` fields are ≤250 chars and follow rules above
- [ ] SKILL.md body is short (orchestrator only); bulky content lives in subdirectories
- [ ] No secrets in code or examples
- [ ] `LICENSE` file present (MIT for personal stuff)
- [ ] Tagged with semver
- [ ] If multi-plugin marketplace: every plugin works independently when others are disabled

## Anti-Patterns (Don't Do These)

- **Inline workflow into SKILL.md** → blows up SKILL.md size; harder to maintain
- **One skill per workflow stage** (e.g., 5 separate skills) → 5× the description cost in context
- **Verbose `<example>` blocks in skill descriptions** → eats hundreds of chars
- **Repeating the skill name inside its description** → wasted budget
- **Auto-discovery skill for something you only invoke manually** → use slash command instead

## Reference Implementations

Study these for patterns:

| Repo | Layout | Why it's good |
|------|--------|---------------|
| [`claude-woow`](https://github.com/NazarKalytiuk/claude-woow) | Single-plugin | Stage-based orchestrator (`ba-workflow` with 5 stages) |
| [`tarn`](https://github.com/NazarKalytiuk/tarn) | Single-plugin, tool + skill | 1 skill (`tarn-api-testing`) + `references/` subfolder |
| [`test-plan-generator`](https://github.com/NazarKalytiuk/test-plan-generator) | Multi-plugin marketplace (`nazar-qa-tools`) | Two plugins under one marketplace, each with its own subfolders |
