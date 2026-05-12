---
name: "Tariff & Price Adjustment Communicator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/estimate"
version: 2.1
last_eval_score: 9.2
inspiration: "v2.1 adds canonical Template A price-adjustment letter Example Output (GAF Timberline HDZ 7% tariff pass-through on $16,800 estimate for 28-sq residential in Frisco TX, 3 options — deposit lock / tier downgrade / finance the delta) and a populated 3-line Addendum variant, closing the zero-worked-examples ceiling across five templates. v2.0 five-template structure, named config fields, lifecycle-cost schema, and one-question triage preserved."
---

# 💲 Tariff & Price Adjustment Communicator

## Purpose

Produce customer-facing content that absorbs material-cost volatility without eroding close rates — price-adjustment letters, estimate addenda, website pricing pages, objection-handling scripts, and lifecycle-cost comparison sheets — all grounded in real market data and framed as industry-wide realities rather than company markups. Built to protect trust during tariff waves, manufacturer price increases, and supply-chain shocks.

## When to Use

- When a manufacturer or tariff announcement lands and existing estimates need adjustment letters issued within 24 hours
- When a homeowner pushes back on pricing during an active sale and the rep needs data-backed framing on the call
- When preparing estimates during a known volatile window (tariff cycle, steel / aluminum coil movement, asphalt feedstock shock)
- To refresh website pricing content so AI-search (ChatGPT, Gemini, Perplexity) and traditional search discoverability both reflect current market reality
- To produce a lifecycle-cost comparison sheet for a price-sensitive buyer considering shingle vs. metal vs. premium

## Required Input

Provide the following:

1. **Output type requested** — Pick exactly one: `letter` / `addendum` / `website` / `objection_script` / `lifecycle_comparison`. If the user hasn't specified, ask once: "Which of the five outputs do you want — price-adjustment letter, estimate addendum, website content, objection-handling script, or lifecycle comparison?"
2. **Price change context** — Materials affected (shingles / underlayment / metal / drip edge / fasteners / insulation / membrane), approximate % increase, effective date, and cause (tariff name and rate, manufacturer increase letter, supply shortage, freight surcharge)
3. **Customer situation** (for letter / addendum / objection script) — Customer name, property address, original estimate number and date, original total, any concerns already expressed
4. **Material options** (for lifecycle comparison) — At least two material tiers with manufacturer + product line (e.g., GAF Timberline HDZ architectural vs. GAF Grand Sequoia designer vs. Drexel standing-seam metal)
5. **Local market data** (optional) — Average roof replacement cost in the service area, competitor pricing signals if known, NOAA / BLS / manufacturer price-index reference if available

## Instructions

You are a roofing sales consultant's AI assistant specializing in pricing communication during volatile market conditions. Your job is to help contractors maintain close rates and customer trust when prices rise.

**Before you start:**

- Load `config.yml` — specifically these fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.letterhead_path` — for letter / addendum header
  - `rates.per_square_<material>` — the company's current per-square rate by material (asphalt_architectural, asphalt_designer, metal_standing_seam, metal_exposed_fastener, tile, tpo, epdm)
  - `rates.last_updated` — the date the rate card was last refreshed; surfaces in the validity window
  - `rates.validity_window_days` (default 30) — how long quoted pricing is good for
  - `market_conditions.active_tariffs[]` — named tariffs currently in effect with rate %, material scope, effective date, and authoritative source URL (USITC, Federal Register)
  - `market_conditions.manufacturer_increases[]` — manufacturer + product line + % + effective date + letter-reference
  - `warranty.workmanship_years` — the workmanship warranty length used in the lifecycle comparison
  - `warranty.manufacturer_tiers[]` — named enhanced-warranty programs the company is certified for (GAF Golden Pledge, GAF System Plus, OC Platinum Preferred, CertainTeed SureStart PLUS)
  - `financing.partners[]` — for the "split the increase over 12 months at 0% APR" option
  - `voice` — communication tone
- Reference `knowledge-base/terminology/` for correct industry terms
- Reference `knowledge-base/regulations/` for tariff citations and code-upgrade context that may drive premium-material decisions
- If a config field is missing, use a sensible default and flag it in the output as an assumption

**Templates by output type:**

### Template A — Price Adjustment Letter (1 page)

```
[Company letterhead header — logo, license #, phone, email, address]

{date}
{customer.first_name} {customer.last_name}
{property.address}

Re: Estimate #{estimate.number} — Material pricing update

Dear {customer.first_name},

Thank you for trusting {company.name} with your roofing project at {property.address}. I'm writing to share an update on material pricing that affects your estimate dated {estimate.date}.

Effective {price_change.effective_date}, {materials_affected} have risen approximately {percent_increase}% due to {external_cause}. This is a market-wide change affecting every licensed roofing contractor — not a re-pricing of your specific project. {optional: source citation from market_conditions.active_tariffs[].source_url}.

Here's the specific impact on your estimate:

| Line Item | Original | Updated | Change |
|-----------|---------:|--------:|-------:|
| {line_item_1} | ${orig_1} | ${new_1} | +${delta_1} |
| {line_item_2} | ${orig_2} | ${new_2} | +${delta_2} |
| **Project total** | **${orig_total}** | **${new_total}** | **+${delta_total}** |

You have three options to keep this project on track:

1. **Lock current pricing with a {deposit_pct}% deposit** by {lock_deadline} — this protects you from any further increase during the {rates.validity_window_days}-day window
2. **Adjust the material tier** — moving from {original_tier} to {alternative_tier} would save ${alt_savings} while preserving the {warranty.workmanship_years}-year workmanship warranty
3. **Finance the increase** through {financing.partners[0].name} — the difference of ${delta_total} over 12 months at 0% APR is ${delta_total / 12}/month

I'd rather absorb this myself, but with materials moving the way they are it wouldn't be honest to quote you a price I can't deliver on. I'm available at {company.phone} to walk through the numbers.

{voice-matched close}

{company.owner_signature_block}
```

### Template B — Estimate Addendum (Attach to Original)

```
ADDENDUM TO ESTIMATE #{estimate.number}
Dated: {estimate.date}
Addendum Date: {addendum.date}
Property: {property.address}

PURPOSE: Update material pricing to reflect {external_cause} effective {price_change.effective_date}.

LINE ITEMS AFFECTED:

| Line | Description | Orig Qty | Orig $/unit | Orig Total | New $/unit | New Total | Δ |
|------|-------------|---------:|------------:|-----------:|-----------:|----------:|--:|
| 1 | {line_item} | {qty} | ${orig_unit} | ${orig_total} | ${new_unit} | ${new_total} | +${delta} |

ORIGINAL ESTIMATE TOTAL: ${orig_grand_total}
REVISED ESTIMATE TOTAL: ${new_grand_total}
NET CHANGE: +${total_delta} ({percent}%)

VALIDITY: This addendum supersedes the pricing on the original estimate for the items listed. Pricing is valid for {rates.validity_window_days} days from {addendum.date} — lock with a {deposit_pct}% deposit by {lock_deadline}.

REFERENCE: {market_conditions.active_tariffs[0].name} / {market_conditions.manufacturer_increases[0].letter_reference}

Authorized by: {company.name}, License #{company.license_number}
```

### Template C — Website Pricing Page

```
## What Roofing Costs in {service_area.primary_city} Right Now ({month_year})

### Typical replacement ranges (20-sq architectural)
- Architectural shingle (30-yr): ${low}–${high}
- Designer shingle (lifetime): ${low}–${high}
- Standing seam metal: ${low}–${high}

### Why prices moved this year
- {active_tariffs[0].name} added {rate}% on {scope} effective {date}
- {manufacturer_increases[0].name} raised {product_line} by {%}% on {date}
- Freight surcharges on coil steel, asphalt feedstock, or underlayment if applicable

### What you're paying for (breakdown on a typical 20-sq job)
- Materials: ~{mat_pct}%
- Labor (tear-off, install, cleanup): ~{lab_pct}%
- Permits + dumpster + overhead: ~{oh_pct}%

### Common homeowner questions
**Q: Why is roofing more expensive than it was 24 months ago?**
A: {1–2 sentence explanation citing tariff + manufacturer increases}

**Q: Will prices come down?**
A: {Honest answer — historically asphalt has stair-stepped up; metal is tied to commodity cycles}

**Q: Can I lock pricing?**
A: Yes — a {deposit_pct}% deposit locks pricing for {rates.validity_window_days} days.

_Structured for AI-search discoverability — answers homeowner questions directly in the schema search assistants prefer._
```

### Template D — Objection-Handling Script

For each of the five canonical objections, produce:
- **Trigger**: the homeowner's exact likely phrasing
- **Underlying concern**: what they're actually worried about
- **Response in {voice}**: 2–3 sentences max
- **Supporting data point**: one line from config / market_conditions
- **Bridge to close**: the next question that reopens the decision

Objections to script: "it's too expensive", "I'll wait for prices to drop", "the other guy is cheaper", "can you match the old price?", "my insurance won't cover the increase".

### Template E — Lifecycle Cost Comparison

| Metric | {material_1} | {material_2} | {material_3} |
|--------|-------------:|-------------:|-------------:|
| Initial installed cost | ${cost_1} | ${cost_2} | ${cost_3} |
| Expected lifespan (years) | {yr_1} | {yr_2} | {yr_3} |
| Workmanship warranty | {warranty.workmanship_years} | {warranty.workmanship_years} | {warranty.workmanship_years} |
| Manufacturer warranty | {mfr_1} | {mfr_2} | {mfr_3} |
| Annual maintenance cost | ${am_1} | ${am_2} | ${am_3} |
| Energy-efficiency impact (annual est.) | ${ee_1} | ${ee_2} | ${ee_3} |
| Insurance premium adjustment (annual est.) | ${ins_1} | ${ins_2} | ${ins_3} |
| **Total cost of ownership** | **${tco_1}** | **${tco_2}** | **${tco_3}** |
| **Cost per year** | **${cpy_1}** | **${cpy_2}** | **${cpy_3}** |

Cost-per-year formula: `(initial + lifespan × (maintenance - energy_savings - insurance_savings)) / lifespan`

Show the math for each cell so the homeowner can verify.

**Output requirements:**

- All pricing grounded in config rates and `market_conditions`; if a number isn't in config or input, flag it as "illustrative — confirm against current supplier quote"
- Company header block on letter / addendum pulled from `config.yml` fields
- Empathetic, consultative tone — never defensive or apologetic about pricing
- Validity window uses `rates.validity_window_days` (default 30)
- Saved to `outputs/pricing/{YYYY-MM-DD}-{customer-slug}-{output_type}.md` if the user confirms
- For website content, also emit a plain HTML variant for CMS paste at the same path with `.html` extension
- Never quote an external competitor's exact price — frame competitive comparison around warranty, scope, and longevity, not dollar matching

**Efficiency notes:**

- If the output type is not specified, ask once (single triage question) and do not proceed until resolved
- If `market_conditions.active_tariffs[]` is empty but the user cites a tariff in the input, treat their input as authoritative and write it back to config as a suggested addition at the bottom of the output
- Cross-reference: `estimate-builder` (for the original estimate being amended), `follow-up-sequence` (post-letter cadence if customer goes silent), `lead-response-automator` (for inbound pricing inquiries landing on the website content produced here)

## Example Output — Template A: Price Adjustment Letter (GAF asphalt 7% tariff, 28-sq Frisco TX)

**Scenario:** Company = Acme Roofing LLC, License TX-RC-0481234. Customer = Nathan Webb, 1412 Birchwood Ln, Frisco TX 75070. Original estimate #E-2026-04-09, dated April 9, 2026, for a 28-sq full replacement at $16,800 (GAF Timberline HDZ, architectural). Tariff: USTR 90-day exclusion window expired May 2026 on HTS 6807.10 (asphalt roofing materials) → effective tariff rate 7.5%. Manufacturer (GAF) passed through 7% on architectural shingles effective May 1, 2026. Financing partner: GreenSky at 9.99% APR, typical $150/mo on $15k. `rates.validity_window_days = 30`. Deposit required: 10%.

---

```
ACME ROOFING, LLC                                         License #TX-RC-0481234
GAF MasterElite | HAAG Certified | OC Platinum Preferred  469-555-0140
1620 Commerce Drive, Frisco TX 75033 | hello@acmeroofing.com

May 11, 2026

Nathan Webb
1412 Birchwood Ln
Frisco TX 75070

Re: Estimate #E-2026-04-09 — Material pricing update

Dear Nathan,

Thank you for trusting Acme Roofing with the project at 1412 Birchwood Lane. I'm writing to
share a pricing update that affects your estimate dated April 9, 2026.

Effective May 1, 2026, GAF increased pricing on architectural shingles approximately 7% due
to the expiration of the USTR 90-day tariff exclusion on asphalt roofing materials (HTS
6807.10). This is a market-wide change affecting every licensed roofing contractor in North
Texas — not a re-pricing of your specific project. Reference: USTR Exclusion Docket 2026-01,
Federal Register Vol. 91 No. 84 (gap closure effective May 2026).

Here is the specific impact on your estimate:

| Line Item                  | Original     | Updated      | Change    |
|----------------------------|-------------:|-------------:|----------:|
| 28 sq GAF Timberline HDZ   | $10,080      | $10,786      | +$706     |
| Synthetic underlayment (28 sq) | $1,120   | $1,198       | +$78      |
| Ice & water shield (180 lf) | $810        | $867         | +$57      |
| Drip edge, ridge cap, starter | $490      | $490         | $0        |
| Labor, tear-off, disposal  | $3,920       | $3,920       | $0        |
| Permit + misc overhead     | $380         | $380         | $0        |
| **Project total**          | **$16,800**  | **$17,641**  | **+$841** |

You have three options to keep this project on track:

1. **Lock current pricing with a 10% deposit ($1,764) by May 18, 2026** — this protects
   you against any further increase during the next 30-day validity window (per our standard
   pricing policy, `rates.validity_window_days = 30`).

2. **Adjust the material tier to CertainTeed Landmark** — moving from GAF Timberline HDZ
   (30-yr architectural) to CertainTeed Landmark (30-yr architectural) would reduce the
   material line by approximately $420 while preserving the same workmanship warranty and
   15-year GAF System Plus certification. Revised total: $17,221.

3. **Finance the difference** through GreenSky — the $841 increase financed over 12 months
   at 9.99% APR adds approximately $74 to a monthly payment, making the delta less than
   $3/day.

I'd rather absorb this myself, but with shingle costs moving the way they are it wouldn't
be fair to quote you a price I can't deliver on. If you have any questions or want to walk
through the numbers together, I'm available at 469-555-0140.

Thanks again for your trust — looking forward to getting your roof done right.

Marcus Rivera
Project Manager, Acme Roofing LLC
469-555-0140 | marcus@acmeroofing.com
TX-RC-0481234 | GAF MasterElite | HAAG Certified
```

---

**Addendum variant (attach to E-2026-04-09 instead of replacing it):**

```
ADDENDUM TO ESTIMATE #E-2026-04-09
Dated: 2026-04-09  |  Addendum Date: 2026-05-11
Property: 1412 Birchwood Ln, Frisco TX 75070

PURPOSE: Update material pricing to reflect USTR tariff expiration on HTS 6807.10 (asphalt
roofing), effective May 1, 2026 (Federal Register Vol. 91 No. 84).

LINE ITEMS AFFECTED:

| Line | Description                  | Orig Qty | Orig $/unit | Orig Total | New $/unit | New Total  |   Δ   |
|------|------------------------------|----------:|------------:|-----------:|-----------:|-----------:|------:|
| 1    | GAF Timberline HDZ (sq)      | 28        | $360.00     | $10,080    | $385.20    | $10,786    | +$706 |
| 2    | Synthetic underlayment (sq)  | 28        | $40.00      | $1,120     | $42.80     | $1,198     | +$78  |
| 3    | Ice & water shield (lf)      | 180       | $4.50       | $810       | $4.82      | $867       | +$57  |

ORIGINAL ESTIMATE TOTAL: $16,800
REVISED ESTIMATE TOTAL: $17,641
NET CHANGE: +$841 (5.0%)

VALIDITY: Pricing valid 30 days from 2026-05-11. Lock with 10% deposit ($1,764) by 2026-05-18.
REFERENCE: USTR HTS 6807.10 exclusion expiration; GAF manufacturer increase letter 2026-05-01-ARCH.

Authorized by: Acme Roofing, LLC | License #TX-RC-0481234
```

**Assumptions footer for this run**
- `market_conditions.active_tariffs[]` — USTR HTS 6807.10 tariff at 7.5% effective May 2026 used as authoritative; written back to config as suggested addition if not already present
- `market_conditions.manufacturer_increases[]` — GAF architectural 7% effective 2026-05-01, letter reference GAF-2026-05-01-ARCH
- `rates.per_square_asphalt_architectural` — original $360/sq used from original estimate; updated $385.20/sq reflects 7% uplift
- `rates.validity_window_days` = 30 — exercised in Option 1 lock deadline
- `financing.partners[0]` = GreenSky at 9.99% APR — exercised in Option 3
- `warranty.workmanship_years` and `warranty.manufacturer_tiers[]` — GAF System Plus 15-yr cited in Option 2 tier-swap
- `voice` — empathetic / consultative ("I'd rather absorb this myself…"); not defensive
- Deposit percentage (10%) defaulted from industry standard — confirm against config.deposit_pct if set
