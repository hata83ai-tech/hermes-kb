# Tasks: hermes-kb Implementation

## Pre-flight Checklist

Before starting, verify:
- [ ] Fork exists: `gh repo list hata83ai-tech | grep hermes-kb`
- [ ] Local clone exists: `ls ~/workspace/hermes-kb`
- [ ] Upstream remote: `git remote -v`
- [ ] OpenSpec CLI: `which openspec`

---

## Phase 1: Repository Setup

### Task 1.1: Create LICENSE
**File:** `LICENSE`
```markdown
MIT License

Copyright (c) 2026 hata83ai-tech
Copyright (c) 2024 heyitsnoah (for claudesidian components)

Permission is hereby granted, free of charge, to any person obtaining a copy...
```

### Task 1.2: Create NOTICE
**File:** `NOTICE`
```markdown
# Notice

hermes-kb is derived from claudesidian by heyitsnoah/claudesidian.

claudesidian is licensed under the MIT License.

Original: https://github.com/heyitsnoah/claudesidian

All claudesidian-derived content retains MIT License.
```

### Task 1.3: Create .gitignore
**File:** `.gitignore`
```
# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
*.swp
*~

# OpenSpec
.openspec.yaml

# Temp
*.tmp
*.log
```

---

## Phase 2: Folder Structure

### Task 2.1: Create PARA Folders
```bash
cd ~/workspace/hermes-kb
mkdir -p 00_Inbox 01_Projects 02_Areas 03_Resources 04_Archive 05_Attachments
mkdir -p 06_Metadata/Reference 06_Metadata/Templates
mkdir -p .agents/skills
```

### Task 2.2: Create Folder README Placeholders
```bash
echo "# Quick capture - process regularly" > 00_Inbox/README.md
echo "# Time-bound projects with clear outcomes" > 01_Projects/README.md
echo "# Ongoing responsibilities" > 02_Areas/README.md
echo "# Reference materials and topics" > 03_Resources/README.md
echo "# Completed/inactive items" > 04_Archive/README.md
echo "# Binary files (images, PDFs)" > 05_Attachments/README.md
echo "# Documentation and guides" > 06_Metadata/Reference/README.md
echo "# Reusable templates" > 06_Metadata/Templates/README.md
echo "# Hermes Agent Skills" > .agents/skills/README.md
```

---

## Phase 3: Port 12 Skills

For each skill:
1. Read source from claudesidian
2. Create directory
3. Create SKILL.md with Hermes frontmatter + original content
4. Verify YAML parses

### Task 3.1: thinking-partner
```bash
mkdir -p ~/workspace/hermes-kb/.agents/skills/thinking-partner
# Create SKILL.md with:
# - Frontmatter: name, description (Use when...), license: MIT, metadata.source
# - Body: from claudesidian .agents/skills/thinking-partner/SKILL.md
```

### Task 3.2: systematic-debugging
### Task 3.3: pragmatic-review
### Task 3.4: skill-creator
### Task 3.5: research-assistant
### Task 3.6: daily-review
### Task 3.7: weekly-synthesis
### Task 3.8: inbox-processor
### Task 3.9: add-frontmatter
### Task 3.10: de-ai-ify
### Task 3.11: pull-request
### Task 3.12: release

**Skill Frontmatter Template:**
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

[Content from claudesidian with minor formatting adjustments]
```

---

## Phase 4: Documentation

### Task 4.1: Create README.md
**File:** `README.md`
**Sections:**
1. Title + fork attribution
2. Features (PARA, llm-wiki, 12 skills, MarkItDown)
3. Installation
4. Skills list with descriptions
5. Workflow diagram
6. License

### Task 4.2: Create SPEC.md
**File:** `SPEC.md`
Copy content from `openspec/specs/hermes-kb-vault/spec.md`

---

## Phase 5: Validation

### Task 5.1: Validate YAML Frontmatter
```bash
cd ~/workspace/hermes-kb
for f in .agents/skills/*/SKILL.md; do
  python3 -c "import yaml; yaml.safe_load(open('$f').read().split('---')[1])" || echo "FAIL: $f"
done
```

### Task 5.2: Verify Skill Count
```bash
ls .agents/skills/ | wc -l  # Should be 12
```

### Task 5.3: Verify Attribution
```bash
grep -l "heyitsnoah/claudesidian" .agents/skills/*/SKILL.md | wc -l  # Should be 12
```

### Task 5.4: Verify LICENSE and NOTICE
```bash
grep -i "MIT" LICENSE
grep "claudesidian" NOTICE
```

---

## Phase 6: Publish

### Task 6.1: Stage and Commit
```bash
cd ~/workspace/hermes-kb
git add -A
git status
git commit -m "feat: initial hermes-kb vault

- Forked from heyitsnoah/claudesidian
- 12 skills ported to Hermes format
- PARA folder structure
- MIT License with attribution
- MarkItDown integration

Closes #1"
```

### Task 6.2: Push
```bash
git push origin main
```

### Task 6.3: Verify Fork Relationship
1. Go to https://github.com/hata83ai-tech/hermes-kb
2. Confirm banner shows "forked from heyitsnoah/claudesidian"

### Task 6.4: Install Skills Locally
```bash
cp -r ~/workspace/hermes-kb/.agents/skills/* ~/.hermes/skills/
hermes skills list | grep -E "thinking-partner|systematic-debugging|pragmatic-review"
```

### Task 6.5: Archive Change
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
| All YAML valid | Task 5.1 | No errors |
| All have attribution | Task 5.3 | 12 matches |
| Skills load | `/skill research-assistant` | Loads successfully |

---

## Rollback

```bash
# Undo commit
git reset --soft HEAD~1

# Or remove and start fresh
cd ~/workspace && rm -rf hermes-kb
gh repo delete hata83ai-tech/hermes-kb --yes
```