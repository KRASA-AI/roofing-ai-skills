---
name: "Insurance Appeal Inspection Report"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~60 min/report"
version: 1.0
last_eval_score: null
inspiration: "2026 insurance-AI landscape — carriers using aerial imagery vendors to trigger non-renewals and coverage reductions, creating a new service category for contractors: homeowner-side counter-documentation designed to match how carrier AI processes roof data"
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
- Load `config.yml` from the repo root for company name, license number, credentials, signature block, and phone
- Reference `knowledge-base/terminology/` for correct defect descriptors (granular loss vs. blistering vs. fishmouthing, etc.)
- Reference `knowledge-base/regulations/` for any state insurance commission documentation standards. If the homeowner is in NY, CA, CO, WA, FL, or TX, consult `knowledge-base/regulations/insurance-ai-landscape.md` for the applicable state-specific hook (DFS Circular Letter 2024-7, C.R.S. §10-3-1104.9, SB 1120, WAC 284-30, OIR memoranda, TDI data-use framework) and include a request for the carrier's explainability documentation in the cover page and closing paragraph
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
| 1 | "Moss growth on north slope" | CAPE Analytics 2026-03-12 | Trace lichen, <3% of slope surface, treatable | 14 sq ft of 820 sq ft slope | Within normal wear — not a condition defect | Photos 04–07 |

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

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
