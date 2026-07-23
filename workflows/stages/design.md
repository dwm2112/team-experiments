# Stage 5 — Design

Produce technical design from approved decision + requirements. Sequential: database → backend → architecture review.

## Inputs

- Requires: `flags.decision_approved = true`, `analysis/03-requirements.md`, `analysis/04-decision.md`
- Template: [templates/design.md](../../templates/design.md)
- Roles: [workflows/lib/roles.md](../lib/roles.md)

## Critical rules

1. Execute steps **in order** — do not merge or skip.
2. Write intermediate sections into `design/05-design.md` as you go (or write partial files then consolidate — final must be one `05-design.md`).
3. Paste requirements + decision into every subagent prompt.
4. Stop before implementation code.

## Steps

### 1. Load context

Read requirements, decision record, board minutes path if referenced, AGENTS.md, template.

### 2. Database design (`database-architect`)

Dispatch with full requirements + decision implications.

Deliverables: entities/relationships, schema notes, indexes, migration/compatibility, query patterns.

Append/write under `## Data Model` in `design/05-design.md`.

### 3. Backend / API architecture (`backend-architect`)

Dispatch with requirements + decision + **full data model section**.

Deliverables: endpoints/contracts, service layer, integrations, frontend touchpoints summary, cross-cutting, risks.

Append remaining sections to `design/05-design.md`.

### 4. Architecture review (`architect-reviewer`)

Dispatch with full `05-design.md` draft + decision.

Ask for: boundary issues, over/under-engineering, missed risks, must-fix before package.

Append `## Architecture Review Notes` with findings. Apply clear must-fix edits to the design if they are small and uncontroversial; list larger ones under unresolved.

### 5. Rough estimate

As chairman (or Product/Analyst), add `## Rough Estimate (design-level)` with S/M/L bands — no false precision.

### 6. Update state

```json
"checkpoints.design": "completed",
"stage": "package",
"files.design": "design/05-design.md",
"last_updated": "<ISO>"
```

### 7. Hand off

```
Design complete: experiments/<id>/design/05-design.md
Next: Stage 6 package.
```
