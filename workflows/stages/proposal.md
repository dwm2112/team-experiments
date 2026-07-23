# Stage 2 — Proposal

Draft client clarifying questions and an Upwork bid from the proposal template. Uses Product/Analyst role.

## Inputs

- Experiment with `checkpoints.qualify = "completed"` and `flags.qualify_verdict` in {`go`, `go-with-conditions`}
- Files: `intake/00-rfp.md`, `intake/01-qualification.md`
- Template: [templates/proposal.md](../../templates/proposal.md)

## Critical rules

1. Do not invent portfolio stories — leave placeholders the human must fill, OR fill only from facts the human provides in this session.
2. Ask clarifying questions **about the client**, not about our internal process.
3. Write both output files before completing the stage.
4. Resolve Product/Analyst via [workflows/lib/roles.md](../lib/roles.md).

## Steps

### 1. Load context

Read RFP, qualification memo, `templates/proposal.md`, `AGENTS.md`.

### 2. Clarifying questions (Product/Analyst)

Dispatch Product/Analyst (or run inline as chairman if dispatch unavailable) with a self-contained prompt containing the RFP + qualification extracts.

Require output: 4–6 sharp client questions, prioritized, split into:

- Business-context (why / workflow / success)
- Stack / data / integration (their system)

Write `intake/02-clarifying-questions.md`:

```markdown
# Clarifying Questions: [id]

## Priority ask (put 1–2 into the proposal)

1. …
2. …

## Backup questions (call / follow-up)

3. …
4. …
…
```

### 3. Draft proposal

Using `templates/proposal.md`, produce a draft bid:

- Fill structural fields from the RFP (surface ask, real ask, rate range if stated, location constraints).
- For stories / honesty / name / exact rate / hours: use `[PLACEHOLDER]` unless the human supplied values this session.
- Embed the top 1–2 priority questions.

Write `intake/02-proposal.md` (paste-ready section + short notes on what the human must still fill).

### 4. Update state

```json
"checkpoints.proposal": "completed",
"stage": "analyze",
"files.clarifying_questions": "intake/02-clarifying-questions.md",
"files.proposal": "intake/02-proposal.md",
"last_updated": "<ISO>"
```

### 5. Hand off

```
Proposal draft ready:
- experiments/<id>/intake/02-clarifying-questions.md
- experiments/<id>/intake/02-proposal.md

Human: fill remaining placeholders before sending on Upwork.
Next: Stage 3 analyze (/team:analyze).
```
