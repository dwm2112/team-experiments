# Board Meeting — legacy-extend-vs-sidecar (2026-07-23)

**Experiment:** `experiments/001-fullstack-webapp/`  
**Decision output:** `analysis/04-decision.md`  
**Dry-run:** yes — simulated Round 0/1 positions to validate minutes + dissent format (not a live multi-agent run)

## Proposal

Extend the legacy PHP/AngularJS app in place for Stripe/QuickBooks reconciliation features vs introduce a thin modern API/sidecar beside it that owns integrations and sync, with the legacy UI calling the sidecar.

## Roster & Roles

- security-auditor — **DEVIL'S ADVOCATE**
- backend-architect
- cloud-architect
- database-architect
- product-analyst

## Round 0 — Independent Positions

### Member A (backend-architect)

- **RECOMMENDATION:** adopt-with-changes (prefer **sidecar** for integrations; keep legacy UI)
- **TOP 3 RISKS / objections:**
  1. In-place Stripe/QBO in legacy PHP will entangle auth and error handling
  2. AngularJS-era client makes robust retry/idempotency UX hard
  3. Dual-write bugs if sync is bolted onto request path
- **KEY ASSUMPTION:** Client will accept a small Node/Python service if it reduces risk
- **CONFIDENCE:** 4

### Member B (cloud-architect)

- **RECOMMENDATION:** adopt-with-changes (**sidecar**)
- **TOP 3 RISKS / objections:**
  1. Extra deployable increases ops burden on a &lt;30 hr/wk engagement
  2. Secrets/config sprawl across two runtimes
  3. Cost of always-on sidecar for low volume may be unnecessary
- **KEY ASSUMPTION:** Workload is modest; a single small service or scheduled worker is enough
- **CONFIDENCE:** 3

### Member C (database-architect)

- **RECOMMENDATION:** adopt-with-changes (**sidecar** owns sync tables; legacy DB remains SoR for app entities)
- **TOP 3 RISKS / objections:**
  1. In-place schema hacks for QBO IDs without clear ownership
  2. Reconciliation reports need stable staging tables either way
  3. Cross-db consistency if sidecar gets its own store
- **KEY ASSUMPTION:** One primary SQL database; sidecar may add schemas/tables, not a second product DB
- **CONFIDENCE:** 4

### Member D (product-analyst)

- **RECOMMENDATION:** adopt-with-changes lean **in-place first slice**, sidecar only if second integration arrives
- **TOP 3 RISKS / objections:**
  1. Sidecar may over-build before first workflow is unblocked
  2. Client asked for shipping, not platform
  3. Dual systems confuse “done” for a vague ask
- **KEY ASSUMPTION:** First unlock is one recon workflow, not multi-integration platform
- **CONFIDENCE:** 3

### Member E (security-auditor) — DEVIL'S ADVOCATE

- **RECOMMENDATION:** **reject** both naive paths as stated — argue against premature sidecar *and* against dumping secrets into legacy PHP
- **TOP 3 RISKS / objections:**
  1. Sidecar expands attack surface (new public/internal API) without threat model
  2. In-place puts payment/accounting tokens in the oldest, least-hardened codepath
  3. Neither proposal names audit logging, least-privilege OAuth scopes, or secret storage
- **KEY ASSUMPTION:** Financial integrations without explicit security baseline will create client trust failures
- **CONFIDENCE:** 5

## Round 1 — Cross-Examination

*(Anonymized packet used in live runs; dry-run shows role labels for readability.)*

### Member A (backend-architect)

- **Strongest objection to each other position:** B underweights long-term entanglement; D’s in-place-first invites the secret-in-legacy problem E flags; C’s single-DB constraint is right; E is correct that security baseline is missing
- **Defend OR revise:** Keep sidecar for **integration + sync worker only**; mandatorily add secret store + scoped OAuth + audit log — adopt-with-changes
- **Revised confidence:** 4
- **Falsifier:** Evidence that legacy already has a solid secrets/module boundary for third-party APIs

### Member B (cloud-architect)

- **Objections:** A may over-service; E’s surface concern is valid — prefer private worker + cron over exposed API initially
- **Revise:** Sidecar as **private worker** (no public API) until UI needs it; confidence 4
- **Falsifier:** Hard requirement for real-time UI callbacks in week 1

### Member C (database-architect)

- **Objections:** D’s in-place risks ID pollution; E correct on audit fields
- **Revise:** Sidecar/worker with dedicated `integration_*` tables + audit columns in same DB — confidence 4
- **Falsifier:** Client forbids any new tables

### Member D (product-analyst)

- **Objections:** A/B may gold-plate; E’s reject is too absolute for delivery
- **Revise:** Accept **thin private worker + integration tables** as the “sidecar” meaning — not a full platform — confidence 4
- **Falsifier:** First deliverable truly is one SQL report with no external API

### Member E (security-auditor) — DEVIL'S ADVOCATE

- **Objections:** Consensus forming around sidecar without authZ diagram; worker still holds tokens
- **Revise:** Still against “just ship”; **adopt-with-changes** only if: secrets in managed store, least-privilege scopes, encrypted tokens at rest, audit log, no tokens in legacy PHP — else reject
- **Revised confidence:** 4
- **Falsifier:** Written security baseline accepted by client before coding

## Round 2 — Revised Positions (optional)

Round 2 skipped — no material adopt-vs-reject split after security conditions; remaining dissent is conditional severity, not opposite recommendation.

## Decision

- **Consensus:** adopt-with-changes — **thin private integration worker/sidecar** beside legacy app; same primary DB with `integration_*` tables; **no third-party tokens in legacy PHP**; security baseline required before implementation
- **Vote tally:** 5× adopt-with-changes (E conditional); 0× pure reject after Round 1
- **Recorded dissents:** E — dissent on proceeding without an explicit security baseline (secrets, scopes, audit); B — dissent on any publicly exposed sidecar API in v1
- **Open questions:** Real-time vs batch recon; which system is SoR for invoices; staging credentials timeline
