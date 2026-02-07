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
| `implementer` | Coding and implementation tasks | inherit |
| `spec-reviewer` | Spec compliance verification | inherit |
| `code-reviewer` | Code quality review | inherit |
| `debugger` | Systematic debugging | inherit |

## Skill-Agent Mapping

| Skill | Role | Agent |
|-------|------|-------|
| `subagent-driven-development` | Implementer | `implementer` |
| `subagent-driven-development` | Spec Reviewer | `spec-reviewer` |
| `subagent-driven-development` | Code Quality | `code-reviewer` |
| `systematic-debugging` | Default | `debugger` |
| `requesting-code-review` | Reviewer | `code-reviewer` |

## How to Customize

Override agent models in your `opencode.json`:

```json
{
  "agent": {
    "implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "debugger": {
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
    "implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "code-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    }
  }
}
```

### Example: Use Different Providers

```json
{
  "agent": {
    "implementer": {
      "model": "openai/gpt-4o"
    },
    "code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

## Usage in Skills

When a skill prompt template specifies:

```
Task tool (subagent_type: implementer):
```

Use that exact agent name in the Task tool invocation:

```
Task(subagent_type="implementer", description="...", prompt="...")
```

The agent's configured model will be used automatically.
