# Hermes-KB — Design Document v2.0

## Design Principles

1. **Simple first** — không over-engineer
2. **Built-in preferred** — dùng native features trước
3. **Human-in-loop** — Sếp kiểm soát sensitive actions
4. **Compounding** — knowledge tích lũy theo thời gian

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     User (Sếp Hà Tạ)                    │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   Hermes Agent (Maika)                  │
│  • llm-wiki skill (knowledge compounding)              │
│  • research-assistant (ad-hoc research)                 │
│  • hermes-code-sync (source tracking)                   │
└─────────────────────────────────────────────────────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
┌─────────────────────────┐   ┌─────────────────────────┐
│   PARA (Workspace)      │   │   llm-wiki (Knowledge)  │
│   • 01-Projects/        │   │   • raw/ (sources)      │
│   • 02-Areas/          │   │   • entities/            │
│   • 03-Resources/     │   │   • concepts/            │
│   • 04-Archive/        │   │   • SCHEMA.md            │
└─────────────────────────┘   └─────────────────────────┘
              │                         │
              └────────────┬────────────┘
                           ▼
              ┌─────────────────────────┐
              │    Obsidian Vault       │
              │    (kb-maika/)          │
              └─────────────────────────┘
```

## Data Flow

### Ingest Flow

```
[User Input] → markitdown → [Markdown]
                              │
                              ▼
              ┌───────────────────────────────┐
              │         llm-wiki              │
              │  1. raw/ → save immutable     │
              │  2. entities/ → extract        │
              │  3. concepts/ → extract        │
              │  4. Cross-ref existing        │
              │  5. Update index + log        │
              └───────────────────────────────┘
```

### Research Flow

```
[User Query] → research-assistant
                    │
                    ▼
           web_search + web_extract
                    │
                    ▼
           Structured output (Summary/BG/KeyPoints)
                    │
                    ▼
           [Deliver to User]
                    │
              Optional: ingest vào llm-wiki
```

## Component Design

### 1. Skills Layer

```
hermes-kb skills/
├── llm-wiki/                 # Built-in — knowledge compounding
├── research-assistant/        # Ported — ad-hoc research
├── hermes-code-sync/         # Custom — source tracking
├── source-knowledge-extraction/  # Custom — KB extraction
└── markitdown/               # External tool — conversion
```

### 2. Storage Layer

```
hermes-kb vault/
├── .agents/skills/           # Skills definitions
├── PARA/                     # Workspace (Obsidian folders)
├── kb-maika/                 # Maika's KB pages
│   ├── 1-Architecture/       # Hermes source knowledge
│   ├── 2-Concepts/          # AI/ML concepts
│   ├── 3-Comparisons/       # Tool comparisons
│   └── log.md              # Activity log
└── README.md                # Entry point
```

### 3. Workflow Patterns

**Pattern A: Ingest**
```
trigger: Sếp gửi PDF/YouTube/link
action:
  1. markitdown <input> -o /tmp/converted.md
  2. llm-wiki ingest /tmp/converted.md
  3. parse entities + concepts
  4. create/update wiki pages
  5. update index + log
  6. report to Sếp
```

**Pattern B: Research**
```
trigger: Sếp ask "research về X"
action:
  1. web_search X
  2. web_extract top results
  3. synthesize
  4. deliver structured output
  5. optionally ingest to wiki
```

**Pattern C: Code Sync**
```
trigger: git pull → post-merge hook
action:
  1. touch ~/.hermes/.kb_sync_needed
  2. cron (15min) detect marker
  3. scan changed files
  4. update KB pages
  5. report changes
```

## Technology Stack

| Component | Technology |
|-----------|------------|
| Agent | Hermes (minimax-m2.7) |
| Knowledge Layer | llm-wiki (Hermes built-in) |
| Research | research-assistant skill |
| Workspace | Obsidian vault (PARA) |
| Ingest Tool | MarkItDown (Microsoft) |
| Code Tracking | git hooks + cron |

## Configuration

```bash
# Environment
WIKI_PATH=~/workspace/hermes-kb/kb-maika  # llm-wiki location
OBSIDIAN_VAULT_PATH=~/workspace/hermes-kb  # Obsidian vault

# MarkItDown install
pip install 'markitdown[all]'

# Cron jobs
hermes-kb-sync (15min) — source sync
```

## Security Considerations

1. **Credentials** — never store in code, use env vars
2. **Raw sources** — immutable, never modify after ingest
3. **User control** — sensitive actions require Sếp approval

## Maintenance

- **Weekly:** Lint KB (check orphans, broken links)
- **Monthly:** Review stale content
- **On-demand:** Re-ingest updated sources (sha256 detect drift)