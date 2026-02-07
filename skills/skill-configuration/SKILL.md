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

All superpowers agents use the `superpowers:` namespace prefix to avoid conflicts with built-in or third-party agents.

| Agent | Purpose | Default |
|-------|---------|---------|
| `superpowers:implementer` | Coding and implementation tasks | inherit |
| `superpowers:spec-reviewer` | Spec compliance verification | inherit |
| `superpowers:code-reviewer` | Code quality review | inherit |
| `superpowers:debugger` | Systematic debugging | inherit |

## Skill-Agent Mapping

| Skill | Role | Agent |
|-------|------|-------|
| `subagent-driven-development` | Implementer | `superpowers:implementer` |
| `subagent-driven-development` | Spec Reviewer | `superpowers:spec-reviewer` |
| `subagent-driven-development` | Code Quality | `superpowers:code-reviewer` |
| `systematic-debugging` | Default | `superpowers:debugger` |
| `requesting-code-review` | Reviewer | `superpowers:code-reviewer` |

## How to Customize

Override agent models in your `opencode.json`:

```json
{
  "agent": {
    "superpowers:implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "superpowers:spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "superpowers:code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "superpowers:debugger": {
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
    "superpowers:implementer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    },
    "superpowers:spec-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    },
    "superpowers:code-reviewer": {
      "model": "anthropic/claude-haiku-4-20250514"
    }
  }
}
```

### Example: Use Different Providers

```json
{
  "agent": {
    "superpowers:implementer": {
      "model": "openai/gpt-4o"
    },
    "superpowers:code-reviewer": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

## Usage in Skills

When a skill prompt template specifies:

```
Task tool (subagent_type: superpowers:implementer):
```

Use that exact agent name in the Task tool invocation:

```
Task(subagent_type="superpowers:implementer", description="...", prompt="...")
```

The agent's configured model will be used automatically.
