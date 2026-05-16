# Hermes-KB — Specification v2.0

> Final vote: Maika + Sen + Osin consensus (2026-05-16)

## System Overview

**Purpose:** Knowledge base system cho Maika (AI agent của Sếp Hà Tạ) để self-learn, compounding knowledge, và support decision-making.

**Paradigm:** PARA + llm-wiki hybrid
- **PARA** — workspace organization (Projects, Areas, Resources, Archive)
- **llm-wiki** — knowledge compounding layer (Karpathy pattern)

## Architecture

```
hermes-kb/
├── .agents/              # Skills ported từ claudesidian
│   ├── llm-wiki/        # Built-in skill (Hermes native)
│   ├── research-assistant/
│   ├── hermes-code-sync/
│   └── source-knowledge-extraction/
├── PARA/                 # Workspace organization
│   ├── 01-Projects/      # Active projects
│   ├── 02-Areas/         # Ongoing responsibilities
│   ├── 03-Resources/     # Reference materials
│   └── 04-Archive/       # Inactive items
├── kb-maika/             # Maika's own KB (Obsidian vault)
│   ├── 1-Architecture/  # Hermes source knowledge
│   ├── 2-Concepts/       # AI/ML concepts
│   ├── 3-Comparisons/    # Tool comparisons
│   └── log.md           # Activity log
└── logs/                 # Execution logs
```

## Voting Results (Final)

| Feature | Status | Priority |
|---------|--------|----------|
| Built-in llm-wiki | ✅ DONE | P0 |
| PARA structure | ✅ DONE | P0 |
| research-assistant | ✅ DONE | P1 |
| MarkItDown | ✅ DONE | P1 |
| Knowledge Freshness Layer | ⏸ DEFER | P2 |
| vault-consolidator | ❌ DROP | — |
| Periodic Review cron | ⏸ DEFER | P2 |
| Human-in-the-loop login | ❌ DROP | — |

## Core Features

### DONE Features

**1. Built-in llm-wiki**
- 3-layer: raw → wiki → schema
- Immutable sources với sha256 tracking
- Confidence scoring (high/medium/low)
- Cross-refs via [[wikilinks]]
- Obsidian-native

**2. PARA Structure**
- 4 folders: Projects, Areas, Resources, Archive
- Human-readable organization
- Integration với llm-wiki

**3. research-assistant**
- Ad-hoc research tasks
- Structured output (summary, background, key points)
- Source citation

**4. MarkItDown**
- Install: `pip install 'markitdown[all]'`
- Support: PDF, DOCX, PPTX, XLSX, YouTube, Audio
- Workflow: `markitdown <input> -o <output.md>`

### DEFER Features (When KB > 500 pages)

**5. Knowledge Freshness Layer**
- Confidence decay tracking
- Supersession mechanism
- Periodic re-validation

**6. Periodic Review cron**
- Weekly/monthly KB health check
- Lint: orphan pages, broken links, stale content
- Update freshness scores

### DROP Features

**7. vault-consolidator** — redundant (llm-wiki đã cover)
**8. Human-in-the-loop login** — Sếp bỏ qua

## Workflows

### Ingest Workflow (DONE)

```
1. Sếp gửi: PDF / YouTube link / URL
2. MarkItDown convert → .md file
3. llm-wiki ingest:
   - raw/ → immutable source
   - entities/ → extract entities
   - concepts/ → extract concepts
4. Cross-ref với existing KB
5. Update index.md + log.md
```

### Research Workflow (DONE)

```
1. Sếp ask: "Research về X"
2. research-assistant activates
3. web_search + web_extract
4. Structured output → deliver to Sếp
5. Optionally ingest vào llm-wiki
```

### Code Sync Workflow (DONE)

```
1. git pull → post-merge hook → touch ~/.hermes/.kb_sync_needed
2. cron (every 15 min) → check marker
3. Scan changed files → update KB pages
4. Report (nếu có thay đổi)
```

## Skills Inventory

| Skill | Status | Source |
|-------|--------|--------|
| llm-wiki | ✅ Active | Hermes built-in |
| research-assistant | ✅ Active | Ported from claudesidian |
| hermes-code-sync | ✅ Active | Custom |
| source-knowledge-extraction | ✅ Active | Custom |
| hermes-agent | ✅ Active | Hermes built-in |
| markitdown | ✅ Install needed | Microsoft |

## Next Steps

1. Install MarkItDown: `pip install 'markitdown[all]'`
2. Test ingest: gửi 1 PDF / YouTube link thử
3. Verify llm-wiki wikiPath config (check $WIKI_PATH env)
4. Run health check: `hermes health`

## Out of Scope (v2)

- Browser login automation
- vault-consolidator (redundant)
- Knowledge Freshness (KB còn nhỏ)
- Periodic Review (add later when needed)