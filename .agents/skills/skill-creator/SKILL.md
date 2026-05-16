---
name: skill-creator
description: "Use when asked to create a skill, plugin, or extend Hermes capabilities. Meta-skill for creating new Hermes skills."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Skill Creator

Create new skills for Hermes Agent. Follow this process for quality.

## When to Create a Skill

- Complex task performed repeatedly
- Procedure discovered through trial and error
- Workflow with multiple steps
- User explicitly asks for a new capability

## Skill Structure

Every skill lives in `<name>/SKILL.md`:

```yaml
---
name: <name>
description: Use when the user [specific action]. [What it does.]
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
---

# Skill Title

[Content]
```

## Creation Process

1. **Identify the Skill** - What triggers it? What output?
2. **Write SKILL.md** - Include trigger, steps, examples
3. **Place it in** `.agents/skills/<name>/SKILL.md`
4. **Test** - Load and verify it works

## Skill Naming

- lowercase-with-hyphens
- action-oriented
- max 3 words

## Good Description Template

> "Use when the user [specific action or request]. [What it does and when to use it.]"

## Anti-Patterns

- Skills that do too much (split into smaller)
- Skills with vague triggers
- Skills without examples
