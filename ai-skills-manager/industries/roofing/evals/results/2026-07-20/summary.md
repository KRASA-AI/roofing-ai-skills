# Roofing Skills — Eval Summary, 2026-07-20

Automated Skill Evaluator cycle. **16 skills** in `skills/` (excluding `_shared/`) graded against rubric v1.0. This is the first cycle to (a) grade **personalization against the committed config fixture** (`evals/fixtures/config.sample.yml`, added 07-13) as that fixture's own header instructs, and (b) grade **every skill cold** rather than carrying dimension scores forward.

## Headline

- **Honest cold average: 8.43** (16 skills) — down from the carried-forward **8.95** reported on 07-13. **After repairing the bottom 3: 8.66.**
- **This is not a regression in the skills. It is the measurement finally acquiring a ground truth.** For five cycles, personalization was capped at 8 because no config fixture existed, and every other dimension was carried forward. The fixture was committed last cycle specifically to *lift* personalization — the projection was "~9.1–9.2 average." Grading against it did the opposite: it revealed that **the library's worked examples were personalized against a config schema that does not exist.** The fixture uses `crew_leads[]`; skills bind `crews[]`. The fixture uses scalar `safety.muster_point`; skills bind array `safety.muster_points[]`. The fixture has no `financing`, `reviews`, `estimators[]`, `maintenance.*`, or `supplement.*` blocks; skills present all of them as "resolved config."
- **The single skill that scored 9.0 on personalization — Commercial Prospect Researcher — is the one whose worked example was written *after* the fixture was committed.** It binds the fixture's real case studies, certs, ZIPs and crew capacity, and gracefully logs the deliberately-absent medical-office vertical. It is the proof that the ceiling is reachable; the rest of the library simply has not been rewritten against the real schema yet.
- **Two concrete, systemic worked-example defects run across the library** (details below): a wrong company phone (`469-555-0140` vs the fixture's `0142`) in **8 skills** — two of which print *both* numbers in a single document — and the wrong company name (`Acme Roofing & Inspection LLC` vs `& Restoration LLC`) in the two admin skills.
- **Cold grading also caught a fresh crop of arithmetic/consistency defects that carry-forward had masked for cycles** — including three that would have gone straight to a carrier or a customer.
- **Bottom 3 repaired, none degraded:** Maintenance Plan **7.4 → 8.8**, Predictive Lead Scorer **7.7 → 8.9**, Insurance Supplement Writer **7.9 → 8.9**.
- **One new skill graded for the first time:** AI Jobsite Hazard Planner (v1.0) enters at **8.6** — a strong, OSHA-accurate net-new safety skill held back only by the same phantom-`safety.*`-schema personalization issue as its sibling safety skill.

## What the cold grade found (by defect class)

### 1. Personalization graded against the real fixture — the big driver

Twelve skills that had sat at personalization 8 ("no fixture, graded on graceful absence") were re-graded against `config.sample.yml`. Most did **not** rise to 9; several fell to 5–7 because their worked examples bind field names the fixture doesn't carry while ignoring the ones it does:

| Skill | Names (absent from fixture) | Should use (present in fixture) |
|---|---|---|
| crew-schedule-optimizer | `crews[]`, `weather_rules`, `shift.*`, `preferred_suppliers` | `crew_leads[]`, `suppliers.preferred[]` |
| lead-response-automator | `company.estimators[]`, `reviews.*`, `crm.system` | `crew_leads[]`, `warranty`, `service_area.zip_codes` |
| safety-toolbox / ai-jobsite-hazard | `safety.muster_points[]`/`nearest_hospitals[]` (arrays), `crew_leads[].language_preference` | scalar `safety.muster_point`/`nearest_hospital`, `crew_leads[].language` |
| maintenance-plan-generator | `maintenance.*` (base_rate_per_square, inspection_rate_flat, tiers[]) | `rates.maintenance_plan_annual_residential`/`_commercial_per_sf` |
| insurance-supplement-writer | `supplement.*`, `company.ein_last4` | `rates.*`, `inspector.*`, `appeals.recent_wins[]` |
| tariff-price-adjuster | fabricated asphalt tariff | `market_conditions.active_tariffs` (Section 232, metal-scoped) |
| follow-up-sequence | `financing.partners[]`, `reviews`, `team.rep_roster[]` | `warranty.manufacturer_tiers[]`, `company.phone` |
| roof-inspection-report | `branding.*`, and the inspector identity (used the account manager) | `inspector.*` (William Reyes #14782) |

Personalization is a 10%-weighted dimension, so the per-skill overall hit is ~0.2–0.4 each — but it is *pervasive*, which is why the average moved a full half-point.

### 2. Arithmetic / consistency defects caught by cold grading

- **Insurance Supplement Writer** — the 24-line summary table asserted a **$7,400** subtotal; lines 1–20 actually sum to **$5,874.40**, and the O&P line was computed **circularly** ("10% on revised RCV", a total that already includes O&P). *This goes to a carrier underwriter whose job is to find a number that disagrees with itself.* **(repaired)**
- **Predictive Lead Scorer** — **7 of 10** ranked composites did not decompose to their own criterion rows under the skill's own weights, violating its explicit "no black-box numbers" guarantee; one error misclassified an 84-composite **Hot** lead as **Warm** (a live call demoted to a drip). **(repaired)**
- **Maintenance Plan Generator** — residential Bronze was "`max($285, $420) = $420 → rounded to $285 (lower of the two)`": `max()` returns the greater value, "$420 rounded to $285" is not rounding, and the whole Silver/Gold ladder was built on the wrong $285. **(repaired)**
- **Estimate Builder** — metal-tier deposit "25% ($14,003 on Better)"; 25% × $54,910 = **$13,728** ($275 off, in a contract number); the metal $/yr column is headed "40-yr proxy" but two rows were computed on 50/75 yr. *(flagged; not in bottom 3)*
- **Crew Schedule Optimizer** — Risk Register J1 "6-day buffer" to a deadline 5 days out, then "push to Thu (still 1 day buffer)". *(flagged)*
- **Insurance Appeal** — roof age is 8 yr 8 mo by its own dates but stated as "8 years 6 months" / "8.5 years". *(flagged)*
- **Follow-Up Sequence** — "good through 2026-07-22 (60 days from delivery)"; 60 days from 2026-04-22 is 2026-06-21. *(flagged)*
- **Storm Canvassing** — Day-1 crew allocation contradicts itself (5/3 in the schedule vs 3/2/3 in the roster). *(flagged)*
- **Tariff Price Adjuster** — Template A ships the wrong phone, email domain (`acmeroofing.com` vs `acmeroofs.com`) and street address. *(flagged)*

### 3. The systemic worked-example identity defect

A cross-library grep confirms it isn't cosmetic:

- **`469-555-0140`** (wrong; fixture is `0142`) appears in **8 skills**: maintenance-plan, follow-up, tariff, lead-response, insurance-appeal, insurance-supplement, ai-jobsite-hazard, safety-toolbox. **follow-up and lead-response print *both* 0140 and 0142 in the same document.**
- **`Acme Roofing & Inspection LLC`** (wrong; fixture is `& Restoration LLC`) in insurance-appeal and insurance-supplement.
- Wrong email domain / address in tariff.

These are exactly the fields a rep pastes and sends. They were invisible under carry-forward because personalization was never graded against a real company record.

## Improvements made (bottom 3, all replaced, none degraded)

1. **Maintenance Plan Generator 7.4 → 8.8 (+1.4), v1.6 → v1.7.** Repaired the Bronze contradiction and re-anchored Bronze on the fixture's `rates.maintenance_plan_annual_residential` ($349); Silver ($1,075) and Gold ($2,380) now derive cleanly with round-half-up stated for the $1,072.50 boundary, and every monthly/prepay figure reconciles. Bound the commercial per-sf base to `rates.maintenance_plan_annual_commercial_per_sf` ($0.09, was a hard-coded $0.10) and recomputed the Silver/Gold/Platinum cascade ($6,650 / $19,625 / $45,250), removing the old "adjusted to" editorial fudges so every number is a true formula output; updated all downstream Gold references (ROI $19,625, 13-yr $255,125). Removed fabricated credentials (JM Peak Advantage, OC Platinum, $5M GL, EMR 0.81) and bound `certifications[]` / `commercial.certifications[]`; corrected phone and company name in **both** the residential header and footer (the header was a residual caught on re-grade and fixed). output_quality 6→9, clarity/specificity/industry_fit 8→9, personalization 5→7.

2. **Predictive Lead Scorer 7.7 → 8.9 (+1.2), v2.1 → v2.2.** Rebuilt the 25-lead example so every composite decomposes under the storm weights (W35/A20/N15/R15/$15 ÷10): all ten now re-derive (97/91/88/84/78/76/75/73/66/36), the ranked table is monotonic, a full decomposition table is shown for all top-10 rows, the mis-scored Hot lead (Lisa Fortuna, 84) is correctly a live call, and the tier summary + routing counts reconcile to 4 Hot / 13 Warm. Replaced the out-of-area ZIP 75071 with the fixture's 75035 in the title, instruction and overlay; labelled fields `[config]` vs `[batch]`; bound the fixture inspector (William Reyes). output_quality 4→9, personalization 6→8.

3. **Insurance Supplement Writer 7.9 → 8.9 (+1.0), v2.2 → v2.3.** Made the flagship supplement reconcile end to end: O&P is now 10/10 (20%) on the pre-O&P base $20,694.40 = $4,138.88; total supplement $10,013.28; revised RCV $24,833.28; ACV $21,251.28; net payable $19,751.28 — with an explicit reconciliation line and the cover letter, diagnostic total and homeowner summary all updated to agree. Replaced the fabricated `{State Farm, $7,200}` recent-win match with a carrier-neutral reference to the fixture's real `{TX, Allstate, granular loss}` win (no invented dollar figure); removed the fabricated "EIN ending 8842" (flagged as an absent config field rather than invented); corrected company name and phone. output_quality 5→9, personalization 6→8.

## Final ranking (after improvements)

| Rank | Skill | Score | 07-13 (carried) | Note |
|-----:|-------|------:|----------------:|------|
| 1 | Commercial Prospect Researcher | 9.0 | 8.9 | only skill to *gain* — example written against the real fixture |
| 2 | **Predictive Lead Scorer** | **8.9** | 9.0 | cold 7.7 → repaired |
| 2 | Material Order Calculator | 8.9 | 8.9 | fixes from 07-13 hold |
| 2 | **Insurance Supplement Writer** | **8.9** | 8.9 | cold 7.9 → repaired |
| 5 | Jobsite Content Repurposer | 8.8 | 8.9 | |
| 5 | **Maintenance Plan Generator** | **8.8** | 8.9 | cold 7.4 → repaired |
| 7 | Roof Inspection Report | 8.7 | 8.9 | inspector identity fabricated |
| 7 | Safety Toolbox Talk Generator | 8.7 | 8.9 | phantom `safety.*` schema |
| 9 | AI Jobsite Hazard Planner | 8.6 | — | **first eval** (new skill) |
| 9 | Insurance Appeal Inspection Report | 8.6 | 8.8 | roof-age inconsistency |
| 9 | Lead Response Automator | 8.6 | 8.9 | phantom `estimators[]` engine |
| 12 | Follow-Up Sequence | 8.5 | 9.0 | 60-day date error |
| 13 | Estimate Builder | 8.4 | 9.1 | metal deposit + financing fabrication |
| 13 | Tariff & Price Adjustment Communicator | 8.4 | 9.0 | wrong contact info; ignores real tariff |
| 13 | Crew Schedule Optimizer | 8.4 | 9.2 | buffer inconsistency; phantom `crews[]` |
| 16 | Storm Canvassing Prioritizer | 8.3 | 8.9 | Day-1 allocation contradiction |

## Score distribution (after improvements)

- **9.0:** 1 · **8.9:** 3 · **8.8:** 2 · **8.7:** 2 · **8.6:** 3 · **8.5:** 1 · **8.4:** 3 · **8.3:** 1

The band widened from the artificial 0.4-point compression of 07-13 (which was an artifact of carry-forward) to a real 0.7-point spread that reflects genuine per-skill quality.

## Persistent weaknesses

1. **The library's worked examples are personalized against a phantom config schema.** This is the dominant finding and it is *fixable per skill*: Commercial Prospect proves a fixture-bound example scores 9. Every other skill needs its example rewritten to bind the real fixture field names.
2. **A wrong company phone and name are hard-coded into 8+ worked examples**, two with internal contradictions. This is a mechanical, high-value, one-afternoon library-wide sweep.
3. **`test-cases/` is still empty.** A per-skill input fixture would let the next cycle regression-test the arithmetic automatically instead of re-deriving it by hand.

## Recommendations for next cycle

1. **Run a library-wide identity sweep first.** Replace `469-555-0140` → `0142`, `Acme Roofing & Inspection LLC` → `& Restoration LLC`, `acmeroofing.com` → `acmeroofs.com` everywhere. Pure win, no judgment calls, lifts personalization on ~8 skills.
2. **Rewrite the highest-traffic skills' examples against the real fixture schema**, starting with the former top scorers that fell hardest: Crew Schedule (`crews[]`→`crew_leads[]`), Lead Response (`estimators[]`→`crew_leads[]`), Safety Toolbox (array→scalar `safety.*`). Use Commercial Prospect as the template.
3. **Fix the flagged (non-bottom-3) arithmetic defects**: Estimate Builder metal deposit, Crew Schedule buffer, Insurance Appeal roof age, Follow-Up 60-day date, Storm Canvassing Day-1 split. Each is a one-line correction.
4. **Point the landscape-monitor's tariff logic at `market_conditions.active_tariffs`** so Tariff and Material Order stop fabricating a tariff and instead apply the real Section 232 (metal-scoped, 7%) one.
5. **Seed `test-cases/`** with per-skill input fixtures now that a config fixture exists to run them against.

## Verification pass

Every number in this cycle was re-derived programmatically and, for the bottom 3, re-graded by a separate adversarial agent after the edits. Confirmed: all 16 weighted `overall` scores recompute correctly; the 16 skills map 1:1 to 16 result files; cold average = 8.4313 → **8.43**, after-improvement average = 8.6562 → **8.66**; the three improved examples reconcile end-to-end (predictive composites 97/91/88/84/78/76/75/73/66/36; maintenance $349/$1,075/$2,380 residential and $6,650/$19,625/$45,250 commercial; supplement line items $5,874.40 + O&P $4,138.88 = $10,013.28 → RCV $24,833.28 → ACV $21,251.28 → net $19,751.28). The re-grade caught three residual inconsistencies introduced during editing (predictive routing counts + a leftover 75071 in the title; maintenance's residential header still carrying the old phone/name/cert); all three were fixed before finalizing.

**Not committed.** All three skill edits, this summary, the 16 result files in `evals/results/2026-07-20/`, and the changelog entry are written to the working tree but left uncommitted, consistent with this task's read-mostly remit — the repo's daily-sync job commits tracked changes.
