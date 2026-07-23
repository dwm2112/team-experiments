# RFP — Full Stack Developer for Web App

**Captured:** 2026-07-23T12:00:00Z  
**Source:** reconstructed from signals in repo `upwork-proposal.md` (original Upwork posting text not archived; this is the seed substrate for experiment 001)  
**Experiment:** `experiments/001-fullstack-webapp/`

## Structured extract

| Field | Value | Confidence |
|-------|-------|------------|
| Title / one-liner | Full stack developer for an existing web app — vague asks, end-to-end shipping | H |
| Budget / rate | $70–90 /hr | H |
| Hours / duration | &lt;30 hrs/week; 6+ months preferred | H |
| Location constraints | **US-based only** (hard filter) | H |
| Stated stack | JS/HTML5; likely PHP + AngularJS (legacy) in place | M |
| Integrations named | Stripe and/or QuickBooks; SQL-heavy reconciliation / reporting | H |
| Deliverables | Features end-to-end (front → DB), automation of manual workflows, data movement/recon | M |
| Vague-ask signals | Explicitly wants someone who can clarify requirements without hand-holding; push back with reasons; honest about gaps | H |
| Red flags | Anti “paste AI until it runs”; vague scope can become unpaid discovery; legacy stack may resist rewrite | M |
| Opportunity signals | Long engagement; independent operator; business-context questions welcomed; rate band clear | H |

## Raw text

```
Looking for a US-based full stack developer for our web application.

This is not a "here's a perfect spec, build exactly this" role. Work often starts as a vague business ask. We need someone who can figure out the requirements, push back with reasons when something doesn't make sense, be honest about gaps, and ship end-to-end — front end through the database — without constant hand-holding.

Tech context:
- Existing app is primarily JS / HTML5 on what is likely a PHP + AngularJS-era stack (not a greenfield rewrite mandate)
- Day-to-day often involves SQL, reconciliation, and reporting
- Integrations of interest: Stripe and/or QuickBooks Online (moving, reconciling, or presenting data)

Also valuable: automation — replacing manual weekly/monthly busywork with reliable scripts or workflows.

AI coding tools are fine if you use them to explore and verify. We do not want someone who pastes errors into a chatbot until something runs and calls it done. You still own architecture and final review.

Logistics:
- Must be located in the United States
- Under 30 hours/week
- Looking for 6+ months
- Budget roughly $70–90/hour

If you've taken messy asks and shipped before — especially around data/API integration or automation — tell us about that. Ask smart questions about our business context and stack.
```

## Seed notes

- Proposal template previously drafted for this posting lives at repo root `upwork-proposal.md` and was generalized into `templates/proposal.md`.
- Pipeline should treat this file as `intake/00-rfp.md` (Stage 0 complete for seeding).
- Suggested decision gate for later board meeting: **Extend legacy PHP/AngularJS in place vs introduce a thin modern API/sidecar beside it for Stripe/QuickBooks reconciliation work.**
