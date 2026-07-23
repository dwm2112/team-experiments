# Stage 0 — Intake

Capture a raw RFP into an experiment workspace and initialize resumable state.

## Inputs

- `$ARGUMENTS`: path to an RFP markdown/text file, OR pasted RFP text, OR path to an existing `experiments/<id>/` to re-intake
- Optional flags: `--id <NNN-slug>` to force experiment id

## Critical rules

1. Write output files before claiming the stage is done.
2. Do not skip to qualify/proposal in this stage.
3. Read [AGENTS.md](../../AGENTS.md) and [workflows/lib/roles.md](../lib/roles.md) for conventions.

## Steps

### 1. Resolve experiment id

- If `$ARGUMENTS` is an existing `experiments/<id>/` directory: use that id.
- Else if `--id` provided: use it.
- Else: invent `NNN-short-slug` from RFP title/theme (next free numeric prefix under `experiments/`).

Create directories if missing:

```
experiments/<id>/
  intake/
  analysis/
  design/
  board/
```

### 2. Capture raw RFP

Write `experiments/<id>/intake/00-rfp.md`:

```markdown
# RFP — [title or first-line summary]

**Captured:** [ISO timestamp]
**Source:** [path or "pasted"]

## Raw text

[verbatim RFP contents]
```

If `$ARGUMENTS` is a file path, copy its contents into the Raw text section. If pasted, use the paste.

### 3. Extract structured fields

Append to `00-rfp.md` (or write a short structured section at the top after the header):

```markdown
## Structured extract

| Field | Value | Confidence |
|-------|-------|------------|
| Title / one-liner | … | H/M/L |
| Budget / rate | … | … |
| Hours / duration | … | … |
| Location constraints | … | … |
| Stated stack | … | … |
| Integrations named | … | … |
| Deliverables | … | … |
| Vague-ask signals | … | … |
| Red flags | … | … |
| Opportunity signals | … | … |
```

Do **not** invent client facts. Mark unknowns as `unknown`.

### 4. Initialize state.json

Write `experiments/<id>/state.json` (overwrite only if user confirms start-fresh when an in-progress session exists):

```json
{
  "experiment_id": "<id>",
  "status": "in_progress",
  "pipeline": "intake-to-design",
  "stage": "qualify",
  "checkpoints": {
    "intake": "completed",
    "qualify": "pending",
    "proposal": "pending",
    "analyze": "pending",
    "board_meeting": "pending",
    "design": "pending",
    "package": "pending"
  },
  "flags": {
    "qualify_verdict": null,
    "decision_approved": false,
    "package_delivered": false
  },
  "files": {
    "rfp": "intake/00-rfp.md"
  },
  "board_decision_slug": null,
  "started_at": "<ISO>",
  "last_updated": "<ISO>"
}
```

If `state.json` already exists with `status: "in_progress"`, ask:

1. Resume (do not rewrite intake)
2. Start fresh (archive prior files under `archive/<timestamp>/` then re-init)

### 5. Hand off

Print:

```
Intake complete: experiments/<id>/
RFP: experiments/<id>/intake/00-rfp.md
Next: run Stage 1 qualify (/team:qualify or /team-qualify) or continue via master router.
```

Update `last_updated`. Set stage pointer to `qualify`.
