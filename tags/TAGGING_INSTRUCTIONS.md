# Tarot Card Tagging Instructions

You are generating keywords, themes, and symbols tags for tarot cards to support semantic search retrieval.

## Input
Each card has a .txt file with YAML frontmatter and prose sections (Upright, Reversed, Symbols, Story, Deeper Meaning, in Love, etc.)

## Output Format
For each card, output a YAML block with these fields:

```yaml
card_id: <from frontmatter>
themes:
  - <array of 5-10 theme strings, derived from the card's meaning and usage>
keywords:
  - <array of 8-15 keyword strings, concrete and search-friendly>
symbols:
  - <array of 3-7 symbols from the Symbols section if present>
```

## Guidelines

**themes** = abstract concepts the card embodies. Think: what emotional or life areas does this card speak to?
- Examples: "endings", "transformation", "new beginnings", "inner wisdom", "conflict", "abundance", "creativity"

**keywords** = concrete, search-friendly terms someone might use when looking for this card.
- Include the card name, alternate names, associated concepts, and common questions about the card.
- Examples: "death ending transition rebirth", "the fool new journey冒险 beginnings"

**symbols** = only if the card has a Symbols section. List the symbols mentioned there.
- Examples: "white horse", "skeleton", "scythe", "rose"

## Rules
- themes: 5-10 items
- keywords: 8-15 items
- symbols: 0-7 items (omit if no Symbols section)
- Use lowercase for searchability
- Be specific to this card, not generic tarot themes
- If a card appears in multiple .txt files (e.g. in both major/ and flat/), use the one in major/minor/court directories as canonical

## Your task
For each card file in your batch:
1. Read the .txt file
2. Extract card_id from frontmatter
3. Generate themes, keywords, and symbols
4. Output a YAML block

Format your final output as one YAML block per card, separated by a blank line. No extra commentary, just the YAML blocks.
