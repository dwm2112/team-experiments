# Stage — Campaign Design (marketing-client)

Produce channel plan, content calendar, SEO plan, and measurement plan after board APPROVE.

## Inputs

- Requires: `flags.decision_approved = true`, `briefs/campaign-brief.md`, `analysis/marketing-decision.md`
- Roles (sequential or limited parallel):
  1. `growth-marketer` — channel / budget / experiments
  2. `content-strategist` + `seo-planner` — content + SEO
  3. `revops-analyst` — measurement + HubSpot lifecycle
  4. `sales-lead` — activation / handoff SLAs if lead-gen

## Critical rules

1. Do not start until board decision approved.
2. Paste brief + decision into every prompt.
3. Write section files under `campaigns/` then merge in package stage.

## Steps

### 1. Channel plan

`campaigns/01-channel-plan.md` — mix, CAC hypotheses, tests, weekly cadence.

### 2. Content + SEO

`campaigns/02-content-seo.md` — pillars, 30–60 day calendar, keyword clusters, refresh list.

### 3. Measurement

`campaigns/03-measurement.md` — metric tree, UTM taxonomy, HubSpot lifecycle mapping, attribution note (tactical vs executive model).

### 4. Activation / sales handoff (if B2B lead-gen)

`campaigns/04-activation.md` — MQL/SQL criteria, SLA, sequences outline.

### 5. Update state

```json
"stage": "package",
"checkpoints.campaign_design": "completed",
"files.channel_plan": "campaigns/01-channel-plan.md",
"files.content_seo": "campaigns/02-content-seo.md",
"files.measurement": "campaigns/03-measurement.md",
"last_updated": "<ISO>"
```

## Done when

- Design section files written
- Stage advanced to package
