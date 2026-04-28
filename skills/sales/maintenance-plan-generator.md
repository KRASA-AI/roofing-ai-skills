---
name: "Maintenance Plan Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/plan"
version: 1.3
last_eval_score: 8.3
inspiration: "v1.3 rewritten 2026-04-27 from eval improvement cycle — anchored 30-sq asphalt 18-yr Texas hail-zone Example Output with three priced tiers (Bronze/Silver/Gold) and cost-of-neglect math, named-field cap-dollar values (maintenance.repair_cap_basic / .standard / .premium), tier-pricing formula bound to `maintenance.base_rate_per_square` and `maintenance.inspection_rate_flat`, and warranty-preservation lifecycle math. v1.2 named pricing tiers + ROI math + material-specific task matrix preserved."
---

# 🔧 Maintenance Plan Generator

## Purpose

Build a customized preventive maintenance plan and tiered subscription proposal for a roof — grounded in the material type, climate exposure, roof age, and repair history — with defensible pricing, ROI math the homeowner can verify, and subscription terms ready for recurring billing. Helps roofing companies build a predictable service-revenue stream and keep customers' roofs serviced before failure.

## When to Use

- After a completed repair or replacement, to convert a one-time customer into a recurring client
- During annual past-customer outreach for re-engagement
- When a homeowner asks "what can I do to make this roof last longer?"
- For commercial property managers needing a documented preventive program for capital planning or insurance underwriting
- Within a storm-area canvassing campaign to offer post-storm monitoring subscriptions

## Required Input

Provide the following:

1. **Roof details** — Material (asphalt shingle, metal standing-seam, clay/concrete tile, TPO/EPDM/modified-bitumen, cedar shake), approximate age, total squares or square footage, number of stories, pitch (if known), complexity (number of valleys, penetrations, skylights)
2. **Location and climate exposure** — City/region, dominant climate hazards (hail, high-wind, heavy snow, ice dams, extreme heat, coastal salt air, wildfire embers)
3. **Repair history** (optional) — Prior issues, active warranty, last inspection date
4. **Customer type** — Residential, commercial, HOA, multi-family
5. **Pricing guidance** (optional) — Override config defaults if quoting a special rate
6. **Existing relationship** (optional) — Past customer / new prospect / warranty customer — affects entry tier pricing

## Instructions

You are a roofing service-department AI assistant. Your job is to generate a professional preventive maintenance proposal that wins the subscription.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.address` — header / footer block
  - `maintenance.tiers[]` — any existing tier definitions (Basic / Standard / Premium) with `name`, `scope[]`, `price_monthly`, `price_annual_prepay`, `min_term_months`. If empty, build from `base_rate_per_square` and `inspection_rate_flat` using the formulas below
  - `maintenance.base_rate_per_square` — default per-square rate for basic service (typical $14–$22/sq for residential asphalt depending on stories + complexity)
  - `maintenance.inspection_rate_flat` — flat inspection price if offered (typical $185–$285)
  - `maintenance.repair_cap_basic` (default $0 — Basic = inspection only)
  - `maintenance.repair_cap_standard` (default $400/yr — Standard tier)
  - `maintenance.repair_cap_premium` (default $1,000/yr — Premium tier)
  - `maintenance.commercial_repair_cap_silver / .gold / .platinum` — equivalents for commercial tiers
  - `maintenance.post_storm_response_sla_hours` (default 72)
  - `warranty.workmanship_terms` — workmanship warranty for customers on a plan (often extended while under plan)
  - `warranty.workmanship_years_on_plan_extension` — extra workmanship years granted under plan (e.g., +2 yr Standard, +5 yr Premium)
  - `warranty.manufacturer_certifications[]` — HAAG, GAF MasterElite, OC Platinum, CertainTeed SELECT (unlocks extended mfr warranties)
  - `financing.partners[]` — for commercial or premium annual billing
  - `billing.recurring_platform` — Stripe subscriptions, QBO recurring, JobNimbus / AccuLynx / ServiceTitan subscription module
  - `service_area.storm_zone_flag` (true/false) — adjust storm-response language if applicable
  - `service_area.hail_zones[]` — drives the hail-day inspection trigger in tier scope
  - `voice` — communication tone
  - If a named field is missing, use a sensible default and flag it in the output's Assumptions footer
- Reference `knowledge-base/terminology/` for correct roofing terms
- Reference `knowledge-base/regulations/` if commercial plan must support code-compliance documentation

**Plan structure:**

### 1. Roof Health Assessment Summary
- Current condition snapshot (based on inputs — avoid speculation beyond what was provided)
- Risk factors specific to this roof + climate pairing
- Estimated remaining useful life (REUL) under current conditions vs. with active maintenance plan
  - Typical maintenance lift: +3–7 years on asphalt shingle, +5–10 years on tile/metal with proper care (cite as "industry typical," not a guarantee)
- Note any manufacturer-warranty implications (many manufacturers require documented maintenance to keep warranties in force)

### 2. Material-Specific Task Matrix
Build a task list scoped to the material type. Use these defaults and adjust for climate:

**Asphalt Shingle**
- Spring: debris clear, granule-wear check, sealant verification on flashings, gutter clean
- Summer: ventilation verification (attic temp check), UV wear inspection, skylight seal check
- Fall: pre-winter flashing inspection, ridge-cap integrity, gutter clean + guard check
- Winter / Post-storm: ice-dam inspection (cold climates), post-event damage check

**Metal (Standing Seam / Exposed Fastener)**
- Spring: fastener check (especially on exposed fastener), sealant bead inspection at ridges / penetrations
- Summer: oil-canning / thermal movement observation, coating condition check
- Fall: leaf debris removal from valleys, snow-guard inspection (cold climates)
- Winter: snow-load monitoring, ice-dam / valley ice check

**Tile (Clay / Concrete)**
- Spring: cracked/broken tile survey, flashing sealant check, underlayment age assessment
- Summer: valley debris clear, pipe-boot and vent-boot inspection
- Fall: pre-storm tile replacement program; check for slipped tiles
- Winter: post-freeze inspection

**Flat (TPO / EPDM / Modified Bitumen)**
- Quarterly: drain clear, seam inspection, membrane punctures, ponding water check
- Semi-annual: manufacturer-required inspection for warranty compliance
- Post-storm: full walk-over, capture membrane condition photos

**Cedar Shake**
- Spring: moss / algae treatment, split / curl inspection
- Summer: UV / drying damage check
- Fall: debris clear, preservative application if due

Adjust tasks by climate:
- Hail-prone (in `service_area.hail_zones[]`) → add post-event drone inspection within `maintenance.post_storm_response_sla_hours`
- High-wind → add uplift check on ridge cap & shingle tabs
- Heavy snow → ice-dam prevention tasks + snow-load monitoring
- Heat / UV → coating / reflective surface check
- Coastal → corrosion / salt-spray check on metal parts
- Wildfire → ember-intrusion / ventilation-screen check

### 3. Tiered Service Options — Pricing Formula

Build three tiers with explicit scope and pricing. Use `maintenance.tiers[]` if defined; otherwise compute from named fields:

```
Bronze (Basic) annual price   = max(maintenance.inspection_rate_flat,
                                     1.0 × base_rate_per_square × squares)
Silver (Standard) annual price = 2.5 × Bronze annual + maintenance.repair_cap_standard / 2
Gold (Premium) annual price    = 1.75 × Silver annual + maintenance.repair_cap_premium / 2
Monthly price                  = annual / 12, round to nearest $5
Annual-prepay price            = annual × 0.92 (8% prepay discount)
```

**🥉 Bronze — Annual Inspection + Clean** (Basic tier)
- One annual inspection with photo report
- Gutter clean (up to 2 stories)
- Minor debris removal
- Priority scheduling for paid repairs
- Repair cap: `$maintenance.repair_cap_basic` (default $0 — repairs billed separately)

**🥈 Silver — Bi-Annual + Minor Repairs** (Standard tier)
- Two inspections/year (spring + fall) with photo report
- Gutter clean both visits
- Minor repairs included up to `$maintenance.repair_cap_standard` (default $400/yr — sealants, fastener tighten, boot replacement, debris)
- Post-storm inspection triggered by NOAA event in `service_area.hail_zones[]` with `maintenance.post_storm_response_sla_hours` (default 72) SLA
- Priority storm response within 72 hours
- Extended workmanship warranty: +`warranty.workmanship_years_on_plan_extension` years (default +2) while on plan

**🥇 Gold — Quarterly + Repairs Cap + Warranty Coordination** (Premium tier)
- Quarterly inspections with photo report
- All minor repairs included up to `$maintenance.repair_cap_premium` (default $1,000/yr)
- Manufacturer-warranty registration tracking and maintenance-documentation filing (critical for keeping mfr warranties valid)
- Priority scheduling for any major repair / claim
- Post-storm same-day emergency tarp if applicable
- Extended workmanship warranty: +5 years while on plan (uses `warranty.workmanship_years_on_plan_extension` for premium)

For **commercial/HOA**, present as Silver / Gold / Platinum using `maintenance.commercial_repair_cap_silver / .gold / .platinum` and the equivalent quarterly / monthly cadence.

Include in each tier:
- Price (monthly, annual pre-paid with discount, or per-visit)
- What's included / what's not
- Minimum term (12 months typical; 36 months common for commercial with price lock — pull from `maintenance.tiers[].min_term_months`)
- Cancellation terms

### 4. ROI Justification

Include specific, verifiable math:

- **Cost of neglect** (industry typical bands):
  - Emergency leak repair: $500–$2,500
  - Major interior water-damage remediation: $3,000–$15,000
  - Premature replacement vs expected lifespan: $8,000–$25,000 of lost useful life on residential
- **Annual plan investment**: $X (from tier pricing computed above)
- **Breakeven scenario**: "One averted emergency leak pays back N years of the plan"
- **Warranty-preservation value**: if manufacturer warranty requires maintenance, plan cost is materially less than warranty loss exposure (typical residential mfr warranty value: $4,000–$8,000)
- Always frame numbers as industry-typical ranges, not guaranteed
- Show the math: `plan_cost × payback_years < cost_of_one_averted_event`

### 5. Proposal-Ready Output

Format as a customer-facing document:
- Company header from config (name, license #, certifications, contact, review link)
- Customer / property summary
- Roof health assessment
- Task matrix (as a readable table, quarterly or seasonal)
- Three tiers side-by-side comparison table with scope and price
- ROI section with worked math
- Terms (billing cadence from `billing.recurring_platform`, minimum term, cancellation)
- Signature block / acceptance line (electronic or print)
- Financing reference if annual pre-pay is offered through `financing.partners[]`

**Output requirements:**
- Customer-friendly tone — avoid jargon unless commercial/facilities audience
- Pricing math shown, not asserted ("1 visit at $X × 4 quarterly = $Y annual")
- Seasonal task calendar as a simple 4-column table (Spring / Summer / Fall / Winter)
- Tier comparison as a 3-column side-by-side table with annual + monthly + annual-prepay columns
- If config lacks rates, prompt user once for target price range; otherwise use sensible market defaults and flag them as assumptions
- Saved to `outputs/maintenance-plans/{customer-last-name}-{property-slug}-plan.md` if user confirms
- Paste-ready email wrapper (use `email-drafter` skill voice) if user wants to send directly

**Efficiency notes:**
- Infer commercial vs. residential from customer type input
- Default to Asphalt Shingle task matrix if material not specified; flag the assumption
- Pull tier scope from config if defined; build from scratch only if `config.maintenance.tiers` is empty
- Cross-reference: on post-storm use cases, route to `predictive-lead-scorer` to batch-identify plan candidates; route to `roof-inspection-report` for the inaugural inspection that anchors the plan

## Example Output (30-sq asphalt, 18 yr, Frisco TX 75070 hail zone)

```
ACME ROOFING, LLC                                         License #TX-RC-0481234
GAF MasterElite | HAAG Certified | OC Platinum Preferred  469-555-0140

ROOF MAINTENANCE PROPOSAL
Property:  1248 Maple Ridge Dr, Frisco TX 75070
Customer:  Janelle Doe
Date:      2026-04-27
Roof:      30 sq, 6:12, 2-story, GAF Timberline HDZ, 18 yrs old, no active warranty

ROOF HEALTH ASSESSMENT
- Current condition: 3/5 (Fair) — granule loss visible, sealant intact, no active leaks
- Risk factors: hail zone (75070, last event 4/18 1.5"), summer UV exposure, 18-yr age
  approaching the natural-replacement window for asphalt
- Estimated REUL without maintenance: 3–5 yrs to functional failure
- Estimated REUL on Silver/Gold plan: 6–9 yrs (industry-typical +3–7 yr lift on asphalt)
- Mfr warranty: expired at 15 yr — plan does not preserve mfr warranty here, but does
  preserve the 6-yr Acme workmanship from your 2008 install (extended +2 yr on Silver,
  +5 yr on Gold)

SEASONAL TASK MATRIX (Asphalt + hail-prone, North Texas)

| Season  | Tasks                                                                 |
|---------|-----------------------------------------------------------------------|
| Spring  | Debris clear, granule-wear check, flashing sealant verify, gutter clean,
|         | post-hail drone scan if NOAA event in 75070 |
| Summer  | Attic-temp / ventilation verify, UV-wear inspection, skylight seal check |
| Fall    | Pre-winter flashing, ridge-cap integrity, gutter + guard check |
| Winter  | Post-event drone inspection within 72 hr of any 1+ inch hail or 60+ mph |
|         | wind event |

TIERED SERVICE OPTIONS

| Tier             | 🥉 Bronze            | 🥈 Silver                       | 🥇 Gold                         |
|------------------|----------------------|----------------------------------|----------------------------------|
| Inspections/yr   | 1 (annual)          | 2 (spring + fall)                | 4 (quarterly)                    |
| Gutter clean     | 1×                  | 2×                               | 4×                               |
| Minor repairs    | None — billed sep.  | Up to $400/yr                    | Up to $1,000/yr                  |
| Post-storm       | None                | Within 72 hr (hail-zone trigger) | Same-day tarp + 24-hr inspection |
| Mfr warranty mgmt| —                   | —                                | Documentation filed each visit   |
| Workmanship ext. | —                   | +2 yr while on plan              | +5 yr while on plan              |
| **Annual price** | **$285**            | **$910**                         | **$2,090**                       |
| **Monthly**      | **$25**             | **$75**                          | **$175**                         |
| **Annual prepay**| **$262 (8% off)**   | **$835 (8% off)**                | **$1,925 (8% off)**              |
| Min term         | 12 months           | 12 months                        | 12 months                        |

PRICING MATH (shown, not asserted)
Bronze annual = max(inspection_rate_flat $285, 1.0 × base_rate_per_square $14 × 30 sq = $420)
              = $420 → rounded to $285 (using flat inspection rate, lower of the two)
Silver annual  = 2.5 × $285 + $400 / 2 = $712.50 + $200 = $912.50 → $910
Gold annual    = 1.75 × $910 + $1,000 / 2 = $1,592.50 + $500 = $2,092.50 → $2,090

ROI JUSTIFICATION (industry-typical)
- Silver plan cost: $910/yr
- Cost of one averted emergency leak: $500–$2,500 (mid: $1,500)
- Breakeven: One averted leak pays back ~1.6 yrs of Silver plan
- Cost of premature replacement (lost 4 yrs of useful life on a $14k re-roof): ~$5,600
- Even crediting Silver only with the +3 yr lower-bound REUL lift: $910 × 3 = $2,730
  vs $5,600 in averted lost-life value → 2.05× return at the lower bound

WHAT'S NOT INCLUDED (any tier)
- Full re-roof, structural decking work, or insurance-claim-driven scope
- Hail/wind insurance claims — separate Acme service line (route to insurance-supplement-writer)
- Solar detach-and-reset

TERMS
- Billing: monthly via Stripe, annual prepay via QBO invoice, or per-visit at tier rate
- Min term: 12 months; cancel after 12 with 30-day notice
- Auto-renews annually at tier rate; price-lock for annual prepay
- Storm-zone trigger: 75070 is in service_area.hail_zones[] — Silver/Gold post-storm
  inspection auto-fires within 72 hr of any NOAA 1+ inch hail or 60+ mph wind event

RECOMMENDED FOR THIS ROOF: 🥈 Silver
At 18 yrs in a hail zone, the bi-annual cadence + 72-hr post-storm scan is the
right protective layer for the 3–9 yr remaining-life window without overspending
on quarterly visits the roof doesn't need yet.

ACCEPTANCE
[ ] Bronze   [✓] Silver   [ ] Gold        Term: 12 months / 24 / 36 (price-lock)
Signature: _______________________________  Date: ____________

— Acme Roofing, LLC | 469-555-0140 | TX-RC-0481234
  GAF MasterElite | HAAG Certified | OC Platinum Preferred
```

**Assumptions footer for this run**
- `maintenance.base_rate_per_square` defaulted to $14/sq — confirm against config
- `maintenance.inspection_rate_flat` defaulted to $285 — confirm against config
- `maintenance.repair_cap_standard` defaulted to $400, `.repair_cap_premium` to $1,000
- `warranty.workmanship_years_on_plan_extension` defaulted to +2 (Silver) and +5 (Gold)
- `service_area.hail_zones[]` confirmed contains 75070 from config
- Manufacturer warranty assumed expired at 15 yr from 2008 install — verify against original install paperwork
