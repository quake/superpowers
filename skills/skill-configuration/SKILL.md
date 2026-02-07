---
name: skill-configuration
description: Use when customizing which models superpowers skills use - provides agent mapping for skill-specific model configuration
---

# Skill Configuration

Configure which models different superpowers skills use by overriding agent settings.

## Overview

Superpowers defines specialized agents for different tasks. By default, all agents use `model: inherit` (inheriting from your primary agent). You can override this in your `opencode.json` to use different models for different tasks.

This allows you to:
- Use powerful models for complex implementation tasks
- Use faster/cheaper models for lightweight review tasks
- Customize models without modifying superpowers files

## Available Agents

| Agent | Purpose | Default |
|-------|---------|---------|
| `sp-implementer` | Coding and implementation tasks | inherit |
| `sp-spec-reviewer` | Spec compliance verification | inherit |
| `sp-code-reviewer` | Code quality review | inherit |
| `sp-debugger` | Systematic debugging | inherit |

## Skill-Agent Mapping

| Skill | Role | Agent |
|-------|------|-------|
| `subagent-driven-development` | Implementer | `sp-implementer` |
| `subagent-driven-development` | Spec Reviewer | `sp-spec-reviewer` |
| `subagent-driven-development` | Code Quality | `sp-code-reviewer` |
| `systematic-debugging` | Default | `sp-debugger` |
| `requesting-code-review` | Reviewer | `sp-code-reviewer` |

## How to Customize

Override agent models in your `opencode.json`:

```json
{
  "agent": {
    "sp-implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "sp-spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "sp-code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "sp-debugger": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

### Example: Cost-Optimized Setup

Use a powerful model for implementation, cheaper models for reviews:

```json
{
  "agent": {
    "sp-implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "sp-spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "sp-code-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    }
  }
}
```

### Example: Use Different Providers

```json
{
  "agent": {
    "sp-implementer": {
      "model": "openai/gpt-4o"
    },
    "sp-code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

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
