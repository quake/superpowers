---
name: skill-configuration
description: Use when customizing which models superpowers skills use - provides agent mapping for skill-specific model configuration
---

# Skill Configuration

Configure which models different superpowers skills use by mapping skills to specialized agents.

## Overview

Superpowers uses preconfigured agents with specific models for different tasks. This allows you to:
- Use powerful models for complex implementation tasks
- Use faster/cheaper models for lightweight review tasks
- Customize models per skill without modifying skill files

## Configuration File

The mapping is defined in `skill-config.json` at the repository root:

```json
{
  "agents": {
    "sp-implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "sp-spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    }
  },
  "skill_agent_mapping": {
    "subagent-driven-development": {
      "implementer": "sp-implementer",
      "spec-reviewer": "sp-spec-reviewer"
    }
  }
}
```

## Agent Definitions

Agent files are in `agents/` directory:

| Agent | Default Model | Purpose |
|-------|---------------|---------|
| `sp-implementer` | claude-sonnet-4 | Coding and implementation tasks |
| `sp-spec-reviewer` | claude-haiku-4 | Spec compliance verification |
| `sp-code-reviewer` | claude-sonnet-4 | Code quality review |
| `sp-debugger` | claude-sonnet-4 | Systematic debugging |

## Skill-Agent Mapping

| Skill | Role | Agent |
|-------|------|-------|
| `subagent-driven-development` | Implementer | `sp-implementer` |
| `subagent-driven-development` | Spec Reviewer | `sp-spec-reviewer` |
| `subagent-driven-development` | Code Quality | `sp-code-reviewer` |
| `systematic-debugging` | Default | `sp-debugger` |
| `requesting-code-review` | Reviewer | `sp-code-reviewer` |

## How to Customize

### Change a Model

Edit the agent file in `agents/`:

```markdown
---
name: sp-implementer
model: openai/gpt-4o  # Change this line
---
```

Or override in your `opencode.json`:

```json
{
  "agent": {
    "sp-implementer": {
      "model": "openai/gpt-4o"
    }
  }
}
```

### Add a New Agent

1. Create `agents/sp-my-agent.md` with frontmatter
2. Add mapping in `skill-config.json`
3. Reference in skill prompt templates

## Usage in Skills

When a skill prompt template specifies:

```
Task tool (subagent_type: sp-implementer):
```

Use that exact agent name in the Task tool invocation:

```
Task(subagent_type="sp-implementer", description="...", prompt="...")
```

The agent's configured model will be used automatically.
