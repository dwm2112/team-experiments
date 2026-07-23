# Stage — Surface (marketing-gtm)

Discover market opportunities and create HubSpot lead payloads. Initial feeders only: `search-specialist`, `startup-analyst`, `seo-planner` (see `leads/feeders.json`).

## Inputs

- Experiment: `experiments/002-marketing_team_workflow` or `$ARGUMENTS` path
- Optional topic / ICP hint in `$ARGUMENTS`
- Requires: `leads/feeders.json` allowlist
- Roles: [workflows/lib/roles.md](../../lib/roles.md)
- Template: [templates/opportunity-brief.md](../../../templates/opportunity-brief.md)

## Critical rules

1. Only dispatch roles listed in `leads/feeders.json` with `enabled: true`.
2. Subagents return structured HubSpot upsert payloads — orchestrator/n8n writes to HubSpot.
3. Write `briefs/opportunity-<slug>.md` before advancing.
4. Do not auto-qualify or send outreach in this stage.

## Steps

### 1. Load context

Read `AGENTS.md`, `state.json`, `leads/feeders.json`, and any ICP notes in `briefs/`.

### 2. Fan-out research (PARALLEL)

Dispatch enabled feeders (typically all three) with:

- Research topic / ICP
- Required return: opportunity candidates + HubSpot contact/company draft fields
- Schema:

```
OPPORTUNITY_SLUG: ...
TITLE: ...
SUMMARY: ...
ICP_FIT: high|medium|low
SOURCES: [urls]
HUBSPOT_CONTACT:
  email: ...
  firstname: ...
  lastname: ...
  company: ...
  lifecyclestage: lead
  lead_source_detail: agent-surface
  surfaced_by: agent:<role-id>
  opportunity_score: 1-100
  experiment_id: <experiment_id>
  pipeline_stage_internal: surface
  mql_criteria_version: v1
HUBSPOT_COMPANY: (optional)
NOTES: ...
```

### 3. Write opportunity brief

Slugify best candidate(s). For each kept opportunity, write `briefs/opportunity-<slug>.md` from the template. Append rejected candidates under `## Parked` with reasons.

### 4. HubSpot upsert (orchestrator)

Upsert contacts/companies via HubSpot API or hand payload to n8n. Record IDs in `state.hubspot`. Set custom properties from the schema above.

If HubSpot credentials are unavailable: write payloads to `leads/pending-upsert-<slug>.json` and note `flags.hubspot_pending = true` — still advance with brief written.

### 5. Update state

```json
"stage": "qualify_lead",
"checkpoints.surface": "completed",
"files.opportunity_brief": "briefs/opportunity-<slug>.md",
"last_updated": "<ISO>"
```

## Done when

- At least one opportunity brief exists
- HubSpot IDs recorded OR pending-upsert file written
- `checkpoints.surface = completed`
