# Stage — Handoff or Close (marketing-gtm)

Move activated demand to opportunity / close. Owner may be human or `sales-lead`.

## Inputs

- Requires: nurture drafts, HubSpot contact; optional Stripe/payment note
- Roles: `sales-lead`; human AE may replace agent for close

## Critical rules

1. SQL acceptance must be documented (fit + intent + recency).
2. Create/update HubSpot **deal** via orchestrator payload — not blackboard-only.
3. Closed-won triggers retain stage; closed-lost records reason.

## Steps

### 1. Assess readiness

Dispatch `sales-lead` (or record human assessment) into `analysis/handoff-<slug>.md`:

```markdown
# Handoff / Close: [slug]

## SQL acceptance
- Fit: …
- Intent: …
- Recency: …
- Verdict: accept | reject | needs_more_nurture

## Deal draft
- name: …
- amount: …
- pipeline / stage: …
- close date: …

## Proposal / next meeting
…

## HubSpot
- lifecyclestage: opportunity | customer
- pipeline_stage_internal: handoff_or_close
- deal properties: …
```

### 2. HubSpot deal upsert

Orchestrator creates/updates deal; link contact. Record `state.hubspot.deal_ids`.

### 3. Checkpoint (optional if human owns close)

If amount or terms are material:

```
1. APPROVE CLOSE PATH — proceed with proposal / negotiate
2. RETURN TO NURTURE
3. LOST — record reason
```

### 4. Route

- Closed-won or customer → `stage = retain_advocate`
- Needs more nurture → `stage = nurture`
- Lost → `status = complete`, `flags.close_result = "lost"`

## Done when

- Handoff memo written
- Deal ID recorded or explicitly deferred
- Next stage set
