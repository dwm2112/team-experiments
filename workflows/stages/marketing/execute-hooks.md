# Stage — Execute Hooks (marketing-client, optional)

Document automation handoffs to n8n / SocialClaw / HubSpot workflows. Does **not** implement client websites or auto-publish without credentials and human confirm.

## Inputs

- Requires: `flags.package_delivered = true`, `campaigns/campaign-package.md`
- Roles: chairman documents hooks; optional `social-publisher` for draft SocialClaw payloads

## Critical rules

1. No live publishes unless user explicitly confirms and API keys are present in env (not in repo).
2. Prefer writing runbooks over executing side effects in dry-runs.

## Steps

### 1. Write runbook

`campaigns/execute-hooks.md`:

```markdown
# Execute Hooks: [experiment id]

## HubSpot
- [ ] Lifecycle automation names
- [ ] Lists / segments
- [ ] Sequences to enroll (post human approve)

## n8n
- [ ] Cron: weekly surface / SEO refresh (if also GTM)
- [ ] Webhook: form → qualify
- [ ] Slack/email: board Decision routing

## SocialClaw
- [ ] Platforms
- [ ] Draft post payloads (paths)
- [ ] Schedule windows

## Ads (optional)
- [ ] LinkedIn / Meta via n8n — pending P2

## Manual human steps
- …
```

### 2. Optional SocialClaw drafts

If `SOCIALCLAW_API_KEY` available and user asks: dispatch `social-publisher` to draft (not send) platform copy into `campaigns/social-drafts.md`.

### 3. Complete

```json
"checkpoints.execute_hooks": "completed",
"files.execute_hooks": "campaigns/execute-hooks.md",
"status": "complete",
"stage": "complete",
"last_updated": "<ISO>"
```

## Done when

- Runbook written
- Pipeline status complete
