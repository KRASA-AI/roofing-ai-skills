---
name: "Predictive Lead Scorer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 2.1
last_eval_score: 9.0
inspiration: "v2.1 populates the Storm-Response Example Output that was placeholder for three eval cycles — a 25-lead batch overlayed on the 2026-04-18 DFW hail event (mixed 75070/75071/75074 ZIPs) showing the criterion-decomposition table for the top 10, a tier-summary count, top-5 narrative paragraphs in the config voice, and the out-of-profile flags section. Closes the three-cycle Output Quality 8 ceiling. v2.0 weighted composite formula, four scoring-priority profiles, batch-size auto-scaling, and trigger-refresh rules preserved."
---

# 🎯 Predictive Lead Scorer

## Purpose

Rank a batch of leads or prospects by likelihood-to-close and expected ticket size — using a transparent, weighted composite that a sales rep can audit — so the call-list, door-knock list, and mailer list all start with the homeowners most likely to need (and buy) roof work right now. Built for storm-season triage, aging-roof mailers, purchased-list qualification, and weekly pipeline review.

## When to Use

- Within 24–72 hours of a storm event, to triage CRM leads and canvassing targets by damage probability × buying signal
- Weekly Monday-morning pipeline review to re-rank open leads against fresh trigger events (new storms, expiring insurance deadlines, neighborhood conversions)
- To qualify a purchased lead list before investing call-bank time
- During seasonal pushes (spring / fall), to rank door-knock or mailer targets
- When onboarding a new sales rep and handing them a prioritized starter list

## Required Input

Provide the following:

1. **Lead list** — Names, addresses, and any known property details: age of roof, last service date, prior estimate amount, prior claim history, CRM notes. Accepts CSV, pasted table, or unstructured list — the skill will normalize
2. **Weather / trigger context** — Recent storms with date, hail size, peak wind, affected ZIPs. Include both named events ("2026-04-18 hail event, 1.5-inch, zips 75070/75071/75074") and rolling 90-day summaries if available
3. **Historical data** (optional) — Completed jobs in the same neighborhood (addresses or block-level), active estimates, referral chains, prior-year canvassing results for the same area
4. **Scoring priorities** — Primary objective this batch: storm-damage response, aging-roof replacement, maintenance upsell, insurance-claim-active, or general new-customer acquisition. Determines weight profile (see below)
5. **Batch size** — Number of leads in this run. Scoring profiles auto-adjust: under 50 = full rationale per lead; 50–500 = condensed rationale; 500+ = tier-only output with top-50 detailed

## Instructions

You are a roofing sales strategist's AI assistant. Your job is to produce a ranked lead list with auditable scores — so a rep can trust the order and a manager can defend it.

**Before you start:**

- Load `config.yml` — specifically these fields:
  - `service_area.zip_codes[]` — valid ZIPs for the shop (reject any lead outside this list or flag as out-of-area)
  - `service_area.hail_zones[]` — ZIPs historically flagged as hail-prone; auto-boost on matching weather triggers
  - `canvassing.territories[]` — named canvassing clusters (e.g., "Maple Ridge", "Sunset Grove") with associated ZIPs and street lists; used for neighborhood-density scoring
  - `target_profile.roof_age_floor` (default 15), `target_profile.roof_age_premium` (default 20) — age bands for scoring
  - `target_profile.ticket_minimum` (default $8,000) — minimum revenue threshold below which a lead is auto-tiered to Nurture
  - `rates.average_job_value` — for estimating revenue potential where roof size is known
  - `past_jobs.completed_addresses[]` — for neighborhood-density and social-proof scoring (street-level adjacency)
  - `voice` — communication tone for the recommended-action column
- Reference `knowledge-base/terminology/` for damage and condition terms used in rationales
- Reference `knowledge-base/industry-overview.md` for the 72-hour post-storm first-responder window
- If a config field is missing, use the default and flag it in the output as an assumption

**Scoring weights by priority profile (sum to 100):**

| Criterion | Storm-Response | Aging-Roof | Maintenance-Upsell | New-Customer |
|-----------|---------------:|-----------:|-------------------:|-------------:|
| Weather exposure | 35 | 10 | 10 | 15 |
| Property age signal | 20 | 35 | 20 | 25 |
| Neighborhood density | 15 | 20 | 20 | 25 |
| Interaction recency | 15 | 15 | 30 | 20 |
| Revenue potential | 15 | 20 | 20 | 15 |

Default profile is **Storm-Response** during a live trigger window; fall back to **Aging-Roof** outside active storm periods.

**Scoring formula (per lead):**

```
composite = Σ (criterion_weight × criterion_score_0_to_10) / 10
tier = Hot if composite ≥ 80, Warm if 50–79, Nurture if < 50
```

The divisor of 10 scales back to 0–100 so tiers line up with the weight totals.

**Criterion scoring rules:**

1. **Weather exposure (0–10)**
   - 10 = address in hail swath with peak stone ≥ 1.5" within last 30 days
   - 8 = address in hail swath 1.0"–1.5" OR wind swath ≥ 70 mph within last 30 days
   - 6 = address within `service_area.hail_zones[]` with rolling 12-month hail event
   - 4 = address within service area with weather activity outside target thresholds
   - 2 = address within service area, no recent weather
   - 0 = address outside service area

2. **Property age signal (0–10)**
   - 10 = roof age ≥ `target_profile.roof_age_premium` (default 20 yrs)
   - 8 = roof age ≥ `target_profile.roof_age_floor` (default 15 yrs)
   - 6 = age unknown BUT neighborhood median build year places likely age ≥ floor
   - 4 = roof age 10–15 yrs
   - 2 = roof age under 10 yrs
   - If age is unknown AND neighborhood context is absent, score 5 and flag for field verification

3. **Neighborhood density (0–10)**
   - 10 = one or more entries in `past_jobs.completed_addresses[]` on same street OR within named `canvassing.territories[]` cluster with active jobs
   - 7 = completed jobs within same ZIP within last 12 months
   - 4 = completed jobs within service area, not in same ZIP
   - 1 = no nearby completed work

4. **Interaction recency (0–10)**
   - 10 = inbound inquiry within last 7 days OR referred lead
   - 8 = inbound inquiry 7–30 days
   - 6 = active-estimate-not-signed 30–90 days
   - 4 = cold lead 90–180 days
   - 2 = cold lead 180–365 days
   - 0 = no prior interaction / purchased list / canvassing target
   - **Trigger refresh rule**: if a new weather event has occurred after the last-interaction date, add +3 to the recency score (cap 10) and note "trigger refresh" in the rationale

5. **Revenue potential (0–10)**
   - Map (estimated squares or known roof size) × `rates.average_job_value` per square to ticket size, then:
   - 10 = ticket ≥ 3× `target_profile.ticket_minimum`
   - 8 = 2× ticket_minimum
   - 6 = 1–2× ticket_minimum
   - 4 = at ticket_minimum
   - 2 = below ticket_minimum (repair-only, partial replacement)
   - If size is unknown, estimate from ZIP median home size and flag as "size estimated"

**Process:**

1. Normalize the input list into rows: lead_id, name, address, ZIP, known_age, known_size, last_interaction, notes
2. Enrich each row with weather overlay, hail-zone flag, territory flag, completed-jobs adjacency
3. Score each criterion with the rules above; compute composite and tier
4. Sort descending; apply batch-size output rules
5. Generate the recommended-action column in the voice from `config.yml`
6. Produce the top-5 summary narrative

**Output artifacts:**

### 1. Ranked Table

| Rank | Lead | Address | ZIP | Age | Size (sq) | Weather | Age | Nbr | Recency | Revenue | Composite | Tier | Recommended Action |
|------|------|---------|-----|-----|-----------|--------:|----:|----:|--------:|--------:|----------:|------|--------------------|
| 1 | Jane Doe | 123 Maple Ridge Dr | 75070 | 22 | 28 | 10 | 10 | 10 | 6 | 8 | 88 | 🔥 Hot | Call today — 1.5" hail, 22-yr roof, 4 completed jobs on block |
| 2 | … | | | | | | | | | | | | |

Columns for criterion scores are 0–10; composite is 0–100.

### 2. Tier Summary

- 🔥 **Hot** (composite ≥ 80): count, dispatch plan (calls today, doors tomorrow)
- 🟡 **Warm** (50–79): count, drip-sequence assignment
- ⚪ **Nurture** (< 50): count, move to long-term nurture list

### 3. Top-5 Narrative

For each of the top 5 leads, a 60–80 word paragraph covering: why this lead, the single strongest trigger, the suggested opening line in the config voice, and the fallback CTA if the primary ask is declined.

### 4. Out-of-Profile Flags

- Leads outside `service_area.zip_codes[]` — listed separately with "out of area" reason
- Leads where ticket estimate < `target_profile.ticket_minimum` but weather score is maxed — flag as "storm repair, low ticket" for a separate repair-crew queue
- Leads with age-unknown AND no neighborhood context — flagged for field verification before call-bank dispatch

**Output requirements:**

- Every composite score must decompose to the five criterion scores × weights (no black-box numbers)
- Recommended-action column uses the voice from `config.yml`, not generic AI prose
- Saved to `outputs/lead-scoring/{YYYY-MM-DD}-{batch-label}-scored.md` if user confirms, with a companion CSV at `outputs/lead-scoring/{YYYY-MM-DD}-{batch-label}.csv` for CRM import
- Batch size > 500: table is truncated to top 50; full CSV still written

**Efficiency notes:**

- If the user provides neither weather context nor a scoring priority, default to **Aging-Roof** profile and note the assumption
- If a config field is missing, default and flag — never block on config gaps
- Cross-reference: route 🔥 tier leads into `lead-response-automator` (instant outreach) and 🔥 storm leads into `storm-canvassing-prioritizer` (route clustering); 🟡 tier into `follow-up-sequence` (drip cadence); 🥉 Nurture into `maintenance-plan-generator` (subscription upsell)

## Example Output — 25-lead Storm-Response batch (2026-04-18 DFW hail, mixed 75070/75071/75074)

**Scenario:** Sales manager Sam Rivera pulls a 25-lead batch on the morning of 2026-04-19 (12 hours after the 4/18 hail event). Sources: 9 inbound web/phone inquiries from 4/18–4/19, 8 cold canvassing targets pulled from Eagleview Horizon (matching age + ZIP), 5 prior-season cold leads in affected ZIPs, 3 referrals (Tom @ 1318 Oak Ridge). Profile = **Storm-Response** (active hail event in service area).

**Resolved config fields:**
- `service_area.zip_codes[]` = [75070, 75071, 75074, 75075, 75033, 75002]; `.hail_zones[]` includes 75070 + 75074
- `canvassing.territories[]` → Maple Ridge (75070), Sunset Grove (75070), Riverside (75074), Willow Creek (75071)
- `target_profile.roof_age_floor` = 15, `.roof_age_premium` = 20, `.ticket_minimum` = $8,000
- `rates.average_job_value` = $620 / square installed (asphalt arch, 28-sq baseline)
- `past_jobs.completed_addresses[]` → 1318 Oak Ridge (Frisco, 75033), 1248 Maple Ridge (Frisco, 75070), 234 Elm Plano (75074), + 11 others
- `voice` = consultative

**Weather overlay (Storm-Response weights apply):** 4/18 hail swath, 1.5" peak in 75070 core / 1.25" tail in 75074 / 1.0" edge in 75071 / clipping 75075. Wind 61 mph peak.

---

### 1. Ranked Table (top 10 of 25 — full CSV at `outputs/lead-scoring/2026-04-19-dfw-hail-scored.csv`)

| Rank | Lead | Address | ZIP | Age | Size (sq) | W | A | N | R | $ | Composite | Tier | Recommended Action |
|------|------|---------|-----|----:|----------:|--:|--:|--:|--:|--:|----------:|------|--------------------|
| 1 | Janelle Doe | 1248 Maple Ridge Dr | 75070 | 22 | 28 | 10 | 10 | 10 | 10 | 8 | **97** | 🔥 Hot | Call today 9 AM — 1.5" hail, 22-yr roof, you've already done Tom's roof on the block; inbound 4/18 |
| 2 | Pat Nguyen | 4221 Cedar Hollow Ln | 75074 | 18 | 30 | 10 | 8 | 7 | 10 | 8 | **88** | 🔥 Hot | Call today — 1.25" hail, 18-yr roof, active leak inbound 4/18 21:47; route to lead-response-automator |
| 3 | Marcus Bell | 234 Elm St | 75074 | 21 | 28 | 10 | 10 | 10 | 6 | 8 | **88** | 🔥 Hot | Call today — completed neighbor; 6-recency (estimate 30-day refresh + storm trigger) |
| 4 | Greg Novak | 789 Oak Dr | 75070 | 17 | 16 | 10 | 8 | 7 | 4 | 4 | **66** | 🟡 Warm | Drop scope-difference visual + 72-hr window text; cold 90-180d with trigger refresh |
| 5 | Diana Chu | 456 Maple Ave | 75070 | 9 | 20 | 10 | 4 | 10 | 6 | 6 | **70** | 🟡 Warm | Email Maple Ridge proof + spring window angle; 9-yr roof low age score offset by hail + adjacency |
| 6 | Lisa Fortuna | 655 Cedar Ct | 75074 | 16 | 22 | 10 | 8 | 10 | 4 | 8 | **78** | 🟡 Warm | Phone callback + storm-trigger SMS; 4-recency from old cold list, refreshed by 4/18 |
| 7 | T. Maddox referral | 1318 Oak Ridge Dr | 75033 | — | 26 | 4 | 5 | 10 | 10 | 8 | **66** | 🟡 Warm | Referral — call today, low weather but 10-recency + adjacency carries; **flag size-est** |
| 8 | Sara Odom | 321 Pine Blvd | 75002 | 14 | 6 | 2 | 4 | 4 | 8 | 2 | **38** | ⚪ Nurture | Repair-queue route — out-of-storm-zone, repair-only ticket; flag for separate repair-crew queue |
| 9 | Cold lead 75070 #1 | (canv 75070 target) | 75070 | est 16 | est 24 | 10 | 8 | 10 | 0 | 6 | **66** | 🟡 Warm | Door knock under Maple Ridge cluster Day 1; **flag age-est + size-est** for field verify |
| 10 | Cold lead 75074 #1 | (canv 75074 target) | 75074 | est 19 | est 28 | 10 | 8 | 10 | 0 | 8 | **72** | 🟡 Warm | Door knock under Riverside cluster Day 1; **flag age-est + size-est** for field verify |

*Composite decomposition for Rank 1 (Janelle Doe):* Weather 10×35 + Age 10×20 + Neighborhood 10×15 + Recency 10×15 + Revenue 8×15 = 350 + 200 + 150 + 150 + 120 = **970 / 10 = 97**.

*Composite decomposition for Rank 2 (Pat Nguyen):* 10×35 + 8×20 + 7×15 + 10×15 + 8×15 = 350 + 160 + 105 + 150 + 120 = **885 / 10 = 88.5 → 88** (rounded down per convention).

### 2. Tier Summary

- 🔥 **Hot** (composite ≥ 80): **3 leads** — Janelle Doe (97), Pat Nguyen (88), Marcus Bell (88). Dispatch plan: Sam calls all three by noon today 2026-04-19; on-call estimator Marcus Patel for Pat Nguyen's active leak (route to lead-response-automator).
- 🟡 **Warm** (composite 50–79): **14 leads** — Diana Chu, Greg Novak, Lisa Fortuna, Tom Maddox referral, 8 cold-canvassing targets, 2 prior-season cold leads with trigger refresh. Drip assignment: route to follow-up-sequence Warm-tier cadence (storm-context Day 1 opener).
- ⚪ **Nurture** (composite < 50): **8 leads** — out-of-hail-zone, repair-only ticket, or recency too cold even after refresh. Move to long-term nurture list; re-score next Monday's pipeline review.

### 3. Top-5 Narrative

**1. Janelle Doe — 1248 Maple Ridge Dr (composite 97):** 22-yr roof in 1.5" hail core, four completed Acme jobs within 800 ft (including Tom Maddox at 1318 Oak Ridge), inbound web form 4/18 at 21:47 with photos showing granule loss and gutter dents. Opening line: "Janelle — Sam at Acme Roofing. Got your photos from 4/18 — those are textbook hail signals. Marcus, our HAAG-certified inspector, has a 2 PM slot at 1248 Maple Ridge today, and we've already worked 9 jobs in 75070 since the storm." Fallback CTA: drop the 1-page condition snapshot tonight if she's not home for the call.

**2. Pat Nguyen — 4221 Cedar Hollow Ln (composite 88):** 18-yr roof in 1.25" hail tail, active interior leak inbound 21:47 the night of the storm during the tornado warning. Open emergency tier — lead-response-automator already paged on-call estimator Marcus Patel; tonight's tarp + tomorrow's full inspection booked. This lead is in motion; the score documents priority for follow-up sequencing post-tarp.

**3. Marcus Bell — 234 Elm St (composite 88):** 21-yr roof in 1.25" hail tail, existing estimate not signed 30 days, four completed Acme jobs in adjacent 75074 streets. The trigger-refresh +3 lifted Recency from 6 (active-estimate 30-90d) to its capped value. Opening line: "Marcus — Sam at Acme. Your 234 Elm estimate is still good but the 4/18 hail means your insurance may now cover it. Want me to swing by and walk it with you Thursday morning?" Fallback CTA: text the 1-page insurance-supplement-writer angle.

**4. Lisa Fortuna — 655 Cedar Ct (composite 78):** 16-yr roof in 1.25" hail tail, cold lead from prior-season canvassing (recency was 4, refreshed to 7 by storm). Opening line: "Lisa — Sam at Acme Roofing. Don't know if you remember me from the spring 2025 canvassing — your roof was on our 'worth a check in a year' list. 4/18 hail in your ZIP was 1.25". 30-min walk + a written report on the house, no charge. Friday morning OK?" Fallback CTA: email + voicemail with the Riverside drone footage.

**5. Diana Chu — 456 Maple Ave (composite 70):** 9-yr roof in 1.5" hail core — younger roof than the target profile but neighborhood density (Maple Ridge cluster, 4 completed jobs within 800 ft) and active estimate-30d carry her into Warm. Cross-skill: she's already in follow-up-sequence Warm-tier cadence (see that skill's Example Output) — Diana is the canonical "getting other quotes" example. The storm refresh now gives Sam a legitimate Day-1 storm-context restart if she's still cold by the next outreach window.

### 4. Out-of-Profile Flags

- **Out-of-area** (excluded — outside `service_area.zip_codes[]`): 2 leads (Lewisville 75077, McKinney 75069). Listed in `outputs/lead-scoring/2026-04-19-dfw-hail-out-of-area.csv` with the "out of area — no licensed coverage in this ZIP" reason.
- **Storm repair, low ticket** (weather maxed but estimate < `target_profile.ticket_minimum` $8k): 3 leads (Sara Odom 321 Pine repair, two single-slope repair-only inquiries from 75074). Flagged to repair-crew queue rather than main pipeline; routed to Tran's Tier C crew rather than full sales engagement.
- **Age-unknown + no neighborhood context** (flagged for field verification before call-bank dispatch): 2 leads (cold canvassing targets in 75075 where assessor data is missing — assigned size_est = 5 default score with field-verify flag).

### 5. Process artifacts written

- `outputs/lead-scoring/2026-04-19-dfw-hail-scored.md` — this report
- `outputs/lead-scoring/2026-04-19-dfw-hail-scored.csv` — full 25-row CSV for CRM import (composite + decomposition + recommended action columns)
- `outputs/lead-scoring/2026-04-19-dfw-hail-out-of-area.csv` — 2-row out-of-area list
- Routing handoffs fired: 3 🔥 → lead-response-automator (immediate outreach), 14 🟡 → follow-up-sequence (Warm storm-context Day 1 cadence), 1 🔥 active-leak → also storm-canvassing-prioritizer (Riverside cluster brief), 8 ⚪ → maintenance-plan-generator (long-term nurture)

### Assumptions footer for this run

- Profile defaulted to **Storm-Response** because the 4/18 hail event was within the 30-day active-trigger window; weights applied per the Storm-Response column
- `service_area.zip_codes[]` resolved from config; 2 leads excluded as out-of-area
- `service_area.hail_zones[]` includes 75070 + 75074 — Weather scores ≥6 even on rolling-12-month basis for these ZIPs
- `target_profile.roof_age_floor` defaulted to 15 / `.roof_age_premium` to 20; ticket_minimum to $8,000
- `rates.average_job_value` defaulted to $620/sq installed (asphalt arch baseline); Revenue scores computed from (squares × $620)
- `past_jobs.completed_addresses[]` resolved 14 nearby completed jobs; adjacency scoring exercised for ranks 1–7
- Trigger-refresh +3 rule applied to Marcus Bell (active estimate >30d, capped at 10) and Lisa Fortuna (cold 4-recency lifted to 7)
- Output format = 50–500 condensed (top-10 detailed in table, full 25 in CSV, narrative on top 5 only) per batch-size auto-scaling rule
- 2 leads with age + size both estimated were flagged for field verification before dispatch
