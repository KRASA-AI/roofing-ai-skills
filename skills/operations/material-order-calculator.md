---
name: "Material Order Calculator"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/order"
version: 1.3
last_eval_score: 8.8
inspiration: "v1.3 (2026-06-08) eval improvement cycle targeting output_quality + industry_fit — fixed an internal inconsistency in the worked Example Output (was labeled 'simple gable' while applying a 15% cut-up waste factor with 4 valleys/2 dormers; now correctly labeled cut-up hip-and-valley), corrected the ridge-cap coverage constant to be product-dependent (~31 lf/bundle cut 3-tab vs ~20 lf/bundle pre-formed TimberTex/TimberCrest/Z-Ridge), and added a Coverage-Constants Quick-Reference table plus an explicit waste-factor-must-match-geometry guard. v1.2 rewritten 2026-04-25 from eval improvement cycle — named config-field binding (suppliers.preferred[], suppliers.account_numbers[], material_brands.*, dump.bundles_per_yard), paste-ready supplier quote-request block (ABC Supply / Beacon Building Products / SRS Distribution formats), tariff-aware price column, and an example 28-square architectural shingle order. v1.1 enhanced with material categories, waste factor tables, supplier-ready formatting, bundle/unit conversion."
---

# 📦 Material Order Calculator

## Purpose

Calculate exact material quantities from roof measurements — broken down by material category with appropriate waste factors, bundle/unit conversions, and supplier-ready formatting — so you can place an accurate order without over- or under-buying. Output formatted to paste directly into the supplier's quote-request portal.

## When to Use

- After completing measurements (field or aerial) and before placing a supply order
- When preparing a material list for a bid or estimate
- To double-check a supplier quote against your own takeoff
- When switching material brands/types and need to recalculate quantities
- During a tariff or price-volatility window when getting two supplier quotes is the standard procedure

## Required Input

Provide the following:

1. **Roof measurements** — Total squares (or square footage), pitch, and roof geometry notes (number of hips, valleys, ridges, and their linear footage if known)
2. **Material system** — Shingle type/brand (e.g., GAF Timberline HDZ, OC Duration, CertainTeed Landmark), underlayment type, and any upgrades (ice & water shield, synthetic underlayment, ridge vent)
3. **Job scope** — Tear-off (single or multi-layer), overlay, or new construction. Number of stories and access notes
4. **Accessories needed** — Pipe boots (count and sizes), drip edge (eave/rake linear feet), step flashing, chimney flashing kit, skylight flashing, vents (type and count)
5. **Supplier preference** (optional) — If ordering from a specific supplier in `config.suppliers.preferred[]`, the output format adjusts to that supplier's quote portal layout

## Instructions

You are a roofing operations manager's AI assistant. Your job is to produce an accurate, supplier-ready material order from roof measurements.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.phone`, `company.delivery_address`, `company.account_manager_name` — for the supplier quote request
  - `suppliers.preferred[]` — ordered list of preferred suppliers with `name`, `account_number`, `portal_url`, `quote_format` (one of: `abc_supply`, `beacon_pro`, `srs_distribution`, `generic`)
  - `suppliers.delivery_window_default` (e.g., "next-day before 7 AM curbside")
  - `material_brands.shingle_default` (e.g., "GAF Timberline HDZ"), `material_brands.shingle_alternates[]`
  - `material_brands.underlayment_default` (e.g., "GAF Tiger Paw"), `material_brands.iws_default` (e.g., "GAF StormGuard")
  - `material_brands.ridge_cap_default`, `material_brands.starter_default`, `material_brands.drip_edge_finish` (color/finish)
  - `material_brands.nail_default` (e.g., "1-1/4\" coil roofing nails, electro-galvanized")
  - `dump.bundles_per_yard` (default 4 for tear-off debris) and `dump.dumpster_sizes_yd[]` for dump-fee planning when tear-off is in scope
  - `pricing_mode` — one of: `quote_only` (no prices, request quote), `last_supplier_pricing` (pull last quoted prices from `suppliers.preferred[].last_pricing`), `tariff_aware` (annotate items affected by `market_conditions.active_tariffs[]` from the tariff-price-adjuster skill)
  - `voice` — communication tone for the cover note
  - If a named field is missing, use a sensible default and flag it in the output's "Assumptions" footer

- Reference `knowledge-base/terminology/` for correct industry terms

**Calculation rules:**

1. **Shingles** — Convert squares to bundles (standard: 3 bundles/square for architectural, 4 for designer/luxury). Apply waste factor based on roof complexity:
   - Simple gable roof: 10% waste
   - Cut-up roof (multiple hips/valleys): 15% waste
   - Complex/steep (>8:12 pitch or many dormers): 18–20% waste
   - Always round up to whole bundles

2. **Starter strip** — Calculate from total eave + rake linear footage. Standard coverage: ~105 lin ft per bundle (varies by brand — note actual when `material_brands.starter_default` differs).

3. **Ridge cap** — Calculate from total ridge + hip linear footage. Coverage depends on the product: ~31 lin ft per bundle for cut-from-field 3-tab cap, but ~20 lin ft per bundle for premium pre-formed cap (GAF TimberTex / TimberCrest, OC DecoRidge, Z-Ridge). Match the constant to `material_brands.ridge_cap_default` and state which you used.

4. **Underlayment** — Calculate from total square footage + 4" overlap per course. Synthetic felt rolls typically cover 10 squares; #15 felt covers ~4 squares per roll. Round up.

5. **Ice & water shield** — If applicable: 3 feet up from eave edge (or to code requirement, often 24" inside warm wall), plus all valleys. Calculate linear footage × 3 ft width, convert to rolls (standard: 65 lin ft per roll @ 36" wide = ~195 sq ft).

6. **Drip edge** — Total eave + rake linear footage. Standard: 10 ft per piece. Round up. Color/finish from `material_brands.drip_edge_finish`.

7. **Ventilation** — Ridge vent: linear footage of ridge to be vented. Box vents: calculate from required NFA (net free area) based on attic square footage (1:150 unbalanced or 1:300 balanced per IRC).

8. **Nails** — Estimate 4–6 nails per shingle, ~80 shingles per square. High-wind zones require 6-nail pattern. Calculate coils or boxes needed.

9. **Accessories** — Pipe boots by count/size, step flashing by linear footage, chimney/skylight kits by count.

10. **Tear-off debris** — If scope is tear-off, compute estimated debris yards = (squares × `dump.bundles_per_yard`) and recommend dumpster size from `dump.dumpster_sizes_yd[]`.

**Coverage-constants quick reference** (use these unless `material_brands.*` specifies a product with a different published coverage — then use the product's actual number and note it):

| Material | Conversion / Coverage | Round to |
|----------|-----------------------|----------|
| Architectural shingle | 3 bundles / square | whole bundle |
| Designer / luxury shingle | 4 bundles / square | whole bundle |
| Starter strip | ~105 lin ft / bundle | whole bundle |
| Ridge cap — 3-tab cut | ~31 lin ft / bundle | whole bundle |
| Ridge cap — pre-formed (TimberTex / TimberCrest / Z-Ridge) | ~20 lin ft / bundle | whole bundle |
| Synthetic underlayment | ~10 squares / roll | whole roll |
| #15 felt | ~4 squares / roll | whole roll |
| Ice & water shield (36" wide) | ~195 sq ft (~65 lin ft) / roll | whole roll |
| Drip edge | 10 ft / piece | whole piece |
| Ridge vent (rolled/strip) | ~7 ft / piece (varies) | whole piece |
| Coil roofing nails | ~80 shingles/sq × nails-per-shingle ÷ ~7,200 nails/box | whole box |

Waste factors: simple gable 10% · cut-up (multiple hips/valleys) 15% · complex/steep (>8:12 or many dormers) 18–20%. **The waste factor stated in the Assumptions footer must match the roof geometry described** — never apply a cut-up waste factor to a roof you've labeled simple gable (and vice versa).

**Process:**

1. Review all measurements and identify any gaps (ask only for critical missing items like unknown pitch or total squares)
2. Calculate each material category using the rules above
3. Apply waste factors and round up to whole purchase units
4. Cross-check totals against the roof size for sanity (e.g., a 25-square roof shouldn't need 100 bundles)
5. Format as a supplier-ready order list using `suppliers.preferred[0].quote_format`
6. If `pricing_mode` is `tariff_aware`, mark each affected line with the tariff/manufacturer increase note from `market_conditions`

**Output structure:**

### 1. Order Summary Table

| # | Material | Brand / Spec | Calculation Basis | Qty Needed | Waste % | Order Qty | Unit | Notes |
|---|----------|--------------|-------------------|-----------:|--------:|----------:|------|-------|
| 1 | Shingles | {material_brands.shingle_default} | 28 sq × 3 bundles | 84 | 15% | 97 | bundles | — |

### 2. Paste-Ready Supplier Quote Request

The exact block format is selected by `suppliers.preferred[0].quote_format`:

**`abc_supply` format:**
```
ABC SUPPLY — QUOTE REQUEST
Account: {suppliers.preferred[0].account_number}
PO/Reference: {job_address_slug}-{YYYY-MM-DD}
Delivery: {company.delivery_address}
Window: {suppliers.delivery_window_default}
AM: {company.account_manager_name} / {company.phone}

LINE ITEMS
{table of items with brand SKU, qty, unit}

DELIVERY NOTES
- Curbside drop OK; no rooftop loading
- Two-day pickup if quantity adjusts
```

**`beacon_pro` format:**
```
BEACON PRO+ ORDER REQUEST
Account: {account_number}    Job: {job_slug}
Ship-to: {delivery_address}
Requested: {delivery_window}

{line items in Beacon Pro+ SKU + qty format}
```

**`srs_distribution` format:**
```
SRS DISTRIBUTION — RFQ
Customer #: {account_number}
Job site: {address}
Delivery: {window}
Contact: {account_manager_name} — {phone}

{line items}
```

**`generic` fallback:** plain table the user can paste into any supplier portal.

### 3. Notes & Assumptions Footer
- Brand assumptions made (e.g., "shingle defaulted to material_brands.shingle_default; specify alternate if needed")
- Waste factor applied and reason
- Any tariff/manufacturer-increase flags from `market_conditions`
- Dumpster recommendation from tear-off debris calc (if applicable)
- Sanity-check pass/fail vs roof size

**Output requirements:**

- Order summary as a table grouped by material category (shingles, underlayment, accessories, fasteners, debris)
- Paste-ready supplier-quote block formatted for `suppliers.preferred[0]` (or all preferred suppliers if user wants two-quote comparison)
- Estimated material cost line if `pricing_mode` is `last_supplier_pricing` (pulled from `suppliers.preferred[].last_pricing`)
- Tariff-aware annotations on affected lines if `pricing_mode` is `tariff_aware`
- Saved to `outputs/material-orders/{job-address-slug}-{YYYY-MM-DD}-order.md` if user confirms

**Efficiency notes:**

- Single clarifying question max — usually pitch when only squares are provided
- If two suppliers are in `suppliers.preferred[]`, produce both blocks so the user can drop simultaneous RFQs
- Cross-reference sibling skills: `estimate-builder` (the takeoff drives the cost basis), `tariff-price-adjuster` (when pricing_mode = tariff_aware, annotations come from this skill's market_conditions block), `crew-schedule-optimizer` (delivery window must align with crew start)

## Example Output (28-sq architectural tear-off, cut-up hip-and-valley roof, ABC Supply)

```
ORDER SUMMARY
# Material            Brand / Spec               Basis             Qty   Waste  Order   Unit       Notes
1 Shingles            GAF Timberline HDZ (Charcoal) 28 sq × 3      84    15%    97      bundles    Tariff-aware: +4% vs Q1 2026
2 Starter strip       GAF Pro-Start              184 lf eave+rake  184   round  2       bundles    @ 105 lf/bundle
3 Ridge cap           GAF TimberTex (pre-formed) 42 lf ridge+hip   42    round  3       bundles    @ ~20 lf/bundle (pre-formed)
4 Synthetic underlay. GAF Tiger Paw              28 sq             28    10%    4       rolls      @ 10 sq/roll
5 Ice & water shield  GAF StormGuard             36 lf eave + 24 lf valley × 3 ft  180 sf  10%   2  rolls   @ 195 sf/roll
6 Drip edge           Aluminum, brown, 10 ft     184 lf            184   round  19      pieces     —
7 Ridge vent          GAF Cobra Snow Country     42 lf             42    round  6        pieces     @ 7 ft each
8 Pipe boots          Lifetime, 1.5" / 3"        2 × 1.5", 1 × 3"  3     —      3        each       —
9 Nails               1-1/4" EG coil roofing     97 bundles × 80 × 5   ~38,800   round  10  boxes  6-nail pattern (high-wind)
10 Tear-off debris    Single-layer architectural 28 sq × 4 yd      ~12  yd      1       30-yd      Recommend 30-yd dumpster

PASTE-READY: ABC SUPPLY QUOTE REQUEST
ABC SUPPLY — QUOTE REQUEST
Account: 18472-ACME
PO/Reference: 1248-maple-ridge-dr-2026-04-25
Delivery: 1248 Maple Ridge Dr, Frisco, TX 75070
Window: next-day before 7 AM curbside
AM: Marcus Patel / 469-555-0142

LINE ITEMS
- 97 ea  GAF Timberline HDZ Charcoal (3 bdl/sq)
- 2 ea   GAF Pro-Start Starter Strip
- 3 ea   GAF TimberTex Ridge Cap (matched)
- 4 ea   GAF Tiger Paw Synthetic Underlayment
- 2 ea   GAF StormGuard Ice & Water Shield
- 19 ea  Aluminum drip edge, brown, 10' (D-style)
- 6 ea   GAF Cobra Snow Country ridge vent
- 3 ea   Lifetime pipe boots (2× 1.5", 1× 3")
- 10 box 1-1/4" electro-galvanized coil roofing nails

DELIVERY NOTES
- Curbside drop OK; no rooftop loading
- Two-day pickup if quantity adjusts

NOTES & ASSUMPTIONS
- Shingle: defaulted to material_brands.shingle_default = GAF Timberline HDZ
- Waste: 15% (cut-up roof — 4 valleys, 2 dormers)
- Tariff flag: shingle line +4% vs Q1 2026 — confirm with supplier on quote return
- Tear-off debris: ~12 yd estimated → 30-yd dumpster recommended
- Sanity check: 28 sq × 3 bundles = 84 → 97 with waste — within expected range
```

(Run with your own measurements + config to replace these illustrative values.)
