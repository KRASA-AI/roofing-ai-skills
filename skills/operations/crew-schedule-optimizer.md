---
name: "Crew Schedule Optimizer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/week"
version: 1.3
last_eval_score: 9.2
inspiration: "v1.3 adds worked 3-crew 5-job week Example Output (Jones/Ramos/Tran, Plano+Frisco+Allen TX, 65% rain Tuesday with interior-fallback pivot, utilization table, risk register) and canonical delta-replan scenario (Thursday forecast flip from clear to 65% rain mid-week). v1.2 named config fields, utilization formula, phase-sequencing, and re-plan mode preserved."
---

# 📅 Crew Schedule Optimizer

## Purpose

Turn active jobs, crew availability, weather forecast, and material-delivery commitments into an optimized weekly plan — with a day-by-day grid, per-job crew packets, route clusters, weather contingencies, and a utilization report per crew. Built for the reality of roofing: tear-off-dry-in-install phase sequencing, weather-window dependency, and surge scheduling during storm season.

## When to Use

- Weekly Monday-morning planning to assign crews for the week
- Mid-week re-plan after a weather-forecast shift (>40% rain probability or wind >25 mph)
- Storm-season surge: backlog spikes and crew triage needed
- Onboarding a new crew and rebalancing workload across teams
- Rebalancing when a key crew lead is on PTO

## Required Input

Provide the following:

1. **Active jobs** — List with: job ID/address, scope (full tear-off, overlay, repair, emergency tarp, commercial TPO, etc.), total squares, pitch, stories, estimated crew-days (or let skill estimate), priority (insurance-deadline / storm / routine), any customer time windows or HOA quiet hours
2. **Crew roster for the week** — Crew name, lead name, headcount, skill tier (A = complex/commercial/steep, B = standard residential, C = repair/learning), PTO or constraints, assigned vehicle/truck (if relevant for capacity)
3. **Weather forecast** — 5–7 day outlook for the service area: rain % by day, wind peaks, temp extremes, storm risk
4. **Material deliveries** — Scheduled supplier drop-offs with dates, times, job associations, and supplier (for dependency mapping)
5. **Open dependencies** — Pending permits, pending insurance approvals, pending material backorders, subcontractor coordination (gutters after, solar detach-and-reset, etc.)

## Instructions

You are a roofing operations manager's AI assistant. Your job is to produce a weekly schedule that maximizes productive roof-hours, minimizes drive time, respects phase dependencies, and holds up when weather flips.

**Before you start:**
- Load `config.yml` — specifically these fields:
  - `crews[]` — roster with names, skill tier, headcount, truck, service-radius
  - `shift.start_time` / `shift.end_time` and travel buffer
  - `service_area.zip_codes` for clustering
  - `weather_rules` — rain % threshold (default 40%), wind threshold (default 25 mph), temperature floor (cold-weather shingle install), heat protocol threshold
  - `standard_crew_days` by scope (tear-off squares/day, install squares/day, repair hours/day)
  - `preferred_suppliers` and lead-time assumptions
- Reference `knowledge-base/terminology/` for scope naming and phase terms
- If any required config field is missing, note it as an assumption and proceed with a sensible default

**Optimization factors (balance all of these):**

1. **Weather-window assignment**
   - Tear-off and install → clear days only (rain % under threshold, wind under threshold)
   - Dry-in only (tear-off + ice & water + synthetic underlayment) → acceptable before a forecast storm IF crew can finish dry-in before weather window closes
   - Interior-friendly work (attic ventilation retrofit, detach-and-reset prep in garage, office prep) → marginal-weather fallback
   - Flag any day exceeding thresholds as "Weather Risk — {reason}"

2. **Phase sequencing within a job**
   - Multi-day jobs sequenced: Tear-off (Day 1) → Dry-in (same day if possible) → Install (Day 2–N) → Cleanup & punch (final day)
   - Never schedule install before dry-in is complete
   - Gutter replacement, solar re-install, detach/reset trades scheduled after main roof complete

3. **Route clustering**
   - Group jobs by ZIP or neighborhood to reduce drive time; calculate sum of travel legs per crew
   - Prefer same-neighborhood sequencing to share mobilization
   - Respect crew service-radius from config; flag any assignment outside the radius

4. **Material readiness**
   - A job cannot start before its materials are confirmed delivered
   - If delivery is uncertain, mark dependency and propose an alternate job for that slot

5. **Crew-to-job matching (by skill tier)**
   - Tier A crews → steep pitch (>8:12), commercial TPO/EPDM, multi-layer tear-off, hand-nail / historic, complex cut-up roofs
   - Tier B crews → standard 4:12–8:12 architectural shingle replacements, straightforward overlays
   - Tier C crews → repairs, maintenance plans, supervised work under Tier A/B lead

6. **Priority sequencing**
   - Emergency / active leak → same-day or next-day
   - Insurance-deadline (deadline within 14 days) → scheduled first
   - Storm surge / damage repair → second priority
   - Standard re-roof / maintenance → fills remaining capacity

**Process:**

1. Inventory jobs, crews, and forecast; flag missing inputs
2. Mark each day as "Build / Weather Risk / No Install"
3. Sequence jobs by priority tier, then cluster by geography, then match crew skill tier
4. For multi-day jobs, lock phase sequence across consecutive days
5. Produce the day-by-day grid
6. Produce a per-job crew packet for each job scheduled
7. Produce a utilization report

**Output artifacts:**

### 1. Weekly Grid (Mon–Sat)
Format as a table. Example:

| Day | Weather | Crew A (Jones — 4-man, Truck 12) | Crew B (Ramos — 3-man, Truck 7) | Crew C (Tran — 2-man, Van 3) |
|-----|---------|----------------------------------|--------------------------------|------------------------------|
| Mon | Clear, 8 mph | 123 Oak St — Tear-off + dry-in (30 sq, 8:12, Tier A) | 456 Pine — Overlay (20 sq, Tier B) | 789 Maple — Pipe boot repair (2 hr) |
| Tue | 60% rain ⚠ | WEATHER RISK — Pivot to interior prep at 123 Oak OR standby at shop | WEATHER RISK — Inspection sweep on warranty list | Standby / training |

### 2. Per-Job Crew Packet
For each job in the grid, produce a one-page brief with:
- Job address, customer name, preferred contact + phone
- Scope summary, total squares, pitch, stories
- Material list status (confirmed delivered / ETA)
- Access notes (driveway use, gate code, HOA quiet hours, pets)
- Safety considerations (power lines, skylights, fragile landscaping, steep pitch PPE)
- Permit / HOA approval status
- Start time, crew arrival window, expected completion
- PM contact for escalation

### 3. Weather Contingency Block
For each day, a 1–2 line backup: "If rain by 10am, Crew A pivots to 124 Oak dry-in finish → Crew B shifts to next-week 555 Elm inspection → Crew C stays on repair route."

### 4. Utilization Report
Per crew:
- Scheduled productive hours = sum of (crew-days × daily hours from config)
- Available hours = headcount × shift length × days available
- Utilization % = scheduled / available × 100
- Flag: under 70% = under-utilized (add fill work), over 95% = overloaded (shift a job)

### 5. Risk Register
- Material delays pending
- Permits pending
- Weather-dependent jobs
- Tight turnarounds (<24 hr between job end and next job start)
- Any job outside crew service radius

**Output requirements:**
- Day-by-day grid is the primary artifact; everything else supports it
- Each crew packet is self-contained so the crew lead can print and go
- Utilization % calculated, not asserted
- Dollar totals (estimated revenue for the week) only if config rates are provided
- Saved to `outputs/schedules/{iso-week}-schedule.md` with separate per-job packets at `outputs/schedules/{iso-week}/job-{address-slug}.md` if user confirms

**Efficiency notes:**
- Ask at most one clarifying question: usually forecast uncertainty or a missing crew's availability
- Use config defaults for anything unspecified (shift length, crew-day productivity)
- Re-plan mode: if inputs say "Tuesday forecast flipped to rain," produce a delta schedule (what moves, what stays) not a full re-write

## Example Output (3-crew, 5-job week — Plano/Frisco/Allen TX, week of 2026-05-11)

**Inputs provided:**

Jobs:
- J1: 234 Elm St, Plano TX 75074 — 28 sq, 6:12, 2-story, insurance-deadline (7 days), Tier B, material confirmed
- J2: 456 Maple Ave, Frisco TX 75070 — 20 sq, 4:12, 1-story, 1-day job, routine, material confirmed
- J3: 789 Oak Dr, Frisco TX 75070 — 16 sq, 9:12, 2-story, **Tier A required** (steep), standard, material confirmed delivery Thursday
- J4: 321 Pine Blvd, Allen TX 75002 — 6 sq repair (3 pipe boots + step-flashing), Tier C, materials on hand
- J5: 655 Cedar Ct, Plano TX 75074 — 22 sq, 4:12, 1-story, standard, material confirmed Monday delivery, 2-day

Crews (from `config.yml → crews[]`):
- Crew A: Jones lead, Tier A, 4-man, Truck 12, 20-mi service radius from Plano shop
- Crew B: Ramos lead, Tier B, 3-man, Truck 7, 20-mi service radius
- Crew C: Tran lead, Tier C, 2-man, Van 3, 20-mi service radius

Weather (`config.yml → weather_rules.rain_pct_threshold = 40%, wind_threshold = 25 mph`):
- Mon 5/11: Clear, 8 mph → ✅ Build
- Tue 5/12: 65% rain ⚠️ → **Weather Risk** (above 40% threshold)
- Wed 5/13: 25% rain, 12 mph → ✅ Build
- Thu 5/14: Clear, 5 mph → ✅ Build
- Fri 5/15: Clear, 9 mph → ✅ Build
- Sat 5/16: Available (emergency overflow only)

Shift: 7:00 AM – 3:30 PM (8 hr productive, from `config.yml → shift.start_time / .end_time`)

---

### 1. Weekly Grid (Mon–Fri)

| Day | Weather | Crew A — Jones (Tier A, 4-man, Truck 12) | Crew B — Ramos (Tier B, 3-man, Truck 7) | Crew C — Tran (Tier C, 2-man, Van 3) |
|-----|---------|------------------------------------------|------------------------------------------|---------------------------------------|
| Mon 5/11 | ✅ Clear | **J1** 234 Elm, Plano — Tear-off + dry-in (28 sq, 6:12, insurance-deadline priority) | **J5** 655 Cedar, Plano — Tear-off + dry-in (22 sq, 4:12 — clusters with J1, same ZIP 75074) | **J4** 321 Pine, Allen — Pipe boot + step-flash repair (6 sq, 4–5 hr, complete in one day) |
| Tue 5/12 | ⚠️ 65% rain | WEATHER RISK — J1 dry-in complete; shingle install not permitted. **Pivot: attic ventilation NFA-compliance check at 234 Elm** (interior-friendly, no weather exposure) | WEATHER RISK — J5 dry-in complete; install not permitted. **Pivot: warranty inspection sweep, 5 homes Plano 75074 backlog** (exterior walk-over under overhangs, no install) | J4 complete (Mon). **Standby + crew training** — equipment inspection + safety tailgate at shop |
| Wed 5/13 | ✅ 25% rain | **J1** 234 Elm, Plano — Install + cleanup (28 sq; insurance deadline met — Day 3, delivered ✓) | **J5** 655 Cedar, Plano — Install + cleanup (22 sq — completes J5, Day 3) | **Warranty punch-list** — follow Ramos at 655 Cedar for gutter inspection + caulk touch-up (paired assist, builds Tran crew skill) |
| Thu 5/14 | ✅ Clear | **J3** 789 Oak, Frisco — Tear-off + dry-in (16 sq, 9:12 steep — Tier A ONLY, material confirmed arrived) | **J2** 456 Maple, Frisco — Tear-off + dry-in + install + cleanup (20 sq, 4:12, 1-day job — clusters with J3 in Frisco 75070) | **Warranty inspection sweep continued** — 3 homes Allen/Plano backlog |
| Fri 5/15 | ✅ Clear | **J3** 789 Oak, Frisco — Install + cleanup (16 sq, 9:12 steep — Jones completes ✓) | **Flex / setup next week** — Ramos available for measure appointment or next-week J start | **Small repair backlog** — any pending repair from this week's warranty sweep findings |

---

### 2. Per-Job Crew Packet

**J1 — 234 Elm St, Plano TX 75074**
- Customer: Marcus Bell | 469-555-0201 | preferred contact 7–8 AM
- Scope: 28 sq, 6:12, 2-story, GAF Timberline HDZ Pewter Gray architectural — full tear-off and replacement
- Material status: ✅ Confirmed delivered to site Friday 5/8 (2 pallets + IWS + drip edge + ridge cap)
- Access: Driveway left side only (truck fits); neighbor's fence on right — keep setback 3 ft
- Safety: 2-story, power line on northwest eave corner — keep 10-ft clearance; no skylights
- Permit / HOA: Permit #PLN-2026-04-8812 issued; HOA quiet hours 8 PM – 7 AM
- Priority: **Insurance deadline 5/18** — tear-off Mon, install Wed, punch/cleanup Wed PM
- PM escalation: Jones calls shop at noon Mon and noon Wed to confirm on track
- Crew A (Jones, 4-man), Truck 12 | Start: 7:00 AM Mon | Expected complete: Wed 3:30 PM

**J2 — 456 Maple Ave, Frisco TX 75070**
- Customer: Diana Chu | 972-555-0417 | text preferred
- Scope: 20 sq, 4:12, 1-story, OC Duration Driftwood — full tear-off and 1-day replacement
- Material status: ✅ Confirmed on-site
- Access: Standard residential, no constraints noted
- Safety: 1-story low-risk; verify gutter guard system before tear-off (reinstall on completion)
- Permit: Frisco #FRS-2026-05-1140 issued
- Crew B (Ramos, 3-man), Truck 7 | Start: 7:00 AM Thu | Expected complete: Thu 3:30 PM
- *Clusters with J3 in same ZIP (75070) — Ramos staging at J2 then J3 same area*

**J3 — 789 Oak Dr, Frisco TX 75070**
- Customer: Greg Novak | 972-555-0788 | call after 4 PM
- Scope: 16 sq, 9:12 very steep, 2-story, GAF Grand Sequoia Weatherwood — full tear-off and replacement
- Material status: ✅ Confirmed delivery Thursday 5/14 AM (supplier drop window 6:30–7:30 AM)
- Access: Steep pitch — confirm anchor-point count (minimum 4 roof anchors required per `config.safety.jha_path`); second-floor window access to attic if needed
- Safety: **Tier A required** — 9:12 slope, 2-story; full harness + standoff ladder protocol; skylight (1) on south slope — cover before tear-off
- Permit: Frisco #FRS-2026-05-1141 issued
- Crew A (Jones, 4-man), Truck 12 | Start: 7:00 AM Thu (material window allows) | Expected complete: Fri 3:30 PM

**J4 — 321 Pine Blvd, Allen TX 75002**
- Customer: Sara Odom | 214-555-0332 | home all day
- Scope: 3 pipe boot replacements (1½" + 2" + 3") + 6 lf step-flashing re-seal at chimney — repair only
- Material status: ✅ Materials on Van 3 from shop stock
- Access: Gate code #4491; dog in backyard — close gate on arrival
- Safety: 1-story, 5:12 — standard PPE
- Permit: Not required (repair under $500 and no structural)
- Crew C (Tran, 2-man), Van 3 | Start: 7:00 AM Mon | Expected complete: Mon ~12:30 PM (4–5 hr)

**J5 — 655 Cedar Ct, Plano TX 75074**
- Customer: Lisa Fortuna | 469-555-0561 | text OK
- Scope: 22 sq, 4:12, 1-story, GAF Timberline HDZ Barkwood — full tear-off and replacement
- Material status: ✅ Confirmed delivery Monday 5/11 AM (7:00 AM window, supplier Beacon)
- Access: Cul-de-sac — Truck 7 can stage at curb; no HOA restrictions
- Safety: 1-story, 4:12 — standard PPE; no skylights
- Permit: Plano #PLN-2026-05-0043 issued
- Crew B (Ramos, 3-man), Truck 7 | Start: 7:00 AM Mon | Expected complete: Wed 3:30 PM

---

### 3. Weather Contingency Block

| Day | If worse than forecast | Pivot |
|-----|------------------------|-------|
| Mon 5/11 | If rain arrives before 10 AM (currently 8% chance) | Delay all crews 2 hr; reassess at 9 AM. If no clear window, push J1+J5 tear-off to Tue/Wed — flag J1 insurance-deadline risk to PM immediately |
| Tue 5/12 | Already Weather Risk day — plan already incorporates pivot | If rain clears by noon: Crew A and B may complete J1/J5 dry-in verification + minor install if ≥ 4 hr of clear window. Call-it-at-10-AM decision. |
| Wed 5/13 | If rain % increases to > 40% | Jones pivots to J1 dry-in finish + interior ventilation work only; Ramos pivots to inside garage measurement + material staging. Push J1 install to Thu; Jones shifts J3 to Fri+Sat. Flag J1 deadline to PM. |
| Thu 5/14 | If rain arrives (currently 5% chance) | Delta-replan: shift J3 tear-off to Fri, J3 install to Sat; Ramos shifts J2 to Fri (still 1-day). Tran continues warranty sweep. No deadline impact. |
| Fri 5/15 | If rain arrives (currently 5% chance) | J3 install and J2 execute only if clear before noon. If not: J3 installs Saturday (Jones available), J2 pushed to Mon 5/18. |

---

### 4. Utilization Report

**Available hours formula:** headcount × 8 hr shift × 5 days

| Crew | Available Hrs | Productive Scheduled Hrs | Utilization % | Flag |
|------|--------------|--------------------------|---------------|------|
| A — Jones (4-man) | 160 | Mon 32 + Tue 16 (interior pivot) + Wed 32 + Thu 32 + Fri 32 = **144** | **90%** | ✅ Optimal (70–95%) |
| B — Ramos (3-man) | 120 | Mon 24 + Tue 12 (warranty sweep) + Wed 24 + Thu 24 + Fri 0 (flex) = **84** | **70%** | ⚠️ At floor — add fill work Fri (measure appointment, warranty inspections, or next-week J6/J7 material staging) |
| C — Tran (2-man) | 80 | Mon 10 (J4 half-day) + Tue 0 (standby) + Wed 6 (paired assist) + Thu 8 (warranty sweep) + Fri 8 (repair backlog) = **32** | **40%** | 🔴 Under-utilized — Tran crew is a repair/training tier; 40% reflects no second repair job in pipeline this week. Recommend adding 2–3 repair calls from backlog (pipe boot + flashing) to reach 60–65%. Not overloading a Tier C crew in a skill-building week is acceptable if pipeline is dry. |

*Revenue note: `config.rates` not provided in this run — revenue totals not computed. Re-run with rates loaded for a full revenue-per-crew-week view.*

---

### 5. Risk Register

| Risk | Jobs Affected | Severity | Mitigation |
|------|--------------|----------|------------|
| J1 insurance deadline 5/18 | J1 | High | Tear-off Mon, install Wed, punch Wed PM — 6-day buffer. Contingency: if Wed is rained out, push to Thu (still 1 day buffer). Notify PM Monday at noon if any slip. |
| J3 material delivery window uncertainty (6:30–7:30 AM Thu) | J3 | Medium | Jones stages at J2 area Thursday morning; if delivery is late, pivots to assist Ramos on J2 or returns to J1 punch walk-over until material confirmed on-site |
| Crew C under-utilization (40%) | All | Low | No production deadline risk; recommend adding repair calls from CRM backlog before Thursday |
| No scheduling buffer for Ramos on Friday | J2, J5 | Low | Friday is flex — if any Wed/Thu slippage, Ramos can absorb on Friday without overtime |
| J3 steep pitch (9:12) — 2-story | J3 | Medium | Tier A (Jones) assigned; verify 4 anchor points and skylight cover before first step on Thu |

---

### Delta-Replan Scenario: Thursday forecast flips to 65% rain (Wednesday evening call)

**Trigger:** As of Wednesday 8 PM, Thursday's forecast updates from Clear → 65% rain.

**Impact analysis:**
- J3 tear-off (Jones, Thu) cannot proceed → shift
- J2 1-day job (Ramos, Thu) cannot proceed → shift

**Delta schedule (changes only — everything else holds):**

| Change | Original | Revised |
|--------|----------|---------|
| J3 Tear-off + dry-in | Thu (Jones) | **Fri 5/15** (Jones) |
| J3 Install + cleanup | Fri (Jones) | **Sat 5/16** (Jones — overtime auth needed; call owner) |
| J2 Tear-off + install | Thu (Ramos) | **Fri 5/15** (Ramos — 1-day job, fits on Fri alongside J3) |
| Thu Jones | Weather Risk | Interior: J1 + J5 final punch walk-overs + photo-documentation for warranty registration |
| Thu Ramos | Weather Risk | Warranty inspection sweep (3 homes 75070 backlog) |
| J1 deadline impact | None (J1 complete Wed) | ✅ No impact |

**Owner call script (Wed evening):** "Hey — Thursday is now showing 65% rain so we're shifting Greg Novak's Oak Dr job to Friday tear-off and Saturday install, and moving Diana Chu's Maple Ave job to Friday as well. Friday and Saturday are both clear. No deadline impact on any job. Wanted to give you the heads-up tonight so you can confirm the Saturday overtime for Jones."

**Assumptions footer for this run**
- `config.yml → crews[]` — Jones/Ramos/Tran loaded with tiers, headcounts, and vehicle assignments as described
- `config.yml → shift.start_time = 7:00` / `.end_time = 15:30` (8 hr productive)
- `config.yml → weather_rules.rain_pct_threshold = 40%`, `.wind_threshold = 25 mph`
- `config.yml → standard_crew_days` — Tier A 4-man: 28 sq tear-off in 1 day, 28 sq install in 1 day; Tier B 3-man: 22 sq in 2 days; 1-day jobs for 20 sq at 4:12 with Tier B confirmed feasible
- Revenue totals not computed — `config.rates` not loaded in this run
- Saturday overtime authorization assumed available — confirm with owner before finalizing delta replan
