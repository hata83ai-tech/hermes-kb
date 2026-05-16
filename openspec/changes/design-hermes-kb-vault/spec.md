# hermes-kb Specification

## 1. Overview

**Project:** hermes-kb — Hermes Agent Skills Vault
**Type:** Forked from [heyitsnoah/claudesidian](https://github.com/heyitsnoah/claudesidian)
**License:** MIT
**Purpose:** Provide reusable skills for Hermes Agent with PARA organization + built-in tools

---

## 2. Source Attribution

| Component | Source | License |
|-----------|--------|---------|
| Skills (12) | claudesidian | MIT |
| PARA method | fortelabs.com/PARA | Public domain |
| llm-wiki | Hermes built-in | Hermes built-in |
| MarkItDown | microsoft/markitdown | MIT |

**Legal requirement:** Include attribution, preserve MIT license, state changes.

---

## 3. Approved Features

### 3.1 Built-in llm-wiki (APPROVED ✅)

**Purpose:** Agent-driven knowledge building — agent tự research và viết wiki entries.

**Usage:**
```bash
# Built-in, no installation needed
hermes skills list | grep llm-wiki
```

**Location:** Hermes built-in at `~/.hermes/skills/llm-wiki/`

### 3.2 PARA Structure (APPROVED ✅)

**Purpose:** Human-driven organization — user tổ chức notes theo Projects, Areas, Resources, Archive.

**Folders:**
```
00_Inbox/         Quick capture
01_Projects/      Time-bound projects
02_Areas/         Ongoing responsibilities
03_Resources/     Reference materials
04_Archive/       Completed/inactive
05_Attachments/   Binary files
06_Metadata/      Reference + Templates
```

### 3.3 research-assistant Skill (APPROVED ✅)

**Purpose:** Deep research workflow với structured output.

**Source:** Ported from claudesidian `.agents/skills/research-assistant/SKILL.md`
**Format:** Hermes skill (YAML frontmatter + markdown body)
**Triggers:** "research X", "investigate Y", "deep dive into Z"

### 3.4 MarkItDown (APPROVED ✅)

**Purpose:** Unified conversion tool — convert PDF, YouTube, DOCX, PPTX, XLSX → markdown.

**Installation:**
```bash
npm install -g markitdown
```

**Usage:**
```bash
markitdown https://example.com/video.mp4 --output summary.md
markitdown document.pdf --output text.md
```

---

## 4. Defered Features

### 4.1 Knowledge Freshness Layer (DEFERRED ⏸)

**Description:** Confidence scoring, supersession tracking, forgetting curve.

**Status:** Not implemented in v1.0
**Reason:** Complex, requires more research before implementation
**Tracking:** Future enhancement

---

## 5. Discarded Features

| Feature | Reason |
|---------|--------|
| vault-consolidator skill | Integrate into llm-wiki instead |
| Login handling for web scraping | Not needed, use public sources |
| kimi bridge | Not needed, Hermes built-in sufficient |

---

## 6. Skills to Port (12 total)

| # | Skill | Purpose | Status |
|---|-------|---------|--------|
| 1 | thinking-partner | Collaborative exploration | Port from claudesidian |
| 2 | systematic-debugging | Bug diagnosis | Port from claudesidian |
| 3 | pragmatic-review | Code review | Port from claudesidian |
| 4 | skill-creator | Create new skills | Port from claudesidian |
| 5 | research-assistant | Deep research | Port from claudesidian |
| 6 | daily-review | End-of-day reflection | Port from claudesidian |
| 7 | weekly-synthesis | Weekly reflection | Port from claudesidian |
| 8 | inbox-processor | Process inbox items | Port from claudesidian |
| 9 | add-frontmatter | Add YAML metadata | Port from claudesidian |
| 10 | de-ai-ify | Remove AI patterns | Port from claudesidian |
| 11 | pull-request | PR workflow | Port from claudesidian |
| 12 | release | Release management | Port from claudesidian |

---

## 7. Skill Format

```yaml
---
name: <skill-name>
description: "Use when the user [trigger]. [What it does.]"
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
---

# Skill Title

[Content]
```

**Required fields:** name, description (starts with "Use when"), license: MIT, metadata.source

---

## 8. File Structure

```
hermes-kb/
├── .agents/skills/          # 12 Hermes skills
├── 00_Inbox/                # PARA inbox
├── 01_Projects/             # PARA projects
├── 02_Areas/                # PARA areas
├── 03_Resources/            # PARA resources
├── 04_Archive/             # PARA archive
├── 05_Attachments/         # PARA attachments
├── 06_Metadata/            # PARA metadata
├── openspec/                # OpenSpec change tracking
├── LICENSE                  # MIT License
├── NOTICE                   # Source attribution
├── README.md                # Installation guide
└── .gitignore
```

---

## 9. Success Criteria

| Criterion | Verification |
|-----------|--------------|
| 12 skills in .agents/skills/ | `ls .agents/skills/ \| wc -l` = 12 |
| Each skill has valid YAML frontmatter | All parse correctly |
| Attribution to claudesidian present | grep finds source in all |
| LICENSE file exists | MIT text present |
| NOTICE file exists | Attribution present |
| PARA folders exist | All 8 folders present |
| GitHub shows fork relationship | Web UI shows "forked from" |

---

## 10. OpenSpec Change Tracking

```bash
cd hermes-kb
openspec init --tools none
openspec new change design-hermes-kb-vault
# SPEC/DESIGN/TASKS created
openspec status
openspec archive design-hermes-kb-vault  # After completion
```

---

## 11. Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-05-16 | Initial release with 12 skills, PARA structure |

---

## 12. References

- claudesidian: https://github.com/heyitsnoah/claudesidian (MIT)
- PARA Method: https://fortelabs.com/blog/para/
- MarkItDown: https://github.com/microsoft/markitdown (MIT)
- OpenSpec: https://openspec.dev