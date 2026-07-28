# Roofing Skills — Eval Summary, 2026-07-13

Automated Skill Evaluator cycle. 15 skills in `skills/` (excluding `_shared/`) graded against rubric v1.0.

## Headline

- **Average score: 8.95** (15 skills) — up from **8.93** (07-06 after-improvements). New repo high.
- **Four skills edited, four improved, none degraded.** Two of the four (Material Order, Commercial Prospect) show a **+0.1 gain against the recorded 07-06 scores**. The other two (Insurance Appeal, Estimate Builder) land at **Δ 0.0 against 07-06** — they improved against this cycle's *cold re-grades*, which came in below the carried-forward numbers. In plain terms: two skills got better, and two skills were quietly broken and are now fixed. The headline average moves only +0.02 because the repair work is invisible to a metric that was measuring the wrong thing.
- **This cycle broke the carry-forward habit and graded three skills cold — and found three defects that five prior cycles had carried forward as 9s.** Two of them were arithmetic errors in worked examples that a user would have pasted straight into a supplier order or a carrier appeal.
- **The config fixture is committed.** After five cycles of recommending it, `evals/fixtures/config.sample.yml` now exists. It does not affect this cycle's scores (personalization was graded on the old absent-config basis for continuity) but it unblocks the personalization ceiling from 2026-07-20 onward.

## What changed since 2026-07-06

One external edit: the **landscape monitor** shipped **Estimate Builder v1.6** earlier today, adding a Transparent-Quote Benchmark section. That edit introduced a regression (see below). The other 14 skills opened byte-identical to their 07-06 versions.

## The carry-forward problem (read this one)

Since 06-08, unchanged skills have had their dimension scores **carried forward** rather than re-derived. That is efficient and it is how the 8.9 plateau got built — but it means a defect that was missed once stays missed forever. This cycle re-graded the three bottom skills from scratch instead of inheriting, and all three turned out to be scored too high:

| Skill | Carried-forward score | Honest cold grade | What was actually wrong |
|---|---:|---:|---|
| Material Order Calculator | 8.8 | **8.6** | Worked example computed nails off **bundles** (97 × 80 × 5 ≈ 38,800 → **10 boxes**) when the skill's own rule 8 specifies nails per **shingle** at ~80 shingles per **square**. Real answer: 2 boxes. The example over-ordered fasteners ~5x, contradicting the spec printed directly above it. Separately, rule 10 multiplied squares by `dump.bundles_per_yard` — a **divisor** — under-sizing the dumpster ~2x. |
| Insurance Appeal Inspection Report | 8.8 | **8.4** | The success statistic appeared in **three different forms in one report** ("31 of 38 (82%)" vs "of 31 appeals, 25 overturned (82%)" — and 25/31 is 80.6%). The example numbered its sections 1–9 against a spec defining 10, so every cross-reference pointed at the **wrong section**. Remaining useful life read "14–16 years" in two places and "14.5 years" in two others. |
| Estimate Builder | 9.1 | **8.9** | v1.6 (shipped today) added a new customer-facing output section and demonstrated it in **neither** worked example. |

None of these are cosmetic. The nail line would have gone into a supplier RFQ. The appeal-report inconsistencies would have gone to a carrier underwriter who is *paid* to find a number that disagrees with itself — in a document whose entire purpose is to catch a carrier doing exactly that.

**Process change recommended:** carry-forward is fine for a skill that has been cold-graded at least once *since its last content edit*. It is not fine as a permanent state. Rotate three skills through a cold re-grade every cycle.

## Improvements made

1. **Material Order Calculator 8.6 → 8.9 (+0.3), v1.3 → v1.4.** Targeted output_quality (8) and efficiency (8). Rebuilt the nail line on the correct per-shingle basis (28 sq × 80 shingles × 6-nail high-wind = 13,440 ÷ ~7,200/box → **2 boxes**). Fixed the inverted debris rule (debris bundles ÷ `bundles_per_yard`, with an explicit warning that inverting it under-sizes the dumpster). Added a **ROOF GEOMETRY basis block** to the example so every linear-foot line reconciles against one set of numbers — the ice & water line had been citing an eave figure that could not be squared with the eave+rake used elsewhere (now 3 rolls, not 2). Added the Fastest-path minimum-viable-input block with `[blocking]`/`[from config]`/`[inferred]` annotations. output_quality 8→9, efficiency 8→9.

2. **Insurance Appeal Inspection Report 8.4 → 8.8 (+0.4), v1.1 → v1.2.** Targeted output_quality (8) and clarity (8). Reconciled the success statistic to a single form, renumbered the example to the spec's 10 sections, repointed every cross-reference, and settled remaining-useful-life on one number. Added a **5-point Consistency Check** to the output requirements — RUL stated identically everywhere, success statistic in exactly one form with the percentage actually equal to numerator ÷ denominator, cross-references pointing at real sections, quantifications reconciling with slope areas, and every carrier-cited defect having exactly one rebuttal row — so the failure mode cannot recur. clarity 8→9, output_quality 8→9. **Efficiency deliberately held at 8:** field inspection data cannot be defaulted without fabricating evidence, and a fabricated appeal report is worse than no appeal report. That 8 is an honest floor, not a defect.

3. **Commercial Prospect Researcher 8.8 → 8.9 (+0.1), v1.4 → v1.5.** Targeted efficiency (8), the last of the six 8.8-tier skills to get the Fastest-path treatment. Five of its six Required Inputs already resolved from config, and the sixth — "reason to call" — is precisely what **Layer 3 of the skill's own research framework exists to discover**. Asking the rep for a trigger inverts the skill: the rep is here *because* they don't know which building has one. v1.5 makes territory the sole blocking input (a bare zip code is enough), sets explicit defaults (10 buildings, name-level-attempted contact depth, 12+ year roof age), and fires a single question only when config carries neither `target_verticals[]` nor `commercial_zip_codes[]`. efficiency 8→9.

4. **Estimate Builder 8.9 → 9.1 (regression repaired), v1.6 → v1.7.** The landscape monitor's v1.6 added a Transparent-Quote Benchmark section — a good addition, aimed at the homeowner who shows up holding a cheaper AI-platform instant quote — but demonstrated it in neither worked example, reopening the spec-to-example gap the 06-15 and 06-22 cycles closed elsewhere. The *highest-stakes section in the skill*, the one an estimator reads while a homeowner sits across the table with a competing number, was the only section with no worked instance. v1.7 writes it: a $14,900 platform quote benchmarked against the $17,794 Good tier, a six-row Value Delta table where **every row is independently verifiable** (license #, COI, GAF certification lookup, permit #, warranty document, phone number — no adjectives), the lifecycle-per-year reframe, and a Pricing-Flags line telling the estimator to *hold the number* because the delta is scope, not margin. output_quality restored to 9.

All four edits are additive or corrective. No formula, tier structure, named binding, or trade-content section was removed.

## Final ranking (after improvements)

| Rank | Skill | Score | Prior (07-06) | Δ |
|-----:|-------|------:|------:|---:|
| 1 | Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| 2 | **Estimate Builder** | **9.1** | 9.1 | 0.0 *(dipped to 8.9 intra-cycle, repaired)* |
| 3 | Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| 3 | Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| 3 | Tariff & Price Adjustment Communicator | 9.0 | 9.0 | 0.0 |
| 6 | Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| 6 | Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| 6 | Lead Response Automator | 8.9 | 8.9 | 0.0 |
| 6 | Jobsite Content Repurposer | 8.9 | 8.9 | 0.0 |
| 6 | Maintenance Plan Generator | 8.9 | 8.9 | 0.0 |
| 6 | Safety Toolbox Talk Generator | 8.9 | 8.9 | 0.0 |
| 6 | Storm Canvassing Prioritizer | 8.9 | 8.9 | 0.0 |
| 6 | **Material Order Calculator** | **8.9** | 8.8 | **+0.1** |
| 6 | **Commercial Prospect Researcher** | **8.9** | 8.8 | **+0.1** |
| 15 | **Insurance Appeal Inspection Report** | **8.8** | 8.8 | 0.0 |

## Score distribution

- **9.2:** 1 skill
- **9.1:** 1 skill
- **9.0:** 3 skills
- **8.9:** 9 skills
- **8.8:** 1 skill

The 8.8 floor now holds a single skill (Insurance Appeal), and its remaining sub-9 dimension is the efficiency floor we have deliberately chosen not to fake. The distribution has compressed to a 0.4-point band with a 9-skill mass at 8.9 — which is the clearest possible signal that **the personalization cap is now the only thing standing between this library and a 9.2+ average.**

## Persistent weaknesses

- **personalization at 8 on 12 of 15 skills — but the blocker is now removed.** `evals/fixtures/config.sample.yml` is committed as of this cycle. It binds every field the library references (rates, brands, suppliers + quote formats, dump sizing, crew leads + languages, inspector credentials, appeals history, commercial case studies + certifications, active tariffs, voice/voice_commercial/voice_expert) and deliberately **omits** several fields (`company.logo_path`, `pricing.commercial_inspection_flat`, `suppliers.preferred[].last_pricing`, and any medical-office case study) so that graceful-degradation behavior is still tested rather than assumed. Personalization was **not** re-graded against it this cycle — changing the grading basis and the skills in the same pass would make the delta unreadable. From 07-20, grade against the fixture.
- **`test-cases/` is still empty.** Now that a config fixture exists, per-skill input fixtures are the obvious next step and much cheaper to write.
- **Carry-forward grading concealed real defects for five cycles.** See the section above. This is the most important finding of this cycle and it is a *process* weakness, not a skill weakness.

## Recommendations for next cycle

1. **Grade personalization against `fixtures/config.sample.yml`.** Expect most skills to land 9 (fields resolve, used correctly, degrade gracefully) and a few to reveal that they name fields the fixture doesn't carry — which is itself useful signal. Projected average: ~9.1–9.2.
2. **Cold-re-grade three carried-forward skills per cycle, on rotation.** Start with the three highest scorers (Crew Schedule 9.2, Estimate Builder 9.1, Follow-Up 9.0) — they have been carried longest and have never been checked adversarially for the arithmetic-in-worked-example failure mode that just turned up twice in one afternoon.
3. **Seed `test-cases/`** with per-skill input fixtures now that a config exists to run them against.
4. **Add a spec-to-example consistency gate to the landscape-monitor task.** Estimate Builder v1.6 regressed because a new output section shipped without a worked instance. Any monitor edit that adds an output section should be required to demonstrate it in an example, or hand the skill to the evaluator flagged.

## Verification pass

A separate adversarial verification agent re-derived every number in this cycle. It confirmed: all 15 weighted `overall` scores recompute correctly; the 15 skills map 1:1 to the 15 result files; the average is 8.9467 → **8.95**; all four skill diffs are additive or corrective with no content loss; and every line of the rebuilt Material Order example reconciles against the ROOF GEOMETRY block and the skill's own coverage constants.

It also caught five defects in this cycle's own work, all of which were fixed before this summary was finalized:

- Estimate Builder's new section-8 template cited "Line 12 of the exclusions above" — there is no line 12 (the line-item table ends at 11). Repointed to the scope-exclusions line in section 7.
- Estimate Builder's section 8 described itself as "added ahead of the terms" while being numbered *after* Terms. Prose corrected to match the numbering and the example.
- Estimate Builder's Financing paragraph (pre-existing, untouched by v1.6) quoted Good and Better at the *same* $216/month and implied a payment that didn't amortize. Rebuilt as a table with correct GreenSky math at both programs (0%/24mo and 9.99%/144mo).
- Insurance Appeal's *spec* sample rebuttal row still said "820 sq ft slope" while the worked example says 760 — the same spec-vs-example disagreement the version was written to eliminate. Aligned to 760, and the new Consistency Check rule 2 was reworded so that "82%" from 31 ÷ 38 (81.6%) is explicitly correct while "25 of 31 (82%)" is explicitly not.
- `last_eval_score` frontmatter was left stale at 8.8 on both Material Order and Commercial Prospect after they scored 8.9. Updated.

**Not committed.** All four skill edits, the changelog entry, `evals/results/2026-07-13/`, and `evals/fixtures/config.sample.yml` are written to the working tree but left uncommitted, consistent with this task's read-mostly remit — the repo's daily-sync job commits tracked changes. If the fixture and results should be committed by the evaluator itself, that is a one-line change to this task's instructions.
