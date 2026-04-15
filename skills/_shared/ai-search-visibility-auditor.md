---
name: "AI Search Visibility Auditor"
category: _shared
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hours/audit"
version: 1.0
last_eval_score: null
inspiration: "2026 AI search landscape — homeowners increasingly ask Claude, ChatGPT, Gemini, and Perplexity for contractor recommendations, rewarding sites with transparent pricing, credentials, local specificity, and structured data over traditional keyword SEO"
---

# 🔎 AI Search Visibility Auditor

## Purpose

Run a diagnostic audit of a roofing contractor's online presence against the way modern AI assistants rank and cite local businesses. Produces a prioritized remediation plan covering authority signals, pricing transparency, local/climate specificity, and structured data — the four levers that determine whether a roofer gets mentioned when a homeowner asks an AI "who should I hire to fix my roof near me?"

## When to Use

- Annual marketing plan review, or whenever a contractor notices declining inbound leads from organic channels
- When a competitor starts showing up in AI-assistant responses and the contractor does not
- Before investing in paid search or SEO, to fix foundational visibility first
- After a website redesign or domain migration
- To produce a before/after measurement baseline when rolling out AI-search-focused content

## Required Input

Provide the following:

1. **Company basics** — Company name, primary service area (counties/cities), website URL, Google Business Profile URL, phone, physical address, years in business
2. **Top service categories** — The 5–8 core services the contractor wants to be cited for (e.g., storm damage repair, insurance claim support, standing seam metal, flat roof replacement)
3. **Climate & region detail** — State, climate zone, common local weather events (hail, hurricane, wildfire, snow load), and any regional code amendments
4. **Current content inventory** — List of existing pages or a sample of 5–10 URLs to audit; blog cadence; whether they have FAQ/How-To pages
5. **Credentials & authority assets** — Licenses, manufacturer certifications (GAF Master Elite, CertainTeed SELECT ShingleMaster, Owens Corning Platinum), insurance certificates, BBB rating, Google review count and average, any awards or press mentions
6. **Competitors to benchmark** — 2–4 local competitors the contractor is losing visibility to

## Instructions

You are running an AI-search-visibility audit. Your output is a diagnostic report plus a prioritized 90-day remediation plan, written for a marketing manager or owner-operator, not a technical SEO specialist.

**Before you start:**
- Load `config.yml` from the repo root for company tone, brand voice, and any prior audits on file
- Reference `knowledge-base/industry-overview.md` for current AI adoption stats
- Reference `knowledge-base/terminology/` to calibrate technical depth for each service category
- Cross-reference `knowledge-base/tools-ecosystem/ai-tools-landscape.md` for the current tool set that contractors should be tracking

**Audit framework — four levers:**

### Lever 1: Authority & Trust Consistency
Check these items across the website, Google Business Profile, Facebook page, Yelp, and any directory listings:
- Business name matches exactly in every location (no "Roofing Co." vs. "Roofing Company, LLC" mismatches)
- Address, phone, hours, and service area identical everywhere
- Credentials published on the site with supporting images (license cards, cert certificates, insurance COI)
- Owner/crew bios with real names and years of experience
- Review count and recency across Google, BBB, Nextdoor, Angi — AI assistants weight recency, not just totals
- Schema markup: `LocalBusiness`, `RoofingContractor`, `AggregateRating`, `FAQPage`, `HowTo` on relevant pages

### Lever 2: Pricing Transparency
AI assistants reward contractors who publish ranges over those who hide pricing. Audit whether the contractor has:
- A dedicated pricing page with ranges by roof size, material, and complexity tier
- Typical project cost examples with enough specificity to anchor homeowner expectations
- Tariff/market adjustment notes (cross-reference the `tariff-price-adjuster` skill output if recent)
- Financing options and payment plans listed plainly
- A one-sentence "what drives cost up or down" explainer per material type

### Lever 3: Local & Climate Specificity
Generic contractor content does not get cited. Audit whether pages include:
- Geographic qualifiers in headings (city/county/zip), not just in metadata
- Climate-specific material recommendations (e.g., Class 4 impact shingles for hail-prone zones, Cool Roof ratings for hot-arid regions)
- Local code citations (wind rating requirements, ice-and-water-shield code, historic district rules)
- Neighborhood/subdivision references with before/after project photos when available
- Storm-event microcontent published within 48 hours of local major weather events

### Lever 4: Structured, AI-Friendly Content
AI assistants pull 150–300 word answer-shaped blocks. Audit whether content is:
- Chunked with descriptive H2/H3 headings that restate the question a homeowner would ask
- Answer-first in each section (lead with the answer, then the caveats)
- Supported by FAQ schema with 5–10 Q&As per service page
- Rich with specific numbers (years, percentages, square footage, lifespan)
- Free of thin/duplicate pages (every service page 600+ words, unique per city if multi-city)
- Linked to original research or proprietary data when possible (e.g., "average hail claim in [county] in 2025")

**Diagnostic output (audit report):**

### 1. Executive Summary
- One-paragraph overall AI-visibility grade (A–F) with rationale
- Top 3 quick wins (items that can be fixed in under 2 weeks)
- Top 3 structural issues (items requiring a 30–90 day project)
- Estimated AI-visibility uplift if all recommendations are implemented

### 2. Scorecard Table

| Lever | Score (0–10) | Top Gap | Top Opportunity |
|---|---|---|---|
| Authority & Trust | | | |
| Pricing Transparency | | | |
| Local & Climate Specificity | | | |
| Structured Content | | | |

### 3. Findings by Lever
For each lever, produce:
- What's working
- What's missing or inconsistent (with specific URL or location)
- Competitor benchmark (how the benchmarked competitors handle this)
- Concrete fix recommendation

### 4. Prompt Battery for Ongoing Monitoring
Deliver a set of 10–15 test prompts the contractor can run monthly against ChatGPT, Claude, Gemini, and Perplexity to track their citation frequency. Examples to draft:
- "Best roofing contractor in [city] for hail damage"
- "Who repairs [material type] in [region]"
- "[Carrier name] insurance claim roofer [city]"
- "Roof replacement cost [zip] 2026"

Instruct the contractor to log, for each prompt: whether they appear, which competitors appear, what supporting detail is cited about each.

### 5. 90-Day Remediation Plan

| Week | Action | Lever | Owner | Deliverable |
|---|---|---|---|---|
| 1 | NAP consistency audit + fixes across directories | Authority | Marketing | Consistency report |
| 2 | Pricing page with ranges | Pricing | Owner + Estimator | Published page |
| 3–4 | 5 city/neighborhood service pages | Local | Writer | 5 pages live |
| ... | | | | |

### 6. Measurement Plan
- Baseline the 10–15 monitoring prompts before any changes
- Re-run monthly and track AI Share of Voice (% of prompts returning your company)
- Pair with standard lead source tracking in CRM to connect visibility to leads

**Output requirements:**
- Written for a non-technical owner-operator — translate jargon (NAP, schema, E-E-A-T) on first use
- Every finding references a specific URL, screenshot callout, or competitor example
- The 90-day plan sequences quick wins first to build momentum
- Saved to `outputs/audits/{company-slug}-ai-visibility-audit-{YYYY-MM}.md` if the user confirms
- Recommend companion skills: `estimate-builder` for pricing-page content, `tariff-price-adjuster` for pricing transparency copy, `follow-up-sequence` for review solicitation

## Example Output

> [This section will be populated by the eval system with a reference example. Run with sample input to anchor format.]
