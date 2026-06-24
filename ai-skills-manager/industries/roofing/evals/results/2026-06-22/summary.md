# Roofing Skills — Eval Summary, 2026-06-22

Automated Skill Evaluator cycle. 15 skills in `skills/` (excluding `_shared/`) graded against rubric v1.0.

## Headline

- **Average score: 8.88** (15 skills) — up from **8.81** (06-15 after-improvements).
- **No skill degraded.** Three improvement candidates replaced with higher- or equal-scoring versions; one externally-changed skill re-graded sharply upward.
- The repo's longest-standing bottom skill (**Roof Inspection Report**, stuck at 8.1–8.3 for three cycles) jumped to **8.9** after a landscape-monitor content addition.

## What changed since 2026-06-15

Only **one** skill file was edited between cycles by other automation: `roof-inspection-report.md` (v1.4→v1.5, landscape-monitor commit `2e008c6`). The other 14 were byte-identical and carried their 06-15 dimension scores forward. The three improvement targets below were identical to their 06-15 versions at grading time, then improved this cycle.

## Final ranking (after improvements)

| Rank | Skill | Score | Prior (06-15) | Δ |
|-----:|-------|------:|------:|---:|
| 1 | Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| 2 | Estimate Builder | 9.1 | 9.1 | 0.0 |
| 3 | Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| 3 | Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| 5 | Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| 5 | **Roof Inspection Report** | **8.9** | 8.3 | **+0.6** |
| 7 | Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| 7 | Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| 7 | Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| 7 | Material Order Calculator | 8.8 | 8.8 | 0.0 |
| 7 | Tariff & Price Adjustment Communicator | 8.8 | 8.8 | 0.0 |
| 7 | Lead Response Automator | 8.8 | 8.8 | 0.0 |
| 7 | **Storm Canvassing Prioritizer** | **8.8** | 8.6 | **+0.2** |
| 7 | **Commercial Prospect Researcher** | **8.8** | 8.6 | **+0.2** |
| 15 | **Jobsite Content Repurposer** | **8.7** | 8.7 | 0.0 |

## Score distribution

- **9.0–9.2:** 4 skills (Crew Schedule, Estimate Builder, Follow-Up, Predictive)
- **8.9:** 2 skills (Insurance Supplement, Roof Inspection)
- **8.8:** 8 skills
- **8.7:** 1 skill (Jobsite)

The library has compressed tightly into the 8.7–9.2 band. The old long tail (8.1–8.4 skills present in prior cycles) is gone: the lowest skill is now 8.7, versus 8.3 last cycle. Spread between best and worst narrowed from 0.9 to 0.5.

## Biggest improvements

1. **Roof Inspection Report 8.3 → 8.9 (+0.6).** A landscape-monitor edit added a "Documentation Completeness Check" (audit photos/notes against the documentation set an insurer's automated claim review expects; mark Present/Partial/Missing; emit a submission-ready vs gaps verdict). Fully and consistently worked into the residential example. Lifted clarity, specificity, industry_fit, and output_quality all to 9. This was an external content change, re-graded fresh — not an eval-improvement edit.

2. **Storm Canvassing Prioritizer 8.6 → 8.8 (+0.2).** Resolved the standing #1 target: the two top-tier 🔥 briefs (#2 Sunset Grove, #3 Riverside) that were stubbed `[follows same structure]` are now written in full, and the truncated canvasser roster was completed (8 knockers + 3 phone, bilingual count reconciled). output_quality 8→9.

3. **Commercial Prospect Researcher 8.6 → 8.8 (+0.2).** The spec requires a brief per 🔥 and 🟡 prospect; only #1 was worked. Added the 🔥 #3 (name-level-contact + solar-permit-trigger path) and 🟡 #2 (contact-not-yet-identified path) briefs; relabeled the section. output_quality solidified at 9 and clarity 8→9.

4. **Jobsite Content Repurposer (held at 8.7).** Strict re-grade caught a spec-to-example gap (the Batching rules promise a "content calendar" the example never sequenced). Added a posting-calendar block with timing rationale + a two-post fallback. Purely additive; output_quality 8→9 on the strict baseline, net overall unchanged at 8.7.

## Persistent weaknesses

- **personalization is the binding constraint now, not output_quality.** It sits at **8** on 9 of the 15 skills and is the *only* sub-9 dimension on Roof Inspection, Insurance Supplement, Insurance Appeal, Maintenance, Safety, Material Order, Storm, and Commercial. The single highest-leverage move in the repo is to **commit a sample `config.yml` fixture** so skills can be graded on live field binding (9–10) rather than on how gracefully they handle an absent config (8). This recommendation has carried for three cycles and is now the dominant ceiling.
- **efficiency at 8** on several operations/admin skills (Maintenance, Lead Response, Insurance Appeal, Safety) — these still imply a couple of clarifying rounds.
- **Tariff** remains the last skill with a stubbed-template output_quality gap (templates B/C/E unworked), holding it at output_quality 8. It is the clearest remaining single-dimension fix.
- **`test-cases/` is still empty** — regression grading is still inferred from file diffs and frontmatter, not run against committed input fixtures.

## Recommendations for next cycle

1. **Commit a sample `config.yml` fixture** (highest leverage — would lift personalization on ~9 skills from 8 toward 9–10; the dominant ceiling now that output_quality gaps are mostly closed).
2. **Populate Tariff templates B/C/E** — the last stubbed-output skill; a clean output_quality 8→9.
3. **Lift Jobsite's industry_fit and personalization (both 8)** — now the single lowest skill; its output is complete, so the remaining headroom is trade-depth and config binding.
4. **Seed `test-cases/`** with per-skill input fixtures so regression grading runs against real inputs.
5. Tighten **efficiency** on the four operations/admin skills still at 8 (move toward single-question triage + defaults).
