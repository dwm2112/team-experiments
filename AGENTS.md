# AGENTS.md — AI Freelance Team

Shared context for Claude Code and Cursor. Read this before running any `/team:*` workflow.

## Mission

Build a freelance company of AI agent teams. Experiments use Upwork (and similar) RFPs as intake. The flagship pipeline runs **intake → qualify → proposal → analyze → board-meeting → design → package** and stops at a design package — no implementation.

## Conventions

### Blackboard discipline

- Never rely on context-window memory across stages.
- Each stage **must** write its output file before the next stage begins.
- Each stage **must** read prior stage files (paths in `state.json` / this doc) and paste relevant contents into subagent prompts.
- Experiment root: `experiments/<id>/`
  - `intake/` — `00-rfp.md`, `01-qualification.md`, `02-clarifying-questions.md`, `02-proposal.md`
  - `analysis/` — `03-requirements.md`, `04-decision.md`
  - `design/` — `05-design.md`, `design-package.md`
  - `board/` — `minutes-<slug>.md`
  - `state.json` — resumable pipeline state

### Checkpoints (mandatory halt)

1. **Qualify** — go / no-go / revise. Do not propose until go.
2. **Board decision** — approve / revise decision. Do not design until approved.
3. **Package** — deliver / revise. Terminal for this pipeline.

### Anti-loop

- Same blocker twice in one chain → escalate to human.
- Board meeting: max 2 debate rounds, max 5 agents.
- Do not re-enter a stage that already resolved the same issue.

### Roles (portable)

Workflows reference **roles**, not harness-specific agent names. Resolve via [workflows/lib/roles.md](workflows/lib/roles.md).

| Role | Purpose |
|------|---------|
| Product/Analyst | Clarifying questions, requirements, proposal draft |
| Backend architect | API, service layer, approach |
| Database architect | Schema, data model |
| Cloud architect | Infra, cost, deployability |
| Security auditor | Risks; **mandatory devil's advocate** in board meetings |
| Architecture reviewer | Design review pass after design draft |

### Board meeting roster (default)

1. Backend architect  
2. Cloud architect  
3. Database architect  
4. Product/Analyst  
5. Security auditor — **DEVIL'S ADVOCATE** (must argue strongest case against the leading proposal)

### Orchestrator rules

- The main session (or `/team:*` command) is the **chairman / message bus**.
- Subagents cannot talk peer-to-peer; mediate via blackboard files.
- Fan-out independent work in parallel; keep dependent work sequential.
- Paste full prior-file contents into each subagent prompt (isolation).

## Harness notes

### Claude Code

- Slash commands live under `.claude/commands/team/` → invoke as `/team:<name>`.
- Prefer marketplace agents from installed plugins (`business-analyst`, `backend-architect`, etc.).
- Skills: use `before-you-build` for Stage 1 qualify.
- Task/Agent tool for subagents; nesting off by default (keep chairman as sole conductor).

### Cursor

- Commands live under `.cursor/commands/` → invoke as `/team-<name>` (flat names; Cursor has no nested namespaces).
- Subagents via Task tool with `subagent_type` from [workflows/lib/roles.md](workflows/lib/roles.md).
- **No `business-analyst` Task subagent** — Product/Analyst falls back to `orchestrate` or `generalPurpose`.
- `before-you-build` skill path (if cached): run its checklist inline when the skill is not loadable as a Cursor skill.

### Both

- Canonical logic: `workflows/*.md` and `workflows/stages/*.md`.
- Shims only say: execute the referenced workflow with `$ARGUMENTS`.
- Templates: `templates/*.md`.

## Experiment ID format

`NNN-short-slug` — e.g. `001-fullstack-webapp`. Numeric prefix = creation order.

## Out of scope (this repo phase)

- Implementation, QA loops, deployment.
- Auto-submitting Upwork bids.
- Billing / client CRM integrations (future outer loop / n8n).
