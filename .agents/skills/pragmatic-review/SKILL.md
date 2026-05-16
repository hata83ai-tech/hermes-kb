---
name: pragmatic-review
description: "Use when asked to review code, PRs, or doing self-review before committing. Practical code review focusing on correctness, clarity, and maintainability."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Pragmatic Code Review

Focus on what matters: correctness, clarity, and maintainability.

## Review Priorities

### Critical (Always Check)
1. **Correctness** - Does it do what it's supposed to?
2. **Security** - SQL injection, XSS, exposed secrets
3. **Edge cases** - Null checks, empty inputs, boundary conditions

### Important (Usually Check)
1. **Readability** - Can you understand it without the author?
2. **Error handling** - Are errors caught appropriately?
3. **Testing** - Are there tests?

## Review Process

1. **Understand Context** - What does this PR do? Why?
2. **Read the Code** - Read diff once without commenting
3. **Comment Constructively** - Phrase as questions, not commands

## Comment Format

```
[BLOCKING] <issue> - <explanation>
[SUGGESTION] <issue> - <explanation>
[QUESTION] <why this approach?>
```

## Self-Review Checklist

Before asking for review:
- [ ] Tests pass locally
- [ ] No debug code left in
- [ ] No secrets committed
- [ ] Commit messages are meaningful
