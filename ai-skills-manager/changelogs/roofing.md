# Roofing Skills — Eval & Improvement Changelog

Maintained by the Skill Evaluator scheduled task. Most recent entry first.

---

## 2026-07-20 14:43 (automated eval cycle)

**Evaluated:** all **16 skills** in `skills/` (excluding `_shared/`) against rubric v1.0 — including the new **AI Jobsite Hazard Planner** (v1.0), graded for the first time. Two firsts this cycle: personalization was graded **against the committed fixture** (`evals/fixtures/config.sample.yml`) as the fixture's header instructs, and **every skill was graded cold** rather than carrying dimension scores forward. Results persisted to `evals/results/2026-07-20/`.

**Averages:** honest cold average **8.43** (16 skills), **8.66 after repairing the bottom 3**. The 07-13 figure of 8.95 was a carry-forward artifact.

**Headline finding — the config fixture, committed last cycle to *lift* personalization, instead revealed that the library's worked examples are personalized against a config schema that does not exist.** The fixture uses `crew_leads[]`; skills bind `crews[]`. The fixture has scalar `safety.muster_point`; skills bind array `safety.muster_points[]`. The fixture has no `financing`, `reviews`, `estimators[]`, `maintenance.*` or `supplement.*` blocks; skills present all of them as "resolved config." The one skill that scored 9.0 on personalization — **Commercial Prospect Researcher** — is the only one whose example was written *after* the fixture existed. Two systemic worked-example defects were confirmed by grep: the wrong company phone (`469-555-0140` vs fixture `0142`) in **8 skills** (two printing *both* numbers in one document), and the wrong company name (`& Inspection LLC` vs `& Restoration LLC`) in the two admin skills.

**Cold grading also caught arithmetic/consistency defects carry-forward had masked**, three of them in the bottom 3 (repaired) and five more flagged for next cycle (Estimate Builder metal deposit $14,003 vs $13,728; Crew Schedule buffer day-count; Insurance Appeal roof age 8yr8mo vs "8yr6mo"; Follow-Up 60-day date landing on 2026-07-22 not 2026-06-21; Storm Canvassing Day-1 crew split).

**Improvements made (bottom 3, all replaced their originals, none degraded):**

1. **Maintenance Plan Generator v1.6 → v1.7: 7.4 → 8.8 (+1.4).** Repaired the residential Bronze contradiction (`max($285,$420)=$420 → rounded to $285, lower of the two` — max returns the greater, and $420 doesn't round to $285) and re-anchored Bronze on the fixture's `rates.maintenance_plan_annual_residential` ($349); Silver ($1,075) / Gold ($2,380) now derive cleanly and every monthly/prepay reconciles. Commercial per-sf bound to `rates.maintenance_plan_annual_commercial_per_sf` ($0.09, was hard-coded $0.10); Silver/Gold/Platinum recomputed ($6,650/$19,625/$45,250) with the "adjusted to" fudges removed; downstream Gold refs updated. Fabricated credentials (JM Peak Advantage, OC Platinum, $5M GL, EMR 0.81) removed and bound to `certifications[]`; phone/company-name corrected in both header and footer. output_quality 6→9, clarity/specificity/industry_fit 8→9, personalization 5→7.
2. **Predictive Lead Scorer v2.1 → v2.2: 7.7 → 8.9 (+1.2).** Rebuilt the 25-lead example so all 10 composites decompose under the storm weights (97/91/88/84/78/76/75/73/66/36), the ranking is monotonic, a full decomposition table is shown for every top-10 row, the mis-scored 84-composite Hot lead (Lisa Fortuna) is correctly a live call, and the tier/routing counts reconcile to 4 Hot / 13 Warm. Out-of-area ZIP 75071 replaced with the fixture's 75035; fields labelled `[config]` vs `[batch]`; inspector bound to William Reyes. output_quality 4→9, personalization 6→8.
3. **Insurance Supplement Writer v2.2 → v2.3: 7.9 → 8.9 (+1.0).** Made the 24-line supplement reconcile: O&P is now 10/10 (20%) on the pre-O&P base $20,694.40 = $4,138.88; total $10,013.28; revised RCV $24,833.28; ACV $21,251.28; net $19,751.28 — with a printed reconciliation line and cover letter/diagnostic/homeowner summary all agreeing (was a $7,400 subtotal against lines that sum to $5,874.40, with a circular O&P). Replaced the fabricated `{State Farm, $7,200}` recent-win with the fixture's real carrier-neutral `{TX, Allstate, granular loss}`; removed the fabricated "EIN ending 8842"; corrected company name and phone. output_quality 5→9, personalization 6→8.

**Final ranking (after improvements):**

| Rank | Skill | Score | 07-13 (carried) |
|-----:|-------|------:|----------------:|
| 1 | Commercial Prospect Researcher | 9.0 | 8.9 |
| 2 | Predictive Lead Scorer | 8.9 | 9.0 |
| 2 | Material Order Calculator | 8.9 | 8.9 |
| 2 | Insurance Supplement Writer | 8.9 | 8.9 |
| 5 | Jobsite Content Repurposer | 8.8 | 8.9 |
| 5 | Maintenance Plan Generator | 8.8 | 8.9 |
| 7 | Roof Inspection Report | 8.7 | 8.9 |
| 7 | Safety Toolbox Talk Generator | 8.7 | 8.9 |
| 9 | AI Jobsite Hazard Planner | 8.6 | — (new) |
| 9 | Insurance Appeal Inspection Report | 8.6 | 8.8 |
| 9 | Lead Response Automator | 8.6 | 8.9 |
| 12 | Follow-Up Sequence | 8.5 | 9.0 |
| 13 | Estimate Builder | 8.4 | 9.1 |
| 13 | Tariff & Price Adjustment Communicator | 8.4 | 9.0 |
| 13 | Crew Schedule Optimizer | 8.4 | 9.2 |
| 16 | Storm Canvassing Prioritizer | 8.3 | 8.9 |

**Persistent weaknesses:** (1) worked examples personalized against a phantom config schema — the dominant, per-skill-fixable issue; (2) a wrong company phone/name hard-coded into 8+ examples, two self-contradicting; (3) `test-cases/` still empty.

**Recommendations for next cycle:** (1) library-wide identity sweep (`0140`→`0142`, `& Inspection`→`& Restoration`, `acmeroofing.com`→`acmeroofs.com`) — a pure win lifting ~8 skills; (2) rewrite the former top scorers' examples against the real fixture schema (Crew Schedule `crews[]`→`crew_leads[]`, Lead Response `estimators[]`→`crew_leads[]`, Safety Toolbox array→scalar `safety.*`), using Commercial Prospect as the template; (3) fix the five flagged arithmetic defects; (4) point tariff logic at the real `market_conditions.active_tariffs` (Section 232, metal-scoped 7%); (5) seed `test-cases/`.

**Verification:** every score recomputed correctly (cold 8.4313 → 8.43; after 8.6562 → 8.66); 16 skills map 1:1 to 16 result files; the three improved examples were re-derived programmatically and re-graded by a separate adversarial agent, which caught three residual editing inconsistencies (predictive routing counts + a leftover 75071 in the title; maintenance's residential header) — all fixed before finalizing. **Not committed** — edits, results, summary and this entry are in the working tree, left for the repo's daily-sync job.

---

## 2026-07-13 (automated eval cycle)

**Evaluated:** all 15 skills in `skills/` (excluding `_shared/`) against rubric v1.0. One external change since 07-06: the landscape monitor shipped **Estimate Builder v1.6** earlier the same day. The other 14 opened byte-identical. Results persisted to `evals/results/2026-07-13/`.

**Averages:** 8.93 (07-06 after-improvements) → **8.95 after improvements** (new repo high). Four skills edited, four improved, **none degraded**.

**Headline finding — carry-forward grading was concealing real defects.** Since 06-08, unchanged skills have inherited their dimension scores rather than being re-derived. This cycle cold-graded the three bottom skills instead, and all three were scored too high. Two of the defects were arithmetic errors sitting inside worked examples that a user would paste straight into a supplier order or a carrier appeal:

- **Material Order Calculator** (carried 8.8, honest cold grade **8.6**) — the worked example computed the nail line off *bundles* (97 × 80 × 5 ≈ 38,800 nails → 10 boxes) while the skill's own rule 8 and coverage-constants table both specify nails per *shingle* at ~80 shingles per *square*. Correct answer: **2 boxes**. A ~5x fastener over-order, contradicting the spec printed directly above it. The tear-off debris rule separately multiplied squares by `dump.bundles_per_yard` — a divisor — under-sizing the dumpster ~2x.
- **Insurance Appeal Inspection Report** (carried 8.8, honest cold grade **8.4**) — the appeals success statistic appeared in three different forms in a single report ("31 of 38 (82%)" vs "of 31 appeals, 25 overturned (82%)"; 25/31 = 80.6%), the example numbered its sections 1–9 against a spec defining 10 so every cross-reference pointed at the wrong section, and remaining useful life read "14–16 years" in two places and "14.5 years" in two others. In a document whose entire purpose is to catch a carrier's numbers disagreeing with themselves.
- **Estimate Builder** (recorded 9.1, cold grade **8.9**) — v1.6, shipped hours earlier by the landscape monitor, added a new customer-facing output section (Transparent-Quote Benchmark) and demonstrated it in neither worked example.

**Improvements made (all four replaced their originals):**

1. **Material Order Calculator v1.3 → v1.4: 8.6 → 8.9 (+0.3).** Nail line rebuilt per-shingle (28 sq × 80 × 6 = 13,440 ÷ ~7,200/box → 2 boxes); debris rule corrected to divide by `bundles_per_yard` with an explicit anti-inversion warning; a ROOF GEOMETRY basis block added so every linear-foot line reconciles against one set of numbers (the I&W line now resolves to 3 rolls, not 2); Fastest-path minimum-viable-input block with `[blocking]`/`[from config]`/`[inferred]` annotations. output_quality 8→9, efficiency 8→9.
2. **Insurance Appeal Inspection Report v1.1 → v1.2: 8.4 → 8.8 (+0.4).** All three inconsistencies reconciled; example renumbered to the spec's 10 sections; a 5-point **Consistency Check** added to output requirements so the failure mode cannot recur. clarity 8→9, output_quality 8→9. Efficiency deliberately held at 8 — field inspection data cannot be defaulted without fabricating evidence, and a fabricated appeal report is worse than none.
3. **Commercial Prospect Researcher v1.4 → v1.5: 8.8 → 8.9 (+0.1).** Efficiency pass, completing the Fastest-path treatment across the former 8.8 tier. Territory (a bare zip code) is now the sole blocking input; "reason to call" was removed as an ask because trigger discovery *is* Layer 3 of the skill's own framework — asking the rep for it inverts the skill. efficiency 8→9.
4. **Estimate Builder v1.6 → v1.7: 8.9 → 9.1 (regression repaired).** The missing Transparent-Quote Benchmark instance written into the asphalt example — a $14,900 AI-platform quote benchmarked against the $17,794 Good tier, a six-row Value Delta table where every row is independently verifiable (license #, COI, GAF certification lookup, permit #, warranty document, phone number), and a Pricing-Flags line telling the estimator to hold the number because the delta is scope, not margin. output_quality restored to 9.

**Also committed: `evals/fixtures/config.sample.yml`** — the fixture that five consecutive cycles named as the single highest-leverage move in the repo. It binds every field the library references and deliberately omits four (`company.logo_path`, `pricing.commercial_inspection_flat`, `suppliers.preferred[].last_pricing`, and any medical-office case study) so graceful-degradation behavior stays under test. Personalization was **not** re-graded against it this cycle — changing the grading basis and the skills in the same pass would make the delta unreadable. From 07-20, personalization is graded against the fixture, which should lift ~12 skills from 8 toward 9–10.

**Scores (after improvements):**

| Skill | Score | Prior (07-06) | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 (dipped to 8.9 intra-cycle, repaired) |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Tariff & Price Adjustment Communicator | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| Lead Response Automator | 8.9 | 8.9 | 0.0 |
| Jobsite Content Repurposer | 8.9 | 8.9 | 0.0 |
| Maintenance Plan Generator | 8.9 | 8.9 | 0.0 |
| Safety Toolbox Talk Generator | 8.9 | 8.9 | 0.0 |
| Storm Canvassing Prioritizer | 8.9 | 8.9 | 0.0 |
| **Material Order Calculator** | **8.9** | 8.8 | **+0.1** |
| **Commercial Prospect Researcher** | **8.9** | 8.8 | **+0.1** |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |

**Recommendations carried to next cycle:** (1) grade personalization against the new fixture; (2) **cold-re-grade three carried-forward skills per cycle on rotation, starting with the three highest scorers** — they have been carried longest and never checked adversarially for the arithmetic-in-worked-example failure mode that surfaced twice today; (3) seed `test-cases/` now that a config fixture exists to run them against; (4) add a spec-to-example consistency gate to the landscape-monitor task, so a new output section cannot ship without a worked instance.

---

## 2026-07-06 (automated eval cycle)

**Evaluated:** all 15 skills in `skills/` (excluding `_shared/`) against rubric v1.0. Since 2026-06-29, **no skill file changed anywhere in the repo** (git diff vs the 06-29 improvement commit shows only README regeneration and a share-image sync) — all 15 opened byte-identical to their last evaluated versions and carried their dimension scores forward as the pre-improvement baseline. Results persisted to `evals/results/2026-07-06/`.

**Averages:** prior cycle 8.91 (15 skills) → this-cycle baseline 8.91 → **after improvements 8.93** (new repo high). No skill degraded. Floor unchanged at 8.8, but the count of skills at the floor dropped from 6 to 3; the 8.9 tier grew from 4 skills to 7.

**Scores (after improvements):**

| Skill | Score | Prior (06-29) | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Tariff & Price Adjustment Communicator | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| Lead Response Automator | 8.9 | 8.9 | 0.0 |
| Jobsite Content Repurposer | 8.9 | 8.9 | 0.0 |
| **Maintenance Plan Generator** | **8.9** | 8.8 | **+0.1** |
| **Safety Toolbox Talk Generator** | **8.9** | 8.8 | **+0.1** |
| **Storm Canvassing Prioritizer** | **8.9** | 8.8 | **+0.1** |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| Material Order Calculator | 8.8 | 8.8 | 0.0 |
| Commercial Prospect Researcher | 8.8 | 8.8 | 0.0 |

**Improvements made (bottom 3 by pre-improvement score, all targeting efficiency):** Six skills entered tied at 8.8 with the identical profile (`clarity 9 / specificity 9 / industry_fit 9 / output_quality 9 / personalization 8 / efficiency 8`). With output_quality and industry_fit already closed library-wide and personalization blocked by eval design, **efficiency** is the only skill-fixable lever. Applied the proven Lead-Response "minimum viable input + one-question triage" pattern to the three where it is most honest:

- **Maintenance Plan Generator** v1.5 → v1.6. Added a "Fastest path — minimum viable input" block establishing roof material + age + city as the only blocking inputs (geometry sized from a market-typical footprint when absent, hazards inferred from ZIP + `service_area.hail_zones[]`, customer type defaults Residential, rates/tiers/warranty/billing from config); re-annotated inputs `[blocking]/[from config]/[inferred]`; tightened Efficiency notes. Purely additive. efficiency 8 → 9, overall 8.8 → **8.9**, replaced.
- **Safety Toolbox Talk Generator** v1.3 → v1.4. Added the Fastest-path block (job-site type the only blocking input; topic auto-picks from weather + Stand-Down/Heat-NEP calendar; crew/language/weather/muster/hospital from config with a "confirm on-site" weather default) plus a new Efficiency-notes section and input annotations. Purely additive; Heat-NEP and Stand-Down logic untouched. efficiency 8 → 9, overall 8.8 → **8.9**, replaced.
- **Storm Canvassing Prioritizer** v1.3 → v1.4. Added the Fastest-path block (storm footprint the only blocking input; service area/roster/thresholds/tone from config; property-intelligence depth auto-degrades; city-headline accepted with a degraded-confidence tag) plus an Efficiency-notes block and input annotations. Purely additive; five-layer framework and pre-storm orchestration layer untouched. efficiency 8 → 9, overall 8.8 → **8.9**, replaced.

**Persistent weaknesses:** **personalization** is now the sole binding constraint (8 on 10 of 15 skills) — still **no committed `config.yml` fixture**, so it is graded on absent-config handling (ceiling 8) rather than live binding. **efficiency 8** now survives only on the three unimproved 8.8 skills; Insurance Appeal's 8 is arguably a legitimate floor (field inspection data can't be defaulted), while Material Order and Commercial Prospect Researcher already carry partial triage language and are the next targets. `test-cases/` remains empty.

**Recommendations for next cycle:** (1) commit a sample `config.yml` fixture — five cycles running, now essentially the only lever left to move the headline; (2) finish the efficiency pass on Material Order Calculator and Commercial Prospect Researcher, leaving Insurance Appeal at its legitimate floor; (3) seed `test-cases/` for regression grading; (4) grade future content edits strictly for spec-to-example consistency to defend against regression.

---

## 2026-06-29 00:14 UTC (automated eval cycle)

**Evaluated:** all 15 skills in `skills/` (excluding `_shared/`) against rubric v1.0. Since 2026-06-22, exactly one skill file changed via other automation — `insurance-supplement-writer.md` (v2.1→v2.2, landscape-monitor commit `3842c8b`); the other 14 were byte-identical and carried forward. Results persisted to `evals/results/2026-06-29/`.

**Averages:** prior cycle 8.88 (15 skills) → this-cycle baseline 8.88 → **after improvements 8.91**. No skill degraded. Floor rose from 8.7 to 8.8; the whole library now sits in a 0.4-point band (8.8–9.2).

**Scores (after improvements):**

| Skill | Score | Prior (06-22) | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 |
| Tariff & Price Adjustment Communicator | 9.0 | 8.8 | **+0.2** |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| Lead Response Automator | 8.9 | 8.8 | **+0.1** |
| Jobsite Content Repurposer | 8.9 | 8.7 | **+0.2** |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| Material Order Calculator | 8.8 | 8.8 | 0.0 |
| Storm Canvassing Prioritizer | 8.8 | 8.8 | 0.0 |
| Commercial Prospect Researcher | 8.8 | 8.8 | 0.0 |

**Improvements made (bottom 3 by pre-improvement score):**

- **Tariff & Price Adjustment Communicator** v2.2 → v2.3 (target: output_quality — the only 8.8 skill with a sub-9 0.2-weight dimension). Worked the last two skeleton-only templates: **Template C** (Frisco "What Roofing Costs Right Now — May 2026" page with populated ranges, 40/38/22 cost split, four homeowner Q&As, and the required plain-HTML CMS variant) and **Template E** (28-sq Frisco three-material lifecycle table — Timberline HDZ vs Grand Sequoia vs Drexel standing-seam — every cost-per-year cell computed from the stated formula with the math shown; arithmetic verified programmatically at $844 / $947 / $660 per year). All five templates now carry a complete worked example. output_quality 8→9. Re-eval 8.8 → **9.0**, replaced.
- **Jobsite Content Repurposer** v1.2 → v1.3 (target: industry_fit — the repo's lone lowest skill, output_quality already 9). Added a **Trade-accuracy guardrails** block (hip-and-ridge vs ridge cap, starter strip vs first course, synthetic underlayment vs felt, step/counter/apron flashing, ice-and-water vs drip edge vs underlayment, ventilation/NFA honesty, system-vs-enhanced-warranty integrity) and deepened the website-gallery example to model the full correct assembly. industry_fit 8→9. Re-eval 8.7 → **8.9**, replaced.
- **Lead Response Automator** v1.4 → v1.5 (target: efficiency). Added a **"Fastest path — minimum viable input"** block (inbound lead message is the only blocking input; all else read from config or inferred), re-annotated Required Input as [blocking]/[from config]/[inferred], and tightened Efficiency notes to "runs from defaults; one question only when urgency can't be classified." efficiency 8→9. Re-eval 8.8 → **8.9**, replaced.

**Notable (not an eval-improvement):** **Insurance Supplement Writer** v2.1→v2.2 was changed by landscape-monitor before this run — it added a Step 0 "Carrier Scope Diagnostic" (derives the gap list from the carrier estimate + field scope instead of requiring hand-itemized gaps) and an optional plain-language homeowner scope summary, both fully worked into the example (diagnostic-first path reconciles to the same $7,400 / $22,220 canonical figures). Re-graded fresh: holds at **8.9** (reinforces efficiency + output value, but both were already 9).

**Persistent weaknesses:** With output_quality gaps now fully closed and industry_fit largely closed, **personalization (8) is the sole binding constraint** — it is the only sub-9 dimension on every 8.8/8.9 skill, all gated by the still-uncommitted `config.yml` fixture. The remaining skills already bind named fields with defaults and Assumptions footers; they are blocked from 9 by eval design, not skill quality. efficiency still sits at 8 on four operations/admin skills. `test-cases/` is still empty.

**Recommendations for next cycle:** (1) **commit a sample `config.yml` fixture** — now the dominant and essentially only ceiling left; with output_quality closed it is the only lever that moves the headline average; (2) apply the Lead-Response "minimum viable input + single-question triage" pattern to the four operations/admin skills still at efficiency 8 (Maintenance, Insurance Appeal, Safety, Material Order); (3) seed `test-cases/` for regression grading; (4) shift future content edits toward regression defense (strict spec-to-example consistency) rather than new headroom — the realistic per-skill ceiling without a config fixture is ~8.9–9.0.

---

## 2026-06-22 14:00 (automated eval cycle)

**Evaluated:** all 15 skills in `skills/` (excluding `_shared/`) against rubric v1.0. Since 2026-06-15, exactly one skill file changed via other automation — `roof-inspection-report.md` (v1.4→v1.5, landscape-monitor commit `2e008c6`); the other 14 were byte-identical and carried forward. Results persisted to `evals/results/2026-06-22/`.

**Averages:** prior cycle 8.81 (15 skills) → this-cycle baseline 8.79 → **after improvements 8.88**. No skill degraded.

**Scores (after improvements):**

| Skill | Score | Prior (06-15) | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Roof Inspection Report | 8.9 | 8.3 | **+0.6** |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| Material Order Calculator | 8.8 | 8.8 | 0.0 |
| Tariff & Price Adjustment Communicator | 8.8 | 8.8 | 0.0 |
| Lead Response Automator | 8.8 | 8.8 | 0.0 |
| Storm Canvassing Prioritizer | 8.8 | 8.6 | **+0.2** |
| Commercial Prospect Researcher | 8.8 | 8.6 | **+0.2** |
| Jobsite Content Repurposer | 8.7 | 8.7 | 0.0 |

**Improvements made (bottom 3 by pre-improvement score, all targeting output_quality):**

- **Storm Canvassing Prioritizer** v1.2 → v1.3. Resolved last cycle's standing #1 target: the two top-tier 🔥 briefs (#2 Sunset Grove, #3 Riverside) stubbed `[follows same structure]` are now fully written — each with a cluster-specific opening line in `canvassing.script_personality` voice, a distinct drive-in damage tell (AC-fin spatter for architectural Sunset Grove; full-slope 3-tab granule loss for mixed Riverside), a tailored objection/response, phone-bank hand-off, TX cooling-off line, and adjacency social-proof. Completed the truncated 8-knocker + 3-phone roster (one bilingual knocker per 🔥 cluster) and reconciled the Assumptions-footer bilingual count (2→3). output_quality 8→9. Re-eval 8.6 → **8.8**, replaced.
- **Commercial Prospect Researcher** v1.3 → v1.4. The spec requires a brief per 🔥 and 🟡 prospect; v1.3 worked only #1 (header said "one worked brief"). Added the 🔥 #3 Allen Tech brief (name-level contact + solar-permit trigger + vertical-matched EPDM case study) and the 🟡 #2 Parkside brief (contact-not-yet-identified path: find-the-PM steps, ponding/aerial trigger, first-reference-project framing, batch-with-#4 note). Relabeled the section; example now internally consistent end-to-end. output_quality solidified at 9, clarity 8→9. Re-eval 8.6 → **8.8**, replaced.
- **Jobsite Content Repurposer** v1.1 → v1.2. Strict re-grade caught a spec-to-example gap (Batching rules promise a "content calendar" the example never sequenced; strict baseline output_quality 8). Added a posting-calendar block (Mon IG/FB → Tue GBP → Wed Nextdoor → Thu video → Fri LinkedIn → evergreen gallery) with per-slot timing rationale and a two-post fallback. Purely additive — no draft altered. output_quality 8→9 on the strict baseline; net overall unchanged at **8.7**, replaced (no degradation).

**Notable (not an eval-improvement):** **Roof Inspection Report** v1.4→v1.5 was changed by landscape-monitor before this run — it added a "Documentation Completeness Check" (audits photos/notes against an insurer's increasingly-automated claim-review documentation set; marks Present/Partial/Missing; emits a submission-ready vs gaps verdict), fully worked into the residential example. Re-graded fresh: 8.3 → **8.9** (clarity/specificity/industry_fit/output_quality all 9). This lifts the repo's longest-standing bottom skill out of the bottom tier.

**Persistent weaknesses:** With output_quality gaps now mostly closed, **personalization (8) is the binding constraint** — it is the sole sub-9 dimension on 8 skills, all gated by the still-uncommitted `config.yml` fixture. Tariff remains the last skill with a stubbed-template output_quality gap (B/C/E). `test-cases/` is still empty. efficiency sits at 8 on four operations/admin skills.

**Recommendations for next cycle:** (1) commit a sample `config.yml` fixture — now the single dominant ceiling, would lift ~9 skills' personalization; (2) populate Tariff templates B/C/E (last stubbed-output skill); (3) lift Jobsite's industry_fit + personalization (now the lone lowest skill at 8.7); (4) seed `test-cases/` for regression grading.

---

## 2026-06-15 (automated eval cycle)

**Evaluated:** all 15 skills in `skills/` (excluding `_shared/`) against rubric v1.0. Library grew from 14 → 15 with the new **Jobsite Content Repurposer** (v1.0, added 2026-06-08 via landscape-monitor commit `fb0bf7a`). 10 skills are byte-identical to their 2026-06-08 versions and carry prior scores; results persisted to `evals/results/2026-06-15/`.

**Averages:** prior cycle 8.81 (14 skills) → this-cycle baseline 8.68 → **after improvements 8.81** (15 skills). No skill's content was degraded.

**Scores (after improvements):**

| Skill | Score | Prior (06-08) | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| Tariff & Price Adjustment Communicator | 8.8 | 8.8 | 0.0 |
| Lead Response Automator | 8.8 | 8.8 | 0.0 |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| Material Order Calculator | 8.8 | 8.8 | 0.0 |
| Jobsite Content Repurposer | 8.7 | — (new, baseline 7.1) | **+1.6** |
| Storm Canvassing Prioritizer | 8.6 | 8.7 | −0.1 (re-grade, file unchanged) |
| Commercial Prospect Researcher | 8.6 | 8.4 | **+0.2** |
| Roof Inspection Report | 8.3 | 8.3 | 0.0 (baseline 8.1 recovered) |

**Improvements made (bottom 3 by pre-improvement score, all targeting output_quality):**

- **Jobsite Content Repurposer** v1.0 → v1.1. Replaced the empty `[will be populated]` Example Output placeholder (output_quality 4 — lowest dimension in the repo) with a complete, paste-ready 6-channel batch (IG/FB, short-form video script + shot-list table, GBP, Nextdoor, LinkedIn, website gallery) from one Frisco TX GAF Timberline HDZ storm-restoration photo set, plus a photo plan and `[confirm]` checklist. Bound named config fields with defaults + missing-field flag (personalization 7→8). Re-eval 7.1 → **8.7**, replaced.
- **Commercial Prospect Researcher** v1.2 → v1.3. Added the **Est. Project Value** column the output rules described but never showed (Est. SF × labeled recover/tear-off $/sf, flagged estimated) to the spec table and the 5-building worked example; wired it into the intra-tier priority sort and the capacity check (🔥 tier now reports ~$790K combined). Re-eval 8.4 → **8.6**, replaced.
- **Roof Inspection Report** v1.3 → v1.4. Fixed the commercial PM example header inconsistency ("60-sq TPO retail strip" vs a 38,000 sf body / per-38,000-sf capital-planning table; 38,000 sf ≈ 380 sq). Header now "38,000 sf / 380-sq". Re-eval 8.1 → **8.3**, replaced (no regression vs the 8.3 it carried).

**Persistent weaknesses:** output_quality still gates every sub-9 skill (Storm Canvassing's 2 stubbed top-tier briefs, Tariff templates B/C/E unworked). personalization plateaus at 8 across ~7 skills — still no committed `config.yml` fixture. `test-cases/` remains empty.

**Note (not a degradation):** Storm Canvassing went 8.7 → 8.6 on stricter re-grade of an **unchanged** file — 2 of 3 top-tier per-cluster briefs in its example are stubbed `[follows same structure]` while the body asks for a brief per 🔥 cluster. Queued as the #1 improvement target for next cycle.

**Recommendations for next cycle:** (1) populate Storm Canvassing's two stubbed briefs — low effort, recovers the dip; (2) commit a sample `config.yml` fixture (highest leverage, now lifts ~7 skills); (3) populate Tariff templates B/C/E; (4) seed `test-cases/` for regression grading.

---

## 2026-06-08 14:00 (automated eval cycle)

**Evaluated:** all 14 skills in `skills/` (excluding `_shared/`) against rubric v1.0.

**Infrastructure:** First run to persist results. Bootstrapped `ai-skills-manager/industries/roofing/evals/` (rubric.yml, results/, test-cases/) and `ai-skills-manager/changelogs/`. Prior scores sourced from each skill's `last_eval_score` frontmatter.

**Averages:** prior 8.71 → this-cycle baseline 8.69 → **after improvements 8.81**. No skill degraded.

**Scores (after improvements):**

| Skill | Score | Prior | Δ |
|-------|------:|------:|---:|
| Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| Estimate Builder | 9.1 | 9.1 | 0.0 |
| Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| Tariff & Price Adjustment Communicator | 8.8 | 8.8 | 0.0 |
| Lead Response Automator | 8.8 | 8.8 | 0.0 |
| Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| Material Order Calculator | 8.8 | 8.1 | **+0.7** |
| Storm Canvassing Prioritizer | 8.7 | 8.7 | 0.0 |
| Commercial Prospect Researcher | 8.4 | 8.1 | **+0.3** |
| Roof Inspection Report | 8.3 | 7.9 | **+0.4** |

**Improvements made (bottom 3, all targeting output_quality):**

- **Material Order Calculator** v1.2 → v1.3. Fixed worked-example contradiction (labeled "simple gable" but applied 15% cut-up waste); corrected product-dependent ridge-cap coverage constant; added Coverage-Constants Quick-Reference table + waste-must-match-geometry guard. Re-eval 8.0 → 8.8, replaced.
- **Roof Inspection Report** v1.2 → v1.3. Completed residential example (added the Photo Log + Disclaimer/Certification sections that were defined but never shown); added a second worked example for the Commercial PM capital-planning variant. Re-eval 7.8 → 8.3, replaced.
- **Commercial Prospect Researcher** v1.1 → v1.2. Populated the prospect-list table (5 buildings, priority tiers) and added a worked Campaign-Level Recommendations block; the primary list deliverable had only ever shown an empty header row. Re-eval 8.0 → 8.4, replaced.

**Persistent weaknesses:** output_quality gates every sub-9 skill (partial example coverage — Tariff templates B/C/E, single Storm example). personalization plateaus at 8 across ~6 skills because no `config.yml` fixture is committed. `test-cases/` is empty.

**Recommendations for next cycle:** (1) commit a sample `config.yml` fixture — highest leverage; (2) populate Tariff templates B/C/E and add a second Storm example; (3) seed `test-cases/` with per-skill input fixtures for regression grading.
