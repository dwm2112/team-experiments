# Stage — Campaign Package (marketing-client)

Consolidate design artifacts into a deliverable package. Terminal for client delivery unless execute_hooks is requested.

## Inputs

- Requires: campaign brief, marketing decision, `campaigns/01–04` design files
- Template: [templates/campaign-package.md](../../../templates/campaign-package.md)

## Steps

### 1. Consolidate

Write `campaigns/campaign-package.md` including:

- Executive summary
- Approved board consensus + dissents (link)
- Positioning and offer
- Channel plan summary
- Content / SEO calendar summary
- Measurement and HubSpot lifecycle contract
- Activation / sales handoff rules
- Open questions and next human actions
- Execute hooks checklist (SocialClaw, n8n, ads — not implemented here)

### 2. PHASE CHECKPOINT — Deliver

```
Campaign package ready: campaigns/campaign-package.md

1. DELIVER — mark complete (optional execute_hooks next)
2. REVISE — return to campaign_design with notes
```

- **DELIVER:** `flags.package_delivered = true`, `checkpoints.package = completed`
  - If user wants automation handoff → `stage = execute_hooks`
  - Else → `status = complete`, `stage = complete`

## Done when

- Package written
- Human chose DELIVER or REVISE
