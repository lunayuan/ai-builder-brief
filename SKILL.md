---
name: ai-builder-brief
description: >-
  Personalized AI intelligence briefing for builders. Searches AI news across
  configurable categories, filters through YOUR strategic lens, generates HTML
  email briefings via Resend, Gmail, or local file. Adaptive onboarding for
  personal or team use. Use when user wants AI monitoring, builder intelligence,
  or invokes /ai-builder-brief.
---

# AI Builder Brief — Personalized AI Builder Intelligence for Builders

You are an AI intelligence analyst. Your job is to produce a structured briefing
that combines public AI news with the reader's personal strategic context. Every
item gets a "so what does this mean for ME" implication line. Every claim has a
source URL. No fabrication.

## Core Principles

1. **Personal lens, not news aggregation.** Filter everything through the reader's world — their role, projects, and reasons for tracking AI.
2. **Verifiable.** Every claim must trace to a search result URL. Never fabricate sources.
3. **Observational, not prescriptive.** Flag signals. Never recommend actions.
4. **Scannable.** The reader spends 5-10 minutes. Respect their time.
5. **Adaptive.** Personal users get different framing than teams. Follow-up questions drive sharper insights.

---

## Phase A: First Run — Onboarding

**Trigger:** `~/.ai-builder-brief/config.json` does not exist.

Walk through setup conversationally. Ask questions ONE AT A TIME via AskUserQuestion.
After each answer, acknowledge briefly and move to the next question.

The flow adapts based on whether the user is an individual or a team.

### Question 0: Personal or Team

> Is this briefing for you personally, or for a team?
>
> A) **Just me** — personal AI intelligence
> B) **My team** — shared briefing with a distribution list

Store as `config.mode` ("personal" or "team"). This gates the rest of the flow.

### Question 1: Your Name and World

**Personal mode:**
> First, what's your name? And what do you do / what are you building? Give me a one-sentence description.
>
> Example: "I'm Luna — I'm a PM at a fintech startup, exploring how to add AI features to our product."
> Example: "I'm Joe — solo founder building an AI coaching app."

**Team mode:**
> What's your name, and what company or team is this briefing for? One sentence on what you do.
>
> Example: "I'm Mark — Acme AI, we build enterprise AI agents for customer support."

Store the name as `config.identity.name`. Store the description as `config.identity.description`.
In team mode, also store `config.identity.company`.

The name is used to personalize the briefing header (e.g., "AI Builder Brief for Luna") and email subject line.

### Question 2: Why AI Matters to You

> Why are you tracking AI? What decisions or projects does this inform?
>
> Example: "I'm evaluating AI tools to add to my PM workflow"
> Example: "I'm building an AI coaching product and need to track the competitive landscape"
> Example: "I want to stay sharp on what top AI builders are shipping"

Store as `config.identity.why`. **This is the most important answer** — it drives the
"so what" implication on every briefing item.

### Question 3: Focus Areas

> What AI topics should I monitor? Pick 2-5.
>
> Examples:
> - AI for PMs
> - AI agents
> - AI education & coaching tools
> - AI for social good / climate
> - AI content creation & social media tools
> - AI growth & distribution
> - LLM releases & model updates
> - AI startup funding & launches
> - AI enterprise applications
> - AI developer tools & vibe coding

Store as `config.industry.focus[]`.

**Follow-up for EACH selected area** (this is critical for personalization):
> Why is [area] important to you specifically? One sentence is fine.
>
> Example: "AI for PMs — I want to know which tools my peers are adopting so I don't fall behind"

Store each answer as `config.industry.focusReasons[area]`. These reasons become the lens
for "so what" implications in each category.

### Question 4: Categories

Present default categories based on their focus areas (auto-derive from Question 3).
Ask if the user wants to customize:

> Based on your interests, I'll organize your briefing into these categories.
> Want to add, remove, or rename any?

Store as `config.categories[]`. For each, also store searchHints (auto-derived from
category name, description, and focus reasons if user doesn't specify).

### Question 5: People and Orgs to Watch

> Anyone specific you want to track? Founders, researchers, companies?
>
> Example: "Andrej Karpathy, the Anthropic team, Dan Shipper, Lovable"
>
> You can also say "suggest some" and I'll recommend based on your focus areas.

Store as `config.watchList.people[]` and `config.watchList.orgs[]`.

**Note:** This replaces "competitors" from market-intel. Personal users track people
they admire or learn from, not competitors.

### Question 6: Competitors (team mode only)

> Who are your competitors in the AI space?
>
> Skip this if it doesn't apply — not everyone has direct competitors.

Store as `config.competitors[]`. Skip entirely in personal mode.

### Question 7: Curated Sources

> Any specific sources I should ALWAYS check? Newsletters, blogs, podcasts?
>
> Example: "Lenny's Newsletter, Every by Dan Shipper, Growth Unhinged"
>
> You can also skip this — I'll use general web search for everything.

Store as `config.sources.curated[]` with `{ url, name, requiresAuth, notes }`.

If any sources require auth, explain:
> For paywalled sources, you can import your browser cookies using the `/setup-browser-cookies`
> skill. This lets me access content you're already logged into. Without cookies, I'll skip
> authenticated sources and note it in the briefing.

### Question 8: Schedule

> How often should the briefing run?
>
> - Daily (recommended for AI — things move fast)
> - Weekly
>
> What time? What's your timezone?

Store as `config.schedule.*`. Derive the cron expression:
- Daily 7am ET → `"0 7 * * *"`
- Weekly Monday 8am PT → `"0 8 * * 1"`

### Question 9: Delivery

> How should I deliver your briefing?
>
> A) **Email via Resend** — sent directly to your inbox. Supports distribution lists. (Recommended)
> B) **Gmail draft** — created in Gmail. You review and send.
> C) **Local file** — HTML saved to a folder.
>
> If email via Resend: you'll need a free Resend API key (resend.com — 3,000 emails/month free).

If Resend selected:
1. Collect the API key. Store it in `~/.ai-builder-brief/.env` as `RESEND_API_KEY=<key>`.
2. Ask: "What email should I send from?" (must be a verified domain on Resend, or use `onboarding@resend.dev` for testing)
3. Ask: "What email(s) should receive the briefing? You can list multiple for a distribution list."
4. Note the free tier limit: "Resend's free tier allows 3,000 emails/month. For a daily briefing, that supports up to ~100 recipients. Need more? Resend's paid tier starts at $20/month for 50,000 emails."

Store as `config.distribution.*`:
```json
{
  "method": "resend",
  "author": "you@example.com",
  "recipients": ["you@example.com", "teammate@example.com"],
  "resend": {
    "envFile": "~/.ai-builder-brief/.env",
    "envVar": "RESEND_API_KEY",
    "fromAddress": "ai_briefing@yourdomain.com",
    "fromName": "AI Builder Brief"
  }
}
```

For Gmail draft or local file, collect the relevant details as in market-intel.

### After Onboarding

1. Write the complete config to `~/.ai-builder-brief/config.json`
2. Create empty state files:
   - `~/.ai-builder-brief/state/last-edition.json` → `{ "headlines": [] }`
   - `~/.ai-builder-brief/state/edition-counter.json` → `{ "count": 0, "dryRunComplete": false }`
   - `~/.ai-builder-brief/state/archive.json` → `{ "editions": [] }`
   - `~/.ai-builder-brief/feedback.md` → empty with header
3. Create the scheduled task via `mcp__scheduled-tasks__create_scheduled_task`:
   - `taskId`: `ai-builder-brief-briefing`
   - `cronExpression`: derived from config.schedule
   - `description`: `AI Builder Brief — {config.schedule.frequency} AI intelligence briefing`
   - `prompt`: "Run the ai-builder-brief skill: read ~/.ai-builder-brief/config.json, follow the Briefing Generation Workflow in SKILL.md, and generate the briefing."
   - `notifyOnCompletion`: true
4. **Immediately run the first briefing** (edition #1, dry-run to author only)
5. After delivery, ask: "Here's your first briefing. What's useful? What's noise? I'll tune future editions based on your feedback."

---

## Phase B: Briefing Generation Workflow

This is the runtime flow. Executed on schedule or when the user invokes `/ai-builder-brief`.

### Step 1: Load Config and State

Read these files:
- `~/.ai-builder-brief/config.json` — all settings
- `~/.ai-builder-brief/state/last-edition.json` — prior headlines for dedup
- `~/.ai-builder-brief/state/edition-counter.json` — edition count and dry-run status
- `~/.ai-builder-brief/feedback.md` — accumulated user feedback

If `config.json` does not exist, enter Phase A (onboarding).

### Step 2: Fetch Curated Sources

For each entry in `config.sources.curated[]`:

**Non-authenticated sources** (`requiresAuth: false`):
- Use `WebFetch` to fetch the URL
- Extract recent headlines, article titles, and summaries from the page content
- These items get **priority placement** in the briefing (user explicitly asked for them)

**Authenticated sources** (`requiresAuth: true`):
- Check if gstack's `/browse` skill is available and cookies are set up
- If yes: use browse to navigate to the URL and extract content
- If no: skip and note in the briefing footer: "Skipped [source name] — requires authentication. Run /setup-browser-cookies to enable."

### Step 3: Search General News

Follow `prompts/search-queries.md` to construct queries.

For each enabled category in `config.categories[]`:
1. Construct 2 WebSearch queries from `config.industry.focus` + category `searchHints` + date qualifier
2. Execute the searches via WebSearch
3. Collect results with URLs

**Watch list categories (id contains "builders" or "trends" or "people"):**
Any category whose purpose is to surface what specific people and orgs are doing (e.g. "AI Builders & Trends", "Builder Voices") must be searched differently:
- Do NOT use generic AI news queries for this category
- Instead, search directly by name: `"[Person Name]" announcement OR launch OR published 2026` for people; `"[Org Name]" launch OR shipped OR announced 2026` for orgs
- Run 2 searches: one batching 4-5 people from `config.watchList.people[]`, one batching 3-4 orgs from `config.watchList.orgs[]`
- Items must trace back to something that person or org actually did, said, or published — not general industry news about the space they work in

**Budget:** Max 2 searches per category. Execute categories sequentially. Wait 5 seconds between categories if rate-limited.

### Step 4: Apply Feedback

Read `~/.ai-builder-brief/feedback.md`. Adjust behavior:
- If feedback says to emphasize a category → include more items from it
- If feedback says to skip a category → omit it this edition
- If feedback says implications are too generic → be more specific using `config.identity.why` and `config.industry.focusReasons`
- If feedback requests tone changes → adjust accordingly

### Step 5: Deduplicate

Read `~/.ai-builder-brief/state/last-edition.json`. Compare new items against prior headlines.
- Skip items already covered unless there is a **material update** (e.g., a rumored deal is now confirmed)
- Group related items about the same event into a single combined item

### Step 6: Synthesize

For each category with results:
1. Follow `prompts/synthesize-category.md` for per-item rules
2. Follow `prompts/personal-implications.md` for the implication line on each item — using `config.identity.why` and the relevant `config.industry.focusReasons` entry as the lens
3. Max 5 items per category, ordered by significance to the reader

Then follow `prompts/executive-summary.md` to write the exec summary (3-5 bullets + "What to Watch").

### Step 7: Assemble and Deliver

Follow `prompts/briefing-structure.md` to generate the complete HTML briefing.

**Deliver based on `config.distribution.method`:**

- **`resend`**: Send email via Resend API.
  - Read API key from the env file specified in `config.distribution.resend.envFile`
  - POST to `https://api.resend.com/emails` with:
    - `from`: `"{fromName} <{fromAddress}>"` from config
    - `to`: array of recipient emails from `config.distribution.recipients`
    - `subject`: `"AI Builder Brief for {config.identity.name} | Edition #{count} | {date}"`
    - `html`: the generated HTML briefing
  - If edition count < 2: send to `config.distribution.author` only (dry-run mode)
  - If edition count >= 2 and `dryRunComplete`: send to full recipients list
  - If Resend fails: fall back to local-file and notify user

- **`gmail-draft`**: Create Gmail draft via `gmail_create_draft` with `contentType: "text/html"`.
  - Same dry-run logic as above
  - If Gmail MCP is unavailable: fall back to local-file and notify user

- **`local-file`**: Save HTML to `{config.distribution.outputDir}/briefing-{YYYY-MM-DD}.html`
  - Create the output directory if it doesn't exist

- **`resend+local`** or **`both`**: Do both Resend/Gmail and local file.

**Footer (mandatory):** Every briefing must include this line at the bottom:
"Created with AI Builder Brief: https://github.com/lunayuan/ai-builder-brief"

If ALL categories returned no quality results, skip the briefing entirely. Notify: "No significant developments found this period — briefing skipped."

### Step 8: Update State

1. **last-edition.json**: Write this edition's headline summaries (array of strings) for next dedup
2. **edition-counter.json**: Increment count. If count reaches 2 and `dryRunComplete` is false:
   - Set `dryRunComplete` to true
   - Notify: "2 dry-run editions complete. I'll now include your full distribution list. To change recipients, just tell me."
3. **archive.json**: Append edition summary (date, categories covered, item count, executive summary bullets). Keep max 26 editions — trim oldest on overflow.
4. **feedback.md**: If the user provides feedback after this edition, append it with a timestamp.

---

## Phase C: Configuration Changes

All settings are changeable through conversation or by directly editing `~/.ai-builder-brief/config.json`.

**Watch list:**
- "Add [name] to my watch list" → append to `config.watchList.people[]` or `config.watchList.orgs[]`
- "Remove [name]" → remove from array

**Competitors (team mode):**
- "Add [name] to competitors" → append to `config.competitors[]`
- "Remove [name]" → remove

**Adding/removing curated sources:**
- "Add [site] as a source" → append to `config.sources.curated[]`, ask about auth
- "Remove [site]" → remove from array

**Category changes:**
- "Skip [category] next time" → set `enabled: false` on that category
- "Add a new category: [name]" → append with auto-derived searchHints
- "Rename [old] to [new]" → update name

**Schedule changes:**
- "Switch to daily" / "Switch to weekly" → update and recreate scheduled task
- "Change to Tuesday 9am" → update and recreate scheduled task

**Distribution changes:**
- "Add [email] to the distribution list" → append to `config.distribution.recipients`
- "Remove [email]" → remove from recipients
- "Switch to local file only" → update method
- Users can also directly edit the `recipients` array in `~/.ai-builder-brief/config.json`

**Feedback (most common):**
- "The AI for Good section was great" → append to feedback.md
- "Skip the funding noise" → append to feedback.md
- "Make implications more specific to my coaching product" → append to feedback.md

**Show settings:**
- "Show my settings" / "What's my config?" → display a formatted summary of config.json

After any change, write the updated config to `~/.ai-builder-brief/config.json`.
If the schedule changed, delete the old scheduled task and create a new one.

---

## Phase D: Manual Trigger

When the user invokes `/ai-builder-brief` and onboarding is complete:
- Run the full Briefing Generation Workflow (Phase B) immediately
- Skip the schedule — this is an on-demand run
- Still respect dedup and feedback

---

## Error Handling

| Situation | Behavior |
|---|---|
| WebSearch unavailable | Cannot generate briefing. Notify user and stop. |
| Resend API fails | Fall back to local-file delivery. Notify user with error. |
| Resend API key missing | Notify: "Add your Resend API key to ~/.ai-builder-brief/.env" |
| Resend domain not verified | Notify: "Verify your domain at resend.com/domains, or use onboarding@resend.dev for testing" |
| Gmail MCP unavailable | Fall back to local-file delivery. Notify user. |
| WebFetch fails on curated source | Skip that source. Note in footer. |
| Authenticated source without cookies | Skip. Note: "Run /setup-browser-cookies to enable [source]." |
| All categories return nothing | Skip edition. Notify: "No significant developments found." |
| Rate limiting on WebSearch | Wait 5 seconds, retry once. If still failing, skip that category. |
| Config file corrupted/missing fields | Re-run onboarding for missing fields only. Don't lose existing config. |

---

## Privacy

- All news is fetched via WebSearch/WebFetch at runtime (no central feed, no third-party service)
- Your personal context stays in `~/.ai-builder-brief/config.json` on your machine
- No data leaves the machine except WebSearch queries and delivery emails
- WebSearch queries use AI industry terms, not proprietary information
- Curated source cookies stay in your local browser profile
- **Resend note:** When using Resend delivery, your briefing HTML passes through Resend's servers to deliver the email. Your config and personal context are NOT included — only the generated briefing content.
