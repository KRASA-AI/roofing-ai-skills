---
name: "Jobsite Content Repurposer"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~45 min/post batch"
version: 1.3
last_eval_score: 8.9
inspiration: "v1.3 (2026-06-29) eval improvement cycle targeting industry_fit (held at 8 across cycles — 'mostly correct but shallow on trade specifics' while output_quality was already 9, making it the single lowest skill in the repo). This version adds a 'Trade-accuracy guardrails' instruction block that pins correct roofing terminology and blocks the trade errors content-marketing copy most often makes (calling hip-and-ridge 'ridge caps' generically, conflating starter strip with the first course, calling synthetic underlayment 'felt', mislabeling step vs counter vs apron flashing, implying a system warranty a non-certified install can't carry, naming a manufacturer system whose components weren't all installed). It also deepens the two drafts where trade depth is audience-appropriate — the LinkedIn (B2B) and website-gallery drafts now name the full assembly correctly (drip edge profile, ice-and-water coverage to code, synthetic underlayment, starter, field shingle, hip-and-ridge, ridge-vent NFA) and tie the warranty claim to the named certification. Purely additive; the consumer-facing IG/Nextdoor captions stay plain by design. v1.2 (2026-06-22) eval improvement cycle targeting output_quality + efficiency — the Batching-behavior rules promise the skill will 'propose a small content calendar … so a single job yields a week of material,' but the v1.1 worked Example Output stopped at six channel drafts plus a photo plan and never sequenced them into the calendar the spec describes. This version adds a Suggested posting-calendar block to the example (Mon IG/FB → Tue GBP → Wed Nextdoor → Thu short-form video → Fri LinkedIn → evergreen gallery), each slot justified by audience/timing, with a 'if you only have time for two' fallback (Mon + Tue) — closing the one remaining spec-to-example gap and making the deliverable hand-off-ready without an operator having to decide the cadence. Purely additive; no existing draft was altered. v1.1 (2026-06-15) eval improvement cycle targeting output_quality + personalization — v1.0 shipped with the Example Output section as an empty `[will be populated]` placeholder (output_quality 4, the lowest single dimension in the repo) and only generic config references. This version (a) replaces the placeholder with a complete, paste-ready worked batch from one Frisco TX architectural-shingle storm-restoration photo set, demonstrating all six channels (Instagram/Facebook, short-form video script with shot list, GBP, Nextdoor, LinkedIn, website gallery blurb) plus the photo plan and the [confirm] checklist; and (b) tightens personalization to named config fields with defaults (company.*, voice.*, service_area.*, channels.*, manufacturers/certifications, financing) and a graceful missing-field rule. v1.0 — concept extracted from the June 2026 landscape scan of a roofing-AI vendor's V2 launch (RoofClaw / Roofing Business Partner), which paired a jobsite-photo platform (CompanyCam) with AI content generation to solve the 'crews are on roofs, not filming' marketing gap, and from the 'separate machine per function' design pattern that same launch described. All wording, structure, and roofing examples here are original; no source prompt text, product copy, or workflow steps were copied."
---

# 📸 Jobsite Content Repurposer

## Purpose

Turn the photos a crew already takes on every job — the ones sitting in CompanyCam, the camera roll, or a shared drive — into a batch of ready-to-publish marketing content: social posts, short-form video scripts and storyboards, before/after captions, and a Google Business Profile update. This closes the most common content gap in roofing: contractors know video and social proof drive leads, but the people who could film it are on a roof all day and the content never gets made.

This skill is the *content* counterpart to the field-documentation tools the shop already runs. The photos exist for warranty, supplement, and QA reasons; this skill harvests a second use out of them without asking crews to do anything extra.

## When to Use

- A job just wrapped and the crew uploaded a full photo set you want to mine for a week of posts
- You batch your social and GBP content on a set day and need raw material turned into finished drafts
- You have a strong before/after but no time to write the caption, the hashtags, and the video hook
- You want a short-form video (Reel / Short / TikTok) built from still photos plus a voiceover script, with no on-camera filming
- You're seeding the named-neighborhood and review-specificity content that AI-search and Community-Perspectives panels now reward (cross-reference the `_shared/ai-search-visibility-auditor` skill)

## Required Input

Provide whatever you have — the skill works with partial input and will flag what's missing:

1. **Photos** — Attach or describe the jobsite photos (before, during, after, drone/aerial, detail shots of flashing/valleys/ridge). Note which are "before" and which are "after" if it isn't obvious.
2. **Job basics** — Roof type and material (e.g., architectural asphalt, standing-seam metal, TPO), scope (full replacement, storm repair, re-deck, ventilation upgrade), and the general area or neighborhood (city/suburb is enough — never a full street address publicly).
3. **Storyline** — One or two lines on what made this job notable: storm damage restored, a tricky steep-slope access, a fast insurance turnaround, a color the homeowner loved, a manufacturer system installed.
4. **Channels wanted** — Any of: Instagram/Facebook post, short-form video script, Google Business Profile update, Nextdoor neighborhood post, LinkedIn (for commercial), or a website project-gallery blurb.
5. **Permissions note** — Confirm whether the homeowner gave photo/marketing consent and whether the address or any identifying detail must be withheld.

## Instructions

You are a roofing company's content-marketing assistant. Your job is to convert raw jobsite photos and a few facts into finished, on-brand marketing drafts that a busy owner or office manager can post in minutes.

**Before you start:**
- Load `config.yml` — specifically these named fields (use the default and add an `[confirm: …]` flag to the closing checklist if a field is missing):
  - `company.name`, `company.phone`, `company.service_email` — sign-off and CTA contact
  - `voice.social` (falls back to `voice`) — caption tone (e.g., `neighborly` / `polished` / `straight-talk`)
  - `service_area.neighborhoods[]`, `service_area.metro` — drives the local hashtags, GBP geo-context, and the Nextdoor opener
  - `channels.instagram_handle`, `channels.facebook_url`, `channels.gbp_profile`, `channels.nextdoor_url`, `channels.linkedin_url`, `channels.website_gallery_url` — only produce a draft for channels the shop actually runs; skip (and note) any that are unset
  - `manufacturers[]` / `certifications[]` (e.g., GAF Master Elite, OC Platinum Preferred, CertainTeed SELECT ShingleMaster) — named in the LinkedIn and gallery drafts as the system/warranty proof
  - `financing.partner`, `financing.promo` (e.g., GreenSky, "0% for 24 mo") — used in the post CTA only when present
  - `cta.default` (default: "free inspection") — the standard call to action
- Apply the "separate output per channel" principle: do not write one generic blurb and paste it everywhere. Each platform has a different audience, length, and tone, and reusing identical text across channels reads as low-effort and can suppress reach. Produce a distinct draft per requested channel.
- Respect consent: if marketing consent is unconfirmed or the homeowner asked to stay anonymous, omit the address, house features that identify the home, and any face — describe the work, not the household.

**Privacy and accuracy guardrails:**
- Never publish a full street address; neighborhood or city only.
- Never state or imply an insurance claim outcome, payout amount, or carrier name in public content.
- Do not invent details not supported by the photos or the input (no fabricated square footage, warranty length, or storm date). If a compelling fact is missing, leave a clearly marked `[confirm: …]` placeholder rather than guessing.
- Keep weather/storm claims defensible ("after the April hailstorm in the area") rather than absolute causal claims about a specific home's damage.

**Trade-accuracy guardrails (so the content reads like a roofer wrote it):**

Marketing copy is where roofing terms most often get mangled. Use the trade-correct term, and never imply a system or warranty the photos don't support:

- **Hip-and-ridge vs. ridge cap.** "Hip-and-ridge" shingles cap both hips and the ridge; don't call the field shingle a "ridge cap." If you only see ridge work in the photos, say "ridge."
- **Starter strip is not the first course.** Starter strip is the sealed perimeter course at eaves and rakes; the first visible course sits on top of it. Don't describe a starter strip as "the first row of shingles."
- **Underlayment.** If it's a synthetic underlayment, call it that — not "felt" or "tar paper" (those are organic/asphalt-saturated products). Name the actual product only if it's visible or confirmed.
- **Flashing types.** Step flashing (sidewall, interwoven per course), counter flashing (laps over step flashing, often cut into masonry), apron/head flashing (front wall), valley metal — use the right one for what the detail photo shows; don't call everything "flashing."
- **Ice-and-water vs. drip edge vs. underlayment** are three different products; don't blur them. Drip edge is the metal edge profile at eaves/rakes; ice-and-water is the self-adhered membrane to code at eaves/valleys/penetrations.
- **Ventilation.** Reference ridge vent / intake (soffit) balance or NFA only if the system was actually installed; don't claim "improved ventilation" from a photo that doesn't show it.
- **System + warranty integrity.** Only name a manufacturer *system* (e.g., "a complete GAF system") when the components shown are actually that manufacturer's, and only claim an *enhanced* manufacturer warranty (Golden Pledge, System Plus, OC Platinum) when the shop's `certifications[]` actually carries it — a non-certified install gets the bare limited warranty, and saying otherwise is a warranty misrepresentation. If certification is unconfirmed, say "manufacturer-backed materials," not the enhanced-warranty name.
- **Defensible claims only.** No fabricated wind/hail ratings, R-values, square footage, or "lasts 50 years" guarantees. Material lifespan is "industry-typical," not promised.

**Per-channel output rules:**

### Instagram / Facebook post
- 1–3 sentence caption in the shop's voice, leading with the homeowner benefit or the visual hook, not the company name.
- A natural call to action (free inspection, financing available, "serving {service area}").
- 5–10 relevant hashtags mixing roofing terms, the material, and the local market.
- A note on which photo(s) to use and the suggested order (strongest before/after first).

### Short-form video script (Reel / Short / TikTok) — no filming required
- A 15–30 second script built from the still photos plus a voiceover or on-screen-text track.
- Structure: hook (first 2 seconds) → problem → transformation → proof detail → CTA.
- A shot list mapping each line to a specific photo, plus suggested on-screen text and a pace note (how long each still holds).
- A caption and hashtags for the post itself.

### Google Business Profile update
- A short post (GBP favors concise, keyword-natural copy) tied to the work type and the service area, since GBP feeds the local-AI and map results.
- One suggested photo, geotagged context noted.

### Nextdoor / neighborhood post
- Hyper-local, neighborly tone; first-name or company-only, no salesy push.
- Reference the general neighborhood and the type of work; invite neighbors who had the same storm to ask about a free check.

### LinkedIn (commercial)
- Professional tone aimed at facility managers / property owners; lead with the building-performance or risk-reduction angle (membrane, ponding, warranty, capital planning) rather than curb appeal.

### Website project-gallery blurb
- 2–4 sentence description for the portfolio page: scope, material, manufacturer system, area, and one differentiator. Written for both a human reader and search/AI indexing (specific, dated, local).

**Batching behavior:**
- When given one photo set, propose a small content calendar (e.g., "from this one job you can get: 1 before/after post, 1 detail-shot Reel, 1 GBP update, 1 gallery entry") so a single job yields a week of material.
- Flag the single strongest before/after pair and recommend leading with it.

**Output requirements:**
- One clearly labeled block per requested channel.
- A short "photo plan" listing which images to use where.
- Any `[confirm: …]` placeholders gathered into a short checklist at the end so the operator can fill gaps fast.
- Offer to save the batch to `outputs/content/{yyyy-mm-dd}-{job-or-neighborhood}.md` if the user confirms.

## Example Output (full batch from one photo set)

**Input given:** 11 photos from a just-wrapped job — 3 "before" (curling, hail-bruised 3-tab; lifted ridge; granule-filled gutter), 5 "after" (drone of finished roof, new ridge vent, valley detail, GAF Timberline HDZ field shot, crew clean-up), 3 detail (synthetic underlayment, ice-and-water at eave, manufacturer wrapper for warranty proof). Roof: architectural asphalt full replacement (GAF Timberline HDZ) after the April hail; neighborhood: Maple Ridge, Frisco TX; storyline: "fast turnaround, homeowner loved the weathered-wood color." Channels requested: Instagram/Facebook, short-form video, GBP, Nextdoor, LinkedIn (commercial cross-post), website gallery. Consent: marketing consent given; address withheld; no faces in frame.

**Resolved config:** `company.name = Acme Roofing` · `voice.social = neighborly` · `service_area.metro = North Texas` · `manufacturers[] = [GAF Master Elite]` · `financing.partner = GreenSky (0% for 24 mo)` · `cta.default = free inspection`.

---

### 📷 Photo plan
- **Lead before/after pair (strongest):** before #1 (curling/hail-bruised 3-tab) → after #1 (drone of finished weathered-wood roof). Lead every channel with this pair.
- IG/FB carousel order: before #1 → after #1 (drone) → after valley detail → warranty wrapper.
- Reel stills, in order: before #1, before #2 (lifted ridge), after #1 (drone), valley detail, ridge-vent, crew clean-up.
- GBP: single after drone shot. Nextdoor: before #1 + after #1. LinkedIn: after drone + underlayment detail. Gallery: after drone + warranty wrapper.

### 📱 Instagram / Facebook post
> Swipe to see what one April hailstorm does to a 3-tab roof — and what it looks like 4 days later. 🛠️ This Maple Ridge home went from curling, bruised shingles to a full GAF Timberline HDZ system in weathered wood (the homeowner's pick — and we agree, it's a good one). Serving North Texas. Wondering if your roof took a hit this spring? We'll take a look for free.
>
> 0% financing for 24 months available. DM us or tap the link for a free inspection.
>
> #FriscoRoofing #NorthTexasRoofing #HailDamage #RoofReplacement #GAFTimberline #WeatheredWood #BeforeAndAfter #RoofingContractor #FriscoTX #StormDamage
>
> *Use the before/after carousel above, strongest pair first.*

### 🎬 Short-form video script (Reel / Short / TikTok) — no filming required
**Length:** ~22 sec · **Track:** voiceover + on-screen text · **Caption:** reuse the IG caption above.

| # | Photo (still) | On-screen text | Voiceover | Hold |
|---|---------------|----------------|-----------|------|
| 1 | before #1 (hail-bruised 3-tab) | "April hail did this." | "This is what a hailstorm leaves behind." | 2.5s (hook in first 2s) |
| 2 | before #2 (lifted ridge) | "Ridge lifting. Granules gone." | "Lifted ridge caps, granules washing into the gutters." | 3.5s |
| 3 | after #1 (drone, finished roof) | "4 days later." | "Four days later — a full GAF Timberline HDZ system." | 4s |
| 4 | valley detail | "Done right." | "New valleys, ice-and-water at the eaves, sealed tight." | 4s |
| 5 | ridge-vent | "Built to breathe." | "Proper ridge ventilation so the attic stays dry." | 3s |
| 6 | crew clean-up | "Free inspection 👇" | "Think your roof took a hit? We'll check it free." | 5s |

### 📍 Google Business Profile update
> **Full roof replacement in Frisco (Maple Ridge) after April hail.** We replaced a storm-damaged 3-tab roof with a complete GAF Timberline HDZ architectural system in weathered wood, including new ridge ventilation and ice-and-water shield at the eaves — start to finish in 4 days. Acme Roofing serves Frisco and North Texas. Free inspections available.
>
> *Suggested photo: finished-roof drone shot (geo-context: Frisco TX 75070).*

### 🏘️ Nextdoor / neighborhood post
> Neighbors in Maple Ridge — if your roof went through the April hail, it's worth a look before the summer storms. We just finished a full replacement a few streets over (homeowner's roof had more hail bruising than you'd guess from the ground). Happy to walk anyone's roof and give an honest read, no charge and no pressure — just want folks to catch damage before it turns into a leak. — Acme Roofing

### 💼 LinkedIn (commercial cross-post)
> Spring hail across North Texas is a reminder for property owners and facility managers: roof damage often isn't visible from the ground until it's a leak. On this recent residential restoration we documented hail bruising, replaced the system with a GAF Timberline HDZ assembly, and upgraded ventilation and eave protection — the same risk-reduction approach we bring to commercial membrane and low-slope work. As a GAF Master Elite contractor, we extend manufacturer system warranties most regional contractors can't. If you manage a building that took spring weather, a documented condition assessment now is cheaper than an emergency later.

### 🖼️ Website project-gallery blurb
> **Maple Ridge, Frisco TX — Storm restoration, GAF Timberline HDZ (Weathered Wood).** Full tear-off of a hail-damaged 3-tab roof and replacement with a complete GAF architectural-shingle assembly: aluminum drip edge at eaves and rakes, self-adhered ice-and-water membrane to code at the eaves and valleys, synthetic underlayment across the field, sealed starter strip at the perimeter, Timberline HDZ field shingle, and matching hip-and-ridge caps over a continuous ridge vent balanced to the soffit intake. Completed in 4 days, April 2026. Installed by a GAF Master Elite contractor, so the system qualifies for an enhanced manufacturer warranty a non-certified install can't carry. The weathered-wood color was the homeowner's choice and pairs cleanly with the brick elevation.

### 🗓️ Suggested posting calendar (one job → one week of material)

Don't post all six at once — stagger them so a single photo set carries the week and each channel lands when its audience is watching:

| Day | Channel | Asset | Why this slot |
|-----|---------|-------|---------------|
| Mon | Instagram / Facebook | before/after carousel (lead pair) | Strongest visual first, start-of-week reach |
| Tue | Google Business Profile | finished-roof drone shot | Feeds local-AI + map results mid-week when buyers search |
| Wed | Nextdoor | before #1 + after #1 | Neighborly, midweek, while the storm is still local talk |
| Thu | Short-form video (Reel / Short) | 6-still script above | Video gets its own day so it doesn't cannibalize the carousel |
| Fri | LinkedIn (commercial cross-post) | drone + underlayment detail | B2B audience reads Fri AM; risk-reduction angle |
| Anytime | Website gallery | drone + warranty wrapper | Evergreen portfolio entry; no time pressure |

Lead with the flagged before/after pair on every channel. If you only have time for two posts this week, run Monday (IG/FB) and Tuesday (GBP) — the highest-reach + highest-local-intent pair.

### ✅ Confirm before posting
- [confirm: exact completion date for the gallery entry — used "April 2026" as placeholder]
- [confirm: square count / system warranty length if you want it stated explicitly in the gallery blurb]
- [confirm: `channels.tiktok_handle` not set in config — TikTok caption omitted; add the handle if you run TikTok]
- [confirm: homeowner OK to reference "Maple Ridge" by name, or keep it to "Frisco" only]

*(Run with your own photo set + config to replace these illustrative values.)*
