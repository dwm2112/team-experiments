# Dry-run report — 001-fullstack-webapp

**Date:** 2026-07-23  
**Router:** `workflows/intake-to-design.md` (invoked conceptually as `/team:ship-design`)

## What was validated

| Check | Result |
|-------|--------|
| Monorepo skeleton + AGENTS/README | PASS |
| `workflows/lib/roles.md` mapping | PASS |
| Templates (5) | PASS |
| Stage bodies (6) + board-meeting + master router | PASS |
| Claude Code shims (8) reference `workflows/` | PASS |
| Cursor shims (8) reference `workflows/` | PASS |
| Experiment seed `intake/00-rfp.md` + dirs | PASS |
| Stage outputs written in order | PASS |
| `state.json` checkpoint progression → `complete` | PASS |
| Board minutes include **recorded dissents** | PASS |
| Terminal `design/design-package.md` | PASS |

## Checkpoint simulation

1. **Qualify** — verdict `go-with-conditions` → proceeded (fixture treated as approved GO)
2. **Board decision** — consensus adopt-with-changes; dissents from security-auditor + cloud-architect recorded → approved
3. **Package deliver** — marked complete

## Artifact chain

```
intake/00-rfp.md
  → intake/01-qualification.md
  → intake/02-clarifying-questions.md + intake/02-proposal.md
  → analysis/03-requirements.md
  → board/minutes-legacy-extend-vs-sidecar.md
  → analysis/04-decision.md
  → design/05-design.md
  → design/design-package.md
```

## Limits of this dry-run

- Board Round 0/1 positions are **simulated fixtures**, not live Task subagent outputs.
- To run a live debate: `/team-board-meeting experiments/001-fullstack-webapp` (Cursor) or `/team:board-meeting` (Claude Code) on a fresh experiment or after resetting board files.
- Proposal placeholders intentionally unfilled (human stories required).

## Next live action

```
/team-ship-design experiments/<new-id>     # or qualify→… on a new RFP
# or re-debate:
/team-board-meeting experiments/001-fullstack-webapp
```
