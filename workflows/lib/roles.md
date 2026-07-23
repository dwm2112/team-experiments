# Role → Harness Subagent Mapping

Workflows reference **roles**. Resolve the concrete `subagent_type` (or Claude Code agent name) using this table before every Task/Agent dispatch.

Detect harness:

- If running under Cursor (Task tool with kebab `subagent_type` values like `backend-development-backend-architect`) → use **Cursor** column.
- If running under Claude Code (plugin agents / Agent tool) → use **Claude Code** column.

## Mapping

| Role ID | Role | Cursor `subagent_type` | Claude Code agent | Notes |
|---------|------|------------------------|-------------------|-------|
| `product-analyst` | Product/Analyst | `orchestrate` (preferred) or `generalPurpose` | `business-analyst` | Cursor has no `business-analyst` Task agent — fallback required |
| `backend-architect` | Backend architect | `backend-development-backend-architect` | `backend-architect` | |
| `database-architect` | Database architect | `database-design-database-architect` | `database-architect` | |
| `cloud-architect` | Cloud architect | `cloud-infrastructure-cloud-architect` | `cloud-architect` | |
| `security-auditor` | Security auditor (devil's advocate) | `comprehensive-review-security-auditor` | `security-auditor` | Board meetings: always assign DEVIL'S ADVOCATE mandate |
| `architect-reviewer` | Architecture reviewer | `comprehensive-review-architect-review` | `architect-reviewer` | Post-design review pass |

## Resolution algorithm

```
1. Read role id from the workflow step.
2. Select column for current harness.
3. If Cursor and role is product-analyst:
     try subagent_type "orchestrate";
     if unavailable, use "generalPurpose" with an explicit Product Manager / BA system prompt in the Task prompt.
4. If Claude Code and agent missing (plugin not installed):
     halt and tell the user which /plugin install is required (see README Prerequisites).
5. Pass the resolved name as subagent_type / agent name. Do not invent other names.
```

## Board meeting default roster (role IDs)

1. `backend-architect`
2. `cloud-architect`
3. `database-architect`
4. `product-analyst`
5. `security-auditor` — mandatory DEVIL'S ADVOCATE

## Design stage sequence (role IDs)

1. `database-architect` → writes data model section  
2. `backend-architect` → writes API/service architecture (reads DB design)  
3. `architect-reviewer` → review pass  

Optional parallel discovery during analyze (examples):

- Payments / billing → `backend-architect` or `generalPurpose` with payment focus  
- Cloud / hosting → `cloud-architect`  
- Security surface → `security-auditor`  

## Prompt hygiene

Every Task prompt must include:

1. Role mandate (who you are + what to produce)  
2. Full contents of prior blackboard files needed for the step  
3. Exact output schema (headings / fields)  
4. Path where the orchestrator will save the result (subagent returns text; chairman writes the file)  
