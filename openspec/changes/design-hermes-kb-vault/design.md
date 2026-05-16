# Design: hermes-kb Vault

## 1. Design Decisions

### 1.1 Fork Strategy

**Decision:** Fork claudesidian repository, then apply customizations

**Alternatives Considered:**
| Approach | Pros | Cons |
|----------|------|------|
| Fork then customize | Clean fork relationship on GitHub | Must manage upstream |
| Independent repo + attribution | Full control | Lose fork recognition |
| Clone and republish | Simple | No GitHub fork relationship |

**Selected:** Fork then customize — maintains GitHub fork relationship and proper attribution.

### 1.2 Skill Porting Strategy

**Decision:** Copy-paste content from claudesidian, then adapt for Hermes format

**Rationale:**
- Skills are markdown text (not code) — minimal technical adaptation needed
- Only need to ensure Hermes-compatible frontmatter
- Attribution preserved via metadata.source field

**Adaptations Required:**
1. Add Hermes-compatible YAML frontmatter
2. Add `metadata.source` pointing to claudesidian
3. No content changes (content is MIT-licensed for use)
4. Ensure skill name uses Hermes convention (lowercase-with-hyphens)

### 1.3 Folder Structure Design

**Decision:** Maintain claudesidian PARA structure with hermes-kb additions

**Rationale:**
- PARA method is project-agnostic — works for any knowledge management
- claudesidian structure is well-proven
- Adding `.agents/skills/` for Hermes compatibility
- Adding `openspec/` for change tracking

### 1.4 License Strategy

**Decision:** MIT License for hermes-kb, with NOTICE file

**License Distribution:**
- **LICENSE file:** Full MIT license text
- **NOTICE file:** Explicit attribution to claudesidian per MIT requirement
- **SKILL.md files:** Each includes `license: MIT` in frontmatter

---

## 2. Architecture

### 2.1 Repository Structure

```
hermes-kb/
├── .github/                      # GitHub config (optional)
├── .agents/                      # Agent skills directory
│   └── skills/                   # Hermes skills
│       ├── <skill-1>/
│       │   └── SKILL.md
│       └── <skill-N>/
│           └── SKILL.md
├── 00_Inbox/                     # Quick capture (PARA)
├── 01_Projects/                   # Projects (PARA)
├── 02_Areas/                      # Areas (PARA)
├── 03_Resources/                  # Resources (PARA)
├── 04_Archive/                    # Archive (PARA)
├── 05_Attachments/                # Attachments (PARA)
├── 06_Metadata/                   # Metadata (PARA)
│   ├── Reference/
│   └── Templates/
├── openspec/                      # OpenSpec change tracking
│   ├── specs/                     # Main specs (source of truth)
│   └── changes/                  # Change proposals
├── LICENSE                       # MIT License
├── NOTICE                        # Source attribution
├── README.md                     # Installation guide
├── SPEC.md                       # Specification document
├── .gitignore                    # Git ignore
└── CHANGELOG.md                   # Version history
```

### 2.2 Skill Internal Structure

```
<skill-name>/
└── SKILL.md                       # Single file per skill

SKILL.md structure:
├── YAML Frontmatter
│   ├── name: <skill-name>
│   ├── description: "Use when..."
│   ├── license: MIT
│   └── metadata:
│       ├── author: hermes-kb
│       ├── version: "1.0"
│       ├── source: https://github.com/heyitsnoah/claudesidian
│       └── ported_from: <original-name-if-renamed>
└── Markdown Body
    ├── # Skill Title
    └── Content sections...
```

### 2.3 GitHub Fork Relationship

```
Original:  heyitsnoah/claudesidian (upstream)
Forked:    hata83ai-tech/hermes-kb (origin)

After fork, configure:
git remote add upstream https://github.com/heyitsnoah/claudesidian.git

This enables:
- git fetch upstream  (sync from original)
- GitHub shows "forked from heyitsnoah/claudesidian"
```

---

## 3. Security & Access Considerations

### 3.1 No Sensitive Data

- No API keys in repository
- No passwords or secrets
- No user-specific information
- Public repo is safe

### 3.2 File Permissions

- All files:644 (readable)
- LICENSE, NOTICE, README:644
- Skills:644
- .gitignore:644

---

## 4. Quality Assurance

### 4.1 Skill Validation Checklist

For each skill, verify:

| Check | Method |
|-------|--------|
| Valid YAML frontmatter | Parse with `python3 -c "import yaml..."` |
| name field exists | `grep "^name:" SKILL.md` |
| description starts with "Use when" | Manual check |
| license field is MIT | `grep "license: MIT" SKILL.md` |
| source metadata points to claudesidian | `grep "heyitsnoah/claudesidian" SKILL.md` |
| File parses as valid markdown | `pandoc --to markdown SKILL.md` |

### 4.2 Hermes Skill Loading Test

```bash
# After installing skills
hermes skills list | grep <skill-name>  # Should appear
/skill <skill-name>                      # Should load
```

---

## 5. Implementation Sequence

### Phase 1: Repository Foundation
1. Fork claudesidian (done)
2. Clone to local workspace
3. Rename to hermes-kb
4. Set upstream remote
5. Create initial commit

### Phase 2: Legal Files
1. Create LICENSE (MIT text)
2. Create NOTICE (claudesidian attribution)
3. Verify license compatibility

### Phase 3: Folder Structure
1. Create PARA folders (00_Inbox through 06_Metadata)
2. Add .gitkeep or README in each
3. Create .agents/skills/ directory
4. Create openspec/ structure

### Phase 4: Skill Porting
1. For each of 12 skills:
   a. Read claudesidian source
   b. Create new SKILL.md with Hermes frontmatter
   c. Copy content (with minor formatting if needed)
   d. Add attribution metadata
   e. Validate YAML frontmatter

### Phase 5: Documentation
1. Create README.md (installation instructions)
2. Create SPEC.md (this document)
3. Create CHANGELOG.md

### Phase 6: Publishing
1. Commit with meaningful message
2. Push to origin
3. Verify GitHub fork relationship shown
4. Verify skills installable

---

## 6. Risk Mitigation

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| claudesidian repo deleted | Low | Content already in our vault |
| MIT license terms change | Low | License is perpetual for code at commit time |
| GitHub fork limit reached | Low | Only forking from public repos we own |
| Skills incompatible with Hermes | Low | Format is simple markdown, no code |
| Upstream merge conflicts | Low | hermes-kb is independent, not syncing upstream |

---

## 7. OpenSpec Artifacts

| Artifact | Location | Status |
|----------|----------|--------|
| SPEC.md | `openspec/specs/hermes-kb-vault/spec.md` | Draft |
| DESIGN.md | `openspec/changes/design-hermes-kb-vault/design.md` | This file |
| TASKS.md | `openspec/changes/design-hermes-kb-vault/tasks.md` | Pending |

Use `openspec status` to view overall change status.