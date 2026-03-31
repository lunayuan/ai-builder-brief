# Briefing Structure

Generate the briefing as a single HTML email with inline styles. No external CSS, no CDN links.

## Layout Rules

- **Max width:** 680px, centered
- **Font:** Arial, Helvetica, sans-serif. Size 15px. Line-height 1.6.
- **Colors:** Dark text (#1a1a1a) on white. Category headers use light gray background (#f0f0f0).
- **Subject line:** `AI Builder Brief for {config.identity.name} | Edition #{count} | {date}`

## Coverage Period

- **Daily cadence:** Do NOT include a coverage period. The date in the header is sufficient.
- **Weekly or other cadence:** Include a coverage period in the header below the edition line:
  `Covering: {start date} – {end date}` (e.g., "Covering: March 24 – March 30, 2026")

## Section Order

1. **Title bar** — "AI Builder Brief for {config.identity.name}", tagline, edition number, date, coverage period (weekly only)
2. **Executive summary** — 3-5 bullets (follow `executive-summary.md`)
3. **Category sections** — one per enabled category in config order (follow `synthesize-category.md`)
4. **What to Watch** — 2-4 forward-looking items
5. **Footer**

## Category Headers

Each category header gets an emoji icon before the name. Pick an icon that matches the category topic:

- Tools/engineering: &#9881;&#65039;
- Products/apps: &#128218;
- Social good/climate: &#127793;
- Content/social: &#127912;
- Growth/distribution: &#128200;
- Trends/funding: &#128640;
- Agents/automation: &#129302;
- Competitors: &#128065;

Format: `<span>&#128200; AI Growth & Distribution</span>` inside the gray header bar.

## Item Format (within each category)

Each item is a `<div>` block (not a list item) with three parts:

```html
<div style="margin-bottom:18px">
  <strong>[Headline]</strong> -- [1-2 sentence summary].
  <a href="[url]" style="color:#9ca3af;text-decoration:none;font-size:13px">[Source name]</a><br>
  <em style="color:[category-color];font-size:13px">Your implication: [observation].</em>
</div>
```

Key styling rules:
- **Source link:** Grayed out (`color:#9ca3af`), inline at the end of the summary text, smaller font (13px). NOT the category accent color. This keeps source attribution visible but unobtrusive.
- **Implication line:** Uses the category accent color, italicized, on its own line below the summary.
- **Headline:** Bold, default text color (#1a1a1a).

If an item is low confidence, prefix headline with: `[Low confidence]`

## Footer

Styled in muted gray, centered, small text:

```html
<div style="padding:20px 24px;background:#f8fafc;border-top:1px solid #e2e8f0;text-align:center">
  <div style="font-size:12px;color:#b0b8c4;line-height:1.8">
    Created with the <a href="https://github.com/lunayuan/ai-builder-brief" style="color:#b0b8c4;text-decoration:underline">AI Builder Brief</a> skill
    &bull; github.com/lunayuan/ai-builder-brief
  </div>
</div>
```

Footer should be minimal — just the attribution line. No "Reply with feedback", no edition info (that's already in the header). The GitHub URL should be visible as plain text so readers can see where it comes from.

## Responsive

The HTML must be readable on mobile email clients. Use inline styles only. No images. No JavaScript.

## What NOT to include

- No greeting ("Dear team" / "Hi all")
- No sign-off ("Best regards")
- No preamble ("This week in AI...")
- Jump straight into the executive summary
