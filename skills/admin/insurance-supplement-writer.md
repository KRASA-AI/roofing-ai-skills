---
name: "Insurance Supplement Writer"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/supplement"
version: 2.1
last_eval_score: 8.9
inspiration: "v2.1 rewritten 2026-04-28 from eval improvement cycle — named config-field binding (supplement.standard_op_rate / .typical_recovery_per_claim / .standard_attachments[], inspector.haag_id / .haag_certifications[] / .years_inspecting, appeals.recent_wins[], service_area.licensed_states[], pricing.supplement_filing_flat) and a populated canonical $7,400 hail Example Output (carrier $14,820 → revised $22,220 with 24-line supplement covering O&P + IWS + drip edge + decking discovery + detach/reset). v2.0 Xactimate line-item structure, state-bulletin escalation, and ten-category framework preserved."
---

# 🛡️ Insurance Supplement Writer

## Purpose

Draft supplement requests to insurance carriers that recover underpaid line items, missing overhead & profit (O&P), code-upgrade costs, depreciation, and code-required accessories — structured around the carrier's Xactimate estimate so adjusters can approve changes quickly.

## When to Use

- After comparing the carrier's scope to your field-verified scope and finding gaps
- When the adjuster's Xactimate estimate omits code-required items (drip edge, ice & water shield to code, ridge vent)
- To recover O&P on claims with three or more trades
- When depreciation is withheld and you need to justify recoverable depreciation release
- After a re-inspection where new damage was discovered (e.g., decking replacement once tear-off starts)
- When material prices shifted between claim settlement and job start

## Required Input

Provide the following:

1. **Claim basics** — Carrier name, claim number, date of loss, adjuster name and email, loss type (hail, wind, fire, wind-driven rain), insured name, property address, policy number (last 4 only)
2. **Carrier's current estimate** — The adjuster's Xactimate (or equivalent) estimate total, RCV, ACV, depreciation held, deductible, and net payable. Paste or reference line items by category if possible
3. **Your field-verified scope** — Measurements (squares, eave/rake/ridge/valley linear footage), photos taken, damage pattern notes, code items required in your jurisdiction (IRC/IBC/state amendments)
4. **Gaps to supplement** — For each item: Xactimate code if known (e.g., RFG 240, RFG IWS, RFG RIDGC), quantity variance, price variance, and reason (missing item, wrong quantity, wrong price, code upgrade, discovered condition)
5. **Supporting documents** — Inspection report reference, photo log numbers, manufacturer specs, local code section citations, NOAA/storm data report if weather-dependent

## Instructions

You are an insurance-supplement specialist's AI assistant. Your job is to produce a supplement request that is organized, justified, and formatted for the carrier's adjuster review workflow — so it gets approved rather than rebutted.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.email_from`, `company.address`, `company.letterhead_path`, `company.tax_id_last4`, `company.ein_last4` — header / footer + cover-letter block + W-9 reference
  - `inspector.name`, `inspector.haag_id`, `inspector.haag_certifications[]`, `inspector.years_inspecting`, `inspector.expert_witness_history` — credentials block surfaced on the cover letter footer when the supplement is contesting an AI-driven denial; required for carriers (e.g., Allstate, USAA TX) that demand inspector credentials with any supplement >$2,500
  - `supplement.standard_op_rate` — default 10/10 unless the carrier's market is a non-O&P state (NJ commercial, certain CA carriers); used to justify the O&P line
  - `supplement.typical_recovery_per_claim` — anonymized rolling-12-month average; surfaces on the cover letter only when the homeowner has asked what to expect
  - `supplement.standard_attachments[]` — ordered list of attachments that ship with every supplement (field inspection report, labeled photo log, measurement diagram, code citations, current supplier quote, signed contract / work authorization). Anchors the Section 6 attachments checklist
  - `appeals.recent_wins[]` — anonymized {state, carrier, defect_type, outcome} entries; cited in the cover letter only when one matches the current carrier + scope
  - `service_area.licensed_states[]` — supplement is filed only for properties in these states; otherwise halt and flag
  - `pricing.supplement_filing_flat` — surfaces only when the homeowner asks how the supplement-filing service is priced (typical $0 when the contractor recovers via the supplement; $250–$500 flat when the homeowner is self-managing)
  - `voice` — communication tone (typically professional / collaborative for adjuster-facing letters)
  - If a named field is missing, use a sensible default and flag it in the Assumptions footer
- Reference `knowledge-base/terminology/` for damage terms and Xactimate code mapping
- Reference `knowledge-base/regulations/` for code citations (IRC R905, local wind/hail amendments). When a supplement appears to be contesting a machine-generated denial pattern — short turnaround, templated denial language, no per-line response — consult `knowledge-base/regulations/insurance-ai-landscape.md` for the applicable state explainability hook (NAIC Model Bulletin adoption state, NY DFS Circular Letter 2024-7, C.R.S. §10-3-1104.9, etc.) and add a supplement-justification paragraph requesting the carrier's AI governance documentation for the denial decision. If the property is in **Tennessee** or **Kentucky** and the carrier's denial referenced or appears to have been driven by aerial imagery, additionally cite Tennessee Bulletin 25-03 (TN) or Kentucky Bulletin 2026-01 (KY) and demand the imagery copies, the capture date (Kentucky requires within twelve months), and the carrier's written summary of what the imagery shows. These two state bulletins give the supplement writer a direct, narrowly-scoped consumer-protection demand that does not depend on the broader AI-governance framework

**Supplement categories to cover (only include categories with actual gaps):**

1. **Missing Line Items** — Items the carrier omitted that your scope requires (starter strip, ridge cap shingles, ice & water shield to code, step flashing replacement, detach & reset of accessories)
2. **Quantity Variance** — Items priced at the wrong quantity (e.g., adjuster paid for 22 squares, field measures 26.4 with 15% waste → 30.4 squares)
3. **Price Variance** — Items priced below current market (cite current manufacturer price list or supplier quote, reference tariff/market conditions if applicable — cross-ref `tariff-price-adjuster` skill)
4. **Code Upgrades** — Items required by current code but not in original construction (ice & water shield 24" inside warm wall, drip edge on eaves and rakes per IRC R905.2.8.5, ridge vent NFA requirements)
5. **O&P (Overhead & Profit)** — Standard 10% overhead + 10% profit when three or more trades are required (roofing, gutters, siding/wrap, painting, framing, electrical for detach/reset)
6. **Recoverable Depreciation** — Itemize the depreciation held and request release upon job completion with invoice and photo documentation per policy terms
7. **Supplemental Discovery** — Items found after tear-off began (rotted decking, compromised fascia, bird-damaged soffit, deteriorated flashing pans)
8. **Detach & Reset Items** — Satellite dishes, solar attic fans, lightning rods, decorative elements
9. **Debris Removal / Dump Fees** — If not included or underpaid for the actual dump volume
10. **Permits & Inspections** — Municipal permit, final inspection fee, HOA review fees when applicable

**Output structure:**

### 1. Cover Letter (1 page)
- Company header from config (name, license #, certifications, contact)
- Date, carrier name, adjuster name, claim number, date of loss, insured name, property address
- Opening paragraph: thank the adjuster for their work, reference the original estimate date, state that upon field inspection and/or during work completion, additional items were identified requiring supplement
- Summary sentence: "Please find enclosed a supplement request in the amount of $X,XXX.XX across N line items, detailed below."
- Professional close with direct line and email

### 2. Supplement Summary Table

| # | Xactimate Code | Description | Original Qty / Price | Supplement Qty / Price | Variance | Justification Ref |
|---|---|---|---|---|---|---|
| 1 | RFG IWS | Ice & water shield | 0 SF | 420 SF @ $1.85 | +$777.00 | Code item — IRC R905.1.2 |

Total original RCV: $X,XXX.XX
Total supplement: $X,XXX.XX
Revised RCV: $X,XXX.XX
Plus O&P (if applicable): $X,XXX.XX

### 3. Line-Item Justifications

For each supplement item, produce a short paragraph:
- **Item**: Xactimate code + plain-English description
- **Original vs. Supplemented**: Qty/unit price/total, then supplemented Qty/unit price/total
- **Reason**: Missing / Qty variance / Price variance / Code / Discovery / O&P trigger
- **Supporting evidence**: Photo #s, measurement diagram reference, code citation, manufacturer spec sheet, or current supplier quote
- **Requested action**: "Please update line X to reflect…"

### 4. O&P Justification (if included)
- List the trades involved (three or more required)
- State the rationale: coordination, scheduling, supervision across multiple specialties
- Cite typical industry standard (10/10) and any carrier-specific language if known

### 5. Depreciation Recovery Request (if applicable)
- State the depreciation withheld amount
- Reference policy language for recoverable depreciation release upon completion
- Commit to providing the final invoice, lien waiver, and completion photos for release

### 6. Attachments Checklist
- Field inspection report (PDF)
- Labeled photo log with captions
- Field measurement diagram
- Code section citations
- Current supplier price quote or manufacturer spec sheet
- Signed contract / work authorization (if requested by carrier)

**Output requirements:**
- Tone: professional, collaborative, never adversarial — adjusters approve what they can defend
- Every supplemented line tied to evidence (photo, code citation, quote, measurement)
- Xactimate code format where known; plain description where not
- Company license, cert, and contact block from config on every page header/footer
- One clean PDF-ready document saved to `outputs/supplements/{claim-number}-supplement.md` if the user confirms
- If the user cannot provide Xactimate codes, use plain-language descriptions and flag items to verify codes before sending

**Efficiency notes:**
- Ask only for the claim basics, the carrier estimate total, and the gaps list if they're not provided
- Infer O&P trigger automatically when three or more trades are listed
- If measurements conflict with the carrier's estimate, compute the variance rather than ask the user to do it
- Reference sibling skills: `roof-inspection-report` for the supporting field report, `tariff-price-adjuster` for current material price justification

## Example Output

### Canonical $7,400 hail supplement — 28-sq architectural in 75070 (carrier $14,820 → revised $22,220)

> **Inbound:**
> "Carrier: State Farm. Claim #58-2K-Q9821. Date of loss: 2026-04-18 (1.5-inch hail, NOAA event 20260418-DFW-HAIL-117). Adjuster: Marcus Hill, marcus.hill@statefarm.com. Insured: Janelle Doe, 1248 Maple Ridge Dr, Frisco TX 75070. Policy ending 9173. Carrier estimate: $14,820 RCV / $11,238 ACV / $3,582 depreciation held / $1,500 deductible / $9,738 net payable. Field-verified scope: 28.5 squares with 15% waste = 32.8 squares; 162 lf eaves, 142 lf rakes, 86 lf ridge, 38 lf valley, 4 plumbing penetrations, 1 chimney chase, gutter detach/reset 162 lf, ridge vent. Code items required: IWS to wall (820 sf), drip edge eaves+rakes (304 lf), ridge vent NFA. Discovered after tear-off: 12 sf rotted decking south slope, fascia replacement at chimney 8 lf. Three trades on the job: roofing, gutter detach/reset, painting (chimney chase repaint). Current ABC Supply quote on file."
>
> **Resolved fields:**
> - `inspector.haag_id` → 14782 (HAAG-Certified-Residential)
> - `inspector.years_inspecting` → 19
> - `supplement.standard_op_rate` → 10/10 (TX is an O&P state)
> - `supplement.standard_attachments[]` → field inspection report + photo log + measurement diagram + code citations + ABC Supply quote
> - `appeals.recent_wins[]` → contains {TX, State Farm, hail, $7,200 supplement approved 2026-03-22} match — 1-line cite in cover letter
> - `service_area.licensed_states[]` → contains TX ✓
>
> ---
>
> **HEADER (every page):** Acme Roofing & Inspection LLC | TX-RC-0481234 | William Reyes HAAG-Certified-Residential #14782 (19 yrs) | EIN ending 8842 | 469-555-0140
>
> ### COVER LETTER
>
> *Acme Roofing & Inspection LLC — Letterhead*
>
> 2026-04-28
>
> Marcus Hill, Claims Adjuster
> State Farm Insurance Companies
> Re: Claim #58-2K-Q9821 / Date of loss 2026-04-18 / Insured Janelle Doe / 1248 Maple Ridge Dr, Frisco TX 75070 / Policy ending 9173
>
> Dear Mr. Hill,
>
> Thank you for your work on the 4/18 hail claim at 1248 Maple Ridge. Your initial estimate dated 2026-04-22 totaled $14,820 RCV. After completion of the field-verified inspection on 2026-04-23 and the start of tear-off on 2026-04-26, additional items were identified requiring supplement.
>
> Please find enclosed a supplement request in the amount of **$7,400.00** across **24 line items**, detailed below. The revised estimate total is **$22,220.00 RCV**.
>
> Of the recent supplements this firm has filed with State Farm in TX following 1.0–1.5-inch hail events, the most analogous prior matter (a 26-sq Frisco re-roof, claim approved 2026-03-22) settled at $7,200 supplement on a similar scope.
>
> Most of the supplemented items are code-required (IWS to wall, drip edge per IRC R905.2.8.5, ridge-vent NFA), discovered conditions (12 sf rotted decking on south slope identified once tear-off began), or O&P (three trades — roofing, gutter detach/reset, painting). Each line is tied to evidence: photo number, code citation, or current supplier quote.
>
> I am available at 469-555-0140 or wreyes@acmeroofs.com to walk through any line.
>
> Respectfully,
>
> William Reyes, HAAG-Certified-Residential #14782
> Acme Roofing & Inspection LLC | TX-RC-0481234 | EIN ending 8842
>
> ### 1. SUPPLEMENT SUMMARY TABLE
>
> | # | Xactimate Code | Description | Original Qty / Price | Supplement Qty / Price | Variance | Justification |
> |---|---|---|---|---|---|---|
> | 1 | RFG IWS | Ice & water shield (to warm wall) | 0 SF | 820 SF @ $1.85 | +$1,517.00 | Code — IRC R905.1.2 (24" inside warm wall) |
> | 2 | RFG DRIP | Drip edge — eaves | 162 LF @ $2.05 | 162 LF @ $2.45 | +$64.80 | Price variance — current ABC Supply quote |
> | 3 | RFG DRIP | Drip edge — rakes | 0 LF | 142 LF @ $2.45 | +$347.90 | Code — IRC R905.2.8.5 (rake drip edge required) |
> | 4 | RFG 240 | 30-yr architectural shingle | 28.0 SQ @ $362 | 32.8 SQ @ $362 | +$1,737.60 | Qty variance — field measure 28.5 sq + 15% waste = 32.8 |
> | 5 | RFG STARTER | Starter strip | 162 LF @ $1.65 | 304 LF @ $1.65 | +$234.30 | Qty variance — eaves + rakes (per manufacturer spec) |
> | 6 | RFG RIDGC | Hip / ridge cap | 86 LF @ $4.95 | 86 LF @ $5.45 | +$43.00 | Price variance — current quote (TimberCrest) |
> | 7 | RFG VENT | Ridge vent (continuous) | 0 LF | 38 LF @ $8.50 | +$323.00 | Code — IRC ventilation NFA 1:300 (Frisco amendment 1:150) |
> | 8 | RFG FELT | Synthetic underlayment | 28.0 SQ @ $32 | 32.8 SQ @ $32 | +$153.60 | Qty variance |
> | 9 | RFG STEP | Step flashing replacement (chimney) | 0 LF | 14 LF @ $9.85 | +$137.90 | Missing — hail-damaged step flashing requires replacement on tear-off |
> | 10 | RFG PIPE | Plumbing vent boot replacement | 2 EA @ $35 | 4 EA @ $42 | +$98.00 | Qty + price variance — 4 boots on field count, current price |
> | 11 | RFG DECK | Decking replacement (rotted south slope) | 0 SF | 12 SF @ $4.85 | +$58.20 | Discovery — rotted decking found at tear-off |
> | 12 | RFG DECK | Fascia replacement (chimney chase) | 0 LF | 8 LF @ $14.50 | +$116.00 | Discovery — bird-damaged fascia at chimney |
> | 13 | RFG R&R | Detach & reset gutters | 0 LF | 162 LF @ $2.85 | +$461.70 | Missing — gutters require detach for IWS install + drip edge |
> | 14 | RFG R&R | Detach & reset satellite dish | 0 EA | 1 EA @ $85 | +$85.00 | Missing item |
> | 15 | RFG R&R | Detach & reset attic vent box | 0 EA | 1 EA @ $65 | +$65.00 | Missing item |
> | 16 | RFG NAIL | High-wind 6-nail pattern (TX wind zone) | 4-nail @ included | 6-nail premium | +$144.00 | Code — TX wind zone amendment (90+ mph design) |
> | 17 | PNT CHIMSEAL | Chimney-chase paint touch-up after step flashing | 0 SF | 24 SF @ $3.85 | +$92.40 | Trade trigger — paint required after metal flash work |
> | 18 | RFG DUMP | Dump fee — 30-yd dumpster | 1 EA @ $385 | 1 EA @ $445 | +$60.00 | Price variance — current dump-rate market |
> | 19 | RFG PERMIT | Municipal permit (City of Frisco) | 1 EA @ $185 | 1 EA @ $235 | +$50.00 | Price variance — 2026 fee schedule update |
> | 20 | RFG INSP | Final municipal inspection fee | 0 EA | 1 EA @ $85 | +$85.00 | Missing item |
> | 21 | RFG OP | Overhead 10% on revised RCV | n/a | $1,930.00 | +$1,930.00 | Three trades trigger (roofing, gutter, paint) — `supplement.standard_op_rate` 10/10 |
> | 22 | RFG OP | Profit 10% on revised RCV | n/a | (rolled into Line 21) | (incl.) | Three trades trigger |
> | 23 | RFG R&R | Detach & reset solar attic fan | 0 EA | 0 EA | $0.00 | None present — line documented as not applicable |
> | 24 | DOC AI-EXPL | State explainability docs | n/a | n/a | $0.00 | Procedural request — not a billing line; see Section 7 |
> | | | | | **Subtotal supplement** | **+$7,400.00** | |
>
> Total original RCV: $14,820.00
> Total supplement: +$7,400.00
> **Revised RCV: $22,220.00**
> Revised ACV (after $3,582 depreciation hold): $18,638.00
> Net payable post-deductible: $17,138.00 (deductible $1,500)
>
> ### 2. LINE-ITEM JUSTIFICATIONS (representative — full set in Section 5 below)
>
> **Line 1 — RFG IWS Ice & water shield (820 SF @ $1.85, +$1,517.00):**
> *Original vs. supplemented:* 0 SF @ $0 → 820 SF @ $1.85
> *Reason:* Code item — IRC R905.1.2 requires ice & water shield 24" inside the warm wall in climate zone 3A (Frisco TX). Original estimate omitted entirely.
> *Supporting evidence:* Photos #11–#14 (eave + valley + chimney perimeter measurement), measurement diagram (Appendix A), IRC R905.1.2 citation (Appendix C), current ABC Supply quote dated 2026-04-25 on Carlisle WIP 300HT (Appendix D).
> *Requested action:* Please update the estimate to add Line RFG IWS at 820 SF × $1.85 = $1,517.00.
>
> **Line 4 — RFG 240 Architectural shingle qty variance (32.8 SQ vs. 28.0 SQ, +$1,737.60):**
> *Original vs. supplemented:* 28.0 SQ @ $362 → 32.8 SQ @ $362
> *Reason:* Quantity variance. Field measurement on 2026-04-23 returned 28.5 squares of net roof area; with the 15% complexity-weighted waste factor (cut-up roof with 4 valleys), bundle order requires 32.8 squares.
> *Supporting evidence:* Field measurement diagram (Appendix A) with eave / rake / ridge / valley LF and slope-by-slope SF, photos #02–#08 (slope corners showing measuring tape).
> *Requested action:* Please update Line RFG 240 from 28.0 SQ to 32.8 SQ.
>
> **Line 11 — RFG DECK Decking replacement (12 SF discovered, +$58.20):**
> *Original vs. supplemented:* 0 SF → 12 SF @ $4.85
> *Reason:* Supplemental discovery — rotted decking identified at tear-off on south slope, valley-adjacent. Photographed before re-deck and before underlayment install.
> *Supporting evidence:* Photos #21 (rotted decking exposed at tear-off), #22 (replacement panels installed), invoice from supplier dated 2026-04-26 (Appendix D).
> *Requested action:* Please add Line RFG DECK at 12 SF × $4.85 = $58.20.
>
> **Line 21 / 22 — Overhead & Profit 10/10 (+$1,930.00):**
> *Reason:* Three trades on the job — roofing, gutter detach & reset (Line 13), painting (Line 17, chimney touch-up after metal flash work). The 10/10 industry standard applies in Texas (TDI consistent with NAIC O&P guidance). Supplement.standard_op_rate is 10/10.
> *Supporting evidence:* Trade-coordination scope: roofing crew, separate gutter sub for detach/reset on the IWS install day, paint sub for the chimney chase finish.
> *Requested action:* Please add 10% overhead and 10% profit on the revised RCV per standard practice.
>
> ### 3. O&P JUSTIFICATION
>
> Three trades are required and were coordinated by the contractor:
>
> 1. **Roofing** — tear-off, decking replacement, IWS / underlayment / shingle / ridge cap install, ventilation (Acme crew)
> 2. **Gutter detach & reset** — required to access the eave for IWS and drip edge install (sub: All-Star Gutters, invoice in Appendix D)
> 3. **Painting** — chimney-chase paint touch-up after step flashing replacement (sub: ProFinish Painters, invoice in Appendix D)
>
> Industry standard 10% overhead + 10% profit applies. `supplement.standard_op_rate` = 10/10. State Farm TX historical practice consistent with this trigger.
>
> ### 4. RECOVERABLE DEPRECIATION RECOVERY REQUEST
>
> Original depreciation withheld: $3,582.00 (per carrier estimate dated 2026-04-22).
>
> Per policy language for recoverable depreciation release upon completion, Acme Roofing & Inspection LLC commits to providing the following on completion: final invoice, lien waiver, completion photos with timestamps, and signed homeowner satisfaction acknowledgment.
>
> Requested: release of $3,582.00 recoverable depreciation upon receipt of completion documentation, anticipated 2026-05-08.
>
> ### 5. ATTACHMENTS CHECKLIST (`supplement.standard_attachments[]`)
>
> ☑ Field inspection report dated 2026-04-23 (PDF, 24 photos with slope / pitch / measurement)
> ☑ Labeled photo log with captions (Appendix B)
> ☑ Field measurement diagram with eave / rake / ridge / valley LF (Appendix A)
> ☑ Code citations — IRC R905.1.2 (IWS), R905.2.8.5 (drip edge), Frisco amendment ventilation 1:150 (Appendix C)
> ☑ Current ABC Supply quote dated 2026-04-25 (Appendix D)
> ☑ All-Star Gutters detach/reset invoice (Appendix D)
> ☑ ProFinish Painters chimney-chase invoice (Appendix D)
> ☑ Signed contract / work authorization (Appendix E)
> ☑ City of Frisco permit + final inspection receipt (Appendix F)
>
> ### 6. ASSUMPTIONS FOOTER
>
> - `inspector.haag_id` resolved to 14782 Residential and surfaced in cover letter footer (denial-context guidance triggered: claim is not contesting an AI denial, but credentials surfaced regardless because supplement is >$2,500)
> - `supplement.standard_op_rate` defaulted to 10/10 (TX is an O&P state)
> - `appeals.recent_wins[]` matched on {TX, State Farm, hail} → cited the 2026-03-22 Frisco supplement settlement in the cover letter
> - `supplement.standard_attachments[]` resolved against config; full nine-item attachments checklist preserved
> - `service_area.licensed_states[]` contains TX ✓
> - `pricing.supplement_filing_flat` not surfaced (homeowner did not ask; service is bundled into the contractor's job recovery)
> - State explainability hook (Line 24) not triggered — the carrier's denial pattern was not machine-generated. Tennessee Bulletin 25-03 / Kentucky Bulletin 2026-01 not applicable (TX property)
