# Decision Record: Legacy extend vs integration sidecar

**Experiment:** `experiments/001-fullstack-webapp/`  
**Minutes:** `board/minutes-legacy-extend-vs-sidecar.md`  
**Date:** 2026-07-23  
**Status:** Approved  
**Dry-run:** yes — approved for fixture progression through design/package

## Proposal Debated

Extend the legacy PHP/AngularJS app in place for Stripe/QuickBooks reconciliation features vs introduce a thin modern API/sidecar beside it that owns integrations and sync, with the legacy UI calling the sidecar.

## Consensus Recommendation

**adopt-with-changes** — Prefer a **thin private integration worker** (sidecar) that owns Stripe/QuickBooks sync and reconciliation staging, sharing the primary SQL database via dedicated `integration_*` tables. Legacy UI/app remains for product UX. **Do not** store third-party tokens in legacy PHP. A written security baseline (secret store, least-privilege OAuth scopes, audit logging) is a condition of the decision.

## Vote Tally

| Member (role) | Final recommendation | Confidence (1–5) | Domain weight note |
|---------------|----------------------|------------------|--------------------|
| backend-architect | adopt-with-changes (private worker) | 4 | High on service boundaries |
| cloud-architect | adopt-with-changes (private worker, no public API v1) | 4 | High on deploy shape |
| database-architect | adopt-with-changes (same DB, integration_* tables) | 4 | High on schema |
| product-analyst | adopt-with-changes (thin, not platform) | 4 | High on scope |
| security-auditor | adopt-with-changes **conditional** on security baseline | 4 | Highest on secrets/trust |

## Confidence-Weighted Summary

Domain weighting favors security + data ownership: sidecar/worker wins over pure in-place, but only with security baseline and same-DB integration tables. Product weight caps scope: private worker, not a full platform.

## Recorded Dissents

1. **security-auditor** — Objects to any implementation kickoff without explicit secrets/scopes/audit baseline; would flip to reject if those are deferred.
2. **cloud-architect** — Objects to exposing a public/internal HTTP API for the sidecar in v1; batch/private worker only until proven need.

## Conditions / Open Questions

- [ ] Security baseline doc before coding (secrets, scopes, audit)
- [ ] Confirm batch vs real-time reconciliation need
- [ ] Confirm invoice/payment system of record
- [ ] Staging credentials for Stripe and/or QBO

## Implications for Design

Design must specify: worker boundaries, `integration_*` schema, secret handling, sync/recon job flow, and how legacy UI reads recon results **without** holding integration credentials.

## Human Approval

- [x] Approved to proceed to design (dry-run fixture)
- [ ] Revise decision
- [ ] Reject / re-open board

**Notes:** Dry-run auto-approved to validate downstream artifact chain.
