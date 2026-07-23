# 002 — Marketing Team Workflow

Experiment workspace for the Marketing Team: dual pipelines (own GTM + client delivery), marketing board meetings, and HubSpot as the live lead/CRM system of record.

## Pipelines

| Pipeline | Start | Flow |
|----------|-------|------|
| Own GTM | `/team:marketing-pipeline gtm` | surface → qualify_lead → enrich → board? → nurture → handoff → retain |
| Client delivery | `/team:marketing-pipeline client` | intake → qualify → brief → board → campaign_design → package → execute_hooks |

Cursor: `/team-marketing-pipeline` with the same arguments.

## Layout

```
briefs/          Opportunity + campaign briefs
campaigns/       Nurture drafts, channel/content plans, package
analysis/        Lead qualification + marketing decisions
board/           Marketing board minutes
intake/          Client raw briefs (marketing-client)
leads/           feeders.json, HubSpot property checklist, pending upserts
state.json       Resumable pipeline state
```

## Lead feeders

[`leads/feeders.json`](leads/feeders.json) allowlists who may create HubSpot contacts during `surface`.

**v1 agents:** `search-specialist`, `startup-analyst`, `seo-planner`.

To add a human or another agent, append an object (see `how_to_add_human` in that file) and optionally document with [`templates/lead-feeder.md`](../../templates/lead-feeder.md).

## HubSpot setup (P0)

1. Create a private app with scopes listed in [`leads/hubspot-properties.md`](leads/hubspot-properties.md).
2. Create the custom properties in that file.
3. Export token to the environment used by the orchestrator or n8n — **never commit tokens**.
4. On first `surface`, IDs are written into `state.hubspot`. If the API is down, payloads land in `leads/pending-upsert-*.json`.

## Integrations (outer loop)

### n8n (P0)

Suggested workflows (self-hosted):

| Trigger | Action |
|---------|--------|
| Cron weekly | Invoke Claude `/team:marketing-surface` (or Cursor equivalent) with ICP topic |
| Cron weekly | SEO refresh via `seo-content-refresher` → write `campaigns/` queue |
| HubSpot webhook (form / lifecycle MQL) | `/team:marketing-qualify-lead` |
| File/Decision update | Slack or email with `analysis/marketing-decision.md` summary |

Keep debate/reasoning inside Claude Code or Cursor; n8n schedules, webhooks, and CRM writes.

### SocialClaw (P1)

```bash
export SOCIALCLAW_API_KEY=your_key_here
```

Used by `social-publishing-publisher` / execute_hooks drafts. Confirm before any live publish.

### Other (planned)

| Tool | Priority | Use |
|------|----------|-----|
| GA4 + Search Console | P1 | Awareness / acquisition measurement |
| Stripe | P1 | Revenue events → HubSpot deals |
| Slack / email | P1 | Checkpoint notifications |
| Figma | P2 | Creative handoff |
| LinkedIn / Meta Ads via n8n | P2 | Paid acquisition |
| Google Stitch (brand-landingpage) | P2 | Landing pages |
| Segment / Mixpanel | P3 | CDP maturity |

## Marketplace plugins (Claude Code)

```
/plugin marketplace add wshobson/agents
/plugin install content-marketing startup-business-analyst customer-sales-automation seo-content-creation seo-technical-optimization seo-analysis-monitoring business-analytics before-you-build social-publishing brand-landingpage payment-processing agent-teams
```

## Scheduling favorites

| Cadence | What |
|---------|------|
| Weekly | `search-specialist` + `/startup-business-analyst:market-opportunity` → HubSpot + opportunity briefs |
| Weekly | SEO refresher + cannibalization check |
| Daily | `sales-automator` drafts only → human approve → HubSpot sequence |
| On MQL | n8n → qualify-lead |

## Quick start

1. Configure HubSpot properties ([checklist](leads/hubspot-properties.md)).
2. GTM: `/team:marketing-pipeline gtm experiments/002-marketing_team_workflow`
3. Or client: `/team:marketing-pipeline client` with a brief path.
4. Honor checkpoints: qualify go/no-go, board APPROVE, nurture send APPROVE, package DELIVER.

Canonical logic: [`workflows/marketing-pipeline.md`](../../workflows/marketing-pipeline.md).
