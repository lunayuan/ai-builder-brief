# Personal Implications

This is the key differentiator. Every news item gets a one-sentence "so what does this mean for YOUR world" implication line. This is what turns a generic news roundup into personal strategic intelligence.

## How to Write Implications

Read `config.identity` and `config.industry` to understand:
- **What the reader does** (`config.identity.description`)
- **Why they track AI** (`config.identity.why`)
- **What specific focus areas matter and why** (`config.industry.focusReasons`)

Then for each item, ask: "If I were this reader, doing what they do, caring about what they care about — why would this news matter to me?"

Use `config.identity.why` as the primary lens. For category-specific items, also use the matching `config.industry.focusReasons` entry to sharpen the implication.

## Tone: Observational, Not Prescriptive

**GOOD — flag the signal:**
- "This new model's coding ability directly affects the AI coaching product you're building"
- "A price drop here changes the economics of your agent architecture"
- "This competitor just shipped the feature you've been prototyping"
- "New regulation could affect how you deploy AI in your product"
- "This framework is gaining traction in the exact space you're working in"
- "Your target users are already adopting this workflow — worth watching"

**BAD — never prescribe action:**
- "You should switch to this model immediately"
- "You need to pivot your roadmap toward..."
- "We recommend exploring this technology"
- "This requires your immediate attention"
- "You must respond to this competitive threat"

## When No Clear Implication Exists

Use: "No direct implication for your work — included for broader AI context."

This is fine. Not every item needs a personal implication. Including it for context is honest and useful.

## Think About These Dimensions

- **Tools and workflow:** Does this change how you build or ship?
- **Competitive landscape:** Does this strengthen or threaten someone in your space?
- **Cost and access:** Does this make something cheaper, faster, or newly possible?
- **Talent and hiring:** Does this shift what skills matter?
- **Regulatory exposure:** Does this change compliance or deployment requirements?
- **User behavior:** Are your users or audience adopting new AI patterns?

## Format

Render as italicized text below the item summary:
```
Your implication: This new pricing tier makes your agent architecture viable at 10x the current scale.
```

Use "Your implication:" as the label — direct, personal, addressed to the reader.
