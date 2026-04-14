# Roofing AI Skills

**Free, open-source AI prompts and workflows built for roofers.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for roofing. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Crew Schedule Optimizer | Balance crew availability, weather windows, job priority, route efficiency, and material delivery timing into an optimized weekly plan — with built-in contingency triggers for weather disruptions. | ~30 min/week |
| Material Order Calculator | Calculate exact material quantities from roof measurements — broken down by material category with appropriate waste factors, bundle/unit conversions, and supplier-ready formatting — so you can place an accurate order without over- or under-buying. | ~15 min/order |
| Roof Inspection Report | Turn field inspection photos and notes into a professional, structured inspection report with section-by-section findings, condition ratings, photo references, and prioritized recommendations — suitable for homeowner presentation, insurance documentation, or real estate transactions. | ~30 min/report |
| Estimate Builder | Convert measurements and material choices into a formatted, customer-ready estimate — with built-in pricing sanity checks, good-better-best option tiers, and clear scope documentation that reduces change orders. | ~25 min/estimate |
| Follow-Up Sequence | Generate a multi-touch follow-up cadence for open roofing estimates — with channel-specific messages (text, email, phone scripts, door knock talking points), roofing-specific urgency hooks, and objection-handling frameworks tailored to why homeowners delay roof decisions. | ~10 min/lead |
| Maintenance Plan Generator | Create a customized preventive maintenance plan and subscription proposal for a customer's roof, based on its age, material type, local climate patterns, and repair history. | ~20 min/plan |
| Predictive Lead Scorer | Analyze a batch of leads or prospects and assign priority scores based on property age, recent weather events, neighborhood patterns, and past interaction history — so your sales team focuses on the homeowners most likely to need roof work right now. | ~30 min/batch |
| Tariff & Price Adjustment Communicator | Help roofing contractors communicate material price increases and tariff impacts to homeowners clearly and professionally — framing higher costs as industry-wide realities rather than company markups, presenting lifecycle cost comparisons across material tiers, and generating customer-facing content that builds trust during periods of price volatility. | ~15 min/estimate |
| Lead Response Automator | Generate instant, channel-appropriate responses to new roofing leads — qualifying them by job type, urgency, and fit — and produce a structured intake sequence that captures property details, schedules inspections, and hands off a fully qualified lead to the estimator within minutes rather than hours. | ~20 min/lead |
| Insurance Supplement Writer | Draft supplement requests to insurance carriers that recover underpaid line items, missing overhead & profit (O&P), code-upgrade costs, depreciation, and code-required accessories — structured around the carrier's Xactimate estimate so adjusters can approve changes quickly. | ~45 min/supplement |
| Email Drafter | Turn rough notes into a polished, roofing-industry-appropriate email — with the right structure, tone, and call-to-action for the recipient type (homeowner, adjuster, supplier, crew lead, referral partner). | ~10 min/email |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft public, professional responses to online reviews on Google, Nextdoor, BBB, Angi, Yelp, Facebook, and manufacturer / contractor directories — with tailored tone based on star rating and the roofing-specific concern raised, plus an escalation path for legitimate complaints or inaccurate claims. | ~10 min/review |

**Total time saved per use: ~270+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/roofing-ai-skills.git
cd roofing-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
roofing-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Roofing AI guide**: [krasa.ai/industries/roofing](https://krasa.ai/industries/roofing)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
