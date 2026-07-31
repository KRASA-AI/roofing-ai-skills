---
name: "Roof Inspection Report"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~30 min/report"
version: 1.7
last_eval_score: 9.0
inspiration: "v1.7 (2026-07-28) eval improvement cycle targeting output_quality and personalization (cold score 8.0: clarity9 specificity9 industry_fit9 output_quality6 personalization5 efficiency9) — three confirmed defects fixed. (1) Section 3a's own trigger rule says run the Composite Condition Index for commercial/HOA/multi-family and skip it for a one-off residential storm/insurance report unless requested; the file did the opposite, running it in the residential example and omitting it from the commercial PM example. The residential example's Composite Condition Index block is removed; the commercial PM example now carries a full Inspection Findings section (9 categories, TPO-appropriate observations) plus the Composite Condition Index with re-derived math (Composite Score 71/100, Grade C), consistent with the example's REPAIR-AND-MONITOR capital-planning verdict. (2) The claim-recommendation rule states <5 strikes with no wind/leak damage = 'Not Recommended,' 5-7 = 'Marginal'; the residential example's South (3 strikes, 0 wind) and East (4 strikes, 0 wind) slopes were mislabeled 'Marginal' instead of 'Not Recommended.' Both the Damage Summary table and the Claim Recommendation narrative now apply 'Not Recommended' correctly and stay consistent with the Monitoring recommendation already present. (3) The inspector identity was fabricated/swapped throughout — both worked examples signed as 'Marcus Patel...HAAG #H-2019-4421,' but Marcus Patel is company.account_manager_name in config, not the inspector; the real inspector per config is William Reyes, HAAG ID 14782, 19 years inspecting. Both examples now sign as William Reyes with the correct HAAG ID and inspector.haag_certifications[]. Fabricated field names (inspector.title, inspector.email, inspector.cell, inspector.years_experience, certifications.haag, certifications.manufacturer[], certifications.state_license_states[], branding.logo_path, branding.report_letterhead_path, branding.primary_color_hex, rates.repair_minimum) were removed or replaced with the fields that actually exist in config (inspector.years_inspecting, flat certifications[] list, company.letterhead_path, rates.tearoff_per_square / rates.install_per_square_<material> / line-item rates), with company.logo_path explicitly flagged as absent-by-design rather than invented. Newly bound: inspector.professional_liability_carrier, inspector.independence_statement, inspector.expert_witness_history, and inspector.haag_certifications[] are now referenced in the Before-You-Start field list and demonstrated in both worked examples' Disclaimer/Certification footers, since these are exactly the credibility fields a receiving adjuster wants to see. v1.6 (2026-07-28) landscape-monitor concept extraction — added an optional Composite Condition Index (Section 3a): a weighted 0-100 score and A-F letter grade rolled up from the existing 1-5 section ratings, bound to a new optional `inspection.condition_index_weights` config field with sensible defaults when absent. Concept extracted from a July 2026 trade-press profile of a contractor operating system built by an Alpharetta, GA shop, which described feeding a standardized numeric inspection score into a continuous asset-condition-tracking program for owners and property managers, rather than a one-off narrative report. The repo's existing 1-5 per-section ratings gave an inspector-to-inspector consistent scale but no single comparable number a portfolio owner or a returning inspector could track over time; the composite index is that number, computed transparently (weights shown, formula shown) so an adjuster or PM can audit it rather than trust it as a black box. Nothing about the source contractor's own scoring formula, weighting, or software was copied — the section list, weights, and letter-grade bands are original to this repo and reuse the same 9 inspection categories and 1-5 scale already established in Section 3. Cross-references `maintenance-plan-generator` for shops that want to track the index across a customer's or portfolio's inspection history over multiple years. v1.5 (2026-06-22) landscape-monitor concept extraction — added a 'Documentation Completeness Check' step that audits the photos/notes provided against the standard documentation set an insurer (and an insurer's automated claim-review system) expects, names exactly which categories are missing, and flags when a gap would invite a coverage denial or force a return trip to the roof. Concept extracted from the 2026 contractor-side observation that carriers increasingly run automated review on the adjuster side to find documentation gaps and use those gaps to deny or underpay claims, and from field-capture tools that verify on-site that every required documentation angle was taken before the crew leaves (e.g., HomePro AI's field-capture/return-trip-prevention positioning surfaced in the May–June 2026 scans). Operationalized vendor-neutrally: it works from whatever photos a shop already takes (CompanyCam, camera roll, drive folder), requires no specific app, and produces a pre-submission gap list rather than a real-time on-roof prompt. All wording, the category taxonomy, and the worked example are original; no source checklist text, product copy, or numeric claims were copied, and no vendor's proprietary 'required-angle' list was reproduced. v1.4 (2026-06-15) eval improvement cycle targeting output_quality — fixed an internal inconsistency in the commercial PM Example Output header, which labeled the building '60-sq TPO retail strip' while the body (and the capital-planning table, priced per 38,000 sf) described a 38,000 sf roof; 38,000 sf is ~380 squares, not 60, so the header now reads '38,000 sf / 380-sq' to match the worked numbers an adjuster or asset manager would cross-check. v1.3 (2026-06-08) eval improvement cycle targeting output_quality (lowest-scoring skill in the repo at 7.8) — the residential Example Output defined a Photo Log and Disclaimer/Certification section in the structure but never showed them; both are now demonstrated end-to-end (23-photo log, full disclaimer + signature/cert footer). Added a second fully-worked Example Output for the Commercial Property-Manager variant (38,000 sf TPO retail strip, 14 yr, Plano TX) exercising the capital-planning table, IBC 1503 / ASCE 7 reserve-study compliance items, tenant-coordination notes, and commercial.cap_planning_horizon_years — the variant was described but unexemplified for two cycles. v1.2 rewritten 2026-04-25 from eval improvement cycle — named config-field binding (inspector.*, certifications.*, branding.*, rates.* for cost-range hints), explicit hail-claim threshold rule (8 strikes per square per FM Global / HAAG industry guidance), commercial property-manager output variant, and an example 28-square hail-damage report skeleton. v1.1 enhanced with structured inspection categories, condition rating scale, photo reference system, and insurance-grade documentation."
---

# 🏠 Roof Inspection Report

## Purpose

Turn field inspection photos and notes into a professional, structured inspection report with section-by-section findings, condition ratings, photo references, and prioritized recommendations — suitable for homeowner presentation, insurance documentation, real estate transactions, or commercial property-manager capital planning.

## When to Use

- After a field inspection to document findings for the homeowner
- When preparing documentation to support an insurance claim
- For real estate transaction roof certifications
- Annual or post-storm inspection reports for maintenance customers
- Commercial property-condition assessments for property managers and capital planners

## Required Input

Provide the following:

1. **Property info** — Customer name (or property-manager + tenant), property address, roof age (if known), material type, approximate total square footage, and customer type (residential / commercial / HOA / multi-family)
2. **Inspection notes** — Field observations organized by area: ridge, slopes, valleys, flashing, gutters, ventilation, penetrations (vents, pipes, skylights, chimney), interior (attic if inspected)
3. **Photos** — Descriptions or references to photos taken. Note what each photo shows and its location on the roof. For storm/insurance work, capture per-slope hail-strike counts within a 10' × 10' (one square) test square
4. **Inspection purpose** — General assessment, storm damage documentation, insurance claim support, real estate certification, maintenance check, or commercial capital-planning report
5. **Weather event** (optional) — If storm-related: date of event, type of damage expected (hail, wind, debris), and known storm data (hail size, peak gust, NOAA event ID)

## Instructions

You are a roofing inspector's AI assistant. Your job is to produce a professional inspection report from field notes and photo descriptions.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.address`
  - `inspector.name`, `inspector.haag_id`, `inspector.years_inspecting`, `inspector.haag_certifications[]` — surfaces in cover page + signature block. No standard config defines `inspector.title`/`.email`/`.cell`; if absent, use `company.phone`/`company.email_from` for signature-block contact info and note the substitution in Assumptions rather than inventing inspector-specific contact fields.
  - `inspector.professional_liability_carrier`, `inspector.independence_statement`, `inspector.expert_witness_history` — surfaces in the Disclaimer/Certification footer (Section 8) so a receiving adjuster or carrier can independently verify the inspector's standing and financial independence; state `expert_witness_history` plainly (true/false) rather than omitting it
  - `certifications[]` (flat list, e.g. "GAF Master Elite", "Owens Corning Preferred", "HAAG Certified") — printed on cover + every page header. There is no separate `certifications.haag` boolean or `certifications.manufacturer[]`/`certifications.state_license_states[]` sub-structure in standard config; treat the presence of a HAAG-type string in the flat list as HAAG certification, and pull state licensure from `company.license_number` only unless a shop's own config defines a state-license list.
  - `company.letterhead_path` — cover page letterhead styling. `company.logo_path` is a commonly-used field for the cover logo but is not guaranteed to be configured (this repo's eval fixture omits it deliberately); when absent, use a text placeholder (e.g. "[Company Logo]") and flag it in the output's Assumptions footer instead of inventing a path. No brand-color field (`branding.primary_color_hex` or similar) exists in standard config — default to plain black/white formatting unless a shop's config defines one.
  - `rates.tearoff_per_square`, `rates.install_per_square_<material>` (e.g. `install_per_square_architectural`), and relevant line-item rates (`ice_water_per_lf`, `drip_edge_per_lf`, `starter_per_lf`, `ridge_vent_per_lf`, `pipe_boot_each`, `permit_rate`) — used to populate the rough cost-range column in the Recommendations table when present. There is no standard repair-minimum/trip-charge field; if a shop's config doesn't define one, show "estimate-on-request" for small repairs rather than inventing a minimum
  - `voice` — communication tone (homeowner-friendly / professional / clinical depending on customer type)
  - `commercial.cap_planning_horizon_years` (default 10) — used in commercial reports to bracket the capital-planning recommendation window
  - `inspection.condition_index_weights` (optional map of section name → weight, summing to 100) — used in the optional Composite Condition Index (Section 3a); if absent, use the default weights shown there and note the default was used
  - If a named field is missing, use a sensible default and flag it in the output's "Assumptions" footer
- Reference `knowledge-base/terminology/` for correct industry damage descriptors
- Reference `knowledge-base/regulations/` for code citations referenced in recommendations (IRC R905 series for residential, IBC Chapter 15 for commercial)

**Report structure:**

### 1. Cover Page / Header
- `company.logo_path` (or "[Company Logo]" placeholder if absent — flag in Assumptions), letterhead styling from `company.letterhead_path`, `company.name`, `company.license_number`, certifications block from the flat `certifications[]` list
- Property address, customer name (or property-manager + building name), inspection date, inspector name (`inspector.name`, `inspector.haag_id`) with signature-block contact pulled from `company.phone`/`company.email_from` unless inspector-specific contact fields are configured
- Report purpose (general assessment, storm damage, insurance claim, real estate cert, capital planning)

### 2. Executive Summary
- 2–3 sentence overall assessment in `voice`
- Overall condition rating (1–5 scale below)
- Estimated remaining useful life
- Recommended action (no action, monitor, repairs, partial replacement, full replacement)
- For commercial: capital-planning horizon (defer / repair within 12 mo / capital replacement within `commercial.cap_planning_horizon_years` years)

### 3. Inspection Findings by Section — For each area, provide:

- **Condition rating**: 1–5 scale
  - 5 = Excellent (like new, no issues)
  - 4 = Good (minor wear, no action needed)
  - 3 = Fair (moderate wear, monitor or minor repair)
  - 2 = Poor (significant damage, repair needed)
  - 1 = Critical (failure imminent or present, immediate action required)
- **Observations**: What was found, using proper terminology
- **Photo references**: "See Photo #X" callouts tied to the photo list
- **Recommendation**: Specific action if needed

**Sections to cover** (skip any not inspected, but note them as "Not inspected"):

- Shingles / Roofing Material (granule loss, cracking, curling, blistering, missing, wind-lifted, hail-bruising)
- Ridge and Hip Caps (alignment, seal integrity, wear)
- Valleys (flashing condition, debris accumulation, wear pattern)
- Flashing (wall flashing, step flashing, counter flashing, chimney, skylight — seal integrity, rust, lifting)
- Gutters and Downspouts (attachment, flow, damage, screens)
- Ventilation (ridge vent, soffit vents, box vents — blockage, damage, NFA adequacy)
- Penetrations (pipe boots, exhaust vents, satellite mounts — seal condition, collar integrity)
- Decking (visible sagging, soft spots, water staining from attic view)
- Interior / Attic (insulation, moisture, daylight visibility, ventilation adequacy)

### 3a. Composite Condition Index (optional roof-health score)

Run this section whenever the customer type is commercial/HOA/multi-family, whenever the inspection is part of a maintenance plan, or whenever the user asks for a trackable score. Skip it for a one-off residential storm/insurance report unless requested — the 1-5 per-section ratings in Section 3 are sufficient there.

The index rolls the per-section 1-5 ratings from Section 3 up into a single 0-100 score and letter grade, so a returning inspector, a portfolio owner, or a maintenance-plan customer has one comparable number to track across visits instead of nine separate ratings to re-read each time.

**Method:**
1. Convert each section's 1-5 rating to a percentage (rating ÷ 5 × 100)
2. Multiply each section's percentage by its weight from `inspection.condition_index_weights`. If not configured, use these defaults (weighted toward waterproofing and structural categories over cosmetic ones): Shingles/Roofing Material 20, Flashing 15, Decking 15, Valleys 10, Penetrations 10, Ventilation 10, Gutters/Downspouts 10, Ridge/Hip Caps 5, Interior/Attic 5 (sums to 100; skip and renormalize any section marked "Not inspected")
3. Sum the weighted percentages for the Composite Score (0-100)
4. Map to a letter grade: A = 90-100, B = 80-89, C = 70-79, D = 60-69, F = below 60

Show the full per-section math in a table so the score is auditable, not a black box:

| Section | Rating (1-5) | % | Weight | Weighted % |
|---------|--------------|---|--------|------------|

State the weights used (config-bound or default) and the resulting **Composite Score + Letter Grade** as a one-line verdict. For maintenance-plan or commercial-portfolio customers, note the index alongside the date so it can be compared against the same property's prior inspection once one exists, and cross-reference `maintenance-plan-generator` for shops tracking the index as part of an ongoing service relationship.

### 4. Damage Summary (storm/insurance only)

For each affected slope, report:

- **Test square**: 10' × 10' representative area location and orientation
- **Hail-strike count**: number of confirmed functional hail strikes (granule displacement + mat exposure + circular bruise) within the test square
- **Strike size range**: small (≤0.75"), medium (0.75–1.25"), large (>1.25")
- **Wind-damage count**: missing/lifted/creased shingles per slope
- **Pattern consistency**: damage pattern aligns with reported event direction and severity (yes / partial / no — explain)

**Claim-recommendation rule** (industry-standard, HAAG-aligned):

- ≥ 8 functional strikes per test square on any slope, OR ≥ 25% of slope shows wind-creased shingles, OR active leak attributable to event → claim is recommended
- 5–7 strikes per test square on multiple slopes → marginal; recommend documentation and homeowner-discretion claim discussion
- < 5 strikes and no wind/leak findings → not recommended; document for monitoring file

State the rule applied and the per-slope count clearly so an adjuster can verify.

### 5. Photo Log
- Numbered list of all photos with location description and what the photo documents
- Format: "Photo #1 — North slope, mid-section: 6 functional hail strikes within 10×10 test square, granule displacement and exposed mat visible"

### 6. Documentation Completeness Check (storm / insurance work)

Before the report is treated as submission-ready, audit the photos and notes provided against the documentation set an insurer — and an insurer's automated claim-review process — expects to see. The goal is to surface every gap *now*, while a recapture is still cheap, instead of after an adjuster (or the carrier's review software) flags missing evidence and uses the gap as grounds to deny, delay, or underpay. A missing-evidence gap is the single most avoidable reason a well-founded claim stalls.

Run the provided documentation against these categories and mark each **Present**, **Partial**, or **Missing**:

- **Address / identity proof** — at least one shot that ties the photos to the property (street number, or a wide shot showing the house in context)
- **Full-roof orientation** — an establishing/overview shot of each slope or elevation so the detail shots can be located on the roof
- **Per-slope test square** — for every slope claimed, a wide shot of the 10×10 test-square location plus close-ups of the strikes/damage inside it, with a scale reference (coin, chalk circle, gauge)
- **Damage detail with scale** — individual functional-damage close-ups (granule displacement + mat exposure for hail; lift/crease for wind) clear enough for an adjuster to count
- **Collateral / soft-metal corroboration** — gutters, downspouts, vents, flashing, fascia, AC condenser fins, window screens, or other soft metals that independently confirm the event happened and its direction
- **Penetrations & flashing** — pipe boots, chimney/skylight flashing, valleys (often the source of disputes)
- **Pre-existing vs. event** — any shots that distinguish storm damage from prior wear, so the carrier cannot reattribute the whole claim to age
- **Interior / attic** (if accessed) — moisture, staining, or daylight that supports an active-leak or decking finding
- **Date / event context** — confirmation the documentation is tied to the reported event date and storm data (NOAA event ID, hail size, gust)

Output a short completeness table and a one-line verdict:

| Category | Status | Return-trip risk if claim proceeds |
|----------|--------|------------------------------------|

Then state one of:

- ✅ **Submission-ready** — all categories Present; no documentation gap an automated carrier review would flag.
- ⚠️ **Gaps before submission** — list each Missing/Partial category and the specific shot to recapture (e.g., "West slope: no scale reference in the strike close-ups — recapture #10–12 with a coin or chalk circle"). Recommend recapturing *before* the report is sent, not after a denial.

Only run this section for storm/insurance work; skip it for routine maintenance or real-estate certifications unless the user asks for it. Never invent photos to fill a gap — flag the gap honestly so the shop can decide to recapture or submit with a documented limitation.

### 7. Recommendations (Prioritized)

- **Immediate** (safety or active leaks): Action + urgency
- **Near-term** (within 6 months): Repairs to prevent escalation
- **Monitoring**: Items to watch at next inspection
- Cost ranges populated from `rates.tearoff_per_square`, `rates.install_per_square_<material>`, and relevant line-item rates when present; "estimate-on-request" otherwise (no repair-minimum field exists in standard config — do not invent one)

### 8. Disclaimer / Certification
- Standard inspection disclaimer (visual inspection only, not a warranty, not a guarantee of insurability)
- Inspector signature block from `inspector.name` / `inspector.haag_id` / `inspector.years_inspecting` / `inspector.haag_certifications[]`
- Inspector credibility block: `inspector.professional_liability_carrier`, `inspector.independence_statement`, and `inspector.expert_witness_history` — this is what lets an adjuster verify the inspector has no financial stake in the claim outcome and is professionally insured; include it in every insurance/claim-support report
- Company license + cert footer (`company.license_number`, `certifications[]`) on every page

**Output requirements:**

- Professional formatting suitable for customer delivery, insurance adjuster review, or commercial PM capital file
- Consistent 1–5 condition ratings throughout
- Photo references integrated into findings (not just appended)
- Actionable recommendations with priority levels and (when config supports) cost ranges
- Cover/letterhead styling from `company.letterhead_path` and `company.logo_path` (placeholder + Assumptions flag if absent) throughout
- Saved to `outputs/inspections/{property-address-slug}-{YYYY-MM-DD}.md` if user confirms

**Variant: Commercial Property-Manager Report**

When `customer_type` is commercial / multi-family / HOA, also produce:

- **Capital-Planning Section**: 5- and 10-year repair-vs-replace cost projection table (rows: defer, repair-only, partial-replacement, full-replacement; columns: scope, year-1 cost, 5-yr total, 10-yr total)
- **Compliance Section**: Code-upgrade items (IBC 1503, ASCE 7 wind zone, energy code if applicable) the PM needs in their reserve study
- **Tenant Coordination Notes**: Any access constraints, business-hours work windows, common-area protection requirements for the bid scope

**Efficiency notes:**

- Generate from field notes + photo descriptions without excessive clarification
- For storm work, infer the test-square count from the photo descriptions if not explicitly tabulated, then state the inference in the Assumptions footer
- For storm/insurance work, always run the Documentation Completeness Check (Section 6) before treating the report as submission-ready — a flagged gap caught here is cheaper than a carrier denial later
- Cross-reference sibling skills: `insurance-supplement-writer` (if claim is recommended), `insurance-appeal-inspection-report` (if the carrier disputes findings), `maintenance-plan-generator` (if condition warrants ongoing service), `estimate-builder` (if a replacement scope is recommended)

## Example Output (28-square asphalt, post-hail, residential)

```
[Company Logo — placeholder, company.logo_path not configured]    HAAG Certified Inspector
ACME ROOFING, LLC                                License #TX-RC-0481234

Roof Inspection Report — Insurance Claim Support
Property:   1248 Maple Ridge Dr, Frisco, TX 75070
Owner:      Jane Doe
Inspected:  2026-04-22, 9:15 AM CDT
Inspector:  William Reyes — 19 yrs inspecting, HAAG ID #14782

EXECUTIVE SUMMARY
Roof shows functional hail damage from the 2026-04-18 event consistent with
1.5" reported peak stone (NOAA event 20260418-DFW-HAIL-117). North and west
slopes meet HAAG-aligned claim threshold (≥8 strikes per test square). Overall
condition: 2 (Poor — storm-driven). Recommended action: file insurance claim
for full replacement of north and west slopes; south and east slopes fall
below the 5-strike marginal threshold with no wind damage — not recommended
for claim, documented below for the monitoring file only.

DAMAGE SUMMARY (per slope)
Slope    Test SQ Location    Strikes  Strike Size      Wind  Pattern  Verdict
North    Mid-slope,  20' E   12       Med (0.75–1.25") 0     ✅       Claim
West     Upper,      8' S    9        Med + 2 Large    1     ✅       Claim
South    Lower,      15' W   3        Small            0     —        Not Rec.
East     Mid-slope,  10' N   4        Small            0     —        Not Rec.

INSPECTION FINDINGS
Shingles            2  See Photos 1–14   GAF Timberline HDZ, 9 yrs old.
                                          Functional hail damage on N + W slopes.
Ridge / Hip Caps    3  See Photo 15     Two cracked caps on N ridge.
Valleys             3  See Photo 16     Open metal valley intact, debris light.
Flashing            4  See Photo 17     Step flashing intact at chimney.
Gutters             2  See Photos 18–19 Hail dents in gutter face confirm event.
Ventilation         4  See Photo 20     Ridge vent intact, NFA adequate.
Penetrations        3  See Photos 21–22 Pipe boots within useful life.
Decking             4  Attic visible    No staining, no soft spots.
Attic               4  See Photo 23     Insulation dry, no moisture intrusion.

Note: this is a one-off residential storm/insurance report, so the Composite
Condition Index (Section 3a) is skipped per its own trigger rule (index runs
for commercial/HOA/multi-family or maintenance-plan customers, or on request).

CLAIM RECOMMENDATION
Industry-standard rule applied: ≥8 functional strikes per 10×10 test square on
any single slope qualifies for claim recommendation. North slope: 12 strikes.
West slope: 9 strikes. Both meet threshold. Recommend filing claim for full
replacement scope. South slope (3 strikes) and East slope (4 strikes) are both
under the 5-strike threshold with zero wind-damage findings — per rule, both
are Not Recommended for claim and are documented for the monitoring file
instead. Routing to insurance-supplement-writer skill once carrier estimate
arrives.

RECOMMENDATIONS
Immediate    None — roof is watertight.
Near-term    File claim within carrier's typical 60-day notice window.
             Replace N + W slopes (est. $9,400–$11,200 from config rates,
             pending carrier scope).
Monitoring   Re-inspect S + E slopes in 12 mo for delayed granule loss.

PHOTO LOG (23 photos)
#1   N slope, mid: 12 functional strikes in 10×10 test square, granule
     displacement + exposed mat.
#2–8 N slope detail: individual bruises, chalk-circled, coin for scale.
#9   W slope, upper: 9 strikes incl. 2 large (>1.25") with mat fracture.
#10–14 W slope detail + 1 wind-creased shingle (8' S of test square).
#15  N ridge: two cracked hip caps.
#16  Open metal valley, light debris, no separation.
#17  Chimney step flashing intact, sealed.
#18–19 Gutter face: hail dents confirm directional event.
#20  Ridge vent intact; NFA adequate for 1,850 sf attic.
#21–22 Pipe boots, collars within useful life.
#23  Attic interior: insulation dry, no daylight, no staining.

DOCUMENTATION COMPLETENESS CHECK (insurance submission)
Category                     Status    Return-trip risk if claim proceeds
Address / identity proof     Present   —
Full-roof orientation        Present   N + W overview shots on file
Per-slope test square        Present   N + W have wide + close-ups w/ scale
Damage detail with scale     Present   coin/chalk in #2–8, #9–14
Collateral / soft-metal      Present   gutter dents (#18–19) confirm event
Penetrations & flashing      Present   #17, #21–22
Pre-existing vs. event       Present   N + W (claimed slopes) documented;
                                       S + E (Not Recommended, monitoring
                                       file only) not required for this claim
Interior / attic             Present   #23
Date / event context         Present   NOAA 20260418-DFW-HAIL-117 cited

VERDICT: ✅ Submission-ready — all categories Present for the N + W claim
scope; no documentation gap an automated carrier review would flag. S + E
were evaluated and found Not Recommended (below the 5-strike threshold, no
wind damage) and are documented for the monitoring file only, not submitted
as part of this claim.

DISCLAIMER / CERTIFICATION
This is a visual inspection only and does not constitute a warranty or a
guarantee of insurability. Findings reflect conditions observed on the
inspection date. Hail-strike counts follow HAAG-aligned functional-damage
criteria (granule displacement + mat exposure within a 10×10 test square).
Independence statement: the undersigned inspector has no contingent fee, no
commission, and no financial interest in the outcome of the homeowner's
policy retention. Professional liability: CNA Insurance, policy #PL-22817-A
($2M each-claim). Expert witness history: yes — available on request.

— William Reyes, Inspector (19 yrs inspecting) — Acme Roofing, LLC
  HAAG ID #14782 — HAAG-Certified-Residential #14782, HAAG-Certified-Wind
  #11203 — GAF Master Elite — License #TX-RC-0481234
  469-555-0142 — estimates@acmeroofs.com
  License + certifications printed on every page footer.
```

(Run with your own field data to replace these illustrative values.)

## Example Output (commercial PM variant — 38,000 sf / 380-sq TPO retail strip, capital planning)

```
[Company Logo — placeholder, company.logo_path not configured]
ACME ROOFING, LLC — Commercial Property-Condition Assessment      License #TX-RC-0481234
Property:  Parkside Commons, 1820 Preston Rd, Plano TX 75093 (retail strip, 38,000 sf)
Prepared for: Lincoln Property Co. (3rd-party PM) — Asset Manager file
Inspected: 2026-06-04 — Inspector: William Reyes, 19 yrs inspecting, HAAG ID #14782

EXECUTIVE SUMMARY
60-mil mechanically-attached TPO, ~14 yrs old, past mid-life. Overall condition
3 (Fair). Localized ponding at two interior drains and seam-fastener backout in
the NE field. No active interior leaks reported. Capital-planning verdict:
REPAIR-AND-MONITOR now; budget partial-to-full recover within the 10-yr horizon
(commercial.cap_planning_horizon_years = 10).

INSPECTION FINDINGS
Roofing Material (TPO) 3  See Photos 1–6    60-mil TPO membrane, 14 yrs. Seam-
                                             fastener backout in NE field (~400
                                             sf), isolated debonding at 3 seams.
Ridge/Hip (Coping)     4  See Photo 7       Perimeter parapet coping caps
                                             intact, sealant tight.
Valleys (Drains)       3  See Photos 8–9    Ponding at Drains 2 and 4; slow
                                             flow, debris in strainer baskets.
Flashing               3  See Photos 10–11  Curb flashing lifting at 2 of 6
                                             HVAC units; counterflashing
                                             sealant cracking.
Gutters (Scuppers)     4  See Photo 12      2 overflow scuppers clear and
                                             functional, minor debris only.
Ventilation (Curbs)    4  See Photo 13      HVAC curb seals intact, no ponding
                                             at curbs.
Penetrations           4  See Photos 14–15  Pipe boots + conduit sealed; 2
                                             pitch pans due for refresh <12 mo.
Decking                4  Interior walk     No sagging, no soft spots, no
                                             deflection at plenum check.
Interior/Plenum        4  See Photo 16      No ceiling-tile staining, no
                                             moisture at drain penetrations.

COMPOSITE CONDITION INDEX (default weights — no inspection.condition_index_weights configured)
Section          Rating  %    Weight  Weighted %
Shingles/Membrane  3     60%    20     12.0
Flashing           3     60%    15      9.0
Decking            4     80%    15     12.0
Valleys            3     60%    10      6.0
Penetrations       4     80%    10      8.0
Ventilation        4     80%    10      8.0
Gutters            4     80%    10      8.0
Ridge/Hip Caps     4     80%     5      4.0
Attic/Plenum       4     80%     5      4.0
COMPOSITE SCORE: 71 / 100 — GRADE: C
Run per Section 3a's trigger rule (commercial customer type) — a one-off
residential storm report would skip this. The Fair (3) ratings on the
membrane, flashing, and drainage categories — the same three areas driving
the repair scope above — pull the score into the C band, consistent with a
REPAIR-AND-MONITOR verdict rather than an urgent full-replacement score.
Track this number against the property's next inspection to confirm the
repair scope arrests further decline.

CAPITAL-PLANNING TABLE (per 38,000 sf)
Option              Scope                              Yr-1      5-yr total  10-yr total
Defer               No work; monitor                   $0        $0          $0 (risk: leak)
Repair-only         Reseam NE field, drain retrofit    $11,200   $24,000     $41,000
Partial replacement Recover NE 12,000 sf               $96,000   $108,000    $128,000
Full replacement    Tear-off + new 60-mil TPO          $0        $0          $323,000

COMPLIANCE / RESERVE-STUDY ITEMS
- IBC 1503 secondary (overflow) drainage — verify scuppers at recover.
- ASCE 7 wind uplift — 75093 in Zone II; confirm fastening pattern on recover.
- Energy code: white TPO meets CRRC reflectance; maintain on replacement.

TENANT COORDINATION NOTES
- Roof access via rear of Suites 110–118; business-hours work windows only.
- Common-area walkway protection required over storefront entries.

DISCLAIMER / CERTIFICATION
Visual inspection only; not a warranty or guarantee of insurability.
Independence statement: the undersigned inspector has no contingent fee, no
commission, and no financial interest in the outcome of any policy or claim
retention. Professional liability: CNA Insurance, policy #PL-22817-A ($2M
each-claim). Expert witness history: yes — available on request.
— William Reyes, Inspector (19 yrs inspecting) — Acme Roofing, LLC
  HAAG ID #14782 — HAAG-Certified-Residential #14782, HAAG-Certified-Wind
  #11203 — License #TX-RC-0481234
```

(Run with your own field data + config to replace these illustrative values.)
