# Roofing Skills — Eval Summary, 2026-06-29

Automated Skill Evaluator cycle. 15 skills in `skills/` (excluding `_shared/`) graded against rubric v1.0.

## Headline

- **Average score: 8.91** (15 skills) — up from **8.88** (06-22 after-improvements).
- **No skill degraded.** Three improvement candidates were edited and re-graded upward; all three replaced their originals.
- **The floor rose from 8.7 to 8.8.** Every skill now scores 8.8–9.2 — a 0.4 spread, the tightest the library has ever been.
- The skill that opened this cycle as the lone lowest (**Jobsite Content Repurposer, 8.7**) is no longer at the bottom; **Tariff** broke into the 9.0 tier by closing its last output_quality gap.

## What changed since 2026-06-22

Only **one** skill file was edited between cycles by other automation: `insurance-supplement-writer.md` (v2.1→v2.2, landscape-monitor commit `3842c8b`, 2026-06-29). It was re-graded fresh. The other 14 were byte-identical to their 06-22 evaluated versions and carried their dimension scores forward; the three improvement targets were then edited this cycle.

**Insurance Supplement Writer v2.2 (external change, re-graded):** added a Step 0 "Carrier Scope Diagnostic" that derives the gap list from the carrier's Xactimate estimate + the field scope (turning gaps from a required input into a generated output), plus an optional plain-language homeowner scope summary. Both are fully and consistently worked into the example — the diagnostic-first path reconciles to the same canonical $7,400 supplement / $22,220 revised RCV. The change reinforces efficiency and output value, but both dimensions were already at 9, so the weighted overall holds at **8.9** (no degradation).

## Final ranking (after improvements)

| Rank | Skill | Score | Prior (06-22) | Δ |
|-----:|-------|------:|------:|---:|
| 1 | Crew Schedule Optimizer | 9.2 | 9.2 | 0.0 |
| 2 | Estimate Builder | 9.1 | 9.1 | 0.0 |
| 3 | **Tariff & Price Adjustment Communicator** | **9.0** | 8.8 | **+0.2** |
| 3 | Follow-Up Sequence | 9.0 | 9.0 | 0.0 |
| 3 | Predictive Lead Scorer | 9.0 | 9.0 | 0.0 |
| 6 | Insurance Supplement Writer | 8.9 | 8.9 | 0.0 |
| 6 | Roof Inspection Report | 8.9 | 8.9 | 0.0 |
| 6 | **Lead Response Automator** | **8.9** | 8.8 | **+0.1** |
| 6 | **Jobsite Content Repurposer** | **8.9** | 8.7 | **+0.2** |
| 10 | Insurance Appeal Inspection Report | 8.8 | 8.8 | 0.0 |
| 10 | Maintenance Plan Generator | 8.8 | 8.8 | 0.0 |
| 10 | Safety Toolbox Talk Generator | 8.8 | 8.8 | 0.0 |
| 10 | Material Order Calculator | 8.8 | 8.8 | 0.0 |
| 10 | Storm Canvassing Prioritizer | 8.8 | 8.8 | 0.0 |
| 10 | Commercial Prospect Researcher | 8.8 | 8.8 | 0.0 |

## Score distribution

- **9.2:** 1 skill (Crew Schedule)
- **9.1:** 1 skill (Estimate Builder)
- **9.0:** 3 skills (Tariff, Follow-Up, Predictive)
- **8.9:** 4 skills (Insurance Supplement, Roof Inspection, Lead Response, Jobsite)
- **8.8:** 6 skills

The library is now a 0.4-point band (8.8–9.2). The old long tail is fully gone: there is no skill below 8.8, versus a lowest of 8.7 last cycle and 8.1–8.3 three cycles ago.

## Improvements made (bottom 3 by pre-improvement score)

1. **Tariff & Price Adjustment Communicator 8.8 → 9.0 (+0.2), v2.2 → v2.3.** Targeted output_quality — the only 8.8 skill with a sub-9 *0.2-weight* dimension, and the standing "Tariff templates B/C/E unworked" recommendation carried for three cycles. Added a fully-worked **Template C** (a Frisco "What Roofing Costs Right Now — May 2026" page: populated replacement ranges, 40/38/22 cost split, four homeowner Q&As, plus the required plain-HTML CMS-paste variant) and a fully-worked **Template E** (a 28-sq Frisco three-material lifecycle table — GAF Timberline HDZ vs GAF Grand Sequoia vs Drexel standing-seam metal — with every cost-per-year cell computed from the stated formula and the math shown). All five templates now carry a complete worked example. Template E arithmetic verified programmatically ($844 / $947 / $660 per year). output_quality 8→9.

2. **Jobsite Content Repurposer 8.7 → 8.9 (+0.2), v1.2 → v1.3.** The repo's lone lowest skill; output_quality was already 9, leaving industry_fit (8, "shallow on trade specifics") as the liftable 0.2-weight dimension. Added a **Trade-accuracy guardrails** block pinning the terms marketing copy most often mangles — hip-and-ridge vs ridge cap, starter strip vs first course, synthetic underlayment vs felt, step/counter/apron flashing, ice-and-water vs drip edge vs underlayment, ventilation/NFA honesty, and system-vs-enhanced-warranty integrity (no warranty misrepresentation on a non-certified install). The website-gallery example draft now models the full correct assembly sequence end to end. industry_fit 8→9.

3. **Lead Response Automator 8.8 → 8.9 (+0.1), v1.4 → v1.5.** Targeted efficiency (8). Despite five strong worked examples, the seven-item Required Input list read as front-loaded interrogation. Added a **"Fastest path — minimum viable input"** block establishing that the inbound lead message is the *only* blocking input; every other field is read from config or inferred from the triage classifier. Re-annotated each input as [blocking]/[from config]/[inferred] and tightened the Efficiency notes to "runs from defaults; one question only when urgency can't be classified." efficiency 8→9.

## Persistent weaknesses

- **personalization is now the sole binding constraint.** It sits at **8** on 10 of the 15 skills and is the *only* sub-9 dimension on every 8.8 and 8.9 skill except where another was the target. The cause is unchanged for a fourth cycle: **no sample `config.yml` fixture is committed**, so personalization is graded on how gracefully a skill handles an *absent* config (ceiling 8) rather than on live field binding (9–10). Every remaining 8.8/8.9 skill already binds named fields with sane defaults and Assumptions footers — they are blocked from 9 by the eval design, not by skill quality. **Committing a fixture is now the single highest-leverage move in the repo and the only path to moving the average meaningfully.**
- **efficiency at 8** still holds on four operations/admin skills (Maintenance, Insurance Appeal, Safety, Material Order). The Lead Response fix this cycle is a reusable template: collapse a long Required Input list into a "minimum viable input + one-question triage" block.
- **`test-cases/` is still empty.** Regression grading remains inferred from file diffs and frontmatter rather than run against committed input fixtures.

## Recommendations for next cycle

1. **Commit a sample `config.yml` fixture** (highest leverage — would lift personalization on ~10 skills from 8 toward 9–10; it is now the dominant and essentially the *only* ceiling left). This recommendation has carried four cycles; with output_quality gaps now fully closed, it is the only lever that changes the headline number.
2. **Apply the Lead-Response efficiency pattern** to the four operations/admin skills still at efficiency 8 (Maintenance, Insurance Appeal, Safety, Material Order): a "minimum viable input + single-question triage" block.
3. **Seed `test-cases/`** with per-skill input fixtures so regression grading runs against real inputs rather than diffs.
4. With output_quality and (largely) industry_fit gaps closed across the library, future content edits should defend against *regression* (strict spec-to-example consistency) rather than chase new headroom — the realistic ceiling without a config fixture is ~8.9–9.0 per skill.
