# Campaign Package: Atlas Sovereign — Agentic AI Kickoff Brand + Campaign

**Experiment:** `experiments/002-marketing_team_workflow/`  
**Date:** 2026-07-24T02:00:00Z  
**Status:** Delivered  
**Decision:** `analysis/marketing-decision.md`  
**Brief:** `briefs/campaign-brief.md`

## Executive Summary

This package delivers branding direction and a kickoff marketing campaign design for **Atlas Sovereign’s Agentic AI Division**: SMB-primary positioning as a **trustworthy digital coworker** (augmentation-first, claim-safe), with a founder-led **LinkedIn + waitlist → discovery** motion. Board approval authorizes **design work only** — not outbound, media spend, package pricing, or numeric funnel commits until the client confirmation package clears. Client hosts the waitlist page from our specs; we do not implement the site.

## Board Consensus

- **Decision:** **adopt-with-changes** (Approved) — proceed on directional bet; strategy not locked for outbound/pricing until confirmation gates clear.
- **Dissents preserved:** Skeptic — against any reading that the vote locks strategy, funnel targets, or channel economics. Minutes: `board/minutes-atlas-sovereign-smb-coworker-kickoff.md`

## Positioning & Offer

| Element | Spec |
|---------|------|
| Brand | Atlas Sovereign — Agentic AI Division only |
| Hero | Custom AI coworkers for small teams — built to work with you, not around you |
| Offer | Founding partner waitlist → qualified application → discovery → First Agent Scoping / pilot |
| ICP | SMB operators (~10–100); nonprofit then freelance as secondary proof lines only |
| Claim rules | Prefer *digital coworker / AI teammate / scoped agentic assistant*; ban *fully autonomous, replaces staff, guaranteed ROI* |

Full lexicon, messaging house, and visual direction: `campaigns/02-content-seo.md`

## Channel Plan Summary

[Full plan: `campaigns/01-channel-plan.md`]

- **P0:** Founder LinkedIn organic (3–5 posts/week) + client-hosted waitlist + 3-touch nurture (post-approve)
- **P2 optional:** ≤$500 LinkedIn boost ASSUMPTION after organic proof
- **Cadence:** Week 0 prep → soft ramp W1–3 → North Star push W4–6 (only if instruments healthy)
- **Funnel bands 40–60 / 12–18 / 4–6:** ASSUMPTION / capacity-contingent — not commits
- **Tests (max 3):** CTA placement; form friction; single boost on winner post

## Content & SEO Summary

[Full plan: `campaigns/02-content-seo.md`]

- Claim lexicon (allowed / banned / soften) — required before public copy
- 3 pillars: trustworthy coworkers · human-in-the-loop · fit stack then improve
- SEO spine: 1 cornerstone + 4 pillar posts; keyword clusters A–E
- 45-day calendar with `utm_content` IDs
- Visual direction notes (not full brand book)

## Measurement & HubSpot Contract

[Full plan: `campaigns/03-measurement.md`]

**North Star:** Qualified waitlist Lead → discovery booked (`sql_at`) ≤14 days after `qualified_lead_at`

| Lifecycle | Entry | Exit | Owner |
|-----------|-------|------|-------|
| Raw waitlist | Form submit | Qualify pass/fail | System |
| Lead | Qualified form (checklist + no hard DQ) | MQL or DQ | System + sales-lead |
| MQL | Score/engagement threshold | Booking ask | System / sales-lead |
| SQL | Discovery booked or invite accepted | Opportunity or recycle | sales-lead |
| Opportunity | Pilot / First Agent Scoping / SOW path | — | sales-lead |

**SOR:** HubSpot preferred **or** one fallback ledger — never both.  
**UTM:** `utm_campaign=atlas-agentic-kickoff-2026`; `experiment_id=002-marketing_team_workflow`

## Activation / Sales Handoff

[Full SOP: `campaigns/04-activation.md`]

- ≤48h time-to-first-touch; ~3–4 discovery slots/week ASSUMPTION
- ICP checklist (MQL floor ≥8/12) + hard disqualifiers
- Waitlist field spec; discovery agenda + claim-safe talk track
- 3-touch nurture outline — **DRAFT ONLY** until nurture-approve
- No-show recovery + referral ask scripts

## Client Confirmation Package (gates before pricing / outbound)

- [ ] Budget band + media ceiling + engagement commercial model
- [ ] Package delivery date + any hard launch date
- [ ] Named claim/legal reviewer + approved claim lexicon
- [ ] Founder LinkedIn cadence commitment
- [ ] Waitlist page owner / ship path (client hosts)
- [ ] Discovery owner + bookable calendar + ≤48h SLA
- [ ] One measurement SOR + Lead→MQL→SQL→Opportunity field contract
- [ ] Human **nurture-approve** before any email/LinkedIn send

## Open Questions

- [ ] Domain, logo, social handles, existing assets
- [ ] Public founder voice identity
- [ ] HubSpot portal vs fallback ledger choice
- [ ] Any must-win warm pilot prospect already in pipeline
- [ ] Package fee `[TBD]`

## Execute Hooks Checklist

*(Not implemented in this stage — optional after DELIVER)*

- [ ] HubSpot lists / workflows / property create from `leads/hubspot-properties.md` + measurement contract
- [ ] n8n schedules / webhooks (form → SOR; calendar → `sql_at`)
- [ ] SocialClaw drafts (founder LinkedIn pack)
- [ ] Ads (P2) — LinkedIn boost only if media unlocked

## Human Sign-off

- [x] DELIVER  
- [ ] REVISE  

**Notes:** Delivered 2026-07-24 — client package complete. execute_hooks not requested; pipeline closed.
