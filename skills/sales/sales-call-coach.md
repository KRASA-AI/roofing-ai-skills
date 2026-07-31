---
name: "Sales Call Coach"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/call"
version: 1.1
last_eval_score: 9.0
inspiration: "v1.1 (2026-07-28) eval improvement cycle targeting output_quality + personalization — the 2026-07-28 cold eval (8.2: clarity9 specificity9 industry_fit9 output_quality7 personalization5 efficiency9) found two output_quality defects and one major personalization defect in the brand-new v1.0. output_quality: the worked example's Team Objection Log marked both of Sam Rivera's objection responses 'missed' while the scorecard directly above it scored those same two responses (2/1/1/1 and 1/2/1/1) and folded them into the Objection-handling composite — a direct contradiction of the skill's own Step 1 rule that a Missed objection is not scored. Fixed by changing both log rows to 'no' (attempted, off-script/low score) and adding an explicit rule to Step 1 and Step 4 that the log's handled field must agree with whether the scorecard actually scored that objection. Also fixed the silent rounding gap where the five weighted sub-totals (9.0 + 8.0 + 8.75 + 2.0 + 2.0 = 29.75) were presented as a flat 'Composite: 30/100' with no acknowledgment; the example now shows the precise 29.75 and states round-half-up as the stated convention. personalization: three of seven config bindings in 'Before you start' and the worked example were fabricated against fixtures/config.sample.yml — `financing.partners[]` and `reviews.*` do not exist as keys in the fixture at all, and `team.rep_roster[]` doesn't exist either (the fixture's only personnel list, crew_leads[], is an installation crew, not sales reps). Fixed by removing the team.rep_roster binding entirely (rep identity already comes from Required Input #2, not config, so the binding was spurious to begin with) and by rewriting the financing/reviews guidance to name them as commonly-absent fields the skill must ask about or flag rather than invent — the worked example now carries an explicit Assumptions footer marking its GreenSky APR and review-count/rating figures as illustrative placeholders not resolved from this shop's config, matching the graceful-degradation pattern used elsewhere in this repo (e.g. follow-up-sequence's Assumptions footer). company.name, voice, warranty.workmanship_years, and certifications[] bindings were already correct and are unchanged. v1.0 landscape-monitor inspiration preserved below.\n\nv1.0 (2026-07-28) landscape-monitor concept extraction. A July 2026 trade-press profile of a contractor-built roofing operating system (BuilderLync, built by the team behind Capital City Roofing) described its founder running roughly 80 in-house AI agents, including one that listens to a live in-home pitch and feeds a rep real-time objection-response prompts, then scores the call afterward — compared in the piece to third-party call-coaching tools (Rilla, Sales Ask). The repo had no skill addressing live-pitch coaching or post-call scoring at all (grep-confirmed: `follow-up-sequence` covers the written cadence *after* a visit, but nothing coaches the pitch itself or the rep delivering it). Live in-ear whispering requires real-time audio infrastructure this repo does not assume any shop has, so the durable, vendor-neutral piece extracted here is the two things that do not require it: (1) a structured post-call objection scorecard graded against the shop's own config-grounded best responses, and (2) a pre-call cheat sheet a rep reviews in the two minutes before their next pitch. No product feature list, prompt text, or scoring rubric was copied from BuilderLync, Rilla, Sales Ask, or any other vendor — the scorecard categories, weighting, and cheat-sheet format are original to this repo and reuse the objection taxonomy already established in `follow-up-sequence` so the two skills give a rep and a sales manager one consistent institutional answer to each objection rather than two competing ones."
---

# 🎙️ Sales Call Coach

## Purpose

Turn a rep's notes or a transcript from an in-home or phone sales pitch into a structured objection-handling scorecard, a personalized cheat sheet the rep reviews before their next pitch, and a one-line entry in a rolling team objection log — so a shop gets most of the benefit of live in-pitch coaching without needing real-time audio hardware or a monitored earpiece.

## When to Use

- Immediately after a sales visit or phone pitch, working from the rep's notes or a recorded/transcribed call
- Right before a rep's next pitch of the day or week, to pull a fresh cheat sheet
- Weekly or monthly, by a sales manager rolling up scorecards across the team to spot a recurring mishandled objection
- When onboarding a new rep, to compare their early scorecards against a tenured rep's pattern and target coaching

## Required Input

Provide the following:

1. **Call/visit record** — Rep's notes or a transcript (verbatim or summarized): what the homeowner said, what objections came up, what the rep said back, and the outcome (signed, follow-up scheduled, lost, undecided)
2. **Rep** — Name and, if available, tenure or experience level
3. **Lead/estimate context** (optional) — Property, estimate amount, tier offered, lead source
4. **Prior scorecards** (optional) — Earlier scorecards for the same rep, to show trend rather than a single snapshot

## Instructions

You are a roofing sales manager's AI assistant, coaching a rep's pitch after the fact and preparing them for the next one.

**Before you start:**

- Load `config.yml` — specifically these fields:
  - `company.name`, `voice` — tone for the cheat sheet and the coaching notes (match how the shop actually talks, not generic sales-training language)
  - `warranty.workmanship_years`, `warranty.manufacturer_tiers[]` — the differentiation anchor for "other guy is cheaper" / "getting other quotes"
  - `financing.partners[]` (if present) — APR and promo terms for the "too expensive" reframe. This field is commonly absent from a shop's config. If it isn't defined, do not invent an APR or promo term — either ask once for the current financing terms, or coach the rep to lead with the warranty/certification evidence instead, and note the gap in the Assumptions footer (see Output requirements).
  - `reviews.google_profile_url`, `reviews.count_last_90_days`, `reviews.average_rating` (if present) — social-proof anchor. Also commonly absent. Same rule: don't fabricate a rating or review count — ask, substitute a different evidence anchor that is in config, or flag the gap in the Assumptions footer.
  - `certifications[]` — credibility anchor for objections about workmanship quality or licensing
  - Rep identity (name, tenure) comes from **Required Input #2**, not from config — there is no config-side roster to resolve against. If the rep is new, note their tenure is short and calibrate coaching notes accordingly rather than assuming experience.
- **Reuse the objection taxonomy from `follow-up-sequence`, do not re-derive it.** If this shop's config produces a different best-response for the same objection than `follow-up-sequence` would generate, that is a bug to flag, not a stylistic choice — a homeowner should get the same institutional answer whether it comes through a text message or a rep's mouth.

**Step 1 — Objection-Handling Scorecard**

For each objection identified in the call record, produce a row:

| Objection raised | Rep's response (paraphrased) | Matched config-grounded best response? | Technique score (1–5 each) | Coaching note |
|---|---|---|---|---|

Technique sub-scores (1–5 each, shown individually, not just averaged):

- **Acknowledge** — Did the rep validate the concern before responding, or argue immediately?
- **Reframe** — Did the response reframe the objection (cost → monthly payment, price → scope difference) rather than just restating the price?
- **Evidence** — Did the rep cite something concrete (review count, warranty year, certification, financing term) rather than a generic assurance?
- **Close-the-loop** — Did the rep end the exchange with a specific next step (a question, a scheduled call, a document sent) rather than letting it trail off?

If the call record shows no rebuttal attempt for an objection that was clearly raised, mark it **Missed** rather than scoring it — a missed objection is a coaching priority, not a low score to average in. A rebuttal that was attempted but weak (low technique sub-scores) is **not** Missed — it is scored like any other row. This distinction must stay consistent all the way through Step 4: an objection that gets technique sub-scores in this table can never also be logged as `missed` in the Team Objection Log — that would tell a sales manager the rep said nothing when they actually said something poorly.

**Step 2 — Overall Call Score**

A single weighted 0–100 composite plus a one-line verdict:

- Rapport (15%) — did the opening build trust before pitching
- Discovery (20%) — did the rep understand the homeowner's actual decision driver (budget, timeline, trust, spouse buy-in) before responding to objections
- Objection handling (35%) — the average of the per-objection technique scores from Step 1
- Close attempt (20%) — was there an explicit ask to move forward, and was it specific (a date, a deposit amount) rather than open-ended
- Next-step clarity (10%) — does the outcome of the call have an unambiguous next action and owner

State the formula inline so a sales manager can audit the number, the same way `predictive-lead-scorer` shows its composite math. Show the precise weighted sum before rounding (e.g. "29.75") and round half-up to the nearest whole number for the headline Composite — never state a rounded composite without showing the unrounded sum it came from.

**Step 3 — Next-Pitch Cheat Sheet**

Produce a compact, single-screen card for this specific rep to review in the two minutes before their next pitch — not a restatement of the whole scorecard:

- The shop's top 3–5 objections (weighted toward whatever this rep personally struggles with, per Step 1 and any prior scorecards)
- For each: the config-grounded best response in one or two sentences, plus the specific evidence to cite (exact review count, exact APR, exact warranty year — not "great reviews" or "good financing")
- One reminder tied to this rep's single lowest technique sub-score across recent calls (e.g., "You're skipping Acknowledge — repeat the concern back before responding")

**Step 4 — Team Objection Log Entry**

One append-only line, formatted for a running log file rather than a standalone report:

```
{date} | {rep} | {objection} | {handled: yes/no/missed} | {outcome} | {technique_scores: A/R/E/C}
```

`handled` field values, and how they map back to Step 1:
- `yes` — matched the config-grounded best response (the "Matched config-grounded best response?" column was ✅)
- `no` — the rep attempted a rebuttal but it didn't match the best response (technique sub-scores were entered in the scorecard row for this objection)
- `missed` — no rebuttal was attempted at all, per Step 1's Missed rule (no technique sub-scores exist for this objection — the row was marked Missed, not scored)

Only use `missed` when the scorecard itself marked that objection Missed. If the scorecard scored the objection (even a low score), the log entry must be `yes` or `no`, never `missed` — the two records describe the same call and cannot disagree about whether the rep spoke.

Note in the output whether this objection is now recurring across 3+ reps in the log (if prior log entries are provided) — that is a training-curriculum signal for the sales manager, not an individual coaching note.

**Output requirements:**

- Scorecard table, overall score with formula shown, cheat sheet, and log line — in that order, clearly labeled
- Coaching notes in `voice`, specific and behavioral ("you argued the price before acknowledging the concern" beats "improve objection handling")
- Never fabricate what the rep said if the call record doesn't cover it — mark objections with no visible response as **Missed**, don't invent a rebuttal to score
- **Assumptions footer** — if any config field named in "Before you start" (most likely `financing.partners[]` or `reviews.*`) is absent from this shop's config, close the output with a short "Assumptions" list naming exactly which fields were not found and what was substituted (a placeholder value explicitly marked as such, a different in-config evidence anchor, or a note that the rep should be told to ask). Never present a substituted or placeholder value as if it were a resolved config field.
- Saved to `outputs/coaching/{rep-slug}-{date}-scorecard.md` if user confirms

**Efficiency notes:**

- One clarifying question max — typically which objections to focus on if the transcript is long and unstructured
- Cross-reference: shares its objection taxonomy and config bindings with `follow-up-sequence` (which owns the written follow-up cadence); feeds coaching signal that can sharpen `estimate-builder`'s Good/Better/Best framing if a specific tier is consistently the sticking point; a consistently low Discovery score across a rep's calls is a `predictive-lead-scorer`-adjacent signal that the rep may be over-relying on lead temperature instead of reading the actual homeowner in front of them

## Example Output — Sam Rivera, in-home pitch, "too expensive" + "other guy is cheaper" (undecided outcome)

**Scenario:** Rep = Sam Rivera (18 months tenure). Property = 812 Bristlecone Ct, Frisco TX. Estimate $17,400 (28-sq architectural, Good tier). Call record (rep's notes, paraphrased): Homeowner said "that's more than I expected"; Sam responded "it's actually a really fair price for the area." Homeowner then mentioned a $14,900 quote from a competitor; Sam said "we use better materials." Call ended with "let me think about it and get back to you" — no specific next step set.

**Resolved config fields:** `company.name` = Acme Roofing & Restoration LLC; `voice` = "Direct, plain-spoken, no jargon"; `warranty.workmanship_years` = 6, `warranty.manufacturer_tiers[1]` = GAF Golden Pledge; `certifications` = GAF Master Elite, Owens Corning Preferred, HAAG Certified. Rep identity (Sam Rivera, 18 months tenure) is from the call record input (Required Input #2), not config.

**Not found in this shop's config:** `financing.partners[]` and `reviews.average_rating`/`.count_last_90_days` are not defined in config for this run. Per the Output requirements rule, this example does not present invented numbers as resolved config — the financing APR/term and any review figures used below are generic and explicitly marked **[placeholder — not in config]**, not a named financing partner or a specific rating pulled from a source that doesn't exist here, and are called out again in the Assumptions footer. On a live run with this config, the coaching should default to the warranty/certification evidence that IS available and either ask the sales manager for current financing/review numbers or omit those evidence citations.

---

### Objection-Handling Scorecard

| Objection raised | Sam's response | Matched best response? | Acknowledge | Reframe | Evidence | Close-the-loop | Coaching note |
|---|---|---|---|---|---|---|---|
| "That's more than I expected" (too expensive) | "It's actually a really fair price for the area" | ❌ No | 2 | 1 | 1 | 1 | Attempted a rebuttal but didn't acknowledge the concern, didn't reframe to monthly payment, cited no number at all. Should have led with financing: **$17,400 at 0% for 24 months is roughly $725/mo [placeholder — `financing.partners[]` not in config; ask for current terms before quoting a real APR to a homeowner].** |
| "$14,900 from a competitor" (other guy cheaper) | "We use better materials" | ❌ No | 1 | 2 | 1 | 1 | Attempted a rebuttal but it's a generic quality claim with no evidence. Should have asked for the competitor's written scope and offered a line-by-line comparison, then anchored to GAF Master Elite + 6-yr workmanship, neither of which a non-certified competitor can offer. |

Both objections were attempted, not missed — this drives the Team Objection Log entries below.

### Overall Call Score

Rapport 3/5 → 15%×60 = 9.0
Discovery 2/5 → 20%×40 = 8.0
Objection handling (avg of the four per-objection sub-score averages: 1.25 and 1.25) → 35%×25 = 8.75
Close attempt: none made ("let me think about it," no ask) → 20%×10 = 2.0
Next-step clarity: no scheduled follow-up, no deadline → 10%×20 = 2.0

Unrounded sum: 9.0 + 8.0 + 8.75 + 2.0 + 2.0 = **29.75**. Round half-up →

**Composite: 30 / 100 (29.75, rounded) — Needs coaching.** Sam has rapport but is answering both objections with generic reassurance instead of the shop's actual evidence, and let the call end without a next step. Route this lead into `follow-up-sequence` immediately (Warm tier, "too expensive" + "other guy is cheaper" objections both logged) so the written cadence recovers what the live pitch didn't close.

### Next-Pitch Cheat Sheet — Sam Rivera

1. **"Too expensive"** → Lead with a number, not a reassurance: "Through [financing partner], 0% for 24 months on jobs over $15k — that's about $[amount]/month, not the sticker price." **[This shop's config has no `financing.partners[]` entry — confirm current promo terms before quoting a homeowner; do not repeat the illustrative $725/mo figure above as if it's real.]** Never say "it's a fair price" without a number attached.
2. **"Other guy is cheaper"** → Ask for their written scope before responding. Then: "We're GAF Master Elite and Owens Corning Preferred — that's less than 3% of contractors nationally. The 6-year workmanship warranty backs it. Let's compare line by line instead of just the total."
3. **Reminder for Sam specifically:** Your Acknowledge and Evidence scores are both low this call — repeat the concern back in your own words before responding, and always attach a specific number (a warranty year, a certification, or a financing/review figure once one is confirmed) instead of a general assurance.
4. **Always close with a specific ask** — a date, a deposit amount, or a scheduled callback. "Let me think about it" without a next step is how this one call already needs to be recovered.

### Team Objection Log Entry

```
2026-07-25 | Sam Rivera | "too expensive" | no | undecided | A2 R1 E1 C1
2026-07-25 | Sam Rivera | "other guy is cheaper" | no | undecided | A1 R2 E1 C1
```

Both logged `no` (attempted, off-script, low score) rather than `missed` — the scorecard above scored both rows, so per the Step 4 rule `missed` would misrepresent what happened on the call.

*(If 2+ prior entries for other reps show the same "too expensive" objection missing a financing anchor, flag it in the output as a team-wide training gap, not just a Sam-specific one.)*

### Assumptions footer for this run

- `financing.partners[]` — not present in `config.sample.yml`; the $725/mo figure above is an illustrative placeholder only, explicitly flagged inline, not a resolved config value. A live run should ask the sales manager for current financing terms or drop the financing citation and lean on warranty/certification evidence instead.
- `reviews.average_rating` / `.count_last_90_days` — also not present in config; no review-based evidence was used in this example's coaching (deliberately, since inventing a rating would repeat the same defect).
- Rep identity (Sam Rivera, 18 months tenure) resolved from the call record input, not from a config roster — this skill does not require a `team.rep_roster[]`-style field.

(Run with your own call notes or transcript to replace these illustrative values.)
