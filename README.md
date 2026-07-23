# team-experiments

Monorepo of experiments for building a freelance company made of AI agent teams.

Postings from Upwork (and similar RFPs) are the primary training substrate. The goal is to learn and develop **Agents**, **Skills**, and **Workflows** — composed from the [wshobson/agents](https://github.com/wshobson/agents) marketplace — into a repeatable intake-to-design pipeline.

## Intake-to-Design Pipeline

```
RFP intake → qualify → proposal → analyze → board-meeting (decision gate) → design → package
```

The pipeline **stops at a design package** (client proposal + requirements + decision record + technical design). Implementation is out of scope for this workflow.

### Commands (either harness)

| Stage | Claude Code | Cursor |
|-------|-------------|--------|
| Master router | `/team:ship-design` | `/team-ship-design` |
| Intake | `/team:intake` | `/team-intake` |
| Qualify | `/team:qualify` | `/team-qualify` |
| Proposal | `/team:proposal` | `/team-proposal` |
| Analyze | `/team:analyze` | `/team-analyze` |
| Board meeting | `/team:board-meeting` | `/team-board-meeting` |
| Design | `/team:design` | `/team-design` |
| Package | `/team:package` | `/team-package` |

### Quick start

1. Drop an RFP into a new experiment (or use `experiments/001-fullstack-webapp/`).
2. Run the master router:
   - Claude Code: `/team:ship-design experiments/001-fullstack-webapp`
   - Cursor: `/team-ship-design experiments/001-fullstack-webapp`
3. Approve the three human checkpoints: **qualify (go/no-go)**, **board decision**, **deliver package**.

## Layout

```
workflows/          Canonical portable workflow bodies (source of truth)
  stages/           One file per pipeline stage
  lib/roles.md      Role → per-harness subagent mapping
templates/          Output document templates
experiments/        Per-RFP experiment workspaces (blackboard + state.json)
.claude/commands/   Claude Code slash-command shims
.cursor/commands/   Cursor command shims
AGENTS.md           Shared roster, conventions, harness notes
```

Portability: real logic lives once under `workflows/`. Harness shims are thin wrappers that say "execute `workflows/<x>.md`".

## Prerequisites

**Cursor:** Specialist agents are available as Task subagents — no install needed.

**Claude Code:**

```
/plugin marketplace add wshobson/agents
/plugin install comprehensive-review backend-development cloud-infrastructure database-design business-analytics before-you-build conductor ship-mate full-stack-orchestration
```

## Mechanics (from wshobson orchestrators)

1. **Blackboard files, not context memory** — each stage writes numbered markdown; the next stage reads the file.
2. **`state.json` for resumable runs** — checkpoints, iteration caps, anti-loop guards.
3. **Isolated subagents** — Task tool; the prompt is the only channel; paste prior file contents into each prompt.
4. **Human checkpoints** — go/no-go, decision approval, deliver.

## First experiment

`experiments/001-fullstack-webapp/` is seeded from a vague-ask full-stack web app posting (Stripe/QuickBooks, PHP/AngularJS signals). See `upwork-proposal.md` for the original proposal template that was generalized into `templates/proposal.md`.
