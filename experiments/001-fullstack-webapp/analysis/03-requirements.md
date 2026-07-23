# Requirements: Full Stack Web App Engagement

**Experiment:** `experiments/001-fullstack-webapp/`  
**Source RFP:** `intake/00-rfp.md`  
**Generated:** 2026-07-23  
**Dry-run:** yes

## Problem Statement

Client needs an independent US-based full-stack developer who can turn vague business asks into shipped features (UI → DB), with emphasis on data reconciliation, automation, and likely Stripe/QuickBooks integration — without requiring a perfect upfront spec or a forced rewrite.

## Users / Stakeholders

- **Primary:** Client operator(s) who raise vague asks and need shipped outcomes
- **Secondary:** Internal users consuming reports / reconciled financial data

## Acceptance Criteria

- [ ] Given a vague business ask, when scoped with clarifying questions, then a concrete deliverable and acceptance checks are agreed before large build
- [ ] Given reconciliation/reporting needs, when data is moved or joined, then outputs are trustworthy and repeatable (not one-off spreadsheets)
- [ ] Given Stripe and/or QuickBooks involvement, when integrating, then sandbox → verify → production path is used
- [ ] Given the existing stack, when shipping, then changes fit the current app unless a side-car is explicitly approved
- [ ] Given AI-assisted development, when delivering, then human-owned architecture and review are evident

## Scope

### In Scope

- Clarifying vague asks into shippable slices
- Full-stack feature work on the existing web app
- SQL / reconciliation / reporting improvements
- Stripe and/or QuickBooks integration work as prioritized
- Automation of recurring manual workflows

### Out of Scope

- Unsolicited greenfield rewrite of the entire app
- Unpaid multi-week discovery
- Guaranteeing PCI program ownership beyond app-level integration hygiene
- Implementation inside this dry-run design package (pipeline stops at design)

## Technical Constraints

- Stack / legacy: JS/HTML5; likely PHP + AngularJS-era codebase
- Integrations: Stripe and/or QuickBooks Online
- Performance / security / compliance: financial data sensitivity; US-only contractor filter
- Budget / timeline / rate signals: $70–90/hr, &lt;30 hrs/wk, 6+ months

## Dependencies

- Access to existing codebase and staging/sandbox credentials
- Chart of accounts / reconciliation rules if QBO involved
- Client availability for clarifying questions

## Risks & Open Questions

| Risk / Question | Impact | Mitigation / Who answers |
|-----------------|--------|--------------------------|
| Vague scope → unpaid discovery | H | Paid discovery cap; questions in proposal |
| Legacy stack fights modern patterns | M | Board decision: extend vs side-car |
| QBO/Stripe not actually near-term | M | Priority question in proposal |
| AI-tool stigma if delivery looks unowned | M | Explicit review ownership in proposal |

## Assumptions

- Client prefers evolution over rewrite unless proven necessary
- First value is workflow unblock (recon/reporting), not a redesign

## Recommended Decision Gate

**Extend the legacy PHP/AngularJS app in place for Stripe/QuickBooks reconciliation features vs introduce a thin modern API/sidecar beside it that owns integrations and sync, with the legacy UI calling the sidecar.**
