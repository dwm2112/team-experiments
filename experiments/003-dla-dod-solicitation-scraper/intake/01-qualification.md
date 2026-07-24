# Qualification: 003-dla-dod-solicitation-scraper

**RFP:** intake/00-rfp.md  
**Date:** 2026-07-24T03:16:01Z

## Risk verdict

**High** — Strong Python/automation fit and a concrete DIBBS-centered ask, but unknown rate, US-only filter, federal-portal ToS/auth risk, and a 1–3 month scope that mixes scrape + pricing intelligence + multi-export + scheduler + portal extensibility.

## Main assumption

We can deliver a **ToS-safe, authenticated or officially supported** DIBBS pull (not brittle anonymous HTML scrape) that reliably flags “online pricing” the way the client means it — and that Intermediate hourly economics cover discovery of DIBBS quirks without unpaid reverse-engineering.

## Evidence to find first

Client answers to: (1) Do they have / can they provide DIBBS login or approved access? (2) What rate band? (3) What does “online pricing” look like on a sample solicitation (URL or screenshot)? (4) Why is Document AI mandatory if scope is scrape/export?

## Do next

**GO-WITH-CONDITIONS:** draft a short proposal that sells a **DIBBS MVP** (filter → detect pricing flags → CSV/Excel first; Sheets/scheduler as phase 2) and puts clarifying questions up front. Do not deep-design multi-portal extensibility until access + pricing definition + rate are confirmed.

## Delay

Full multi-portal architecture; unpaid sample scraper against live DIBBS; claiming Document AI / OCR pipeline unless client explains the need; Google Sheets + hourly scheduler as day-one commitments.

## Checklist scores

| Lens | Score (1–5) | Notes |
|------|-------------|-------|
| Demand / fit | 4 | Python scrape/API/automation + federal procurement niche matches well if we have (or can learn) DIBBS |
| Positioning | 4 | One-liner: reliable DIBBS solicitation pull + online-pricing filter with clean tabular export and ops docs |
| Monetization / rate vs scope | 2 | Hourly Intermediate, no band; scope looks like productized intel tool — easy to underprice discovery |
| Duration fit | 3 | 1–3 months / &lt;30 hrs can fit an MVP; “ongoing” helps, but full ask may overrun |
| Trust / filters | 3 | US-only hard filter must be honest; federal data + scraping optics need clear compliance posture |
| Win probability | 3 | Niche (DIBBS/DLA) can thin competition; Document AI tag + Intermediate rate may attract low-ball scrape bids |
| Scope-creep risk | 2 | Pricing-history / quote-automation / new portals are unbounded without samples and phased SOW |

## Recommended verdict

**GO-WITH-CONDITIONS**

Conditions:
- Confirm **US location eligibility** before applying
- Proposal must **phase** work: DIBBS retrieval + filters + core fields → pricing detection with client-provided examples → export (CSV/Excel) → schedule → Sheets / extra portals
- Explicitly address **access & compliance** (account, robots/ToS, rate limits) — no anonymous scrape promise
- Get **rate band** (or floor) before investing past proposal
- Clarify **Document AI** mandatory skill vs stated scope (mismatch = red flag or different workstream)
- Cap unpaid clarification; push paid discovery if reverse-engineering DIBBS UI is required
