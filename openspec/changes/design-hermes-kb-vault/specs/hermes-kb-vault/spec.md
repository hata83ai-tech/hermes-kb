# Hermes Knowledge Base (hermes-kb) — Specification

## 1. Overview

**Project Name:** hermes-kb
**Type:** Hermes Agent skills vault
**Source:** Forked from [heyitsnoah/claudesidian](https://github.com/heyitsnoah/claudesidian) (MIT License)
**License:** MIT
**Purpose:** Provide reusable skills for Hermes Agent, organized using PARA method

---

## 2. Source Attribution

| Item | Original | Adaptation |
|------|----------|------------|
| Skills content | claudesidian `.agents/skills/` | Ported to Hermes format |
| PARA method | [fortelabs.com/PARA](https://fortelabs.com/blog/para/) | Same implementation |
| Vault structure | Claudesidian vault layout | Adapted for Hermes |

**Claudesidian License (MIT):**
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software... to deal in the Software without restriction...

**Our obligation:**
- Include original copyright notices
- State changes made
- Preserve MIT license on adapted content

---

## 3. Requirements

### 3.1 Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-01 | Port 12 skills from claudesidian to Hermes format | MUST | See skill list below |
| FR-02 | Implement PARA folder structure | MUST | 00_Inbox through 06_Metadata |
| FR-03 | Create valid SKILL.md for each skill | MUST | YAML frontmatter + markdown body |
| FR-04 | Include attribution to claudesidian source | MUST | In each skill metadata |
| FR-05 | Provide README with installation instructions | MUST | Copy skills to ~/.hermes/skills/ |
| FR-06 | Create LICENSE file (MIT) | MUST | Include claudesidian attribution |
| FR-07 | Add NOTICE file acknowledging source | MUST | List all sourced materials |

### 3.2 Skills to Port

| # | Skill Name | Source File | Purpose |
|---|------------|-------------|---------|
| 1 | thinking-partner | `.agents/skills/thinking-partner/SKILL.md` | Collaborative exploration |
| 2 | systematic-debugging | `.agents/skills/systematic-debugging/SKILL.md` | Bug diagnosis methodology |
| 3 | pragmatic-review | `.agents/skills/pragmatic-review/SKILL.md` | Code review |
| 4 | skill-creator | `.agents/skills/skill-creator/SKILL.md` | Create new skills |
| 5 | research-assistant | `.agents/skills/research-assistant/SKILL.md` | Deep research |
| 6 | daily-review | `.agents/skills/daily-review/SKILL.md` | End-of-day reflection |
| 7 | weekly-synthesis | `.agents/skills/weekly-synthesis/SKILL.md` | Weekly reflection |
| 8 | inbox-processor | `.agents/skills/inbox-processor/SKILL.md` | Process inbox items |
| 9 | add-frontmatter | `.agents/skills/add-frontmatter/SKILL.md` | Add YAML metadata |
| 10 | de-ai-ify | `.agents/skills/de-ai-ify/SKILL.md` | Remove AI patterns |
| 11 | pull-request | `.agents/skills/pull-request/SKILL.md` | PR workflow |
| 12 | release | `.agents/skills/release/SKILL.md` | Release management |

---

## 4. Design

### 4.1 Vault Folder Structure

```
hermes-kb/
├── 00_Inbox/                    # Quick capture, temporary holding
├── 01_Projects/                 # Time-bound projects with goals
├── 02_Areas/                    # Ongoing responsibilities
├── 03_Resources/                # Reference materials & topics
├── 04_Archive/                  # Completed/inactive items
├── 05_Attachments/              # Images, PDFs, files
├── 06_Metadata/
│   ├── Reference/               # Documentation & guides
│   └── Templates/               # Reusable templates
├── .agents/
│   └── skills/                  # Hermes Agent skills
│       └── <skill-name>/
│           └── SKILL.md
├── openspec/                    # OpenSpec change tracking
│   ├── specs/                   # Source of truth specs
│   └── changes/                # Proposed/implemented changes
├── LICENSE                      # MIT License
├── NOTICE                       # Source attribution
├── README.md                    # Installation & usage
└── .gitignore                   # Git ignore patterns
```

### 4.2 Skill File Format

Every skill follows this exact structure:

```yaml
---
name: <skill-name>
description: "Use when the user [trigger condition]. [What it does.]"
license: MIT
metadata:
  author: hermes-kb
  version: "1.0"
  source: https://github.com/heyitsnoah/claudesidian
  ported_from: <original-skill-name>
---

# Skill Title

[Content following conventions]
```

**Required fields:**
- `name` — lowercase-with-hyphens, max 3 words
- `description` — starts with "Use when the user..."
- `metadata.source` — URL to claudesidian source

**Optional fields:**
- `metadata.ported_from` — original skill name if renamed

### 4.3 Hermes Skill Metadata Fields

| Field | Required | Description |
|-------|---------|-------------|
| name | YES | Skill identifier |
| description | YES | Trigger condition and purpose |
| license | YES | MIT |
| metadata.author | YES | hermes-kb |
| metadata.version | YES | Semver string |
| metadata.source | YES | Source URL |
| metadata.ported_from | NO | Original name if changed |

---

## 5. Implementation Tasks

### Phase 1: Repository Setup
- [ ] Create LICENSE file (MIT with claudesidian attribution)
- [ ] Create NOTICE file listing sources
- [ ] Create .gitignore
- [ ] Create empty PARA folders with .gitkeep or placeholder

### Phase 2: Skill Porting (12 skills)
- [ ] Port thinking-partner
- [ ] Port systematic-debugging
- [ ] Port pragmatic-review
- [ ] Port skill-creator
- [ ] Port research-assistant
- [ ] Port daily-review
- [ ] Port weekly-synthesis
- [ ] Port inbox-processor
- [ ] Port add-frontmatter
- [ ] Port de-ai-ify
- [ ] Port pull-request
- [ ] Port release

### Phase 3: Documentation
- [ ] Create README.md with installation instructions
- [ ] Create SPEC.md documenting vault structure
- [ ] Verify all skills load in Hermes

### Phase 4: Publishing
- [ ] Commit all changes with meaningful messages
- [ ] Push to GitHub
- [ ] Verify fork relationship shown on GitHub

---

## 6. Success Criteria

| Criterion | Verification |
|-----------|--------------|
| All 12 skills exist with valid SKILL.md | `ls .agents/skills/` shows 12 folders |
| Each skill has correct YAML frontmatter | `grep "^name:" .agents/skills/*/SKILL.md` works |
| Attribution to claudesidian present | `grep "heyitsnoah/claudesidian" .agents/skills/*/SKILL.md` |
| LICENSE file exists with MIT | `cat LICENSE` shows MIT license |
| NOTICE file exists with sources | `cat NOTICE` lists claudesidian |
| README exists with install instructions | `cat README.md` has installation steps |
| Fork relationship on GitHub | GitHub shows "forked from heyitsnoah/claudesidian" |

---

## 7. Legal Compliance

### MIT License Requirements (from claudesidian):
1. ✅ Redistribute source code with copyright notice
2. ✅ Include license text in redistribution
3. ✅ Source code may be redistributed in binary form with attribution

### Our compliance:
- LICENSE file contains MIT license text
- NOTICE file explicitly credits heyitsnoah/claudesidian
- Each SKILL.md metadata shows source URL
- README credits original project

---

## 8. OpenSpec Change Tracking

This spec is managed via OpenSpec:

```bash
cd hermes-kb
openspec status                    # View change status
openspec show design-hermes-kb-vault  # View this change
openspec archive design-hermes-kb-vault  # Archive after completion
```