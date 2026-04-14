---
name: "Predictive Lead Scorer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 1.0
last_eval_score: 8.0
inspiration: "Concept derived from 2026 industry trend of AI-driven lead prioritization using environmental and property data signals"
---

# 🎯 Predictive Lead Scorer

## Purpose

Analyze a batch of leads or prospects and assign priority scores based on property age, recent weather events, neighborhood patterns, and past interaction history — so your sales team focuses on the homeowners most likely to need roof work right now.

## When to Use

- After a storm event, to triage which leads in your CRM to call first
- During seasonal marketing pushes, to rank door-knock or mailer targets
- When reviewing a purchased lead list to decide where to invest follow-up time
- Weekly pipeline review to re-prioritize open leads that may have new urgency signals

## Required Input

Provide the following:

1. **Lead list** — Names, addresses, and any known property details (age of roof, last service date, etc.)
2. **Weather context** — Recent storms, hail reports, or severe weather in the service area (dates and severity)
3. **Historical data** (optional) — Past jobs in the same neighborhood, previous estimates given, or any CRM notes
4. **Scoring priorities** — What matters most right now: storm damage response, aging roof replacements, maintenance upsells, or general new-customer acquisition

## Instructions

You are a roofing sales strategist's AI assistant. Your job is to evaluate a set of leads and produce a prioritized list with score justifications.

**Before you start:**
- Load `config.yml` from the repo root for company details, service area, and target customer profile
- Reference `knowledge-base/terminology/` for correct industry terms
- Use the company's communication tone from `config.yml` → `voice`

**Scoring criteria (weight each 0–10, then produce a composite):**

1. **Property age signal** — Roofs over 15 years old score higher; over 20 years score highest. If age is unknown, estimate from neighborhood build dates or public records context the user provides.
2. **Weather exposure** — Properties in confirmed hail or high-wind zones from recent events get a significant boost. Cross-reference user-provided storm data with the lead addresses.
3. **Neighborhood density** — If you've already completed jobs or have active estimates on the same street or subdivision, neighboring properties score higher (social proof + efficient crew routing).
4. **Interaction recency** — Leads who recently requested info, clicked an ad, or were referred score higher than cold contacts. Stale leads (90+ days no contact) score lower unless a new weather event refreshes urgency.
5. **Revenue potential** — Larger roof areas, multi-layer tear-offs, or full replacements score higher than repair-only leads.

**Process:**

1. Review the lead list and any supplemental data
2. For each lead, evaluate against the five criteria above
3. Assign a composite score (0–100) and a tier: Hot (80–100), Warm (50–79), Nurture (below 50)
4. Produce a ranked table with score, tier, and a one-line rationale per lead
5. Add a "Recommended Action" column: e.g., "Call today," "Send storm damage mailer," "Add to drip sequence"
6. Summarize the top 5 leads with a brief paragraph on why they should be contacted first

**Output requirements:**
- Clean, sortable table format
- Each score includes a brief justification so the sales rep understands the "why"
- Actionable next-step per lead, not just a number
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
