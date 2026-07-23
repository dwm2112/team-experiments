# Role → Harness Subagent Mapping

Workflows reference **roles**. Resolve the concrete `subagent_type` (or Claude Code agent name) using this table before every Task/Agent dispatch.

Detect harness:

- If running under Cursor (Task tool with kebab `subagent_type` values like `backend-development-backend-architect`) → use **Cursor** column.
- If running under Claude Code (plugin agents / Agent tool) → use **Claude Code** column.

## Mapping — Engineering (intake-to-design)

| Role ID | Role | Cursor `subagent_type` | Claude Code agent | Notes |
|---------|------|------------------------|-------------------|-------|
| `product-analyst` | Product/Analyst | `orchestrate` (preferred) or `generalPurpose` | `business-analyst` | Cursor has no `business-analyst` Task agent — fallback required |
| `backend-architect` | Backend architect | `backend-development-backend-architect` | `backend-architect` | |
| `database-architect` | Database architect | `database-design-database-architect` | `database-architect` | |
| `cloud-architect` | Cloud architect | `cloud-infrastructure-cloud-architect` | `cloud-architect` | |
| `security-auditor` | Security auditor (devil's advocate) | `comprehensive-review-security-auditor` | `security-auditor` | Engineering board: always assign DEVIL'S ADVOCATE mandate |
| `architect-reviewer` | Architecture reviewer | `comprehensive-review-architect-review` | `architect-reviewer` | Post-design review pass |

## Mapping — Marketing (marketing-gtm / marketing-client)

| Role ID | Role | Cursor `subagent_type` | Claude Code agent | Notes |
|---------|------|------------------------|-------------------|-------|
| `growth-marketer` | Growth / acquisition marketer | `generalPurpose` | `content-marketer` | Channel mix, CAC, experiments; paste Growth mandate in Cursor prompt |
| `content-strategist` | Content / SEO / messaging | `generalPurpose` | `content-marketer` | Prefer `seo-content-planner` for calendars; `seo-content-writer` for drafts |
| `sales-lead` | Sales / outreach lead | `generalPurpose` | `sales-automator` | Activation → revenue; nurture drafts only until human approve |
| `revops-analyst` | RevOps / lifecycle analyst | `orchestrate` or `generalPurpose` | `business-analyst` | Lifecycle, scoring, KPIs; Cursor fallback like product-analyst |
| `skeptic` | Marketing devil's advocate | `generalPurpose` | `startup-analyst` | Marketing board: **mandatory DEVIL'S ADVOCATE**; fallback `business-analyst` |
| `search-specialist` | Web research / opportunity surfacing | `generalPurpose` | `search-specialist` | Proactive market/competitive research |
| `startup-analyst` | Market opportunity / competitive | `generalPurpose` | `startup-analyst` | TAM/SAM/SOM, competitive landscape |
| `seo-planner` | SEO content planning | `generalPurpose` | `seo-content-planner` | Topic gaps, content calendar |
| `seo-refresher` | SEO content freshness | `generalPurpose` | `seo-content-refresher` | Scheduled refresh queues |
| `customer-success` | Retention / advocacy | `generalPurpose` | `customer-support` | Post-sale CX, evangelist motions |
| `social-publisher` | Social publishing | `generalPurpose` | `social-publishing-publisher` | Requires SocialClaw API key |

## Resolution algorithm

```
1. Read role id from the workflow step.
2. Select column for current harness.
3. If Cursor and role is product-analyst or revops-analyst:
     try subagent_type "orchestrate";
     if unavailable, use "generalPurpose" with an explicit BA / RevOps system prompt in the Task prompt.
4. If Cursor and role maps to generalPurpose:
     use "generalPurpose" and paste the Role mandate + Claude Code agent persona name into the Task prompt.
5. If Claude Code and agent missing (plugin not installed):
     halt and tell the user which /plugin install is required (see README Prerequisites).
6. Pass the resolved name as subagent_type / agent name. Do not invent other names.
```

## Engineering board meeting default roster (role IDs)

1. `backend-architect`
2. `cloud-architect`
3. `database-architect`
4. `product-analyst`
5. `security-auditor` — mandatory DEVIL'S ADVOCATE

## Marketing board meeting default roster (role IDs)

1. `growth-marketer`
2. `content-strategist`
3. `sales-lead`
4. `revops-analyst`
5. `skeptic` — mandatory DEVIL'S ADVOCATE

## Design stage sequence (role IDs)

1. `database-architect` → writes data model section  
2. `backend-architect` → writes API/service architecture (reads DB design)  
3. `architect-reviewer` → review pass  

Optional parallel discovery during analyze (examples):

- Payments / billing → `backend-architect` or `generalPurpose` with payment focus  
- Cloud / hosting → `cloud-architect`  
- Security surface → `security-auditor`  

## GTM surface feeders (default)

Initial feeders that may create HubSpot leads (see `leads/feeders.json`):

1. `search-specialist`
2. `startup-analyst`
3. `seo-planner`

Humans or additional agents are added by appending to `leads/feeders.json` (`type: agent|human`).

## Prompt hygiene

Every Task prompt must include:

1. Role mandate (who you are + what to produce)  
2. Full contents of prior blackboard files needed for the step  
3. Exact output schema (headings / fields)  
4. Path where the orchestrator will save the result (subagent returns text; chairman writes the file)  

### HubSpot write contract (marketing pipelines)

- Subagents **return** structured lead/deal payloads; they do **not** call HubSpot APIs themselves.
- The orchestrator (or n8n) owns HubSpot upserts.
- Never put API keys or tokens in blackboard files.
