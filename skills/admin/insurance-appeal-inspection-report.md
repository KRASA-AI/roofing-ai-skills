---
name: "Insurance Appeal Inspection Report"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~60 min/report"
version: 1.2
last_eval_score: 8.8
inspiration: "v1.2 (2026-07-13) eval improvement cycle targeting output_quality + clarity — the worked Example Output was internally inconsistent in three ways that would each survive into a document a carrier reads: (1) the appeals success statistic appeared in three different forms in the same report ('31 of 38 (82%)' in the resolved-fields block, 'of 31 appeals, 25 overturned (82%)' — which is 80.6%, not 82% — on the cover page and again in the opinion letter); (2) the example's section numbering ran 1-9 against a report spec that defines 10 numbered sections, so every cross-reference in the example pointed at the wrong section ('see Section 5 methodology' pointed at the rebuttal table, not the remaining-life analysis; 'methodology limitation in Section 7' pointed at remaining-life, not the photo log); (3) remaining useful life was stated as '14-16 years' on the cover page and in the rebuttal table but '14.5 years' in the opinion letter and the RUL table. A report whose own numbers disagree is the exact thing this skill exists to catch a carrier doing. All three reconciled, the example renumbered to the spec's 10 sections, and a Consistency Check added to the output requirements so the failure mode cannot recur. Efficiency deliberately left at its honest floor: field inspection data cannot be defaulted without fabricating evidence, and a fabricated appeal report is worse than no appeal report. v1.1 rewritten 2026-04-28 from eval improvement cycle — named config-field binding (inspector.haag_id / .haag_certifications[] / .expert_witness_history / .years_inspecting / .professional_liability_carrier / .independence_statement, appeals.success_rate_last_12_months / .recent_wins[] / .typical_turnaround_business_days, service_area.licensed_states[], pricing.appeal_inspection_flat) and a populated Example Output (CAPE Analytics moss-and-discoloration non-renewal in Knoxville TN contested under Bulletin 25-03 with the imagery-and-summary demand). v1.0 algorithm-compatible-language design preserved."
---

# 🛡️ Insurance Appeal Inspection Report

## Purpose

Produce a homeowner-side counter-inspection report specifically engineered to contest a carrier-driven non-renewal, repair demand, or ACV downgrade triggered by third-party aerial imagery AI. Unlike a general inspection report, this deliverable is structured to mirror how insurance algorithms process roof data — objective measurements, geo-tagged evidence, and quantified remaining life — so a human reviewer can override a flawed algorithmic decision quickly.

## When to Use

- A homeowner received a non-renewal notice, repair demand letter, or ACV conversion based on aerial/satellite imagery they never saw
- An insurer is threatening to drop coverage because of a remote-assessed "roof age" or "condition" flag
- A homeowner wants proactive documentation before a policy renewal window to prevent a surprise cancellation
- A claim was denied or underpaid because the carrier's automated assessment disagreed with visible damage
- A real estate transaction is being held up by a buyer's carrier flagging the roof remotely
- The homeowner has 30–60 days to submit evidence appealing a coverage decision

## Required Input

Provide the following:

1. **Carrier situation** — Carrier name, policy number (last 4), what decision is being contested (non-renewal, repair demand, ACV conversion, claim denial), deadline for appeal/response, any reference number the carrier assigned, and the carrier's stated reason (paste or paraphrase)
2. **Aerial imagery trigger** (if known) — Whether the carrier cited CAPE Analytics, Nearmap, EagleView, Zesty.ai, or another vendor; the date of the imagery if provided; specific defects the carrier flagged (moss, discoloration, missing shingles, overhanging branches, etc.)
3. **Homeowner & property info** — Owner name, property address, year roof was installed (invoices if available), material type, manufacturer and product line if known, total roof square footage, any prior insurance claims on the roof
4. **Field inspection data** — Date and time of inspection, weather conditions, photos taken (with descriptions of what each shows and its location), measurements (pitch, squares, valley/ridge/eave linear footage), moisture readings, attic findings if inspected, thermal imaging results if used
5. **Defects actually observed vs. carrier claims** — For each carrier-cited defect: whether it exists, whether it's within normal wear, whether it's serviceable vs. replacement-triggering, and measurement/quantification
6. **Inspector credentials** — License number, HAAG or manufacturer certifications, years of experience, any prior expert-witness or adjuster background

## Instructions

You are drafting a counter-documentation report for a homeowner fighting an algorithmically-driven insurance decision. The reader is a carrier underwriter, ombudsman, or state insurance commissioner — not a casual homeowner. Every claim must be defensible.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.address`, `company.letterhead_path` — header / footer block + every-page credential bar
  - `inspector.name`, `inspector.haag_id` (HAAG residential / commercial cert numbers), `inspector.haag_certifications[]` (e.g., HAAG-Certified-Residential / HAAG-Certified-Commercial / HAAG-Certified-Wind), `inspector.years_inspecting`, `inspector.expert_witness_history` (boolean + brief history line if true), `inspector.professional_liability_carrier` + policy number reference, `inspector.independence_statement` (one-sentence company-defined no-financial-interest declaration). Drives the Section 3 credentials block and the every-page header
  - `appeals.success_rate_last_12_months` — used as the credibility anchor in the cover-page conclusion line ("X of Y non-renewal appeals overturned"), not in the rebuttal sections themselves
  - `appeals.recent_wins[]` — anonymized {state, carrier, defect_type, outcome} entries, optionally cited in the opinion letter when one matches the current case
  - `appeals.typical_turnaround_business_days` (default 5) — surfaces in the cover page so the homeowner / carrier knows when to expect the deliverable on a follow-up appeal
  - `service_area.licensed_states[]` — the inspector is licensed to inspect / submit reports in these states; if the property's state is not in the list, halt and flag rather than produce
  - `pricing.appeal_inspection_flat` (default $375) — referenced at the end of the report only if the user requests a pricing line, not in the report body
  - `voice.expert` (falls back to `voice`) — clinical / objective / third-party tone for this skill specifically; never the marketing voice
  - If a named field is missing, use a sensible default and flag it in the Assumptions footer
- Reference `knowledge-base/terminology/` for correct defect descriptors (granular loss vs. blistering vs. fishmouthing, etc.)
- Reference `knowledge-base/regulations/` for any state insurance commission documentation standards. If the homeowner is in NY, CA, CO, WA, FL, or TX, consult `knowledge-base/regulations/insurance-ai-landscape.md` for the applicable state-specific hook (DFS Circular Letter 2024-7, C.R.S. §10-3-1104.9, SB 1120, WAC 284-30, OIR memoranda, TDI data-use framework) and include a request for the carrier's explainability documentation in the cover page and closing paragraph. If the homeowner is in **Tennessee**, additionally cite **Bulletin 25-03** and demand (a) copies of the aerial imagery the carrier relied on, (b) the date the imagery was captured, and (c) the carrier's accuracy verification — Tennessee's bulletin entitles the homeowner to all three. If the homeowner is in **Kentucky**, cite **Bulletin 2026-01** and demand the date-stamped aerial imagery (must be within the last twelve months) plus the written summary the bulletin requires the carrier to produce. Absence of either is a procedural defect the Kentucky Department of Insurance recognizes
- Cross-reference with `roof-inspection-report` skill for base field findings, but override with the appeal-specific structure below

**Design principles for this report (critical):**

1. **Algorithm-compatible language** — Carriers feed appeal documents into their own NLP pipelines. Use objective, quantified language ("granular loss measured at 12% of surface area in south-facing plane") not subjective language ("roof looks a little worn"). Avoid hedging words.
2. **Geo-tagged evidence** — Every photo must be anchored to a GPS coordinate, slope orientation, and measurement. Screenshots of phone GPS metadata or a map overlay diagram strengthen the filing.
3. **Direct rebuttal of each flagged defect** — For every item the carrier's AI cited, include a labeled section that names the cited defect, presents counter-evidence, and states the verdict (no defect present / defect present but within serviceability / defect misidentified).
4. **Quantified remaining useful life** — Provide a defensible remaining-life estimate with the methodology used (manufacturer specs + observed wear + local climate adjustment).
5. **Credential block on every page** — The report's weight comes from the inspector's credentials. Repeat license/cert info on each page header.

**Report structure:**

### 1. Cover Page
- Title: "Independent Roof Condition Assessment — Insurance Appeal Documentation"
- Property address, owner name, carrier name, policy reference
- Inspector name, license #, certifications
- Inspection date, report date
- A one-sentence conclusion line (e.g., "This roof is in serviceable condition with an estimated X years of remaining useful life and does not warrant non-renewal on condition grounds")

### 2. Purpose & Scope
- One paragraph stating why this report was commissioned (cite the carrier's letter/decision with reference number)
- The specific defects or conditions being contested (bulleted list drawn from the carrier's letter)
- The inspection methodology used (ground + ladder + walkthrough + drone/thermal if applicable)

### 3. Inspector Credentials & Independence
- Full credentials, licensure, HAAG/manufacturer certs, years inspecting
- Statement of independence: no financial interest in policy retention
- Professional liability coverage confirmation if applicable

### 4. Property & Roof System Overview
- Manufacturer, product line, install date, warranty status
- Total squares, pitch, slope count, penetrations count, ventilation system
- Reference the original installation permit / invoice if available

### 5. Defect-by-Defect Rebuttal Table

| # | Carrier-Cited Defect | Carrier's Source | Field Finding | Quantification | Verdict | Photo Refs |
|---|---|---|---|---|---|---|
| 1 | "Moss growth on north slope" | CAPE Analytics 2026-03-12 | Trace lichen, <3% of slope surface, treatable | 14 sq ft of 760 sq ft slope = 1.84% | Within normal wear — not a condition defect | Photos 04–07 |

For each row, include 1–2 sentences of supporting narrative below the table explaining the finding.

### 6. Positive Condition Findings
- Items that support continued serviceability: intact flashings, proper ventilation, sound decking (if observed via attic), no active leaks, no soft spots, manufacturer warranty still active
- Each finding tied to a photo or measurement

### 7. Remaining Useful Life Analysis
- Manufacturer-stated product life
- Age-based remaining life (product life minus roof age)
- Adjustment for observed wear and local climate (hail frequency, UV exposure, freeze-thaw)
- Final estimated remaining useful life in years with methodology footnote

### 8. Photo Log
- Numbered photo references with:
  - GPS coordinate (lat/long to 5 decimals)
  - Slope orientation and pitch
  - Focal subject and measurement (if applicable)
  - Date/time stamp
- Minimum 20 photos for a standard residential appeal; commercial larger

### 9. Professional Opinion Letter
- One page on letterhead with signature
- Clear statement of overall condition assessment
- Explicit statement rebutting the carrier's non-renewal grounds with reference to defect-by-defect findings
- Professional recommendation: continued insurability / targeted repair scope / replacement scope (only if warranted)

### 10. Appendix
- Copy of the carrier's letter being appealed (first page only, with policy number redacted to last 4)
- Manufacturer product specification sheet
- Local code citations if relevant
- Credentials documentation (license copy, cert cards)

**Consistency check (run before delivering — this report will be read adversarially):**

A carrier reviewer looking for a reason to affirm the original decision will look for a number that disagrees with itself. Before output, verify:

1. **The remaining-useful-life figure is one number, stated identically** on the cover page, in the rebuttal table, in Section 7, and in the opinion letter. If Section 7's methodology yields 14.5 years, no other section may say "14–16 years." Ranges are acceptable only if every section carries the same range.
2. **The appeals success statistic appears in exactly one form** — the `appeals.success_rate_last_12_months` numerator and denominator as configured ("31 of 38 (82%)"), and the displayed percentage must equal numerator ÷ denominator rounded to the nearest whole percent (31 ÷ 38 = 81.6% → "82%" is correct; "25 of 31 (82%)" is not, because 25 ÷ 31 = 80.6%). It appears on the cover page and, if a matching `appeals.recent_wins[]` entry exists, in the opinion letter. Nowhere else.
3. **Every internal cross-reference points at the right section number.** The report has 10 numbered sections in the order specified above; the photo-log methodology limitation lives in Section 8, the remaining-life methodology in Section 7.
4. **Every quantification in the rebuttal table reconciles with the photo log and the slope areas** in Section 4 (e.g., "14 sq ft of 760 sq ft north slope = 1.84%" — check the division).
5. **Every carrier-cited defect in Section 2 has exactly one matching row in Section 5.** No orphans in either direction.

Any mismatch is a hand-back to the carrier. Fix it before the homeowner signs anything.

**Output requirements:**
- Tone: objective, clinical, third-party expert — never adversarial toward the carrier
- Every quantified claim tied to a measurement method
- No marketing language, no emotional appeals, no homeowner testimonials
- Company header and inspector credentials on every page
- Saved as `outputs/insurance-appeals/{carrier}-{property-address-slug}-appeal-report.md` if the user confirms
- Recommend a companion cover letter for the homeowner to sign (draft separately using the `email-drafter` shared skill)

**Pricing guidance for this service:**
- This is a premium service (typical market range: $250–$500 per inspection + report) because homeowners facing a full roof replacement without coverage are losing $15,000–$25,000 if they lose their policy
- Contractor positioning: neutral advocate, not sales-driven — this report protects the contractor's reputation as well as the homeowner's coverage
- Successful appeals generate referrals and reviews that boost AI-search visibility for the phrase "roofer who helped me fight insurance"

**Efficiency notes:**
- If the carrier letter isn't provided, ask for it before producing the rebuttal section — the defect list must match the carrier's language
- If thermal or drone imagery wasn't captured, note this as a methodology limitation rather than fabricate
- Cross-reference sibling skills: `roof-inspection-report` (base field report), `insurance-supplement-writer` (if the outcome triggers a claim), `follow-up-sequence` (post-appeal communication cadence)
- If the homeowner's roof actually is in poor condition, advise the user candidly rather than draft a misleading report — losing credibility on one appeal destroys the service line

## Example Output

### CAPE Analytics moss-and-discoloration non-renewal — Knoxville TN, Bulletin 25-03 demand

> **Inbound:**
> "Carrier: Stillwater P&C. Policy ending in 4421. Decision being contested: non-renewal effective 2026-06-15 based on a 'condition assessment' from CAPE Analytics imagery dated 2026-02-19. Cited defects: moss growth on north slope, discoloration on south slope, and 'roof age beyond serviceable life.' Homeowner has 30 days to submit evidence. Property: 412 Birch Hollow Rd, Knoxville TN 37920. Owner: Patricia Lambert. Roof: Owens Corning Duration Storm architectural, installed 2017-08-14 (8 years, 6 months old at inspection). Total roof: 24.5 squares. Field inspection performed 2026-04-22. Inspector: William Reyes (HAAG-Certified-Residential #14782, 19 years inspecting, expert-witness history). Photos taken: 28. No active leaks. Attic dry. Field findings: trace lichen <2% of north slope, no granular loss above manufacturer thresholds, all flashings intact, ridge cap sealant sound, manufacturer warranty 17 years remaining."
>
> **Resolved fields:**
> - `inspector.haag_id` → 14782 (HAAG-Certified-Residential)
> - `inspector.years_inspecting` → 19
> - `inspector.expert_witness_history` → true (4 prior carrier-appeal expert testimony engagements in TN, KY, NC since 2023)
> - `inspector.professional_liability_carrier` → CNA Insurance, policy #PL-22817-A ($2M limit)
> - `inspector.independence_statement` → "The undersigned inspector has no contingent fee, no commission, and no financial interest in the outcome of the homeowner's policy retention. The inspection fee is paid in full at scheduling regardless of finding."
> - `appeals.success_rate_last_12_months` → 31 of 38 (82%) non-renewal appeals overturned in last 12 months
> - `appeals.recent_wins[]` → contains {TN, Stillwater P&C, moss, overturned 2026-01-09} match — cited in opinion letter
> - `appeals.typical_turnaround_business_days` → 5
> - `service_area.licensed_states[]` → contains TN ✓
>
> ---
>
> **HEADER (every page):** Acme Roofing & Inspection LLC | TX-RC-0481234 / TN-Reg-RC-3392 | William Reyes, HAAG-Certified-Residential #14782, 19 yrs | 469-555-0140
>
> ### 1. COVER PAGE
>
> **Independent Roof Condition Assessment — Insurance Appeal Documentation**
>
> Property: 412 Birch Hollow Rd, Knoxville TN 37920
> Owner: Patricia Lambert
> Carrier: Stillwater Property & Casualty Insurance Company
> Policy reference: ending 4421 | Decision: Non-renewal effective 2026-06-15
> Inspection date: 2026-04-22 | Report date: 2026-04-25
> Inspector: William Reyes, HAAG-Certified-Residential #14782, 19 years inspecting
>
> **Conclusion:** This 8-year, 6-month-old Owens Corning Duration Storm asphalt-shingle roof at 412 Birch Hollow Rd is in serviceable condition with an estimated 14.5 years of remaining useful life and does not warrant non-renewal on condition grounds. Of 38 non-renewal appeals this firm has supported in the last 12 months, 31 were overturned in the homeowner's favor (82%); 4 of those involved Tennessee carriers and CAPE Analytics imagery as the trigger. Per the inspector's standard practice, this report was delivered within 5 business days of inspection completion.
>
> ---
>
> ### 2. PURPOSE & SCOPE
>
> This report was commissioned by Patricia Lambert in response to a non-renewal letter from Stillwater Property & Casualty Insurance Company dated 2026-04-04 (carrier reference NR-2026-04-0991), which cited the following defects sourced from CAPE Analytics imagery dated 2026-02-19:
>
> 1. "Moss growth observed on north slope"
> 2. "Discoloration consistent with granular loss observed on south slope"
> 3. "Roof age approaching end of serviceable life"
>
> **Methodology:** Ground-level survey + ladder access to all four slopes + interior attic inspection + handheld moisture-meter readings at five eave / valley locations + 28 geo-tagged photographs. No drone or thermal imaging on this engagement; the lack of those modalities is noted as a methodology limitation in Section 8 (Photo Log) rather than fabricated.
>
> ### 3. INSPECTOR CREDENTIALS & INDEPENDENCE
>
> William Reyes — HAAG-Certified-Residential #14782 (current, recertification 2027-09); HAAG-Certified-Wind #11203; 19 years inspecting residential and small commercial roofs across TX, TN, KY, NC, AL. Expert-witness history: testified or signed affidavits in 4 carrier-appeal proceedings (TN x2, KY x1, NC x1) since 2023 with no rebuttal of credentials by opposing counsel.
>
> Professional liability: CNA Insurance, policy #PL-22817-A, $2,000,000 each-claim limit, current.
>
> Independence statement: "The undersigned inspector has no contingent fee, no commission, and no financial interest in the outcome of the homeowner's policy retention. The inspection fee is paid in full at scheduling regardless of finding."
>
> **Tennessee Bulletin 25-03 demand (preserved on cover-page reference):** Pursuant to Bulletin 25-03 (issued April 2026), the homeowner has separately requested from Stillwater P&C, by certified mail dated 2026-04-22, copies of (a) the aerial imagery the carrier relied on, (b) the date the imagery was captured, and (c) the carrier's accuracy verification documentation. As of report date, the carrier has not produced any of the three. The Tennessee Department of Commerce & Insurance recognizes absence of any of these as a procedural defect supporting reversal.
>
> ### 4. PROPERTY & ROOF SYSTEM OVERVIEW
>
> | Item | Value |
> |---|---|
> | Manufacturer / Product | Owens Corning Duration Storm (impact-resistant SBS-modified architectural) |
> | Install date | 2017-08-14 (per receipt + permit on file with City of Knoxville #B-2017-08-1284) |
> | Roof age at inspection | 8 years, 6 months |
> | Manufacturer life | 25-year limited (StainGuard Plus) — 17 years remaining as of report date |
> | Total area | 24.5 squares (verified by drone-free measurement; eave / rake / ridge / valley table in Appendix B) |
> | Slopes | 4 slopes — N (760 sf), S (820 sf), E (430 sf), W (440 sf) |
> | Pitch | 6:12 main field, 4:12 porch |
> | Penetrations | 3 plumbing vents, 1 attic vent box, 1 chimney chase |
> | Ventilation | Soffit intake (continuous, 192 lf) + ridge vent (CertainTeed Cool Ridge, 38 lf); NFA ratio 1:158 (passes IRC 1:300) |
> | Permit / install records | On file (City of Knoxville permit + manufacturer registration + warranty card) |
>
> ### 5. DEFECT-BY-DEFECT REBUTTAL
>
> | # | Carrier-Cited Defect | Carrier's Source | Field Finding | Quantification | Verdict | Photo Refs |
> |---|---|---|---|---|---|---|
> | 1 | "Moss growth observed on north slope" | CAPE Analytics image dated 2026-02-19 | Trace lichen colonies on lower 6 ft of north slope only. Light gray, dry, non-mat-forming. No moss; lichen ≠ moss in roofing-condition assessment. Treatable by single zinc-strip installation ($85 service). | 14 sq ft of 760 sq ft north slope = 1.84% of slope area. Industry threshold for "moss-driven condition defect" is typically ≥10% surface coverage AND mat formation. | **No defect present.** Trace lichen is within normal wear, treatable, and does not affect waterproofing integrity. | Photos 04, 05, 06, 07 |
> | 2 | "Discoloration consistent with granular loss observed on south slope" | CAPE Analytics image dated 2026-02-19 | Discoloration present and confirmed in field. Photos taken in matched-lighting conditions show the discoloration is **algae-streaking** (Gloeocapsa magma) on the StainGuard Plus product, a known cosmetic phenomenon explicitly disclaimed by Owens Corning's StainGuard 10-year algae warranty. Granule layer measured at 3.1 mm uniform thickness across all 5 sample test squares — within manufacturer spec for an 8-year-old shingle. No granule loss observed. | 5 test squares × 100 sq ft sampled. Granule depth: 3.1 mm avg (manufacturer spec for 8-yr Duration Storm: 2.8–3.4 mm). | **Defect misidentified.** Algae-streaking ≠ granule loss. The product carries an active 10-year algae warranty from manufacturer. | Photos 09, 10, 11, 12, 13 |
> | 3 | "Roof age approaching end of serviceable life" | CAPE Analytics image dated 2026-02-19 | Roof age is 8 years, 6 months. Manufacturer-stated product life is 25 years (StainGuard Plus). Field inspection confirms intact granule layer, sealed flashings, sound underlayment (verified via attic), no soft spots, no active leaks. | Remaining useful life: 14.5 years (see Section 7 methodology). | **No defect present.** A roof with 14.5 years of remaining useful life is not "approaching end of serviceable life" by any industry standard. | Photos 01, 02, 03, 14, 15, 16 |
>
> *Narrative for Defect #2:* The CAPE Analytics imagery dated 2026-02-19 was captured in late winter, when low-angle sun and wet substrate conditions accentuate algae streaking on light-colored shingles. The field inspection, performed at midday on a dry roof, confirms the discoloration is surface algae growth — a known cosmetic phenomenon — not granule loss. Granule depth measurements taken with calibrated digital depth gauge across 5 test squares (5 × 5 random sampling, north / south / east / west / ridge) returned 3.1 mm average against an 8-year manufacturer spec window of 2.8–3.4 mm. The Owens Corning StainGuard Plus warranty explicitly covers this algae appearance for 10 years from install (active through 2027-08-14).
>
> ### 6. POSITIVE CONDITION FINDINGS
>
> | Finding | Evidence |
> |---|---|
> | All flashings intact and sealed | Photos 17, 18 (chimney + plumbing penetrations); sealant pliable, no fishmouthing |
> | Ridge vent operational, no intrusion | Photo 19; CertainTeed Cool Ridge installed continuous, no debris |
> | Decking sound | Photo 20 (attic from interior); no daylight visible at any penetration; no soft spots on walk-test |
> | No active leaks | Moisture-meter readings at 5 attic locations ≤ 12% (manufacturer threshold ≤ 19%) |
> | Manufacturer warranty active | StainGuard Plus through 2042-08-14; algae warranty through 2027-08-14 |
> | Permit + install records on file | City of Knoxville permit B-2017-08-1284; manufacturer registration confirmed |
>
> ### 7. REMAINING USEFUL LIFE ANALYSIS
>
> | Component | Value |
> |---|---|
> | Manufacturer-stated product life | 25 years |
> | Roof age | 8.5 years |
> | Age-based remaining life | 16.5 years |
> | Observed-wear adjustment | -1.0 yr (algae presence cosmetic; lichen treatable; no functional wear) |
> | Local-climate adjustment (East TN UV / freeze-thaw) | -1.0 yr (moderate freeze-thaw, low-to-moderate UV) |
> | **Estimated remaining useful life** | **14.5 years** |
>
> Methodology: Manufacturer life from Owens Corning Duration Storm published spec sheet (Appendix C). Climate adjustment per the IBHS Knoxville-region exposure rating. Observed-wear per HAAG-aligned wear methodology.
>
> ### 8. PHOTO LOG (representative — full log Appendix A)
>
> | # | Subject | GPS | Slope / Pitch | Measurement | Date / Time |
> |---|---|---|---|---|---|
> | 01 | Full roof from front yard | 35.91842, -83.95571 | All slopes visible | 24.5 sq total | 2026-04-22 11:14 EDT |
> | 04 | North slope lichen colony | 35.91845, -83.95569 | N / 6:12 | 14 sq ft of 760 sq ft slope = 1.84% | 2026-04-22 11:23 EDT |
> | 09 | South slope algae streak (matched lighting) | 35.91839, -83.95573 | S / 6:12 | Streaking confirmed; not granule loss | 2026-04-22 11:31 EDT |
> | 11 | South slope test square 3 (granule depth) | 35.91839, -83.95573 | S / 6:12 | 3.1 mm granule depth | 2026-04-22 11:34 EDT |
> | 19 | Ridge vent operational | 35.91841, -83.95572 | Ridge | 38 lf continuous | 2026-04-22 11:42 EDT |
> | 20 | Attic — south slope underside | 35.91841, -83.95572 | Interior | No daylight; sound decking | 2026-04-22 11:51 EDT |
>
> 28 photos total in Appendix A, each with GPS to 5 decimals, slope, pitch, focal subject, measurement (where applicable), and timestamp.
>
> **Methodology limitation:** No drone or thermal-imaging used on this engagement. All slopes were accessed by ladder and walked. Where a drone or thermal pass would have added evidence, it is noted in the photo log as "ladder access only — drone/thermal not used."
>
> ### 9. PROFESSIONAL OPINION LETTER
>
> *Acme Roofing & Inspection LLC — Letterhead*
>
> 2026-04-25
>
> Stillwater Property & Casualty Insurance Company
> Re: Patricia Lambert / Policy #4421 / Decision NR-2026-04-0991
>
> The roof at 412 Birch Hollow Rd, Knoxville TN 37920, is in serviceable condition with an estimated 14.5 years of remaining useful life. The non-renewal grounds cited in your letter dated 2026-04-04 are not supported by field evidence:
>
> - The "moss growth on north slope" is trace lichen at 1.84% of slope area, treatable, and does not affect waterproofing integrity.
> - The "discoloration consistent with granular loss on south slope" is surface algae streaking expressly covered by the active Owens Corning StainGuard Plus warranty (through 2027-08-14). Granule depth measured at 3.1 mm — within the manufacturer's 8-year spec window of 2.8–3.4 mm — confirms no granule loss.
> - The "roof age approaching end of serviceable life" assertion is inconsistent with an 8-year-old, 25-year-rated, fully-warranted roof.
>
> Of 38 non-renewal appeals this firm has supported in the last 12 months, 31 were overturned (82% reversal rate). The most analogous prior matter — Stillwater P&C non-renewal of a 9-year Owens Corning Duration roof in Chattanooga TN, January 2026, citing CAPE Analytics moss findings — was reversed by Stillwater on review, with the homeowner's policy reinstated.
>
> Pursuant to Tennessee Bulletin 25-03, the homeowner has separately demanded copies of the aerial imagery, the imagery capture date, and the carrier's accuracy verification documentation. As of this report date the carrier has not produced any of the three. Absence of these is a procedural defect under the bulletin.
>
> The roof should be retained on its current policy without modification. We respectfully request reversal of the non-renewal decision.
>
> William Reyes, HAAG-Certified-Residential #14782
> Acme Roofing & Inspection LLC | TN-Reg-RC-3392 | 469-555-0140 | wreyes@acmeroofs.com
>
> ### 10. APPENDIX
>
> A. Full photo log (28 photos with full metadata)
> B. Field measurement diagram (eave / rake / ridge / valley linear footage by slope)
> C. Owens Corning Duration Storm published spec sheet
> D. Manufacturer warranty registration confirmation
> E. City of Knoxville permit B-2017-08-1284 (install)
> F. CNA professional liability declaration page (current)
> G. Inspector HAAG certification cards (Residential #14782, Wind #11203)
> H. Copy of Stillwater P&C non-renewal letter (page 1 only, policy redacted to last 4)
> I. Copy of homeowner's certified-mail Bulletin 25-03 demand letter to Stillwater (delivery confirmation included)
>
> ---
>
> **Companion deliverable:** A signed cover letter for Patricia Lambert is drafted separately via the `email-drafter` shared skill so the report can be transmitted under her signature, as the bulletin requires the homeowner (not the inspector) to be the demand party.
>
> ### Assumptions footer for this run
>
> - `inspector.haag_id` resolved (14782 Residential / 11203 Wind) and the recertification date pulled from the cert library
> - `appeals.success_rate_last_12_months` displayed as "38 appeals / 31 overturned (82%)" — identical wording on the cover page and in the opinion letter, and nowhere else. The rebuttal sections stay inspector-credential-anchored, never appeal-volume-anchored.
> - `appeals.recent_wins[]` matched on {TN, Stillwater P&C, moss} → cited the Chattanooga 2026-01-09 reversal in the opinion letter
> - `service_area.licensed_states[]` contains TN ✓ — proceeded with the report
> - `pricing.appeal_inspection_flat` not surfaced (user did not request a pricing line)
> - `voice.expert` defaulted to clinical / objective / third-party tone (separate from the marketing voice)
