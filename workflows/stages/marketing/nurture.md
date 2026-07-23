# Stage — Nurture / Outreach (marketing-gtm)

Draft personalized nurture or outbound sequences. **Human must approve before any send.**

## Inputs

- Requires: opportunity brief, qualification, enrichment; optional `analysis/marketing-decision.md` if board ran
- Roles: `sales-lead` (`sales-automator`), optional `content-strategist` for content touches
- Template: [templates/campaign-brief.md](../../../templates/campaign-brief.md) (nurture section)

## Critical rules

1. Draft only — never send email/LinkedIn/Upwork without checkpoint APPROVE.
2. Write drafts under `campaigns/nurture-<slug>.md`.
3. Sequences target HubSpot workflows or manual send notes — orchestrator owns CRM enrollment.

## Steps

### 1. Load context

Paste enrichment + qualification (+ board decision if present) into prompts.

### 2. Draft sequence

Dispatch `sales-lead`. Output schema:

```markdown
# Nurture Draft: [slug]

## Goal
[activation event — reply, demo, proposal request]

## Sequence (3–5 touches)
| # | Channel | Delay | Subject / hook | Body |
|---|---------|-------|----------------|------|
| 1 | email | day 0 | … | … |

## Personalization variables
…

## Objection handlers
…

## Tracking
- UTM: …
- HubSpot enrollment: [workflow name or manual]
- pipeline_stage_internal: nurture
```

Optional: `content-strategist` adds one content asset CTA aligned to ICP.

### 3. PHASE CHECKPOINT — Approve send

```
Nurture drafts ready: campaigns/nurture-<slug>.md

1. APPROVE SEND — enroll in HubSpot / allow human to send
2. REVISE — edit copy
3. HOLD — keep as draft only
```

- **APPROVE SEND:** `flags.nurture_send_approved = true`; orchestrator enrolls or queues via n8n
- Do not mark SQL until acceptance criteria met (see handoff stage)

### 4. Update state

```json
"checkpoints.nurture": "completed",
"files.nurture": "campaigns/nurture-<slug>.md",
"stage": "handoff_or_close",
"last_updated": "<ISO>"
```

If still early-funnel after send, set `stage` back to wait note or keep `nurture` with `status: awaiting_reply` — do not invent replies.

## Done when

- Draft written
- Human resolved send checkpoint
- State reflects approval and next stage
