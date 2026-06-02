---
name: "Lead Response Automator"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/lead"
version: 1.4
last_eval_score: 8.8
inspiration: "v1.4 (2026-06-01) adds the fifth Example Output covering the documented 'biggest capacity gain' use case from the When to Use section: a 🔴 in-shift call-miss recovery during business hours (separate from the v1.3 storm-night active leak). Scenario: 75070 inbound voice call at 2:43 PM Wed dropped to voicemail because the office was on three other calls and the AI receptionist's ring-time threshold expired without a human pickup. The skill demonstrates the 60-second AI-assisted callback pattern (transcript-from-VM auto-classify → callback-bot dials within 60 sec → live SMS with two real calendar slots → estimator handoff brief in JobNimbus). Exercises a new named binding (response.in_shift_missed_call_recovery_window_seconds, default 60) plus the existing response.business_hours and company.calendar_integration = google_workspace path. Closes the documented-but-unexemplified gap flagged as the 2026-05-25 recommended next-cycle improvement. v1.3 adds the fourth ⚪ Low-fit Example Output — a Tyler TX out-of-service-area lead with a polite-referral script, no-estimator-routed handoff, and a CRM note flagging the redirect for source-quality analytics. v1.2 SLA windows, named bindings, estimator-matching algorithm, and three existing tier examples preserved."
---

# 🚀 Lead Response Automator

## Purpose

Generate instant, channel-appropriate responses to new roofing leads — qualifying them by job type, urgency, and fit — and produce a structured intake sequence that captures property details, schedules inspections against the actual estimator calendar, and hands off a fully qualified lead to the right estimator within minutes rather than hours.

## When to Use

- When a new lead comes in from any source (web form, ad click, phone call, referral)
- During storm season when inbound volume overwhelms office staff
- To standardize the first-touch experience across all lead sources
- When building or auditing an automated lead intake workflow
- To create scripts for AI receptionist or chatbot configuration
- To recover in-shift missed calls — the majority of unanswered inbound calls at most roofing shops occur during business hours when the office is swamped and crews are on roofs, so the biggest capacity gains come from AI-assisted booking during the workday, not only after hours

## Required Input

Provide the following:

1. **Lead source** — Where the inquiry came from (Google ad, website form, referral, storm canvassing, social media, home advisor, etc.)
2. **Initial lead info** — Whatever the lead provided: name, phone, email, address, description of need, photos if available
3. **Inquiry type** — If known: storm damage, routine maintenance, new roof, commercial, emergency leak, gutter/siding, insurance claim
4. **Current capacity** — Estimator availability for the next 5 business days, preferred inspection time blocks (or `live` if `company.calendar_integration` is configured for direct booking)
5. **Response channels available** — Which channels are active: SMS, email, phone, chat widget
6. **Calendar access** — Whether the AI layer can read the estimator calendar in real time and book directly (driven by `company.calendar_integration`), or whether it must offer two to three time blocks and hand off to a human to confirm
7. **Fallback routing rules** — Resolved from `response.fallback_routing[]` if configured; otherwise ask once

## Instructions

You are an AI-powered intake specialist for a roofing company. Your job is to produce a complete lead response package that gets qualified leads booked for inspection within minutes of first contact.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.phone`, `company.sms_from_number`, `company.email_from`, `company.website` — sender identity on every channel
  - `company.estimators[]` — each with `name`, `phone`, `email`, `calendar_id`, `skill_tier` (A = commercial / steep / complex, B = standard residential, C = repairs / new-hire), `service_zips[]`, `is_on_call_after_hours`. Drives both routing and the named handoff in the brief
  - `company.calendar_integration` — one of: `google_workspace` / `microsoft_365` / `jobnimbus_calendar` / `acculynx_calendar` / `servicetitan` / `none`. Determines booking path: live in-interaction vs offer-and-confirm
  - `response.sla_overrides[]` — per-tier windows (e.g., `{tier: "emergency", first_touch_seconds: 60, inspection_window_hours: 4}`). If empty, use the defaults below
  - `response.fallback_routing[]` — ordered list per tier (e.g., `[primary_estimator, secondary_estimator, owner_line, after_hours_voicemail, outside_answering_service]`). 🔴 emergencies skip voicemail
  - `response.business_hours` — for the in-shift-vs-after-hours branching
  - `service_area.zip_codes[]` — drives the ⚪ Low-fit out-of-area routing
  - `reviews.google_profile_url`, `reviews.average_rating`, `reviews.count_last_90_days` — social-proof anchor in 🟢 standard tier email body
  - `warranty.workmanship_years`, `warranty.manufacturer_tiers[]` — credibility anchor in standard-tier email
  - `voice.intake` — communication tone for first-touch (typically warmer + more empathetic than `voice` for sales follow-ups). Falls back to `voice` if missing
  - `crm.system` (e.g., JobNimbus / AccuLynx / ServiceTitan) — determines paste-ready format for the estimator handoff brief
- Reference `knowledge-base/terminology/` for correct industry terms
- If a named field is missing, use the default and flag it in the output's Assumptions footer

**Default SLA windows (override with `response.sla_overrides[]` if present):**

| Tier | First touch | Inspection window | Routing override |
|------|-------------|-------------------|------------------|
| 🔴 Emergency | ≤ 60 seconds | Same-day, ideally ≤ 4 hours | Page on-call estimator; never voicemail |
| 🟡 Urgent | ≤ 30 minutes | ≤ 48 hours | Primary → secondary → owner line |
| 🟢 Standard | ≤ 1 hour | ≤ 5 business days | Primary estimator with normal cadence |
| ⚪ Low-fit | ≤ 1 hour | Polite redirect / referral | No estimator routed |

**Response generation process:**

1. **Classify the lead** by triage signals from the inbound message:
   - 🔴 Emergency — active leak with interior water, tarp needed, "ceiling is dripping right now," storm-night call
   - 🟡 Urgent — recent storm damage, insurance deadline ≤ 30 days, visible damage in photos, dropped shingles, ridge cap blown off
   - 🟢 Standard — re-roof planning, maintenance inquiry, "getting estimates," general quote, age-driven renewal
   - ⚪ Low-fit — outside `service_area.zip_codes[]`, non-roofing scope (siding-only, solar-only without roof), tire-kicker signals (no address, repeated form spam)

2. **Match an estimator** from `company.estimators[]`:
   - Filter by `service_zips[]` containing the lead's ZIP
   - Match `skill_tier` to job type (A for commercial / steep / complex, B for standard residential, C for repairs)
   - For 🔴 outside business hours, route to the first estimator with `is_on_call_after_hours: true`
   - If no match, walk `response.fallback_routing[]` for that tier in order

3. **Generate the initial response** by channel — substitute every named field rather than leaving placeholders

4. **Build the qualification sequence** (2–3 follow-up messages over the first hour):
   - Message 1 (T+0): Immediate acknowledgment + single qualifying question (property type, damage type, or timeline)
   - Message 2 (T+15 min if no response): Value hook + scheduling offer with a real slot from the matched estimator's calendar
   - Message 3 (T+45 min if still no response): Social proof from `reviews.*` + direct CTA

5. **Produce the estimator handoff brief** in the format expected by `crm.system`

6. **Close the booking loop** with calendar logic:
   - If `company.calendar_integration ≠ none`: pull two real slots from the matched estimator's `calendar_id`, book on confirmation, fire auto-confirmation with directions and reschedule link
   - If `company.calendar_integration = none`: offer two to three typical slot options, capture the preferred slot, hand off to a human within the SLA window — never let a lead end the interaction with only "we'll get back to you"
   - For 🔴 + 🟡, override normal business-hours routing — page the on-call estimator from `company.estimators[].is_on_call_after_hours`

**Qualification questions to weave into the sequence:**

- Is this for your primary residence or a rental/commercial property?
- What type of damage or concern are you seeing? (leak, missing shingles, storm damage, age-related wear)
- Do you have an insurance claim open or plan to file one?
- What is your preferred timeline for getting the work done?
- Have you gotten other estimates? (handle with care — use for positioning, not pressure)

**Output requirements:**

- Complete response package organized by channel (SMS, email, phone, chat) for the matched tier
- Qualification sequence with timing and branching logic
- Estimator handoff brief in `crm.system` paste format
- Each message personalized with lead details and named-field config values (no `{placeholder}` left unresolved)
- Saved to `outputs/lead-response/{YYYY-MM-DD}-{lead-slug}-{tier}.md` if the user confirms

**Efficiency notes:**

- Single clarifying question max — typically only when the inbound message is ambiguous about urgency
- If `response.fallback_routing[]` is empty and the matched estimator is unavailable, default to: secondary estimator → owner line → outside answering service for 🔴, and: secondary estimator → next-day callback for 🟡 / 🟢
- Cross-reference: hands warm leads off to `follow-up-sequence` (Day-1 cadence resumes once intake completes), routes 🔴 storm-driven leads back into `predictive-lead-scorer` for batch upgrade if multiple come from the same ZIP, escalates commercial inquiries to `commercial-prospect-researcher` for company-level enrichment

## Example Output

### 🔴 Emergency — active leak (web form, 9:47 PM Tuesday)

> **Inbound** (web form, 2026-04-27 21:47 CT):
> "Pat Nguyen — 4221 Cedar Hollow Ln, Plano TX 75074. Water dripping from kitchen ceiling, started about an hour ago. We had a tornado warning earlier."
>
> **Tier classification:** 🔴 Emergency (active interior water + storm context)
> **Matched estimator:** Marcus Patel (Tier A, ZIP 75074 ✓, on-call ✓)
> **SLA:** First touch ≤ 60 sec, inspection ≤ 4 hr, page on-call — skip voicemail
>
> ---
>
> **SMS (T+0, sent 21:48):**
> "Pat — Acme Roofing here. Got your message about the leak at 4221 Cedar Hollow. Sit tight, our on-call estimator Marcus is calling you in the next 60 sec from 469-555-0142. In the meantime, put a bucket under the drip and a tarp or bowl on the wet drywall if you can. — Sam"
>
> **Phone script (Marcus, T+1 min):**
> "Pat — Marcus with Acme Roofing, returning your form. Quick first question: is the water still actively dripping or has it slowed? OK, two things: I'm 18 minutes out and can put a temporary tarp on tonight if you want me to come now — no charge for the emergency tarp on a storm night. Second, while I'm driving, can you take three photos: the ceiling drip, the room below, and the roof from the ground if you have a flashlight. I'll text you my number so you can send them. Sound good?"
>
> **Auto-confirmation email (T+2 min):**
> Subject: "Marcus is on the way — Acme Roofing"
> Body: "Pat — confirming Marcus Patel will arrive at 4221 Cedar Hollow Ln by 22:10 CT for an emergency tarp + assessment. He's licensed (TX-RC-0481234), HAAG-certified, and 14 years on the roof. Reschedule link if needed: [acmeroofs.com/r/em-2026042700418]. — Acme Roofing, 469-555-0140."
>
> **Estimator handoff brief (JobNimbus paste, T+3 min):**
> ```
> LEAD: Pat Nguyen / 4221 Cedar Hollow Ln, Plano TX 75074 / 469-555-0188
> SOURCE: web_form / 2026-04-27 21:47 CT
> TIER: 🔴 Emergency — active interior leak + tornado warning earlier
> ASSIGNED: Marcus Patel — on-call, ETA 22:10 CT
> SCOPE: Emergency tarp + roof assessment; full inspection + estimate scheduled for tomorrow 9 AM
> NOTES: Water dripping from kitchen ceiling, started ~20:47 CT. Photos requested via SMS. Storm context = NOAA event 20260427-DFW-WIND-091.
> NEXT: Marcus calls in to dispatch on tarp completion; full inspection booked tomorrow at 9 AM (Marcus's calendar pre-blocked).
> ```

### 🟡 Urgent — post-storm 28-sq inquiry (Google ad, Wednesday 10:15 AM)

> **Inbound** (Google ad form, 2026-04-29 10:15 CT):
> "Janelle Doe — 1248 Maple Ridge Dr, Frisco TX 75070. Roof is 9 yrs old, big hailstorm here on 4/18, neighbor's filing a claim. Want an inspection. Photos attached: shingle granules in flowerbed, dent in gutter face."
>
> **Tier classification:** 🟡 Urgent (4/18 hail event in 75070 hail zone + visible damage signals + insurance-adjacent)
> **Matched estimator:** Marcus Patel (Tier A, ZIP 75070 ✓)
> **SLA:** First touch ≤ 30 min, inspection ≤ 48 hr, calendar live in `google_workspace`
>
> ---
>
> **SMS (T+0, sent 10:18):**
> "Janelle — Sam from Acme Roofing. Got your photos — those are classic hail signals. We've worked 9 jobs in 75070 since 4/18. Marcus, our HAAG-certified inspector, has 2:00 PM today or 9:30 AM tomorrow open at 1248 Maple Ridge. Which works? — Sam, Acme Roofing, 469-555-0140"
>
> **Email (T+5 min, value-add):**
> Subject: "Hail inspection — 1248 Maple Ridge"
> Body: "Hi Janelle,
>
> Thanks for the photos — granule loss in the flowerbed plus a dented gutter face is the pattern we look for after a 1.5-inch hail event like 4/18. Most carriers in 75070 are giving homeowners 60 days to file, so the inspection-then-decide window is open.
>
> Two slots are held on Marcus's calendar: 2:00 PM today or 9:30 AM tomorrow. The inspection is free and includes a HAAG-aligned strike count per slope and a written report you can hand to your adjuster.
>
> A few quick questions to bring the right gear:
> 1. Have you filed a claim yet, or are you deciding after the inspection?
> 2. Is the roof a single layer (most common) or has it been re-roofed over the original?
>
> See our 4.9★ Google rating from 142 verified reviews: acmeroofs.com/google
>
> — Sam, Acme Roofing | 469-555-0140 | sam@acmeroofs.com"
>
> **Phone script (Marcus, T+30 min if no SMS reply):**
> "Janelle — Marcus with Acme Roofing. Reaching out on the 4/18 hail inspection. Two questions: should I plan on a full strike-count walk for an insurance claim, or is this a 'just want to know if it's bad' check? And — driveway access OK for my ladder truck on Wednesday morning? Got 9:30 AM held for you."
>
> **Estimator handoff brief (JobNimbus paste, on slot confirmation):**
> ```
> LEAD: Janelle Doe / 1248 Maple Ridge Dr, Frisco TX 75070 / janelle.doe@email.com
> SOURCE: google_ad_hail_2026 / 2026-04-29 10:15 CT
> TIER: 🟡 Urgent — 4/18 hail (75070, NOAA 20260418-DFW-HAIL-117), 9-yr roof, photos show granule + gutter dents, insurance-adjacent
> ASSIGNED: Marcus Patel — 2026-04-29 14:00 OR 2026-04-30 09:30 (slot to be locked from SMS / phone reply)
> SCOPE: HAAG-aligned inspection + per-slope strike count + written report ready to hand adjuster
> NOTES: Single-vs-multi-layer unknown; ladder access TBD. Neighbor filing claim. Photos in attachment 1, 2.
> NEXT: Lock slot, fire auto-confirm with directions + Marcus credentials. Route to roof-inspection-report skill on inspection completion.
> ```

### 🟢 Standard — re-roof planning (referral, Saturday 11:30 AM)

> **Inbound** (text from referral, 2026-05-02 11:30 CT):
> "Hi, my neighbor Tom on Oak Ridge said you guys did his roof last year. Mine is 22 years old and still OK but probably time. Just want a couple estimates. No rush. — Daniel R, 1322 Oak Ridge Dr, Frisco 75033."
>
> **Tier classification:** 🟢 Standard (age-driven, referral, no urgency)
> **Matched estimator:** Andrea Cole (Tier B, ZIP 75033 ✓)
> **SLA:** First touch ≤ 1 hr, inspection ≤ 5 business days
>
> ---
>
> **SMS (T+0, sent 11:42):**
> "Daniel — Sam from Acme Roofing. Tom's a good guy, glad he sent you our way. Andrea, our Frisco estimator, can stop by Tue 5/5 between 4–5 PM or Thu 5/7 at 10 AM for a free roof check + written estimate. Either work? — Sam, 469-555-0140"
>
> **Email (T+10 min, social proof + low-pressure):**
> Subject: "Roof check on Oak Ridge — same crew that did Tom's"
> Body: "Daniel,
>
> Appreciate Tom passing your name along. We installed his GAF Timberline UHDZ in March 2025 — happy to walk you down the street to look up at it before the inspection if you want.
>
> A 22-year roof is right at the natural-replacement window for an asphalt system in North Texas. The inspection is free, takes about 45 minutes, and you'll get a written estimate the same day with three tier options (Good / Better / Best). No deposit, no pressure to decide on the spot.
>
> Andrea Cole is our Frisco estimator — 11 years on the roof, GAF MasterElite certified. She has Tue 5/5 4–5 PM and Thu 5/7 10 AM open. Just reply with your pick.
>
> A few things to know about Acme:
> - 6-year workmanship warranty (Better tier extends to 10 years; Best to 25)
> - 4.9★ on Google with 142 reviews in the last 90 days (acmeroofs.com/google)
> - GAF MasterElite + OC Platinum Preferred — only ~3% of roofers in TX hold both
>
> Talk soon,
> Sam | Acme Roofing | 469-555-0140 | sam@acmeroofs.com"
>
> **Phone script (Andrea, T+45 min if no reply):**
> "Hi Daniel — Andrea with Acme Roofing, following up on Tom's referral. No pressure on timing — just wanted to lock a slot for the free roof check. Tuesday 4–5 PM or Thursday 10 AM both work for me. Quick question: is the 22-yr roof the original install or has it been replaced once already? Helps me bring the right materials samples."
>
> **Estimator handoff brief (JobNimbus paste, on slot confirmation):**
> ```
> LEAD: Daniel R. / 1322 Oak Ridge Dr, Frisco TX 75033 / phone TBD
> SOURCE: referral (Tom @ 1318 Oak Ridge) / 2026-05-02 11:30 CT
> TIER: 🟢 Standard — 22-yr roof, age-driven, referral, no urgency
> ASSIGNED: Andrea Cole — 2026-05-05 16:00 OR 2026-05-07 10:00 (TBD on reply)
> SCOPE: Free roof check + free written estimate (Good/Better/Best from estimate-builder skill)
> NOTES: Referral from Tom (1318 Oak Ridge — installed GAF Timberline UHDZ 2025-03). Single-vs-multi-layer unknown. Bring GAF + OC samples.
> NEXT: Lock slot, route to estimate-builder skill on inspection completion. Already in follow-up-sequence Hot tier post-estimate.
> ```

### ⚪ Low-fit — out-of-service-area redirect (Facebook ad form, Thursday 2:15 PM)

> **Inbound** (Facebook lead-ad form, 2026-05-08 14:15 CT):
> "Wendy Ortega — 1840 Pine Bluff Rd, Tyler TX 75703. Looking for a quote on a 22-sq replacement, asphalt. Saw your ad. Single story, no urgency."
>
> **Tier classification:** ⚪ Low-fit (Tyler is in Smith County, ~95 mi SE of Frisco — outside `service_area.zip_codes[]` and outside `service_area.licensed_counties[] = [Collin, Denton, Dallas, Tarrant]`)
> **Matched estimator:** None routed (geo-exclusion fires before estimator match)
> **SLA:** First touch ≤ 1 hr, polite referral, no inspection scheduled, no follow-up cadence triggered
> **Source-quality note:** Tyler is outside service area but receiving the Facebook ad — flag for the marketing-audit channel
>
> ---
>
> **SMS (T+0, sent 14:32):**
> "Hi Wendy — Sam with Acme Roofing. Thanks for reaching out about the roof at 1840 Pine Bluff in Tyler. We actually only service the Dallas-Fort Worth metro (Collin / Denton / Dallas / Tarrant counties) so I can't send a crew that far, but I don't want to leave you stuck. Two reputable Tyler roofers I'd send my own mom to: Henson Roofing in Tyler (903-555-0188, also GAF MasterElite) and Cypress Roofing in Whitehouse (903-555-0142, HAAG certified). Both will give you a free estimate. Good luck with the project! — Sam, Acme Roofing"
>
> **Email (T+5 min, polite-referral with reference list):**
> Subject: "Roof in Tyler — outside our DFW service area, here are two we'd recommend"
> Body: "Hi Wendy,
>
> Thanks for the inquiry about the 22-sq replacement at 1840 Pine Bluff Rd. I want to be upfront: Acme Roofing only services the Dallas-Fort Worth metro — specifically Collin, Denton, Dallas, and Tarrant counties — so we wouldn't be the right fit for a Tyler property. (Driving a crew 95 miles each way burns cost we'd have to pass to you, and we wouldn't be local enough for warranty service.)
>
> Two roofers we know and trust in East Texas — both we'd send a family member to:
>
> 1. **Henson Roofing** (Tyler) — 903-555-0188, GAF MasterElite certified, ~25 yrs in business. Ask for Dale.
> 2. **Cypress Roofing** (Whitehouse) — 903-555-0142, HAAG-certified inspector, strong on insurance claims if your roof has any storm damage history.
>
> Both will give you a free estimate with no pressure. Mention you came from Acme in Frisco — neither will charge a referral fee back to you.
>
> If you ever move to the DFW metro or have a property up here, our door's open.
>
> — Sam | Acme Roofing | 469-555-0140 | sam@acmeroofs.com"
>
> **Phone script (no callback unless inbound caller asks for one — ⚪ tier does not auto-page):**
> If Wendy calls back asking for a recommendation: "Wendy, glad you got the message. The two Tyler roofers I'd point you toward are Henson Roofing (Dale, 903-555-0188) and Cypress Roofing (903-555-0142). Tell them Sam from Acme in Frisco sent you — they know us. If you want, I can also forward your inquiry directly to Dale at Henson — just say the word."
>
> **CRM handoff brief (JobNimbus paste, T+10 min — closed-lost-redirected):**
> ```
> LEAD: Wendy Ortega / 1840 Pine Bluff Rd, Tyler TX 75703 / phone TBD
> SOURCE: facebook_lead_ad_dfw_2026_q2 / 2026-05-08 14:15 CT
> TIER: ⚪ Low-fit — Tyler is outside service_area.zip_codes[] AND outside .licensed_counties[]
> ASSIGNED: NONE (no estimator routed — geo-exclusion fired)
> SCOPE: Polite referral to Henson Roofing (Tyler) + Cypress Roofing (Whitehouse). No inspection scheduled.
> STATUS: Closed-Lost-Redirected (source: out-of-area)
> SOURCE-QUALITY FLAG: ⚠️ Facebook ad set "dfw_2026_q2" is delivering to Tyler ZIP — audit campaign targeting. 4th out-of-area lead this month from same ad set; recommend tightening geo-radius in ad manager.
> FOLLOW-UP: None. ⚪ tier does NOT enter follow-up-sequence; does NOT enter predictive-lead-scorer batch.
> ```
>
> **Marketing-audit note (auto-routed to office@acmeroofs.com):**
> "⚪ Low-fit redirect #4 this month from `facebook_lead_ad_dfw_2026_q2`. Recommend reducing campaign geo-radius from 'Texas' to 'DFW metro 50 mi' in ad manager. Estimated wasted CPL on out-of-area leads this month: ~$320 (4 × $80 avg lead cost)."

### Assumptions footer for this run

- `voice.intake` defaulted to consultative-friendly (warmer than `voice`); confirm against config
- `response.sla_overrides[]` used defaults — no entries found in config
- `crm.system` resolved to `jobnimbus` from config; brief format matches JobNimbus paste expectations
- ZIP-to-estimator routing assumed from `company.estimators[].service_zips[]`; if a multi-estimator overlap exists, primary picked by `skill_tier` match first
- ⚪ Low-fit redirect partners (Henson Roofing / Cypress Roofing) pulled from `response.fallback_routing[].out_of_area_referrals[]` if configured; otherwise the rep is asked once for two trusted partners on first run and they are cached to config
- ⚪ Low-fit closed-lost-redirected status does NOT trigger `follow-up-sequence` and does NOT enter `predictive-lead-scorer` batch — confirmed at the CRM handoff step
- Source-quality flag fires when ≥3 out-of-area leads in 30 days route from the same ad source — count maintained in `response.source_quality_log_path`

### 🔴 In-shift call-miss recovery — 75070 inbound, voicemail dropped during business hours (Wed 2:43 PM)

> **Inbound** (voice call → voicemail, 2026-06-03 14:43 CT, ring timeout after 22 sec because the front desk was on three concurrent calls and no human picked up before the AI receptionist's `response.in_shift_missed_call_recovery_window_seconds` voicemail-handoff threshold):
> "Hi, uh, this is David Park at 2207 Heritage Oaks Ln in Frisco, 75070. We've got a leak in the upstairs ceiling — not gushing, but the drywall is wet and it's still raining out. Wondering if anyone can come look today or tomorrow. My cell is 469-555-0294. Thanks."
>
> **AI transcription + auto-classification (T+5 sec after voicemail save):**
> - Transcript captured by `company.calendar_integration = google_workspace` voice-AI layer
> - Keywords scored: "leak" + "drywall is wet" + "still raining" = 🔴 active interior water under storm context
> - ZIP 75070 = inside `service_area.zip_codes[]` AND inside `service_area.licensed_counties[] = [Collin, Denton, Dallas, Tarrant]`
> - `response.business_hours = Mon-Fri 08:00-18:00 CT` → 14:43 CT is in-shift → in-shift-recovery branch fires, not after-hours branch
>
> **Tier classification:** 🔴 Emergency (active interior leak + active rain), in-shift call-miss
> **Matched estimator:** Marcus Patel (Tier A, ZIP 75070 ✓, on-call NOT required because in-shift). Andrea Cole (Tier B, ZIP 75070 ✓) flagged as secondary if Marcus's calendar holds nothing in the next 4 hours
> **SLA:** First touch ≤ 60 sec from VM save (not from original call drop) — the in-shift recovery clock starts the moment the VM transcribes
>
> ---
>
> **AI callback (T+45 sec from VM save, ringing David's cell at 14:44 CT):**
> "David — Sam, the AI receptionist at Acme Roofing. I just got your voicemail about the leak at 2207 Heritage Oaks. Three things real quick: are you still home, is anyone safe, and is the water still actively coming through? … OK — Marcus, our HAAG-certified estimator who lives 6 minutes from you, has 3:30 PM today open OR 8:00 AM tomorrow. With the rain still going I'd take the 3:30. Want me to book that and have Marcus call you in the next 5 minutes to coordinate?"
>
> (Note: if David doesn't pick up the AI callback within 2 rings, the call routes directly to Marcus's cell — escalation does NOT wait for a second AI attempt because the active-leak signal already qualifies for human override on the in-shift miss.)
>
> **SMS (T+90 sec from VM save, sent 14:45):**
> "David — Sam at Acme Roofing. Just called you back about the leak at 2207 Heritage Oaks. While the rain's still going, here are two slots for Marcus (our HAAG-certified estimator, 6 min from you):
>
> 🕒 Today 3:30 PM (recommended — leak is active)
> 🕗 Tomorrow 8:00 AM (if today doesn't work)
>
> Reply 1 for today or 2 for tomorrow and I'll confirm immediately. — Sam, Acme Roofing, 469-555-0140"
>
> **Phone script (Marcus, T+4 min — calls David directly while AI receptionist holds the slot):**
> "David — Marcus with Acme Roofing, the guy Sam mentioned. I'm at 1620 Commerce 6 minutes north of you and I'm walking out the door right now. While I drive, two quick things: any electrical fixtures near the wet drywall? Don't touch them. And can you take a phone photo of the ceiling stain so I know which slope to start on when I get there. Send to this number. See you in 8."
>
> **Auto-confirmation email (T+5 min, sent on slot lock):**
> Subject: "Marcus is on the way to 2207 Heritage Oaks — 3:30 PM today"
> Body: "David — confirming Marcus Patel will arrive at 2207 Heritage Oaks Ln by 3:35 PM CT for an emergency tarp + leak assessment. He's licensed (TX-RC-0481234), HAAG-certified, and 14 years on the roof. No charge for the emergency tarp on an active-leak callout. Reschedule link if needed: [acmeroofs.com/r/em-2026060300027]. — Acme Roofing, 469-555-0140."
>
> **Estimator handoff brief (JobNimbus paste, T+6 min from VM save):**
> ```
> LEAD: David Park / 2207 Heritage Oaks Ln, Frisco TX 75070 / 469-555-0294
> SOURCE: inbound_voice → voicemail → ai_callback_recovery / 2026-06-03 14:43 CT (VM saved 14:43:22)
> TIER: 🔴 Emergency (in-shift call-miss recovery) — active interior leak + active rain
> RECOVERY-PATH: response.in_shift_missed_call_recovery_window_seconds = 60 fired at 14:43:27; AI callback 14:44:07; SMS 14:44:52; Marcus dispatched 14:47
> ASSIGNED: Marcus Patel — ETA 15:35 CT today (T+0:52 from original call drop; T+0:48 from VM save)
> SCOPE: Emergency tarp + leak source ID + full inspection booked for tomorrow if scope warrants
> NOTES: Photo of ceiling stain requested via SMS. Rain still active at dispatch time per local radar. Storm context = no NOAA event ID yet — could be local microburst, confirm on arrival.
> NEXT: Marcus calls dispatch on tarp completion. Route to roof-inspection-report skill on inspection completion. Route to insurance-supplement-writer if claim opens.
> CAPACITY-DIAGNOSTIC: 3-concurrent-calls block at 14:43 = repeat of the Tuesday afternoon pattern (3rd in-shift miss this week between 2-3 PM). Recommend adding a second front-desk seat or extending AI receptionist primary-pickup threshold from 22 sec to 35 sec during the 14:00-15:00 CT window.
> ```
>
> **Capacity-diagnostic note (auto-routed to office@acmeroofs.com, daily summary):**
> "In-shift call-miss recovery #3 this week, all between 14:00-15:00 CT (Tue/Wed). Pattern: front desk on 3+ concurrent calls when 4th inbound rings. Estimated value of recovered leads this week: ~$48k (3 × $16k avg residential ticket). Recommend Tier 1 fix: extend AI receptionist primary-pickup threshold from 22 sec to 35 sec during the 14:00-15:00 CT window (config: response.business_hours[].peak_window_pickup_threshold_seconds). Tier 2 fix: stagger front-desk lunch coverage to keep ≥2 humans on the desk through 14:00-15:00."

### Assumptions footer for the v1.4 in-shift recovery example

- `response.in_shift_missed_call_recovery_window_seconds` = 60 — new named binding introduced in v1.4. Default fires the AI callback within 60 sec of voicemail save, separate from the original call drop time. This binding is the single config knob that determines how aggressively the in-shift recovery layer competes with the human front desk
- `response.business_hours = Mon-Fri 08:00-18:00 CT` resolved from config; in-shift branch selected because 14:43 CT is inside the window
- `company.calendar_integration = google_workspace` — voice-AI layer is the same that transcribed and scored the voicemail; calendar reads Marcus's real availability for the slot offer
- ZIP 75070 routed Marcus (Tier A) primary, Andrea (Tier B) secondary — both have 75070 in `service_zips[]`. Marcus picked first because 🔴 Emergency routes to Tier A by default
- Photo-request via SMS uses the same workflow as the v1.2 🔴 storm-night example for continuity — Marcus arrives knowing which slope is leaking
- Capacity-diagnostic note fires when ≥3 in-shift call-misses cluster in the same hour-of-day window over a 7-day rolling period; counter maintained alongside `response.source_quality_log_path` (the ⚪ tier source-quality counter from v1.3) — same logging path, different counter name
- In-shift recovery does NOT enter `predictive-lead-scorer` batch (real-time path, not batch path) but DOES enter `follow-up-sequence` Hot tier after the inspection completes, just like any other 🔴 lead
- New named binding documented in the diagnostic note for future config schema bump: `response.business_hours[].peak_window_pickup_threshold_seconds` — allows per-window override of the AI receptionist's primary-pickup threshold (default 22 sec; commonly extended to 30-35 sec during predictable peak windows like 2-3 PM in residential markets)
