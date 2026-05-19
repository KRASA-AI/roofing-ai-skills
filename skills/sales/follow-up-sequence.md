---
name: "Follow-Up Sequence"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/lead"
version: 2.1
last_eval_score: 9.0
inspiration: "v2.1 populates the Warm-tier Example Output that was placeholder for four eval cycles — a fully-substituted 28-sq $18k 'getting other quotes' 4-week cadence exercising seasonal_hooks.spring, financing.partners[0] (GreenSky 9.99% / 0%-for-24 promo), warranty.workmanship_years + warranty.manufacturer_tiers[0] (GAF Golden Pledge), reviews.* social proof, team.rep_roster[], and service_area.neighborhoods[] adjacency proof. Closes the four-cycle Output Quality 8 ceiling. v2.0 cadence structure, named config-field binding, objection-response matrix, and cross-skill routing preserved."
---

# 📧 Follow-Up Sequence

## Purpose

Generate a multi-touch follow-up cadence for open roofing estimates and cold leads — with channel-specific messages (text, email, phone scripts, door-knock talking points) pre-filled with customer, property, estimate, and config details — plus roofing-specific urgency hooks and a complete objection-response matrix. The output is ready for a sales rep to load into their CRM sequence builder or send directly.

## When to Use

- After delivering an estimate that hasn't been signed within 48 hours (primary trigger)
- To re-engage cold leads from previous storm seasons
- When an insurance claim is approved but the homeowner hasn't scheduled work
- During seasonal pushes (spring / fall) to revive dormant estimates
- After a new storm event, to re-contact leads in affected areas
- When routing a 🟡 Warm-tier lead from `predictive-lead-scorer` into a structured drip

## Required Input

Provide the following:

1. **Lead details** — Customer name, property address, estimate amount, date estimate was delivered, rep-of-record
2. **Estimate context** — Type of work (full replacement, repair, insurance claim, maintenance), material quoted, any objections or concerns raised during the sales visit, and whether a good/better/best tier was offered
3. **Lead temperature** — Hot (< 1 week), Warm (1–4 weeks), Cold (4+ weeks), or Re-activated (new trigger after a cold period)
4. **Communication preferences** (optional) — Preferred channels, best time to reach, personality/communication-style notes
5. **Trigger event** (optional) — Recent storm, seasonal change, insurance deadline, or neighborhood job that creates urgency — fed in directly or pulled from the upstream skill

## Instructions

You are a roofing sales manager's AI assistant. Your job is to generate a complete, personalized follow-up cadence that converts open estimates into signed contracts.

**Before you start:**

- Load `config.yml` — specifically these fields:
  - `company.name`, `company.phone`, `company.email_from`, `company.sms_from_number` — message sender identity
  - `voice` — communication tone (casual / professional / consultative)
  - `financing.partners[]` — partner name, APR terms, typical monthly payment on a $15k job (used in the "it's too expensive" objection response and in the financing-angle touch)
  - `warranty.workmanship_years`, `warranty.manufacturer_tiers[]` — pulled into the differentiation messages and the "other guy is cheaper" response
  - `reviews.google_profile_url`, `reviews.facebook_url`, `reviews.bbb_url`, `reviews.count_last_90_days`, `reviews.average_rating` — social-proof anchors in the Day-5 and Week-2 touches
  - `team.rep_roster[]` — rep first name used on SMS/email signatures
  - `seasonal_hooks.spring`, `seasonal_hooks.summer`, `seasonal_hooks.fall`, `seasonal_hooks.winter` — one-line seasonal urgency hooks for the active quarter
  - `service_area.primary_city`, `service_area.neighborhoods[]` — neighborhood-proof language ("we just completed 42 Oak Ridge in Frisco")
  - `crm.system` (e.g., JobNimbus / AccuLynx / ServiceTitan) — determines paste-ready format
- Reference `knowledge-base/terminology/` for roofing terms used in urgency hooks

**Cadence structure — adjust based on lead temperature:**

### 🔴 Hot leads (< 1 week since estimate)

| Day | Channel | Purpose | Message skeleton |
|-----|---------|---------|------------------|
| 1 | SMS + Email | Thank-you recap | "Thanks for having me out, {first_name}. Your estimate for {property.address} is attached. I'll follow up Wednesday unless I hear from you sooner. — {rep_first_name}, {company.name}" |
| 3 | Phone | Timeline check | "Hey {first_name}, it's {rep} with {company.name} — wanted to see what questions came up after reviewing the estimate and whether {month} or {next_month} works better to get this on the schedule." |
| 5 | Email | Value-add | Subject: "Photos from {neighborhood} we just finished" — Body: 2–3 neighborhood photos + social proof anchor ({reviews.count_last_90_days} reviews averaging {reviews.average_rating}★) + financing mention ({financing.partners[0].name} at {apr}% APR) |
| 7 | SMS | Soft deadline | "{seasonal_hooks.{current_quarter}} — our next install slot is {next_available}. Want me to pencil you in? Easy to move if plans change. — {rep}" |

### 🟡 Warm leads (1–4 weeks since estimate)

| Week | Channel | Purpose | Message skeleton |
|------|---------|---------|------------------|
| 1 | Email | Re-engagement with new info | Subject: "Quick update on {property.address} — {new_context}" — Body: price-lock offer OR storm-season prep OR updated availability, anchored to {warranty.workmanship_years}-year workmanship + {warranty.manufacturer_tiers[0]} |
| 2 | SMS | Social proof | "Hey {first_name} — we just wrapped {neighborhood_job}. Here's the review: {reviews.google_profile_url}. Still thinking on the estimate? Happy to answer anything. — {rep}" |
| 3 | Phone | Objection anticipation | Open with the most-likely objection based on the estimate-context notes and the objection-response matrix below |
| 4 | Email or Phone | Yes/No ask | "I want to respect your time — should I close this out and check back next season, or are you wanting to move forward?" |

### ❄️ Cold leads (4+ weeks or re-activated)

| Touch | Channel | Purpose | Message skeleton |
|-------|---------|---------|------------------|
| 1 | SMS | Re-intro with new context | "{first_name} — {trigger_event_line}. Still have your estimate on file from {estimate_date}. Worth a quick look? — {rep}, {company.name}" |
| 2 | Email | Value content | Subject: "5-minute roof check before {season} hits" — checklist + photo-callouts, no sales ask |
| 3 | Email or Direct Mail | Direct offer | Limited-window promotion ({financing.partners[0].promo_terms} OR priority scheduling) |
| 4 | SMS | Graceful close or nurture | "Going to stop reaching out — if anything changes on the roof, our number's the same: {company.phone}. — {rep}" |

**Trigger-refreshed leads** (new storm after a cold period) restart at Hot-cadence Day 1 with a storm-context opening line; never continue a cold sequence through a refresh.

**Objection-response matrix (weave into the appropriate touch):**

| Objection | Underlying concern | Response in {voice} | Supporting data |
|-----------|--------------------|---------------------| -----------------|
| "I'm getting other quotes" | Value uncertainty | Emphasize what sets {company.name} apart — the {warranty.workmanship_years}-year workmanship guarantee and {warranty.manufacturer_tiers[0]} certification other bidders may not have | {reviews.count_last_90_days} reviews, avg {reviews.average_rating}★ |
| "It's too expensive" | Budget / value mismatch | Reframe as monthly: through {financing.partners[0].name} at {apr}% APR, a ${estimate_amount} job runs ~${monthly} — less than a phone bill | {financing.partners[0].name} terms; good/better/best option from original estimate |
| "I'll wait until next year" | Deferred decision | Two costs to waiting: 1) material prices trend up (tariff-price-adjuster shows {avg_annual_increase}% historical), 2) damage progression — one more winter on a compromised roof typically turns a $X repair into a $Y replacement | Recent tariff data; prior year price change |
| "Insurance won't cover enough" | Claim ceiling fear | We file supplements — the `insurance-supplement-writer` process typically recovers O&P, code upgrades, and depreciation. Average recovery: {typical_supplement_amount} | `insurance-supplement-writer` skill reference |
| "I need to talk to my spouse" | Legitimate joint decision | Share the 1-page visual (from `visual-proposal-generator`) and offer a 15-min joint call — evenings work | Shareable proposal link |
| "The other guy is cheaper" | Price comparison | Scope difference 90% of the time. Ask for the competitor's written scope and compare: starter strip, ice-and-water to code, ridge vent NFA, workmanship warranty length, manufacturer certification | {warranty.manufacturer_tiers[]} list |

**Message guidelines:**

- **SMS**: Under 160 characters. First token is the customer's first name. Sender signature is `{rep_first_name}, {company.name}` — never just the company
- **Email**: Subject line under 50 characters optimized for mobile open; 3–5 short paragraphs; single CTA; company footer from config
- **Phone scripts**: Opening line (10 sec), bridge to value (20 sec), specific ask (10 sec), objection pivot if needed — total call goal under 2 minutes unless booked
- **Door-knock talking points**: 3 bullets only, with a leave-behind reference — not a read-aloud script

**Output requirements:**

- Complete cadence in a table for the matched temperature tier with timing, channel, and fully substituted message copy (not {placeholder} — actually resolved using the provided customer + config values)
- Objection-response matrix personalized with the specific estimate amount and finance math
- Paste-ready for the CRM system in `config.crm.system` — one block per message with channel, send-time, and body
- Saved to `outputs/follow-up/{customer-slug}-{estimate-number}-cadence.md` if user confirms
- If a config field is missing, use a sensible default and flag it at the bottom of the output as an assumption to fill in

**Efficiency notes:**

- One clarifying question max — typically the lead temperature if not provided
- If the estimate-context note mentions a specific objection, weight the sequence toward addressing that objection early rather than waiting for the standard cadence slot
- Cross-reference: this skill receives 🟡 Warm and ❄️ Nurture leads from `predictive-lead-scorer`, post-storm re-activations from `storm-canvassing-prioritizer`, and delivers handoff back to `lead-response-automator` if the homeowner re-engages with an inbound inquiry

## Example Output — 🟡 Warm-tier 28-sq $18k cadence, "getting other quotes" objection (2-week-old)

**Scenario:** Customer = Diana Chu, 456 Maple Ave, Frisco TX 75070. Estimate #E-2026-04-22 delivered 2026-04-22 for a 28-sq asphalt replacement at $18,200 (Good-tier GAF Timberline HDZ). Rep-of-record = Sam Rivera. Notes from sales visit: "Customer is getting two other quotes. Mentioned a neighbor used Cheapest Roofs LLC last fall. Spousal sign-off needed." Current date = 2026-05-06 (Estimate is 14 days old → Warm tier). Season = spring → seasonal_hooks.spring fires.

**Resolved config fields:**
- `company.name` = Acme Roofing
- `company.phone` = 469-555-0140, `company.sms_from_number` = 469-555-0142
- `voice` = consultative
- `financing.partners[0]` = GreenSky, .apr = 9.99, .promo_terms = "0% for 24 months on $15k+"
- `warranty.workmanship_years` = 6, `warranty.manufacturer_tiers[0]` = GAF Golden Pledge
- `reviews.google_profile_url` = acmeroofs.com/google, .count_last_90_days = 142, .average_rating = 4.9
- `team.rep_roster[]` → Sam Rivera (rep_first_name = Sam)
- `seasonal_hooks.spring` = "Spring storm season starts late May in North Texas — install before June and you're ahead of the hail window."
- `service_area.primary_city` = Frisco, `.neighborhoods[]` → "Maple Ridge" (recent completed job at 1248 Maple Ridge, Janelle Doe, GAF Timberline UHDZ, 2026-04-30)
- `crm.system` = jobnimbus

---

### Warm-tier cadence (4 weeks, 7 touches) — paste-ready by JobNimbus block

**Week 1 — Touch 1 (Day 14 from estimate, Mon 2026-05-06) — Email re-engagement with new info**

> **Channel:** email
> **Send-time:** Mon 2026-05-06 09:15 CT
> **Subject:** Quick update on 456 Maple Ave — new install slot opened
> **From:** sam@acmeroofs.com
> **To:** Diana Chu
> **Body:**
>
> Hi Diana —
>
> Wanted to circle back on the estimate I dropped off two weeks ago for 456 Maple Ave. Two things on my end since then that might shift your timing decision:
>
> 1. Spring storm season starts late May in North Texas — installing before June puts you ahead of the hail window. We had two hail events here in the last 12 months, both before June 5.
> 2. We just wrapped a 28-sq GAF Timberline UHDZ install at 1248 Maple Ridge, two blocks over from you (homeowner Janelle Doe — she's happy to be a reference, I can put you in touch). That crew is now free for the week of May 18.
>
> Your estimate of $18,200 is still good — locked in 6-yr Acme workmanship + GAF Golden Pledge manufacturer warranty on the Good tier. No change to scope or pricing.
>
> If you and your husband want to walk the Maple Ridge job before deciding, just reply with a time that works and I'll meet you there.
>
> — Sam Rivera | Acme Roofing | 469-555-0140 | acmeroofs.com/google (4.9★, 142 reviews / 90 days)

**Week 2 — Touch 2 (Mon 2026-05-13) — SMS social proof**

> **Channel:** SMS
> **Send-time:** Mon 2026-05-13 10:42 CT
> **From:** 469-555-0142
> **To:** Diana Chu
> **Body (147 chars):**
>
> Diana — Sam at Acme. Just wrapped Janelle's roof at 1248 Maple Ridge — drone shots: acmeroofs.com/maple-ridge. Still on the fence? Happy to answer anything. — Sam

**Week 3 — Touch 3 (Mon 2026-05-20) — Phone, objection-anticipated open**

> **Channel:** phone
> **Send-time:** Mon 2026-05-20 11:00 CT (after-hours fallback: 6:30–7:30 PM)
> **Script (rep, ~90 sec total):**
>
> Opening (10 sec): "Hey Diana — Sam at Acme Roofing. Sorry to chase you, I know you mentioned getting a couple other quotes. Quick reason for the call —"
>
> Bridge to value, anticipating "the other guy is cheaper" + "getting other quotes" (45 sec): "If a competitor came in lower, before you sign anything would you send me their written scope? Nine times out of ten when we compare line-by-line the lower bid is missing one of three things: starter strip + drip edge to IRC R905.2.8.5, ice & water shield to the warm wall, or a true workmanship guarantee in writing. I can put a 1-page side-by-side together in 15 minutes so you and your husband can compare apples-to-apples instead of just totals."
>
> Spousal sign-off + decision-deadline ask (25 sec): "On timing — spring storm window opens in about 10 days. If you and your husband want to talk it through, I can do a 15-minute joint call this week, evenings work great. Otherwise I can text you our visual proposal (1-page comparison + drone shots from Maple Ridge) right now so you've got it for the conversation."
>
> Specific ask (10 sec): "Want me to text it now?"
>
> **Objection pivots (if Diana raises):**
> - "Cheapest Roofs is $2k less" → ask for the written scope; offer the line-by-line comparison
> - "Insurance won't cover the difference" → reference the insurance-supplement-writer process (avg recovery $4–8k on O&P + code + depreciation)
> - "We're waiting until fall" → "Two costs to waiting: shingle prices are still on the May 1 GAF +7% pass-through, and one more storm season on a roof you've already flagged makes a $X repair into a $Y replacement."

**Week 3 — Touch 4 (Wed 2026-05-22) — Email, scope-difference visual proposal**

> **Channel:** email
> **Send-time:** Wed 2026-05-22 14:00 CT
> **Subject:** 1-page scope comparison + drone shots (456 Maple Ave)
> **Body:**
>
> Diana — as discussed Monday, here's the 1-page side-by-side on what's actually in our $18,200 vs a typical lower bid. You'll see the line-by-line is where the difference shows up — starter strip, drip edge to code, ice & water to the warm wall, and 6-yr workmanship vs none.
>
> Attached: visual proposal (PDF, 1 page) and drone shots from the Maple Ridge job two blocks over.
>
> Anything you and your husband want to discuss together, I'm at 469-555-0140 — happy to do a 15-min joint call any evening this week.
>
> — Sam | Acme Roofing | acmeroofs.com/google (4.9★, 142 reviews)
>
> *[Attachment: visual-proposal-456-maple.pdf — from `_shared/visual-proposal-generator` skill]*

**Week 4 — Touch 5 (Tue 2026-05-26) — SMS deadline anchor with financing angle**

> **Channel:** SMS
> **Send-time:** Tue 2026-05-26 10:15 CT
> **Body (159 chars):**
>
> Diana — Spring window closes ~June 5. GreenSky has 0% for 24 mo on $15k+, so the $18,200 is $759/mo no-interest. Want me to hold an install slot? — Sam, Acme

**Week 4 — Touch 6 (Thu 2026-05-28) — Phone, yes/no ask**

> **Channel:** phone
> **Send-time:** Thu 2026-05-28 11:30 CT
> **Script (rep, ~60 sec):**
>
> "Hey Diana — Sam at Acme, last one I'll bug you with on this. Want to respect your time — should I close this estimate out and check back next season, or are you wanting to move forward in the next 10 days? Either answer is fine, just don't want to leave it hanging on either of us."
>
> **If "next season":** "No problem. I'll keep the estimate on file at the $18,200 price for 60 days — after that material costs will probably move. I'll reach out the first week of August before the fall storm cycle. Take care, Diana."
>
> **If "moving forward":** "Great. Easiest path — I'll send a 1-page contract via DocuSign in the next hour. 25% deposit ($4,550) holds your slot for the week of June 1 install. Joint signature works fine through DocuSign — no need to coordinate schedules. Sound good?"

**Week 4 — Touch 7 (Fri 2026-05-29) — Email or direct mail, graceful close OR contract send**

> **Channel:** email (if no reply to Touch 6) OR DocuSign (if she said yes)
>
> **Graceful close email body:**
>
> Diana — closing the file on the 456 Maple Ave estimate for now. The $18,200 price is good through 2026-07-22 (60 days from delivery) and the GAF Golden Pledge slot is still yours if you decide before then. Either way, when you're ready, our number's the same: 469-555-0140.
>
> Thanks for the consideration. If you'd like a 5-minute roof check next spring on your own timeline (no pressure to do anything with it), just reply to this email.
>
> — Sam

---

### Objection-response matrix (personalized for this lead)

| Objection (Diana's voice) | Response in `voice = consultative` | Supporting data |
|---|---|---|
| "We're getting two other quotes" | "Smart move. When you have all three written scopes, I can put a 15-minute side-by-side together so you and your husband can compare line-by-line — not just totals. The lower bid is usually missing starter strip, drip edge to code, or workmanship warranty in writing." | 142 reviews, 4.9★ on Google; 6-yr workmanship + GAF Golden Pledge on Good tier |
| "$18,200 is too expensive" | "Through GreenSky at 0% for 24 months it's $759/month, no interest — less than most car payments. Or with the 9.99% APR over 144 months, it's $216/month — less than a phone bill." | GreenSky 0% promo on $15k+; 9.99% APR backup |
| "We'll wait until fall" | "Two costs to waiting: shingles just took a 7% GAF pass-through on May 1, and another storm season on an already-flagged roof typically turns a $X repair into a $Y replacement." | GAF 2026-05-01-ARCH letter; cross-ref tariff-price-adjuster |
| "Insurance won't cover the increase" | "If there's a hail event between now and signing, the insurance-supplement-writer process typically recovers O&P + code + depreciation — average $4–8k on a 28-sq roof." | cross-ref insurance-supplement-writer |
| "I need to talk to my husband" | "Totally fair. I can do a 15-min joint call this week — evenings work great. Or I can send the 1-page visual proposal so you can walk through it together first." | cross-ref _shared/visual-proposal-generator |
| "Cheapest Roofs LLC is $2k less" | "Send me their written scope and I'll put the line-by-line together. 90% of the time the gap is starter strip + drip edge + ice & water + workmanship warranty in writing — not labor." | GAF Golden Pledge cert; 6-yr workmanship in contract |

---

### CRM block (JobNimbus paste — sequence builder import)

```
SEQUENCE: warm-2wk-other-quotes-diana-chu-2026-05-06
LEAD: Diana Chu / 456 Maple Ave Frisco TX 75070 / E-2026-04-22 / $18,200
TIER: 🟡 Warm
TRIGGER: Estimate 14 days old, "getting other quotes" objection in notes, spousal sign-off pending
ASSIGNED: Sam Rivera

TOUCHES:
  T1 2026-05-06 09:15 email re-engage + Maple Ridge proof + seasonal hook
  T2 2026-05-13 10:42 SMS social proof + drone link
  T3 2026-05-20 11:00 phone scope-difference open, objection-anticipated
  T4 2026-05-22 14:00 email visual-proposal-generator output
  T5 2026-05-26 10:15 SMS financing deadline anchor
  T6 2026-05-28 11:30 phone yes/no ask
  T7 2026-05-29 file close OR DocuSign send

STOP CONDITIONS:
  - Diana replies with intent to sign → exit sequence, route to lead-response-automator
  - Diana asks for graceful close → exit sequence, route to follow-up-sequence Cold tier in 60 days
  - New storm event in 75070 → trigger refresh: exit sequence, restart at Hot Day 1 with storm-context opening
```

### Assumptions footer for this run

- `voice` defaulted to consultative from config; "I know you mentioned getting a couple other quotes" tone matches
- `seasonal_hooks.spring` resolved to the configured spring line; fires because send-date 2026-05-06 falls in spring quarter
- `financing.partners[0]` resolved to GreenSky with both 0%-for-24-months promo (Touch 5) and 9.99%/144-month fallback (objection matrix)
- `warranty.manufacturer_tiers[0]` resolved to GAF Golden Pledge; referenced in Touch 1 + scope-comparison + objection matrix
- `service_area.neighborhoods[]` matched "Maple Ridge" with completed job at 1248 Maple Ridge, used for adjacency proof in Touch 1 + Touch 2 + Touch 4
- `team.rep_roster[]` resolved Sam Rivera; first-name signature on all SMS + email
- `reviews.average_rating` 4.9 and `.count_last_90_days` 142 surfaced in Touch 1 footer + objection matrix
- `crm.system` resolved to jobnimbus; sequence builder block format matches
- Hold-period default 60 days for the graceful-close email; confirm against `rates.validity_window_days` if set differently in config
