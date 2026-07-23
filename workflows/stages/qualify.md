# Stage 1 — Qualify (go / no-go)

Run a bid pre-mortem on the captured RFP using the **before-you-build** risk checklist. Halt for human go/no-go.

## Inputs

- Active experiment: from `$ARGUMENTS` (`experiments/<id>`) or `state.json` in cwd experiment
- Requires: `intake/00-rfp.md`

## Critical rules

1. Read the RFP file; do not rely on memory.
2. Write `intake/01-qualification.md` before checkpoint.
3. **STOP** at checkpoint — do not auto-start proposal.
4. Prefer loading the `before-you-build` skill if available; otherwise apply the checklist below inline.

## before-you-build checklist (bid framing)

Score for **bidding and engagement fit**, not "should this product exist forever":

- **Demand / fit:** Does this engagement match skills, availability, and rate we can honestly claim?
- **Positioning:** Can we state in one sentence why we are the right hire for *this* post?
- **Monetization:** Is rate × hours × duration credible vs stated scope? Hidden unpaid discovery?
- **Retention / duration:** Is this a short spike we will abandon, or a 6+ month fit?
- **Trust:** Location filters, NDAs, credential theater, "AI paste" hostility — can we clear them honestly?
- **Distribution:** N/A for single bid — replace with **win probability** (competition signals, vague vs clear ask).
- **Adoption / scope creep:** Will vague ask become unbounded unpaid clarification?

## Steps

### 1. Load context

Read `AGENTS.md`, `intake/00-rfp.md`, and current `state.json`.

### 2. Produce qualification memo

Write `intake/01-qualification.md`:

```markdown
# Qualification: [experiment id]

**RFP:** intake/00-rfp.md
**Date:** [ISO]

## Risk verdict

[Low | Medium | High] — [one sentence why]

## Main assumption

[Single assumption most likely to break the bid or engagement]

## Evidence to find first

[Smallest useful signal before investing more time — e.g. client reply to Q1]

## Do next

[One concrete validation step OR reduced bid scope]

## Delay

[What not to do yet — e.g. full architecture, custom sample app]

## Checklist scores

| Lens | Score (1–5) | Notes |
|------|-------------|-------|
| Demand / fit | | |
| Positioning | | |
| Monetization / rate vs scope | | |
| Duration fit | | |
| Trust / filters | | |
| Win probability | | |
| Scope-creep risk | | |

## Recommended verdict

**GO** | **NO-GO** | **GO-WITH-CONDITIONS**

Conditions (if any):
- …
```

### 3. Update state

```json
"stage": "qualify",
"checkpoints.qualify": "awaiting_approval",
"files.qualification": "intake/01-qualification.md",
"last_updated": "<ISO>"
```

### 4. PHASE CHECKPOINT — User approval required

Present the memo summary and ask (use AskQuestion / AskUserQuestion if available):

```
Qualification ready: experiments/<id>/intake/01-qualification.md
Recommended: [GO | NO-GO | GO-WITH-CONDITIONS]

1. GO — proceed to proposal
2. NO-GO — archive; record reason; stop pipeline
3. REVISE — tell me what to change in the memo
```

- **GO:** set `flags.qualify_verdict = "go"`, `checkpoints.qualify = "completed"`, `stage = "proposal"`.
- **NO-GO:** set `flags.qualify_verdict = "no-go"`, `status = "archived"`, write reason into qualification file, **stop**.
- **REVISE:** edit memo, re-checkpoint.

Do **not** proceed to Stage 2 until GO.
