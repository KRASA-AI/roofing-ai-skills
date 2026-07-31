# Roofing Skills Eval — 2026-07-28

**Evaluated:** all **17 skills** in `skills/` (excluding `_shared/`) against rubric v1.0, cold, with personalization graded against `evals/fixtures/config.sample.yml`. Two firsts this cycle: **Sales Call Coach** (v1.0) is a brand-new skill and was graded for the first time; **Roof Inspection Report** shipped a new feature (v1.5 → v1.6, Composite Condition Index) mid-cycle and was graded as changed. Results persisted to `evals/results/2026-07-28/` (17 files).

## Averages

- Cold average (before this cycle's fixes): **8.45** (was 8.43 on 2026-07-20)
- After-improvement average: **8.61**
- No skill degraded relative to its own pre-improvement cold score this cycle.

## 1. Bottom 3 (improvement candidates) and what was fixed

All three were improved and replaced; none degraded on any dimension.

1. **Roof Inspection Report v1.6 → v1.7: 8.0 → 9.0 (+1.0).** The new Composite Condition Index (Section 3a) added last cycle had its own trigger rule backwards in practice — the residential storm-claim example ran the index (rule says skip it there) and the commercial PM example omitted it (rule says run it there). Moved the index into the commercial example with re-derived math (71/100, Grade C) and removed it from the residential one. Separately, the claim-recommendation rule (<5 strikes + no wind = "Not Recommended") was being misapplied to the residential example's South (3 strikes) and East (4 strikes) slopes, both mislabeled "Marginal" — relabeled and the executive summary/claim narrative and Documentation Completeness verdict updated to match. Biggest fix: the inspector identity was fabricated throughout — both examples signed as "Marcus Patel," who is `company.account_manager_name` in the fixture, not the inspector. Replaced with the real fixture inspector (William Reyes, HAAG #14782, 19 yrs), removed fabricated field names (`inspector.title/.email/.cell`, `branding.*`, `rates.repair_minimum`), and newly bound the fixture's inspector-credibility fields an adjuster actually wants (`professional_liability_carrier`, `independence_statement`, `expert_witness_history`, `haag_certifications[]`). output_quality 6→9, personalization 5→9.
2. **Commercial Prospect Researcher v1.5 → v1.6: 8.2 → 9.0 (+0.8).** This cycle's biggest score drop (was 9.0, the repo's #1 skill, on 2026-07-20) — an unchanged file that a closer adversarial pass caught two real defects in. The Est. Project Value $/sf basis was stated twice with two different, irreconcilable ranges (spec: "$4-6/sf recover"; footnote: "$5-7/sf recover"), and 3 of 5 worked-example prospect rows exceeded both. Replaced with a single reconciled three-tier basis (coating ~$3-5/sf, recover ~$5-8/sf, tear-off ~$9-13/sf) and verified all five rows now land inside the range matching their labeled scope. Separately — despite the skill's Purpose promising a brief "the sales rep can turn into email... without rewriting" — all three worked outreach briefs shipped with literal unresolved `{company.name}` / `{company.commercial_phone}` / `{company.commercial_email_from}` placeholders; a full-file grep confirmed zero real company values anywhere in the file. Resolved all three to the fixture's actual values. Also fixed `voice.commercial` → the fixture's actual top-level `voice_commercial` key. specificity 8→9, output_quality 7→9, personalization 7→9.
3. **Sales Call Coach v1.0 → v1.1 (first evaluation): 8.2 → 9.0 (+0.8).** New skill, graded cold for the first time. Its worked example scored two rebutted objections in the coaching scorecard (A2/R1/E1/C1 and A1/R2/E1/C1) while simultaneously logging both as "missed" in the Team Objection Log — the skill's own Step 1 rule defines "missed" as *no rebuttal attempted*, so a scored row can never also be missed; a sales manager reading the log would wrongly conclude the rep never responded. Fixed the log entries and added a hard consistency rule tying the log's status field to whether the scorecard actually scored the objection. Also fixed a silent 29.75→"30/100" rounding gap (now shown explicitly, round-half-up stated) and removed three fabricated config namespaces (`financing.partners[]`, `reviews.*`, `team.rep_roster[]` — none exist in the fixture; the $9.99% APR and 4.9/142-review figures were invented) in favor of an explicit Assumptions-footer pattern matching the rest of this repo. output_quality 7→9, personalization 5→9.

## 2. Skills that dropped since 2026-07-20 (cold re-grade, mostly unchanged files)

| Skill | 07-20 | 07-28 cold | Δ | Status |
|---|---:|---:|---:|---|
| Roof Inspection Report | 8.7 | 8.0 | −0.7 | **repaired → 9.0** |
| Commercial Prospect Researcher | 9.0 | 8.2 | −0.8 | **repaired → 9.0** |
| Insurance Supplement Writer | 8.9 | 8.4 | −0.5 | flagged, not fixed |
| Lead Response Automator | 8.6 | 8.3 | −0.3 | flagged, not fixed |
| Follow-Up Sequence | 8.5 | 8.3 | −0.2 | flagged, not fixed |
| Insurance Appeal Inspection Report | 8.6 | 8.4 | −0.2 | flagged, not fixed |
| Safety Toolbox Talk Generator | 8.7 | 8.5 | −0.2 | flagged, not fixed |
| Jobsite Content Repurposer | 8.8 | 8.7 | −0.1 | flagged, not fixed |

None of these drops came from file changes — every one is a previously-carried score that a deeper adversarial re-read caught new defects in (see each result file's `note:`). This is the same pattern that has driven every cold re-grade since 07-13: carry-forward grading systematically overstates scores on unchanged files.

## 3. Dimension-specific weaknesses across the library

- **Personalization remains the dominant weak dimension** (5-9 across the library, weighted average still visibly lower than the other five dimensions). Three recurring failure modes, worst-to-best: (a) **literal unresolved template placeholders** shipping in a worked example — newly caught this cycle in Commercial Prospect Researcher, a skill that had previously scored 9/9 on this exact dimension, showing that a passing grade doesn't guarantee no regression on re-read; (b) **fabricated config namespaces with confident, specific numbers** — `estimators[]` (Lead Response), `supplement.*` (Insurance Supplement), `safety.muster_points[]`/`nearest_hospitals[]` arrays vs. the fixture's scalars (Safety Toolbox, AI Jobsite Hazard), `crews[]` vs. the fixture's `crew_leads[]` (Crew Schedule), `financing.partners[]`/`reviews.*`/`team.rep_roster[]` (Sales Call Coach, fixed this cycle); (c) **the wrong company phone** (`469-555-0140` instead of the fixture's `469-555-0142`) still hard-coded into worked examples across roughly 7 skills, unfixed since it was first flagged 07-20.
- **Output_quality** is the second-most-common gate, almost always a worked-example arithmetic or logical-consistency defect rather than a missing example: Estimate Builder's $275 metal-deposit error and mislabeled $/yr column, Crew Schedule's self-contradicting buffer count, Insurance Appeal's 8yr8mo-vs-"8yr6mo" roof age, Follow-Up's 60-day date that lands 91 days out, Storm Canvassing's Day-1 crew-allocation contradiction, Tariff's fabricated USTR tariff standing in for the fixture's real Section 232 one. All five were flagged in the 07-20 cycle and remain unfixed.
- **Clarity, specificity, industry_fit, efficiency** are consistently strong (8-9 across nearly the entire library) — this repo's structural/instructional writing is not where defects concentrate; the defects concentrate in the worked examples that demonstrate the instructions.

## 4. Final ranking (after this cycle's improvements)

| Rank | Skill | Score | 07-20 |
|-----:|-------|------:|------:|
| 1 | Roof Inspection Report | 9.0 | 8.7 |
| 1 | Commercial Prospect Researcher | 9.0 | 9.0 |
| 1 | Sales Call Coach | 9.0 | — (new) |
| 4 | Material Order Calculator | 8.9 | 8.9 |
| 4 | Predictive Lead Scorer | 8.9 | 8.9 |
| 6 | Maintenance Plan Generator | 8.8 | 8.8 |
| 7 | Jobsite Content Repurposer | 8.7 | 8.8 |
| 8 | AI Jobsite Hazard Planner | 8.6 | 8.6 |
| 9 | Safety Toolbox Talk Generator | 8.5 | 8.7 |
| 10 | Crew Schedule Optimizer | 8.4 | 8.4 |
| 10 | Estimate Builder | 8.4 | 8.4 |
| 10 | Insurance Appeal Inspection Report | 8.4 | 8.6 |
| 10 | Insurance Supplement Writer | 8.4 | 8.9 |
| 10 | Tariff & Price Adjustment Communicator | 8.4 | 8.4 |
| 15 | Follow-Up Sequence | 8.3 | 8.5 |
| 15 | Lead Response Automator | 8.3 | 8.6 |
| 15 | Storm Canvassing Prioritizer | 8.3 | 8.3 |

## Recommendations for next cycle

1. **Run the library-wide phone/name identity sweep that's been recommended for two cycles running.** Replace `469-555-0140` → `469-555-0142` and any remaining `& Inspection LLC` → `& Restoration LLC` wherever they appear. Pure, low-risk win, still not done.
2. **Fix the five specific, already-diagnosed arithmetic/logic defects** that keep getting flagged but never scheduled: Estimate Builder's metal deposit + $/yr header, Crew Schedule's buffer contradiction, Insurance Appeal's roof-age date math, Follow-Up's 60-day date, Storm Canvassing's Day-1 split. Each is a one-line correction per the 07-20 and 07-28 notes.
3. **Rewrite the remaining phantom config schemas against the real fixture**, using the now-twice-proven playbook (Commercial Prospect, Roof Inspection, Sales Call Coach all went 0.8-1.0 points on exactly this fix): Lead Response's `estimators[]` → `crew_leads[]`, Safety Toolbox's and AI Jobsite Hazard's array `safety.*` → the fixture's scalar fields, Crew Schedule's `crews[]` → `crew_leads[]`, Insurance Supplement's `supplement.*` namespace.
4. **Add a pre-ship placeholder check.** Commercial Prospect Researcher shipped three worked briefs with literal `{company.name}`-style unresolved template syntax and still scored 9/9 on personalization on 2026-07-20 — the prior review missed it. A simple grep for `{` / unresolved-brace syntax in worked-example sections before finalizing a skill would have caught this immediately.
5. **`test-cases/` is still empty after 8 eval cycles.** Seeding it with per-skill input fixtures would let arithmetic be regression-tested programmatically instead of re-derived by hand every week — the single highest-leverage infrastructure gap left in this pipeline.

## Verification

Every `overall` score in `results/2026-07-28/*.yml` recomputes correctly from its six weighted dimension scores (0.20×4 dims + 0.10×2 dims). The 17 skills map 1:1 to 17 result files. Cold average 143.7/17 = 8.4529 → **8.45**; after-improvement average (three bottom-3 scores replaced with their post-fix values) 146.3/17 = 8.6059 → **8.61**. All three bottom-3 improvements were re-graded independently by the agent that made the edit, re-deriving every changed number and re-checking every changed personalization binding against the fixture before the replacement was finalized; none show a regressed dimension versus their own pre-fix cold score.

**Not committed.** All 17 result files, this summary, the three skill edits (`roof-inspection-report.md` v1.6→v1.7, `commercial-prospect-researcher.md` v1.5→v1.6, `sales-call-coach.md` v1.0→v1.1), and the changelog entry are written to the working tree, consistent with this task's read-mostly remit — the repo's daily-sync job commits tracked changes.
