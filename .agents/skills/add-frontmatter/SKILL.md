---
name: add-frontmatter
description: "Use when the user asks to add, fix, or improve YAML frontmatter, properties, metadata, or tags on notes."
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Add Frontmatter

Add YAML frontmatter to markdown notes for organization.

## Frontmatter Template

```yaml
---
title: <title>
created: YYYY-MM-DD
tags: []
status: draft|ready|archived
---
```

## Adding to Existing Notes

1. Read the file
2. Check if frontmatter exists
3. If yes, update; if no, add at top (before first heading)

## Common Properties

| Property | Type | Description |
|----------|------|-------------|
| title | string | Note title |
| created | date | Creation date (YYYY-MM-DD) |
| tags | array | Tags for organization |
| status | string | draft/in-progress/ready/archived |
| aliases | array | Alternative names for search |

## Example

### Before
```markdown
# My Project Notes
Some content here...
```

### After
```yaml
---
title: My Project Notes
created: 2026-05-16
tags: [project, notes]
status: in-progress
---

# My Project Notes

Some content here...
```
