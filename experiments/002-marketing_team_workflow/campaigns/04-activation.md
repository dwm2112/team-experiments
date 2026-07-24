# Activation & Sales Handoff: Atlas Sovereign Agentic Kickoff

**Experiment:** `002-marketing_team_workflow`  
**Pipeline:** marketing-client  
**Role owner:** sales-lead (founder or designated closer executes)  
**Offer path:** Founding partner waitlist → qualify → discovery → First Agent Scoping / pilot  
**North Star (definition):** Qualified waitlist signup books discovery (or accepts calendar invite) within 14 days  
**Status:** Design SOP — drafts only; no outbound until gates + human nurture-approve

---

## Funnel stages & owners

| Stage | Definition (operational) | Owner | Exit criteria |
|-------|--------------------------|-------|---------------|
| **Subscriber** | Waitlist form submitted; auto-confirm sent | growth (page) + sales-lead (follow-up queue) | Valid email + form complete |
| **Lead** | Application fields complete; fit screen started | sales-lead | ICP checklist scored |
| **MQL** | Passes ICP checklist; no hard disqualifier; engaged (opened/replied or LinkedIn connect) | sales-lead | Ready for booking ask |
| **SQL** | Discovery booked **or** accepted calendar invite | sales-lead / closer | Event on calendar ≤14d of signup preferred |
| **Opportunity** | Discovery complete; First Agent Scoping / pilot SOW path agreed | sales-lead | Written next step (scoping call or pilot proposal) |
| **Disqualified / nurture hold** | Hard DQ or soft “not now” | sales-lead | Tagged reason; optional long-cycle nurture later |

**Owner of record for kickoff:** Founder (or named closer). Growth/content create demand; they do not book or run discovery unless delegated in writing.

**Lifecycle mapping (ASSUMPTION — HubSpot if SOR locked):**  
`subscriber` → `lead` → `marketingqualifiedlead` → `salesqualifiedlead` → `opportunity`  
Tag: `experiment_id=002-marketing_team_workflow`, `utm_campaign=atlas-agentic-kickoff-2026`

---

## ICP qualification checklist + hard disqualifiers

Score each **fit** item 0–2 (0 = no, 1 = partial, 2 = yes). **MQL floor:** total ≥ 8 / 12 **and** zero hard disqualifiers.

### Fit checklist (primary ICP: SMB ops)

| # | Criterion | 2 | 1 | 0 |
|---|-----------|---|---|---|
| 1 | Org size / complexity | ≈10–100 employees **or** clear multi-person ops load | Solo/freelance with team-like ops pain (secondary) | Hobby / student / no org |
| 2 | Role authority | Owner / COO / ops lead / budget influence | Manager who can intro decision-maker | No path to buyer |
| 3 | Pain | Repeatable ops work, hiring lag, tool sprawl, “need another person” | Vague “interested in AI” | Curiosity only |
| 4 | Use case | Named workflow(s) agents could assist inside existing tools | Directional use case | “Chatbot for website” only |
| 5 | Build posture | Wants **custom-built** scoped assistant / coworker | Open to custom after education | Demands plug-and-play commodity only |
| 6 | Augmentation stance | Accepts human oversight on critical decisions | Unsure; coachable | Explicit “replace my team / fully autonomous employees” expectation |

**Secondary segments (kickoff):** Nonprofit program/ops leads may MQL if checklist passes. Freelance/solo = later wave unless warm referral or clear ACV path — do **not** burn primary discovery slots.

### Hard disqualifiers (auto no-book)

Immediate **do not schedule discovery** if any apply:

1. Wants **fully autonomous** agents that **replace employees** / “fire headcount with AI” as the buying goal  
2. Expects **guaranteed ROI**, “works for any business,” or commodity chatbot SaaS pricing with no scoping  
3. Primary need is **Robotics** or **Intelligence/surveillance** (out of Agentic AI Division kickoff)  
4. No identifiable human + org (fake email, no company/context, spam)  
5. Hostile / abusive / clearly non-commercial troll  
6. Explicit legal/compliance ask that client has not authorized answering (park; escalate to claim reviewer — do not improvise)

**Soft hold (nurture, don’t force book):** timing (“not for 6+ months”), no budget conversation readiness, wrong season — tag `nurture_hold` + reason; do not count toward discovery capacity.

---

## Waitlist application fields (spec)

**Purpose:** Qualify before calendar burn. Keep form short; longer = higher abandon.

### Required fields

| Field | Type | Why |
|-------|------|-----|
| Full name | text | Identity |
| Work email | email | Routing + CRM |
| Company / org name | text | Fit |
| Role / title | text | Authority |
| Approximate team size | select: `1` / `2–9` / `10–49` / `50–100` / `100+` | ICP band |
| Primary pain (one sentence) | textarea (≤280 chars) | Use-case signal |
| Workflow to improve first | textarea (≤280 chars) | Scoping seed |
| Goal stance | select: `Augment our team` / `Not sure yet` / `Reduce headcount with AI` | Hard DQ if reduce headcount |
| Consent | checkbox: OK to contact about founding partner waitlist | Compliance |

### Optional / recommended

| Field | Type | Why |
|-------|------|-----|
| Website / LinkedIn | URL | Enrichment |
| Tools in stack today | multi-select or free text | Fit narrative |
| Timeline | select: `0–30d` / `30–90d` / `90d+` / `Exploring` | Priority |
| How heard | select + UTM passthrough | Attribution |
| Referral name | text | Referral loop |

### Hidden / system

- `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`  
- `experiment_id=002-marketing_team_workflow`  
- `submitted_at` (ISO)  
- `segment_guess` (smb|nonprofit|freelance) if inferable

**Post-submit:** Instant auto-confirm (“you’re on the founding partner list — we’ll reply within 48 hours”). **Do not** auto-book calendar for everyone — qualify first.

---

## MQL → SQL handoff SLA

| Step | SLA | Owner action |
|------|-----|--------------|
| Form → first human touch | **≤48 hours** (business days preferred; weekends: next business day still counts toward spirit of SLA) | Email **or** LinkedIn DM referencing waitlist + one clarifying question |
| Fit screen complete | Same day as first touch when possible; ≤2 business days | Score checklist; apply hard DQ |
| MQL → booking ask | Within first-touch reply or Touch 2 | Send calendar link **only if** MQL |
| Signup → discovery booked | **Target ≤14 days** (North Star) | Persist through 3-touch nurture |
| Discovery → written next step | ≤48h after call | First Agent Scoping agenda **or** polite close / hold |

**SQL definition (kickoff):** Discovery **booked** or invite **accepted** (not “said interested”).

**Handoff checklist (before SQL tag):**

- [ ] No hard disqualifier  
- [ ] Fit score ≥ 8 / 12  
- [ ] Claim-safe language used in all touches  
- [ ] Calendar event created with agenda blurb  
- [ ] CRM/stage updated (`pipeline_stage_internal=sql` or SOR equivalent)  
- [ ] Prep note: pain, stack, size, stance (augment vs replace)

**If capacity full:** Do not overbook. MQL goes to waitlist queue with honest ETA (“next open discovery windows are …”). Never promise a slot you don’t have.

---

## Discovery call agenda (30–45 min) + claim-safe talk track

### Agenda

| Min | Block | Outcome |
|-----|-------|---------|
| 0–3 | Open + frame | Confirm time, goal = fit + scope first agent, not a pitch dump |
| 3–10 | Their world | Team size, tools, who feels the ops pain |
| 10–20 | Workflow deep-dive | One painful, repeatable workflow; inputs/outputs; where judgment must stay human |
| 20–28 | Fit & guardrails | Augmentation model; what we won’t claim; custom build vs commodity |
| 28–38 | First Agent Scoping path | Pilot shape, rough effort bands (no invented ROI), decision process |
| 38–45 | Next step + referral | Book scoping / send summary; ask for 1 warm intro |

### Claim-safe talk track (use / avoid)

**Allowed framing**

- “We build **custom, trustworthy AI coworkers** — agents that help with **defined workflows** alongside your team.”  
- “**Human-in-the-loop** on critical decisions: agent proposes → you decide → agent executes where safe.”  
- “Kickoff path: founding partner waitlist → discovery → **First Agent Scoping** / scoped pilot.”  
- “Built to fit your stack, then improve — not rip-and-replace your systems.”

**Banned / redirect**

| They say / temptation | Do not say | Redirect |
|----------------------|------------|----------|
| “Fully autonomous digital employees” | Agree or amplify | “We design **scoped agentic assistants** with clear guardrails and human oversight.” |
| “Replace my staff” | Promise headcount reduction | “Our model is **augmentation**. If the goal is replacement-first, we’re not the right fit.” |
| “Guaranteed ROI / works for any business” | Guarantees | “Outcomes depend on workflow clarity and adoption. We scope one agent carefully first.” |
| Robotics / intel / surveillance | Cross-sell other divisions | “This kickoff is **Agentic AI Division** only.” |

**Discovery questions (steal sheet)**

1. Which workflow, if improved, would free the most hours this quarter?  
2. Who owns that process today, and where must a human still decide?  
3. What tools must the agent live inside (not beside)?  
4. What have you already tried (chatbots, Zapier, hiring)?  
5. If a first agent earned trust in 30–60 days, what would “good” look like — without inventing ROI numbers?

**Close lines**

- Fit: “Next step is a **First Agent Scoping** session — we map one workflow, guardrails, and a pilot boundary.”  
- No-fit: “I’d rather decline than overpromise. Here’s a resource; stay on the founding partner list for updates.”

---

## 3-touch nurture sequence outline (email or LinkedIn DM) — DRAFT ONLY

> **DRAFT ONLY — do not send until human nurture-approve checkpoint.**  
> Channel: email preferred if work email given; else LinkedIn DM. Same beats either way.  
> Personalize with `{first_name}`, `{company}`, `{pain_snippet}`, `{workflow_snippet}`.

### Touch 1 — Value (Day 0 / within 48h of signup)

- **Goal:** Confirm receipt + show you read the application + one useful frame  
- **Subject (email):** `You’re on the Atlas founding partner list — quick question`  
- **Body beat:** Thanks → restate their pain in their words → 3-sentence “digital coworker ≠ replace judgment” → one clarifying question (stack or workflow owner) → no hard book yet if not screened  
- **CTA:** Reply with answer **or** (if already MQL) offer 2 calendar windows  

### Touch 2 — Trust (Day 3–4 if no book)

- **Goal:** Reduce replacement fear; prove process  
- **Subject:** `How we scope a first agent (without the hype)`  
- **Body beat:** Point to principles / “3 gates before an agent acts” (link when asset live) → short story of propose→decide→execute → ask if discovery still useful this month  
- **CTA:** Book discovery (calendar link) **only if MQL**; else soft “reply and I’ll advise if fit”  

### Touch 3 — Book (Day 7–9 if no book)

- **Goal:** Convert or release capacity  
- **Subject:** `Still want a First Agent Scoping conversation?`  
- **Body beat:** Direct ask → 2 concrete slots → permission to close the loop (“if timing’s off, I’ll pause outreach”)  
- **CTA:** Book **or** “not now” (tag hold)  

**After Touch 3 with no reply:** Move to no-show/stalled recovery cadence (below). Do **not** add a 4th push in week 1.

**Objection one-liners (for replies)**

| Objection | Reply seed |
|-----------|------------|
| “Will this replace people?” | “We won’t take that mandate. We build coworkers with human oversight on critical calls.” |
| “We’re too small / not technical.” | “Custom doesn’t mean enterprise theater — we start with one clear workflow.” |
| “Worried about lock-in.” | “Interoperability is a design principle — we fit your stack first.” |
| “Just send pricing.” | “Pricing follows scope. Discovery is how we avoid fake quotes.” |

---

## No-show / stalled recovery

### No-show (booked, didn’t attend)

| When | Action |
|------|--------|
| +0–2h | Short note: assume conflict; offer 2 reschedule links; no guilt |
| +24h | Second touch: same offer + “happy to send a 5-min async questionnaire instead” |
| +72h | Final: release the slot; invite to rebook when ready; tag `no_show` |
| After rebook | Same agenda; if second no-show → soft DQ from active capacity (keep on list) |

**No-show note (draft):**  
“Looks like we missed each other — totally fine. I still have your waitlist notes on `{workflow_snippet}`. Want to grab one of these times, or should I send a short async scope form?”

### Stalled (MQL, no book after 3 touches)

1. Day 14: one “breakup” touch — value + open door; stop weekly chasing  
2. Tag `stalled_mql`; optional monthly founding-partner update later (separate approve)  
3. Do **not** keep burning LinkedIn DMs — protects founder brand  

### Show-rate hygiene

- Calendar reminder 24h + 1h (native tool)  
- Agenda in invite body (5 bullets max)  
- Confirm timezone  
- Cap same-week rebooks so recovery doesn’t crowd new MQLs  

---

## Capacity model (ASSUMPTION)

> All funnel numbers are **ASSUMPTIONS** and **capacity-contingent**. Not commits until founder calendar + budget band confirmed.

| Parameter | ASSUMPTION |
|-----------|------------|
| Discovery slots | **~3–4 / week** founder or closer |
| Time-to-first-touch | **≤48h** |
| Call length | 30–45 min + 15 min notes |
| Weekly discovery hours | ≈3–4h calls + ≈2h admin/nurture |
| Kickoff window | ~6 weeks working (per brief) |
| Illustrative yield (if capacity holds) | 40–60 waitlist → 12–18 discovery → 4–6 SQL/opportunities |

**Capacity math (use weekly):**

- Max discoveries/week = booked slots (3–4)  
- If MQL backlog > 2× weekly slots → stop aggressive TOFU pushes; lengthen nurture; protect show quality  
- If slots < 2 / week with no closer → **drop numeric targets** (per board falsifier); keep qualify + SLA hygiene only  

**Weekly ops ritual (founder/closer — 30 min)**

1. New waitlist → score → MQL / DQ / hold  
2. Touches due today (T1/T2/T3)  
3. Confirm tomorrow’s discoveries + prep notes  
4. No-show / stalled queue  
5. Update stages in SOR  

---

## Referral ask script

**When:** End of discovery (fit or warm no-fit), and after any good LinkedIn exchange.

**Live (end of call):**  
“We’re keeping the founding partner group small on purpose. Who else in your circle runs ops at an SMB and is feeling hiring lag or tool sprawl? A warm intro beats any cold post — even a two-line email works.”

**Async (email/DM draft):**  
“If one peer comes to mind who’d benefit from a **scoped AI coworker** conversation (not a replacement pitch), I’d appreciate an intro. Happy to lead with a short note you can forward.”

**Forwardable blurb:**  
“Atlas Sovereign’s Agentic AI Division builds custom, trustworthy AI coworkers for small teams — agents for defined workflows with human oversight. They’re talking to founding partners about First Agent Scoping. OK if I intro you?”

**Waitlist reshare ask (after T2 or post-call):**  
“If the waitlist page was useful, sharing with one ops-minded peer helps more than likes — claim-safe coworker framing only.”

Track referrals as source `referral_warm` + referrer name.

---

## Gates before any outbound

**Do not send nurture, LinkedIn DMs at scale, or booking blasts until all are true:**

1. [ ] Client confirmation package cleared (budget band, dates, claim reviewer) — see `analysis/marketing-decision.md`  
2. [ ] Claim lexicon approved (allowed: digital coworker / AI teammate / scoped agentic assistant; banned: fully autonomous employees, replaces staff, guaranteed ROI, any-business)  
3. [ ] Discovery owner + **bookable calendar** live  
4. [ ] ≤48h first-touch owner named  
5. [ ] This qualify checklist + hard DQs accepted by founder  
6. [ ] Measurement SOR chosen (HubSpot **or** single fallback — no dual tracking)  
7. [ ] Waitlist host path live (client ships page from specs)  
8. [ ] Human checkpoint: **APPROVE SEND** on nurture drafts (`flags.nurture_send_approved`)  
9. [ ] No Robotics/Intelligence bleed in copy  
10. [ ] Capacity ≥ ~3 discovery slots/week **or** numeric funnel targets explicitly waived  

**Kill / pause outbound if:** client insists on banned claims; calendar/capacity collapses; SOR/scoring incomplete; waitlist host unresolved.

---

## Founder / closer quick-reference SOP

### Daily (≤20 min when queue non-empty)

1. Pull new waitlist rows  
2. Hard DQ screen → reject politely if needed  
3. Score fit → MQL or hold  
4. First touch within 48h  
5. Log stage + next action date  

### On every MQL

1. Booking ask with ≤2 concrete slots  
2. Invite includes agenda + claim-safe one-liner  
3. Prep note before call  

### After every discovery

1. Same-day summary email (what we heard, fit yes/no, next step)  
2. Stage → Opportunity **or** hold/DQ with reason  
3. Referral ask once  
4. No invented logos, metrics, or ROI in follow-up  

### Never

- Auto-send sequences without approve  
- Book tire-kickers who fail hard DQs  
- Promise autonomy, replacement, or guaranteed outcomes  
- Overbook past capacity to hit vanity waitlist goals  

---

**Related artifacts:** `briefs/campaign-brief.md` · `analysis/marketing-decision.md` · `board/minutes-atlas-sovereign-smb-coworker-kickoff.md`  
**Next package merge:** Include this file under Activation / Sales Handoff in `campaigns/package` (or equivalent campaign package).
