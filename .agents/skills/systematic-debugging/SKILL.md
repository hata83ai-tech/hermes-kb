---
name: systematic-debugging
description: "Use when the user reports a bug, crash, error, or unexpected behavior. Systematic debugging methodology to diagnose and fix issues methodically."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Systematic Debugging

## Core Principle: The Iron Law

> **NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST**
>
> Random fixes waste time and create new bugs.

## When to Use

**Use for ANY technical issue:** test failures, bugs, unexpected behavior, performance problems, build failures.

**Use ESPECIALLY when:** under time pressure, "quick fix" seems obvious, already tried multiple fixes.

## The Four Phases

### Phase 1: Root Cause Investigation

1. **Read Error Messages Carefully** - Don't skip errors or warnings
2. **Reproduce Consistently** - Can you trigger it reliably?
3. **Check Recent Changes** - Git diff, new dependencies, config changes
4. **Trace Data Flow** - Find where bad value originates

### Phase 2: Pattern Analysis

1. Find working examples in same codebase
2. Compare against references completely
3. Identify every difference
4. Understand dependencies

### Phase 3: Hypothesis and Testing

1. Form single hypothesis: "I think X is root cause because Y"
2. Test minimally - smallest possible change
3. Verify before continuing
4. If doesn't work → form NEW hypothesis

### Phase 4: Implementation

1. Create failing test case
2. Implement single fix
3. Verify fix works

**If 3+ Fixes Failed → STOP and question architecture.**

## Red Flags - STOP

- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "I don't fully understand but this might work"
- "One more fix attempt" (when already tried 2+)

## Output Template

```
## Bug Summary
[One-line description]

## Reproduction Steps
1. ...
2. ...

## Expected vs Actual
Expected: ...
Actual: ...

## Evidence
[error logs, screenshots]

## Hypothesis
[Your best guess at root cause]

## Proposed Fix
[Code change or configuration]
```
