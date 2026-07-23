# team-experiments

Monorepo of experiments for building a freelance company made of AI agent teams.

Postings from Upwork (and similar RFPs) are a primary training substrate for the engineering design pipeline. The Marketing Team adds own-GTM and client-delivery pipelines with HubSpot as the lead CRM. The goal is to learn and develop **Agents**, **Skills**, and **Workflows** — composed from the [wshobson/agents](https://github.com/wshobson/agents) marketplace — into repeatable team pipelines.

## Pipelines

### Intake-to-Design (engineering)

```
RFP intake → qualify → proposal → analyze → board-meeting (decision gate) → design → package
```

Stops at a **design package**. Implementation is out of scope.

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

### Marketing Team (GTM + client delivery)

```
GTM:    surface → qualify_lead → enrich → board? → nurture → handoff → retain
Client: intake → qualify → brief → board → campaign_design → package → execute_hooks
```

Leads/deals live in **HubSpot**. Briefs, minutes, and packages live on the experiment blackboard.

| Stage | Claude Code | Cursor |
|-------|-------------|--------|
| Master router | `/team:marketing-pipeline` | `/team-marketing-pipeline` |
| Surface (GTM) | `/team:marketing-surface` | `/team-marketing-surface` |
| Qualify lead | `/team:marketing-qualify-lead` | `/team-marketing-qualify-lead` |
| Marketing board | `/team:marketing-board-meeting` | `/team-marketing-board-meeting` |
| Nurture | `/team:marketing-nurture` | `/team-marketing-nurture` |
| Client intake | `/team:marketing-client-intake` | `/team-marketing-client-intake` |
| Campaign package | `/team:marketing-package` | `/team-marketing-package` |

Workspace: [`experiments/002-marketing_team_workflow/`](experiments/002-marketing_team_workflow/).

### Quick start — engineering

1. Drop an RFP into a new experiment (or use `experiments/001-fullstack-webapp/`).
2. Run `/team:ship-design experiments/001-fullstack-webapp` (Cursor: `/team-ship-design`).
3. Approve checkpoints: **qualify**, **board decision**, **deliver package**.

### Quick start — marketing

1. Create HubSpot custom properties ([checklist](experiments/002-marketing_team_workflow/leads/hubspot-properties.md)).
2. GTM: `/team:marketing-pipeline gtm experiments/002-marketing_team_workflow`
3. Or client: `/team:marketing-pipeline client` with a brief path.
4. Approve checkpoints: **qualify**, **board** (when used), **nurture send**, **package deliver**.

## Layout

```
workflows/          Canonical portable workflow bodies (source of truth)
  stages/           Engineering stages
  stages/marketing/ Marketing stages
  lib/roles.md      Role → per-harness subagent mapping
templates/          Output document templates
experiments/        Per-experiment workspaces (blackboard + state.json)
.claude/commands/   Claude Code slash-command shims
.cursor/commands/   Cursor command shims
AGENTS.md           Shared roster, conventions, harness notes
```

Portability: real logic lives once under `workflows/`. Harness shims are thin wrappers that say "execute `workflows/<x>.md`".

## Prerequisites

**Cursor:** Specialist agents are available as Task subagents where mapped; marketing roles often use `generalPurpose` with an explicit mandate — see [workflows/lib/roles.md](workflows/lib/roles.md).

**Claude Code — engineering plugins:**

```
/plugin marketplace add wshobson/agents
/plugin install comprehensive-review backend-development cloud-infrastructure database-design business-analytics before-you-build conductor ship-mate full-stack-orchestration
```

**Claude Code — marketing plugins:**

```
/plugin install content-marketing startup-business-analyst customer-sales-automation seo-content-creation seo-technical-optimization seo-analysis-monitoring business-analytics before-you-build social-publishing brand-landingpage payment-processing agent-teams
```

**HubSpot + n8n (marketing):** private app token and n8n credentials in env only — see [experiments/002-marketing_team_workflow/README.md](experiments/002-marketing_team_workflow/README.md).

## Scheduling (marketing)

| Cadence | What |
|---------|------|
| Weekly | Market surface (`search-specialist` + `/startup-business-analyst:market-opportunity`) |
| Weekly | SEO refresh / cannibalization check |
| Daily | Nurture **drafts** via `sales-automator` → human approve → HubSpot |
| On HubSpot MQL | n8n webhook → `/team:marketing-qualify-lead` |

## Mechanics

1. **Blackboard files, not context memory** — each stage writes numbered markdown; the next stage reads the file.
2. **`state.json` for resumable runs** — checkpoints, iteration caps, anti-loop guards.
3. **Isolated subagents** — Task tool; the prompt is the only channel; paste prior file contents into each prompt.
4. **Human checkpoints** — go/no-go, decision approval, nurture send, deliver.
5. **HubSpot for leads** — marketing pipelines upsert CRM via orchestrator/n8n; feeders allowlisted in `leads/feeders.json`.

## Experiments

| ID | Purpose |
|----|---------|
| `001-fullstack-webapp` | Engineering intake-to-design dry-run fixture |
| `002-marketing_team_workflow` | Marketing Team dual-pipeline workspace |

See `upwork-proposal.md` for the original engineering proposal template that was generalized into `templates/proposal.md`.
