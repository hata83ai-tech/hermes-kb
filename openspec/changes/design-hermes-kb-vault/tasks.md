# Tasks: hermes-kb Vault Implementation

## Pre-flight Checklist

Before starting, verify:
- [ ] Fork exists: `gh repo list hata83ai-tech | grep hermes-kb`
- [ ] Local clone exists: `ls ~/workspace/hermes-kb`
- [ ] Upstream remote configured: `git remote -v`
- [ ] OpenSpec CLI installed: `which openspec`

---

## Phase 1: Repository Foundation

### Task 1.1: Configure Git Remotes
```bash
cd ~/workspace/hermes-kb
git remote add upstream https://github.com/heyitsnoah/claudesidian.git
git remote -v  # Verify: origin → hata83ai-tech/hermes-kb, upstream → heyitsnoah/claudesidian
```
**Verify:** Output shows both remotes correctly

### Task 1.2: Verify Fork Relationship on GitHub
```bash
gh repo view hata83ai-tech/hermes-kb --json forkSource
```
**Verify:** JSON output contains `"parent"` with claudesidian URL

---

## Phase 2: Legal Files

### Task 2.1: Create LICENSE File
**File:** `LICENSE`
**Content:** Standard MIT License text
```markdown
MIT License

Copyright (c) 2026 hata83ai-tech

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software... [full MIT text]
```

### Task 2.2: Create NOTICE File
**File:** `NOTICE`
**Content:**
```markdown
# Notice

hermes-kb is forked from claudesidian by heyitsnoah/claudesidian.

claudesidian is licensed under the MIT License.

Original claudesidian repository: https://github.com/heyitsnoah/claudesidian

This project ports claudesidian skills to Hermes Agent format.
All ported content retains MIT License.
```

### Task 2.3: Create .gitignore
**File:** `.gitignore`
**Content:**
```
# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
*.swp
*~

# OpenSpec (user-specific)
.openspec.yaml

# Temp
*.tmp
*.log
```

---

## Phase 3: Folder Structure

### Task 3.1: Create PARA Folders
```bash
cd ~/workspace/hermes-kb
mkdir -p 00_Inbox 01_Projects 02_Areas 03_Resources 04_Archive 05_Attachments
mkdir -p 06_Metadata/Reference 06_Metadata/Templates
mkdir -p .agents/skills
```

### Task 3.2: Create Folder README Placeholders
Each PARA folder needs a README so git tracks it:
```bash
echo "# Inbox - Quick capture" > 00_Inbox/README.md
echo "# Projects - Time-bound initiatives" > 01_Projects/README.md
echo "# Areas - Ongoing responsibilities" > 02_Areas/README.md
echo "# Resources - Reference materials" > 03_Resources/README.md
echo "# Archive - Completed items" > 04_Archive/README.md
echo "# Attachments - Binary files" > 05_Attachments/README.md
echo "# Reference - Documentation" > 06_Metadata/Reference/README.md
echo "# Templates - Reusable templates" > 06_Metadata/Templates/README.md
echo "# Hermes Agent Skills" > .agents/skills/README.md
```

---

## Phase 4: Skill Porting

For each skill, follow this template:

### Skill Template
```
Source: https://github.com/heyitsnoah/claudesidian/tree/main/.agents/skills/<name>
Target: ~/workspace/hermes-kb/.agents/skills/<name>/SKILL.md
```

### Task 4.1: thinking-partner
```bash
# Create directory
mkdir -p ~/workspace/hermes-kb/.agents/skills/thinking-partner

# Create SKILL.md with:
# - YAML frontmatter (name, description, license, metadata)
# - Content from claudesidian .agents/skills/thinking-partner/SKILL.md
# - Add metadata.source: https://github.com/heyitsnoah/claudesidian
```

### Task 4.2: systematic-debugging
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/systematic-debugging
# Create SKILL.md
```

### Task 4.3: pragmatic-review
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/pragmatic-review
# Create SKILL.md
```

### Task 4.4: skill-creator
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/skill-creator
# Create SKILL.md
```

### Task 4.5: research-assistant
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/research-assistant
# Create SKILL.md
```

### Task 4.6: daily-review
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/daily-review
# Create SKILL.md
```

### Task 4.7: weekly-synthesis
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/weekly-synthesis
# Create SKILL.md
```

### Task 4.8: inbox-processor
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/inbox-processor
# Create SKILL.md
```

### Task 4.9: add-frontmatter
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/add-frontmatter
# Create SKILL.md
```

### Task 4.10: de-ai-ify
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/de-ai-ify
# Create SKILL.md
```

### Task 4.11: pull-request
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/pull-request
# Create SKILL.md
```

### Task 4.12: release
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/release
# Create SKILL.md
```

---

## Phase 5: Documentation

### Task 5.1: Create README.md
**File:** `README.md`
**Sections:**
1. Project title + description
2. Fork attribution
3. Features list
4. Installation instructions
5. Skills list
6. PARA method overview
7. License

### Task 5.2: Create SPEC.md
**File:** `SPEC.md`
**Sections:** (already created in openspec/specs/)

### Task 5.3: Create CHANGELOG.md
**File:** `CHANGELOG.md`
```markdown
# Changelog

## [1.0.0] - YYYY-MM-DD

### Added
- Initial release
- 12 skills ported from claudesidian
- PARA folder structure
- MIT License with attribution
- NOTICE file
```

---

## Phase 6: Validation

### Task 6.1: Validate YAML Frontmatter
```bash
cd ~/workspace/hermes-kb
for f in .agents/skills/*/SKILL.md; do
  python3 -c "import yaml; yaml.safe_load(open('$f').read().split('---')[1])" || echo "FAIL: $f"
done
```

### Task 6.2: Verify Skill Count
```bash
ls .agents/skills/ | wc -l  # Should be 12
```

### Task 6.3: Verify Attribution
```bash
grep -r "heyitsnoah/claudesidian" .agents/skills/*/SKILL.md | wc -l  # Should be 12
```

### Task 6.4: Verify LICENSE and NOTICE
```bash
cat LICENSE | grep -i "MIT"      # Should show MIT license
cat NOTICE | grep "claudesidian" # Should show attribution
```

---

## Phase 7: Publishing

### Task 7.1: Stage All Files
```bash
cd ~/workspace/hermes-kb
git add -A
git status  # Review staged files
```

### Task 7.2: Commit
```bash
git commit -m "feat: initial hermes-kb vault with 12 skills

- Forked from heyitsnoah/claudesidian
- Ported 12 skills to Hermes format
- Added PARA folder structure
- Added MIT LICENSE and NOTICE files
- Closes #1
"
```

### Task 7.3: Push to GitHub
```bash
git push origin main
```

### Task 7.4: Verify GitHub Fork Relationship
1. Go to https://github.com/hata83ai-tech/hermes-kb
2. Verify banner shows "forked from heyitsnoah/claudesidian"

### Task 7.5: Archive Change
```bash
cd ~/workspace/hermes-kb
openspec archive design-hermes-kb-vault
openspec status
```

---

## Post-Implementation Verification

| Check | Command | Expected |
|-------|---------|----------|
| GitHub fork shown | Web UI | "forked from heyitsnoah/claudesidian" |
| Skills count | `ls .agents/skills/ \| wc -l` | 12 |
| LICENSE present | `cat LICENSE` | MIT text |
| NOTICE present | `cat NOTICE` | claudesidian attribution |
| All skills have YAML | Task 6.1 | No errors |
| All skills have attribution | Task 6.3 | 12 matches |

---

## Rollback Plan

If issues occur:
```bash
# Undo last commit
git reset --soft HEAD~1

# Or remove repo and start fresh
cd ~/workspace && rm -rf hermes-kb
gh repo delete hata83ai-tech/hermes-kb --yes
```