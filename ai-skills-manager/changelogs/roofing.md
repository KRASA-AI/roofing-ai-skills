# Roofing Skills — Eval & Improvement Changelog

Maintained by the Skill Evaluator scheduled task. Most recent entry first.

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
