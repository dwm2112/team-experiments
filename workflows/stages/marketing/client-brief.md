# Stage — Client Campaign Brief (marketing-client)

Turn the raw client intake into a structured campaign brief for the marketing board.

## Inputs

- Requires: `intake/00-client-brief.md`, `intake/01-qualification.md`
- Roles: `growth-marketer` + `content-strategist` (parallel drafts OK; chairman merges)
- Template: [templates/campaign-brief.md](../../../templates/campaign-brief.md)

## Steps

### 1. Draft brief

Write `briefs/campaign-brief.md` covering:

- Objective and North Star metric
- Audience / ICP / personas
- Positioning and offer
- Channel hypothesis (awareness → acquisition)
- Content pillars
- Measurement (UTM, HubSpot lifecycle, GA4 events)
- Constraints and out of scope
- Decision Gate sentence for the board (`pending_board_proposal`)

### 2. Optional HubSpot

Create/update client company + primary contact; set `pipeline_stage_internal: brief`.

### 3. Update state

```json
"stage": "board_meeting",
"checkpoints.brief": "completed",
"files.campaign_brief": "briefs/campaign-brief.md",
"pending_board_proposal": "<Decision Gate sentence>",
"last_updated": "<ISO>"
```

## Done when

- Campaign brief written with an explicit Decision Gate
- State points board at that proposal
