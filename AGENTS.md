# AGENTS.md — AI Freelance Team

Shared context for Claude Code and Cursor. Read this before running any `/team:*` workflow.

## Mission

Build a freelance company of AI agent teams. Experiments use Upwork (and similar) RFPs as intake for engineering design work, and market/campaign briefs for marketing. Two flagship pipelines:

1. **intake-to-design** — `intake → qualify → proposal → analyze → board-meeting → design → package` (stops at design package — no implementation).
2. **marketing** — dual entry: **own GTM** (`marketing-gtm`) and **client delivery** (`marketing-client`), with a marketing board meeting for high-stakes decisions.

## Conventions

### Blackboard discipline

- Never rely on context-window memory across stages.
- Each stage **must** write its output file before the next stage begins.
- Each stage **must** read prior stage files (paths in `state.json` / this doc) and paste relevant contents into subagent prompts.
- Experiment root: `experiments/<id>/`

**Engineering (intake-to-design):**

- `intake/` — `00-rfp.md`, `01-qualification.md`, `02-clarifying-questions.md`, `02-proposal.md`
- `analysis/` — `03-requirements.md`, `04-decision.md`
- `design/` — `05-design.md`, `design-package.md`
- `board/` — `minutes-<slug>.md`
- `state.json` — resumable pipeline state

**Marketing (`002-marketing_team_workflow` and siblings):**

- `briefs/` — opportunity + campaign briefs
- `campaigns/` — nurture drafts, channel/content plans, package
- `analysis/` — lead qualification + `marketing-decision.md`
- `board/` — marketing minutes
- `intake/` — client raw briefs (client pipeline)
- `leads/` — `feeders.json`, HubSpot property checklist, pending upserts
- `state.json` — resumable pipeline state (`pipeline`: `marketing-gtm` | `marketing-client`)

### Systems of record

| Data | SOR |
|------|-----|
| Engineering artifacts / decisions | Experiment blackboard markdown |
| Marketing briefs / board minutes / packages | Experiment blackboard markdown |
| Leads, contacts, companies, deals, lifecycle | **HubSpot CRM** (from day one for marketing) |

Subagents return HubSpot property payloads; the orchestrator or n8n performs upserts. Never put API keys in blackboard files.

### Checkpoints (mandatory halt)

**Engineering pipeline:**

1. **Qualify** — go / no-go / revise. Do not propose until go.
2. **Board decision** — approve / revise decision. Do not design until approved.
3. **Package** — deliver / revise. Terminal for this pipeline.

**Marketing pipelines:**

1. **Qualify lead / client qualify** — go / no-go. Do not enrich or brief until go.
2. **Marketing board decision** — approve before campaign execution or major channel/budget bets.
3. **Nurture send** — approve before any outbound (email/LinkedIn/Upwork).
4. **Campaign package** — deliver / revise (client pipeline).

### Anti-loop

- Same blocker twice in one chain → escalate to human.
- Board meeting: max 2 debate rounds, max 5 agents.
- Do not re-enter a stage that already resolved the same issue.
- Marketing: only feeders in `leads/feeders.json` may create HubSpot contacts during surface.

### Roles (portable)

Workflows reference **roles**, not harness-specific agent names. Resolve via [workflows/lib/roles.md](workflows/lib/roles.md).

**Engineering**

| Role | Purpose |
|------|---------|
| Product/Analyst | Clarifying questions, requirements, proposal draft |
| Backend architect | API, service layer, approach |
| Database architect | Schema, data model |
| Cloud architect | Infra, cost, deployability |
| Security auditor | Risks; **mandatory devil's advocate** in engineering board meetings |
| Architecture reviewer | Design review pass after design draft |

**Marketing**

| Role | Purpose |
|------|---------|
| Growth marketer | Awareness / acquisition, CAC, channel experiments |
| Content strategist | Messaging, SEO, content calendar |
| Sales lead | Activation → revenue, nurture drafts, handoff SLAs |
| RevOps analyst | Lifecycle, scoring, KPIs, measurement |
| Skeptic | **Mandatory devil's advocate** on marketing board |
| Search specialist / Startup analyst / SEO planner | GTM surface feeders (extensible via `feeders.json`) |
| Customer success | Retention / advocacy |

### Engineering board meeting roster (default)

1. Backend architect  
2. Cloud architect  
3. Database architect  
4. Product/Analyst  
5. Security auditor — **DEVIL'S ADVOCATE**

### Marketing board meeting roster (default)

1. Growth marketer  
2. Content strategist  
3. Sales lead  
4. RevOps analyst  
5. Skeptic — **DEVIL'S ADVOCATE**

### Orchestrator rules

- The main session (or `/team:*` command) is the **chairman / message bus**.
- Subagents cannot talk peer-to-peer; mediate via blackboard files.
- Fan-out independent work in parallel; keep dependent work sequential.
- Paste full prior-file contents into each subagent prompt (isolation).

## Harness notes

### Claude Code

- Slash commands live under `.claude/commands/team/` → invoke as `/team:<name>`.
- Prefer marketplace agents from installed plugins (`business-analyst`, `backend-architect`, `content-marketer`, `sales-automator`, etc.).
- Skills: use `before-you-build` for qualify stages; startup-business-analyst skills for market opportunity.
- Task/Agent tool for subagents; nesting off by default (keep chairman as sole conductor).

### Cursor

- Commands live under `.cursor/commands/` → invoke as `/team-<name>` (flat names; Cursor has no nested namespaces).
- Subagents via Task tool with `subagent_type` from [workflows/lib/roles.md](workflows/lib/roles.md).
- **No `business-analyst` Task subagent** — Product/Analyst and RevOps fall back to `orchestrate` or `generalPurpose`.
- Most marketing roles map to `generalPurpose` with an explicit role mandate in the Task prompt.
- `before-you-build` skill path (if cached): run its checklist inline when the skill is not loadable as a Cursor skill.

### Both

- Canonical logic: `workflows/*.md` and `workflows/stages/*.md` (marketing stages under `workflows/stages/marketing/`).
- Shims only say: execute the referenced workflow with `$ARGUMENTS`.
- Templates: `templates/*.md`.

## Experiment ID format

`NNN-short-slug` — e.g. `001-fullstack-webapp`, `002-marketing_team_workflow`. Numeric prefix = creation order.

## Out of scope (this repo phase)

- Implementation, QA loops, deployment (engineering package stops at design).
- Auto-submitting Upwork bids or cold email without human approve.
- Auto-publishing social without explicit confirm + credentials.
- Full CDP / Segment maturity layer (P3).
