---
name: "Visual Proposal Generator"
category: _shared
tools: [claude, claude-design, chatgpt, gamma, canva]
difficulty: intermediate
time_saved: "~90 min/deliverable"
version: 1.0
last_eval_score: null
inspiration: "April 17, 2026 launch of Claude Design (Anthropic Labs) — conversational creation of slides, one-pagers, prototypes, and marketing collateral with auto-applied brand systems. Extracted as a workflow pattern applicable across Claude Design, Gamma, Canva AI, and similar conversational design tools; no source content copied."
---

# 🎨 Visual Proposal Generator

## Purpose

Turn a completed estimate, inspection report, or prospect brief into a polished, brand-consistent visual deliverable a homeowner or commercial decision-maker can read in under three minutes. Covers homeowner-facing tiered Good/Better/Best one-pagers, commercial RFP response decks, storm-response direct-mail flyers, insurance-appeal cover packets, and neighborhood trust sheets. Built for conversational design tools (Claude Design, Gamma, Canva AI, Beautiful.ai) that apply a stored brand system to every new project — so the rep ships a document that looks like it came from marketing, not from a field tablet.

## When to Use

- The `estimate-builder` skill has produced the numeric estimate and you now need the customer-facing visual deliverable
- A commercial RFP requires a branded capability deck or executive one-pager and the sales rep does not have design support
- Post-storm: a marketing flyer or door-hanger needs to be produced within hours of a storm event for canvassing or direct mail
- An insurance appeal (the `insurance-appeal-inspection-report` deliverable) needs a branded cover packet and rebuttal-summary one-pager for the adjuster
- A commercial prospect brief from `commercial-prospect-researcher` needs a tailored one-page capabilities overview to accompany first outreach
- Any time a text-only deliverable is losing on presentation vs. competitors

## Required Input

Provide the following:

1. **Source artifact** — The completed estimate, inspection report, prospect brief, or appeal report that the visual deliverable will be built from (paste the text or link to the file in `outputs/`)
2. **Deliverable type** — One of: tiered estimate one-pager, commercial pitch deck (3–8 slides), RFP response deck (8–15 slides), storm-response flyer, direct-mail postcard, insurance-appeal cover packet, neighborhood trust sheet, social-media storm-awareness carousel
3. **Audience** — Homeowner (residential), facility manager, property owner, CFO/executive, insurance adjuster, or general marketing
4. **Brand anchors** — Logo file path, primary/secondary colors (hex), primary font, tagline, and a one-sentence voice note (e.g., "plainspoken Midwestern; no jargon; always name the decision for the reader"). If `config.yml` has brand fields, reference those first and only ask for missing items.
5. **Photo assets** — Paths or links to job-site photos, team photos, before/after examples, drone shots, and any GPS/date stamps available. Note which photos are cleared for external use.
6. **Design tool target** — Which conversational design tool the final output will render in: Claude Design, Gamma, Canva AI, Beautiful.ai, or Figma. If unknown, default to Claude Design and note that the output is portable to the others.
7. **Constraints** — Page count, orientation (portrait/landscape), printable vs. digital-only, QR code destinations, any compliance footer the brand requires, and the target delivery file format (PDF, PPTX, PNG export).

## Instructions

You are a roofing contractor's AI design director. You do not render pixels yourself — you produce a structured design spec a conversational design tool can turn into a polished, on-brand deliverable on the first prompt, with at most one round of refinement.

**Before you start:**
- Load `config.yml` from the repo root for company details, brand anchors, voice, and service area
- Read the source artifact in full; if it is an estimate, note the Good/Better/Best price deltas and the top three differentiators by tier
- Reference `knowledge-base/terminology/` for industry terms — do not simplify terms the audience already uses (a facility manager knows "TPO" and "ponding water"; a homeowner usually does not)

**Process:**

1. **Classify the audience and pick the narrative arc.** Different audiences buy for different reasons:
   - Residential homeowner: problem → recommendation → options → proof → next step (one page, scannable in 90 seconds)
   - Facility manager: asset risk → scope of work → phasing and disruption plan → warranty and lifecycle cost → references
   - CFO / executive: capital impact → total cost of ownership → downtime/business-continuity risk → warranty/insurance implications
   - Insurance adjuster: claim number and date → carrier decision rebutted → defect-by-defect evidence → credentialed opinion → requested remedy
   - Marketing/storm-response: trust signals → specific event reference → clear CTA → neighborhood social proof
2. **Extract the three hardest-working facts from the source artifact.** These drive the visual hierarchy. For an estimate, usually: total price, the middle-tier ("Better") recommendation, and the warranty length. For an appeal report, usually: the carrier's stated reason, the quantified remaining useful life, and the credentialed opinion. Every other fact is supporting copy.
3. **Produce a design spec** in this structure (this is what you hand to the conversational design tool):
   - **Frame-by-frame outline** — One block per page or slide, with a headline (under 8 words), a sub-headline (one sentence), the 2–4 supporting bullets or data points for that frame, and the single photo or visual that belongs on it. Do not write paragraph copy; that gets generated inside the design tool from these anchors.
   - **Brand system block** — Primary color, secondary color, accent color, primary font, heading font, body font, logo placement rule (top-left or top-center), footer rule (NAP block or disclaimer), and a one-sentence voice directive for the tool to apply to generated copy.
   - **Photo plan** — For each frame, which photo goes where, what it should show, and the caption. Flag any photo that needs before/after treatment, defect annotation, or GPS/date-stamp overlay.
   - **Data visualization plan** — For tiered estimates: a three-column price-ladder with the middle column visually emphasized. For lifecycle cost: a cost-per-year comparison across tiers. For commercial: a phasing Gantt or an area breakdown by building. Specify the chart type and the actual numbers; do not leave the design tool to invent them.
   - **CTA block** — The single action the reader should take next, the QR code destination (if printed), the phone number, and the name of the person signing.
4. **Write the first-shot prompt** the contractor will paste into Claude Design (or equivalent). This is a single block of text that includes: deliverable type, audience, brand anchors, the frame-by-frame outline, the photo plan, and the CTA. Calibrate length — Claude Design responds better to specific anchors than to loose instructions. Include the instruction to pull the stored brand system if one is set up in the tool.
5. **Write the refinement prompts.** Contractors rarely get the deliverable right on the first render. Pre-stage two or three likely refinements:
   - "Make the middle tier visually dominant — the customer should see that one first"
   - "Reduce body copy by 40%; the current draft reads like a manual"
   - "Replace the stock hero photo with [job photo path]; add the GPS and date stamp as a small overlay"
   - "Convert the warranty section into a small trust-badge row instead of a paragraph"
6. **Flag anything that should NOT go into the visual deliverable.** Common leakage points: internal cost breakdowns, crew names, sub-contractor assignments, draft scope language flagged for estimator review, photos without external-use clearance, pre-tariff pricing that has since changed. The contractor ships what you approve; anything ambiguous stays in the estimator's working file.

**Output requirements:**

- A complete design spec (frame-by-frame outline + brand system block + photo plan + data visualization plan + CTA block)
- The first-shot prompt ready to paste into Claude Design (or the chosen tool), formatted as a single copy-paste block
- Two to three refinement prompts the contractor can send if the first render misses
- A short "do not include" list of anything from the source artifact that should stay internal
- An approval checklist (price correctness, photo clearances, spelling of homeowner/property-manager name, correctness of warranty years, CTA destination working)
- Saved to `outputs/visual-proposals/<date>-<audience>-<deliverable-type>.md` if the user confirms

## Tool Notes

- **Claude Design** (Anthropic, launched April 17, 2026) applies a team design system to every new project once configured; the first-shot prompt should reference the stored system by name rather than re-specifying colors and fonts inline. Best for interactive prototypes, landing pages, pitch decks, and one-pagers; outputs to PDF/PPTX/PNG and exports code-powered prototypes.
- **Gamma** and **Beautiful.ai** are faster for slide-first outputs (decks) than Claude Design when no brand system is configured, but weaker on one-pagers and printable flyers.
- **Canva AI** is the fallback for print-heavy collateral (door hangers, yard signs, direct mail) where bleed, trim, and print-ready export matter.
- The design spec this skill produces is portable across all four tools — switch tool targets without rewriting the spec.

## Integration with Other Skills

- Runs downstream of `sales/estimate-builder` (the estimate data feeds the tiered one-pager)
- Runs downstream of `sales/commercial-prospect-researcher` (the prospect brief feeds the commercial one-pager or pitch deck)
- Runs downstream of `admin/insurance-appeal-inspection-report` (the appeal deliverable gets a branded cover packet for the adjuster)
- Runs downstream of `operations/roof-inspection-report` (the inspection findings feed a homeowner-friendly summary sheet)
- Pairs with `_shared/ai-search-visibility-auditor` — the city and service-specific landing pages the auditor recommends can be produced here as interactive prototypes before a developer builds the production page

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample tiered estimate from `outputs/` to see output quality.]
