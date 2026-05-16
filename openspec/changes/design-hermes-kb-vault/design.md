# Design: hermes-kb Vault Implementation

## 1. Design Decisions (Final)

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Knowledge system | PARA + llm-wiki (combined) | User organize + agent auto-build |
| research-assistant | Port from claudesidian | Structured deep research |
| Content conversion | MarkItDown | Unified tool for PDF/YT/DOCX |
| Knowledge Freshness | DEFER | Complex, need more research |
| Consolidator skill | DISCARD | Integrate into llm-wiki instead |
| Login handling | DISCARD | Not needed for v1 |

---

## 2. Architecture

### 2.1 Repository Structure

```
hermes-kb/
├── .agents/skills/              # 12 ported skills
│   ├── thinking-partner/SKILL.md
│   ├── systematic-debugging/SKILL.md
│   ├── pragmatic-review/SKILL.md
│   ├── skill-creator/SKILL.md
│   ├── research-assistant/SKILL.md
│   ├── daily-review/SKILL.md
│   ├── weekly-synthesis/SKILL.md
│   ├── inbox-processor/SKILL.md
│   ├── add-frontmatter/SKILL.md
│   ├── de-ai-ify/SKILL.md
│   ├── pull-request/SKILL.md
│   └── release/SKILL.md
├── 00_Inbox/                    # PARA folders (empty, user creates)
├── 01_Projects/
├── 02_Areas/
├── 03_Resources/
├── 04_Archive/
├── 05_Attachments/
├── 06_Metadata/Reference/
├── 06_Metadata/Templates/
├── openspec/                    # OpenSpec change tracking
├── LICENSE                      # MIT License
├── NOTICE                       # Source attribution
├── README.md                    # Installation guide
├── SPEC.md                      # This spec
└── .gitignore
```

### 2.2 Tool Integration

**Built-in Hermes tools** (no installation):
- `llm-wiki` — knowledge building
- `search_files` — vault search
- `web_search` — web research
- `web_extract` — URL content extraction
- `browser_*` — web navigation

**External tool** (requires installation):
- `markitdown` — `npm install -g markitdown`

---

## 3. Skill Format

### 3.1 SKILL.md Structure

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

[Content following Hermes skill conventions]
```

### 3.2 Required Fields

| Field | Required | Description |
|-------|----------|-------------|
| name | YES | lowercase-with-hyphens |
| description | YES | Starts with "Use when the user" |
| license | YES | MIT |
| metadata.author | YES | hermes-kb |
| metadata.version | YES | "1.0" |
| metadata.source | YES | URL to claudesidian |

---

## 4. Workflow: Research → Store

### 4.1 Content Ingestion Flow

```
User provides: link/pdf/YouTube/video
        ↓
┌─────────────────────────────┐
│   research-assistant        │
│   - Extract key information │
│   - Cross-check facts       │
│   - Structured output       │
│     (summary, sources)      │
└─────────────────────────────┘
        ↓
┌─────────────────────────────┐
│      MarkItDown             │
│   (if PDF/DOCX/PPTX/XLSX)   │
│   - Convert to markdown     │
└─────────────────────────────┘
        ↓
┌─────────────────────────────┐
│      llm-wiki               │
│   - Store analyzed info    │
│   - Agent queries           │
└─────────────────────────────┘
        ↓
┌─────────────────────────────┐
│      PARA folders           │
│   - User manual organize   │
│   - Personal knowledge     │
└─────────────────────────────┘
```

### 4.2 When to Use Each System

| User says... | System | Why |
|--------------|--------|-----|
| "research X" | research-assistant + llm-wiki | Agent-driven deep dive |
| "find notes about Y" | search_files | Fast grep in vault |
| "organize my notes" | PARA folders | Human organization |
| "what do I know about Z" | llm-wiki query | LLM-powered recall |
| "convert this PDF" | MarkItDown | Unified conversion |

---

## 5. GitHub Fork Setup

### 5.1 Fork Relationship

```
Original:  heyitsnoah/claudesidian (upstream)
Forked:    hata83ai-tech/hermes-kb (origin)
```

### 5.2 Remote Configuration

```bash
git remote add upstream https://github.com/heyitsnoah/claudesidian.git
git remote set-url origin https://github.com/hata83ai-tech/hermes-kb.git
```

### 5.3 Verification

```bash
gh repo view hata83ai-tech/hermes-kb --json forkSource
# Should show: {"parent": {"full_name": "heyitsnoah/claudesidian"}}
```

---

## 6. Quality Assurance

### 6.1 Skill Validation

```bash
# Check all skills have valid YAML
for f in .agents/skills/*/SKILL.md; do
  python3 -c "import yaml; yaml.safe_load(open('$f').read().split('---')[1])" || echo "FAIL: $f"
done

# Check attribution
grep -l "heyitsnoah/claudesidian" .agents/skills/*/SKILL.md | wc -l
# Should return: 12

# Check skill count
ls .agents/skills/ | wc -l
# Should return: 12
```

### 6.2 Hermes Skill Loading

```bash
# Verify skills visible
hermes skills list | grep -E "thinking-partner|systematic-debugging|pragmatic-review"

# Load and test
/skill research-assistant
```

---

## 7. Implementation Phases

| Phase | Tasks | Status |
|-------|-------|--------|
| 1 | Repo setup, legal files (LICENSE, NOTICE, .gitignore) | Pending |
| 2 | Create PARA folders with README placeholders | Pending |
| 3 | Port 12 skills from claudesidian | Pending |
| 4 | Create README.md, SPEC.md | Pending |
| 5 | Validate all skills | Pending |
| 6 | Commit + push + verify fork relationship | Pending |

---

## 8. Dependencies

| Dependency | Purpose | Installation |
|------------|---------|---------------|
| git | Version control | System |
| gh CLI | GitHub operations | System |
| openspec | Change tracking | `npm install -g @fission-ai/openspec` |
| markitdown | Content conversion | `npm install -g markitdown` |

---

## 9. Deferred: Knowledge Freshness (v2)

For future enhancement:
- Confidence scoring on facts
- Supersession tracking (update without chaos)
- Forgetting curve (auto-clean stale)
- Consolidation tiers (raw → established → archived)

---

## 10. Risks & Mitigations

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| claudesidian repo deleted | Low | Content already ported |
| MIT license change | Low | License is perpetual for v1.0 |
| Skills incompatible | Low | Simple markdown format |
| GitHub fork limit | Low | Public repos only |