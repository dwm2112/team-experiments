# Lead Feeder Entry

Use this template when documenting a new human or agent feeder before adding them to `leads/feeders.json`.

## Identity

| Field | Value |
|-------|-------|
| id | `agent:search-specialist` or `human:jane` |
| type | agent \| human |
| display_name | |
| enabled | true \| false |
| role_id | (if agent — must exist in roles.md) |

## Permissions

- [ ] May create HubSpot contacts
- [ ] May create HubSpot companies
- [ ] May create HubSpot deals (usually false for surface-only)
- [ ] May trigger qualify_lead automatically via n8n

## Default properties written

| Property | Value / rule |
|----------|--------------|
| surfaced_by | same as id |
| lead_source_detail | |
| experiment_id | from state |
| mql_criteria_version | v1 |

## Notes

[Cadence, topics owned, escalation path]
