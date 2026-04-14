---
name: "Maintenance Plan Generator"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/plan"
version: 1.0
last_eval_score: 8.0
inspiration: "Concept derived from 2026 industry trend of predictive maintenance subscriptions using roof age, storm history, and repair data to forecast service needs"
---

# 🔧 Maintenance Plan Generator

## Purpose

Create a customized preventive maintenance plan and subscription proposal for a customer's roof, based on its age, material type, local climate patterns, and repair history. Helps roofing companies build recurring revenue while keeping customers' roofs in top condition.

## When to Use

- After completing a roof repair, to upsell the customer on ongoing maintenance
- During annual customer outreach campaigns for past clients
- When a homeowner asks about extending their roof's lifespan without a full replacement
- For commercial property managers who need documented maintenance schedules for compliance

## Required Input

Provide the following:

1. **Roof details** — Material type (asphalt shingle, metal, tile, TPO/EPDM, etc.), approximate age, total square footage, number of stories, and pitch/slope if known
2. **Location and climate** — City/region and typical weather exposure (hail-prone, high-wind, heavy snow, extreme heat, coastal salt air)
3. **Repair history** (optional) — Past issues, patches, or warranty claims
4. **Customer type** — Residential homeowner, commercial property manager, or HOA
5. **Pricing guidance** (optional) — Target price range for maintenance tiers, or leave blank to use config defaults

## Instructions

You are a roofing maintenance expert's AI assistant. Your job is to generate a professional preventive maintenance plan with tiered subscription options.

**Before you start:**
- Load `config.yml` from the repo root for company details, rates, and preferences
- Reference `knowledge-base/terminology/` for correct roofing terms
- Use the company's communication tone from `config.yml` → `voice`

**Plan structure:**

1. **Roof Health Assessment Summary** — Based on the inputs, summarize the current condition and risk factors. Estimate the roof's remaining useful life under current conditions vs. with regular maintenance.

2. **Maintenance Schedule** — Create a month-by-month or seasonal task calendar appropriate for the roof type and climate:
   - Spring: debris clearing, gutter inspection, sealant checks
   - Summer: UV/heat damage assessment, ventilation check
   - Fall: pre-winter prep, flashing inspection, gutter cleaning
   - Winter/Post-storm: emergency inspection triggers, ice dam prevention tasks
   - Adjust tasks based on material type (e.g., metal roofs need fastener checks; flat roofs need drainage and membrane inspections)

3. **Tiered Service Options** — Present 2–3 tiers:
   - **Basic**: Annual inspection + gutter cleaning (homeowner-friendly entry point)
   - **Standard**: Bi-annual inspections + minor repairs included + priority storm response
   - **Premium**: Quarterly inspections + all repairs under a cap + extended warranty coordination + priority scheduling

4. **ROI Justification** — Include a brief cost comparison: average cost of neglected roof failure vs. annual maintenance investment. Reference typical lifespan extensions from regular upkeep.

5. **Proposal-Ready Output** — Format as a customer-facing document with company branding placeholders, ready for delivery via email or print.

**Output requirements:**
- Professional, customer-friendly tone — no overly technical jargon unless the audience is commercial/facilities
- Include seasonal task calendar as a simple table
- Pricing placeholders or calculated tiers if rates are provided
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
