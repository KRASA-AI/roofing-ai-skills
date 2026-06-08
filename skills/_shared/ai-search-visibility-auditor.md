---
name: "AI Search Visibility Auditor"
category: _shared
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hours/audit"
version: 1.2
last_eval_score: null
inspiration: "2026 AI search landscape — homeowners increasingly ask Claude, ChatGPT, Gemini, and Perplexity for contractor recommendations, rewarding sites with transparent pricing, credentials, local specificity, and structured data over traditional keyword SEO. v1.1 (May 2026) adds a Mobile-AI lever covering Apple Business Connect (pre-WWDC 2026 / Siri-with-Gemini, June 8 keynote), Google's May 6 'Five Doors' update to AI Overviews and AI Mode (Community Perspectives pulling Reddit/forums, inline citations, Further Exploration deep-dive content), camera-search readiness (visual-index alignment between branded photos and GBP), and the AI-call attribution shift from website sessions to GBP-routed phone numbers. v1.2 (June 8 2026) updates the Mobile-AI lever from anticipated to confirmed after the WWDC keynote shipped the rebuilt Gemini-backed Siri inside the Dynamic Island, formally deprecated the older SiriKit intent framework, and made the App Intents framework the only path by which a third-party app can expose actions Siri can call and chain — concept extraction from keynote-day reporting only, no source text reused."
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

**Audit framework — five levers (v1.2):**

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
- Quotable in single sentences a Google AI Overviews inline citation can lift verbatim (e.g., "Most roof replacements in [city] run between $X and $Y in 2026.") — the May 6, 2026 "Five Doors" update moves citations from end-of-response footers to inline placement beside the supporting sentence, so a paragraph that has no liftable sentence does not get the inline link
- Capable of populating Google's "Further Exploration" section — at least one 1,200+ word, dated, cost-itemized pillar page per highest-margin service line, with real local timelines, costs, and named neighborhoods
- Surfaced through hover-preview-friendly title tags and meta descriptions (specific phrasing like "Same-week tear-off and re-roof in [city]" beats generic taglines like "Quality roofing since 1998" in the new desktop hover-preview popup)

### Lever 5: Mobile-AI Readiness (Apple Business Connect, AI Overviews, Camera Search)
The mobile-AI surface is a separate channel from the desktop-web SEO channel, with its own ranking signals and its own data sources. The May 6, 2026 Google AI Overviews "Five Doors" update plus the **now-shipped** June 8, 2026 WWDC keynote — which confirmed the rebuilt, Gemini-backed Siri living in the Dynamic Island, the formal deprecation of the older SiriKit intent framework, and App Intents as the sole framework through which a third-party app can expose actions Siri can call and chain across apps — make this lever decisive through the rest of the 2026 storm season. Two practical consequences for a roofer reading this after the keynote: (1) the assistant a homeowner now talks to on the lock screen reasons over the same local-business data feeds (Apple Business Connect, Google Business Profile, the visual index) rather than a keyword index, so the items below are the levers that decide whether the shop is one of the names Siri reads aloud; (2) any roofing app or booking tool the shop relies on will surface inside Siri only if its developer has migrated to App Intents — worth asking the shop's CRM/booking vendor where they stand on that migration, since a vendor still on the deprecated framework will quietly drop out of Siri's reach over the 2–3 year deprecation window. Audit the following:

- **Apple Business Connect listing claimed and complete.** Hours, services, photos, place card, social/web links. As of mid-May 2026 industry reporting cites 86% of contractors as not having claimed their Apple Business Connect listing — the single largest mobile-AI visibility gap in the trades. Claiming is free and takes about 10 minutes; for shops in iPhone-heavy markets (most of the U.S. coastal metros and many suburban storm-belt zips) this is the single highest-leverage move on the lever
- **Google Business Profile depth and freshness.** Every service category populated, every Q&A answered, 20+ fresh photos added in the last 90 days, response to every review (positive and negative), special-hours updates seasonally, posts within 7 days for storm-response messaging
- **Reddit / forum / local-Facebook-group presence.** Google's new "Community Perspectives" panel routes Reddit threads from r/Roofing, r/HVAC, r/Insurance, city subreddits, Nextdoor, and local-contractor Facebook groups directly into AI answers. Audit whether the shop has a claimed business handle in three local subreddits, one local Facebook group, and the city subreddit; whether the shop's CSRs and PMs are trained to give real (non-promotional) answers in those forums; and whether mentions of the brand in those forums use the exact business name (so AI can disambiguate)
- **Review specificity for AI fuel.** Generic "five stars great job" reviews still help the aggregate star rating but no longer fuel Community Perspectives citations. Audit whether the shop is asking for reviews that name the service (panel upgrade vs. tear-off vs. supplement appeal), the neighborhood, and the technician — "Could you mention that we replaced your storm-damaged roof in [neighborhood]?" Reviews like that become the entity-strength fuel the new AI surface rewards
- **Visual-index alignment.** Both Google Search Live (global on March 26, 2026) and the new Siri can match what the camera sees to a contractor. Audit whether the shop's van photos, job-site photos, and product photos are in Google Images, properly alt-tagged, and tied to the GBP. A clean shingle photo with branded markings can surface when an iPhone points at storm damage; an unbranded stock-photo gallery generally does not
- **CarPlay-readiness.** ChatGPT-in-CarPlay shipped April 9, 2026 with precise device location. Audit whether the GBP indicates 24/7 emergency availability accurately (don't claim it if you don't deliver it), whether the service-area polygon covers all commute corridors a driving homeowner would reach the shop from, and whether the storm-response messaging on the GBP can survive being read aloud by a voice agent (no abbreviations, no jargon)
- **AI-routed-call attribution.** As AI assistants increasingly dial the GBP-routed phone number directly rather than referring the user to the website, GA4 reports an artificial AI-search decline while inbound call volume is actually rising. Audit whether the shop has GBP-call tracking enabled (separate number, source-tagged), whether the CRM ingests the call-source data automatically, and whether the dashboard is benchmarking against the call tracker rather than only website sessions

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
| Mobile-AI Readiness (v1.2) | | | |

### 3. Findings by Lever
For each lever, produce:
- What's working
- What's missing or inconsistent (with specific URL or location)
- Competitor benchmark (how the benchmarked competitors handle this)
- Concrete fix recommendation

### 4. Prompt Battery for Ongoing Monitoring
Deliver a set of 10–15 test prompts the contractor can run monthly against ChatGPT, Claude, Gemini, Perplexity, Google AI Mode, and the now-live Siri-with-Gemini on an iPhone running iOS 27 (rebuilt Siri shipped at the June 8, 2026 WWDC keynote). Examples to draft:
- "Best roofing contractor in [city] for hail damage"
- "Who repairs [material type] in [region]"
- "[Carrier name] insurance claim roofer [city]"
- "Roof replacement cost [zip] 2026"
- (For Google AI Mode) "Do I need to check my roof before hurricane season in [metro]"
- (For Siri-with-Gemini / camera mode) Point the phone camera at a roof photo and ask "What's wrong with this roof and who fixes it near me?"
- (For ChatGPT-in-CarPlay) "ChatGPT, my roof is leaking after the storm last night, find me a roofer who can come today" (test from a driving phone within service-area drive time)

Instruct the contractor to log, for each prompt: whether they appear, which competitors appear, what supporting detail is cited about each, whether the citation is an inline link beside a sentence (the May 6, 2026 Five Doors pattern) or an end-of-response footer link, and whether any Reddit / Nextdoor / Facebook quote in a "Community Perspectives" or "Expert Advice" block names the shop or a competitor.

### 5. 90-Day Remediation Plan

| Week | Action | Lever | Owner | Deliverable |
|---|---|---|---|---|
| 1 | NAP consistency audit + fixes across directories | Authority | Marketing | Consistency report |
| 1 | Apple Business Connect: claim, complete, verify (free; ~10 minutes) | Mobile-AI | Marketing | Claimed listing |
| 2 | Pricing page with ranges | Pricing | Owner + Estimator | Published page |
| 2 | GBP audit: service categories, Q&A, 20 fresh photos, review response | Mobile-AI | Marketing | GBP scorecard |
| 3–4 | 5 city/neighborhood service pages | Local | Writer | 5 pages live |
| 3–4 | Top three service-page H2s rewritten as homeowner-spoken questions | Structured / Mobile-AI | Writer | Updated H2s live |
| 5 | One 1,200+ word "Further Exploration" pillar page on highest-margin service | Structured | Writer | Pillar page live |
| 5 | Local-subreddit / Facebook-group business handles claimed; first non-promotional post | Mobile-AI | Marketing | Three claimed handles |
| 6 | CSR review-request script update for service-specific, neighborhood-specific phrasing | Mobile-AI | Owner + CSR Lead | Updated script + 20 new reviews |
| 7 | Call tracking enabled on GBP number with source data flowing to CRM | Mobile-AI | Marketing + Ops | Source-tagged call dashboard |
| 8–12 | Re-run prompt battery monthly; expand pillar pages to remaining service lines | All | Marketing | Monthly visibility report |

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
