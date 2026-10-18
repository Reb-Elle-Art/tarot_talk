---
type: project
owner: agent
source: self-built
access: shared
updated: 2026-10-18
status: active
tags: [tarot, rag, telegram-bot, local-llm]
covers: [retrieval, embedding, telegram]
---

# Tarot Talk Bot — Project Plan

## Overview

**Name:** Tarot Talk Bot
**Repo:** https://github.com/Reb-Elle-Art/tarot_talk
**Purpose:** A dedicated tarot RAG bot for Telegram. Users ask questions about tarot cards in plain language — the bot retrieves relevant passages from a curated 78-card corpus and synthesizes answers using local models.

**Interface:** Telegram bot (lean, minimal)
**Models (all local, OpenAI-compatible endpoints):**
- Embedding: Owen-3.8
- Reranker: Quen-3.8
- Generation: Gemma-4-26b-a4b

---

## Architecture

```
User message (Telegram)
    ↓
Intent detection (keyword matching)
    ↓
Template selection
    ↓
Embedding (Owen-3.8) → Chroma search
    ↓
Reranking (Quen-3.8) → top candidates
    ↓
Generation (Gemma-4-26b-a4b) → response
    ↓
Telegram reply
```

---

## Current Status

**Phase 1 (Chunking + Tagging):** COMPLETE.

All 78 card tag files are in `tags/tarot_tags.yaml`. Tags include:
- `card_id`
- `themes` — abstract concepts (5-10 per card)
- `keywords` — concrete search-friendly terms (single concatenated string per card)
- `symbols` — physical symbols from card imagery

**Astrological correspondences:** Compiled from 7 sources (Golden Dawn tradition), stored in `tags/astrological-correspondences.json` in the tarot-scraper repo (https://github.com/Reb-Elle-Art/tarot-scraper). To be integrated into tag files.

Awaiting Elle's review/edit pass before Phase 2.

---

## Intent Templates

### 1. thematic_search
**Triggers:** "what cards touch on", "cards about", "cards dealing with", "completion", "endings", etc.
**Behavior:** Semantic search across all card sections, returns top 5 cards with relevance explanation.
**Default position:** Upright (unless reversed is specified)
**Sections prioritized:** Upright, Deeper Meaning

### 2. card_lookup
**Triggers:** "tell me about [card]", "what does [card] mean", "[card] tarot"
**Behavior:** Direct card retrieval, returns full card content.
**Position handling:** Default upright; "reversed" or "inverted" triggers reversed section.

### 3. random_draw
**Triggers:** "draw a card", "give me a card", "random card"
**Behavior:** Random selection from 78 cards, returns full card content.

### 4. spread (simplified v1)
**Triggers:** "give me a reading", "do a reading", "6 card spread"
**Behavior:** Draws 6 cards, assigned to:
- Past (2 cards)
- Present (2 cards)
- Future (2 cards)
Returns each card with position context.

**Celtic Cross / complex spreads:** Deferred to later version.

---

## Data Structure

### Repo Location
```
https://github.com/Reb-Elle-Art/tarot_talk
Local: /home/enid/.openclaw/workspace/projects/tarot_talk/
```

### Source Files
Canonical source: `cards/major/`, `cards/minor/`, `cards/court/` (78 cards total)

### Tag Files
```
tags/tarot_tags.yaml       # All 78 cards — consolidated tags (themes, keywords, symbols)
tags/tags/                 # Per-card JSON tag files (to be generated in Phase 2)
tags/astrological-correspondences.json  # In tarot-scraper repo — link to be added
```

### Card File Format (.txt with YAML frontmatter)
```yaml
---
card_id: "death"
card_name: "Death"
card_type: "major"
card_number: "13"
source_url: "https://www.aeclectic.net/tarot/tarot-card-meanings/major-arcana/death.shtml"
date_scraped: "2026-10-17"
---

**Symbols:**
[raw text]

**Upright Meaning:**
[raw text]
...
```

### Tag File Format (YAML)
```yaml
card_id: death
themes:
  - endings
  - transformation
  - rebirth
keywords:
  - death ending transition rebirth transformation Scorpio change loss grief mortality skeleton scythe rose sunrise
symbols:
  - skeletal death
  - black robes
  - scythe
  - white rose
```

---

## Chunking Strategy

**Decision:** One chunk per named section per card.

Rationale: Fine-grained retrieval. If user asks about "completion," you get the specific section that addresses it rather than the whole card. Tags apply at chunk level.

Example — Death card becomes multiple chunks:
- Chunk 1: Symbols section + metadata
- Chunk 2: Story section + metadata
- Chunk 3: Upright Meaning + metadata
- Chunk 4: Deeper Meaning + metadata
- Chunk 5: Reversed section + metadata
- Chunk 6: in Love section + metadata

---

## Phases

### Phase 1 — Chunking + Tagging
- [x] Scrape all 78 cards from aeclectic.net
- [x] Generate themes/keywords/symbols for all 78 cards
- [x] Review/edit pass (Elle reviews output — keyword quality, brief_summary coherence)
- [ ] Integrate astrological correspondences into tag files (from tarot-scraper repo)
- [x] Finalize tag files in tags/ directory

### Phase 2 — Index Build
- [ ] Parse all 78 cards, chunk by section
- [ ] Attach metadata + generated tags
- [ ] Embed with Owen-3.8
- [ ] Store in Chroma (file-based)
- [ ] Index reranker (Quen-3.8)

### Phase 3 — Bot Build
- [ ] Intent template system (4 templates)
- [ ] Retrieval pipeline (embed → search → rerank → generate)
- [ ] Telegram bot interface
- [ ] System prompt: domain rules only (no tarot reader persona), default upright

### Phase 4 — Test + Tune
- [ ] Run queries, fix retrieval issues
- [ ] Tune reranking thresholds
- [ ] User testing

---

## Project Structure

```
tarot_talk/               # Repo root
├── cards/
│   ├── major/           # 22 .txt files
│   ├── minor/           # 40 .txt files
│   └── court/           # 16 .txt files
├── tags/
│   ├── tarot_tags.yaml        # All 78 card tags (YAML, current)
│   ├── TAGGING_INSTRUCTIONS.md
│   ├── COURT_TAGGING_INSTRUCTIONS.md
│   ├── astrological-correspondences.json  # (in tarot-scraper repo)
│   └── [to be organized in Phase 2]
├── src/
│   ├── prompts/         # Prompt fragments
│   └── templates/        # Intent templates
├── cards/                # Card source files (from aeclectic.net)
├── requirements.txt
└── README.md
```

**Out of scope for repo:** chroma_db/, src/ (bot code) — to be added in Phase 2-3.

---

## Out of Scope (v1)
- Complex spreads (Celtic Cross, etc.)
- Tarot reader persona
- Multi-card comparison queries
- Reading history / persistence
- Image generation for cards

---

## Waiting On
- Elle's review/edit pass on Phase 1 tag output
- Integration of astrological correspondences into tag files

---

_Last updated: 2026-10-18_
