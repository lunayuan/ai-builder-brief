# Search Query Construction

For each enabled category in `config.categories`, construct exactly 2 WebSearch queries.

## Query Template

```
[AI focus terms] [category searchHints] [date qualifier]
```

## Rules

- **Max 15 words per query.** WebSearch works better with focused queries.
- **Date qualifier:** Use current month and year (e.g., "March 2026"). For daily briefings, use "this week" or specific date range.
- **No quotes inside queries.** WebSearch handles them differently from Google.
- **For Watch List items:** Inject names from `config.watchList.people[]` and `config.watchList.orgs[]` into relevant category queries.
- **For Competitors category (team mode):** Inject competitor names from `config.competitors[]` into the query.
- **Geography:** If `config.industry.geography` is not "global", add geographic qualifier (e.g., "North America", "Europe").

## Curated Sources (priority)

Before general searches, check `config.sources.curated[]`:
- For each curated source with `requiresAuth: false`: use WebFetch to fetch the page URL and extract recent headlines.
- For each curated source with `requiresAuth: true`: use gstack `/browse` with imported cookies. If cookies not set up, skip and note it.
- Curated source items get priority placement in the briefing.

## General Search Strategy

For each category, combine the first 2-3 items from `config.industry.focus` with the category's `searchHints`:

| Config | Example |
|---|---|
| `industry.focus`: ["AI agents", "AI developer tools"] | |
| Category: "Model Releases" with hints: ["LLM", "foundation model", "benchmark"] | |
| **Query 1:** | `AI agent LLM foundation model release March 2026` |
| **Query 2:** | `AI developer tools new model benchmark update March 2026` |

## Rate Limiting

- Execute categories sequentially, not in parallel
- If you hit rate limits, wait 5 seconds between categories
- If a search fails, skip that query and note in the briefing footer

## Source Domain Scoping

If `config.sources.preferredDomains` is populated, consider adding `site:` qualifiers to at least one query per category for higher-quality results. Example:
```
AI startup funding launch announcement site:techcrunch.com OR site:theverge.com March 2026
```

Only use domain scoping on one of the two queries per category — keep the other broad to catch items from unexpected sources.
