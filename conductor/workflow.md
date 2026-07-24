# Workflow — team-experiments

## TDD Policy

**Flexible.** Tests are recommended for complex logic and are not applicable to pure Markdown workflow authoring. When Python tooling lands, add tests for non-trivial logic (state handling, parsing, integrations); do not block simple scripts on tests.

## Commit Strategy

**Conventional Commits.** Use `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:` prefixes. Keep commits scoped and descriptive.

## Code Review

**Required for non-trivial changes.** Trivial edits (typos, small doc tweaks) may be self-reviewed. Structural changes to `workflows/`, role mappings, or pipeline logic warrant a review pass.

## Verification Checkpoints (mandatory halts)

Manual verification is required at defined **pipeline gates**, mirroring `AGENTS.md`:

**Engineering (intake-to-design):**
1. **Qualify** — go / no-go / revise. Do not propose until go.
2. **Board decision** — approve / revise. Do not design until approved.
3. **Package** — deliver / revise. Terminal for this pipeline.

**Marketing:**
1. **Qualify lead / client qualify** — go / no-go before enrich or brief.
2. **Marketing board decision** — approve before campaign execution or major channel/budget bets.
3. **Nurture send** — approve before any outbound (email/LinkedIn/Upwork).
4. **Campaign package** — deliver / revise (client pipeline).

## Task Lifecycle

1. Read prior stage file(s) referenced in `state.json` / `AGENTS.md`.
2. Do the work; fan out independent subagent work in parallel, keep dependent work sequential.
3. Write the stage output file **before** the next stage begins.
4. Update `state.json` (stage, checkpoint status, iteration counters).
5. Halt at the next mandatory checkpoint for human approval.

## Anti-Loop Rules

- Same blocker twice in one chain → escalate to a human.
- Board meeting: max 2 debate rounds, max 5 agents.
- Do not re-enter a stage that already resolved the same issue.
