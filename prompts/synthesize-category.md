# Synthesize Category

For each category with search results, write a section following these rules.

## Per-Item Rules

- **1-2 sentences** per item. Lead with the fact, not the source.
- **Source URL required** on every item. Use the actual URL from search results. NEVER fabricate a URL.
- **Max 5 items per category.** Pick the most significant. Quality over quantity.
- **Group related items.** Two articles about the same event = one combined item, not two.
- **Skip items with no substance.** Listicles, SEO content, opinion pieces with no new information — skip them.
- **Date context.** Include when something happened if it's time-sensitive (e.g., "announced March 12").

## Source Confidence

- **High confidence:** Reuters, Bloomberg, TechCrunch, The Verge, Ars Technica, company press releases, research lab blogs (OpenAI, Anthropic, DeepMind, Meta AI), arXiv papers, government/regulatory body sites
- **Low confidence:** Personal blogs without original reporting, forums, non-tech publications, aggregator sites without original reporting
- If source is low confidence, prefix item with `[Low confidence]`
- If a source URL is in `config.sources.preferredDomains`, it's automatically high confidence
- If a source URL is in `config.sources.lowConfidenceDomains`, tag it

## Personal Implication

Every item gets a personal implication line. Follow `personal-implications.md` for rules.

## Empty Categories

If a category has no quality results after searching, **skip it entirely.** Do not write "No significant developments this week" — just omit the section. The executive summary can note: "Quiet week for [category]."

## Tone

Match `config.tone`:
- **internal-memo:** Direct, opinionated, uses "you" language. Like a smart friend briefing you.
- **executive-brief:** More formal, third-person. Like an analyst note.
- **analyst-note:** Formal, data-driven, includes specific numbers and percentages where available.
