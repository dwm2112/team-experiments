# Marketing Board Meeting — Structured Multi-Agent Debate

Reusable marketing/sales debate sub-workflow. The main session is **CHAIRMAN**: board members cannot talk peer-to-peer. All messages flow through the minutes blackboard file.

Implements the blackboard + round protocol (independent positions → anonymized rebuttal → optional revision → consensus-with-dissent). Same mechanics as [board-meeting.md](board-meeting.md); roster and decision artifacts are marketing-scoped.

## Inputs

- `$ARGUMENTS`: proposal / GTM or campaign question to debate (required), OR use `state.pending_board_proposal`
- Experiment root: `experiments/<id>/` from `$ARGUMENTS` path prefix or active state
- Requires (paste into every prompt when present):
  - GTM: `briefs/opportunity-<slug>.md` and/or qualify notes
  - Client: `briefs/campaign-brief.md` and intake qualification
- Roles: [workflows/lib/roles.md](lib/roles.md)
- Template: [templates/marketing-minutes.md](../templates/marketing-minutes.md)
- Decision template: [templates/marketing-decision-record.md](../templates/marketing-decision-record.md)

## Caps (hard)

- **Max 5 agents** (default roster below)
- **Max 2 debate rounds** after Round 0 (Round 1 required; Round 2 only if material disagreement)
- No subagent nesting — chairman is sole conductor

## Default roster

Resolve each role ID via `lib/roles.md`:

| Seat | Role ID | Special mandate |
|------|---------|-----------------|
| 1 | `growth-marketer` | Domain: awareness / acquisition / CAC / channels |
| 2 | `content-strategist` | Domain: messaging / SEO / content |
| 3 | `sales-lead` | Domain: activation → revenue / handoff SLAs |
| 4 | `revops-analyst` | Domain: lifecycle / scoring / measurement |
| 5 | `skeptic` | **DEVIL'S ADVOCATE** — must argue strongest case AGAINST the proposal every round |

## Critical rules

1. Round 0 is **blind** — each agent gets ONLY the proposal (+ brief context), never other positions.
2. Round 1 identities are **anonymized** as Member A/B/C/D/E in the prompt (map privately in chairman notes).
3. Append each response **verbatim** to the minutes file before the next round.
4. Never discard minority views in the Decision section.
5. Write `analysis/marketing-decision.md` and **STOP** for human approval before campaign execution or major channel/budget shifts.
6. HubSpot is the lead/deal SOR — board minutes do **not** store lead records; note HubSpot IDs in the decision if relevant.

## When to run

- **High-stakes only:** new ICP, channel bet, pricing/packaging, major campaign budget, or client strategy fork.
- Skip for routine nurture drafts and single-lead qualify (those use stage checkpoints instead).

## Setup

1. Slugify the proposal into `board_decision_slug` (e.g. `linkedin-vs-seo-bet`).
2. Create `board/minutes-<slug>.md` from `templates/marketing-minutes.md`:
   - Fill Proposal, Roster, date, experiment id
3. Update `state.json`:
   - `stage`: `board_meeting`
   - `board_decision_slug`: `<slug>`
   - `files.minutes`: `board/minutes-<slug>.md`

## Round 0 — Independent positions (PARALLEL)

Dispatch all five agents **in one turn** (multiple Task/Agent calls). Each prompt must include:

- Role mandate (+ DEVIL'S ADVOCATE text for `skeptic`)
- Full proposal text
- Full opportunity or campaign brief (or condensed — include Decision Gate, constraints, budget, ICP)
- Required return schema:

```
RECOMMENDATION: adopt | adopt-with-changes | reject
TOP 3 RISKS or objections (in your domain):
1. ...
2. ...
3. ...
KEY ASSUMPTION: ...
CONFIDENCE: 1-5
```

Append each under `## Round 0` as `### Member X (role)`.

## Round 1 — Cross-examination (PARALLEL)

Concatenate all Round 0 positions into the minutes (already there). Re-dispatch all five **in parallel** with:

- Anonymized Round 0 text (Member A–E only — strip role names from the shared packet)
- Private reminder of *their own* prior recommendation
- Instructions:

```
1. Name the single strongest objection to EACH other position
2. Defend OR revise your recommendation and confidence
3. State what evidence would change your mind (falsifier)
```

Devil's advocate must still argue against the leading consensus if one is forming.

Append under `## Round 1`.

## Round 2 — Only if material disagreement

Material disagreement = final recommendations still split across adopt vs reject, or adopt-with-changes with incompatible changes.

If disagreement remains: one more parallel revision round (same anonymization). Cap here — do not run Round 3.

If recommendations converged: skip Round 2; note "Round 2 skipped — no material disagreement" in minutes.

## Consensus + Dissent (chairman)

As chairman (no extra agent required), synthesize into minutes `## Decision` and write `analysis/marketing-decision.md` using `templates/marketing-decision-record.md`:

- **CONSENSUS RECOMMENDATION** — confidence-weighted; weight domain experts higher on their own domain (e.g. revops on scoring, growth on CAC)
- **VOTE TALLY**
- **RECORDED DISSENTS** — every minority view + rationale
- **OPEN QUESTIONS / conditions**
- **Implications for Campaign / GTM** — what nurture, content, or HubSpot lifecycle must assume

Update state:

```json
"checkpoints.board_meeting": "awaiting_approval",
"files.decision": "analysis/marketing-decision.md",
"flags.decision_approved": false,
"last_updated": "<ISO>"
```

## PHASE CHECKPOINT — Approve decision

```
Marketing board decision ready:
- board/minutes-<slug>.md
- analysis/marketing-decision.md

Consensus: [summary]

1. APPROVE — proceed to next stage (campaign_design / nurture / package per pipeline)
2. REVISE — adjust decision or re-open one round with guidance
3. REJECT — stop or reframe proposal
```

- **APPROVE:** `flags.decision_approved = true`, `checkpoints.board_meeting = "completed"`, advance `stage` per master router
- **REVISE:** apply edits or limited re-debate; re-checkpoint
- **REJECT:** do not enter campaign execution stages

Do **not** start campaign package execution or paid spend until APPROVE.
