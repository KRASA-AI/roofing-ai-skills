# Roofing AI Skills

**Free, open-source AI prompts and workflows built for roofers.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for roofing. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Crew Schedule Optimizer | Turn active jobs, crew availability, weather forecast, and material-delivery commitments into an optimized weekly plan — with a day-by-day grid, per-job crew packets, route clusters, weather contingencies, and a utilization report per crew. | ~45 min/week |
| Material Order Calculator | Calculate exact material quantities from roof measurements — broken down by material category with appropriate waste factors, bundle/unit conversions, and supplier-ready formatting — so you can place an accurate order without over- or under-buying. | ~15 min/order |
| Roof Inspection Report | Turn field inspection photos and notes into a professional, structured inspection report with section-by-section findings, condition ratings, photo references, and prioritized recommendations — suitable for homeowner presentation, insurance documentation, or real estate transactions. | ~30 min/report |
| Safety Toolbox Talk Generator | Draft a short (5–10 minute) OSHA-aligned toolbox talk for a roofing crew that targets the specific hazards of today's job, ties to current weather and site conditions, and produces a sign-off sheet that doubles as compliance evidence. | ~25 min/talk |
| Commercial Prospect Researcher | Build a qualified commercial-roofing prospect list by gathering public data about target buildings (age, size, industry, ownership), identifying the actual decision-maker (facility manager, property manager, or building owner), and producing a personalized outreach brief the sales rep can turn into email, LinkedIn, or phone outreach. | ~2 hours/prospect list |
| Estimate Builder | Convert measurements and material choices into a formatted, customer-ready estimate — with built-in pricing sanity checks, good-better-best option tiers, and clear scope documentation that reduces change orders. | ~25 min/estimate |
| Follow-Up Sequence | Generate a multi-touch follow-up cadence for open roofing estimates and cold leads — with channel-specific messages (text, email, phone scripts, door-knock talking points) pre-filled with customer, property, estimate, and config details — plus roofing-specific urgency hooks and a complete objection-response matrix. | ~10 min/lead |
| Maintenance Plan Generator | Build a customized preventive maintenance plan and tiered subscription proposal for a roof — grounded in the material type, climate exposure, roof age, and repair history — with defensible pricing, ROI math the homeowner can verify, and subscription terms ready for recurring billing. | ~25 min/plan |
| Predictive Lead Scorer | Rank a batch of leads or prospects by likelihood-to-close and expected ticket size — using a transparent, weighted composite that a sales rep can audit — so the call-list, door-knock list, and mailer list all start with the homeowners most likely to need (and buy) roof work right now. | ~30 min/batch |
| Storm Canvassing Prioritizer | After a hail or wind event, turn the storm footprint into a prioritized canvassing plan in under an hour. | ~3 hours/storm event |
| Tariff & Price Adjustment Communicator | Produce customer-facing content that absorbs material-cost volatility without eroding close rates — price-adjustment letters, estimate addenda, website pricing pages, objection-handling scripts, and lifecycle-cost comparison sheets — all grounded in real market data and framed as industry-wide realities rather than company markups. | ~15 min/estimate |
| Lead Response Automator | Generate instant, channel-appropriate responses to new roofing leads — qualifying them by job type, urgency, and fit — and produce a structured intake sequence that captures property details, schedules inspections, and hands off a fully qualified lead to the estimator within minutes rather than hours. | ~20 min/lead |
| Insurance Appeal Inspection Report | Produce a homeowner-side counter-inspection report specifically engineered to contest a carrier-driven non-renewal, repair demand, or ACV downgrade triggered by third-party aerial imagery AI. | ~60 min/report |
| Insurance Supplement Writer | Draft supplement requests to insurance carriers that recover underpaid line items, missing overhead & profit (O&P), code-upgrade costs, depreciation, and code-required accessories — structured around the carrier's Xactimate estimate so adjusters can approve changes quickly. | ~45 min/supplement |
| AI Search Visibility Auditor | Run a diagnostic audit of a roofing contractor's online presence against the way modern AI assistants rank and cite local businesses. | ~3 hours/audit |
| Email Drafter | Turn rough notes into a polished, roofing-industry-appropriate email — with the right structure, tone, and call-to-action for the recipient type (homeowner, adjuster, supplier, crew lead, referral partner). | ~10 min/email |
| Meeting Summarizer | Convert raw meeting notes or transcripts from roofing-industry meetings into structured, role-appropriate summaries — with action items (owner, due date), decisions logged, RFIs captured, safety notes flagged, and schedule-impact surfaced — so nothing falls through the cracks between the trailer, the truck, and the back office. | ~15 min/meeting |
| Review Responder | Craft public, professional responses to online reviews on Google, Nextdoor, BBB, Angi, Yelp, Facebook, and manufacturer / contractor directories — with tailored tone based on star rating and the roofing-specific concern raised, plus an escalation path for legitimate complaints or inaccurate claims. | ~10 min/review |
| Visual Proposal Generator | Turn a completed estimate, inspection report, or prospect brief into a polished, brand-consistent visual deliverable a homeowner or commercial decision-maker can read in under three minutes. | ~90 min/deliverable |

**Total time saved per use: ~470+ minutes across all skills.**

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
