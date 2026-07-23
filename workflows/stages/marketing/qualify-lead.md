# Stage — Qualify Lead (marketing-gtm)

Score and lifecycle-recommend surfaced opportunities. Halt for human go/no-go before enrich/nurture.

## Inputs

- Requires: `briefs/opportunity-<slug>.md`, `state.hubspot`
- Roles: `revops-analyst` (+ apply `before-you-build` checklist inline)
- HubSpot lifecycle target: Lead → MQL when criteria met

## Critical rules

1. Read the opportunity brief; do not rely on memory.
2. Write `analysis/lead-qualification-<slug>.md` before checkpoint.
3. **STOP** at checkpoint — do not auto-start enrich.
4. Prefer `before-you-build` skill for demand/positioning/distribution risk.

## before-you-build checklist (lead framing)

- **Demand:** Evidence this buyer/account wants the offer now?
- **Positioning:** One-sentence why we are the right partner?
- **Monetization:** Credible path to paid engagement?
- **Retention:** Reason they would stay / expand after first win?
- **Trust:** Credibility barriers (NDA, location, AI hostility)?
- **Distribution:** Can we reach them repeatedly (channel access)?
- **Adoption:** Scope/engagement risk if vague?

## Steps

### 1. Load context

Read opportunity brief, feeder notes, and HubSpot IDs from state.

### 2. Produce qualification memo

Dispatch `revops-analyst`. Write `analysis/lead-qualification-<slug>.md`:

```markdown
# Lead Qualification: [slug]

**Brief:** briefs/opportunity-<slug>.md
**HubSpot contact id:** …
**Date:** [ISO]

## Risk verdict
[Low | Medium | High] — [one sentence]

## Main assumption
…

## Evidence to find first
…

## Do next
…

## Delay
…

## Checklist scores
| Lens | Score (1–5) | Notes |
|------|-------------|-------|
| Demand | | |
| Positioning | | |
| Monetization | | |
| Retention | | |
| Trust | | |
| Distribution | | |
| Adoption / scope | | |

## Scoring
- opportunity_score: 1–100
- recommended_lifecycle: subscriber | lead | mql | sql
- mql_criteria_version: v1

## Recommended verdict
**GO** | **NO-GO** | **GO-WITH-CONDITIONS**

## Conditions (if any)
- …
```

### 3. HubSpot patch payload

Return fields for orchestrator upsert: `opportunity_score`, `lifecyclestage` (recommendation only until human GO), `pipeline_stage_internal: qualify_lead`, notes.

### 4. PHASE CHECKPOINT

```
Lead qualification ready:
- analysis/lead-qualification-<slug>.md

Recommended: [GO | NO-GO | GO-WITH-CONDITIONS]

1. GO — proceed to enrich
2. NO-GO — park lead; do not nurture
3. REVISE — adjust scoring criteria or gather evidence
```

- **GO:** set `flags.qualify_verdict = "go"`, apply HubSpot lifecycle if score meets MQL threshold, `stage = enrich`
- **NO-GO:** `flags.qualify_verdict = "no-go"`, HubSpot `pipeline_stage_internal: parked`
- **REVISE:** re-run scoring with guidance

## Done when

- Qualification memo written
- Checkpoint resolved by human
- State updated
