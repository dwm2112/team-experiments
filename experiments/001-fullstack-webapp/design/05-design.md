# Technical Design: Full Stack Web App Engagement

**Experiment:** `experiments/001-fullstack-webapp/`  
**Requirements:** `analysis/03-requirements.md`  
**Decision:** `analysis/04-decision.md`  
**Generated:** 2026-07-23  
**Dry-run:** yes

## Summary

Per the board decision: keep the legacy PHP/AngularJS app for product UX; add a **thin private integration worker** that owns Stripe/QuickBooks sync and reconciliation staging. Share one primary SQL database with dedicated `integration_*` tables. Third-party tokens never live in legacy PHP. v1 worker is not a public API.

## Data Model

### Entities & Relationships

- Existing app entities (users, domain records) — unchanged SoR in legacy schema
- `integration_connection` — provider, status, external account refs (no raw secrets)
- `integration_sync_run` — job runs, timestamps, status, error summary
- `integration_object` — mirrored external objects (invoice/payment/customer) with external ids + checksums
- `integration_recon_issue` — mismatches for human/workflow follow-up
- Audit columns on all integration tables (`created_at`, `updated_at`, `actor`)

### Schema Notes

| Entity | Key fields | Indexes / constraints |
|--------|------------|------------------------|
| integration_connection | id, provider, status, external_account_id | unique(provider, external_account_id) |
| integration_sync_run | id, connection_id, started_at, status | index(connection_id, started_at) |
| integration_object | id, connection_id, object_type, external_id, payload_hash | unique(connection_id, object_type, external_id) |
| integration_recon_issue | id, object_id, issue_type, status | index(status, issue_type) |

### Migration / Compatibility

Additive migrations only. No destructive changes to legacy tables in v1. Legacy app gains **read-only** views or simple queries into recon issue lists.

## Backend / API Architecture

### Endpoints / Contracts

| Method | Path / operation | Purpose | Auth |
|--------|------------------|---------|------|
| n/a (v1) | private worker CLI/cron entrypoints | pull sync + recon | runtime identity + secret store |
| GET | legacy admin page data via existing PHP | list open recon issues | existing app session |

Optional later (not v1): internal HTTP API behind VPN/mTLS if UI needs richer interaction.

### Service Layer

- **SyncService** — pull provider objects idempotently
- **ReconService** — compare mirrored objects vs internal records; open/close issues
- **SecretProvider** — resolve tokens from managed secret store only

### Integrations

Stripe and/or QuickBooks: sandbox → reconciliation report verify → production. Least-privilege OAuth scopes. Token refresh handled only in worker.

## Frontend / Client Touchpoints

Minimal legacy UI: list/filter recon issues; mark resolved. No new SPA framework in v1.

## Cross-Cutting

- **AuthZ:** Worker uses machine identity; humans use existing app roles for recon UI
- **Error handling:** Failed sync runs recorded; alerts on repeated failure
- **Observability:** Structured logs per sync_run id
- **Security notes:** No tokens in PHP; audit log; encrypt tokens at rest in secret store

## Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| Ops burden of second runtime | Single small worker; cron; shared DB |
| Scope → platform | Hard cap: one provider + one recon workflow first |
| Secret leakage | Secret store + dissent conditions from board |

## Architecture Review Notes

- Boundaries OK if worker stays private and schema stays additive
- Unresolved: pick first provider (Stripe vs QBO) after client answers clarifying Q2
- Must-fix before real implementation: security baseline checklist signed off

## Rough Estimate (design-level)

| Slice | Effort band | Notes |
|-------|-------------|-------|
| Security baseline + secret wiring | S | Condition of decision |
| integration_* migrations | S | |
| Worker sync for one provider | M | |
| Recon issue detection + legacy UI list | M | |
| Second provider | M | defer |

**Total band:** M–L for first provider end-to-end  
**Assumptions:** Sandbox access within first week; one primary DB; batch recon OK
