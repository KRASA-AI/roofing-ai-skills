---
name: "Storm Canvassing Prioritizer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hours/storm event"
version: 1.1
last_eval_score: 8.7
inspiration: "v1.1 rewritten 2026-04-27 from eval improvement cycle — named config-field binding (canvassing.territories[].name / .zips / .streets / .priority_streets, canvassing.script_personality, team.canvasser_roster[].name / .languages / .vehicle / .shift_window, weather_rules.hail_size_threshold_in / .wind_threshold_mph, service_area.licensed_counties[]), populated 4/18 DFW hail Example Output (1.5 inch peak, 75070/75071/75074, 3 named clusters, agentic-platform tool-call pattern), and per-state cooling-off compliance lookup. v1.0 five-layer framework + agentic Eagleview Horizon orchestration preserved."
---

# 🌩️ Storm Canvassing Prioritizer

## Purpose

After a hail or wind event, turn the storm footprint into a prioritized canvassing plan in under an hour. Combines storm-track data, property intelligence (roof age, size, material, prior condition signals), and neighborhood density analysis to produce a ranked list of streets and individual addresses where canvassers and callers should spend the first 72 hours. Designed for shops that either use an agentic property-intelligence platform (e.g., Eagleview Horizon) or have to assemble the same picture from hail-maps, county assessor data, and drive-by observations.

## When to Use

- Within 12 hours of a confirmed hail or severe-wind event in the service area
- When a purchased storm-map report arrives and the sales manager needs to convert it into a door-by-door plan
- For pre-season storm readiness drills — rehearse the workflow on a simulated storm track so crews are already trained when the real one hits
- Between storm cycles, to refresh an evergreen canvassing heat map of aging roofs before the next event arrives
- Whenever a competitor's trucks appear in a neighborhood and the sales manager needs a same-day counter-plan

## Required Input

Provide the following:

1. **Storm footprint** — Hail swath (start/end coordinates, peak size, date/time), wind swath (peak gust, date/time), or the storm-map report file path. If only a city-level headline is available, say so and the skill will work from the approximate center with a degraded-confidence tag
2. **Service-area constraints** — Drive-time radius or ZIP whitelist; pulled from `service_area.zip_codes[]` and licensed-county filter from `service_area.licensed_counties[]` if not specified
3. **Crew and caller capacity** — Pulled from `team.canvasser_roster[]` if available; otherwise specify number of door-knockers + phone canvassers, shifts/day, daylight-only knocking constraint, HOA-restricted subdivisions
4. **Property intelligence access** — Which platforms the shop can call: Eagleview Horizon / Eagleview One API, Nearmap, CAPE Analytics, county assessor data, plain public search, or none (manual map only). This drives the depth of the per-address brief
5. **Priority criteria** — Target roof age cutoff (commonly 10+ years for hail, 15+ for wind), target roof size bracket, material preference, carrier-based targeting policy
6. **Existing CRM overlap** — A list of addresses already in the CRM (previous customers, prior estimates, active leads) so canvassing can skip or differently-script those contacts

## Instructions

You are a storm-response canvassing strategist. Your output is a ranked canvassing plan the sales manager can hand to a room of knockers and a caller bank on the morning after a storm.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.phone`, `company.canvassing_email_from` — sender identity for canvasser leave-behinds
  - `service_area.zip_codes[]` — outer envelope; addresses outside are excluded
  - `service_area.licensed_counties[]` — the legally operational area (often narrower than the ZIP envelope); addresses inside ZIP but outside licensed county are flagged not-knocked
  - `service_area.hail_zones[]` — historical hail-prone ZIPs, used as a tie-breaker when the live storm overlay is ambiguous
  - `canvassing.territories[]` — each with `name`, `zips[]`, `streets[]` (optional, for cul-de-sac / grid-block fidelity), `priority_streets[]`, `last_canvassed_date`, `last_competitor_seen_date`. Drives the named clusters in the ranked plan
  - `canvassing.script_personality` — one of: `consultative_neighbor` / `urgency_first_responder` / `quiet_documenter`. Picks the opening-line tone in each cluster brief; falls back to `voice` if missing
  - `canvassing.cooling_off_states[]` — states with mandatory cooling-off / disclosure rules (e.g., `["TX", "MI", "OK", "CO", "GA"]`); when the storm falls inside one, the per-cluster brief auto-includes the state-specific compliance line
  - `team.canvasser_roster[]` — each with `name`, `languages[]` (e.g., `["en", "es"]`), `vehicle` (truck / van / on-foot), `shift_window` (e.g., `"10:00-18:00"`), `day_capacity` (default 50 doors)
  - `team.phone_canvasser_roster[]` — same shape, for the call-bank
  - `weather_rules.hail_size_threshold_in` (default 1.0), `weather_rules.wind_threshold_mph` (default 60), `weather_rules.structural_wind_threshold_mph` (default 70)
  - `past_jobs.completed_addresses[]` — for adjacency proof in the per-cluster brief social-proof line
  - `voice` (fallback for canvassing.script_personality) — communication tone
  - If a named field is missing, use a sensible default and flag it in the output's Assumptions footer
- Reference `knowledge-base/industry-overview.md` for the post-storm 72-hour AI-assistant query spike and the first-responder advantage window
- Reference `knowledge-base/tools-ecosystem/ai-tools-landscape.md` for the agentic property-intelligence landscape and the adjacent voice-AI canvassing-handoff tools (Leaping AI, VoiceDrop, Alivo)
- Reference `knowledge-base/regulations/osha-heat-enforcement.md` if the storm falls inside the summer enforcement window

**Research framework — five layers per neighborhood:**

### Layer 1: Damage Probability
Score each neighborhood 0–100 on likelihood of real damage based on:
- Overlap with hail swath (use `weather_rules.hail_size_threshold_in` — default 1.0in qualifies, 1.5in flags most insurers)
- Peak wind gust within the polygon (`weather_rules.wind_threshold_mph` for shingle lift, `weather_rules.structural_wind_threshold_mph` for structural)
- Distance from storm centerline (damage falls off sharply at the edges)
- Canopy cover (heavy tree cover can mask damage but also produce impact debris)
- Topography (windward slopes take more punishment than leeward)

### Layer 2: Roof Stock Suitability
For each neighborhood, summarize the property intelligence:
- Median roof age in the target bracket
- Median roof size (commercial-adjacent zones routed to `commercial-prospect-researcher`)
- Dominant roof material (3-tab asphalt highest yield on hail; metal/tile low-volume, high-ticket)
- Prior-condition signals — granule loss, worn flashing, moss/staining

### Layer 3: Access & Receptivity
- Street layout, owner-occupied share, length-of-tenure, median home value bracket
- Competitive heat (other roofing trucks reported, `canvassing.territories[].last_competitor_seen_date`)
- HOA / gated-community flags
- Drive-through clustering potential

### Layer 4: Legal & Ethical Guardrails
- If the storm state is in `canvassing.cooling_off_states[]`, surface the mandatory disclosure / cooling-off line for the per-cluster brief
- Exclude properties with posted no-solicitation signs from door-knock lists (mailable/callable)
- Flag CRM-existing addresses so the script does not imply a cold outreach
- Confirm `service_area.licensed_counties[]` only

### Layer 5: Outreach Channel Assignment
- **Door knock** — 10+ year roofs within a drive-through cluster, accessible frontage, owner-occupied
- **Phone call with AI voice-canvassing handoff** — High-value outliers / non-door-accessible
- **Direct mail / voicemail drop** — Whole-swath coverage at lower cost-per-touch
- **Digital retargeting** — Match list for Meta/Google campaigns running same week

**Output structure:**

### Section 1: Storm Summary (1 page)
- Storm headline, date, hail size or peak wind, polygon size, estimated impacted rooftops within service area
- Confidence band on damage probability (Green >70%, Yellow 40–70%, Red <40%)
- First-responder window — the 72-hour mark from storm time

### Section 2: Ranked Neighborhood Plan
A table with one row per `canvassing.territories[]` cluster within the storm overlap (extend with manually-added clusters if territories don't cover the whole storm):

| Rank | Territory (name) | Damage Score | Target Roof Count | Roof-Stock Fit | Access Score | Channel Mix | Day-1 Touches |
|------|------------------|--------------|-------------------|----------------|--------------|-------------|---------------|

### Section 3: Per-Cluster Canvassing Brief
For each cluster in the top tier (🔥 priority), produce a 100–150 word brief covering:
- Opening line in `canvassing.script_personality` voice, tuned to this cluster's trigger
- Key drive-in identifier (granule line at the gutter, lifted ridge caps, dislodged downspouts, tree debris)
- Likely objection + one-sentence response
- Hand-off moment to phone bank or AI voice agent
- Required compliance language for the operating state if in `canvassing.cooling_off_states[]`
- Adjacent social-proof from `past_jobs.completed_addresses[]`

### Section 4: Day-by-Day Schedule
- Day 1 (storm day +1): Top three 🔥 clusters, full crew + caller bank
- Day 2 (+2): Remaining 🔥 + top 🟡
- Day 3 (+3): 🟡 + phone-first tail
- Days 4–7: Mail/voicemail drop + digital retargeting
- Weekly refresh against CRM conversions

### Section 5: Measurement Plan
- Touches per canvasser per day (target: 40–60 doors, scaled by `team.canvasser_roster[].day_capacity`)
- Contact rate per touch (target: 35–50%)
- Inspection booking rate per contact (target: 20–35%)
- 30-day closed-deal rate per inspection — the final metric

**Agentic-platform orchestration note:**
If the shop has access to an agentic property-intelligence platform with MCP exposure, the research layers can be invoked as tool calls rather than manually assembled. A typical prompt pattern is: "Return every roof over {age_threshold} years old, {size_range}, within {radius} of polygon {storm_id}, filtered to {material_list}, excluding {crm_overlap_list}, grouped into drive-through clusters of {cluster_size}." When the platform is unavailable, this skill produces the same structure from map + assessor + drive-by inputs, flagged with lower confidence.

**Output storage:**
- Save the plan as `outputs/storm-canvassing/{YYYY-MM-DD}-{storm-name}-plan.md`
- Save the per-cluster briefs as individual files under `outputs/storm-canvassing/{YYYY-MM-DD}-{storm-name}/`
- Save the measurement-plan template pre-populated with cluster targets

**Guardrails:**
- This skill produces a canvassing plan — not a damage assessment. Actual inspection findings on each home override the plan's scoring
- Do not produce scripts that imply damage has been confirmed on a specific property until a physical inspection has been performed
- Respect state-specific registered-contractor, insurance-disclosure, and cooling-off period rules; the compliance line in Section 3 is not optional when in `canvassing.cooling_off_states[]`
- Ethical canvassing produces more long-term referrals than aggressive canvassing — the priority list is about targeting accuracy, not pressure

## Example Output (4/18 DFW hail, 1.5in peak, 75070/75071/75074)

```
ACME ROOFING — POST-STORM CANVASSING PLAN
Storm:    NOAA event 20260418-DFW-HAIL-117
Date:     2026-04-18 18:42 CT
Peak:     1.5" hail (Plano core), 1.0–1.25" tail (Frisco / McKinney south)
Wind:     61 mph peak gust (under structural threshold but above shingle-lift)
Polygon:  75070, 75071, 75074, 75075 partial — ~14,200 rooftops in service area
Window:   72-hr first-responder = expires 2026-04-21 18:42 CT

CONFIDENCE BANDS
- 🟢 75070 core (Maple Ridge / Sunset Grove): >75% real-damage probability
- 🟢 75074 core (Riverside): 70%
- 🟡 75071 (Willow Creek): 55% — mix of newer construction, more 1-tab metal
- 🟡 75075 partial: 45% — partial coverage, edge of swath

RANKED NEIGHBORHOOD PLAN

| Rank | Territory          | Dmg | Target Roofs | Roof-Stock Fit            | Access | Channel Mix              | Day-1 |
|------|--------------------|----:|-------------:|---------------------------|-------:|--------------------------|------:|
| 1 🔥 | Maple Ridge (75070)|  86 |          142 | Strong — 3-tab/arch 15–22y|  9/10  | Door + AI voice follow   |  90 d |
| 2 🔥 | Sunset Grove (75070)| 78 |           98 | Strong — arch 12–18y      |  8/10  | Door + mail tail         |  70 d |
| 3 🔥 | Riverside (75074)  |  74 |          118 | Mixed — 60% 3-tab, 40% arch| 8/10  | Door + phone hybrid      |  80 d |
| 4 🟡 | Willow Creek (75071)| 64 |          210 | Mixed (metal/shingle)     |  7/10  | Phone-first + door tail  | 120 c, 40 d |
| 5 🟡 | Stonebridge (75075)| 48 |           76 | Newer construction, low fit| 6/10  | Mail + digital retarget  |   0 d |

PER-CLUSTER BRIEF — 🔥 #1 MAPLE RIDGE (75070)

Opening line (script_personality = consultative_neighbor):
  "Hi, I'm Jamie with Acme Roofing — I just got off a roof on Oak Ridge two streets
  over. The 4/18 hail dropped 1.5-inch stones across this whole pocket and we're
  seeing classic functional damage on the 3-tab and architectural roofs from your
  build era. No charge to walk your roof and give you a written report — homeowner
  decides what to do with it."

Drive-in identifier: granule line at the eave-side gutter, lifted ridge cap on the west
  slope (winds came from the WSW), dented gutter face at the corner downspouts.

Likely objection: "My neighbor's insurance already denied theirs."
Response: "That's a common first answer in week one. The strike count per slope on
  the actual roof determines the file — we put it on paper so the carrier has to
  evaluate the specific evidence rather than the swath estimate."

Hand-off moment: "If you'd like the full report on paper, our scheduling line will
  set up a 45-minute walk in the next 24 hours — I can text you that link right now."

Compliance line (TX is in canvassing.cooling_off_states[]):
  "TX requires a 3-day right of rescission on any contract signed at the door. You
  don't sign anything today — this is just a free inspection scheduling."

Adjacent social-proof (from past_jobs.completed_addresses[]):
  "We installed at 1318 Oak Ridge Dr (Tom Maddox, GAF Timberline UHDZ) in March 2025
  — happy to point you to that roof on the way in if you'd like to see the work."

PER-CLUSTER BRIEF — 🔥 #2 SUNSET GROVE (75070) — [follows same structure]

PER-CLUSTER BRIEF — 🔥 #3 RIVERSIDE (75074) — [follows same structure]

DAY-BY-DAY SCHEDULE

| Day | Date       | Knockers (8) Coverage                | Phone Bank (3) Coverage          |
|-----|------------|--------------------------------------|----------------------------------|
| 1   | 2026-04-19 | Maple Ridge (5) + Sunset Grove (3)   | Riverside warm-up (60 calls)     |
| 2   | 2026-04-20 | Riverside (5) + Maple Ridge tail (3) | Willow Creek phone-first (90)    |
| 3   | 2026-04-21 | Willow Creek door-tail (4) + recanvass missed (4) | Stonebridge mail follow-up (60) |
| 4–7 | 2026-04-22+| Mail drop full polygon (~9,500 pieces) + Meta retargeting match list (6,400) | |

CANVASSER ASSIGNMENTS (from team.canvasser_roster[])

| Canvasser   | Languages  | Vehicle | Shift       | Day-1 cluster      | Cap   |
|-------------|------------|---------|-------------|--------------------|-------|
| Jamie Reed  | en         | truck   | 10:00–18:00 | Maple Ridge        | 50    |
| Luis Mora   | en, es     | van     | 10:00–18:00 | Maple Ridge (es)   | 50    |
| ...                                                                          |

MEASUREMENT TARGETS
- Day 1 doors knocked: 240 (90 + 70 + 80)
- Contact rate target: 40% → 96 contacts
- Inspection booking target: 28% → 27 inspections booked
- 30-day closed deal target: 35% of inspections → 9 deals from Day 1 alone

ASSUMPTIONS
- canvassing.territories[] confirmed for Maple Ridge / Sunset Grove / Riverside / Willow Creek / Stonebridge
- canvassing.cooling_off_states[] = ["TX", "MI", "OK", "CO", "GA"] — TX matched
- weather_rules.hail_size_threshold_in defaulted to 1.0; .wind_threshold_mph to 60
- team.canvasser_roster[] resolved 8 door knockers + 3 phone canvassers; bilingual coverage on 2
- past_jobs.completed_addresses[] yielded 14 nearby completed jobs across the 5 territories
```

(Run with your own storm data + config to replace these illustrative values.)
