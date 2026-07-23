# Technical Design: [ENGAGEMENT TITLE]

**Experiment:** `experiments/[id]/`  
**Requirements:** `analysis/03-requirements.md`  
**Decision:** `analysis/04-decision.md`  
**Generated:** [ISO date]

## Summary

[1–2 paragraphs: approach locked by the board decision]

## Data Model

### Entities & Relationships

[ER overview]

### Schema Notes

| Entity | Key fields | Indexes / constraints |
|--------|------------|------------------------|
| … | … | … |

### Migration / Compatibility

[How this fits legacy DB or greenfield]

## Backend / API Architecture

### Endpoints / Contracts

| Method | Path / operation | Purpose | Auth |
|--------|------------------|---------|------|
| … | … | … | … |

### Service Layer

[Responsibilities and boundaries]

### Integrations

[Stripe, QuickBooks, etc. — sandbox → reconcile → prod path]

## Frontend / Client Touchpoints

[Pages/components or "API-only for this design slice"]

## Cross-Cutting

- **AuthZ:** …
- **Error handling:** …
- **Observability:** …
- **Security notes:** …

## Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| … | … |

## Architecture Review Notes

[Findings from architect-reviewer pass; unresolved items]

## Rough Estimate (design-level)

| Slice | Effort band | Notes |
|-------|-------------|-------|
| … | S/M/L | … |

**Total band:** …  
**Assumptions:** …
