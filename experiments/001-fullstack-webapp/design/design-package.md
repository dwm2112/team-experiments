# Design Package: Full Stack Web App Engagement

**Experiment:** experiments/001-fullstack-webapp/  
**Generated:** 2026-07-23  
**Pipeline status:** complete (dry-run)  
**Dry-run:** yes

## Executive summary

For this Upwork-style engagement, the locked approach is a **thin private integration worker** beside the legacy PHP/AngularJS app: the worker owns Stripe/QuickBooks sync and reconciliation staging in shared-DB `integration_*` tables; the legacy app keeps UX and never holds third-party tokens. A security baseline is a precondition to implementation.

## Client-facing proposal (draft)

See `intake/02-proposal.md` — **placeholders remain** for stories, honesty line, location, hours, rate, and name. Do not send until filled.

## Clarifying questions

Priority (in proposal):

1. First workflow to unblock — billing recon, reporting, or other?
2. Stripe/QBO already in production or near-term?

Full list: `intake/02-clarifying-questions.md`

## Requirements summary

Independent US full-stack hire; vague-ask → ship; SQL/recon; likely Stripe/QBO; evolve legacy rather than unpaid rewrite. Details: `analysis/03-requirements.md`

## Decision record

**adopt-with-changes** — private worker/sidecar + `integration_*` tables + security baseline.  
Dissents recorded: security baseline mandatory; no public sidecar API in v1.  
Full: `analysis/04-decision.md` · Minutes: `board/minutes-legacy-extend-vs-sidecar.md`

## Technical design summary

Additive schema; Sync/Recon/Secret services in worker; minimal legacy recon UI; batch-first. Full: `design/05-design.md`

## Rough estimate

First provider end-to-end: **M–L**. Second provider deferred.

## Artifact index

| Artifact | Path |
|----------|------|
| RFP | intake/00-rfp.md |
| Qualification | intake/01-qualification.md |
| Questions | intake/02-clarifying-questions.md |
| Proposal | intake/02-proposal.md |
| Requirements | analysis/03-requirements.md |
| Decision | analysis/04-decision.md |
| Board minutes | board/minutes-legacy-extend-vs-sidecar.md |
| Design | design/05-design.md |

## Out of scope for this package

- Implementation, tests, deployment
- Sending the Upwork bid (human action)
