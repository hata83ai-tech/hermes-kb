# Hermes-KB — Task List v2.0

> Generated: 2026-05-16 | Status: ACTIVE

## P0 — Must Do (Immediate)

### P0.1: Install MarkItDown

**Status:** Pending

**Task:**
```bash
pip install 'markitdown[all]'
```

**Verification:**
```bash
markitdown --version
markitdown --help
```

**Owner:** Maika

**Dependencies:** None

---

### P0.2: Test Ingest — Single PDF

**Status:** Pending

**Task:**
1. Sếp gửi 1 PDF thử
2. Maika convert: `markitdown <pdf> -o /tmp/test.md`
3. Maika ingest vào llm-wiki
4. Verify: check raw/ + entities/ + index.md updated

**Owner:** Maika

**Dependencies:** P0.1 (MarkItDown installed)

---

### P0.3: Test Ingest — YouTube Link

**Status:** Pending

**Task:**
1. Sếp gửi 1 YouTube link
2. Maika: `markitdown "<youtube-url>" -o /tmp/yt.md`
3. Maika ingest vào llm-wiki
4. Verify: check transcript extracted

**Owner:** Maika

**Dependencies:** P0.1 (MarkItDown installed)

---

### P0.4: Verify llm-wiki Config

**Status:** Pending

**Task:**
```bash
echo $WIKI_PATH
# Nếu empty → set in ~/.hermes/.env
# WIKI_PATH=~/workspace/hermes-kb/kb-maika
```

**Owner:** Sếp (cần set env var)

**Dependencies:** None

---

## P1 — Should Do (This Week)

### P1.1: Document Skills in KB

**Status:** Pending

**Task:**
- Tạo page `kb-maika/2-Concepts/hermes-skills.md`
- List all skills: llm-wiki, research-assistant, hermes-code-sync, etc.
- Include trigger conditions và usage patterns

**Owner:** Maika

**Dependencies:** P0.2 (basic ingest working)

---

### P1.2: Test research-assistant

**Status:** Pending

**Task:**
1. Sếp ask: "Research về AI coding agents"
2. Maika activate research-assistant
3. Deliver structured output
4. Optionally ingest vào kb-maika

**Owner:** Maika

**Dependencies:** P0.4 (llm-wiki config verified)

---

### P1.3: Create HERMES-KB README

**Status:** Pending

**Task:**
- Entry point: `hermes-kb/README.md`
- Mục đích, cấu trúc, quick start
- Hướng dẫn gửi input (PDF/YouTube/link)

**Owner:** Maika

**Dependencies:** P1.1 (skills documented)

---

## P2 — Nice to Have (Later)

### P2.1: Knowledge Freshness Layer

**Status:** DEFERRED

**Task:**
- Add confidence decay tracking
- Supersession mechanism
- When: KB > 500 pages

**Owner:** TBD

**Dependencies:** KB size > 500 pages

---

### P2.2: Periodic Review Cron

**Status:** DEFERRED

**Task:**
- Weekly lint: orphan pages, broken links
- Monthly: stale content review
- When: KB > 500 pages

**Owner:** TBD

**Dependencies:** KB size > 500 pages

---

## DONE — Completed

### ✅ setup-vault-structure

**Completed:** 2026-05-16

**Summary:** Created hermes-kb vault với PARA structure + kb-maika subdirectory

---

### ✅ source-knowledge-extraction

**Completed:** 2026-05-16

**Summary:** Extracted 15+ KB pages từ Hermes source code (run_agent.py, gateway/run.py, etc.)

---

### ✅ hermes-code-sync

**Completed:** 2026-05-16

**Summary:** Created skill + git hook + cron job cho KB sync on source update

---

## Notes

- **Priority Order:** P0 → P1 → P2
- **Blocking:** P0.4 (WIKI_PATH) blocks P0.2, P0.3 testing
- **Sếp action needed:** Set WIKI_PATH in env

## Changelog

| Date | Change |
|------|--------|
| 2026-05-16 | v2.0 — Added voting results, updated priorities |
| 2026-05-16 | v1.0 — Initial task list |