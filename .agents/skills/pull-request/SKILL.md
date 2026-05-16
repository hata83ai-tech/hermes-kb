---
name: pull-request
description: "Use when the user asks to open a PR, create a pull request, or push changes for review."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Pull Request Workflow

Create and manage pull requests effectively.

## Before Opening a PR

- [ ] Code compiles/builds
- [ ] Tests pass locally
- [ ] No debug code or TODOs left
- [ ] Commits are meaningful
- [ ] Documentation updated

## PR Description Template

```markdown
## Summary
[What does this PR do?]

## Changes
- [List of specific changes]

## Testing
- [How was this tested?]

## Related Issues
Closes #[number]
```

## Best Practices

### Title
- Imperative mood: "Add feature" not "Added feature"
- Under 72 characters

### Size
- Small PRs review faster
- If >500 lines, consider splitting

## Merge Strategies

| Strategy | When to Use |
|----------|-------------|
| Squash | Small PRs, clean history |
| Merge | Preserving commits |
| Rebase | Linear history |
