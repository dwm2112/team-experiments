# Stage — Client Intake (marketing-client)

Capture a client marketing RFP or campaign brief into the experiment workspace.

## Inputs

- `$ARGUMENTS`: path to client brief/RFP, pasted text, or existing `experiments/<id>/`
- Template pattern mirrors engineering intake

## Critical rules

1. Write files before claiming done.
2. Do not skip to qualify/board in this stage.
3. Create marketing dirs: `intake/`, `briefs/`, `campaigns/`, `analysis/`, `board/`, `leads/`

## Steps

### 1. Resolve experiment

Prefer `experiments/002-marketing_team_workflow` or create `NNN-marketing-<slug>`.

### 2. Capture raw brief

Write `intake/00-client-brief.md`:

```markdown
# Client Brief — [title]

**Captured:** [ISO]
**Source:** [path or pasted]

## Raw text
[verbatim]

## Structured extract
| Field | Value | Confidence |
|-------|-------|------------|
| Client / brand | | |
| Objective | | |
| Audience / ICP | | |
| Budget | | |
| Timeline | | |
| Channels named | | |
| Deliverables | | |
| Brand constraints | | |
| Success metrics | | |
| Red flags | | |
```

### 3. Initialize or update state.json

```json
{
  "experiment_id": "<id>",
  "status": "in_progress",
  "pipeline": "marketing-client",
  "stage": "qualify",
  "checkpoints": { "client_intake": "completed" },
  "flags": {
    "qualify_verdict": null,
    "decision_approved": false,
    "package_delivered": false
  },
  "hubspot": {
    "experiment_id_property": "<id>",
    "contact_ids": [],
    "company_ids": [],
    "deal_ids": []
  },
  "files": {
    "client_brief_raw": "intake/00-client-brief.md"
  },
  "started_at": "<ISO>",
  "last_updated": "<ISO>"
}
```

Optional: create HubSpot company for the client with `surfaced_by: human|intake` and `experiment_id`.

## Done when

- Raw brief written
- State initialized with `pipeline: marketing-client`
- Stage set to `qualify`
