# Client Engagement Qualification: 002-marketing_team_workflow

## Risk verdict
Medium  
Fit and deliverable type are clear (branding + kickoff campaign package), but budget, timeline, channels, and success metrics are unset, so commercial and measurement risk remains material until conditions close.

## Main assumption
Atlas Sovereign will fund and approve a focused Agentic AI Division brand + kickoff campaign for SMB / nonprofit / freelance buyers—not a multi-division corporate brand program—with enough budget and a near-term decision window to make a fixed-scope package worthwhile.

## Evidence gaps / clarifications needed
- Budget range (or ceiling) and commercial model (fixed package vs hourly vs milestone)
- Target go-live / decision date and any hard launch event
- In/out scope for “branding” (name lock, messaging, visual system, brand book) vs campaign package only
- Primary kickoff channels (web landing, LinkedIn, email, Upwork, events, paid) and any must-use assets
- Success metrics for the kickoff (leads, demos, waitlist, awareness proxy) and who owns measurement
- Brand hierarchy: Agentic AI Division only vs Atlas Sovereign umbrella (Robotics / Intelligence)
- Existing assets: domain, site, logo, social handles, prior messaging, legal/compliance review path for AI claims
- Decision maker / approval SLA and whether outbound execution is in-scope later or design-package only

## Checklist scores
| Lens | Score (1–5) | Notes |
|------|-------------|-------|
| Demand / urgency | 3 | Kickoff branding for a new commercial line implies near-term need; no deadline or buying trigger stated |
| Positioning fit (can we deliver) | 5 | Branding + kickoff campaign package matches marketing-client pipeline; we stop at package / optional execute hooks |
| Monetization / budget realism | 2 | Budget absent; cannot confirm package ROI or price band until client states range |
| Scope clarity | 3 | Objective and ICP clear; deliverable depth, channels, and brand hierarchy still ambiguous |
| Brand / trust constraints | 4 | Charter values (trust, human-centered, responsible AI, interoperability) give usable guardrails; claim-risk needs claim/legal checklist |
| Timeline feasibility | 2 | “Kickoff” implies urgency but no dates; feasibility unknown until week count and approval cadence are set |
| Measurement readiness | 2 | No KPIs, attribution, CRM lifecycle, or baseline; HubSpot can be stood up but client must define success |

## Recommended verdict
**GO-WITH-CONDITIONS**

## Conditions (if GO-WITH-CONDITIONS)
- Confirm budget band (or max) and engagement commercial terms before drafting package pricing
- Lock timeline (target package delivery date + any launch date) and single decision owner with approval SLA
- Scope freeze: Agentic AI Division brand + kickoff campaign only; Robotics/Intelligence out of scope for this engagement
- Narrow ICP for kickoff creative (pick primary segment among SMB / nonprofit / freelance, or explicit priority order)
- Agree deliverable checklist (positioning one-pager, messaging house, visual direction, campaign plan, nurture drafts as applicable)—no site/build implementation
- Define 2–3 kickoff success metrics and whether HubSpot tracking is required for this client
- Obtain claim-safety constraints for agentic AI / “digital employee” language before public-facing copy is finalized

## HubSpot company payload (for orchestrator; no API keys)
```json
{
  "name": "Atlas Sovereign",
  "properties": {
    "experiment_id": "002-marketing_team_workflow",
    "surfaced_by": "intake",
    "pipeline": "marketing-client",
    "engagement_type": "branding_kickoff_campaign",
    "icp_segments": "SMB|nonprofit|freelance"
  }
}
```
