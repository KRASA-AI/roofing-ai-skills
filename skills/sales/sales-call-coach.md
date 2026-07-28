---
name: "Sales Call Coach"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/call"
version: 1.0
last_eval_score: null
inspiration: "v1.0 (2026-07-28) landscape-monitor concept extraction. A July 2026 trade-press profile of a contractor-built roofing operating system (BuilderLync, built by the team behind Capital City Roofing) described its founder running roughly 80 in-house AI agents, including one that listens to a live in-home pitch and feeds a rep real-time objection-response prompts, then scores the call afterward — compared in the piece to third-party call-coaching tools (Rilla, Sales Ask). The repo had no skill addressing live-pitch coaching or post-call scoring at all (grep-confirmed: `follow-up-sequence` covers the written cadence *after* a visit, but nothing coaches the pitch itself or the rep delivering it). Live in-ear whispering requires real-time audio infrastructure this repo does not assume any shop has, so the durable, vendor-neutral piece extracted here is the two things that do not require it: (1) a structured post-call objection scorecard graded against the shop's own config-grounded best responses, and (2) a pre-call cheat sheet a rep reviews in the two minutes before their next pitch. No product feature list, prompt text, or scoring rubric was copied from BuilderLync, Rilla, Sales Ask, or any other vendor — the scorecard categories, weighting, and cheat-sheet format are original to this repo and reuse the objection taxonomy already established in `follow-up-sequence` so the two skills give a rep and a sales manager one consistent institutional answer to each objection rather than two competing ones."
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
  - `financing.partners[]` — APR and promo terms for the "too expensive" reframe
  - `reviews.google_profile_url`, `reviews.count_last_90_days`, `reviews.average_rating` — social-proof anchor
  - `certifications[]` — credibility anchor for objections about workmanship quality or licensing
  - `team.rep_roster[]` — resolve the rep's name; if the rep is new, note their tenure is short and calibrate coaching notes accordingly rather than assuming experience
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

If the call record shows no rebuttal attempt for an objection that was clearly raised, mark it **Missed** rather than scoring it — a missed objection is a coaching priority, not a low score to average in.

**Step 2 — Overall Call Score**

A single weighted 0–100 composite plus a one-line verdict:

- Rapport (15%) — did the opening build trust before pitching
- Discovery (20%) — did the rep understand the homeowner's actual decision driver (budget, timeline, trust, spouse buy-in) before responding to objections
- Objection handling (35%) — the average of the per-objection technique scores from Step 1
- Close attempt (20%) — was there an explicit ask to move forward, and was it specific (a date, a deposit amount) rather than open-ended
- Next-step clarity (10%) — does the outcome of the call have an unambiguous next action and owner

State the formula inline so a sales manager can audit the number, the same way `predictive-lead-scorer` shows its composite math.

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

Note in the output whether this objection is now recurring across 3+ reps in the log (if prior log entries are provided) — that is a training-curriculum signal for the sales manager, not an individual coaching note.

**Output requirements:**

- Scorecard table, overall score with formula shown, cheat sheet, and log line — in that order, clearly labeled
- Coaching notes in `voice`, specific and behavioral ("you argued the price before acknowledging the concern" beats "improve objection handling")
- Never fabricate what the rep said if the call record doesn't cover it — mark objections with no visible response as **Missed**, don't invent a rebuttal to score
- Saved to `outputs/coaching/{rep-slug}-{date}-scorecard.md` if user confirms

**Efficiency notes:**

- One clarifying question max — typically which objections to focus on if the transcript is long and unstructured
- Cross-reference: shares its objection taxonomy and config bindings with `follow-up-sequence` (which owns the written follow-up cadence); feeds coaching signal that can sharpen `estimate-builder`'s Good/Better/Best framing if a specific tier is consistently the sticking point; a consistently low Discovery score across a rep's calls is a `predictive-lead-scorer`-adjacent signal that the rep may be over-relying on lead temperature instead of reading the actual homeowner in front of them

## Example Output — Sam Rivera, in-home pitch, "too expensive" + "other guy is cheaper" (undecided outcome)

**Scenario:** Rep = Sam Rivera (18 months tenure). Property = 812 Bristlecone Ct, Frisco TX. Estimate $17,400 (28-sq architectural, Good tier). Call record (rep's notes, paraphrased): Homeowner said "that's more than I expected"; Sam responded "it's actually a really fair price for the area." Homeowner then mentioned a $14,900 quote from a competitor; Sam said "we use better materials." Call ended with "let me think about it and get back to you" — no specific next step set.

**Resolved config fields:** `company.name` = Acme Roofing & Restoration LLC; `voice` = "Direct, plain-spoken, no jargon"; `warranty.workmanship_years` = 6, `warranty.manufacturer_tiers[1]` = GAF Golden Pledge; `financing.partners[0]` = GreenSky, 9.99% APR / 0% for 24 months on $15k+; `reviews.average_rating` = 4.9, `.count_last_90_days` = 142; `certifications` = GAF Master Elite, Owens Corning Preferred, HAAG Certified; `team.rep_roster` → Sam Rivera.

---

### Objection-Handling Scorecard

| Objection raised | Sam's response | Matched best response? | Acknowledge | Reframe | Evidence | Close-the-loop | Coaching note |
|---|---|---|---|---|---|---|---|
| "That's more than I expected" (too expensive) | "It's actually a really fair price for the area" | ❌ No | 2 | 1 | 1 | 1 | Didn't acknowledge the concern, didn't reframe to monthly payment, cited no number at all. Should have led with GreenSky: $17,400 at 0% for 24 months is $725/mo. |
| "$14,900 from a competitor" (other guy cheaper) | "We use better materials" | ❌ No | 1 | 2 | 1 | 1 | Generic quality claim with no evidence. Should have asked for the competitor's written scope and offered a line-by-line comparison, then anchored to GAF Master Elite + 6-yr workmanship, neither of which a non-certified competitor can offer. |

### Overall Call Score

Rapport 3/5 → 15%×60 = 9.0
Discovery 2/5 → 20%×40 = 8.0
Objection handling (avg of the four per-objection sub-score averages: 1.25 and 1.25) → 35%×25 = 8.75
Close attempt: none made ("let me think about it," no ask) → 20%×10 = 2.0
Next-step clarity: no scheduled follow-up, no deadline → 10%×20 = 2.0

**Composite: 30 / 100 — Needs coaching.** Sam has rapport but is answering both objections with generic reassurance instead of the shop's actual numbers, and let the call end without a next step. Route this lead into `follow-up-sequence` immediately (Warm tier, "too expensive" + "other guy is cheaper" objections both logged) so the written cadence recovers what the live pitch didn't close.

### Next-Pitch Cheat Sheet — Sam Rivera

1. **"Too expensive"** → Lead with the number, not a reassurance: "Through GreenSky, 0% for 24 months on jobs over $15k — that's about $[amount]/month, not the sticker price." Never say "it's a fair price" without a number attached.
2. **"Other guy is cheaper"** → Ask for their written scope before responding. Then: "We're GAF Master Elite and Owens Corning Preferred — that's less than 3% of contractors nationally. The 6-year workmanship warranty backs it. Let's compare line by line instead of just the total."
3. **Reminder for Sam specifically:** Your Acknowledge and Evidence scores are both low this call — repeat the concern back in your own words before responding, and always attach a specific number (an APR, a warranty year, a review count) instead of a general assurance.
4. **Always close with a specific ask** — a date, a deposit amount, or a scheduled callback. "Let me think about it" without a next step is how this one call already needs to be recovered.

### Team Objection Log Entry

```
2026-07-25 | Sam Rivera | "too expensive" | missed | undecided | A2 R1 E1 C1
2026-07-25 | Sam Rivera | "other guy is cheaper" | missed | undecided | A1 R2 E1 C1
```

*(If 2+ prior entries for other reps show the same "too expensive" objection missing a financing anchor, flag it in the output as a team-wide training gap, not just a Sam-specific one.)*

(Run with your own call notes or transcript to replace these illustrative values.)
