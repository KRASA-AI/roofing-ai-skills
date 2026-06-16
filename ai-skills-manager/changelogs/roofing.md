# Roofing Skills — Eval & Improvement Changelog

Maintained by the Skill Evaluator scheduled task. Most recent entry first.

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
