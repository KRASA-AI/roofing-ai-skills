# Roofing Skills — Eval & Improvement Changelog

Maintained by the Skill Evaluator scheduled task. Most recent entry first.

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
