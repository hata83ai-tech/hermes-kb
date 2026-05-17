---
name: kb-builder
description: "Use when the user wants to build, organize, or understand a knowledge base. Use to discover available skills, plan KB workflows, select appropriate tools, or guide skill selection for any knowledge management task. NOT for general help -- use help skill instead."
version: "1.0"
author: hermes-kb
license: MIT
metadata:
  source: https://github.com/hata83ai-tech/hermes-kb
  triggers:
    - "build knowledge base"
    - "create vault"
    - "organize notes"
    - "which skill"
    - "what skills"
    - "how to use hermes-kb"
    - "kb-builder"
    - "convert to markdown"
    - "ingest document"
    - "process file"
---

# kb-builder

Knowledge base builder meta-skill -- guides you through building, organizing, and managing knowledge bases using the hermes-kb vault.

## Section 1: Vault Index

The hermes-kb vault contains 12 skills. Here's the complete inventory:

| # | Skill | Purpose | Trigger Keywords | Location |
|---|-------|---------|------------------|----------|
| 1 | thinking-partner | Collaborative exploration, brainstorming | "think", "brainstorm", "explore idea", "consider" | `.agents/skills/thinking-partner/SKILL.md` |
| 2 | systematic-debugging | Structured bug diagnosis | "debug", "fix bug", "error", "trace", "issue" | `.agents/skills/systematic-debugging/SKILL.md` |
| 3 | pragmatic-review | Practical code review | "review", "check code", "feedback", "evaluate" | `.agents/skills/pragmatic-review/SKILL.md` |
| 4 | skill-creator | Create new Hermes skills | "create skill", "make skill", "new skill", "define workflow" | `.agents/skills/skill-creator/SKILL.md` |
| 5 | research-assistant | Deep research workflow | "research", "investigate", "find info", "analyze" | `.agents/skills/research-assistant/SKILL.md` |
| 6 | daily-review | End-of-day reflection and triage | "daily", "standup", "end of day", "reflect" | `.agents/skills/daily-review/SKILL.md` |
| 7 | weekly-synthesis | Weekly reflection and summary | "weekly", "summary", "week review", "recap" | `.agents/skills/weekly-synthesis/SKILL.md` |
| 8 | inbox-processor | GTD-style inbox processing | "process inbox", "import notes", "capture", "organize" | `.agents/skills/inbox-processor/SKILL.md` |
| 9 | add-frontmatter | Add YAML frontmatter to notes | "format", "add metadata", "frontmatter", "standardize" | `.agents/skills/add-frontmatter/SKILL.md` |
| 10 | de-ai-ify | Humanize text, remove AI patterns | "humanize", "natural", "de-ai", "make readable" | `.agents/skills/de-ai-ify/SKILL.md` |
| 11 | pull-request | PR workflow management | "pr", "pull request", "merge", "submit" | `.agents/skills/pull-request/SKILL.md` |
| 12 | release | Release management | "release", "publish", "version", "changelog" | `.agents/skills/release/SKILL.md` |

---

## Section 2: Trigger Decision Tree

When user says something like "build knowledge base", "help me organize", or "convert this file", follow this tree:

```
START: User wants to [build/organize/convert/access] knowledge base
        |
        v
+---------------------------------------------------------------+
| Q1: What is the user asking for?                              |
+---------------------------------------------------------------+
| "build new KB" / "create vault" / "start project"             |
|    -> Go to Q2                                                |
|                                                                |
| "organize notes" / "process inbox" / "import files"           |
|    -> inbox-processor -> add-frontmatter                      |
|                                                                |
| "convert file" / "ingest document" / "PDF to markdown"        |
|    -> Go to Content Ingestion (Section 5)                     |
|                                                                |
| "find info" / "search vault" / "what do I know about"         |
|    -> search_files (built-in) or llm-wiki query               |
|                                                                |
| "research topic" / "investigate" / "find out about"          |
|    -> research-assistant                                      |
|                                                                |
| "create skill" / "define workflow" / "make new tool"          |
|    -> skill-creator                                           |
|                                                                |
| "review code" / "check this" / "give feedback"                |
|    -> pragmatic-review                                        |
|                                                                |
| "debug issue" / "fix bug" / "something's wrong"                |
|    -> systematic-debugging                                     |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| Q2: What is the source?                                       |
+---------------------------------------------------------------+
| PDF, DOCX, XLSX, PPTX, EPUB -> MarkItDown                     |
| Audio (mp3, wav, ogg, m4a) -> MarkItDown                      |
| YouTube URL -> MarkItDown                                     |
| Web URL -> web_extract                                        |
| Existing markdown -> Pass through                              |
| Plain text -> Pass through                                    |
| Folder of mixed files -> Batch MarkItDown                     |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| Q3: PARA or llm-wiki?                                        |
+---------------------------------------------------------------+
| "save to my notes" / "organize project" / "personal"          |
|    -> inbox-processor -> PARA folders                         |
|                                                                |
| "agent knowledge" / "remember this" / "build KB for later"    |
|    -> research-assistant -> llm-wiki store                    |
|                                                                |
| "what do I know about X" / "query knowledge"                  |
|    -> llm-wiki query                                          |
+---------------------------------------------------------------+
        |
        v
PLAN -> APPROVE -> EXECUTE
```

---

## Section 3: Workflow Recipes

### Recipe 1: Build KB from Scratch

Use when: user wants to build a new knowledge base from research or source materials.

```
1. research-assistant
   -> Gather source materials on target topic
   -> Output: structured research notes with citations

2. inbox-processor
   -> Organize raw research into notes
   -> Output: categorized notes in PARA structure

3. add-frontmatter
   -> Add YAML frontmatter to all notes
   -> Output: standardized metadata (title, tags, date)

4. skill-creator
   -> Identify patterns in notes -> create reusable skills
   -> Output: new SKILL.md files ready for vault

5. de-ai-ify
   -> Clean content to natural language
   -> Output: human-readable, no AI patterns

6. pragmatic-review
   -> Verify quality of all outputs
   -> Output: error-checked, consistent vault
```

### Recipe 2: Port Existing Knowledge

Use when: user has existing files, notes, or documents to import.

```
1. MarkItDown (if file-based)
   -> Convert all files to markdown
   -> Output: plain markdown content

2. inbox-processor
   -> Import raw files, process and categorize
   -> Output: organized notes with initial structure

3. add-frontmatter
   -> Standardize format across all imported files
   -> Output: uniform YAML frontmatter everywhere

4. pragmatic-review
   -> Verify integrity, check for errors
   -> Output: error-checked, consistent content

5. skill-creator
   -> Extract recurring patterns as skills
   -> Output: new skills from proven workflows
```

### Recipe 3: Maintain Existing Vault

Use when: user wants to keep their knowledge base fresh and organized.

```
1. daily-review
   -> Process new items captured during the day
   -> Output: triaged items (act/delegates/defer/drop)

2. inbox-processor
   -> Organize triaged items into PARA
   -> Output: placed in correct PARA folders

3. weekly-synthesis
   -> Summarize week's work and learning
   -> Output: summary with cross-references

4. de-ai-ify
   -> Refresh stale or repetitive content
   -> Output: natural, readable vault
```

---

## Section 4: Skill Output Standards

When creating new skills for the vault, follow these standards:

### SKILL.md Template

```markdown
---
name: <skill-name>
description: "Use when the user [trigger]. [What it does.]"
version: "1.0"
author: hermes-kb
license: MIT
metadata:
  source: https://github.com/hata83ai-tech/hermes-kb
---

# <Skill Title>

## Purpose
[One paragraph: what this skill does and why you'd use it]

## When to Use
- [Scenario 1]
- [Scenario 2]
- [Scenario 3]

## When NOT to Use
- [Scenario 1]
- [Scenario 2]

## Workflow
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Example
```
[Example input and expected output]
```
```

### YAML Frontmatter Requirements

| Field | Required | Notes |
|-------|----------|-------|
| name | YES | lowercase-with-hyphens, matches folder name |
| description | YES | Starts with "Use when the user" |
| version | YES | "1.0" for new skills |
| author | YES | "hermes-kb" |
| license | YES | "MIT" |
| metadata.source | YES | URL to source or "created new" |

### Validation Checklist

Before committing a new skill:

- [ ] `name:` matches folder name (lowercase-with-hyphens)
- [ ] `description:` starts with "Use when the user"
- [ ] YAML parses without error:
  ```bash
  python3 -c "import yaml; yaml.safe_load(open('SKILL.md').read().split('---')[1])"
  ```
- [ ] `version:` is set to "1.0"
- [ ] `metadata.source:` is present
- [ ] Content has `##` headers (markdown structure)
- [ ] Examples are realistic and testable

---

## Section 5: Content Ingestion Guide

### MarkItDown: Unified Content Converter

MarkItDown is the **primary ingestion tool** for all file types. It converts PDFs, documents, audio, and YouTube URLs to markdown.

**Installation:**
```bash
pip install markitdown
which markitdown  # verify
```

**Supported Formats:**

| Format | Extension | Output |
|--------|-----------|--------|
| PDF | .pdf | Text markdown |
| Word | .docx, .doc | Formatted markdown |
| Excel | .xlsx, .xls | Tables as markdown |
| PowerPoint | .pptx, .ppt | Slides as markdown |
| Audio | .mp3, .wav, .ogg, .m4a, .flac | **Transcription** (Whisper) |
| YouTube | URL | **Transcript/subtitles** |
| Markdown | .md | Pass through |
| Text | .txt | Pass through |
| HTML | .html, .htm | Converted to markdown |
| EPUB | .epub | Book content |

### Ingestion Workflow

```
User provides: [any file type or URL]
        |
        v
+---------------------------------------------------------------+
| STEP 1: CHECK if markitdown is available                      |
| `which markitdown`                                             |
|        |                                                      |
|        +-- NO -> `pip install markitdown`                     |
|        |        if fails -> STOP "Cannot install MarkItDown"  |
|        |                                                      |
|        +-- YES -> Continue                                    |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| STEP 2: CONVERT with markitdown                               |
|                                                                |
| PDF/DOCX/XLSX/PPTX/EPUB:                                      |
|   markitdown input.file -o output.md                          |
|                                                                |
| Audio (mp3, wav, ogg, m4a, flac):                             |
|   markitdown input.mp3 -o output.md                           |
|   -> Uses Whisper for transcription                           |
|                                                                |
| YouTube URL:                                                  |
|   markitdown "https://youtube.com/watch?v=..." -o output.md   |
|   -> Fetches transcript/subtitles                              |
|                                                                |
| HTML:                                                         |
|   markitdown input.html -o output.md                         |
|                                                                |
| Markdown/Text:                                                |
|   cp input.md output.md  (pass through)                       |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| STEP 3: HANDLE CONVERSION FAILURE                             |
|        |                                                      |
|        +-- MarkItDown fails on file                           |
|        |   -> Suggest: (1) convert manually, (2) different     |
|        |      format, (3) use web_extract if URL               |
|        |                                                      |
|        +-- Unsupported format                                 |
|        |   -> STOP: "Cannot process [format]. Convert to       |
|        |      supported format or provide different input."   |
|        |                                                      |
|        +-- Partial conversion                                 |
|        |   -> Proceed with available content, note what's     |
|        |      missing in output                               |
+---------------------------------------------------------------+
        |
        v
OUTPUT: markdown content -> feed to research-assistant or inbox-processor
```

### Batch Ingestion

When user provides a folder of files:

```bash
# Convert all supported files in folder
for f in /path/to/folder/*; do
  case "$f" in
    *.pdf|*.docx|*.xlsx|*.pptx|*.epub)
      markitdown "$f" -o "/path/to/output/$(basename "$f").md"
      ;;
    *.mp3|*.wav|*.ogg|*.m4a|*.flac)
      markitdown "$f" -o "/path/to/output/$(basename "$f").md"
      ;;
    *.md|*.txt) cp "$f" "/path/to/output/" ;;
  esac
done
```

---

## Section 6: Knowledge System Guide

The hermes-kb vault supports two complementary knowledge systems:

### PARA (User-driven Organization)

- **Use when:** User wants manual control over structure
- **Good for:** Personal notes, project-specific information, human organization
- **Tools:** File management, search_files, inbox-processor
- **Trigger phrases:** "save to my notes", "organize project X", "where should I put this"

### llm-wiki (Agent-driven Knowledge)

- **Use when:** Agent needs to recall information across sessions
- **Good for:** Learned facts, accumulated knowledge, cross-referenced memory
- **Tools:** `llm-wiki` (built-in Hermes tool)
- **Trigger phrases:** "what do I know about X", "remember this for later", "agent knowledge"

### Decision Flow

```
Content arrives
        |
        v
+---------------------------------------------------------------+
| Q1: Is this personal/organizational?                          |
|     "save to my notes" / "organize project"                   |
|        |                                                      |
|        +-- YES -> PARA (user owns organization)               |
|        |                                                      |
|        +-- NO -> Continue                                     |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| Q2: What is the primary use case?                             |
+---------------------------------------------------------------+
| "find my notes"          -> PARA + search_files              |
| "what do I know"          -> llm-wiki query                   |
| "build KB for agent"      -> llm-wiki (agent memory)          |
| "organize project Y"      -> PARA (project-specific)          |
| "learn from Z"            -> llm-wiki (accumulated knowledge) |
| "personal journal"        -> PARA (user-driven)               |
+---------------------------------------------------------------+
```

### Combined Workflow

```
Content arrives
        |
        v
+---------------------------------------------------------------+
| 1. MarkItDown (if file-based)                                 |
|    -> markdown content                                         |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| 2. research-assistant (if research needed)                     |
|    -> structured analysis                                       |
+---------------------------------------------------------------+
        |
        v
+---------------------------------------------------------------+
| 3. DECISION: PARA or llm-wiki?                                |
|        |                                                       |
|        +-- User wants personal notes                          |
|        |   -> inbox-processor -> PARA folders                  |
|        |                                                       |
|        +-- User wants agent knowledge                          |
|        |   -> llm-wiki store                                  |
|        |                                                       |
|        +-- Both (user wants personal AND agent memory)         |
|            -> Both: inbox-processor -> PARA AND llm-wiki      |
+---------------------------------------------------------------+
```

### Migration Between Systems

**PARA -> llm-wiki:**
1. Read notes from PARA folders
2. Ask: "what are the key facts in these notes?"
3. Store facts in llm-wiki

**llm-wiki -> PARA:**
1. Query llm-wiki for relevant knowledge
2. Organize findings into PARA structure
3. User approves final placement

---

## Quick Reference

| Task | Skill(s) to Use |
|------|----------------|
| Build KB from scratch | research-assistant -> inbox-processor -> add-frontmatter -> skill-creator -> de-ai-ify -> pragmatic-review |
| Convert file to markdown | markitdown |
| Convert YouTube/audio | markitdown -> research-assistant |
| Port existing files | markitdown -> inbox-processor -> add-frontmatter -> pragmatic-review |
| Maintain vault | daily-review -> inbox-processor -> weekly-synthesis -> de-ai-ify |
| Create new skill | skill-creator |
| Debug an issue | systematic-debugging |
| Review code | pragmatic-review |
| Brainstorm ideas | thinking-partner |
| Manage PRs | pull-request -> release |

---

*kb-builder v1.0 -- hermes-kb meta-skill catalog and usage guide*