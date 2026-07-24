# Tech Stack — team-experiments

## Primary Substrate

This repo is authored primarily in **Markdown** (workflow bodies, stages, templates) and **JSON** (`state.json` per experiment). There is no application code today. **Python** tooling/scripts are expected soon (e.g. helpers for state, HubSpot upserts, or automation glue).

| Layer | Choice |
|-------|--------|
| Workflow logic | Markdown under `workflows/` and `workflows/stages/` (source of truth) |
| Templates | Markdown under `templates/` |
| Pipeline state | JSON — `experiments/<id>/state.json` (resumable runs, checkpoints, anti-loop guards) |
| Future tooling | Python (planned) |

## AI Harnesses

Targets **both** Claude Code and Cursor, kept portable from one source of truth.

| Harness | Command location | Invocation | Subagents |
|---------|------------------|------------|-----------|
| Claude Code | `.claude/commands/team/` | `/team:<name>` | Task/Agent tool; marketplace plugins from `wshobson/agents` |
| Cursor | `.cursor/commands/` | `/team-<name>` (flat) | Task tool with `subagent_type` from `workflows/lib/roles.md` |

## Frontend

None. Runs inside the AI harness chat; there is no UI.

## Integrations & Systems of Record

| Data | System of Record |
|------|------------------|
| Leads, contacts, companies, deals, lifecycle | **HubSpot CRM** (marketing, from day one) |
| Engineering artifacts / decisions | Blackboard markdown (`experiments/<id>/`) |
| Marketing briefs / minutes / packages | Blackboard markdown |
| Automation / webhooks | n8n (e.g. HubSpot MQL → qualify) |
| Intake substrate | Upwork / RFP sources, campaign briefs |

Secrets (HubSpot private app token, n8n credentials) live in the environment only — never in blackboard files.

## Runtime

Runs locally inside the AI harness (Claude Code / Cursor) against this git repo. Marketing scheduling (weekly surface/SEO, daily nurture drafts) can be driven via n8n when configured.
