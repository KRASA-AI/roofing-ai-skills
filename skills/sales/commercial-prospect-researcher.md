---
name: "Commercial Prospect Researcher"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hours/prospect list"
version: 1.5
last_eval_score: 8.9
inspiration: "v1.5 (2026-07-13) eval improvement cycle targeting efficiency — the six-item Required Input list read as an interrogation, but five of the six already resolve from config (verticals from commercial.target_verticals[], territory bounds from service_area.commercial_zip_codes[], channel/tone from voice.commercial, case studies and certifications from their arrays) and the sixth (trigger event) is precisely what Layer 3 of the research framework exists to discover — asking the rep for it inverts the skill. Added a 'Fastest path — minimum viable input' block (territory is the sole blocking input; a bare zip code is enough), [blocking]/[from config]/[inferred] annotations on every input, and an expanded Efficiency-notes block that fixes the default list depth (10 buildings), default contact depth (name-level attempted, company-level accepted), and a single-question fallback used only when config carries no verticals and no commercial zips. Strictly additive: the four-layer framework, decision-maker lookup, Est. Project Value math, priority tiers, campaign plan, and all three worked briefs are untouched. v1.4 (2026-06-22) eval improvement cycle targeting output_quality — Section 2 of the output spec requires an outreach brief for *every* 🔥 and 🟡 prospect, but the worked Example Output demonstrated only the single 🔥 #1 brief (the header literally read 'one worked brief'), leaving the second 🔥 (#3 Allen Tech, the name-level contact the campaign plan says to lead the week with) and the 🟡 (#2 Parkside) undemonstrated. This version writes both missing briefs in full and relabels the section: the 🔥 #3 brief exercises the name-level-contact + fresh-solar-permit-trigger path with the vertical-matched EPDM case study and a pre-solar-readiness angle; the 🟡 #2 brief exercises the harder contact-not-yet-identified path (explicit find-the-PM steps, ponding/aerial trigger, first-reference-project framing, and a batch-with-#4 note that ties back to the campaign plan). All three briefs now sort by the Est. Project Value/trigger logic the campaign plan describes, so the example is internally consistent end-to-end. v1.3 (2026-06-15) eval improvement cycle targeting output_quality — prior versions' frontmatter and output rules referenced 'estimated-value flagging' but the prospect-list table had no value column, so reps had no way to sort the biggest qualified jobs to the top. Added an Est. Project Value column (Est. SF × a labeled recover/tear-off $/sf assumption, always flagged estimated) to both the spec table and the 5-building worked example, with an explicit basis note; wired it into the priority sort (intra-tier ties broken by value) and the capacity check (🔥 tier now reports ~$790K combined). v1.2 (2026-06-08) eval improvement cycle targeting output_quality — the skill's primary deliverable is a prospect LIST, but prior versions only demonstrated a single building brief and left the prospect-list table as an empty header row. Added a fully populated 5-building Example Output (Plano/Allen warehouse + retail + MOB mix, with 🔥/🟡/🟢 priority tiers, estimated-value flagging, and decision-maker titles from the vertical lookup) plus a worked Campaign-Level Recommendations block (send order, geo cluster, shared-PM batching, vertical gaps, and a crew_capacity_sf_per_week saturation check). v1.1 rewritten 2026-04-25 from eval improvement cycle — named config-field binding (commercial.case_studies[].vertical/zip/system, commercial.certifications[], commercial.target_verticals[], voice.commercial), example outreach brief showing voice-tuned opening, and explicit decision-maker-by-vertical lookup table. v1.0 concept from 2026 commercial-roofing B2B AI outreach tools that aggregate facility-manager and building-owner data from public sources to seed cold outreach."
---

# 🏢 Commercial Prospect Researcher

## Purpose

Build a qualified commercial-roofing prospect list by gathering public data about target buildings (age, size, industry, ownership), identifying the actual decision-maker (facility manager, property manager, or building owner), and producing a personalized outreach brief the sales rep can turn into email, LinkedIn, or phone outreach. Commercial roofing cycles are longer than residential, so accurate targeting of the right human on the right building matters more than volume.

## When to Use

- Opening a new commercial vertical (schools, warehouses, medical office, retail strips, multifamily)
- Filling a pipeline gap between storm cycles when residential volume dips
- Preparing for a trade-show event where you want a target list of local facility managers
- Quarterly territory planning when a sales rep asks "which 50 buildings in my zip code should I be calling?"
- Post-inspection walkaround spots an aging neighboring roof and the rep wants to qualify that building

## Required Input

**Fastest path — minimum viable input:** *a territory.* A zip code is enough. Everything below resolves from `config.yml` or is discovered by the research framework itself — a rep who types "75074" should get a full 10-building list, campaign plan, and briefs back without a second exchange.

1. **Territory definition** `[blocking]` — City, county, zip codes, or drive-time radius from the shop. This is the one thing that cannot be inferred. If the rep gives a city, intersect it with `service_area.commercial_zip_codes[]` and work only the licensed-and-bonded overlap (say so if you drop zips).
2. **Building criteria** `[from config / inferred]` — Verticals default to `commercial.target_verticals[]`. Roof size defaults to the band the shop's crews actually run (bracket around `commercial.crew_capacity_sf_per_week`; if absent, 10K–100K sf). Roof age defaults to 12+ years — the point at which a low-slope membrane enters the recover conversation. Exclusions default to single-family and owner-occupied residential.
3. **Contact depth** `[inferred]` — Default: attempt name-level, accept company-level. Never ask this; a rep always prefers the name and will take the switchboard when the name isn't public.
4. **Source list** `[optional]` — If an address list, CRE export, or database dump is provided, enrich it rather than re-discover. If not, run "research from scratch" — this is the normal mode, not a fallback.
5. **Outreach channel** `[from config / inferred]` — Default to email + LinkedIn for name-level contacts and phone-to-switchboard for company-level; tone from `voice.commercial`. Only ask if the rep is working a channel the shop doesn't normally run (e.g., direct mail).
6. **Reason to call** `[inferred — do not ask]` — Trigger discovery *is* Layer 3 of the research framework. Asking the rep for a trigger inverts the skill: the rep is here because they don't know which building has one. If the rep volunteers a trigger (a storm date, a drive-by observation), weight it heavily; otherwise discover it and cite the source.

**Single-question fallback:** ask exactly one question, and only when config carries *neither* `commercial.target_verticals[]` *nor* `service_area.commercial_zip_codes[]` — "Which verticals do you have crews and case studies for?" Anything less than that, proceed on defaults and list them in the Assumptions footer.

**Default list depth:** 10 buildings unless the rep says otherwise. Fewer looks thin for a week of territory work; more dilutes the briefs.

## Instructions

You are a commercial-roofing prospect researcher supporting an outside sales rep. Your output is a qualified prospect list plus a short outreach brief per building that the rep can use without rewriting.

**Before you start:**

- Load `config.yml` — specifically these named fields:
  - `company.name`, `company.commercial_phone`, `company.commercial_email_from` — surfaces in the outreach brief signature block
  - `commercial.target_verticals[]` — the verticals the shop has crews and case studies for (e.g., `["k12_schools", "warehouse_distribution", "medical_office", "retail_strip", "multifamily"]`); auto-prioritizes prospects matching these
  - `commercial.certifications[]` — TPO/EPDM/PVC manufacturer authorizations (Carlisle CCM Authorized Applicator, GAF Master Select, JM Peak Advantage, Versico Authorized, Sika Sarnafil Roofing Contractor) — printed in differentiation lines
  - `commercial.case_studies[]` — each with `vertical`, `zip`, `system` (TPO / EPDM / PVC / mod-bit / coating), `square_footage`, `completion_year`, `client_quote_or_metric`. Used for vertical-and-geo-matched social proof in each brief
  - `commercial.preferred_systems[]` — the membrane systems the shop installs (drives the "what we'd recommend" reflex line in briefs)
  - `commercial.crew_capacity_sf_per_week` — used to flag any prospect that would saturate or stall capacity if it converted
  - `service_area.commercial_zip_codes[]` — the licensed-and-bonded commercial-work area (often narrower than residential)
  - `voice.commercial` — communication tone for commercial briefs (typically more consultative than residential `voice`); falls back to `voice` if missing
  - If a named field is missing, use a sensible default and flag it in the output's "Assumptions" footer

- Reference `knowledge-base/industry-overview.md` for commercial-vs-residential context
- Reference `knowledge-base/terminology/` for commercial-roofing specific terms (TPO, EPDM, PVC, mod-bit, built-up, coating systems) so the outreach sounds like it's from a roofer, not a generic vendor

**Decision-maker lookup by vertical (use this to populate the title column):**

| Vertical | Roof <50K sf typical buyer | Roof ≥50K sf typical buyer | Backup buyer |
|----------|----------------------------|----------------------------|--------------|
| K-12 schools | Director of Facilities / Operations | Asst. Superintendent of Operations | Business Manager |
| Warehouse / distribution | Plant or Site Facilities Manager | VP Real Estate / Director of Facilities | Regional FM |
| Medical office (MOB) | Property Manager (3rd-party) | Director of Real Estate (health system) | Building Engineer |
| Retail strip | Property Manager (3rd-party) | Asset Manager (REIT) | Lease Admin |
| Hospitality (hotel) | Chief Engineer | Director of Facilities (brand) | GM (limited authority) |
| Multifamily | Regional Property Manager | VP Operations | Maintenance Supervisor |
| Light industrial | Site / Plant Manager | Corporate Director of Facilities | EHS Manager (if penetrations) |
| Municipal / public | Public Works Director / Facilities | Capital Projects Manager | Procurement |

**Research framework — four layers per building:**

### Layer 1: Building & Roof Fundamentals
Pull publicly available data:
- Property type (light industrial, retail, office, school, etc.)
- Approximate roof age from permit records, county property records, or aerial imagery dating
- Roof system best-guess from satellite imagery (membrane vs. metal vs. built-up) — label as "estimated"
- Approximate square footage
- Structure count (multi-building campuses often warrant a campus-level conversation)

### Layer 2: Ownership & Decision Structure
- Owner of record (LLC, individual, REIT, corporate)
- Property manager if different from owner
- Decision-maker title from the vertical lookup table above
- For franchised locations, whether roof decisions sit with the franchisee or the corporate parent
- If a tenant occupies, flag that and note the typical CAM/lease-passthrough dynamic

### Layer 3: Trigger Events (reason to call now)
- Recent severe weather in the zip/county in the last 30–90 days
- Building permits filed for HVAC, solar, or skylight work (roof penetration events often justify a roof review)
- Ownership change or refinancing event
- Expiring manufacturer warranties (if the roof system and install year are known)
- Public RFPs or bid postings for capital-improvement work
- Observable condition signals from satellite imagery: ponding, seam separation visible from above, heavy patching patterns, vegetation growth

### Layer 4: Relationship Angle
- Any shared connections via LinkedIn, Chamber of Commerce, BNI, or industry associations
- Prior work the contractor has done for a similar building type in the same zip — pull from `commercial.case_studies[]` filtered by `vertical` AND `zip` (or county)
- Case study to cite by name and year (use `client_quote_or_metric` if present)
- Mutual-vendor signals (same electrician, same HVAC company) that can warm the intro

**Output structure:**

### 1. Prospect List (tabular)

| # | Building | Address | Roof Est. Age | Est. SF | Est. Project Value | Industry | Decision-Maker Title | Decision-Maker Name (if known) | Trigger | Vertical-Matched Case Study | Priority |
|---|----------|---------|---------------|---------|--------------------|----------|----------------------|---------------------------------|---------|------------------------------|----------|
| 1 | | | | | | | | | | | 🔥/🟡/🟢 |

**Est. Project Value column:** rough planning figure = Est. SF × a per-sf assumption (recover/coating ~$4–6/sf, full tear-off + new membrane ~$8–12/sf); pick recover vs replace from the roof's estimated age/condition and label the basis (e.g., `~$430K (recover)`). Always flag as estimated. Use it to (a) sort within a priority tier so reps work the largest qualified jobs first, and (b) feed the capacity check below.

Priority legend:
- 🔥 Hot — documented trigger + identified contact + in `service_area.commercial_zip_codes[]` + matches `commercial.target_verticals[]`
- 🟡 Warm — likely fit + known contact but no trigger, OR strong trigger but contact not yet identified
- 🟢 Nurture — fits criteria but no current trigger and no contact identified

### 2. Outreach Brief per 🔥 and 🟡 Prospect

For each priority prospect, produce a 150–220 word brief:
- **Building summary** in two sentences (what it is, what the roof likely is)
- **Decision-maker line** — name if known, else role from the vertical lookup table + best hypothesis on how to find them (LinkedIn search string, switchboard ask)
- **Reason-to-call** anchored to a trigger or observable signal
- **Opening line drafted in `voice.commercial`** — not generic AI prose. Use the vertical-matched case study from `commercial.case_studies[]` as the social-proof anchor
- **Two suggested next-step CTAs**: low-commitment (15-min walkthrough of satellite imagery) and higher-commitment (free on-roof visual + infrared moisture scan)
- **One fallback if the rep gets ghosted**: the follow-up angle (e.g., send a 1-page condition snapshot by mail 10 days after first touch — feeds into `follow-up-sequence` skill)

### 3. Campaign-Level Recommendations

- Suggested send order for the week
- Which buildings cluster geographically for efficient drive-through
- Which buildings share an owner/property manager (batch the outreach to that contact)
- Which industries in the list warrant vertical-specific case studies (use `commercial.case_studies[].vertical` matching) and which existing case studies apply
- Capacity check: total estimated SF in the 🔥 list vs `commercial.crew_capacity_sf_per_week` — flag if conversion would saturate

**Output requirements:**

- Flag anything inferred as "estimated" or "unconfirmed" — never present guesses as facts to a sales rep
- Never include personal contact info that was not publicly disclosed (no scraped phone numbers, no guessed personal emails)
- Use vertical-and-geo-matched case studies from `commercial.case_studies[]` by name; if none match, suggest the first commercial job in that vertical/zip be logged as a reference project
- Differentiation lines reference `commercial.certifications[]` (e.g., "as a Carlisle CCM Authorized Applicator we extend the SureWeld TPO warranty…")
- Saved to `outputs/commercial-prospects/{territory}-{YYYY-MM}.md` if the user confirms

**Efficiency notes:**

- **Runs from a zip code.** Territory is the only blocking input. Verticals, licensed zips, capacity band, certifications, case studies, and tone all come from config; triggers and decision-makers are the skill's job to discover, not the rep's job to supply.
- **One question, only when config is empty** — see the Single-question fallback above. Do not stack clarifiers; a rep between drive-bys will abandon the thread.
- **Degrade depth, don't stall.** If property-intelligence sources are thin for a territory, produce the list anyway at the best available fidelity, label every inferred figure "estimated," and say plainly which layer was thin (e.g., "ownership resolved for 7 of 10; three are in LLC shells with no public registered agent").
- Source-list mode: enrich rather than re-discover
- Cross-reference sibling skills: `lead-response-automator` (once outreach lands responses), `estimate-builder` (once a walkthrough is booked), `follow-up-sequence` (for the post-first-touch cadence), `roof-inspection-report` commercial variant (for the capital-planning deliverable after on-roof visit)

## Example Output (full run: list + campaign plan + a brief for every 🔥 and 🟡 prospect)

### 1. Prospect List — Plano/Allen TX 75074/75013, warehouse + retail verticals, 2026-06

| # | Building | Address | Roof Est. Age | Est. SF | Est. Project Value | Industry | Decision-Maker Title | DM Name (if known) | Trigger | Vertical-Matched Case Study | Priority |
|---|----------|---------|--------------:|--------:|-------------------:|----------|----------------------|--------------------|---------|------------------------------|----------|
| 1 | Riverside Logistics Center | 4400 Industrial Pkwy, Plano 75074 | ~17 (est.) | 62,000 | ~$430K (recover) | Warehouse/distribution | VP Real Estate / Dir. Facilities | — (Lincoln Property Co. PM) | 4/18 hail 1.25"; 9 nearby HVAC permits 12 mo | Frisco Logistics Hub 78k TPO 2024 | 🔥 Hot |
| 2 | Parkside Commons retail strip | 1820 Preston Rd, Plano 75093 | ~14 (est.) | 38,000 | ~$300K (recover) | Retail strip | Property Manager (3rd-party) | — | 4/18 hail footprint edge; visible ponding (aerial) | none — log as first 75093 retail ref | 🟡 Warm |
| 3 | Allen Tech Business Park (Bldg C) | 700 Central Expy, Allen 75013 | ~12 (est.) | 51,000 | ~$360K (recover) | Light industrial | Corporate Dir. of Facilities | Dana W. (LinkedIn) | Solar permit filed 2026-03 (penetration risk) | Plano flex-space EPDM 2023 | 🔥 Hot |
| 4 | Cedarbrook Medical Office | 1100 W McDermott, Allen 75013 | ~19 (est.) | 28,000 | ~$280K (tear-off) | Medical office (MOB) | Property Manager (3rd-party) | — | Roof past mfr. warranty window (est.) | none — MOB vertical gap | 🟢 Nurture |
| 5 | Greenline Distribution | 3900 Mapleshade Ln, Plano 75075 | ~9 (est.) | 84,000 | ~$420K (recover) | Warehouse/distribution | Regional FM | — | None current; large SF, monitor | Frisco Logistics Hub 78k TPO 2024 | 🟢 Nurture |

*All age/SF/value/system figures flagged estimated (assessor + aerial; value = Est. SF × $/sf assumption, recover ~$5–7/sf or tear-off ~$10/sf per estimated condition). No personal contact info included beyond publicly listed PM switchboards.*

### 2. Campaign-Level Recommendations

- **Send order this week:** #3 Allen Tech (Dana W. is name-level + fresh solar-permit trigger) → #1 Riverside (largest 🔥 at ~$430K, route through Lincoln Property switchboard) → #2 Parkside (PM hunt first). Within the 🔥 tier, ties broken by Est. Project Value so reps work the biggest qualified jobs first.
- **Geo cluster:** #3 and #4 are both Allen 75013, 0.6 mi apart — one drive-by photo run covers both.
- **Shared contact:** #2 and #4 are both 3rd-party-managed retail/MOB; identify the PM firm and batch one intro covering both buildings.
- **Vertical gaps:** no MOB (#4) or 75093 retail (#2) case study on file — pitch #4 as a reference-project candidate and flag for `commercial.case_studies[]` logging on win.
- **Capacity check:** 🔥 list = 113,000 sf / ~$790K combined est. value (#1 ~$430K + #3 ~$360K). At `commercial.crew_capacity_sf_per_week` (assume 25,000 sf), simultaneous conversion of #1+#3 would stall ~4.5 weeks — stagger walk-throughs so the larger #1 leads.

### 3. Outreach Briefs — one per 🔥 and 🟡 prospect

Per the output rules, every 🔥 and 🟡 row gets its own brief (the two 🔥 first, sorted by Est. Project Value, then the 🟡). The 🟢 Nurture rows (#4, #5) get no brief this cycle — they re-enter when a trigger appears.

#### 🔥 #1 — Riverside Logistics Center (largest 🔥, ~$430K)

```
PROSPECT — Riverside Logistics Center
Address: 4400 Industrial Pkwy, Plano TX 75074
Type:    Distribution warehouse, ~62,000 sf single-membrane roof (TPO est.)
Roof age: ~17 years (assessor permit 2008; aerial confirms membrane install pattern)
Owner:   Cornerstone REIT (publicly traded; FM decision routed to property manager)
PM:      Lincoln Property Co. — Plano office (940-555-0119 switchboard)
Decision-maker (per vertical lookup): VP Real Estate / Director of Facilities
  (>50k sf bracket); backup is Regional FM
Trigger: 2026-04-18 hail event with 1.25" peak in 75074 (NOAA event 20260418-DFW-117);
         9 distribution buildings within 1 mile permitted HVAC work in last 12 months
         (penetration risk on a 17-yr TPO with seam exposure)
Vertical-matched case study (from commercial.case_studies[]):
  "Frisco Logistics Hub, 78k sf TPO restoration, 2024 — recovered $0.42/sf vs full
  replacement; client extended Carlisle SureWeld warranty 10 years."

OPENING LINE (voice.commercial = consultative):
  "Quick note from {company.name} — we just wrapped a 78k-sf TPO restoration on a
  distribution roof in Frisco that was about the same age as Riverside Logistics
  Center. The 4/18 hail and the recent HVAC permits in the area are usually when
  the seam separations start showing up. If you'd want a 15-minute walk-through
  of satellite imagery before we'd ever talk about a quote, I can send a
  condition snapshot this week."

CTAs:
  Low:  15-min satellite walkthrough — no roof access, just imagery
  High: free 60-min infrared moisture scan + drone seam survey

GHOSTING FALLBACK (10 days):
  Mail 1-page condition snapshot with redacted Frisco case study; route into
  follow-up-sequence Warm cadence.

DIFFERENTIATION LINE (from commercial.certifications):
  As a Carlisle CCM Authorized Applicator we can extend SureWeld warranty terms
  on a TPO restoration — most regional contractors can't.

— {company.name} | {company.commercial_phone} | {company.commercial_email_from}
```

#### 🔥 #3 — Allen Tech Business Park, Bldg C (name-level contact + fresh trigger → lead the week)

```
PROSPECT — Allen Tech Business Park (Bldg C)
Address: 700 Central Expy, Allen TX 75013
Type:    Light-industrial flex building, ~51,000 sf low-slope roof (EPDM est.)
Roof age: ~12 years (assessor permit 2014; aerial shows ballasted-EPDM field)
Owner:   Allen Tech Park Holdings LLC (self-managed; facilities run corporately)
Decision-maker: Dana W. — Corporate Director of Facilities (name-level via LinkedIn;
  vertical lookup confirms the ≥50k-sf light-industrial buyer is Corporate Dir. of
  Facilities, backup EHS Manager given the new penetration)
Trigger: Solar permit filed 2026-03 — a rooftop PV install drives dozens of new
  penetrations and attachment points into a 12-yr EPDM field; the seam and flashing
  review window is *now*, before panels go down and cover the membrane
Vertical-matched case study (from commercial.case_studies[]):
  "Plano flex-space EPDM, 2023 — re-flashed and coated ahead of a tenant solar
  install; zero leak callbacks through two summers."

OPENING LINE (voice.commercial = consultative):
  "Dana — quick note from {company.name}. We saw Bldg C pulled a rooftop-solar
  permit this spring. On a 12-year EPDM field, the moment to settle seams and
  flashing details is before the array goes down and you can't get to the membrane
  under it. We did exactly this on a Plano flex building in 2023 — re-flashed and
  coated ahead of the tenant's panels, no callbacks since. Worth a 15-minute look
  at the imagery before your installer mobilizes?"

CTAs:
  Low:  15-min satellite + permit-overlay walkthrough — no roof access
  High: free pre-solar roof + flashing condition survey (infrared moisture scan
        on the EPDM field before panel layout is finalized)

GHOSTING FALLBACK (10 days):
  Send a 1-page "pre-solar roof readiness" checklist referencing the Plano EPDM
  project; route into follow-up-sequence Warm cadence. Re-time the next touch to
  the installer's likely mobilization date.

DIFFERENTIATION LINE (from commercial.certifications):
  As a Carlisle CCM Authorized Applicator we can coordinate the membrane warranty
  around a third-party PV attachment — most contractors leave that gap unaddressed.

— {company.name} | {company.commercial_phone} | {company.commercial_email_from}
```

#### 🟡 #2 — Parkside Commons retail strip (strong trigger, contact not yet identified)

```
PROSPECT — Parkside Commons retail strip
Address: 1820 Preston Rd, Plano TX 75093
Type:    Multi-tenant retail strip, ~38,000 sf low-slope roof (TPO est.)
Roof age: ~14 years (assessor + aerial; past mid-life for mechanically-attached TPO)
Owner:   Held in a 75093 retail LLC; 3rd-party property-managed (PM not yet named)
Decision-maker (per vertical lookup): Property Manager (3rd-party) for <50k sf retail;
  backup Lease Admin. *Contact hunt is the gating step — this is 🟡, not 🔥, only
  because the human isn't identified yet.* Find-the-PM path: tenant signage / the
  leasing sign on the pad → CRE listing broker → county "care-of" mailing address.
Trigger: On the edge of the 4/18 hail footprint AND visible ponding at two interior
  drains in current aerial imagery — a documented standing-water pattern an asset
  manager can't unsee once it's shown to them.
Vertical-matched case study: none on file for 75093 retail — pitch this as the
  shop's first 75093 retail reference project and flag for commercial.case_studies[]
  logging on win (also note for the #4 MOB vertical gap).

OPENING LINE (voice.commercial = consultative, PM-agnostic until named):
  "Hi — I'm trying to reach whoever manages the roof at Parkside Commons on Preston.
  We're a {company.name} commercial crew; current aerial imagery shows standing
  water at two of the interior drains, which on a 14-year TPO usually means the
  fasteners are starting to back out under the membrane. No charge to put a drone
  over it and send you a one-page condition snapshot — then you decide if it's
  worth a closer look."

CTAs:
  Low:  emailed 1-page drone + ponding-overlay snapshot (no roof access, no cost)
  High: free on-roof drain + seam-fastener survey with infrared moisture scan

GHOSTING FALLBACK (10 days):
  Mail the snapshot to the county "care-of" address with a note inviting the PM to
  call; route into follow-up-sequence Cold→Warm cadence once a human responds.
  Batch with #4 Cedarbrook MOB if the same PM firm manages both (see campaign plan).

DIFFERENTIATION LINE (from commercial.certifications):
  As a Carlisle CCM Authorized Applicator a TPO recover here can carry a renewed
  manufacturer warranty rather than a contractor-only patch.

— {company.name} | {company.commercial_phone} | {company.commercial_email_from}
```

(Run with your own territory + config to replace these illustrative values.)
