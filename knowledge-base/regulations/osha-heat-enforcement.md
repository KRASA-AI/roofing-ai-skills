# OSHA Heat Enforcement for Roofers (April–June 2026)

*Inspiration: April 10, 2026 OSHA announcement replacing the expired Heat National Emphasis Program with Directive CPL 03-00-024 — extracted as a compliance snapshot and contractor action framework. Refreshed 2026-06-01 with operational specifics on Appendix J citation framework, co-inspection trigger (heat coded simultaneously with fall-protection or trenching cases), and the random-inspection authority on NWS heat-advisory days for listed industries. No source text copied.*

## Why This File Exists

Roofing crews carry a disproportionate heat-exposure burden among construction trades — rooftop surface temperatures routinely exceed ambient by 30–60°F, shade is limited, and the work is continuous rather than intermittent. OSHA has signaled through 2026 enforcement posture that heat citations against roofing and other outdoor-trade employers will continue climbing, with fewer warnings and more penalties. This file consolidates the current enforcement landscape and the contractor-level controls needed to stay defensible.

## Current Enforcement Status (refreshed June 1, 2026)

- The prior Heat National Emphasis Program expired April 8, 2026 and was immediately replaced on April 10, 2026 by a new directive (reference: CPL 03-00-024, "National Emphasis Program — Outdoor and Indoor Heat-Related Hazards")
- The renewal continues the programmatic heat-inspection focus and formalizes inspection triggers (employee complaints, media-reported heat-illness events, forecasted heat above local thresholds, prior heat-related citations)
- Since the original NEP launched in 2022, heat-specific inspections have averaged several thousand per year industry-wide — an order-of-magnitude increase over pre-NEP baseline — indicating the enforcement lever is firmly in place even without a final federal heat standard
- No final federal heat-specific standard has been promulgated; citations are issued under the General Duty Clause (Section 5(a)(1) of the OSH Act), which requires employers to maintain a workplace "free from recognized hazards that are causing or are likely to cause death or serious physical harm"
- Several states (CA, WA, OR, CO, NV, MD, MN) operate state-plan heat standards that are more prescriptive than the federal directive — crews working across state lines should treat the stricter standard as the operating floor

### New operational specifics under the 2026 update (surfaced by mid-May reporting)

- **Appendix J — citation framework.** The 2026 directive ships with an appendix that standardizes how field observations are translated into specific citation items by the compliance officer. The practical effect for roofers: two crews with similar gaps now generate similar citation lists across regions, narrowing the cross-district variance contractors used to lean on for less-strict enforcement geographies
- **Co-inspection trigger — heat coded alongside the active case.** A compliance officer arriving for a fall-protection or trenching visit is now expected to evaluate heat-related gaps during the same site walk and to record those findings as additional citation items in the same case file. Practical implication for a roofing employer: any OSHA visit — for any reason — during the heat season effectively functions as a heat inspection too. The defensive posture is to keep the heat program continuously defensible, not just on the days a heat-specific case is most likely
- **Random-inspection authority during NWS heat advisories.** On a day when the National Weather Service has an active heat advisory or warning out for the operating area, compliance officers carry standing authority to drop in unannounced on jobsites in listed high-exposure industries — roofing being one — without a prior complaint trigger. A roofing crew on a deck during an active heat advisory should plan around the possibility of a no-notice site visit
- **Acclimatization gap is the primary citation trigger.** The 2026 framing elevates the absence of a documented gradual-introduction period for new or returning workers from a common citation item to the standard first item an inspector codes when an incident occurs inside a worker's opening week on the job. The evidentiary anchor for the General Duty citation is the CDC- and OSHA-referenced concentration of outdoor-heat fatalities (roughly half to seven-tenths of all such deaths) into the earliest days of exposure to warm or hot working conditions

## What OSHA Inspectors Look for on a Heat Visit

When an inspector opens a heat-related case against a roofing employer, the common lines of questioning focus on:

1. **Written heat illness prevention plan** — Does one exist, and does it name trigger temperatures, required controls, and responsibilities by role
2. **Acclimatization protocol** — How new workers and returning-after-absence workers are ramped up over the first 7–14 days
3. **Water, rest, shade availability** — Evidence of cool potable water within reach of the work area, a designated shaded break area at the site, and a mandatory break cadence
4. **Heat-illness recognition training** — Records of training in the crew's primary language, covering symptoms, emergency response, and the report-without-penalty expectation
5. **Supervisor monitoring practices** — Buddy systems, foreman spot-checks, and escalation protocol for a symptomatic worker
6. **Recordkeeping** — Toolbox talks delivered on heat topics, sign-off sheets, and incident logs for any heat-related near-misses

The absence of any one of these usually supports at least one citation item.

## Contractor Action Framework

### Baseline (applies to all roofing crews, all seasons)
- Maintain a written heat illness prevention plan referenced in the employee handbook and on the jobsite safety board
- Stock potable water coolers rated for the crew size with an enforced 8-ounce-every-15-minutes hydration cadence when temperatures rise above 80°F
- Identify a shaded break area on every job before work starts — vehicle cab, canopy, neighbor's tree line (with permission), or rented shade structure for multi-day projects
- Train each new hire on heat-illness symptoms in their primary language within the first shift, documented in the personnel file
- Make heat-illness reporting non-punitive in writing and communicate that policy every season

### Elevated (temperature ≥ 85°F or heat index ≥ 90°F)
- Run a heat-specific toolbox talk at shift start with sign-off sheet — use the `safety-toolbox-talk-generator` skill with temperature and UV index in the weather input to auto-select heat as the topic
- Rotate crew members off the roof on a defined cadence (commonly 15 minutes off every 45 minutes on; stricter in higher-risk conditions)
- Verify each crew member's personal water supply at shift start
- Foreman performs a visible check of every crew member every 30 minutes — name recognition, sweat pattern, coordination

### High (temperature ≥ 95°F or heat index ≥ 105°F)
- Consider early-start schedules to complete rooftop work before the afternoon heat curve
- Deploy buddy-pair monitoring — no solo work on the deck
- Stage a cool-down resource (ice sleeves, cooling vests, air-conditioned vehicle) at the site
- Document the day's heat index readings, break cadence, and any symptom reports in the daily site log — this becomes the defense artifact if an incident occurs
- Management-level decision required on whether to stop work entirely — local competitors stopping often signals that the hazard is recognized in the trade

### Acclimatization (first 7–14 days for any new worker or a worker returning after 7+ days absent)
- First day: no more than 20% of the normal workload in heat conditions
- Progressive increase of 20% per day until full workload reached
- Heightened supervisor monitoring throughout the acclimatization window
- This is the single most-commonly-cited gap in heat enforcement actions — a roofer who collapses in the first three days on the job nearly always generates an acclimatization-program citation

## Compliance Evidence Trail

For a defensible posture when an inspector opens a file, the paper trail should include:

- Signed heat-specific toolbox talks for every day above the elevated threshold
- A maintained heat illness prevention plan with a version history
- Personnel records showing acclimatization tracking for every new hire
- Site logs capturing daily heat index readings, water supply checks, and break cadence adherence
- Incident and near-miss reports (even non-recordable ones) with follow-up actions

The `safety-toolbox-talk-generator` skill produces the daily signed sheet that doubles as the primary compliance artifact. The toolbox-talk-log file in `outputs/safety/` aggregates those sheets for monthly review.

## Cross-References

- **Skill:** `operations/safety-toolbox-talk-generator` — Produces the daily talk and sign-off sheet that anchors the compliance trail for heat days
- **Skill:** `operations/crew-schedule-optimizer` — Weather-aware scheduling that can flag forecasted heat days and trigger start-time shifts
- **KB:** `tools-ecosystem/ai-tools-landscape.md` — Wearable heat-monitoring and AI-based thermal-stress sensors as an emerging layer (SmartQHSE, Kenzen, SafeGuard)
- **External:** OSHA Heat Illness Prevention page (osha.gov/heat-exposure) for citation updates and state-plan differences

## Notable Uncertainty

- A final federal heat standard has been in rulemaking since 2021; its arrival date remains indefinite as of April 2026. Contractors should plan for the standard to be prescriptive (specific temperature triggers, mandatory break schedules) rather than performance-based
- State-plan variation is widening. Roofers operating in California, Washington, Oregon, Colorado, Nevada, Maryland, and Minnesota need state-specific content in their prevention plans beyond the federal baseline
- Insurance underwriters are starting to ask for heat-program documentation on renewal, separate from OSHA. An existing compliance binder doubles as the underwriting artifact

---

*Last updated: 2026-06-01 by landscape monitor*
