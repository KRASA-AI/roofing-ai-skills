---
name: "Storm Canvassing Prioritizer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hours/storm event"
version: 1.0
last_eval_score: null
inspiration: "April 21, 2026 launch of agentic property-intelligence platforms (Eagleview Horizon-class) that expose Model-Context-Protocol tools for filtering rooftops by age, size, condition, and storm proximity — extracted as a workflow pattern for post-storm canvassing prioritization. No source content copied; concept reinterpreted as a roofer-facing deliverable."
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
2. **Service-area constraints** — Drive-time radius or zip-code whitelist the shop will actually dispatch to. Exclude zip codes with no licensed permit or insurance authorization
3. **Crew and caller capacity** — Number of door-knockers available, number of phone canvassers, shifts per day, and any hard limits (e.g., daylight-only knocking, local noise ordinances, HOA-restricted subdivisions)
4. **Property intelligence access** — Which of the following the shop can call: Eagleview Horizon / Eagleview One API, Nearmap, CAPE Analytics, county assessor data, plain public search, or none (manual map only). This drives the depth of the per-address brief
5. **Priority criteria** — Target roof age cutoff (commonly 10+ years for hail, 15+ for wind), target roof size bracket, material preference (3-tab vs architectural vs metal vs tile — each responds differently to hail), and any carrier-based targeting (e.g., skip known direct-repair-network properties or prioritize them, depending on the shop's posture)
6. **Existing CRM overlap** — A list of addresses already in the CRM (previous customers, prior estimates, active leads) so canvassing can skip or differently-script those contacts

## Instructions

You are a storm-response canvassing strategist. Your output is a ranked canvassing plan the sales manager can hand to a room of knockers and a caller bank on the morning after a storm.

**Before you start:**
- Load `config.yml` from the repo root for company service area, license/insurance zones, canvasser roster, and brand voice
- Reference `knowledge-base/industry-overview.md` for the post-storm 72-hour AI-assistant query spike and the first-responder advantage window
- Reference `knowledge-base/tools-ecosystem/ai-tools-landscape.md` for the agentic property-intelligence landscape and the adjacent voice-AI canvassing-handoff tools (Leaping AI, VoiceDrop, Alivo)
- Reference `knowledge-base/regulations/osha-heat-enforcement.md` if the storm falls inside the summer enforcement window — storm chasers climbing roofs in extreme heat still need the compliance trail

**Research framework — five layers per neighborhood:**

### Layer 1: Damage Probability
Score each neighborhood 0–100 on likelihood of real damage based on:
- Overlap with hail swath (1-inch hail at least, 1.5-inch flags most insurers)
- Peak wind gust within the polygon (60+ mph for shingle lift, 70+ for structural)
- Distance from storm centerline (damage falls off sharply at the edges)
- Canopy cover (heavy tree cover can mask damage but also produce impact debris)
- Topography (windward slopes take more punishment than leeward)

### Layer 2: Roof Stock Suitability
For each neighborhood, summarize the property intelligence:
- Median roof age in the target bracket (flag if >60% of roofs are in the target-age range)
- Median roof size (flag commercial-adjacent zones separately — those feed the `commercial-prospect-researcher`, not residential canvassing)
- Dominant roof material (3-tab asphalt is the highest-yield retail target on hail events; metal and tile are low-volume but high-ticket)
- Prior-condition signals where available — granule loss, worn flashing, moss/staining visible in last aerial pass

### Layer 3: Access & Receptivity
Rate each neighborhood on logistics:
- Street layout (cul-de-sacs and grid both work; long country driveways drop door-knock productivity)
- Homeowner demographic signals (owner-occupied share, length-of-tenure, median home value bracket — informs script choice but not targeting exclusion)
- Competitive heat (how many other roofing trucks or canvassers have been reported in the area)
- HOA / gated-community flags (require a different approach, not necessarily skip)
- Drive-through clustering potential — how many target-age roofs can a canvasser physically reach in one day without backtracking

### Layer 4: Legal & Ethical Guardrails
- Confirm state-specific post-storm canvassing rules — several storm-prone states (TX, MI, OK, CO, GA) have mandatory disclosure or cooling-off periods that must be scripted in
- Exclude properties with posted no-solicitation signs from door-knock lists (can still be mailed or called)
- Flag any addresses with recent loss history already in the CRM so the script does not imply a cold outreach
- Confirm licensed counties / municipalities only (the map is larger than the legally operational area)

### Layer 5: Outreach Channel Assignment
Assign each address to the right first-touch channel:
- **Door knock** — 10+ year roofs within a drive-through cluster, accessible frontage, owner-occupied
- **Phone call with AI voice-canvassing handoff** — High-value outliers that break the cluster geometry or non-door-accessible properties (gated, long driveways)
- **Direct mail / voicemail drop** — Whole-swath coverage at lower cost-per-touch for the tail of the target list
- **Digital retargeting** — Match list for Meta/Google campaigns running the same week

**Output structure:**

### Section 1: Storm Summary (1 page)
- Storm headline, date, hail size or peak wind, polygon size, estimated impacted rooftops within the service area
- Confidence band on damage probability (Green >70%, Yellow 40–70%, Red <40%)
- First-responder window — the 72-hour mark from storm time

### Section 2: Ranked Neighborhood Plan
A table with one row per neighborhood or canvassing cluster:

| Rank | Neighborhood / Polygon | Damage Score | Target Roof Count | Roof-Stock Fit | Access Score | Channel Mix | Estimated Day-1 Touches |
|------|------------------------|--------------|-------------------|----------------|--------------|-------------|-------------------------|
| 1 | 🔥 Maple Ridge | 86 | 142 | Strong (3-tab, 15–22 yrs) | 9/10 | Door + AI voice follow | 90 doors |
| 2 | 🔥 Sunset Grove | 78 | 98 | Strong (architectural, 12–18 yrs) | 8/10 | Door + mail tail | 70 doors |
| 3 | 🟡 Willow Creek | 64 | 210 | Mixed (metal/shingle) | 7/10 | Phone-first + door | 120 calls, 40 doors |

### Section 3: Per-Cluster Canvassing Brief
For each cluster in the top tier (🔥 priority), produce a 100–150 word brief covering:
- Opening line the canvasser or caller leads with, tuned to this cluster's trigger (hail size, wind gust, neighbor patterns)
- Key identifier the canvasser should look for on the drive-in (granule line at the gutter, lifted ridge caps, dislodged downspouts, tree debris patterns)
- Objection that's likely in this cluster (e.g., "my neighbor's insurance already denied theirs") and the one-sentence response
- Script hand-off moment to the phone bank or AI voice agent for appointment booking
- Required compliance language for the operating state (must-say phrases, mandatory disclosure)

### Section 4: Day-by-Day Schedule
- Day 1 (storm day +1): Top three 🔥 clusters, full crew + caller bank, priority AI voice agent coverage
- Day 2 (+2): Remaining 🔥 clusters + top 🟡
- Day 3 (+3): 🟡 clusters and phone-first tail list
- Days 4–7: Mail/voicemail drop on the remaining service-area addresses, digital retargeting on match lists
- Weekly refresh: Re-run the prioritizer against CRM conversions so subsequent touches skip converted homes

### Section 5: Measurement Plan
- Touches per canvasser per day (target: 40–60 doors depending on cluster density)
- Contact rate per touch (target: 35–50% depending on time-of-day and demographic)
- Inspection booking rate per contact (target: 20–35% inside the 72-hour window)
- 30-day closed-deal rate per inspection booked — the final metric the prioritizer is judged on

**Agentic-platform orchestration note:**
If the shop has access to an agentic property-intelligence platform (Eagleview Horizon or similar with Model-Context-Protocol exposure), the research layers above can be invoked as tool calls rather than manually assembled. A typical prompt pattern to the platform agent is: "Return every roof over {age_threshold} years old, {size_range}, within {radius} of polygon {storm_id}, filtered to {material_list}, excluding {crm_overlap_list}, grouped into drive-through clusters of {cluster_size}." The output of the tool call becomes the direct input to Section 2. When the platform is unavailable, this skill produces the same structure from map + assessor + drive-by inputs, flagged with lower confidence.

**Output storage:**
- Save the plan as `outputs/storm-canvassing/{YYYY-MM-DD}-{storm-name}-plan.md`
- Save the per-cluster briefs as individual files under `outputs/storm-canvassing/{YYYY-MM-DD}-{storm-name}/`
- Save the measurement-plan template pre-populated with cluster targets so the daily stand-up has a scoreboard

**Guardrails:**
- This skill produces a canvassing plan — not a damage assessment. Actual inspection findings on each home override the plan's scoring
- Do not produce scripts that imply damage has been confirmed on a specific property until a physical inspection has been performed
- Respect state-specific registered-contractor, insurance-disclosure, and cooling-off period rules; the compliance line in Section 3 is not optional
- Ethical canvassing produces more long-term referrals than aggressive canvassing — the priority list is about targeting accuracy, not pressure

## Example Output

> [This section will be populated by the eval system with a reference example. Run with sample input to see output quality.]
