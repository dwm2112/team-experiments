# Stage — Enrich (marketing-gtm)

Add firmographics, pain points, and proof hooks to a GO-qualified lead.

## Inputs

- Requires: `briefs/opportunity-<slug>.md`, `analysis/lead-qualification-<slug>.md`, HubSpot contact id
- Roles: `search-specialist` and/or `sales-lead` (parallel OK)

## Critical rules

1. Paste prior brief + qualification into every subagent prompt.
2. Write `briefs/enrichment-<slug>.md` before advancing.
3. Set `flags.needs_board = true` only for high-stakes forks (new ICP, channel bet, pricing change).

## Steps

### 1. Research (PARALLEL optional)

Dispatch `search-specialist` for company/person research and `sales-lead` for pain/CTA framing.

### 2. Write enrichment brief

```markdown
# Enrichment: [slug]

**Contact / company:** …
**Date:** [ISO]

## Firmographics
…

## Pain hypotheses
1. …
2. …

## Proof points / social proof to use
…

## Buying committee (if known)
…

## Recommended next motion
nurture | outbound_sequence | board_required

## HubSpot property updates
- jobtitle, industry, website, linkedinbio / notes
- pipeline_stage_internal: enrich
```

### 3. HubSpot upsert

Orchestrator patches contact/company notes and properties. Append note body with enrichment summary.

### 4. Route

- If `needs_board` → set `pending_board_proposal` and `stage = board_meeting`
- Else → `stage = nurture`

```json
"checkpoints.enrich": "completed",
"files.enrichment": "briefs/enrichment-<slug>.md",
"flags.needs_board": false,
"last_updated": "<ISO>"
```

## Done when

- Enrichment brief written
- HubSpot updated or pending-upsert recorded
- Next stage selected
