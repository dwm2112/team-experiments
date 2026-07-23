# Intake-to-Design — Master Router

Master pipeline entry point. Routes an RFP through:

`intake → qualify → proposal → analyze → board_meeting → design → package`

Modeled on ship-mate `/ship`: `state.json`, stage routing, human checkpoints, anti-loop guards.

**Stops at the design package.** No implementation.

## Commands

| Invocation | Meaning |
|------------|---------|
| `/team:ship-design experiments/<id-or-rfp>` | Start or continue pipeline for that experiment / RFP path |
| `/team:ship-design status` | Print pipeline status |
| `/team:ship-design resume` | Continue from `state.stage` after a checkpoint |

Cursor flat equivalents: `/team-ship-design` with the same arguments.

## Critical behavioral rules

1. Execute stages **in order**. Do not skip, reorder, or merge.
2. Each stage **must** write its blackboard files (see stage docs) before the next begins. Read files — do not rely on context memory.
3. At every `PHASE CHECKPOINT`, **STOP** and wait for explicit user approval.
4. Halt on failure — present error; do not silently continue.
5. Resolve subagent names via [workflows/lib/roles.md](lib/roles.md).
6. Read [AGENTS.md](../AGENTS.md) before starting.
7. Never enter a separate "plan mode" that replaces this router — this file **is** the plan; execute it.

## Stage file map

| Stage key | Workflow body |
|-----------|---------------|
| `intake` | [stages/intake.md](stages/intake.md) |
| `qualify` | [stages/qualify.md](stages/qualify.md) |
| `proposal` | [stages/proposal.md](stages/proposal.md) |
| `analyze` | [stages/analyze.md](stages/analyze.md) |
| `board_meeting` | [board-meeting.md](board-meeting.md) |
| `design` | [stages/design.md](stages/design.md) |
| `package` | [stages/package.md](stages/package.md) |

## Command: status

Read `experiments/<id>/state.json` (id from `$ARGUMENTS` or sole in-progress experiment). Print:

```
Pipeline Status — intake-to-design
  Experiment:  <id>
  Status:      <status>
  Stage:       <stage>

  Checkpoints:
  [ ] intake
  [ ] qualify        (verdict: …)
  [ ] proposal
  [ ] analyze
  [ ] board_meeting  (decision_approved: …)
  [ ] design
  [ ] package

  Files: [list from state.files]
```

Mark completed checkpoints with `[x]`, awaiting with `[~]`.

## Command: start

**When `$ARGUMENTS` is an RFP path, pasted intent, or `experiments/<id>` without `resume`/`status`:**

### 1. Resolve experiment

- If path is `experiments/<id>` and exists → use it
- Else execute **intake** stage first ([stages/intake.md](stages/intake.md)) with the RFP argument

### 2. Existing session

If `state.json` has `status: "in_progress"`:

```
Found in-progress session for <id> at stage <stage>.
1. Resume
2. Start fresh (archive then re-intake)
```

### 3. Run pipeline

Enter **Stage Routing** below from `state.stage` (after intake, stage is `qualify`).

## Command: resume

Read `state.json`. Continue from `state.stage` per Stage Routing. If checkpoint was awaiting approval, the user message after approval should include their choice — apply it then advance.

## Stage Routing

After reading state, execute the matching stage body in full, then continue unless the stage ends in a checkpoint pause.

### `intake`

Execute [stages/intake.md](stages/intake.md). On completion → `qualify`.

### `qualify`

Execute [stages/qualify.md](stages/qualify.md).  
**PAUSE** at go/no-go checkpoint.  
On GO → `proposal`. On NO-GO → stop (archived).

### `proposal`

Execute [stages/proposal.md](stages/proposal.md). On completion → `analyze`.

### `analyze`

Execute [stages/analyze.md](stages/analyze.md). On completion → `board_meeting`.

### `board_meeting`

Execute [board-meeting.md](board-meeting.md) with proposal = `state.pending_board_proposal` or `$ARGUMENTS` override.  
**PAUSE** at decision approval.  
On APPROVE → `design`.

### `design`

Execute [stages/design.md](stages/design.md). On completion → `package`.

### `package`

Execute [stages/package.md](stages/package.md).  
**PAUSE** at deliver checkpoint.  
On DELIVER → `status = complete`.

## Anti-loop guards

1. **Repeated blocker** — same blocker twice → escalate to human.
2. **No cyclic handoff** — do not send work back to a stage that already resolved the same issue.
3. **Board caps** — max 5 agents, max 2 debate rounds (see board-meeting.md).
4. **No implementation** — if asked to code, refuse within this router; point to a future pipeline.

## State file

Path: `experiments/<id>/state.json`

Update `last_updated` on every write. Maintain `files` map as artifacts appear. Canonical shape is created in intake and extended by later stages.
