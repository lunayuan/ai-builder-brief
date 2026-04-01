# AI Builder Brief

**Your daily AI signal, filtered through YOUR world.**

Most people get AI news from LinkedIn, X, people they follow, podcasts, news channels — a patchwork of sources with no filter for what actually matters to *them*.

AI Builder Brief fixes that. It asks you 8 questions about your world, then delivers a personalized briefing where every item has a "so what does this mean for ME" line — tailored to your role, your projects, and your reasons for tracking AI.

Built for people who build with AI. Not for people who just talk about it.

![Sample Briefing](examples/briefing-screenshot.png)

**[See the full interactive sample](https://htmlpreview.github.io/?https://github.com/lunayuan/ai-builder-brief/blob/master/examples/sample-briefing.html)**

## Runs on Autopilot

Once set up, AI Builder Brief uses **Claude Code's built-in scheduling feature** to send your briefing automatically — no manual triggers, no cron jobs, no scripts.

During onboarding, the skill registers a scheduled task directly inside Claude Code. From then on, Claude Code wakes up on your chosen schedule, runs the full search-and-synthesis workflow, and delivers the briefing to your inbox. You don't have to do anything.

```
Set up once → briefing arrives in your inbox every morning
```

This is powered by Claude Code's native `/schedule` capability. The scheduled task lives in your Claude Code environment and runs even when you're not in a session.

## What It Scans For

Every edition automatically searches for what matters most to AI builders:

**Latest Models & Technology** — New model releases, benchmark results, pricing changes, capability jumps. Know when to switch models, when to wait, and how new releases affect your cost structure.

**Trending Demos & Product Inspiration** — Viral AI products, impressive demos, breakout apps. See what other builders are shipping so you can spot patterns, get inspired, and stay ahead of what users will expect.

**Popular Agent Skills & GitHub Repos** — Trending repos, new MCP tools, agent frameworks, skill packs gaining traction. Find tools that make your own agents smarter and discover what the open-source community is building.

**Builder Voices** — Notable quotes and takes from the builders on your watch list. What are Andrej Karpathy, Sam Altman, and the people you follow actually saying this week? Not news *about* them — their own words, opinions, and predictions.

**AI for Good** — AI applied to education, climate change, coaching, and social impact. Who's using AI to solve real problems beyond productivity? Track the builders and orgs making AI work for people, not just profits.

These are the defaults. You can add, remove, or rename categories during setup — or later, just by telling your agent.

## Quick Start

1. Install:
   ```bash
   mkdir -p ~/.claude/skills && git clone https://github.com/lunayuan/ai-builder-brief.git ~/.claude/skills/ai-builder-brief
   ```

2. In Claude Code, say **"set up ai builder brief"** or type `/ai-builder-brief`

3. Answer 8 questions. Your first briefing generates immediately.

4. The skill registers a **scheduled task** in Claude Code — your briefing will now run automatically on your chosen schedule (daily, weekly) without any further action from you.

No config files. No code. The agent walks you through everything in conversation.

**Requirements:** [Claude Code](https://claude.ai/code) with WebSearch. Optional: [Resend](https://resend.com) (free) for email delivery, Gmail MCP for draft delivery.

## The 8 Questions

The magic is the onboarding. These questions shape everything — from what gets searched to how each item is framed for you:

| # | Question | Why it matters |
|---|----------|---------------|
| 1 | What's your name? What do you do? | Your name appears in the email subject ("AI Builder Brief for Luna") and header. Your role frames every insight. |
| 2 | Why does AI matter to you? | Drives the "so what" on every item |
| 3 | What AI topics to track? | Focuses the search |
| 4 | What categories? | Organizes your briefing |
| 5 | Who to watch? | Tracks specific builders and orgs |
| 6 | Must-read sources? | Checks your favorite newsletters first |
| 7 | How often? | Daily or weekly |
| 8 | How to deliver? | Email, Gmail draft, or local file |

When you pick your topics, the agent asks a follow-up: *"Why is this important to you specifically?"* That answer becomes the lens for every implication line. Two people tracking the same topic get different briefings based on their reasons.

## What You Get

- **Executive summary** — 3-5 biggest signals, scannable in 60 seconds
- **News by category** — customizable sections tailored to your focus
- **Personal implications** — every item filtered through your world ("This means X for what you're building")
- **Source links** — every item links to the original source, inline and verifiable. No hallucinated intelligence.
- **"What to Watch"** — forward-looking items for the coming days
- **Deduplication** — never see the same story twice across editions

## Default Watch List

Out of the box, AI Builder Brief tracks the builders and orgs shaping the AI ecosystem. You can customize everything, but here's the starting lineup:

**Builders & Leaders**

Andrej Karpathy, Sam Altman, Dan Shipper, Swyx, Peter Yang, Garry Tan, Amjad Masad, Matt Turck, Aaron Levie, Boris Cherny, Tomer Cohen, Claire Vo, Chip Huyen, Ravi Mehta, Elena Verna, Ethan Smith, Jenny Wen, Nick Turley, Howie Liu, Asha Sharma, Bret Taylor, Marc Andreessen, Fei-Fei Li, Kyle Poyar, Grant Lee, Eoghan McCabe

**Organizations**

OpenAI, Anthropic, Google DeepMind, Hugging Face, Lovable, Replit, Cursor, Gamma, ElevenLabs, HeyGen, Graphite

**Curated Sources**

Lenny's Newsletter, Every by Dan Shipper, Growth Unhinged, GitHub Blog

## Example Setups

| Who | Focus | Default categories |
|-----|-------|--------------------|
| **AI Builder** | Models, tools, repos, voices | Latest Models, Trending Demos, Agent Skills, Builder Voices |
| **AI PM** | Workflows, products | Latest Models, AI Products, Growth & Distribution |
| **AI Founder** | Funding, competitive landscape | Trending Demos, Funding & Launches, Competitors |
| **AI Team** | Shared competitive intel | Enterprise AI, Agents & Automation, Competitors |

See `config/examples/` for complete sample configurations.

## Delivery

| Method | How it works |
|--------|-------------|
| **Email (Resend)** | Sent directly to your inbox. Supports distribution lists. Free tier: 3,000 emails/month. |
| **Gmail draft** | Created in Gmail. You review, edit, and send when ready. |
| **Local file** | Saves HTML to a folder. Copy into Outlook, Teams, or any system. |

## Changing Settings

Tell your agent conversationally — no files to edit:

- "Add Anthropic to my watch list"
- "Switch to daily briefings"
- "Emphasize the GitHub repos section"
- "Skip the funding noise unless there's a major round"
- "Add sarah@team.com to the distribution list"
- "Show me my current settings"

You can also directly edit `~/.ai-builder-brief/config.json` if you prefer.

## Customizing the Briefing

The briefing's behavior is controlled by plain-English prompt files in `prompts/`:

| File | What it controls |
|---|---|
| `briefing-structure.md` | HTML layout and section ordering |
| `synthesize-category.md` | How items are summarized |
| `personal-implications.md` | How "so what" lines are written |
| `executive-summary.md` | How the exec summary is composed |
| `search-queries.md` | How search queries are built from your config |

These are plain English instructions, not code. To customize, copy any file to `~/.ai-builder-brief/prompts/` and edit it there. Your overrides always take priority.

## Personal vs Team Mode

| | Personal | Team |
|---|---|---|
| Identity question | "What do you do?" | "What company is this for?" |
| Competitors | Skipped | Asked |
| Watch list | People you follow | People + competitor orgs |
| Implications | "So what for ME" | "So what for OUR company" |
| Distribution | Your inbox | Distribution list |

## Privacy

- All news is fetched via WebSearch at runtime — no central feed, no third-party service
- Your personal context stays on your machine in `~/.ai-builder-brief/config.json`
- Search queries use AI industry terms, not proprietary information
- When using Resend, only the generated briefing HTML passes through their servers — your config and personal context stay local
- Nothing is shared with anyone unless you choose to

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT — see [LICENSE](LICENSE)
