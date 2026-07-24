# Documentation — Standalone Capability

Produce a consolidated **documentation package** for a target scope. Portable across pipelines: run it on an experiment blackboard, the whole team-experiments system, or a code deliverable from a future pipeline.

Modeled on the package stages: fan-out to documentation roles, write blackboard files, human **deliver** checkpoint. Terminal artifact: `docs/documentation-package.md`.

**Does not implement or modify product code.** It reads sources and writes documentation only.

## Commands

| Invocation | Meaning |
|------------|---------|
| `/team-docs experiment experiments/<id>` | Document an experiment's blackboard artifacts + how to run/resume it |
| `/team-docs repo` | Document the whole team-experiments system (architecture, commands, workflows, roles) |
| `/team-docs codebase <path>` | Document a code deliverable at `<path>` (integrations, API surface, module map) |
| `/team-docs status experiments/<id>` | Print documentation checkpoint status for that experiment |
| `/team-docs resume experiments/<id>` | Continue after a checkpoint |

Claude Code equivalent: `/team:docs` with the same arguments.

If no scope keyword is given, infer: an `experiments/<id>` path → `experiment`; a repo root or omitted arg → `repo`; any other path → `codebase`.

## Critical rules

1. Read [AGENTS.md](../AGENTS.md) first.
2. **Read sources, do not rewrite them.** Documentation is additive — never edit the artifacts being documented.
3. Each role's output **must** be written to its blackboard file before assembly. Do not rely on context memory.
4. Paste the full contents of every source file into each subagent prompt (isolation).
5. Resolve subagent names via [workflows/lib/roles.md](lib/roles.md).
6. Write `docs/documentation-package.md` before the checkpoint, then **STOP** for human deliver approval.
7. Use the [templates/documentation-package.md](../templates/documentation-package.md) schema for the terminal artifact.

## Output location

All documentation writes under `<scope-root>/docs/`:

- `experiment` scope → `experiments/<id>/docs/`
- `repo` scope → `docs/` at repo root
- `codebase` scope → `<path>/docs/`

Doc set (produced per scope; skip a file only when its inputs do not exist — note the skip in the package):

| File | Role | Purpose |
|------|------|---------|
| `docs/00-doc-plan.md` | `docs-architect` | Scope, audiences, outline, source inventory |
| `docs/01-architecture.md` | `docs-architect` + `diagram-specialist` | Long-form system / pipeline guide with Mermaid diagrams |
| `docs/02-reference.md` | `reference-builder` | Exhaustive reference (commands, workflows, roles, files, config) |
| `docs/03-tutorial.md` | `tutorial-engineer` | Getting-started + step-by-step "how to run this" |
| `docs/04-integrations.md` | `api-documenter` | Integration / API contracts (HubSpot, n8n, external APIs) — only when integrations exist |
| `docs/documentation-package.md` | orchestrator | Terminal consolidated index (template) |

## Steps

### 1. Resolve scope and inventory sources

- Determine scope + root from `$ARGUMENTS` (see command table / inference rule).
- Build a **source inventory**: list every file the docs will draw from.
  - `experiment` → `state.json`, all blackboard markdown (`intake/`, `analysis/`, `board/`, `design/`, `briefs/`, `campaigns/`, `leads/`), the experiment `README.md`.
  - `repo` → `AGENTS.md`, `README.md`, `workflows/**`, `templates/**`, `.cursor/commands/**`, `.claude/commands/**`.
  - `codebase` → source tree under `<path>` (prefer entry points, configs, public interfaces). For a full repo scan, the `ship-mate` `scan` skill may be used to seed `project-doc.md`.
- Create the `docs/` directory if absent.

### 2. Plan (docs-architect)

Dispatch `docs-architect` with the source inventory. It writes `docs/00-doc-plan.md`:

```markdown
# Documentation Plan: [scope title]

**Scope:** experiment | repo | codebase
**Root:** [path]
**Generated:** [ISO]

## Audiences
- [e.g. operator running the pipeline; future engineer; client]

## Source inventory
| Source | Path | Covered in |
|--------|------|------------|

## Outline
- [ ] 01-architecture — [what it will cover]
- [ ] 02-reference — [what it will cover]
- [ ] 03-tutorial — [what it will cover]
- [ ] 04-integrations — [include? why]

## Open questions for human
- …
```

### 3. Draft (parallel fan-out)

Dispatch the drafting roles **in parallel** (independent), each with the plan + relevant source contents pasted in. Chairman writes each returned draft to its file:

- `docs-architect` → `docs/01-architecture.md` (request Mermaid diagram stubs inline)
- `reference-builder` → `docs/02-reference.md`
- `tutorial-engineer` → `docs/03-tutorial.md`
- `api-documenter` → `docs/04-integrations.md` (only if integrations exist in inventory)

Then dispatch `diagram-specialist` with `docs/01-architecture.md` to finalize/validate the Mermaid diagrams; write the result back.

Prefer HADS (human + AI dual-readable, token-efficient) structure for each doc — the `hads` skill format is the house style. For decisions worth preserving, use the `architecture-decision-records` skill format inside `docs/01-architecture.md`.

### 4. Assemble (orchestrator)

Write `docs/documentation-package.md` from [templates/documentation-package.md](../templates/documentation-package.md). Link each section doc, list any skipped docs with the reason, and surface the plan's open questions.

### 5. Update state (pre-checkpoint)

For `experiment` scope, update `experiments/<id>/state.json`:

```json
"checkpoints.docs": "awaiting_approval",
"files.doc_plan": "docs/00-doc-plan.md",
"files.documentation_package": "docs/documentation-package.md",
"last_updated": "<ISO>"
```

For `repo` / `codebase` scope (no `state.json`), skip state; the package file is the record.

### 6. PHASE CHECKPOINT — Deliver

```
Documentation package ready: <root>/docs/documentation-package.md

1. DELIVER — mark documentation complete
2. REVISE — specify which doc / section to change
3. PAUSE — leave awaiting_approval
```

- **DELIVER:** for `experiment` scope set `checkpoints.docs = "completed"`. For all scopes, print the completion banner.
- **REVISE:** re-dispatch only the affected role(s) with the revision note; rewrite that file and re-assemble; re-checkpoint.
- **PAUSE:** stop.

### 7. Completion banner

```
Documentation complete: <root>/docs/
Terminal artifact: docs/documentation-package.md
Sources were read-only; no product artifacts were modified.
```

## Anti-loop guards

1. **Repeated blocker** — same missing-source or unclear-scope blocker twice → escalate to human.
2. **No source edits** — if a role proposes changing a source file, refuse; capture it as an "Open question" in the package instead.
3. **Role caps** — at most one draft per doc file per revision round; max 2 revision rounds before escalating.
