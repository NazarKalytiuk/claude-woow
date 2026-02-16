# claude-woow

Custom agents and skills plugin for Claude Code.

## Agents

- **business-analyst** — Senior business analyst for requirements gathering, process analysis, and stakeholder management

## Adding New Agents

Drop a `.md` file in `agents/` with YAML frontmatter:

```yaml
---
name: agent-name
description: "When to use this agent..."
model: sonnet
---

Agent instructions here...
```

## Adding New Skills

Create `skills/<skill-name>/SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name
description: "When to use this skill..."
---

Skill reference guide here...
```

## Installation

```bash
claude /install /path/to/claude-woow
```
