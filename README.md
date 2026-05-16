# hermes-kb

**Hermes Agent Skills Vault** — Forked from [claudesidian](https://github.com/heyitsnoah/claudesidian), adapted for Hermes Agent.

## Overview

hermes-kb provides reusable skills for Hermes Agent, organized using the PARA method, enhanced with built-in llm-wiki and MarkItDown for knowledge management.

## Features

| Feature | Description |
|---------|-------------|
| **PARA Folders** | Organize notes by Projects, Areas, Resources, Archive |
| **12 Skills** | Thinking partner, debugging, code review, research, etc. |
| **llm-wiki** | Built-in knowledge building (agent-driven) |
| **MarkItDown** | Convert PDF, YouTube, DOCX, PPTX, XLSX to markdown |

## Skills (12 Total)

| Skill | Purpose |
|-------|---------|
| `thinking-partner` | Collaborative exploration through questioning |
| `systematic-debugging` | Structured bug diagnosis |
| `pragmatic-review` | Practical code review |
| `skill-creator` | Create new Hermes skills |
| `research-assistant` | Deep research workflow |
| `daily-review` | End-of-day reflection |
| `weekly-synthesis` | Weekly reflection |
| `inbox-processor` | Process inbox items (GTD) |
| `add-frontmatter` | Add YAML metadata to notes |
| `de-ai-ify` | Remove AI patterns |
| `pull-request` | PR workflow |
| `release` | Release management |

## Installation

```bash
# Clone the vault
git clone https://github.com/hata83ai-tech/hermes-kb.git ~/hermes-kb

# Copy skills to Hermes
cp -r ~/hermes-kb/.agents/skills/* ~/.hermes/skills/

# Verify skills are installed
hermes skills list | grep hermes-kb
```

## Usage

```bash
# Load a skill in Hermes
/skill thinking-partner
/skill research-assistant
/skill systematic-debugging

# Or use built-in llm-wiki
llm-wiki query "what do I know about X"
```

## PARA Method

The vault follows the PARA method for organizing information:

- **Projects** (01_Projects/) — Time-bound initiatives with goals
- **Areas** (02_Areas/) — Ongoing responsibilities
- **Resources** (03_Resources/) — Reference materials and topics
- **Archive** (04_Archive/) — Completed/inactive items

Quick capture goes to **00_Inbox/** for processing.

## Workflow: Research → Store

```
User Input (link/pdf/YouTube)
        ↓
┌─────────────────────────────┐
│   research-assistant        │
│   - Deep analysis            │
│   - Structured output        │
└─────────────────────────────┘
        ↓
┌─────────────────────────────┐
│      llm-wiki               │
│   - Store analyzed info      │
└─────────────────────────────┘
        ↓
┌─────────────────────────────┐
│      PARA folders           │
│   - User manual organize    │
└─────────────────────────────┘
```

## License

MIT License. See [LICENSE](LICENSE) and [NOTICE](NOTICE) for details.

This project is derived from [claudesidian](https://github.com/heyitsnoah/claudesidian) by heyitsnoah.