---
name: "Safety Toolbox Talk Generator"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/talk"
version: 1.3
last_eval_score: 8.8
inspiration: "v1.3 (2026-06-01) surfaces the three 2026-06-01 KB refresh items on osha-heat-enforcement.md inside the skill itself — Appendix J citation framework, co-inspection trigger (any active-case visit becomes a de facto heat inspection during heat season), and NWS heat-advisory random-inspection authority on listed industries including roofing. The heat-day Example Output assumptions footer now carries the Appendix J reference, the co-inspection-posture flag, and a 'random-inspection-eligible today' line gated on an active NWS advisory; a new Instructions-side bullet teaches foremen to treat every site visit as a heat-program visit during heat season. v1.2 rewritten 2026-04-28 from eval improvement cycle — named config-field binding (safety.muster_points[] / .nearest_hospitals[] / .jha_path / .standdown_in_progress / .heat_index_data_source / .stop_work_authority_statement, crew_leads[].name / .language_preference, company.osha_jurisdiction), populated Example Output (105°F heat-illness talk on a mixed-language crew with one new hire on Day-3 acclimatization, plus the Construction Safety Week May 4–8 three-pillar fall-protection variant), and date-window auto-default for the Stand-Down. v1.1 Heat NEP / CPL 03-00-024 logic preserved."
---

# 🦺 Safety Toolbox Talk Generator

## Purpose

Draft a short (5–10 minute) OSHA-aligned toolbox talk for a roofing crew that targets the specific hazards of today's job, ties to current weather and site conditions, and produces a sign-off sheet that doubles as compliance evidence. Designed so any foreman can print, deliver, and log a talk without needing a safety coordinator on call.

## When to Use

- Start of every workday or shift change on active roofing projects
- Before high-risk tasks: steep-slope work, tear-off, hot work, confined-space attic work, crane or lift operations
- After a near-miss or incident, as part of a corrective action
- During heat advisories, cold snaps, or high-wind days when roofing hazards escalate
- During new-hire onboarding, to quickly deliver the baseline fall-protection talk
- As part of monthly safety audits to show an OSHA compliance trail

## Required Input

Provide the following:

1. **Crew & site details** — Crew lead name, number of workers on site, project address, material type being installed/torn off, roof pitch, height, and any unique site hazards (power lines, skylights, fragile substrate)
2. **Today's topic** — Either a specific topic (e.g., "ladder setup on uneven ground") or a general category (fall protection, heat illness, electrical, PPE, material handling, ladder safety, housekeeping, hazard communication). If blank, pick the highest-relevance topic based on weather + site hazards
3. **Weather & conditions** — Temperature range, wind speed/gusts, precipitation, humidity, UV index if known
4. **Recent events** — Any near-misses, incidents, OSHA updates, or equipment changes within the last 30 days that warrant attention
5. **Language preference** — English, Spanish, or bilingual (for crews with mixed language backgrounds)

## Instructions

You are a foreman's AI assistant. Your job is to produce a ready-to-deliver toolbox talk that a crew lead can print, read aloud, and collect signatures on in under 10 minutes total.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.license_number`, `company.phone`, `company.osha_jurisdiction` (federal-OSHA / state-plan-CA / state-plan-WA / state-plan-MI / etc., for the right citation flavor)
  - `crew_leads[]` — each with `name`, `phone`, `language_preference` (`en` / `es` / `bilingual`). Drives the foreman name on the header and the language-preference auto-pick if the input field is blank
  - `safety.muster_points[]` — preferred staging areas by job-site type (`residential_default`, `commercial_default`, named projects). Used in the emergency-reminder footer
  - `safety.nearest_hospitals[]` — name + address + phone, optionally indexed by ZIP for the larger service areas. The first matching hospital fills the footer; otherwise the `residential_default` entry
  - `safety.jha_path` — file path to the company's job hazard analysis library; cited in the header as "Companion JHA: {path}" so the signed sheet can be cross-referenced in an OSHA inspection
  - `safety.standdown_in_progress` (boolean, optional) — when `true` the topic defaults to fall protection in the Construction Safety Week three-pillar framing regardless of weather; auto-set when the request_date falls inside the May 4–8, 2026 window if the foreman has not opted out
  - `safety.heat_index_data_source` (default `NWS-NDFD`) — cited on heat-day talks so the heat-index reading on the sign-off sheet is provenance-tagged
  - `safety.stop_work_authority_statement` — one-sentence company commitment that any crew member can stop work without consequence; pulled into the commitment line of every fall-protection talk and any high-energy-hazard talk during the Stand-Down window
  - `safety.acclimatization_log_path` (optional) — file path used to cross-reference Day-N acclimatization status for any new-hire talk
- Reference `knowledge-base/regulations/` for current OSHA citations (29 CFR 1926 Subpart M fall protection, Subpart X ladders/stairways, Subpart L scaffolds, Subpart E PPE) and `knowledge-base/regulations/osha-heat-enforcement.md` for the 2026 Heat NEP (Directive CPL 03-00-024, effective April 10, 2026) and acclimatization expectations
- **2026-06-01 Heat NEP operational specifics (apply on every heat-day talk during the heat season):**
  - **Appendix J citation framework** — the 2026 directive standardizes how field observations translate into citation items across regions; the heat-day talk should be written so each control bullet maps to a named citation item (acclimatization gap / hydration cadence / shade availability / supervisor monitoring / written plan / recordkeeping). The signed sheet becomes the defense artifact against each Appendix J item
  - **Co-inspection trigger** — a compliance officer arriving for ANY active case (fall-protection, trenching, ladder, electrical) during heat season is expected to evaluate heat-program gaps in the same site walk and code them to the same case file. Practical posture: keep the heat program continuously defensible every day above the elevated threshold, not just on days a heat-specific inspection is most likely. Foremen running a fall-protection talk during the heat season should still print the day's heat sign-off sheet alongside it
  - **Random-inspection authority on NWS heat-advisory days** — on a day when the NWS has an active heat advisory or heat warning out for the operating area, compliance officers carry standing authority to drop in unannounced on roofing jobsites without a prior complaint trigger. The talk should record the heat-advisory status on the sign-off sheet ("NWS heat-advisory active: ☐ yes / ☐ no") so the artifact reflects the inspection-risk context the talk was delivered under
  - **Acclimatization gap is the primary citation item** when an incident occurs in a worker's opening week — the talk's acclimatization section is the first item an inspector reads for a Day-1 to Day-14 worker on the crew. Capture the acclimatization Day-N for every such worker on the sign-off sheet
- Reference `knowledge-base/terminology/` for consistent hazard language across talks
- If a named field is missing, use a sensible default and flag it in the Assumptions footer
- Pick the topic intelligently if not specified — prioritize weather-driven hazards (heat ≥ 85°F or heat index ≥ 90°F or wind > 20 mph escalates the day), then `safety.standdown_in_progress` (defaults to fall protection in the three-pillar framing), then site-specific hazards, then rotational baseline topics (falls weekly minimum). During the heat-index elevated window, the topic defaults to heat illness unless the foreman explicitly overrides — the Heat NEP makes a signed heat-day toolbox talk the primary compliance artifact

**Construction Safety Week (May 4–8, 2026) three-pillar default:**

When `safety.standdown_in_progress = true` (or the request_date falls inside the window and the foreman has not overridden), the fall-protection talk auto-adopts the OSHA Stand-Down 'Recognize, Respond, Respect' framing:

- **Recognize** — name the high-energy fall hazard on this specific roof today (eave with no guardrail above 6 ft, skylight without screen/cover, hip-line transition, valley loading point)
- **Respond** — name the prescribed control for that hazard (anchor-and-tieoff to a rated point with the harness inspected this morning, guardrail / personal fall-arrest / safety-net by Subpart M hierarchy)
- **Respect** — read `safety.stop_work_authority_statement` aloud verbatim; record on the sign-off sheet that every crew member acknowledged stop-work authority

The Stand-Down expects an on-site demonstration component (a foreman-led walk-through of one anchor point and one harness inspection). The talk includes a free-form line on the sign-off sheet: "On-site demonstration performed: ☐ anchor point inspected — by ___  ☐ harness donned + inspected — by ___."

**Talk structure (keep it tight — 5 minutes spoken):**

### 1. Header Block
- Date, project address, crew lead name, topic, weather conditions
- Applicable OSHA citation(s)
- Expected duration (5 / 10 minutes)

### 2. Opening Hook (2 sentences)
- A real-world roofing-specific scenario or near-miss that illustrates why today's topic matters
- Avoid generic construction examples; use roofing context (shingle tear-off, valley flashing, eave work, etc.)

### 3. Key Hazards Today (3–5 bullets)
- Specific hazards on this job and this topic, not theoretical ones
- Each bullet names what could go wrong and the consequence in one sentence
- Tie at least one bullet to today's weather or site conditions

### 4. Required Controls & PPE (checklist format)
- Each control stated as a crew-level action ("anchor tie-off before stepping onto the slope — not after")
- Include minimum PPE for today's work and verification that each crew member has it
- Call out any equipment that must be inspected before use and by whom
- Reference the specific OSHA rule being satisfied

### 5. Discussion Questions (2–3)
- Open-ended questions the foreman asks the crew — not yes/no
- Designed to surface hazards the foreman may have missed
- Examples: "What's one thing about today's pitch or layout that worries you?" or "If you saw a tie-off coming loose, who do you tell and how fast?"

### 6. Commitment & Sign-Off
- One-sentence commitment crew members are agreeing to when they sign
- Sign-off sheet: Name / Signature / Time columns for every crew member on site
- Foreman signature with date/time and site address

### 7. Emergency Reminders (footer block)
- Nearest hospital — name, address, phone (resolved from `safety.nearest_hospitals[]` by ZIP; otherwise the `residential_default` entry)
- Emergency contact number — 911 + the foreman's direct line from `crew_leads[].phone` + the company safety lead from config
- Muster point on this site — resolved from `safety.muster_points[]` for the matching site type, with a free-form override line for the foreman to write the actual on-site location
- Companion JHA reference — `safety.jha_path` cited so the signed sheet can be paired during an OSHA inspection

**Special-case expansions:**
- **Fall protection talks** must specify anchor points identified for today, harness inspection outcome, and the written rescue plan reference (29 CFR 1926.502 requires a rescue plan, not just a harness)
- **Heat illness talks** (temp ≥ 85°F, heat index ≥ 90°F) must specify hydration schedule (8 oz every 15 min), shade break cadence (typical 15 off / 45 on at the elevated threshold; stricter at heat index ≥ 105°F), buddy-check routine, and an acclimatization note for any worker in their first 7–14 days on the crew or returning after 7+ days absent — acclimatization gaps are the single most-cited heat finding under CPL 03-00-024. For heat index ≥ 105°F, also specify the staged cool-down resource available on site (ice sleeves, cooling vest, AC vehicle) and capture the heat index reading on the sign-off sheet as a compliance artifact
- **Ladder talks** must specify the 4-to-1 setup rule, extension above roofline (minimum 3 feet), tie-off at the top, and inspection before each use
- **Electrical talks** must identify any power lines within 10 feet and the assigned spotter
- **New-hire talks** include a short walk-through of the site identifying each hazard from the crew's perspective, plus an explicit acclimatization commitment if the forecast is above the heat threshold — first-day workload capped at roughly 20% of normal, progressing 20% per day

**Bilingual handling:**
- If "bilingual" is selected, produce the talk in two columns — English left, Spanish right — so the foreman can deliver each point in both languages
- Preserve OSHA citation numbers in both columns (they're the same)

**Output requirements:**
- Single printable page (or two for bilingual) — no longer
- Plain language; avoid jargon except OSHA citations
- Signature sheet fits on the same page if possible
- Saved as `outputs/safety/toolbox-talks/{YYYY-MM-DD}-{topic-slug}.md` if the user confirms
- Pair with a monthly log file the foreman updates: `outputs/safety/toolbox-talk-log.md`

**Compliance reminders:**
- The AI-generated talk is a starting point — the foreman must adapt for site-specific conditions and is accountable for accuracy
- Retain signed sheets for minimum 3 years (longer in some states) as OSHA recordkeeping evidence
- If an incident occurs that a prior toolbox talk covered, the signed sheet becomes a key defense artifact
- Cross-reference sibling skills: `crew-schedule-optimizer` (for linking hazards to daily route), `roof-inspection-report` (for site-hazard documentation)

## Example Output

### Heat-illness talk — 105°F day, mixed-language crew with one Day-3 new hire (2026-08-12)

> **Inbound:**
> "Crew lead Hector Vega, 4-man crew at 1248 Maple Ridge Dr Frisco TX 75070 — full tear-off + install on a 28-sq architectural roof, 6:12 pitch, 2 stories. Tomorrow's high 105°F, heat index 112°F by noon. New hire Jamal Wright on Day 3 of acclimatization. Crew is 2 English / 2 Spanish-preferred. Topic: foreman wants the heat talk."
>
> **Resolved fields:**
> - `crew_leads[]`: Hector Vega (es / bilingual). Spanish-preferred crew → bilingual two-column output
> - `safety.nearest_hospitals[]` for 75070 → Texas Health Frisco, 5400 Dallas Pkwy, 469-303-2000
> - `safety.muster_points[].residential_default` → "Driveway end at the street curb, opposite side from the dumpster"
> - `safety.heat_index_data_source` → NWS-NDFD
> - `safety.acclimatization_log_path` → confirms Jamal Wright Day 3 (workload cap 60% — 20% Day 1, +20% Day 2, +20% Day 3)
> - `safety.standdown_in_progress` → false (08-12 outside May 4–8 window)
> - **NWS heat-advisory status for 75070 on 2026-08-12** → ACTIVE (Heat Advisory in effect 11:00 AM – 8:00 PM CT, NWS Fort Worth). Random-inspection authority is in effect today under the 2026 Heat NEP — talk is printed with the advisory-active line on the sign-off sheet
>
> ---
>
> **TOOLBOX TALK — Heat Illness Prevention | Charla de Seguridad — Prevención de Enfermedad por Calor**
> Date: 2026-08-12 | Project: 1248 Maple Ridge Dr, Frisco TX 75070 | Foreman / Capataz: Hector Vega
> Topic: Heat Illness — Heat Index 112°F (NWS-NDFD) | Tema: Enfermedad por Calor — Índice 112°F
> Companion JHA: `safety/jha/heat-illness.md` | OSHA: 29 CFR 1926.95 + Heat NEP CPL 03-00-024 (eff. 2026-04-10) | Appendix J citation framework applies
> Duration: 7 min | Duración: 7 min
> NWS heat-advisory in effect for ZIP 75070 today: **☑ YES** / ☐ no — random-inspection-eligible under CPL 03-00-024
> Co-inspection posture: any OSHA visit today (for ANY reason) will be evaluated as a heat-program inspection in the same case file
>
> **OPENING / APERTURA**
>
> | English | Español |
> |---------|---------|
> | Three weeks ago a crew north of us in Plano had a 22-year-old roofer collapse on a 102°F day — kidneys shut down, hospitalized 4 days. He was on Day 4 of his first week. Today is hotter and one of us is at the same point in acclimatization. We do this talk every heat day until the index drops back under 90. | Hace tres semanas un techador de 22 años se desplomó en Plano a 102°F — riñones fallaron, 4 días en el hospital. Era su 4° día de su primera semana. Hoy hace más calor y uno de nosotros está en el mismo punto. Hacemos esta charla cada día de calor hasta que el índice baje de 90. |
>
> **HAZARDS TODAY / RIESGOS HOY**
>
> | English | Español |
> |---------|---------|
> | • Heat index 112°F by noon — at this level OSHA expects 15-minute shade breaks every 45 min, ice on site, and a buddy-check every break. | • Índice de calor 112°F al mediodía — a este nivel OSHA espera descansos de 15 min cada 45 min, hielo en sitio, y revisión entre compañeros. |
> | • Jamal is on Day 3 — workload cap is 60% of normal. He does setup, ground material handling, and ladder spotting only — no continuous tear-off cycles. | • Jamal está en Día 3 — máximo 60% de carga normal. Solo setup, manejo de material en tierra, y vigía de escalera — no ciclos continuos de arranque. |
> | • Black architectural shingles on a south-facing 6:12 pitch — surface temp will hit ~155°F. Knee pads + long sleeves required despite the heat. | • Tejas negras arquitectónicas en pendiente sur 6:12 — temperatura de superficie ~155°F. Rodilleras + manga larga requeridas a pesar del calor. |
> | • Tear-off + install means dehydration is silent — by the time you're thirsty you're already 2% deficit. Hydrate on schedule, not on thirst. | • Arrancar + instalar = deshidratación silenciosa — cuando tienes sed ya estás 2% deficit. Hidratar por horario, no por sed. |
>
> **CONTROLS & PPE / CONTROLES Y EPP**
>
> | English | Español |
> |---------|---------|
> | ☐ 8 oz water every 15 min — Hector calls the timer. (29 CFR 1926.95) | ☐ 8 oz de agua cada 15 min — Hector marca el tiempo. |
> | ☐ Shade break in the truck or under the canopy 15 min after every 45 min on the roof. | ☐ Descanso en la sombra (camioneta o toldo) 15 min después de cada 45 min en el techo. |
> | ☐ Buddy check every break — name the symptom you're watching for: dizziness, no sweat, confusion, cramping. | ☐ Revisión entre compañeros — nombra el síntoma: mareo, no sudar, confusión, calambres. |
> | ☐ Cool-down resources on site: ice sleeves, AC vehicle, cooler with ice water. (CPL 03-00-024 staged at index ≥ 105°F) | ☐ Recursos de enfriamiento: mangas de hielo, vehículo con AC, hielera con agua fría. |
> | ☐ Jamal — 60% workload, no roof shifts, ground-only tasks, doubled water cadence (8 oz every 10 min). | ☐ Jamal — 60% de carga, no turnos en techo, solo tareas en tierra, hidratación doble (8 oz cada 10 min). |
> | ☐ Stop work and call 911 if anyone shows confusion, no-sweat skin, or vomiting — heat stroke. Hector is the spotter. | ☐ Parar y llamar 911 ante confusión, piel sin sudor, o vómito — golpe de calor. Hector es el supervisor. |
>
> **DISCUSSION QUESTIONS / PREGUNTAS DE DISCUSIÓN**
>
> 1. What's one early symptom of heat illness you've felt on a roof before that you ignored? / ¿Qué síntoma temprano has sentido en un techo y has ignorado?
> 2. If Jamal looks off at 11 AM, what do we do in the first 60 seconds? / Si Jamal se ve mal a las 11 AM, ¿qué hacemos en los primeros 60 segundos?
> 3. Where's the cooler? Where's the AC truck? Who's the closest backup if Hector goes down? / ¿Dónde está la hielera? ¿La camioneta con AC? ¿Quién es el respaldo más cercano si Hector cae?
>
> **COMMITMENT / COMPROMISO**
>
> By signing, I commit to taking my breaks, drinking on the 15-min cadence, watching my crewmates, and stopping work if I or anyone else shows heat-illness symptoms — no penalty, no questions.
> Al firmar, me comprometo a tomar mis descansos, hidratarme cada 15 min, vigilar a mis compañeros, y parar el trabajo ante síntomas de calor — sin penalización, sin preguntas.
>
> Heat index reading at sign-off: **___°F** (NWS-NDFD)
> NWS heat-advisory active at sign-off: ☐ yes / ☐ no  |  Acclimatization Day-N captured for all Day-1–14 workers: ☐ yes  |  Appendix J defense items addressed today: ☐ acclimatization gap (program ref / Day-N captured)  ☐ hydration cadence (8 oz / 15 min documented)  ☐ shade available (vehicle / canopy)  ☐ supervisor monitoring (buddy-check cadence)  ☐ written plan referenced (Companion JHA)  ☐ recordkeeping (this sheet retained 3+ yrs)
>
> | Name / Nombre | Signature / Firma | Time / Hora |
> |---|---|---|
> | Hector Vega (foreman) | _______________ | _______ |
> | Jamal Wright (Day 3) | _______________ | _______ |
> | Carlos Reyes | _______________ | _______ |
> | Diego Morales | _______________ | _______ |
> | Brandon Lee | _______________ | _______ |
>
> Foreman signature & date: Hector Vega ________________ 2026-08-12 ____ AM
>
> **EMERGENCY / EMERGENCIA**
>
> - Nearest hospital: Texas Health Frisco — 5400 Dallas Pkwy, Frisco TX — 469-303-2000 (8 min from site)
> - Call: 911 → then Hector (469-555-0163) → then Acme safety lead Janelle Park (469-555-0140)
> - Muster point: Driveway end at street curb, opposite side from the dumpster — override: ____________
> - Companion JHA: `safety/jha/heat-illness.md`

### Fall-protection talk — Construction Safety Week three-pillar framing (2026-05-05)

> **Inbound:**
> "Crew lead Andrea Cole, 3-man crew at 4221 Cedar Hollow Ln, Plano TX 75074 — re-roof on a 35-sq architectural job, 9:12 pitch with two skylights and an open eave on the front porch. Forecast clear, 78°F, wind 9 mph. Topic: foreman left it blank."
>
> **Resolved fields:**
> - `safety.standdown_in_progress` → true (05-05 inside May 4–8 Stand-Down window). Topic auto-defaults to fall protection in the Recognize-Respond-Respect three-pillar framing
> - `crew_leads[]`: Andrea Cole (en). English-only crew → single-language output
> - `safety.nearest_hospitals[]` for 75074 → Medical City Plano, 3901 W 15th St, 972-596-6800
> - `safety.muster_points[].residential_default` → "Driveway end at street curb, opposite side from the dumpster"
> - `safety.stop_work_authority_statement` → "Any crew member at Acme Roofing has the authority to stop any task at any time without consequence if they observe an unsafe condition. Acme management commits to a no-retaliation review of every stop-work call."
>
> ---
>
> **TOOLBOX TALK — Fall Protection (Construction Safety Week 'Recognize · Respond · Respect')**
> Date: 2026-05-05 | Project: 4221 Cedar Hollow Ln, Plano TX 75074 | Foreman: Andrea Cole
> Topic: Fall Protection on a 9:12 pitch with two skylights + open-porch eave
> Companion JHA: `safety/jha/fall-protection.md` | OSHA: 29 CFR 1926 Subpart M (1926.501 / .502 / .503), 1926.502(d)(20) rescue plan
> Duration: 8 min — Stand-Down on-site demonstration included
>
> **OPENING — Why we're doing this today**
>
> Today is Day 2 of Construction Safety Week. Falls have been the #1 cause of construction fatalities in the U.S. for 13 straight years. Last year a roofer in Garland — same pitch as ours, same skylight configuration — stepped on an unscreened skylight and fell 22 feet. He survived but still can't return to roofing. We are working a 9:12 with two skylights today. This talk uses the OSHA Stand-Down framing: Recognize the hazard, Respond with the right control, Respect every crew member's right to stop work.
>
> **RECOGNIZE — High-energy fall hazards on THIS roof TODAY (29 CFR 1926.501)**
>
> 1. **Open-porch eave at the front entry** — 11 ft to grade, no guardrail, will be the first place anyone steps when carrying material from the truck. High-energy hazard #1.
> 2. **Two unscreened skylights** on the south slope — both within 6 ft of the work line during install. Walking-working surface 8.5 ft above the bedroom floor. High-energy hazard #2.
> 3. **9:12 pitch on the south face** — above 4:12 triggers Subpart M. Sliding distance to the eave is under 6 ft from the ridge.
>
> **RESPOND — Prescribed controls per the Subpart M hierarchy (29 CFR 1926.502)**
>
> - **Open-porch eave** → temporary guardrail in place by 7:30 AM before any material movement. Chris owns the install. Brandon spots.
> - **Skylights** → screen with the labeled OSHA-rated covers staged in Truck 7 OR install personal fall-arrest with anchors set north of each opening. Andrea inspects covers before crew steps onto the south slope.
> - **9:12 south face** → personal fall-arrest required from first ladder step. Anchor points: ridge anchor at the gable + secondary anchor on the north hip. Each harness inspected this morning by the wearer + cross-checked by Andrea.
> - **Written rescue plan** posted in Truck 7 — call 911 → Andrea executes suspension trauma protocol (relieve pressure within 15 min) → Medical City Plano transport.
>
> **STAND-DOWN ON-SITE DEMONSTRATION**
>
> ☐ Anchor point inspected — by ____________ at ____ AM
> ☐ Harness donned + inspected — by ____________ at ____ AM
> ☐ Skylight covers verified rated + intact — by ____________ at ____ AM
>
> **RESPECT — Every crew member's stop-work authority**
>
> "Any crew member at Acme Roofing has the authority to stop any task at any time without consequence if they observe an unsafe condition. Acme management commits to a no-retaliation review of every stop-work call."
>
> Read aloud by Andrea. Each crew member acknowledges by initialing below.
>
> **DISCUSSION QUESTIONS**
>
> 1. If you saw the rope on someone's harness fraying right before they walked to the ridge, what's the first thing you say — and to whom?
> 2. The skylight cover gets nudged and shifts an inch while you're 4 ft away. What's your next step?
> 3. We get one cell-tower bar at the ridge. Where's the second comm device on this site?
>
> **COMMITMENT**
>
> By signing, I commit to: tying off before stepping on any pitch over 4:12, using only the rated anchor points named above, inspecting my harness before donning it, and stopping work — for myself or anyone else — if I see an unsafe condition.
>
> | Name | Signature | Time | Stop-work acknowledged (initial) |
> |---|---|---|---|
> | Andrea Cole (foreman) | ___________ | ____ | ____ |
> | Chris Marquez | ___________ | ____ | ____ |
> | Brandon Lee | ___________ | ____ | ____ |
>
> Foreman signature & date: Andrea Cole ________________ 2026-05-05 _____ AM
>
> **EMERGENCY**
>
> - Nearest hospital: Medical City Plano — 3901 W 15th St, Plano TX — 972-596-6800 (12 min from site)
> - Call sequence: 911 → Andrea (469-555-0157) → safety lead Janelle Park (469-555-0140)
> - Muster point: Driveway end at street curb, opposite side from the dumpster — override: ____________
> - Companion JHA: `safety/jha/fall-protection.md`

### Assumptions footer for these runs

- `crew_leads[].language_preference` resolved bilingual for the heat-illness talk based on Hector being `es / bilingual` plus 2-of-4 Spanish-preferred crew; English-only resolution for the fall-protection talk based on Andrea + crew all `en`
- `safety.nearest_hospitals[]` resolved by ZIP (75070 → Texas Health Frisco; 75074 → Medical City Plano)
- `safety.standdown_in_progress` auto-set to `true` for 2026-05-05 (inside May 4–8 window); foreman did not opt out so the topic auto-defaulted to the three-pillar fall-protection framing
- `safety.heat_index_data_source` defaulted to `NWS-NDFD` since not set
- Acclimatization Day-3 status pulled from `safety.acclimatization_log_path`; 60% workload cap applied per CPL 03-00-024 acclimatization curve and surfaced as the primary Appendix J defense item for Jamal Wright on the sign-off sheet
- **2026 Heat NEP defensive-posture flags surfaced inline on the heat-illness talk:**
  - **Appendix J citation framework** cited in the header so each control bullet maps to a named Appendix J item (acclimatization gap / hydration cadence / shade availability / supervisor monitoring / written plan / recordkeeping); the sign-off sheet's Appendix J checklist captures all six items as defense artifacts in one place
  - **Co-inspection posture** disclosed in the header — Hector's crew is reminded that any OSHA visit today (even an unrelated fall-protection or trenching visit at a sibling jobsite, if the same compliance officer rotates) will be evaluated as a heat-program inspection in the same case file
  - **NWS heat-advisory random-inspection authority** — the talk records the NWS Fort Worth Heat Advisory active for 75070 on 2026-08-12 (11 AM – 8 PM CT) and surfaces "random-inspection-eligible under CPL 03-00-024" on the sign-off sheet so the artifact reflects the inspection-risk context the talk was delivered under. The fall-protection talk on 2026-05-05 does NOT surface this flag (no advisory in effect that day, outside heat season)
- New named-binding introduced: `safety.nws_advisory_source` (default `NWS-WFO`) — cited in the heat-advisory-active line so the advisory provenance is traceable on the sign-off sheet
