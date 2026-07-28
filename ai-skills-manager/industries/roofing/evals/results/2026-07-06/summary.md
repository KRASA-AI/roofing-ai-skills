# Roofing Skills — Eval Summary, 2026-07-06

Automated Skill Evaluator cycle. 15 skills in `skills/` (excluding `_shared/`) graded against rubric v1.0.

## Headline

- **Average score: 8.93** (15 skills) — up from **8.91** (06-29 after-improvements). New repo high.
- **No skill degraded.** Three improvement candidates were edited and re-graded upward; all three replaced their originals.
- **The floor is unchanged at 8.8, but the tail keeps shrinking.** The number of skills sitting at the 8.8 floor dropped from **6 to 3**; the middle of the distribution is now a 7-skill mass at 8.9.
- **No external changes this cycle.** No skill file changed anywhere in the repo since the 06-29 improvement commit, so all 15 opened byte-identical to their last evaluated versions. The three improvements below are this cycle's only edits.

## What changed since 2026-06-29

**Nothing externally.** `git diff` against the 06-29 improvement commit shows zero skill-content changes repo-wide (only README regeneration and a share-image sync touched tracked files). All 15 skills carried their 06-29 dimension scores forward as the pre-improvement baseline; the three improvement targets were then edited this cycle and re-graded fresh.

## Improvements made (three efficiency-8 skills, applying the proven Lead-Response pattern)

With output_quality and industry_fit gaps closed across the library in prior cycles, **efficiency at 8** is now the only *skill-fixable* sub-9 dimension (personalization at 8 is blocked by eval design — see Persistent weaknesses). Six skills entered this cycle tied at 8.8 with the identical profile `clarity 9 / specificity 9 / industry_fit 9 / output_quality 9 / personalization 8 / efficiency 8`. Efficiency is the lever; the 06-29 cycle proved the fix on Lead Response Automator (+0.1). This cycle applied the same "minimum viable input + one-question triage" pattern to the three of those six where it is most honest — each has a single genuinely-blocking input with everything else derivable from config or inference.

1. **Maintenance Plan Generator 8.8 → 8.9 (+0.1), v1.5 → v1.6.** Targeted efficiency (8). The six-item Required Input list read as interrogation, but roof **material + age + city** are the only truly blocking inputs. Added a **"Fastest path — minimum viable input"** block: geometry is sized from a market-typical footprint when absent, climate hazards infer from ZIP + `service_area.hail_zones[]`, customer type defaults to Residential, and all rates/tiers/warranty/billing read from config. Re-annotated each input `[blocking]/[from config]/[inferred]` with graceful defaults, and tightened Efficiency notes to "runs from defaults; ask one question (target price range) only when config has no maintenance rates." Purely additive — no pricing formula, tier scope, or worked example touched. efficiency 8→9.

2. **Safety Toolbox Talk Generator 8.8 → 8.9 (+0.1), v1.3 → v1.4.** Targeted efficiency (8). The strongest "runs from near-defaults" case in the set: the topic auto-picks from weather + the Stand-Down/Heat-NEP calendar, crew lead + language come from `crew_leads[]`, weather is pulled for the location, and muster point + hospital come from config — leaving **job-site type** as effectively the only blocking input. Added the Fastest-path block plus a new Efficiency-notes section, with `[blocking]/[from config]/[inferred]` annotations and a "confirm on-site" weather-provenance default. No talk structure, Heat-NEP logic, or worked example changed. efficiency 8→9.

3. **Storm Canvassing Prioritizer 8.8 → 8.9 (+0.1), v1.3 → v1.4.** Targeted efficiency (8). Five of its six inputs already resolve from config; the **storm footprint** is the lone blocking input, and the skill already accepts a city-level headline with a degraded-confidence tag. Made that explicit with a Fastest-path block (service area, roster, thresholds, and script tone from config; property-intelligence depth auto-degrades to the best available source), input annotations, and an Efficiency-notes block tightening the ask to "one question only when no roster and no capacity numbers exist." No five-layer framework, scoring, pre-storm orchestration layer, or worked example changed. efficiency 8→9.

All three edits are strictly additive — no worked example, formula, named binding, or trade content was altered, so the dimensions earning the existing 9s cannot regress.

## Final ranking (after improvements)

| Rank | Skill | Score | Prior (06-29) | Δ |
|-----:|-------|------:|------:|---:|
| 1 | Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| 2 | Estimate Builder | 9.1 | 9.1 | 0.0 |
| 3 | Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| 3 | Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| 3 | Tariff & Price Adjustment Communicator | 9.0 | 9.0 | 0.0 |
| 6 | Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| 6 | Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| 6 | Lead Response Automator | 8.9 | 8.9 | 0.0 |
| 6 | Jobsite Content Repurposer | 8.9 | 8.9 | 0.0 |
| 6 | **Maintenance Plan Generator** | **8.9** | 8.8 | **+0.1** |
| 6 | **Safety Toolbox Talk Generator** | **8.9** | 8.8 | **+0.1** |
| 6 | **Storm Canvassing Prioritizer** | **8.9** | 8.8 | **+0.1** |
| 13 | Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| 13 | Material Order Calculator | 8.8 | 8.8 | 0.0 |
| 13 | Commercial Prospect Researcher | 8.8 | 8.8 | 0.0 |

## Score distribution

- **9.2:** 1 skill (Crew Schedule)
- **9.1:** 1 skill (Estimate Builder)
- **9.0:** 3 skills (Follow-Up, Predictive, Tariff)
- **8.9:** 7 skills (Insurance Supplement, Roof Inspection, Lead Response, Jobsite, Maintenance, Safety, Storm)
- **8.8:** 3 skills (Insurance Appeal, Material Order, Commercial Prospect)

Still a 0.4-point band (8.8–9.2), but the mass has shifted up: the 8.9 tier grew from 4 skills to 7, and the 8.8 floor shrank from 6 skills to 3.

## Persistent weaknesses

- **personalization is the sole binding constraint, for a fifth cycle.** It sits at **8** on 10 of the 15 skills and is the *only* sub-9 dimension on all three remaining 8.8 skills and most of the 8.9 tier. The cause is unchanged: **no sample `config.yml` fixture is committed**, so personalization is graded on how gracefully a skill handles an *absent* config (ceiling 8) rather than on live field binding (9–10). Every 8.8/8.9 skill already binds named fields with sane defaults and Assumptions footers — they are blocked from 9 by eval design, not skill quality. **Committing a fixture remains the single highest-leverage move in the repo and now the only lever left that can move the average meaningfully.**
- **efficiency 8 now survives on only the three unimproved 8.8 skills.** Insurance Appeal legitimately needs its inputs (field inspection data can't be defaulted without fabricating), so its 8 is arguably a correct floor rather than a defect. Material Order and Commercial Prospect Researcher already carry partial one-question-triage language and are the natural targets for the next efficiency pass.
- **`test-cases/` is still empty.** Regression grading remains inferred from file diffs and frontmatter rather than run against committed input fixtures.

## Recommendations for next cycle

1. **Commit a sample `config.yml` fixture** (highest leverage — would lift personalization on ~10 skills from 8 toward 9–10). This has carried five cycles; with efficiency now fixed on all but three skills (one of which is a legitimate floor), it is essentially the *only* remaining path to a higher headline number.
2. **Finish the efficiency pass on Material Order Calculator and Commercial Prospect Researcher** — both already have partial triage language; a Fastest-path block + `[blocking]/[from config]/[inferred]` annotations would likely lift each 8→9. Leave Insurance Appeal at 8 unless its input genuinely compresses (do not manufacture a "runs from defaults" frame for a skill that legitimately needs field data).
3. **Seed `test-cases/`** with per-skill input fixtures so regression grading runs against real inputs rather than diffs.
4. **Defend against regression.** With output_quality and industry_fit closed and efficiency nearly so, future content edits should be graded strictly for spec-to-example consistency; the realistic per-skill ceiling without a config fixture is ~8.9–9.2.
