# Stage 6 — Package

Consolidate proposal + requirements + decision + design into the terminal **design package**. Human checkpoint: deliver.

## Inputs

- Requires completed: proposal (or note if skipped), requirements, decision, design
- Files: `intake/02-proposal.md`, `analysis/03-requirements.md`, `analysis/04-decision.md`, `design/05-design.md`

## Critical rules

1. Do not start implementation.
2. Write `design/design-package.md` before checkpoint.
3. **STOP** for human deliver approval.

## Steps

### 1. Load all artifacts

Read the four (or more) blackboard files. If proposal placeholders remain, call them out in a "Human still must fill" section — do not invent stories.

### 2. Write design package

Write `design/design-package.md`:

```markdown
# Design Package: [title]

**Experiment:** experiments/<id>/
**Generated:** [ISO]
**Pipeline status:** ready for human deliver

## Executive summary

[One short paragraph: what we would build and the locked approach]

## Client-facing proposal (draft)

[Embed or link intake/02-proposal.md — note unfilled placeholders]

## Clarifying questions

[Link or embed top questions]

## Requirements summary

[Problem, in/out scope, acceptance criteria — condensed from 03-requirements]

## Decision record

[Consensus, dissents, conditions — from 04-decision]

## Technical design summary

[Data model + API approach + integrations + key risks — condensed from 05-design]

## Rough estimate

[From design]

## Artifact index

| Artifact | Path |
|----------|------|
| RFP | intake/00-rfp.md |
| Qualification | intake/01-qualification.md |
| Questions | intake/02-clarifying-questions.md |
| Proposal | intake/02-proposal.md |
| Requirements | analysis/03-requirements.md |
| Decision | analysis/04-decision.md |
| Board minutes | board/minutes-*.md |
| Design | design/05-design.md |

## Out of scope for this package

- Implementation, tests, deployment
- Sending the Upwork bid (human action)
```

### 3. Update state (pre-checkpoint)

```json
"checkpoints.package": "awaiting_approval",
"files.design_package": "design/design-package.md",
"last_updated": "<ISO>"
```

### 4. PHASE CHECKPOINT — Deliver

```
Design package ready: experiments/<id>/design/design-package.md

1. DELIVER — mark pipeline complete
2. REVISE — specify what to change
3. PAUSE — leave awaiting_approval
```

- **DELIVER:** `flags.package_delivered = true`, `checkpoints.package = "completed"`, `status = "complete"`, `stage = "complete"`.
- **REVISE:** edit package/upstream as needed; re-checkpoint.
- **PAUSE:** stop.

### 5. Completion banner

```
Intake-to-design complete: experiments/<id>/
Terminal artifact: design/design-package.md
Implementation is out of scope for this pipeline.
```
