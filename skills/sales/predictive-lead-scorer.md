---
name: "Predictive Lead Scorer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/batch"
version: 2.0
last_eval_score: 7.6
inspiration: "v2.0 rewrite from 2026-04-24 eval cycle — explicit weighted composite formula, named config fields (hail_zones, canvassing_territories, target_profile, rates.average_job_value), roof-age estimation rules, batch-size guidance, and trigger-refresh rules for storm-season re-scoring. v1.0 concept from 2026 AI-driven lead prioritization industry trend."
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

## Example Output

> [To be populated with a 25-lead batch example in next cycle — storm-response profile, mixed ZIPs, 2026-04-18 hail event overlay — to anchor format consistency.]
