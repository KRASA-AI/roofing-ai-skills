# Insurance-AI Regulatory Landscape for Roofers (April 2026)

*Inspiration: Carried-over priority from the April 15, 16, and 23 monitor logs. April 2026 state of the insurance-AI regulatory environment synthesized from public NAIC, state department of insurance, and published legal analysis sources. Extracted as a compliance-and-strategy snapshot for roofers. No source text copied.*

## Why This File Exists

Carriers now run AI models against roofs, claims, and policyholders at every stage of the lifecycle — underwriting, rating, renewal, claim triage, and payout calculation. That shift has created a parallel shift on the regulatory side: states are moving, at different speeds, to force explainability, disclosure, bias testing, and appeal rights around those models. Roofing contractors sit in the middle — field-verifying algorithmic decisions, writing supplements that must survive an NLP pipeline, and advising homeowners who received a non-renewal letter with no imagery attached. This file consolidates the current regulatory posture so the `insurance-supplement-writer` and `insurance-appeal-inspection-report` skills can reference the right rules, and so contractors know which consumer protections they can cite on a homeowner's behalf.

## Current Carrier-AI Use Against Roofs (April 2026)

The dominant adversarial patterns documented in early 2026 reporting are:

1. **Pre-renewal aerial scoring** — Carriers contract aerial imagery vendors (CAPE Analytics, Nearmap, Eagleview, Zesty.ai) to score every insured roof on a recurring interval. A score below a carrier threshold triggers a non-renewal notice, a repair demand, or an RCV-to-ACV conversion, often within 30–60 days, and usually without the homeowner ever seeing the scored imagery
2. **Claim-intake triage scoring** — When a storm claim arrives, the carrier's AI assigns a "risk of fraud / risk of denial" score that routes the claim into a fast-approve, full-investigation, or automated-deny lane before a human adjuster is assigned
3. **Predictive denial models** — Claim characteristics (homeowner profile, neighborhood, prior claims, roof age, zip-code storm history) are fed into a score that predicts whether a claim "should" be denied, with the downstream adjuster decision weighted by that score. Industry reporting cites sub-two-second per-claim review times on the first-pass models
4. **Post-submission supplement triage** — Supplement requests are scanned by NLP for certain phrases, formats, and line-item patterns; requests that score below a threshold receive a templated denial letter rather than a line-by-line reply

The roofer's position is that every one of these models is challengeable — they produce false positives (solar panels read as damage, neighbor imagery attributed to the wrong parcel, seasonal shadows read as defects), the underlying vendor scoring is usually undisclosed, and most state regulators now require some level of explainability on adverse action.

## Federal Posture

There is no federal insurance AI statute as of April 2026. Insurance regulation is state-level under the McCarran-Ferguson Act, with the National Association of Insurance Commissioners (NAIC) publishing model guidance that individual states adopt at different speeds.

The baseline reference document is the **NAIC Model Bulletin on the Use of Artificial Intelligence Systems by Insurers**, adopted December 2023. Its operating requirements are: (a) a written AI governance program documented at the board level; (b) risk assessment of each AI system before deployment; (c) vendor due diligence where a third-party model is used; (d) monitoring and auditing of outcomes for disparate impact; (e) explainability sufficient to respond to a consumer complaint. More than half of states have adopted the bulletin or a variation as of Q1 2026, and most of the rest are expected to follow through 2026.

A **2026 NAIC Model Law on Third-Party Oversight** is anticipated for vote in mid-to-late 2026 based on published working-group agendas. The anticipated provisions include licensing or registration of third-party AI vendors selling into regulated insurers, and contractual disclosure obligations that run from the carrier through the vendor chain. Roofers should watch this because it would likely force aerial-imagery vendors to disclose more about their scoring models than they do today.

## State-Level Rules That Affect Roofing Appeals

Roofers working in these states can cite specific state rules in supplements and appeal reports. The list is not exhaustive — multi-state operators should verify with counsel or a state insurance department before citing — but these are the provisions most commonly useful in a counter-documentation workflow.

### New York — DFS Circular Letter 2024-7
Applies to New York-licensed insurers using AI or external consumer data for underwriting or rating. Requires each insurer to: document that its AI system does not proxy for a protected class; test for disparate impact with quantitative methods; maintain explainability documentation sufficient to respond to a consumer request; keep logs available for DFS review; require vendor audits when third-party models are used. Practical contractor angle: when a New York homeowner receives a non-renewal letter based on an aerial score, the contractor's appeal packet can request the explainability documentation that the Circular Letter already requires the carrier to maintain. Most carriers will respond faster to keep the question from escalating to the DFS.

### Colorado — C.R.S. §10-3-1104.9 and Reg. 10-1-1
Prohibits use of external consumer data and predictive models that produce "unfair discrimination" in insurance. The implementing regulation currently applies fully to life insurance, with a broader expansion under active rulemaking for property and casualty. The direction of travel is clear — quantitative disparate-impact testing, insurer-level documentation, and a state-level audit right. Practical contractor angle: a Colorado homeowner contesting an aerial-imagery non-renewal can cite the prohibition on unfair discrimination and request the insurer's disparate-impact testing record. Even before the P&C expansion finalizes, the statutory language supports the consumer-side ask.

### California — SB 1120 and related DOI bulletins
Focuses on human oversight of AI-assisted coverage decisions in health lines, with expanding application patterns for P&C through DOI bulletins. The operative concept — that an AI decision adverse to a consumer must be reviewable by a human with sufficient expertise — is the cleanest lever on a claim denial routed through a predictive denial model. Practical contractor angle: appeal letters in California routinely request "the name and credentials of the human who reviewed this decision before the adverse notice issued," which forces the carrier to either provide a named reviewer or reopen the decision.

### Washington — WAC 284-30 supplement and the 2024 OIC guidance
Washington requires that an insurer's claim-handling practices be documentable and that automated denial decisions be reviewable on request. Practical contractor angle: a supplement denial that appears to have been machine-generated can be challenged with a request for the manual review record; absent one, the denial is procedurally vulnerable.

### Texas — SB 1399 and the TDI data-use framework
Texas does not have a single AI-in-insurance statute, but the Texas Department of Insurance enforces a data-use and notice framework that requires insurers to disclose adverse underwriting actions and the "principal reasons." When the principal reason is an AI score, the score must be explainable to the consumer on request. Practical contractor angle: request the principal-reasons letter in writing and pair it with the contractor's counter-documentation before the policy lapse deadline.

### Florida — OIR Informational Memoranda on aerial imagery
Florida's Office of Insurance Regulation has issued multiple informational memoranda around aerial imagery and roof age triggers, most recently tightening disclosure expectations for non-renewal decisions. Practical contractor angle: cite the OIR memoranda in the appeal letter; Florida carriers are attentive to OIR posture and will often restore coverage to avoid formal review.

### Other States with Active AI Insurance Activity
Connecticut, Illinois, Maryland, Massachusetts, New Jersey, Oregon, Rhode Island, Vermont, and Washington, D.C. have adopted some version of the NAIC Model Bulletin or a functional equivalent. Contractors in those states can reference the adoption and request the carrier's AI governance documentation in a supplement or appeal packet. The response rate on such requests is materially higher than on an unsupported appeal letter.

## What Roofers Can Cite in a Supplement or Appeal

A consumer-side appeal packet or supplement request that cites the applicable state rule and requests explainability documentation will move faster than one that does not. The standard pattern:

1. **Name the regulatory hook.** If the carrier's decision relied on an AI or external-data model, cite the applicable state rule (DFS Circular Letter 2024-7, C.R.S. §10-3-1104.9, etc.) or the NAIC Model Bulletin where the state has adopted it
2. **Request the explainability record.** Ask for the specific factors that drove the adverse decision, the vendor if a third-party model was used, and the date of the underlying imagery or data. Most states now either require or strongly expect this on request
3. **Submit quantified counter-documentation.** Use the `insurance-appeal-inspection-report` skill to produce an algorithm-compatible counter-report with geo-tagged evidence and point-by-point rebuttal of each flagged defect
4. **Name the human reviewer.** Particularly in California, Washington, and New York, request the name and credentials of the human reviewer who signed the adverse decision. Absence of a named reviewer is itself a procedural flag
5. **Escalate on a clock.** Cite the state complaint process (DFS, DOI, OIR, TDI) in the letter, with the complaint deadline, to signal that the contractor and homeowner will escalate if the carrier does not respond. This materially shortens response time

## What Roofers Should Not Do

- Do not misrepresent the state rule's scope. If the rule applies only to life insurance (Colorado's current implementing regulation) or only to health insurance (some California provisions), citing it as binding on the property carrier is a weak move that can backfire
- Do not claim the AI model is "illegal." Most state rules do not prohibit AI in underwriting or claims — they require documentation, explainability, and oversight. The ask is for that documentation, not a prohibition
- Do not promise the homeowner a guaranteed outcome. The counter-documentation pathway is effective but variable — typical reversal rates are meaningful but not universal, and the contractor's value proposition is the documentation quality, not the guaranteed reversal
- Do not submit appeal letters signed by the contractor alone. The homeowner must sign the consumer-facing complaint; the contractor's role is the inspection, the technical rebuttal, and the supporting documentation

## Emerging Items to Watch Through 2026

- **NAIC Third-Party Oversight Model Law (expected mid-to-late 2026)** — Likely to raise vendor disclosure obligations on the aerial-imagery vendors used by carriers. Watch for state adoption timelines through late 2026 and 2027
- **State AI liability bills** — Several state legislatures have bills moving in the 2026 session that would create a private right of action against carriers for disparate-impact outcomes from AI-driven decisions. If any pass, the leverage on appeal letters increases further in those states
- **Federal posture shift** — Insurance remains state-regulated, but federal activity on AI consumer protections (CFPB, FTC, state attorneys general) may create indirect pressure on carriers through adjacent statutes (consumer protection, deceptive practices). Watch for federal guidance letters with insurance implications
- **Vendor consolidation** — The aerial-imagery AI market is consolidating. Roofers should track whether a carrier's vendor changes over time and whether that changes the scoring thresholds. Vendor changes are frequently the reason a previously-insured roof suddenly fails a scoring threshold

## Cross-References

- **Skill:** `admin/insurance-appeal-inspection-report` — Homeowner-side counter-documentation for algorithmic non-renewals and repair demands. The applicable state rule from this file should be cited in the cover page of the report and in the closing paragraph requesting explainability documentation
- **Skill:** `admin/insurance-supplement-writer` — Supplement-request drafting for underpaid Xactimate estimates. When the supplement is contesting a machine-generated denial pattern, cite the state regulatory hook from this file in the supplement justification section
- **KB:** `industry-overview.md` — Insurance-AI Shift section for the market-context framing
- **KB:** `tools-ecosystem/ai-tools-landscape.md` — Insurance-AI Landscape (Adversarial) section for the vendor-layer context
- **External:** NAIC Model Bulletin on AI Systems (Dec 2023), NY DFS Circular Letter 2024-7, Colorado C.R.S. §10-3-1104.9, California SB 1120, Florida OIR memoranda

## Notable Uncertainty

- State adoption speed of the NAIC Model Bulletin varies widely. A contractor should verify the current status with the state department of insurance before citing adoption, not rely on this file
- The anticipated 2026 NAIC Third-Party Oversight Model Law is on the working-group agenda but has not been voted as of April 2026; timelines may slip into 2027
- Vendor-side disclosure rules may evolve rapidly. Aerial imagery vendors are actively negotiating the scope of disclosure they would accept; outcomes will depend on the model law text
- This file is a working reference, not legal advice. Contractors writing to state insurance commissioners, carriers, or adjusters with specific statutory citations should have those citations confirmed by counsel in the relevant state
