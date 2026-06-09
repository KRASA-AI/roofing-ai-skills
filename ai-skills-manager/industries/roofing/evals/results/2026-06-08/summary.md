# Roofing Skills Eval — Daily Summary, 2026-06-08

**Skills evaluated:** 14 (all of `skills/`, excluding `_shared/`)
**Rubric:** v1.0 — 6 dimensions (clarity, specificity, industry_fit, output_quality @ 20% each; personalization, efficiency @ 10% each)
**Note:** This is the first run that persists results to `ai-skills-manager/`. The directory tree, rubric, and test-cases folder were bootstrapped this cycle. Prior-cycle scores were read from each skill's `last_eval_score` frontmatter field and used as the comparison baseline.

## Headline numbers

| Metric | Value |
|--------|------:|
| Average score (after improvements) | **8.81** |
| Average score (this cycle, before improvements) | 8.69 |
| Average score (prior cycle, from frontmatter) | 8.71 |
| Skills improved & replaced | 3 |
| Skills degraded | 0 |
| Highest | Crew Schedule Optimizer (9.2) |
| Lowest | Roof Inspection Report (8.3, up from 7.8) |

## Full ranking (post-improvement)

| Rank | Skill | Score | Δ vs prior |
|-----:|-------|------:|-----------:|
| 1 | Crew Schedule Optimizer | 9.2 | 0.0 |
| 2 | Estimate Builder | 9.1 | 0.0 |
| 3 | Follow-Up Sequence | 9.0 | 0.0 |
| 3 | Predictive Lead Scorer | 9.0 | 0.0 |
| 5 | Insurance Supplement Writer | 8.9 | 0.0 |
| 6 | Maintenance Plan Generator | 8.8 | 0.0 |
| 6 | Tariff & Price Adjustment Communicator | 8.8 | 0.0 |
| 6 | Lead Response Automator | 8.8 | 0.0 |
| 6 | Insurance Appeal Inspection Report | 8.8 | 0.0 |
| 6 | Safety Toolbox Talk Generator | 8.8 | 0.0 |
| 6 | **Material Order Calculator** | **8.8** | **+0.7** |
| 12 | Storm Canvassing Prioritizer | 8.7 | 0.0 |
| 13 | **Commercial Prospect Researcher** | **8.4** | **+0.3** |
| 14 | **Roof Inspection Report** | **8.3** | **+0.4** |

## Score distribution (post-improvement)

```
9.0–9.2  ████ 4
8.7–8.9  █████████ 9
8.3–8.4  ██ 2
```

## Improvement candidates this cycle (bottom 3)

The bottom three were the same trio as the prior cycle. All three shared the same weakest dimension — **output_quality** — driven by incomplete or internally inconsistent worked examples rather than weak instructions.

1. **Roof Inspection Report** 7.8 → **8.3** (+0.5). The residential example defined a Photo Log and a Disclaimer/Certification section in its structure but never showed them; both are now demonstrated (23-photo log + full cert footer). Added a second fully-worked example for the **Commercial Property-Manager variant** (38,000 sf TPO retail strip) exercising the capital-planning table, IBC 1503 / ASCE 7 reserve-study items, and tenant-coordination notes — the variant had been described but unexemplified.
2. **Material Order Calculator** 8.0 → **8.8** (+0.8, biggest gain). Fixed a verifiable internal contradiction in the worked example (labeled "simple gable" while applying a 15% cut-up waste factor); corrected the ridge-cap coverage constant to be product-dependent (~31 lf/bundle cut 3-tab vs ~20 lf/bundle pre-formed cap); added a Coverage-Constants Quick-Reference table and an explicit "waste factor must match geometry" guard.
3. **Commercial Prospect Researcher** 8.0 → **8.4** (+0.4). The skill's primary deliverable is a prospect *list*, but prior versions only demonstrated a single building brief and left the list table an empty header row. Added a populated 5-building example (warehouse/retail/MOB mix with 🔥/🟡/🟢 tiers and estimated-value flagging) plus a worked Campaign-Level Recommendations block (send order, geo cluster, shared-PM batching, capacity saturation check).

## Persistent / dimension-wide weaknesses

- **output_quality is the gating dimension.** Every sub-9.0 skill is held there by an example gap, not by unclear instructions. Remaining offenders: Tariff Communicator (templates B/C/E still unworked) and Storm Canvassing Prioritizer (single worked example).
- **personalization plateaus at 8** for roughly half the library. Skills bind named `config.yml` fields well, but there is **no committed `config.yml` fixture** in the repo, so voice-tuning and missing-field handling can't be graded against a live config or smoke-tested. This is an infrastructure gap, not a per-skill defect.
- **`evals/test-cases/` is empty.** There are no input fixtures, so grading is structural rather than execution-based.

## Recommendations for next cycle

1. **Add a sample `config.yml` fixture** to the repo root. This is the single highest-leverage change — it would lift the personalization ceiling across ~6 skills and enable execution-based grading.
2. **Populate Tariff Communicator templates B/C/E** and **add a second Storm Canvassing example** (e.g., a wind/derecho event vs. the existing hail event) — both are gated at output_quality 8 by partial example coverage.
3. **Seed `test-cases/`** with at least one input fixture per skill to enable regression grading and catch contradictions like the Material Calculator gable/waste mismatch automatically.
4. No regressions to address: zero skills dropped vs. the prior cycle after improvement. The three −0.1 baseline dips were stricter re-grading and were fully recovered by the improvement pass.
