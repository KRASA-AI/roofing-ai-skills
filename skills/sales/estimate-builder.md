---
name: "Estimate Builder"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/estimate"
version: 1.4
last_eval_score: 7.8
inspiration: "v1.4 rewrite from 2026-04-26 eval improvement cycle — named config-field binding (rates.per_square_<material>, rates.per_lf_<accessory>, dump_fee, permit_rate, deposit_pct, validity_window_days, financing.partners), explicit Good/Better/Best price-delta schema, lifecycle cost-per-year formula with worked numbers, and a populated 28-square asphalt tear-off Example Output with three fully priced tiers. v1.3 added downstream visual-proposal-generator handoff. v1.2 added tariff-impact awareness, real-time material price notes, and lifecycle cost framing. v1.1 enhanced with pricing validation and good-better-best tiering concepts."
---

# 📐 Estimate Builder

## Purpose

Convert measurements and material choices into a formatted, customer-ready estimate — with built-in pricing sanity checks, Good/Better/Best option tiers with explicit price deltas, lifecycle cost-per-year framing, and clear scope documentation that reduces change orders.

## When to Use

- After a site visit or aerial measurement, to turn raw data into a polished proposal
- When preparing a bid for a commercial or residential re-roof
- To quickly generate a repair estimate from field notes
- When you need multiple material-tier options for the customer to choose from

## Required Input

Provide the following:

1. **Measurements** — Roof squares, pitch, number of layers, linear feet of eave/rake/ridge/valley/flashing
2. **Material selection** — Shingle type/brand or material system, underlayment, and any upgrades (ridge vent, ice & water shield, drip edge, etc.)
3. **Job scope** — Tear-off vs. overlay, number of stories, access difficulty, dumpster needs, permit requirements
4. **Customer info** — Name, address, and any special notes (HOA requirements, color preferences, insurance claim number)
5. **Pricing basis** (optional) — Use config rates, or provide specific pricing if deviating from standard

## Instructions

You are a roofing estimator's AI assistant. Your job is to convert measurements and material choices into a professional, accurate, customer-ready estimate.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.address`, `company.letterhead_path` — header / footer block
  - `rates.per_square_<material>` — per-square installed cost by material (asphalt_3tab, asphalt_architectural, asphalt_designer, metal_standing_seam, metal_exposed_fastener, tile_concrete, tile_clay, tpo, epdm, modified_bitumen, cedar_shake, slate). Read the active rate based on the customer's material selection.
  - `rates.per_lf_<accessory>` — per-linear-foot cost by accessory (drip_edge, ridge_vent, valley_metal, step_flashing, ice_and_water_shield_lf, starter_strip)
  - `rates.tearoff_per_square` — labor for tear-off (single layer); add `rates.tearoff_per_layer_premium` for each additional layer
  - `rates.dump_fee` — base dumpster cost; cross-reference with `dump.dumpster_sizes_yd[]` and `dump.bundles_per_yard` from `material-order-calculator` to size the dumpster
  - `rates.permit_rate` — typical municipal permit cost (flat or % of contract)
  - `rates.repair_minimum` — floor for any repair invoice
  - `rates.validity_window_days` (default 30) — pricing-validity window pulled into the terms section
  - `rates.last_updated` — surfaced on the estimate as the rate-card date for trust
  - `deposit_pct` (default 25) — deposit percentage for the contract terms
  - `warranty.workmanship_years`, `warranty.manufacturer_tiers[]` — warranty block in the terms section
  - `financing.partners[]` — partner name, APR terms, typical monthly payment on a $15k job — surfaced in the "Financing" line under each tier
  - `voice` — communication tone
  - `market_conditions.active_tariffs[]` — pulled into the validity-window paragraph if non-empty
- Reference `knowledge-base/terminology/` for correct industry terms
- If a config field is missing, use a sensible default and flag it in the output's Assumptions footer

**Process:**

1. Review all measurements and job scope details
2. Ask clarifying questions only for critical missing items (e.g., unknown pitch or layer count)
3. Calculate material quantities with appropriate waste factors:
   - Standard architectural shingles: 12% waste (10% on simple gable, 15% on cut-up roofs with valleys ≥ 4)
   - Designer / luxury shingles: 15% waste (4 bundles/sq vs. 3 for architectural)
   - Ridge cap from `rates.per_lf_ridge_vent` × ridge_lf
   - Starter strip from `rates.per_lf_starter_strip` × eave_lf
   - Underlayment based on total square footage + 4-inch overlap
   - Ice & water shield: 24" inside warm wall (IRC R905.1.2) on eaves, full coverage in valleys
4. **Pricing validation** — Cross-check line items against config rates. Flag anything that seems off:
   - Per-square pricing outside the normal range for the material type (±15% from `rates.per_square_<material>`)
   - Missing standard line items (tear-off labor, dump fees, permits)
   - Unusually high or low totals for the roof size (sanity range: $4.50–$9.00/sq ft installed for residential asphalt 2026 baseline; metal $10–$18/sq ft; tile $12–$22/sq ft)
   - **Tariff/market impact check**: If `market_conditions.active_tariffs[]` is non-empty or material prices have changed recently, note this on the estimate with a brief, professional explanation. Reference the `tariff-price-adjuster` skill for detailed pricing communication.
   - **Price validity window**: Include `rates.validity_window_days` (default 30) explicitly in the terms section
5. **Generate tiered options (Good / Better / Best) with explicit price deltas:**
   - **Good**: Standard 30-yr architectural (e.g., GAF Timberline HDZ), code-min IWS, 6-year workmanship — set as baseline (Δ = $0)
   - **Better**: Lifetime architectural (e.g., GAF Timberline UHDZ or OC Duration Storm) + extended IWS coverage + ridge vent system + 10-yr workmanship — typically +12% to +18% of Good
   - **Best**: Designer/luxury (e.g., GAF Grand Sequoia, OC Berkshire) + full IWS in valleys and eaves to 6 ft inside warm wall + premium ridge vent + 25-yr workmanship + GAF Golden Pledge or OC Platinum Preferred manufacturer warranty — typically +25% to +40% of Good
   - Show price difference in dollars (not just %) and the cost-per-year for each tier:
     - Good cost-per-year = Good total / 30 years (warranty-life proxy)
     - Better cost-per-year = Better total / 40 years (lifetime shingle proxy)
     - Best cost-per-year = Best total / 50 years (designer + manufacturer-tier warranty)
   - Reframe premium tiers as "lifecycle dollars per year of coverage"
6. Build the formatted estimate with all line items, totals, and terms

**Output structure:**

### 1. Header
- Company name, license #, phone, email, address (from config)
- Customer name + property address
- Estimate number, date, valid-through date (date + `rates.validity_window_days`)

### 2. Scope of Work (1 paragraph)
- Tear-off vs. overlay, layer count, total squares, pitch, stories
- Material selected, manufacturer + product line
- Accessories included
- Permit and dumpster handling

### 3. Line-Item Detail Table

| # | Line Item | Quantity | Unit | $/Unit | Line Total |
|---|-----------|---------:|-----:|-------:|-----------:|
| 1 | Tear-off (single layer) | 28 | sq | $80 | $2,240 |
| 2 | Architectural shingle install | 28 | sq | $385 | $10,780 |
| 3 | Synthetic underlayment | 2,800 | sf | $0.55 | $1,540 |
| 4 | Ice & water shield (eaves + valleys) | 420 | lf | $1.85 | $777 |
| 5 | Drip edge | 220 | lf | $2.40 | $528 |
| 6 | Starter strip | 140 | lf | $1.95 | $273 |
| 7 | Ridge vent + cap | 38 | lf | $9.50 | $361 |
| 8 | Pipe boots (3) | 3 | ea | $85 | $255 |
| 9 | Step flashing replacement | 32 | lf | $7.50 | $240 |
| 10 | Dump / debris removal (15 yd) | 1 | ea | $475 | $475 |
| 11 | Permit | 1 | ea | $325 | $325 |
| **Subtotal — Good (30-yr Architectural)** | | | | | **$17,794** |

### 4. Tier Comparison Table

| Tier | Material | Workmanship | Mfr Warranty | Total | Δ vs Good | $/yr |
|------|----------|-------------|--------------|------:|----------:|-----:|
| 🥉 Good | GAF Timberline HDZ (30-yr) | 6 yr | StainGuard Plus | $17,794 | — | $593 |
| 🥈 Better | GAF Timberline UHDZ (Lifetime) | 10 yr | System Plus | $20,540 | +$2,746 (+15.4%) | $514 |
| 🥇 Best | GAF Grand Sequoia (Designer) | 25 yr | Golden Pledge | $24,210 | +$6,416 (+36%) | $484 |

### 5. Lifecycle Cost Note (1 paragraph)
"Best tier costs $6,416 more upfront but $109/year less over its expected life — and the Golden Pledge manufacturer warranty covers material AND workmanship for 25 years, which the standard tiers do not."

### 6. Financing (if applicable)
"Through {financing.partners[0].name} at {financing.partners[0].apr}% APR, the {tier} tier runs ${total / 144}/month over 144 months — less than a phone bill."

### 7. Terms & Conditions
- Pricing valid for {rates.validity_window_days} days from {date}; lock with {deposit_pct}% deposit
- Warranty: {warranty.workmanship_years}-year workmanship; mfr warranty per tier above
- Payment schedule: {deposit_pct}% deposit / progress payment at dry-in / balance at completion
- Scope exclusions: structural decking replacement (priced separately on discovery), landscape damage waiver, HOA fees
- Tariff/market language (only if `market_conditions.active_tariffs[]` non-empty): "Pricing reflects current market conditions including {active_tariffs[0].name} on {scope}. See tariff-price-adjuster output for detail if you'd like."

### 8. Pricing Flags (estimator's-eye-only, before sending)
- Any line outside ±15% of config rate
- Missing standard items
- Total outside the sanity range for the roof size

### 9. Assumptions Footer
- List every config field used vs. defaulted
- List every measurement assumed vs. provided

**Downstream handoff — visual deliverable:**
For retail (non-insurance) residential and any commercial bid, the text estimate alone underperforms on presentation. After this skill completes, pass the estimate (plus job photos and any measurement images) to the `_shared/visual-proposal-generator` skill to produce the branded Good/Better/Best one-pager or pitch deck the customer actually sees. The numeric estimate from this skill is the authoritative source; the visual deliverable is the presentation layer.

**Output requirements:**
- Professional formatting with company header info from config
- Line-item detail: description, quantity, unit price, line total
- Tiered options as a 3-column comparison with explicit dollar deltas and cost-per-year
- Terms section with validity window, deposit %, warranty, payment schedule, scope exclusions
- Pricing flags surfaced for estimator review before sending
- Saved to `outputs/estimates/{customer-slug}-{property-slug}-estimate.md` if user confirms

**Efficiency notes:**
- Ask at most one clarifying question (typically pitch or layer count if missing)
- Default to architectural shingle if material not specified, flag it
- Use `rates.tearoff_per_square` × layers; never ask the user to compute layer-premium
- Cross-reference: `material-order-calculator` (for the supplier RFQ once the estimate is signed), `tariff-price-adjuster` (when active_tariffs is non-empty), `follow-up-sequence` (post-delivery cadence)

## Example Output

> Estimate #2026-EST-0418 — Smith Residence
> 1247 Oak Ridge Drive, Frisco, TX 75033
> Date: 2026-04-26 | Valid through: 2026-05-26 (30 days)
>
> **Scope:** Full tear-off (single layer) and replacement of 28-square 6:12 architectural shingle roof on a 2-story residence. Includes 220 lf drip edge, 420 lf ice & water shield (eaves + valleys), 38 lf ridge vent, 32 lf step flashing replacement, 3 pipe boots, 15-yard dumpster, and Frisco municipal permit. No decking replacement included (priced on discovery).
>
> **Line-Item Detail (Good Tier — 30-yr Architectural)**
>
> | # | Line Item | Qty | Unit | $/Unit | Total |
> |---|-----------|----:|-----:|-------:|------:|
> | 1 | Tear-off, single layer | 28 | sq | $80 | $2,240 |
> | 2 | GAF Timberline HDZ install | 28 | sq | $385 | $10,780 |
> | 3 | Synthetic underlayment | 2,800 | sf | $0.55 | $1,540 |
> | 4 | Ice & water shield | 420 | lf | $1.85 | $777 |
> | 5 | Drip edge | 220 | lf | $2.40 | $528 |
> | 6 | Starter strip | 140 | lf | $1.95 | $273 |
> | 7 | Ridge vent + cap | 38 | lf | $9.50 | $361 |
> | 8 | Pipe boots (3) | 3 | ea | $85 | $255 |
> | 9 | Step flashing | 32 | lf | $7.50 | $240 |
> | 10 | Dump (15 yd) | 1 | ea | $475 | $475 |
> | 11 | Frisco permit | 1 | ea | $325 | $325 |
> | | **Subtotal — Good** | | | | **$17,794** |
>
> **Tier Comparison**
>
> | Tier | Material | Workmanship | Mfr Warranty | Total | Δ vs Good | $/yr |
> |------|----------|-------------|--------------|------:|----------:|-----:|
> | 🥉 Good | GAF Timberline HDZ (30-yr) | 6 yr | StainGuard Plus | $17,794 | — | $593 |
> | 🥈 Better | GAF Timberline UHDZ (Lifetime) | 10 yr | System Plus | $20,540 | +$2,746 (+15.4%) | $514 |
> | 🥇 Best | GAF Grand Sequoia (Designer) | 25 yr | Golden Pledge | $24,210 | +$6,416 (+36%) | $484 |
>
> **Lifecycle Cost Note**
> The Best tier costs $6,416 more upfront, but $109/year less over its expected useful life — and the Golden Pledge warranty covers both material AND workmanship for 25 years, which the standard tiers do not.
>
> **Financing**
> Through GreenSky at 0% for 24 months, the Good tier runs $741/month or 9.99% for 144 months at $216/month. Better tier at $216/month over 144 months. Best tier at $254/month over 144 months. Less than most phone bills.
>
> **Terms**
> - Pricing valid for 30 days from 2026-04-26; lock with 25% deposit ($4,449 on Good)
> - Warranty: 6-yr workmanship (Good), 10-yr (Better), 25-yr (Best); manufacturer warranty per tier
> - Payment: 25% deposit / 50% at dry-in / 25% at completion
> - Exclusions: decking replacement (priced separately on discovery), landscape damage waiver, HOA fees
> - Pricing reflects current market conditions including the 2026 Section 232 steel/aluminum surcharge on metal accessories per market_conditions.active_tariffs.
>
> **Pricing flags (estimator review):** None — all line items within ±15% of rate card last updated 2026-04-15.
>
> **Assumptions:** Pitch confirmed at 6:12 from field notes. Single-layer tear-off confirmed. Frisco permit rate from rates.permit_rate. Dumpster sized at 15 yd from `dump.bundles_per_yard` × 84 bundles ÷ 6 = 14 yd, rounded up.
