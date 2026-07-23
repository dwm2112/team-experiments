# Stage 3 — Analyze

Produce the requirements / analysis package. Optional parallel specialist discovery when the RFP names heavy domains (payments, cloud, security).

## Inputs

- `intake/00-rfp.md`, `intake/01-qualification.md`, `intake/02-clarifying-questions.md`, `intake/02-proposal.md` (proposal optional if analyze run early — prefer all)
- Template: [templates/requirements.md](../../templates/requirements.md)

## Critical rules

1. Write `analysis/03-requirements.md` before completing.
2. End with a **Recommended Decision Gate** — one architecture/approach question for the board.
3. Resolve roles via [workflows/lib/roles.md](../lib/roles.md).
4. Paste prior file contents into every subagent prompt.

## Steps

### 1. Load context

Read all intake files + AGENTS.md.

### 2. Core requirements (Product/Analyst)

Dispatch Product/Analyst to draft requirements following `templates/requirements.md`.

Save draft to `analysis/03-requirements.md`.

### 3. Optional parallel discovery

If the RFP clearly involves any of these, fan out **in parallel** (same message, multiple Task calls):

| Signal | Role | Ask |
|--------|------|-----|
| Payments / Stripe / QuickBooks / recon | `backend-architect` | Integration risks, sandbox→prod path, data ownership |
| Hosting / scale / multi-tenant | `cloud-architect` | Deploy shape, cost, constraints |
| Auth, PII, PCI-ish, "security" | `security-auditor` | Trust boundaries, non-negotiables |

Each returns a short markdown brief. Chairman merges relevant bullets into Requirements sections (Constraints, Risks, Dependencies) — do not leave discovery only in chat.

If no strong signals, skip fan-out.

### 4. Decision gate sentence

Ensure `## Recommended Decision Gate` names **one** irreversible or high-cost approach question, e.g.:

- Extend legacy PHP/AngularJS in place vs side-car modern API
- QuickBooks as source of truth vs Stripe-first with periodic sync

Copy that sentence into `state.json` as `pending_board_proposal` (string).

### 5. Update state

```json
"checkpoints.analyze": "completed",
"stage": "board_meeting",
"files.requirements": "analysis/03-requirements.md",
"pending_board_proposal": "<decision gate sentence>",
"last_updated": "<ISO>"
```

### 6. Hand off

```
Analysis complete: experiments/<id>/analysis/03-requirements.md
Decision gate: "<pending_board_proposal>"
Next: Stage 4 board-meeting on that proposal.
```
