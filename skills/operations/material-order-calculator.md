---
name: "Material Order Calculator"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/order"
version: 1.4
last_eval_score: 8.9
inspiration: "v1.4 (2026-07-13) eval improvement cycle targeting output_quality + efficiency — the worked Example Output computed the nail line off BUNDLES (97 bundles × 80 × 5 ≈ 38,800 nails → 10 boxes) when the skill's own Calculation rule 8 and the coverage-constants table both specify nails per SHINGLE with ~80 shingles per SQUARE; the example over-ordered fasteners ~5x and contradicted the spec it sits under. Nail line rebuilt on the correct basis (28 sq × 80 shingles × 6-nail high-wind pattern = 13,440 → 2 boxes @ ~7,200/box). The example also cited an eave-only figure (36 lf) for the ice & water calc that could not be reconciled with the 184 lf eave+rake used elsewhere, so an explicit ROOF GEOMETRY basis block now anchors every line (eave 104 lf / rake 80 lf / ridge+hip 42 lf / valley 24 lf), and the I&W line recomputes from it (3 rolls, not 2). Added a 'Fastest path — minimum viable input' block with [blocking]/[from config]/[inferred] annotations: squares + geometry are the only blocking inputs; brands, supplier format, delivery window, and dump sizing all resolve from config. v1.3 (2026-06-08) eval improvement cycle targeting output_quality + industry_fit — fixed an internal inconsistency in the worked Example Output (was labeled 'simple gable' while applying a 15% cut-up waste factor with 4 valleys/2 dormers; now correctly labeled cut-up hip-and-valley), corrected the ridge-cap coverage constant to be product-dependent (~31 lf/bundle cut 3-tab vs ~20 lf/bundle pre-formed TimberTex/TimberCrest/Z-Ridge), and added a Coverage-Constants Quick-Reference table plus an explicit waste-factor-must-match-geometry guard. v1.2 rewritten 2026-04-25 from eval improvement cycle — named config-field binding (suppliers.preferred[], suppliers.account_numbers[], material_brands.*, dump.bundles_per_yard), paste-ready supplier quote-request block (ABC Supply / Beacon Building Products / SRS Distribution formats), tariff-aware price column, and an example 28-square architectural shingle order. v1.1 enhanced with material categories, waste factor tables, supplier-ready formatting, bundle/unit conversion."
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

**Fastest path — minimum viable input:** *squares + roof geometry (eave / rake / ridge / hip / valley linear feet) + tear-off vs. overlay.* That is the whole blocking set. Everything else resolves from `config.yml` or is inferred and stated in the Assumptions footer. If the user hands you a measurement report (EagleView / Hover / GAF QuickMeasure), the geometry is already in it — read it and go.

1. **Roof measurements** `[blocking]` — Total squares (or square footage), pitch, and geometry (linear feet of eave, rake, ridge, hip, valley). If only squares are given, ask once for pitch and eave/rake footage — those two drive starter, drip edge, and I&W and cannot be honestly inferred.
2. **Material system** `[from config]` — Defaults to `material_brands.shingle_default`, `.underlayment_default`, `.iws_default`, `.ridge_cap_default`, `.starter_default`. Only ask if the customer specified a brand that isn't the shop's default.
3. **Job scope** `[blocking, but binary]` — Tear-off (single/multi-layer), overlay, or new construction. Assume single-layer tear-off if unstated (the market-typical case) and flag it; it changes only the debris/dumpster line, not the material quantities.
4. **Accessories** `[inferred]` — If not provided, infer a market-typical set from the geometry (pipe boots ≈ 1 per plumbing stack, typically 2–4 on a residential roof; drip edge = eave + rake; ridge vent = ridge lf) and label each inferred count in the footer for the estimator to confirm on site. Never inflate.
5. **Supplier preference** `[from config]` — Defaults to `suppliers.preferred[0]` and its `quote_format`; delivery window from `suppliers.delivery_window_default`.

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

10. **Tear-off debris** — If scope is tear-off, compute debris in bundles first, then convert: debris bundles = squares × 3 (architectural; × 4 for designer/luxury, × 2 for 3-tab) × number of layers. Debris yards = debris bundles ÷ `dump.bundles_per_yard` (default 4). Recommend the smallest size in `dump.dumpster_sizes_yd[]` that clears the result with headroom. *Do not multiply squares by `bundles_per_yard` — that constant is a divisor, not a multiplier, and inverting it under-sizes the dumpster by ~2x.*

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

- **Runs from defaults.** Brands, supplier + quote format, delivery window, account numbers, dump sizing, and pricing mode all come from config. The only genuinely blocking ask is geometry — and if a measurement report is attached, there is no ask at all.
- Single clarifying question max — usually pitch and eave/rake footage when only squares are provided. Ask once, in one message, not in sequence.
- Never fabricate geometry to avoid the question: starter, drip edge, and ice & water are all linear-foot lines, and a guessed eave length silently mis-orders three of them.
- If two suppliers are in `suppliers.preferred[]`, produce both blocks so the user can drop simultaneous RFQs
- Cross-reference sibling skills: `estimate-builder` (the takeoff drives the cost basis), `tariff-price-adjuster` (when pricing_mode = tariff_aware, annotations come from this skill's market_conditions block), `crew-schedule-optimizer` (delivery window must align with crew start)

## Example Output (28-sq architectural tear-off, cut-up hip-and-valley roof, ABC Supply)

```
ROOF GEOMETRY (basis for every line below)
28.0 squares · 6:12 main field · cut-up hip-and-valley (4 valleys, 2 dormers)
Eave 104 lf · Rake 80 lf (= 184 lf eave+rake) · Ridge 22 lf · Hip 20 lf (= 42 lf ridge+hip) · Valley 24 lf

ORDER SUMMARY
# Material            Brand / Spec               Basis             Qty   Waste  Order   Unit       Notes
1 Shingles            GAF Timberline HDZ (Charcoal) 28 sq × 3      84    15%    97      bundles    Tariff-aware: +4% vs Q1 2026
2 Starter strip       GAF Pro-Start              184 lf eave+rake  184   round  2       bundles    @ 105 lf/bundle
3 Ridge cap           GAF TimberTex (pre-formed) 42 lf ridge+hip   42    round  3       bundles    @ ~20 lf/bundle (pre-formed)
4 Synthetic underlay. GAF Tiger Paw              28 sq             28    10%    4       rolls      @ 10 sq/roll (30.8 sq → 4)
5 Ice & water shield  GAF StormGuard             (104 lf eave + 24 lf valley) × 3 ft  384 sf  10%   3  rolls   422 sf ÷ 195 sf/roll → 3
6 Drip edge           Aluminum, brown, 10 ft     184 lf eave+rake  184   round  19      pieces     184 ÷ 10 → 19
7 Ridge vent          GAF Cobra Snow Country     22 lf ridge       22    round  4        pieces     @ 7 ft each (ridge only, not hips)
8 Pipe boots          Lifetime, 1.5" / 3"        2 × 1.5", 1 × 3"  3     —      3        each       —
9 Nails               1-1/4" EG coil roofing     28 sq × 80 shingles × 6 nails  13,440   round  2  boxes  6-nail high-wind pattern @ ~7,200 nails/box
10 Tear-off debris    Single-layer architectural 28 sq × 3 bdl ÷ 4 bdl/yd  84 bdl ≈ 21 yd  —  1  30-yd  30-yd dumpster (21 yd + headroom)

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
- 3 ea   GAF StormGuard Ice & Water Shield
- 19 ea  Aluminum drip edge, brown, 10' (D-style)
- 4 ea   GAF Cobra Snow Country ridge vent
- 3 ea   Lifetime pipe boots (2× 1.5", 1× 3")
- 2 box  1-1/4" electro-galvanized coil roofing nails

DELIVERY NOTES
- Curbside drop OK; no rooftop loading
- Two-day pickup if quantity adjusts

NOTES & ASSUMPTIONS
- Shingle: defaulted to material_brands.shingle_default = GAF Timberline HDZ
- Waste: 15% (cut-up roof — 4 valleys, 2 dormers) — matches the geometry block above
- Nails: 6-nail high-wind pattern per code for this county. Basis is 28 sq × ~80 shingles/sq = 2,240 shingles × 6 = 13,440 nails ÷ ~7,200/box = 1.87 → 2 boxes. (Nails are counted per shingle installed, never per bundle ordered — the waste bundles are not nailed down.)
- Ridge vent: ridge lf only (22 lf). Hips are not vented on this roof; if hip vent is specified, add 20 lf.
- Tariff flag: shingle line +4% vs Q1 2026 — confirm with supplier on quote return
- Tear-off debris: 84 bundles ÷ 4 bundles/yd ≈ 21 yd → 30-yd dumpster (nearest size up in dump.dumpster_sizes_yd[])
- Inferred (confirm on site): pipe-boot count (3) inferred from a typical 2-bath stack layout; no chimney or skylight flashing in scope
- Sanity check: 28 sq × 3 bundles = 84 → 97 with waste — within expected range
```

(Run with your own measurements + config to replace these illustrative values.)
