# Roofing Skills Eval — Daily Summary, 2026-06-15

**Skills evaluated:** 15 (all of `skills/`, excluding `_shared/`)
**Rubric:** v1.0 — 6 dimensions (clarity, specificity, industry_fit, output_quality @ 20% each; personalization, efficiency @ 10% each)
**Since last cycle (2026-06-08):** one new skill was added — **Jobsite Content Repurposer** (v1.0, landscape-monitor commit `fb0bf7a`) — bringing the graded library from 14 to 15. The other four files touched since 06-08 were the three skills improved last cycle plus this new one; the remaining 10 skills are byte-identical to their 06-08 versions and carry their prior scores.

## Headline numbers

| Metric | Value |
|--------|------:|
| Average score (after improvements) | **8.81** |
| Average score (this cycle, before improvements) | 8.68 |
| Average score (prior cycle, 2026-06-08) | 8.81 |
| Skills improved & replaced | 3 |
| Skills degraded (file regressed) | 0 |
| New skills brought to standard | 1 (Jobsite Content Repurposer → 8.7) |
| Highest | Crew Schedule Optimizer (9.2) |
| Lowest | Roof Inspection Report (8.3) |

The library average held at 8.81 even after absorbing a brand-new skill that entered at a baseline of 7.1 — the improvement pass pulled it (and two others) up enough to keep the mean flat while growing the catalog.

## Full ranking (post-improvement)

| Rank | Skill | Score | Δ vs 06-08 |
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
| 6 | Material Order Calculator | 8.8 | 0.0 |
| 12 | **Jobsite Content Repurposer** | **8.7** | new (baseline 7.1) |
| 13 | Storm Canvassing Prioritizer | 8.6 | −0.1 |
| 13 | **Commercial Prospect Researcher** | **8.6** | **+0.2** |
| 15 | Roof Inspection Report | 8.3 | 0.0 |

## Score distribution (post-improvement)

```
9.0–9.2  ████ 4
8.6–8.9  ██████████ 10
8.3–8.5  █ 1
```

## Improvement candidates this cycle (bottom 3 by pre-improvement score)

All three were gated by **output_quality** — incomplete or internally inconsistent worked examples, not unclear instructions. This is the same pattern flagged in the last two cycles.

1. **Jobsite Content Repurposer** 7.1 → **8.7** (+1.6, biggest gain). The new skill shipped with its Example Output section as an empty `[will be populated]` placeholder — output_quality 4, the single lowest dimension anywhere in the repo. Replaced it with a complete, paste-ready batch built from one Frisco TX GAF Timberline HDZ storm-restoration photo set: all six channels (Instagram/Facebook, a short-form video script with a shot-list table, GBP, Nextdoor, LinkedIn, website gallery), plus a photo plan and a `[confirm]` checklist. Also tightened personalization (4→8) by binding named `config.yml` fields with defaults and a missing-field flag rule.
2. **Commercial Prospect Researcher** 8.4 → **8.6** (+0.2). The output rules referenced "estimated-value flagging," but the prospect-list table had no value column, so reps couldn't sort the biggest qualified jobs to the top. Added an **Est. Project Value** column (Est. SF × a labeled recover/tear-off $/sf assumption, always flagged estimated) to both the spec table and the 5-building worked example, and wired it into the priority sort and the capacity check (🔥 tier now reports ~$790K combined).
3. **Roof Inspection Report** 8.1 → **8.3** (+0.2). The commercial PM example header read "60-sq TPO retail strip" while the body and the per-38,000-sf capital-planning table described a 38,000 sf roof — and 38,000 sf is ~380 squares, not 60. Corrected the header to "38,000 sf / 380-sq" so the figure an adjuster or asset manager would cross-check is consistent. No file regression vs. the 8.3 it carried; the stricter baseline simply caught the blemish the previous cycle missed.

## Notable scoring note (not a degradation)

**Storm Canvassing Prioritizer** moved 8.7 → 8.6 **without any file change.** On a close re-read, two of the three top-tier per-cluster briefs in its Example Output are stubbed as `[follows same structure]`, while the skill body instructs producing a full brief for *each* 🔥 cluster. That partial coverage warrants output_quality 8, not 9. The file was **not** modified, so the skill itself is not degraded — this is stricter grading, and Storm is now the clearest improvement target for next cycle (it just missed this cycle's bottom-3 cut).

## Persistent / dimension-wide weaknesses

- **output_quality remains the gating dimension.** Every sub-9.0 skill is held there by an example gap, not by unclear instructions. Remaining offenders: Storm Canvassing (2 of 3 top-tier briefs stubbed), Tariff Communicator (templates B/C/E still unworked), Roof Inspection (8 across the board — solid but nothing at the 9–10 "deep trade fluency" tier).
- **personalization plateaus at 8** across roughly half the library. Skills bind named `config.yml` fields well, but there is still **no committed `config.yml` fixture**, so voice-tuning and missing-field handling can't be graded against a live config or smoke-tested. Unchanged infrastructure gap from the last three cycles.
- **`evals/test-cases/` is still empty.** Grading remains structural rather than execution-based; contradictions like the Roof Inspection sq/sf mismatch are caught by manual re-read, not by an automated fixture.

## Recommendations for next cycle

1. **Improve Storm Canvassing Prioritizer first** — populate the two stubbed top-tier briefs (Sunset Grove, Riverside) so the example matches the "brief per 🔥 cluster" instruction. Low effort, recovers the 0.1 and likely reaches 8.8+.
2. **Add a sample `config.yml` fixture** to the repo root. Still the single highest-leverage change — it would lift the personalization ceiling across ~7 skills (now including Jobsite Content Repurposer) and enable execution-based grading.
3. **Populate Tariff Communicator templates B/C/E** (only A and D are worked end-to-end).
4. **Seed `test-cases/`** with at least one input fixture per skill to enable regression grading and auto-catch internal contradictions.
5. No file regressions to address: zero skills had their content degraded. The three −baseline dips (jobsite as a new entry, roof, commercial) were all recovered or exceeded by the improvement pass; the lone net −0.1 (Storm) is a re-grade on an unchanged file, queued as rec #1.
