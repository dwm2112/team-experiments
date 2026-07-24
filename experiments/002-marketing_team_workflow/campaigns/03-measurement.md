# Measurement Plan: Atlas Sovereign Agentic Kickoff

**Experiment:** `002-marketing_team_workflow`  
**Pipeline:** `marketing-client`  
**Client / brand:** Atlas Sovereign — Agentic AI Division  
**UTM campaign:** `atlas-agentic-kickoff-2026`  
**Date:** 2026-07-24  
**Owner role:** revops-analyst  
**Status:** Design-package draft — measurement prerequisites are **kill criteria** before package pricing or outbound

---

## Metric tree (North Star → leading → lagging)

### North Star (executive)

**Definition:** Count of contacts who (1) become a **qualified waitlist Lead**, then (2) reach **SQL** by booking a discovery call (or accepting a calendar invite) **≤14 days** after qualified-Lead timestamp.

| Field | Rule |
|-------|------|
| Numerator | Contacts with `sql_at` set AND `sql_at − qualified_lead_at ≤ 14 days` |
| Denominator (rate view) | Contacts with `qualified_lead_at` in the kickoff window |
| Window | Kickoff working window (ASSUMPTION: 6 weeks after waitlist live; dates `[TBD]`) |
| Tag filter | `experiment_id = 002-marketing_team_workflow` AND `utm_campaign = atlas-agentic-kickoff-2026` (or equivalent first-touch campaign property) |

**Do not** treat impressions, raw waitlist volume, or LinkedIn follows as North Star proxies in executive reporting.

### Leading indicators (weekly operating)

| Metric | Definition | Cadence | Why it leads North Star |
|--------|------------|---------|-------------------------|
| Founder LinkedIn publish rate | Posts published / week (claim-safe) | Weekly | Feeds ICP traffic to waitlist |
| ICP-impression proxy | LinkedIn impressions on posts with waitlist CTA (directional) | Weekly | Early signal of reach; not a commit |
| Waitlist form starts | Form view → first field interaction | Weekly | Funnel health before submit |
| Waitlist submits (raw) | Form submissions with valid email | Weekly | Top of acquisition |
| Qualified Lead rate | Qualified Leads / raw submits | Weekly | Filters vanity signups |
| MQL rate | MQLs / Qualified Leads | Weekly | Nurture engagement quality |
| Time-to-first-touch (TTF) | Hours from Qualified Lead → first human/system touch | Weekly | SLA health (target ≤48h) |
| Book-link CTR | Clicks on discovery booking CTA / MQL | Weekly | Intent before SQL |
| ≤14d booking rate | North Star rate on trailing 14d Qualified Lead cohort | Weekly | Direct North Star pulse |

### Lagging indicators (end-of-window / biweekly)

| Metric | Definition | Note |
|--------|------------|------|
| Discovery show rate | Attended / booked | Capacity & nurture quality |
| Opportunity create rate | Opportunity / SQL | Kickoff metric stops at SQL handoff; Opportunity is lagging success |
| ASSUMPTION funnel band | 40–60 waitlist → 12–18 discovery → 4–6 Opportunity | **ASSUMPTION / capacity-contingent — not commits** until founder calendar + budget band confirmed |
| Soft ramp vs full window | Weeks 1–3 soft; weeks 4–6 full North Star push | Per board; do not judge weeks 1–3 against full-band ASSUMPTIONS |

### Explicit non-goals

- Optimize for impressions, video views, or follower count alone  
- Dual-count the same contact across two SORs  
- Commit to 40–60 / 12–18 / 4–6 before client confirmation package clears  

---

## UTM taxonomy

**Required on every paid or tracked CTA** (LinkedIn posts, boosts, email, DM short-links):

| Parameter | Value / pattern | Required |
|-----------|-----------------|----------|
| `utm_campaign` | `atlas-agentic-kickoff-2026` | Yes |
| `utm_source` | `linkedin` \| `email` \| `dm` \| `referral` \| `direct` \| `other` | Yes |
| `utm_medium` | `organic-social` \| `paid-social` \| `email` \| `message` \| `referral` \| `qr` | Yes |
| `utm_content` | `{pillar}-{asset}-{variant}` | Yes |
| `utm_term` | `smb` \| `nonprofit` \| `freelance` | Optional; preferred when segment-aware |

### `utm_content` tokens

| Token | Allowed values (v1) |
|-------|---------------------|
| `{pillar}` | `coworker` \| `hitl` \| `stack` |
| `{asset}` | `post` \| `cornerstone` \| `principles` \| `faq` \| `scoping` \| `waitlist` \| `nurture1` \| `nurture2` \| `nurture3` |
| `{variant}` | `a` \| `b` \| short slug e.g. `ops-capacity` |

**Examples**

```text
https://{waitlist-host}/waitlist?
  utm_campaign=atlas-agentic-kickoff-2026
  &utm_source=linkedin
  &utm_medium=organic-social
  &utm_content=coworker-post-ops-capacity
  &utm_term=smb
```

### Capture & persistence rules

1. On first form submit, persist raw UTM params to contact properties (first-touch).  
2. Also store **last non-direct** `utm_content` + `utm_source`/`utm_medium` on each tracked click that lands on waitlist or book link (tactical).  
3. Always set `experiment_id = 002-marketing_team_workflow` on Contact (and Deal when created).  
4. Never invent UTMs after the fact to “fix” attribution; if missing → `utm_source=direct`, `utm_medium=none`, `utm_content=unknown`.

---

## HubSpot lifecycle contract (table)

**Canonical stages for this kickoff** (board): **Lead (qualified form) → MQL → SQL (discovery booked) → Opportunity**.

Map to HubSpot standard `lifecyclestage` values below. Do **not** use `subscriber` as the North Star entry for this campaign; raw waitlist that fails qualification stays operationally tagged but does **not** enter Lead.

| Campaign stage | HubSpot `lifecyclestage` | Entry criteria (all must pass) | Timestamp property (custom) | Owner |
|----------------|--------------------------|--------------------------------|-----------------------------|-------|
| Raw waitlist (pre-Lead) | *(no advance)* keep prior or unset; set `hs_lead_status=WAITLIST_RAW` | Valid email submitted; form received | `waitlist_submitted_at` | System |
| **Lead** (qualified form) | `lead` | Passes qualification checklist; fails zero hard disqualifiers; `experiment_id` set | `qualified_lead_at` | System + sales-lead review exceptions |
| **MQL** | `marketingqualifiedlead` | Lead + engagement threshold (see Scoring) OR sales marks sales-ready | `mql_at` | System (workflow) |
| **SQL** | `salesqualifiedlead` | Discovery **booked** or calendar invite **accepted**; bookable instrument logged | `sql_at` | System (calendar sync) or sales-lead manual |
| **Opportunity** | `opportunity` | Deal created for scoped pilot / First Agent Scoping / SOW path | `opportunity_at` (+ Deal id) | sales-lead |

### Stage movement rules

| Rule | Spec |
|------|------|
| Forward-only default | Stages only move forward during kickoff unless explicit recycle |
| Recycle | No-show or disqualify after Lead → set `hs_lead_status=DISQUALIFIED` or `NO_SHOW`; do **not** delete; do **not** keep counting toward North Star |
| SQL clock | North Star uses `qualified_lead_at` → `sql_at` ≤14 days |
| Opportunity | Create Deal with `experiment_id`, `pipeline_stage_internal=kickoff_opportunity`, associate Contact |
| Identity | Email is primary key; company domain secondary |

### Required contact properties (kickoff contract)

| Property | Type | Required at | Notes |
|----------|------|-------------|-------|
| `email` | standard | Raw submit | Identity |
| `firstname`, `lastname` | standard | Lead | Prefer both; one + company may pass if checklist allows |
| `company`, `jobtitle` | standard | Lead | ICP fit |
| `experiment_id` | custom | Lead | Fixed: `002-marketing_team_workflow` |
| `mql_criteria_version` | custom | MQL | `kickoff-v1` |
| `opportunity_score` | custom number | Lead→MQL | 0–100 |
| `pipeline_stage_internal` | custom | Each stage | `waitlist_raw` \| `lead` \| `mql` \| `sql` \| `opportunity` |
| `lead_source_detail` | custom | Lead | e.g. `linkedin-organic-waitlist` |
| `utm_campaign`, `utm_source`, `utm_medium`, `utm_content`, `utm_term` | custom or HS analytics | Lead | First-touch snapshot |
| `utm_content_last` | custom | On book click | Last non-direct content id |
| `icp_segment` | custom enum | Lead | `smb` \| `nonprofit` \| `freelance` |
| `qualified_lead_at`, `mql_at`, `sql_at`, `opportunity_at` | custom datetime | Stage entry | ISO timestamps |
| `discovery_owner` | custom | SQL | Person responsible for call |
| `time_to_first_touch_hours` | custom number | After first touch | Measured vs ≤48h SLA |

---

## Scoring / field definitions + hard disqualifiers

### Scorecard version

`mql_criteria_version = kickoff-v1`

### Qualification checklist → Lead (qualified form)

Contact becomes **Lead** only if **all** fit gates pass and **zero** hard disqualifiers fire.

| Gate | Pass condition | Field / evidence |
|------|----------------|------------------|
| Identity | Work-capable email (not disposable if detectable) | `email` |
| Org signal | Company name or website present | `company` / `website` |
| Role signal | Operator / owner / COO / ops / program lead (or equivalent) | `jobtitle` + form role field |
| Segment | Primary `smb` (10–100 emp ASSUMPTION band) OR secondary nonprofit/freelance tagged honestly | `icp_segment`, `company_size_band` |
| Problem fit | States repeatable ops workflow pain (not “chatbot curiosity” only) | Form: `primary_use_case` |
| Augmentation fit | Accepts human-in-the-loop / coworker framing (not “replace my team”) | Form: `automation_goal` ≠ replace-staff |
| Claim-safe consent | Agrees to founding-partner / discovery process | Form consent |

### `opportunity_score` (0–100) — kickoff-v1

| Component | Points | Scoring |
|-----------|--------|---------|
| Segment fit | 0–30 | SMB 10–100 = 30; nonprofit = 20; freelance/solo = 10; unknown = 0 |
| Role fit | 0–20 | Owner/COO/ops lead = 20; manager = 12; IC/other = 5 |
| Pain concreteness | 0–20 | Named workflow + tools = 20; vague “need AI” = 5 |
| Timeline | 0–15 | Exploring ≤90d = 15; 3–6 mo = 8; no timeline = 0 |
| Engagement | 0–15 | Reserved for MQL uplift (nurture clicks / replies) |

**Lead minimum:** checklist pass (score may still be low).  
**MQL minimum:** Lead + (`opportunity_score ≥ 60` **OR** ≥1 meaningful nurture engagement + score ≥ 45) **OR** sales-lead manual MQL.  
**SQL:** booking event only — score does not create SQL.

### Hard disqualifiers (auto — block Lead / strip from North Star)

| Disqualifier | Detection | Action |
|--------------|-----------|--------|
| Wants employee replacement / “fully autonomous digital employees” as outcome | Form `automation_goal` or notes | `hs_lead_status=DISQUALIFIED`; reason=`replace_staff` |
| Commodity chatbot / plug-and-play only | Form `build_preference=commodity` | Disqualify; reason=`commodity_only` |
| Students / job-seekers / vendors pitching | Role + free-text | Disqualify; reason=`non_buyer` |
| Robotics / Intelligence division inquiry (off-kickoff) | Form `division_interest` ≠ agentic | Route out of kickoff; reason=`wrong_division` |
| No bookable path possible (no email, fake domain) | Validation | Reject submit / Disqualify |
| Explicit “guaranteed ROI” shopping only with no workflow | Notes | Disqualify; reason=`guarantee_seeker` |

### Soft holds (do not book discovery yet)

- Segment = freelance **and** score < 45 → nurture-only, later wave  
- Missing calendar capacity this week → queue; do not fake SQL  
- Claim-review flag on contact notes → pause outbound touches  

---

## Single SOR decision tree (HubSpot vs fallback)

**Board rule:** Exactly **one** system of record for leads, lifecycle, and North Star. No dual tracking.

```text
START: Client confirmation package — measurement SOR
│
├─ HubSpot portal available + private app scopes OK
│  + contact/company/deal write works
│  + forms or webhook can upsert Contact
│  + calendar booking can write sql_at (native Meetings or Zap/n8n)
│     → SELECT SOR = HubSpot
│     → Create properties in leads/hubspot-properties.md + this contract
│     → All lifecycle & North Star reports read HubSpot only
│
├─ HubSpot NOT available OR scopes blocked OR no owner to operate portal
│     → SELECT SOR = Fallback Spreadsheet Ledger (single Google Sheet / Excel)
│        Tab: contacts
│        Columns: email, names, company, jobtitle, icp_segment, scores,
│                 lifecycle_stage, timestamps, UTMs, experiment_id,
│                 discovery_owner, booking_url, deal_flag
│        Rules identical to HubSpot contract (same stage names & clocks)
│     → Forms write ONLY to this ledger (via form tool export or n8n)
│     → Calendar bookings update sql_at in the SAME ledger only
│
└─ Neither HubSpot nor single fallback ledger ready
       → KILL: do not price package for launch; do not outbound
```

### Hard bans

- HubSpot **and** spreadsheet both “live” for stage changes  
- GA4 as lifecycle SOR (GA4 may receive events; it is **not** the lead SOR)  
- Founder inbox as undocumented SOR  

### Preferred default

**HubSpot** (aligns with repo SOR table). Fallback is allowed only as a **complete** substitute with the same field contract.

---

## Events & instrumentation checklist

Complete **before** outbound / paid / public waitlist push. Incomplete → kill criteria.

### A. Identity & SOR

- [ ] SOR chosen (HubSpot **or** fallback ledger) documented in client confirmation package  
- [ ] `experiment_id` stamped on every new kickoff contact  
- [ ] Property/column set for lifecycle timestamps live  
- [ ] Dedupe rule: email match updates existing contact (no duplicate North Star credit)

### B. Waitlist form

- [ ] Fields: name, email, company, jobtitle, company_size_band, icp_segment, primary_use_case, automation_goal, build_preference, consent  
- [ ] Server- or tool-side validation on email  
- [ ] On submit: persist UTMs + `waitlist_submitted_at`  
- [ ] Qualification workflow runs ≤15 minutes → set Lead or DISQUALIFIED  
- [ ] Event name: `waitlist_submit` (to SOR + optional GA4)

### C. Nurture instrumentation

- [ ] 3-touch sequence IDs: `nurture1`, `nurture2`, `nurture3` in `utm_content`  
- [ ] Click → `nurture_click` logged on contact; can uplift Engagement score  
- [ ] Unsubscribe / LinkedIn decline stops sequence; stage unchanged unless disqualified

### D. Booking instrument (mandatory)

- [ ] Single discovery calendar link owned by named `discovery_owner`  
- [ ] Booking or invite-accept writes `sql_at` + advances lifecycle to SQL  
- [ ] Event name: `discovery_booked`  
- [ ] No-show workflow sets `hs_lead_status=NO_SHOW` (or ledger equivalent); recovery touch scheduled  
- [ ] ≤48h time-to-first-touch measured into `time_to_first_touch_hours`

### E. Optional analytics (non-SOR)

- [ ] GA4 (if site live): `waitlist_submit`, `nurture_click`, `discovery_booked`  
- [ ] LinkedIn post UTM short-links used for all CTA posts  
- [ ] If ≤$500 boost: campaign named `atlas-agentic-kickoff-2026`; spend logged weekly (manual OK)

### F. Pre-flight test (must pass)

- [ ] Test contact traverses Raw → Lead → MQL → SQL with timestamps  
- [ ] North Star query returns the test contact when SQL ≤14d  
- [ ] Disqualifier test contact does **not** enter Lead  
- [ ] Dual-SOR write test fails closed (only one destination configured)

---

## Attribution (tactical vs executive)

### Executive (board / client)

| Report | Model | Includes | Excludes |
|--------|-------|----------|----------|
| North Star | Qualified Lead → SQL ≤14d | Count + rate by week; segment breakout | Impressions, raw submits, follows |
| Funnel ASSUMPTION check | Cohort of Qualified Leads | Conversion Lead→MQL→SQL→Opportunity | Treating ASSUMPTION bands as commits |
| SLA | Median/p75 TTF hours | ≤48h compliance % | Vanity engagement |

**Executive rule:** One number story — *Are qualified waitlist leads booking discovery inside 14 days?* Channel debate is secondary.

### Tactical (growth / content / sales ops)

| Decision | Model | Key dimension |
|----------|-------|----------------|
| Which post/asset to boost or repeat | Last non-direct `utm_content` on waitlist submit | `utm_content` |
| Which nurture touch books | Last non-direct `utm_content` on book click (`utm_content_last`) | nurture1/2/3 |
| Segment creative | `utm_term` + `icp_segment` | smb vs nonprofit vs freelance |
| Source mix | First-touch `utm_source`/`utm_medium` | organic-social vs paid-social vs email |

**Tactical rule:** Use content IDs to allocate founder time and optional ≤$500 boost. Do **not** promote tactical winners into executive North Star redefinition.

### Conflict resolution

If tactical volume is high but ≤14d booking rate <25% by end of week 3 (post-instrument lock) → **revoke any commit framing**, pause boost spend, reopen board per RevOps falsifier — do not “fix” with impression narratives.

---

## Weekly review ritual

**Cadence:** 30 minutes, same weekday after waitlist is live  
**Chair:** revops-analyst (or founder ops designee)  
**Required attendees:** growth-marketer, sales-lead; content-strategist as needed  

### Agenda (fixed)

1. **SOR hygiene (5m)** — Duplicate contacts? Missing timestamps? Dual-write incidents?  
2. **North Star pulse (5m)** — Trailing Qualified Lead cohort ≤14d booking rate; absolute SQL count  
3. **Leading panel (10m)** — Publish rate, qualified Lead rate, TTF SLA, book-link CTR  
4. **ASSUMPTION bands (5m)** — Compare to soft-ramp / full-window bands **without** converting to commits  
5. **Kill / gate check (5m)** — Any kill criterion true? Outbound still allowed?

### Standard weekly snapshot (paste into blackboard or Sheet)

| Field | Value |
|-------|-------|
| Week # / dates | |
| SOR | HubSpot / Fallback |
| Raw submits | |
| Qualified Leads | |
| MQLs | |
| SQLs (booked) | |
| North Star ≤14d rate | |
| Median TTF (hours) | |
| Top `utm_content` → Leads | |
| Top `utm_content_last` → SQL | |
| Disqualified count (by reason) | |
| Kill flags | none / list |
| Decision | continue / pause boost / reopen board |

### Reporting hygiene

- Label every numeric funnel target **ASSUMPTION** until client confirmation package + capacity lock  
- Screenshot or export from **one SOR only**  
- No API keys in blackboard files  

---

## Kill criteria (measurement)

**Auto-stop package pricing and all outbound** (email / LinkedIn / Upwork / paid) if any are true:

| # | Kill criterion | Verification |
|---|----------------|--------------|
| K1 | **No single SOR chosen** (HubSpot preferred **or** one fallback ledger) | Client confirmation package unchecked; dual tools writing stages |
| K2 | **Scoring / lifecycle contract incomplete** — Lead→MQL→SQL→Opportunity fields, timestamps, or hard disqualifiers not implemented in SOR | Pre-flight test A–F fails |
| K3 | **Booking instrument incomplete** — no named discovery owner, no calendar that writes `sql_at`, or SQL cannot be measured | Cannot fire `discovery_booked` end-to-end |
| K4 | North Star **clock** cannot be computed (`qualified_lead_at` or `sql_at` missing by design) | Query returns null for all contacts |
| K5 | Post-lock dual-tracking detected (stages diverge across tools) | Audit finds two live writers |
| K6 | Week-3 falsifier (after instruments locked): qualified→discovery **<25%** with non-trivial Qualified Lead volume | Weekly ritual; reopen board; revoke commit framing |

### Related non-measurement kills (from board — still block outbound)

- Claim lexicon refused / banned autonomy-replacement copy insisted  
- Founder cadence or discovery capacity not committed  
- Waitlist host path unresolved  
- Robotics/Intelligence creative bleed  

### Resume rules

Measurement kills clear only when:

1. One SOR operational,  
2. kickoff-v1 scorecard + disqualifiers live,  
3. booking → SQL instrumentation passes pre-flight,  
4. Human nurture-approve checkpoint still required before send.

---

## Assumptions Made

- Funnel bands 40–60 / 12–18 / 4–6 are **ASSUMPTIONS**, not commits.  
- HubSpot is preferred SOR; fallback ledger is acceptable only as a full substitute.  
- Client hosts waitlist; this package specifies measurement, not implementation.  
- Discovery SLA ≤48h TTF and ≤14d Lead→SQL clock are board-aligned operating targets.  
- GA4/ads are optional event sinks, never lifecycle SOR.

## Out of Scope

- Building the waitlist site or HubSpot portal  
- Auto-sending nurture without human approve  
- Multi-touch revenue attribution models beyond last non-direct content id  
- Customer/evangelist lifecycle stages for kickoff success reporting  
- Robotics / Intelligence division tracking  

## Security Notes

- No API keys or private app tokens in blackboard files; use env / n8n.  
- Prefer work emails; minimize sensitive ops detail in free-text form fields.  
- Disqualified and raw contacts retained for audit — do not republish lists without consent basis.
