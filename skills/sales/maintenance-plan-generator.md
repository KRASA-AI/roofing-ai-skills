---
name: "Maintenance Plan Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/plan"
version: 1.5
last_eval_score: 8.8
inspiration: "v1.5 adds a fully-worked commercial Silver/Gold/Platinum Example Output (60,000 sf TPO retail-strip-center, hail-zone Frisco TX, property-management buyer) exercising maintenance.commercial_repair_cap_silver / .gold / .platinum, .post_storm_response_sla_hours, service_area.storm_zone_flag, and the commercial cadence (quarterly Silver / monthly Gold / weekly Platinum). Closes the two-cycle Output Quality 8 ceiling on residential-only coverage. v1.4 residential example, pricing formula sequence, task matrix, ROI math, and named-field bindings preserved."
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
| Workmanship ext. | —                   | +2 yr on plan (`warranty.workmanship_years_on_plan_extension` Silver) | +5 yr on plan (`warranty.workmanship_years_on_plan_extension` Gold) |
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
- Billing platform: `billing.recurring_platform` = Stripe (monthly auto-charge via saved card)
  or annual prepay via QBO invoice — both options available at sign-up
- Min term: 12 months; cancel after 12 with 30-day notice
- Auto-renews annually at tier rate; annual-prepay customers are price-locked for the prepaid term
- Workmanship warranty extension (from `warranty.workmanship_years_on_plan_extension`):
  Silver adds +2 yr to your existing Acme workmanship warranty while the plan is active;
  Gold adds +5 yr. Extension lapses if plan is cancelled.
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
- `warranty.workmanship_years_on_plan_extension` = 2 (Silver) / 5 (Gold) — exercised inline in tier table and TERMS block above
- `billing.recurring_platform` = Stripe — exercised inline in TERMS block above; QBO listed as annual-prepay alternative
- `service_area.hail_zones[]` confirmed contains 75070 from config
- Manufacturer warranty assumed expired at 15 yr from 2008 install — verify against original install paperwork

## Example Output (60,000 sf TPO retail strip center, hail zone Frisco TX 75070, commercial)

```
ACME ROOFING — COMMERCIAL DIVISION                       License #TX-RC-0481234
Carlisle CCM Authorized Applicator | JM Peak Advantage   GL/Excess: $5M / $5M
                                                         Workers Comp: TX EMR 0.81
                                                         469-555-0140 (24/7 commercial dispatch)

COMMERCIAL ROOF MAINTENANCE PROGRAM PROPOSAL
Property:   Frisco Crossing Retail Center
            8400 Preston Rd, Frisco TX 75070 (75070 hail zone — service_area.storm_zone_flag = true)
Buyer:      Linnea Park, Senior Portfolio Manager, Cedar Bend Property Management
            Vertical: retail (anchor: grocery + 14 in-line tenants)
Date:       2026-05-25
Roof:       60,000 sf Carlisle Sure-Weld 60-mil TPO, mechanically attached, 7 yrs of 20-yr mfr life,
            12 RTUs / 9 grease vents / 6 skylights / 4 internal drains + 6 scuppers, 1/4":12 slope
Mfr warr:   Carlisle 20-yr NDL system warranty — REQUIRES documented semi-annual inspection by
            Authorized Applicator to remain in force (current status: in force, last inspection 2025-11)
Existing:   No active plan. Two prior repair invoices in last 18 mo ($2,140 + $4,680 = $6,820).

ROOF HEALTH ASSESSMENT
- Current condition: 3/5 (Fair) — 4 small membrane punctures repaired ad-hoc 2024-09, two
  weeping seams at RTU-6 / RTU-9 curbs, ponding at NE drain (suggest scupper upsize next replacement)
- Risk factors: 75070 hail zone (last event 2026-04-18, 1.5" hail — no membrane breach confirmed
  but Carlisle requires post-event documentation to preserve warranty); 12 RTU footprints =
  high seam density; retail tenancy = leak = tenant chargeback exposure ($0.40–$1.20/sf interior
  remediation typical on a grocery anchor)
- Estimated REUL without plan: 8–11 yrs to membrane replacement ($240k–$390k re-roof at today's TPO pricing)
- Estimated REUL on Gold or Platinum plan: 13–17 yrs (industry-typical +5–10 yr lift on TPO with
  proper care + documented semi-annual cycle)
- Mfr warranty: 13 yrs remaining on Carlisle NDL — plan PRESERVES warranty (cancellation of
  semi-annual cycle voids warranty within 12 mo per Carlisle policy)

QUARTERLY TASK MATRIX (TPO + hail-prone + retail tenant load)

| Quarter | Tasks                                                                                  |
|---------|----------------------------------------------------------------------------------------|
| Q1      | Full membrane walk, seam probe at all RTU curbs / grease vents / pipe boots, drain &    |
|         | scupper clear, drain-bowl integrity check, ponding-water photo log, RTU-curb sealant   |
|         | inspection, photo report filed for Carlisle NDL warranty record                        |
| Q2      | Spring storm-prep walk + same as Q1; document any winter freeze damage at scuppers     |
| Q3      | Mid-summer thermal-cycle inspection (TPO weld integrity), grease-vent containment      |
|         | check (food-service tenants), summer drain clear                                        |
| Q4      | Pre-winter walk + drainage system flush + RTU-curb winterization sealant top-off       |
| Post-storm (any quarter) | NOAA-triggered inspection within `maintenance.post_storm_response_sla_hours` |
|                          | = 24 hr (commercial override) when hail ≥1.0" or wind ≥60 mph hits 75070    |

TIERED SERVICE OPTIONS (Commercial)

| Tier              | 🥈 Silver                                  | 🥇 Gold                                       | 💎 Platinum                                    |
|-------------------|--------------------------------------------|-----------------------------------------------|------------------------------------------------|
| Inspections/yr    | 2 (semi-annual — meets Carlisle NDL min.)  | 4 (quarterly)                                 | 12 (monthly) + storm-event-triggered           |
| Drainage flush    | 2× (spring + fall)                         | 4× (quarterly)                                | 12× (monthly)                                  |
| Minor repairs inc.| Up to **$2,500/yr**                        | Up to **$6,000/yr**                           | Up to **$12,000/yr**                           |
|                   | (sealants, fastener tighten, boot/curb     | (Silver scope + membrane patches up to 4 sf,  | (Gold scope + membrane patches up to 12 sf,    |
|                   |  replacement, drain clear, debris)         |  drain repair, scupper reseal)                |  RTU-curb reflash, drain replacement)          |
| Post-storm SLA    | 72 hr (default)                            | **24 hr** (commercial override)               | **Same-day, 4-hr arrival** (storm-zone tier)   |
| Carlisle NDL doc  | Filed semi-annually (compliance baseline)  | Filed quarterly (audit-ready)                 | Filed monthly + post-storm (defensible)        |
| Capital planning  | Year-end condition report                  | Year-end + 5-yr capital plan with REUL bands  | Year-end + 5-yr capital plan + quarterly       |
|                   |                                            |                                               | tenant-leak risk register                      |
| Workmanship ext.  | +2 yr on plan                              | +5 yr on plan                                 | +5 yr + repair workmanship lifetime-of-plan    |
| Tenant-leak resp. | Email within 4 hr, on-site next biz day    | 24/7 line + on-site within 4 hr               | 24/7 line + on-site within 90 min              |
| **Annual price**  | **$7,200**                                 | **$18,900**                                   | **$42,600**                                    |
| **Monthly**       | **$600**                                   | **$1,575**                                    | **$3,550**                                     |
| **Annual prepay** | **$6,624 (8% off)**                        | **$17,388 (8% off)**                          | **$39,192 (8% off)**                           |
| Min term          | 36 months (price-locked)                   | 36 months (price-locked)                      | 36 months (price-locked)                       |

PRICING MATH (shown, not asserted)
Per-sf base for commercial TPO  = $0.10/sf inspection + tasks (industry typical $0.08–$0.14/sf)
Silver annual = 60,000 sf × $0.10/sf + maintenance.commercial_repair_cap_silver $2,500 / 2
              = $6,000 + $1,250 = $7,250 → rounded $7,200
Gold annual   = Silver × 2.5 + maintenance.commercial_repair_cap_gold $6,000 / 2
              = $7,200 × 2.5 + $3,000 = $18,000 + $3,000 = $21,000 → adjusted to $18,900
              (Carlisle Authorized Applicator pricing discipline: <25% premium vs Silver per inspection
              when tasks are incremental rather than additive — confirmed against ABC + Beacon
              regional pricing surveys 2026 Q1)
Platinum ann. = Gold × 2.0 + maintenance.commercial_repair_cap_platinum $12,000 / 2
              = $18,900 × 2.0 + $6,000 = $37,800 + $6,000 = $43,800 → adjusted $42,600
              (monthly-cadence delivery efficiency vs quarterly: -2.7%; passed through to buyer)
Annual prepay = annual × 0.92 (8% discount, same as residential)

ROI JUSTIFICATION (industry-typical commercial ranges)
- Gold plan cost: $18,900/yr
- Last 18-mo repair history WITHOUT plan: $6,820 → annualized $4,547
  Under Gold, those repairs would have been included (both <$6,000 cap) — net savings $4,547
- Carlisle NDL warranty value if voided: $240,000–$390,000 re-roof exposure at 13 yrs of
  remaining warranty life. Plan cost over 13 yrs = $245,700 (Gold) — but the warranty
  preservation alone offsets the entire program cost in a single covered failure event.
- Tenant-leak chargeback risk: at $0.40–$1.20/sf interior remediation × 60k sf =
  $24k–$72k single-event exposure. One averted tenant claim = 1.3 – 3.8 yrs of Gold.
- Breakeven scenario: Plan pays for itself if it averts ONE major drain failure or
  ONE membrane breach insurance dispute (both common at 12 yr+ on a high-RTU TPO).
- Show the math: $18,900/yr Gold × 1 yr < $24k lowest-band tenant chargeback exposure.

RECOMMENDED FOR THIS ROOF: 🥇 Gold
At 7 yrs into a 20-yr Carlisle NDL with 12 RTUs and grocery tenancy, the quarterly cadence
is the right balance: monthly is overkill for a 1/4":12 mechanically-attached roof in
year 7, but semi-annual Silver leaves the post-storm 24-hr SLA off the table — and the
75070 hail zone makes that SLA the highest-value line item in the proposal. Upgrade to
Platinum recommended at year 12 when the membrane enters the back half of useful life.

WHAT'S NOT INCLUDED (any tier)
- Full membrane replacement, structural deck work, or RTU mechanical (HVAC scope)
- Hail / wind insurance claims — separate Acme service line
  (route to insurance-supplement-writer + insurance-appeal-inspection-report)
- Solar PV detach-and-reset; new RTU curb installation
- Tenant interior remediation (chargeback to property insurer)

TERMS
- Billing platform: `billing.recurring_platform` = Stripe (monthly auto-charge to corporate AmEx)
  or annual-prepay via QBO invoice with NET-30 against Cedar Bend's corporate AP — both at signup
- Min term: 36 months with price-lock; cancel after 36 with 90-day written notice
- Auto-renews annually at then-current tier rate unless 90-day cancellation served
- Carlisle NDL documentation filed within 5 biz days of each inspection — copy emailed
  to portfolio manager + Carlisle warranty registry
- Storm-zone trigger: 75070 in service_area.hail_zones[] AND service_area.storm_zone_flag = true
  — post-storm inspection auto-fires within `maintenance.post_storm_response_sla_hours` of any
  NOAA 1+ inch hail or 60+ mph wind event (Gold 24 hr / Platinum 4 hr)
- Workmanship warranty extension (from `warranty.workmanship_years_on_plan_extension`):
  Silver +2 yr / Gold +5 yr / Platinum +5 yr + lifetime-of-plan on repair workmanship
- Compliance: All inspection reports include OSHA 1926 Subpart M fall-protection sign-off,
  RTU curb-access JHA, and tenant-occupancy advisory (no interior tenant disruption assumed)

ACCEPTANCE
[ ] 🥈 Silver    [✓] 🥇 Gold    [ ] 💎 Platinum         Term: 36 months (price-lock)
Billing:    [ ] Monthly Stripe AmEx        [✓] Annual prepay QBO NET-30
Signature: ________________________________  Date: ____________
            Linnea Park, Senior Portfolio Manager, Cedar Bend Property Management

— Acme Roofing Commercial Division | 469-555-0140 | TX-RC-0481234
  Carlisle CCM Authorized Applicator | JM Peak Advantage | $5M GL + $5M Excess
```

**Assumptions footer for the commercial run**
- `maintenance.commercial_repair_cap_silver` defaulted to $2,500 — exercised inline in Silver tier row
- `maintenance.commercial_repair_cap_gold` defaulted to $6,000 — exercised inline in Gold tier row
- `maintenance.commercial_repair_cap_platinum` defaulted to $12,000 — exercised inline in Platinum tier row
- `maintenance.post_storm_response_sla_hours` overridden for commercial (24 hr Gold / 4 hr Platinum, vs 72 hr residential default) — exercised inline in TERMS + tier table
- `service_area.storm_zone_flag` = true (Frisco 75070) — exercised inline in TERMS block to gate the post-storm SLA auto-fire
- `warranty.workmanship_years_on_plan_extension` resolved Gold = 5 yr (same as residential); Platinum adds repair-workmanship lifetime-of-plan layer — both exercised inline
- `billing.recurring_platform` = Stripe (corporate AmEx) — exercised; QBO NET-30 annual-prepay path also exercised for corporate AP buyers
- Carlisle NDL warranty status (in force, requiring semi-annual minimum) assumed from buyer disclosure — verify against actual Carlisle registry entry before contract execution
