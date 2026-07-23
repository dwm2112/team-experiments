# Stage — Client Qualify (marketing-client)

Go/no-go on taking the marketing engagement. Uses before-you-build framed for client delivery.

## Inputs

- Requires: `intake/00-client-brief.md`
- Roles: `revops-analyst` or `growth-marketer`

## Steps

### 1. Produce qualification

Write `intake/01-qualification.md`:

```markdown
# Client Engagement Qualification: [id]

## Risk verdict
[Low | Medium | High]

## Main assumption
…

## Checklist scores
| Lens | Score (1–5) | Notes |
|------|-------------|-------|
| Demand / urgency | | |
| Positioning fit (can we deliver) | | |
| Monetization / budget realism | | |
| Scope clarity | | |
| Brand / trust constraints | | |
| Timeline feasibility | | |
| Measurement readiness | | |

## Recommended verdict
**GO** | **NO-GO** | **GO-WITH-CONDITIONS**
```

### 2. PHASE CHECKPOINT

```
1. GO — proceed to brief
2. NO-GO — stop
3. REVISE — gather clarifications
```

On GO: `flags.qualify_verdict = "go"`, `stage = brief`, `checkpoints.qualify = completed`.

## Done when

- Qualification written and human resolved checkpoint
