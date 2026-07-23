# HubSpot Custom Properties (v1)

Create these properties on **Contact** (and mirror on Company where noted) before running `surface`. Use a HubSpot private app; store the token in env / n8n — never in this repo.

## Required custom properties

| Internal name | Object | Type | Description |
|---------------|--------|------|-------------|
| `lead_source_detail` | Contact | Single-line text | Granular source (e.g. `agent-surface-search`) |
| `pipeline_stage_internal` | Contact, Deal | Single-line text | Internal marketing pipeline stage key |
| `surfaced_by` | Contact, Company | Single-line text | `agent:<role-id>` or `human:<id>` |
| `opportunity_score` | Contact | Number | 1–100 score from qualify |
| `experiment_id` | Contact, Company, Deal | Single-line text | e.g. `002-marketing_team_workflow` |
| `mql_criteria_version` | Contact | Single-line text | Scorecard version (`v1`) |

## Standard properties used

| Property | Usage |
|----------|--------|
| `email`, `firstname`, `lastname` | Contact identity |
| `company`, `website`, `jobtitle`, `industry` | Enrichment |
| `lifecyclestage` | subscriber → lead → marketingqualifiedlead → salesqualifiedlead → opportunity → customer → evangelist |
| `hs_lead_status` | Optional operational status |

## Private app scopes (minimum)

- `crm.objects.contacts.read`
- `crm.objects.contacts.write`
- `crm.objects.companies.read`
- `crm.objects.companies.write`
- `crm.objects.deals.read`
- `crm.objects.deals.write`

## Agent / orchestrator contract

1. Subagents return JSON/YAML property maps — they do not call HubSpot.
2. Chairman session or n8n performs upsert and writes IDs into `state.hubspot`.
3. If API unavailable, write `leads/pending-upsert-<slug>.json` and set `flags.hubspot_pending = true`.

## MQL criteria (v1 — draft)

Document and version here when you lock ICP. Placeholder:

- Fit score ≥ 3/5 on Demand + Positioning + Monetization
- `opportunity_score` ≥ 60
- Identifiable email + company
- Recency: activity or research within 30 days
