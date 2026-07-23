# Stage — Retain & Advocate (marketing-gtm)

Post-sale retention, expansion, and advocacy motions. Maps to HubSpot Customer → Evangelist.

## Inputs

- Requires: closed-won deal context, contact/company IDs
- Roles: `customer-success`; optional `content-strategist` for case study

## Critical rules

1. Treat sales→CS handoff as a first-class event (buyer context, promised outcomes, risks).
2. Write `campaigns/retain-<slug>.md`.
3. Advocacy asks (review, referral, case study) require consent notes.

## Steps

### 1. CS kickoff memo

Dispatch `customer-success`:

```markdown
# Retain & Advocate: [slug]

## Handoff packet
- Promised outcomes: …
- Use case / scope: …
- Stakeholders: …
- Renewal / next milestone: …
- Risk flags: …

## Onboarding plan (first value moment)
…

## Health signals to watch
…

## Advocacy plan
- Case study eligibility: …
- Referral ask timing: …
- Review sites: …

## HubSpot
- lifecyclestage: customer | evangelist
- pipeline_stage_internal: retain_advocate
```

### 2. Optional content

`content-strategist` drafts case-study outline under `campaigns/case-study-<slug>.md` (draft only).

### 3. Update state

```json
"checkpoints.retain_advocate": "completed",
"files.retain": "campaigns/retain-<slug>.md",
"status": "complete",
"stage": "complete",
"last_updated": "<ISO>"
```

Advocacy loops may reopen later via a new surface/nurture cycle; do not infinite-loop here.

## Done when

- Retain memo written
- HubSpot lifecycle updated (or pending payload)
- Pipeline marked complete or explicitly left open with note
