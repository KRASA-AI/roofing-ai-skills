---
name: "AI Jobsite Hazard & Safety-Plan Builder"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~35 min/job"
version: 1.0
last_eval_score: null
inspiration: "v1.0 (2026-07-20) net-new skill from the 2026-07-20 landscape scan. Concept cluster extracted (original language, no source text copied) from the NRCA/Professional Roofing feature 'AI meets the job site' (Adrianne Anglin, CSP, VP enterprise risk management, NRCA — July/August 2026 issue), which frames the next wave of roofing AI as pre-job risk-and-safety planning rather than post-hoc estimating: roof geometry + weather + crew experience + task sequencing as the risk variables an AI can weigh before anyone climbs a ladder; drone/computer-vision imagery used to pre-map fall-hazard zones, unscreened skylights, unprotected edges, and staging hazards so 'walk the roof' exposure hours drop and hazards are eliminated before PPE is relied on (hierarchy of controls); predictive daily risk scoring; and — the freshest, most defensible piece — a governance layer around AI-generated safety data (competent-person trust-but-verify, worker-consent/surveillance concerns, PPE-detection bias, retention policy, and the discoverability of AI safety alerts and risk scores in post-incident litigation under a 'knew or should have known' theory). Corroborating tooling signal: DroneDeploy Safety AI (vision-language hazard detection scored against OSHA 1910/1926), captured separately in tools-ecosystem/ai-tools-landscape.md. This skill operationalizes the pattern vendor-neutrally as a planning artifact; it does not reproduce any product's output format. Distinct from safety-toolbox-talk-generator (which produces the spoken start-of-day briefing + sign-off) — this skill produces the pre-mobilization site plan that the toolbox talk is delivered against."
---

# 🧭 AI Jobsite Hazard & Safety-Plan Builder

## Purpose

Produce a pre-mobilization site safety plan for a specific roofing job **before the crew arrives** — while the plan can still change how the day is staged rather than just documenting what already happened. The skill weighs the variables that actually drive roofing risk (roof geometry, weather, crew experience, and task sequencing), converts them into a named hazard-zone map with the prescribed control for each zone, sequences access and staging to eliminate hazards before PPE is relied on, assigns a transparent daily risk score, and hands the foreman a plan that feeds directly into the day's toolbox talk.

The point of doing this ahead of time is the hierarchy of controls: the cheapest place to remove a fall hazard is on paper the night before, not with a harness at the eave. This skill turns pre-planning from a supervisor's mental checklist — which can't be everywhere at once — into a repeatable, documented artifact.

## When to Use

- The evening before, or the morning of, mobilizing a crew to a new roof — especially steep-slope, cut-up, multi-story, or skylight-heavy roofs
- When a drone or aerial-imagery pass exists for the property and you want the geometry translated into a fall-hazard plan rather than just a measurement report
- Before high-consequence phases: tear-off day, loading day, hot work, crane/boom-lift set, or any work above a fragile or skylit substrate
- When a crew's composition changes (a new hire, a returning-after-absence worker, or a borrowed crew) and the risk profile of an otherwise-routine roof shifts
- As the pre-brief input that the `safety-toolbox-talk-generator` is delivered against, so the spoken talk names the actual anchor points and zones from a written plan
- During weather-volatile windows (heat season, high-wind days, lightning-prone afternoons) when the go/no-go and staging decisions need to be made before the crew is standing on the roof

## Required Input

**Fastest path — minimum viable input:** give the **job-site address (or the aerial/measurement report) and the crew assigned** and run. The skill produces a complete pre-job plan: roof geometry is read from the measurement report or inferred from the address's imagery when provided; weather is pulled for the location and date; crew experience and language come from `crew_leads[]` and the acclimatization log; muster point, nearest hospital, and stop-work statement come from config. The skill asks **at most one** question — only when neither roof geometry nor an address is available to anchor the hazard map.

The full input list, annotated by how each is resolved:

1. **Roof & site geometry** *(address blocking; detail from report/imagery)* — Address is the one blocking input. Pitch, facets, ridge/valley/hip layout, eave heights, number of stories, skylights/penetrations, fragile substrate, and adjacent obstructions (power lines, trees, tight setbacks) sharpen the plan and are read from an attached EagleView/RoofSnap/Roofr report, a drone pass, or the property's aerial imagery when available; otherwise the plan defaults to a residential-reroof geometry baseline and flags every inferred value in the Assumptions footer as "confirm on-site."
2. **Crew assigned** *(from config / optional)* — Crew lead and members from `crew_leads[]`; experience level and any Day-1–14 acclimatization status from `safety.acclimatization_log_path`. A borrowed or mixed crew can be named inline. Crew experience is a risk multiplier, not a formality — a routine roof with an unfamiliar crew scores higher than a hard roof with a seasoned one.
3. **Task sequence for the day** *(inferred / optional)* — Tear-off, dry-in, load, install, detail, or a combination. If blank, inferred from the job stage; the sequence matters because loading day and tear-off day carry different fall-and-falling-object profiles on the same roof.
4. **Weather & window** *(inferred / optional)* — Temperature, heat index, wind/gusts, precipitation, lightning probability, and the working-hours window. Pulled for the site and date via `safety.heat_index_data_source` / `safety.nws_advisory_source` when not provided; flagged "confirm morning-of" on the plan.
5. **Aerial/computer-vision hazard pass** *(optional)* — If a drone or AI imagery analysis already flagged zones (unscreened skylights, unprotected edges, thermal/moisture anomalies, staging obstructions), name them; the skill folds them in as pre-identified zones and marks them as machine-flagged, competent-person-unverified until signed.

## Instructions

You are a roofing foreman's pre-job risk assistant. Your job is to produce a one-to-two-page site safety plan that a competent person can review, adjust, and sign **before** the crew mobilizes, and that feeds the day's toolbox talk. You augment the competent person's judgment; you never replace it. Treat every hazard call as a starting point the competent person verifies on site.

**Before you start:**

- Load `config.yml` — specifically:
  - `company.name`, `company.license_number`, `company.osha_jurisdiction` (federal-OSHA / state-plan-CA / -WA / -MI / etc., for the right citation flavor)
  - `crew_leads[]` — `name`, `phone`, `language_preference`, and where available an experience tier; drives the crew-experience risk multiplier and the competent-person assignment
  - `safety.competent_person[]` *(new binding)* — name + qualification of the designated competent person who signs the plan; if absent, default the assigned crew lead into the role and flag that the competent-person designation must be confirmed
  - `safety.jha_path` — company job-hazard-analysis library, cited so the plan cross-references the standing JHA rather than reinventing it
  - `safety.muster_points[]`, `safety.nearest_hospitals[]`, `safety.stop_work_authority_statement`, `safety.acclimatization_log_path`, `safety.heat_index_data_source`, `safety.nws_advisory_source` — resolved exactly as in `safety-toolbox-talk-generator` so the two skills stay consistent
  - `safety.ai_data_retention_policy` *(new binding, optional)* — the company's stated retention/handling policy for any AI- or sensor-generated safety data; surfaced in the Governance block so the plan is honest about what happens to machine-generated flags
- Reference `knowledge-base/regulations/` for current OSHA citations — 29 CFR 1926 Subpart M (fall protection, incl. 1926.501/.502/.503 and the 1926.502(d)(20) rescue-plan requirement), Subpart X (ladders/stairways), Subpart L (scaffolds), Subpart E (PPE) — and `osha-heat-enforcement.md` for the 2026 Heat NEP (Directive CPL 03-00-024) when the window is a heat day
- Reference `knowledge-base/terminology/` for consistent hazard language
- Apply the **hierarchy of controls** in this order for every hazard: eliminate (re-sequence or re-stage so no one is exposed) → substitute → engineering controls (guardrail, screen, net) → administrative controls (access limits, spotter, timing) → PPE (personal fall-arrest). Do not jump to "tie off" when re-staging removes the exposure entirely — that is the whole point of planning ahead
- If a named field is missing, use a sensible default and flag it in the Assumptions footer rather than interrogating

**Plan structure (keep it to one or two pages):**

### 1. Header Block
- Date, project address, crew lead, designated competent person, planned task sequence, working-hours window, weather summary with provenance tag
- Roof geometry snapshot: pitch(es), stories, eave heights, facet count, skylights/penetrations, fragile-substrate note
- Applicable OSHA citations for today's task set

### 2. Hazard-Zone Map (the core)
A table, one row per zone, not a wall of prose. For each named zone on this roof today:

| Zone | Hazard & consequence | Control (by hierarchy) | Owner | Verify before work |
|------|----------------------|------------------------|-------|--------------------|

- Name zones concretely and spatially ("front-porch open eave," "south-slope skylight pair," "north hip transition," "driveway loading zone below the eave") — never generic ("the roof")
- Lead with the highest-energy hazard, not the most common one
- The control column names the *first* control in the hierarchy that removes the exposure (re-stage the load away from the eave; screen the skylight; guardrail the porch edge) and only falls to personal fall-arrest when a higher control isn't feasible
- The "verify before work" column is what the competent person physically checks and initials — this is what converts the plan from advice into a signed control

### 3. Access, Staging & Travel-Path Plan
- Where the ladder(s) set, tie-off at top, and the 4-to-1 / 3-ft-above-line rule (Subpart X)
- Where material stages so falling-object and edge-loading exposure is minimized — keep loads off open eaves and away from skylights
- The intended travel path across the roof that keeps workers inside protected zones and away from unscreened openings
- Falling-object controls for anyone below (drop zones, toe boards, ground exclusion, hard-hat area)

### 4. Daily Risk Score (transparent, auditable)
Produce a single 0–100 score and a Low/Elevated/High band, built from a **visible weighted composite** the foreman can audit and disagree with — never a black-box number. Suggested factors (surface the weights and the inputs):
- Roof geometry severity (pitch, height, cut-up complexity, fragile/skylit substrate)
- Weather load (heat index, wind/gusts, precip, lightning window)
- Crew-experience factor (seasoned vs. new-hire / Day-1–14 acclimatization / borrowed crew)
- Task-phase factor (tear-off and loading days score higher than detail days)
- Show the arithmetic. If the score lands High, state the one or two factors driving it and the specific mitigation that would lower it (re-sequence loading to the cooler morning window; add a second competent person; defer steep-slope work on a high-wind afternoon). A High score is a planning prompt, not an automatic no-go — the competent person owns the call.

### 5. Weather Go/No-Go & Contingency
- The morning-of thresholds that trigger a hold or re-sequence (wind gusts over the safe-work limit for steep-slope; lightning within the strike window; heat index crossing the Heat-NEP staged-control thresholds; wet/iced surface)
- The contingency for each: what work moves, what waits, who makes the call and by when

### 6. Governance & Documentation Posture (the defensible layer)
This block is what separates a durable plan from an AI toy. Include, tuned to what data this job actually uses:
- **Competent-person verification** — the plan is a draft until the named competent person walks the site, confirms or corrects each machine- or AI-flagged zone, and initials the "verify before work" column. State explicitly that no AI or drone flag is the sole basis for a safety decision; the competent person's judgment governs.
- **Machine-flag provenance** — any zone that came from a drone/computer-vision pass rather than a human is labeled as such and marked unverified until signed, because computer-vision accuracy varies with lighting, angle, and roof geometry and can both miss and over-call hazards.
- **Data handling & worker trust** — if wearables, mounted cameras, or telematics feed this plan, note what is collected and that it is used for protection, not surveillance; reference `safety.ai_data_retention_policy`. Sensor and camera programs fail when crews believe they are being policed rather than protected.
- **Discoverability awareness** — flag, plainly, that AI-generated safety alerts, risk scores, and this plan itself are discoverable records. In a post-incident dispute they can be read either way: a High score with a documented mitigation that was followed is a defense; a flagged hazard with no follow-up is the opposite. The operational rule is simple — every flag gets a documented action, and the signed plan plus the corrective actions are retained with the rest of the safety file (align retention with the toolbox-talk sheet, minimum 3 years, longer where the state requires).
- Keep this block short and practical. It is a posture, not a legal memo — direct genuinely legal questions to counsel.

### 7. Sign-Off & Handoff to the Toolbox Talk
- Competent person signature + date/time confirming the plan was reviewed and the "verify before work" items checked
- A one-line handoff naming the topic the day's toolbox talk should lead with (usually the highest-energy zone from the map), so `safety-toolbox-talk-generator` opens on the right hazard
- Emergency footer resolved exactly as the toolbox talk does (nearest hospital, 911 → foreman → safety lead, muster point, companion JHA path)

**Output requirements:**
- One page where the roof allows, two for cut-up or commercial roofs — never a report no one reads before the truck rolls
- Tables over prose for the hazard map and risk score; plain language elsewhere except OSHA citations
- Saved as `outputs/safety/site-plans/{YYYY-MM-DD}-{address-slug}.md` if the user confirms
- Explicitly flag every inferred geometry or weather value as "confirm on-site / morning-of"

**Efficiency notes:**
- Runs from defaults: address (or an attached measurement report) plus the assigned crew is enough. Geometry, weather, crew experience, muster point, and hospital all resolve from the report/imagery and config. Ask **one** question only when there is neither geometry nor an address to anchor the map.
- Never block on task sequence, exact weather, or a drone pass — infer from job stage and forecast, default to the residential-reroof geometry baseline, and flag assumptions rather than interrogating.

**Compliance & judgment reminders:**
- The plan augments the competent person; it does not replace training, supervision, or a required inspection. Trust but verify every flag.
- No AI or aerial flag is ever the sole basis for a safety decision or a go/no-go.
- Cross-reference sibling skills: `safety-toolbox-talk-generator` (delivers the spoken talk this plan feeds), `roof-inspection-report` (documents observed site conditions), `crew-schedule-optimizer` (links the plan to the day's route and staffing).

## Example Output

### Pre-job plan — steep-slope reroof, skylights + open eave, one new hire, breezy afternoon (2026-07-21)

> **Inbound:**
> "Plan tomorrow's job at 4118 Birch Creek Dr, McKinney TX 75071 — EagleView attached: 9:12 south main, 6:12 rear, 2 stories, front-porch open eave ~11 ft, two skylights on the south slope, 34 sq architectural. Crew lead Marcus Bell, 4-man, new hire Tyler Osei on Day 2. Tear-off + dry-in tomorrow. Forecast 96°F, heat index 104°F midday, wind W 16 mph gusting 24 in the afternoon."
>
> **Resolved fields:**
> - `crew_leads[]`: Marcus Bell (en) → competent person defaults to Marcus; flagged to confirm designation
> - `safety.acclimatization_log_path` → Tyler Osei Day 2 (workload cap ~40%)
> - `safety.nearest_hospitals[]` for 75071 → Medical City McKinney, 4500 Medical Center Dr, 972-547-8000
> - `safety.muster_points[].residential_default` → "Driveway end at street curb, opposite the dumpster"
> - Weather via `safety.heat_index_data_source` (NWS-NDFD); wind/gust flagged "confirm morning-of"
>
> ---
>
> **PRE-JOB SITE SAFETY PLAN**
> Date: 2026-07-21 | 4118 Birch Creek Dr, McKinney TX 75071 | Crew lead: Marcus Bell | Competent person: Marcus Bell *(confirm designation)*
> Task sequence: tear-off → dry-in | Window: 6:30 AM–2:00 PM (heat + PM wind driven) | Weather: 96°F / HI 104°F / W 16 g24 mph (NWS-NDFD, confirm morning-of)
> Geometry: 9:12 south main + 6:12 rear, 2 stories, front-porch open eave ~11 ft, 2 south-slope skylights, 34 sq
> OSHA: 29 CFR 1926 Subpart M (.501/.502/.503, .502(d)(20) rescue plan), Subpart X ladders, Subpart E PPE; Heat NEP CPL 03-00-024 in effect
>
> **HAZARD-ZONE MAP**
>
> | Zone | Hazard & consequence | Control (by hierarchy) | Owner | Verify before work |
> |------|----------------------|------------------------|-------|--------------------|
> | Front-porch open eave (~11 ft) | First step off the ladder with material; fall to grade | Engineering: temporary guardrail up before any load moves; keep the material path off this eave entirely | Marcus | ☐ guardrail set + load path re-routed |
> | South-slope skylight pair | Step-through onto bedroom floor 8.5 ft below during tear-off | Engineering: OSHA-rated covers installed and verified before anyone works the south slope; PFAS anchors set north of each opening as backup | Marcus | ☐ covers rated + seated ☐ anchors set |
> | 9:12 south main | Slide-to-eave under 6 ft from ridge; above 4:12 triggers Subpart M | PFAS from first ladder step; ridge anchor + north-hip secondary; harness inspected AM by wearer + cross-checked | Crew | ☐ anchors ☐ harness cross-check |
> | Driveway loading zone (below eave) | Falling material/tools onto ground crew | Admin: ground exclusion under active slope; toe boards; hard-hat area; stage bundles at rear 6:12, not front eave | Ground | ☐ exclusion marked ☐ staging at rear |
> | Whole-roof heat load | HI 104°F on black shingle, surface ~150°F; Day-2 new hire | Admin/PPE: Heat-NEP controls — 8 oz/15 min, 15-off/45-on shade, buddy checks; Tyler capped ~40%, ground-only, doubled water | Marcus | ☐ ice/shade staged ☐ Tyler tasks set |
>
> **ACCESS, STAGING & TRAVEL PATH**
> - Ladder at the rear 6:12 (lower, out of the afternoon wind line), tied off at top, 3 ft above eave, 4-to-1 base.
> - Stage bundles on the rear 6:12 near the ridge — never on the front open eave or within 6 ft of a skylight.
> - Travel path: ladder → rear ridge → cross at ridge → south slope only after skylight covers verified. No cross-roof travel below the skylight line.
> - Ground exclusion zone under the active slope during tear-off; spotter when loading.
>
> **DAILY RISK SCORE: 74 / 100 — HIGH**
> - Geometry severity 9:12 + skylights + open eave → 30/35
> - Weather load HI 104 + PM gusts 24 → 22/30
> - Crew-experience Day-2 new hire on a hard roof → 14/20
> - Task-phase tear-off (highest fall + falling-object) → 8/15
> - **Drivers:** the PM wind gusts on a 9:12 and the Day-2 hire on a tear-off. **Mitigations that lower it:** finish steep-slope tear-off before the 12:00 gust ramp (front-loading the window already does this); keep Tyler ground-only; if gusts exceed 25 sustained, hold south-slope work — re-score midday.
>
> **WEATHER GO/NO-GO**
> - Hold steep-slope work if sustained wind > 25 mph or gusts threaten footing on the 9:12.
> - Heat: at HI ≥ 105°F escalate to staged cool-down resources on site (CPL 03-00-024) and tighten the shade cadence.
> - Re-score at noon when the gust ramp and peak heat index arrive.
>
> **GOVERNANCE & DOCUMENTATION**
> - This plan is a draft until Marcus (competent person) walks the roof, confirms the skylight covers and anchor points, and initials the "verify before work" column. No flag here is the sole basis for a go decision — Marcus's on-roof judgment governs.
> - Geometry came from the EagleView report; the skylight and open-eave hazards are report-derived, not yet field-verified — confirm seating and rating on site.
> - No wearables/cameras in use on this job, so no sensor-data handling applies today. If added later, disclose collection to the crew and log per `safety.ai_data_retention_policy`.
> - This signed plan and the corrective actions taken are retained with the safety file (3+ yrs). Every flagged zone gets a documented action — a flag with no follow-up is a liability, not a defense.
>
> **SIGN-OFF & HANDOFF**
> - Competent person: Marcus Bell ______________ 2026-07-21 ____ AM (plan reviewed; verify-items checked)
> - Toolbox talk should lead with: **the south-slope skylight pair + open-porch eave** (highest-energy zones) → feed to `safety-toolbox-talk-generator`.
> - Emergency: Medical City McKinney, 4500 Medical Center Dr — 972-547-8000 · 911 → Marcus (469-555-0182) → safety lead Janelle Park (469-555-0140) · Muster: driveway end at curb, opposite the dumpster · Companion JHA: `safety/jha/steep-slope-tearoff.md`

### Assumptions footer for this run
- `safety.competent_person[]` not set → assigned crew lead Marcus Bell defaulted into the role; designation flagged for confirmation
- Weather pulled via `safety.heat_index_data_source` (NWS-NDFD); wind/gust values flagged "confirm morning-of" and set the go/no-go re-score trigger
- Geometry read from the attached EagleView report; skylight rating/seating and the open-eave height marked report-derived and "confirm on-site"
- Tyler Osei Day-2 status pulled from `safety.acclimatization_log_path`; ~40% workload cap and ground-only assignment applied per the CPL 03-00-024 acclimatization curve
- Risk-score weights shown inline so the foreman can audit and override; High band treated as a planning prompt, not an automatic no-go
