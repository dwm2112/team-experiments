# Product Guidelines — team-experiments

## Voice and Tone

Concise and direct. Dense, skimmable, and free of filler. Favor tables, numbered steps, and short declarative sentences over prose. Deliverables should be usable by both humans and downstream agents.

## Design Principles

1. **Blackboard over context memory** — every stage writes its output file before the next stage begins; the next stage reads that file. Never rely on context-window memory across stages.
2. **Portability** — canonical logic lives once under `workflows/`; harness shims (`.claude/commands/team/`, `.cursor/commands/`) are thin wrappers that only say "execute the referenced workflow."
3. **Human checkpoints** — mandatory halts at defined gates (qualify go/no-go, board decision, nurture send, deliver/package). Do not proceed past a gate without approval.
4. **Anti-loop discipline** — same blocker twice in one chain escalates to a human; board meetings cap at 2 debate rounds / 5 agents; never re-enter a resolved stage.

## Additional Standards

- Never put API keys or secrets in blackboard files — env only.
- Subagents cannot talk peer-to-peer; the orchestrator mediates via blackboard files and pastes prior-file contents into each subagent prompt.
- Reference **roles**, not harness-specific agent names — resolve via [`workflows/lib/roles.md`](../workflows/lib/roles.md).
