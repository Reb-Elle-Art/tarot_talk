# Court Card Tagging Instructions — Phase 2

You are adding court-specific tags to tarot court cards to support retrieval in multi-meaning scenarios (e.g., "what does this card mean for my relationship?").

## Input
Each court card has a .txt file with:
- YAML frontmatter (card_id, card_name, card_type, suit, court_position)
- Named sections including "Upright" which may contain sub-sections like "As a development" and "As an adult woman"

## Output Format
For each card, output a YAML block:

```yaml
card_id: <from frontmatter>
court_archetype:
  position: <page|knight|queen|king>
  energy: <one-line description of the position's core energy>
  role: <what this court card typically represents as a person>
suit_domain:
  element: <fire|water|air|earth>
  theme: <one-line description of the suit's core domain>
  zodiac_tendancy: <typical zodiac associations for this suit>
persona_tags:
  - <5-8 descriptive phrases about what this card-as-person is like>
  - <include how they approach relationships, career, emotions, etc.>
context_signals:
  development: <1-2 sentences about what "as development" means for this card>
  adult_interpretation: <1-2 sentences about what "as adult woman/man" means for this card (use gender from card)>
  shadow: <1-2 sentences about the negative/reversed interpretation of the person-aspect>
```

## Guidelines

**court_archetype:**
- Page = messages, curiosity, learning, child-like energy
- Knight = movement, travel, active pursuit, youthful ambition
- Queen = nurturing, mastery from within, established feminine energy
- King = management, authority, mastery over the suit domain

**suit_domain:**
- Wands = Fire = passion, creativity, ambition, action
- Cups = Water = emotion, intuition, relationships, love
- Swords = Air = intellect, conflict, truth, clarity
- Pentacles = Earth = material world, work, finances, home

**persona_tags:**
Think about this card as a person:
- How do they show up in relationships?
- What do they care about?
- What's their strength? Their blind spot?
- Are they more action-oriented or reflective?

**context_signals:**
The "As a development" and "As an adult [woman/man]" sections in the card text are key. Extract the difference between these two interpretations.

## Your task
For each court card file:
1. Read the .txt file
2. Extract card_id, suit, court_position from frontmatter
3. Generate all court_archetype, suit_domain, persona_tags, and context_signals
4. Output a YAML block

Format: one YAML block per card, separated by blank lines. No extra commentary. Write to /home/enid/.openclaw/workspace/projects/tarot_talk/tags/court_tags.yaml
