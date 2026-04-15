---
name: "Safety Toolbox Talk Generator"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/talk"
version: 1.0
last_eval_score: null
inspiration: "2026 OSHA toolbox talk AI generators — structured 5-minute pre-start briefings tailored to roofing-specific hazards (falls, heat, steep-slope work) with attendance logging for compliance evidence"
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
- Load `config.yml` from the repo root for company name, crew lead roster, and any company-specific safety standards
- Reference `knowledge-base/regulations/` for current OSHA citations (29 CFR 1926 Subpart M fall protection, Subpart X ladders/stairways, Subpart L scaffolds, Subpart E PPE)
- Reference `knowledge-base/terminology/` for consistent hazard language across talks
- Pick the topic intelligently if not specified — prioritize weather-driven hazards (heat > 85°F or wind > 20 mph escalates the day), then site-specific hazards, then rotational baseline topics (falls weekly minimum)

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
- Nearest hospital name and address
- Emergency contact number (911 + company safety lead direct line)
- Muster point on this site in case of evacuation

**Special-case expansions:**
- **Fall protection talks** must specify anchor points identified for today, harness inspection outcome, and the written rescue plan reference (29 CFR 1926.502 requires a rescue plan, not just a harness)
- **Heat illness talks** (temp > 85°F, heat index > 90°F) must specify hydration schedule (8 oz every 15 min), shade break cadence, and buddy-check routine
- **Ladder talks** must specify the 4-to-1 setup rule, extension above roofline (minimum 3 feet), tie-off at the top, and inspection before each use
- **Electrical talks** must identify any power lines within 10 feet and the assigned spotter
- **New-hire talks** include a short walk-through of the site identifying each hazard from the crew's perspective

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

> [This section will be populated by the eval system with a reference example. Run with sample input to see output quality.]
