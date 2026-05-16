# hermes-kb Vault — Specification

## Overview

**Project Name:** hermes-kb
**Type:** Hermes Agent skills vault
**Source:** Forked from [heyitsnoah/claudesidian](https://github.com/heyitsnoah/claudesidian) (MIT License)
**License:** MIT
**Purpose:** Provide reusable skills for Hermes Agent, organized using PARA method, enhanced with built-in llm-wiki and MarkItDown.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    hermes-kb                            │
├──────────────────────┬──────────────────────────────────┤
│   Built-in Tools     │      Ported Skills              │
├──────────────────────┼──────────────────────────────────┤
│ • llm-wiki           │ • thinking-partner               │
│ • search_files       │ • systematic-debugging           │
│ • web_search         │ • pragmatic-review               │
│ • MarkItDown (npm)   │ • skill-creator                  │
│ • browser tools      │ • research-assistant             │
│                      │ • daily-review                   │
│                      │ • weekly-synthesis               │
│                      │ • inbox-processor                │
│                      │ • add-frontmatter                │
│                      │ • de-ai-ify                      │
│                      │ • pull-request                   │
│                      │ • release                        │
└──────────────────────┴──────────────────────────────────┘
```

**Two Complementary Systems:**
- **llm-wiki** (built-in): Agent-driven knowledge building — agent tự viết/update notes
- **PARA** (manual): Human-driven organization — user tổ chức folders

---

## Decision Summary

| Feature | Status | Notes |
|---------|--------|-------|
| Built-in llm-wiki | ✅ APPROVED | Feature-complete, use as-is |
| PARA structure | ✅ APPROVED | Workspace organization |
| research-assistant | ✅ APPROVED | Port from claudesidian |
| MarkItDown | ✅ APPROVED | Unified conversion (PDF, YT, etc.) |
| Knowledge Freshness Layer | ⏸ DEFERRED | Complex, defer to v2 |
| vault-consolidator skill | ❌ DISCARDED | Integrate into llm-wiki instead |
| Login handling | ❌ DISCARDED | Not needed for v1 |

---

## Skills Specification

### 12 Skills to Port

All skills follow Hermes skill format:

```yaml
---
name: <name>
description: "Use when the user [trigger]. [What it does.]"
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---
```

| # | Skill Name | Trigger Example | Purpose |
|---|------------|------------------|---------|
| 1 | thinking-partner | "think about this" | Collaborative exploration |
| 2 | systematic-debugging | "debug this bug" | Structured bug diagnosis |
| 3 | pragmatic-review | "review this code" | Practical code review |
| 4 | skill-creator | "create a skill" | Create new Hermes skills |
| 5 | research-assistant | "research X" | Deep research workflow |
| 6 | daily-review | "daily review" | End-of-day reflection |
| 7 | weekly-synthesis | "weekly synthesis" | Weekly reflection |
| 8 | inbox-processor | "process inbox" | GTD inbox processing |
| 9 | add-frontmatter | "add metadata" | Add YAML frontmatter |
| 10 | de-ai-ify | "make it human" | Remove AI patterns |
| 11 | pull-request | "open a PR" | PR workflow |
| 12 | release | "create a release" | Release management |

---

## PARA Folder Structure

```
00_Inbox/           # Quick capture, temporary holding
01_Projects/        # Time-bound initiatives with goals
02_Areas/           # Ongoing responsibilities (no end date)
03_Resources/       # Topics of interest, reference materials
04_Archive/         # Completed/inactive items
05_Attachments/     # Binary files (images, PDFs, etc.)
06_Metadata/        # System files
  ├── Reference/    # Documentation and guides
  └── Templates/    # Reusable templates
```

---

## Tools Used

### Built-in Hermes Tools

| Tool | Purpose |
|------|---------|
| `llm-wiki` | Agent-driven knowledge building and query |
| `search_files` | Search content in vault |
| `web_search` | Search the web |
| `web_extract` | Extract content from URLs |
| `browser_*` | Web navigation if needed |

### External Tool

| Tool | Purpose | Install |
|------|---------|---------|
| `markitdown` | Convert PDF, YouTube, DOCX, PPTX, XLSX to markdown | `npm install -g markitdown` |

---

## Workflow: Research → Store

```
User Input (link/pdf/YT)
        ↓
┌──────────────────────────────┐
│   research-assistant         │
│   - Deep analysis            │
│   - Structured output        │
│   - Source citations         │
└──────────────────────────────┘
        ↓
┌──────────────────────────────┐
│      llm-wiki               │
│   - Store analyzed info     │
│   - Cross-reference          │
│   - Query when needed        │
└──────────────────────────────┘
        ↓
┌──────────────────────────────┐
│   PARA folders              │
│   - Human organize          │
│   - Manual placement        │
└──────────────────────────────┘
```

---

## Legal Compliance

### MIT License Requirements

1. ✅ Include copyright notice
2. ✅ Include license text
3. ✅ State changes made
4. ✅ Include NOTICE file with attribution

### Implementation

- **LICENSE:** Full MIT text
- **NOTICE:** Explicitly credits heyitsnoah/claudesidian
- **Each SKILL.md:** Contains `metadata.source` pointing to claudesidian
- **README.md:** Credits original project

---

## Deferred: Knowledge Freshness Layer (v2)

For future enhancement:

- **Confidence scoring** — biết fact nào đáng tin
- **Supersession** — update mà không loạn
- **Forgetting curve** — tự clean stale info
- **Consolidation tiers** — raw → established

---

## Version

| Version | Date | Description |
|---------|------|-------------|
| 1.0.0 | 2026-05-16 | Initial spec with 12 skills, PARA, llm-wiki, MarkItDown |