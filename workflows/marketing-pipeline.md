# Marketing Pipeline — Master Router

Master entry point for the Marketing Team. Dual pipelines share roles, board protocol, and HubSpot as the lead/deal system of record:

| Pipeline | Key | Flow |
|----------|-----|------|
| Own GTM | `marketing-gtm` | `surface → qualify_lead → enrich → board_meeting? → nurture → handoff_or_close → retain_advocate` |
| Client delivery | `marketing-client` | `client_intake → qualify → brief → board_meeting → campaign_design → package → execute_hooks` |

Shared stage vocabulary (measurement lens): **Awareness → Acquisition → Activation → Revenue → Retention/Expansion → Referral/Advocacy**.

Blackboard files hold briefs, minutes, and campaign packages. **HubSpot** holds contacts, companies, deals, and lifecycle — never store lead records only in markdown.

## Commands

| Invocation | Meaning |
|------------|---------|
| `/team:marketing-pipeline gtm [experiments/<id>\|topic]` | Start or continue own-GTM pipeline |
| `/team:marketing-pipeline client [experiments/<id>\|brief]` | Start or continue client-delivery pipeline |
| `/team:marketing-pipeline status` | Print pipeline status |
| `/team:marketing-pipeline resume` | Continue from `state.stage` after a checkpoint |

Cursor flat equivalents: `/team-marketing-pipeline` with the same arguments.

Stage shortcuts (optional): `/team:marketing-surface`, `/team:marketing-qualify-lead`, `/team:marketing-board-meeting`, `/team:marketing-nurture`, `/team:marketing-client-intake`, `/team:marketing-package`.

## Critical behavioral rules

1. Execute stages **in order** for the selected pipeline. Do not skip, reorder, or merge across pipelines.
2. Each stage **must** write its blackboard files before the next begins. Read files — do not rely on context memory.
3. At every `PHASE CHECKPOINT`, **STOP** and wait for explicit user approval.
4. Halt on failure — present error; do not silently continue.
5. Resolve subagent names via [workflows/lib/roles.md](lib/roles.md).
6. Read [AGENTS.md](../AGENTS.md) before starting.
7. Subagents return HubSpot payloads; orchestrator or n8n upserts. Never put API keys in blackboard files.
8. Only feeders listed in `leads/feeders.json` may create new HubSpot contacts during `surface` (extend that file to add humans/agents).
9. Do not auto-send cold email or Upwork bids without human approve.

## Stage file map

### Shared

| Stage key | Workflow body |
|-----------|---------------|
| `board_meeting` | [marketing-board-meeting.md](marketing-board-meeting.md) |

### marketing-gtm

| Stage key | Workflow body |
|-----------|---------------|
| `surface` | [stages/marketing/surface.md](stages/marketing/surface.md) |
| `qualify_lead` | [stages/marketing/qualify-lead.md](stages/marketing/qualify-lead.md) |
| `enrich` | [stages/marketing/enrich.md](stages/marketing/enrich.md) |
| `nurture` | [stages/marketing/nurture.md](stages/marketing/nurture.md) |
| `handoff_or_close` | [stages/marketing/handoff-or-close.md](stages/marketing/handoff-or-close.md) |
| `retain_advocate` | [stages/marketing/retain-advocate.md](stages/marketing/retain-advocate.md) |

### marketing-client

| Stage key | Workflow body |
|-----------|---------------|
| `client_intake` | [stages/marketing/client-intake.md](stages/marketing/client-intake.md) |
| `qualify` | [stages/marketing/client-qualify.md](stages/marketing/client-qualify.md) |
| `brief` | [stages/marketing/client-brief.md](stages/marketing/client-brief.md) |
| `campaign_design` | [stages/marketing/campaign-design.md](stages/marketing/campaign-design.md) |
| `package` | [stages/marketing/campaign-package.md](stages/marketing/campaign-package.md) |
| `execute_hooks` | [stages/marketing/execute-hooks.md](stages/marketing/execute-hooks.md) |

## Command: status

Read `experiments/<id>/state.json` (id from `$ARGUMENTS` or sole in-progress marketing experiment). Print:

```
Pipeline Status — <marketing-gtm | marketing-client>
  Experiment:  <id>
  Status:      <status>
  Stage:       <stage>

  Checkpoints:
  [list from state.checkpoints]

  HubSpot: experiment_id / active contact ids from state.hubspot
  Files: [list from state.files]
```

Mark completed checkpoints with `[x]`, awaiting with `[~]`.

## Command: start

### 1. Resolve pipeline and experiment

Parse `$ARGUMENTS`:

- First token `gtm` → `pipeline = marketing-gtm`
- First token `client` → `pipeline = marketing-client`
- If omitted and `state.pipeline` exists → use it
- Else ask user which pipeline

Experiment:

- Default workspace for this team: `experiments/002-marketing_team_workflow` unless another `experiments/<id>` is given
- Create dirs if missing: `briefs/`, `campaigns/`, `analysis/`, `board/`, `leads/`, `intake/` (client)

### 2. Existing session

If `state.json` has `status: "in_progress"`:

```
Found in-progress marketing session for <id> at stage <stage> (pipeline <pipeline>).
1. Resume
2. Start fresh (archive then re-init)
```

### 3. Run pipeline

Enter **Stage Routing** below from `state.stage`.

## Command: resume

Read `state.json`. Continue from `state.stage` per Stage Routing. If checkpoint was awaiting approval, apply the user's choice then advance.

## Stage Routing — marketing-gtm

### `surface`

Execute [stages/marketing/surface.md](stages/marketing/surface.md). On completion → `qualify_lead`.

### `qualify_lead`

Execute [stages/marketing/qualify-lead.md](stages/marketing/qualify-lead.md).  
**PAUSE** at go/no-go checkpoint.  
On GO → `enrich`. On NO-GO → archive lead note; stop or return to surface.

### `enrich`

Execute [stages/marketing/enrich.md](stages/marketing/enrich.md). On completion:

- If `state.flags.needs_board` or high-stakes flags set → `board_meeting`
- Else → `nurture`

### `board_meeting`

Execute [marketing-board-meeting.md](marketing-board-meeting.md).  
**PAUSE** at decision approval.  
On APPROVE → `nurture` (GTM) or `campaign_design` (client).

### `nurture`

Execute [stages/marketing/nurture.md](stages/marketing/nurture.md).  
**PAUSE** before any outbound send.  
On APPROVE send → `handoff_or_close` when SQL/opportunity criteria met; else remain in nurture.

### `handoff_or_close`

Execute [stages/marketing/handoff-or-close.md](stages/marketing/handoff-or-close.md). On closed-won → `retain_advocate`. On closed-lost → complete with note.

### `retain_advocate`

Execute [stages/marketing/retain-advocate.md](stages/marketing/retain-advocate.md). On completion → `status = complete` (or keep open for advocacy loops).

## Stage Routing — marketing-client

### `client_intake`

Execute [stages/marketing/client-intake.md](stages/marketing/client-intake.md). On completion → `qualify`.

### `qualify`

Execute [stages/marketing/client-qualify.md](stages/marketing/client-qualify.md).  
**PAUSE** at go/no-go.  
On GO → `brief`.

### `brief`

Execute [stages/marketing/client-brief.md](stages/marketing/client-brief.md). On completion → `board_meeting`.

### `board_meeting`

Execute [marketing-board-meeting.md](marketing-board-meeting.md).  
**PAUSE** at decision approval.  
On APPROVE → `campaign_design`.

### `campaign_design`

Execute [stages/marketing/campaign-design.md](stages/marketing/campaign-design.md). On completion → `package`.

### `package`

Execute [stages/marketing/campaign-package.md](stages/marketing/campaign-package.md).  
**PAUSE** at deliver checkpoint.  
On DELIVER → `execute_hooks` (optional) or `status = complete`.

### `execute_hooks`

Execute [stages/marketing/execute-hooks.md](stages/marketing/execute-hooks.md). Hand off to n8n/SocialClaw notes only — do not implement client sites. Then `status = complete`.

## Anti-loop guards

1. **Repeated blocker** — same blocker twice → escalate to human.
2. **No cyclic handoff** — do not send work back to a stage that already resolved the same issue.
3. **Board caps** — max 5 agents, max 2 debate rounds.
4. **No silent outbound** — nurture drafts require human approve before send.
5. **Feeder allowlist** — reject surface from unknown agents/humans not in `leads/feeders.json`.

## State file

Path: `experiments/<id>/state.json`

Required fields:

```json
{
  "experiment_id": "<id>",
  "status": "in_progress",
  "pipeline": "marketing-gtm",
  "stage": "surface",
  "checkpoints": {},
  "flags": {
    "qualify_verdict": null,
    "decision_approved": false,
    "needs_board": false,
    "nurture_send_approved": false,
    "package_delivered": false
  },
  "hubspot": {
    "experiment_id_property": "<id>",
    "contact_ids": [],
    "company_ids": [],
    "deal_ids": []
  },
  "files": {},
  "pending_board_proposal": null,
  "board_decision_slug": null,
  "started_at": "<ISO>",
  "last_updated": "<ISO>"
}
```

Update `last_updated` on every write. Maintain `files` and `hubspot` maps as artifacts appear.
