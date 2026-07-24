# Client Letter — Concerns & Information Request

**Experiment:** 003-dla-dod-solicitation-scraper
**Purpose:** First-contact Upwork message: signal fit, surface the concerns from `01-qualification.md`, and request the evidence we need before scoping/pricing.
**Status:** DRAFT — fill every `[FIELD]` before sending. Keep it warm and specific; this doubles as a filtering signal that we understand DIBBS/DLA.

---

## Paste-ready body

```
Hi [CLIENT_NAME],

Your post is squarely in my wheelhouse — Python automation that pulls government
solicitations (DIBBS / DLA in particular), filters them, and lands clean, usable
data for the team. I've done [ONE_LINE_RELEVANT_EXPERIENCE — e.g. "similar
scraping + data-extraction pipelines against structured/semi-structured portals
and delivered them as scheduled CSV/Excel feeds"].

Before I quote an approach or price, I want to be straight about a few things that
will make or break this build — and ask for the details that let me scope it
accurately rather than guess.

Access & compliance (the big one):
DIBBS behaves differently depending on how you reach it. A reliable, maintainable
pull usually depends on approved/authenticated access rather than anonymous
scraping, which tends to break and can run into terms-of-use or rate limits.
  1. Do you have a DIBBS account (or approved API/data access) I can build
     against, or should the tool operate as a public, unauthenticated user?
  2. Are there any internal rules on your side about how this data may be
     collected or stored that I should design around?

Defining "online pricing":
You listed online pricing, competitive historical pricing, NSN-level price
history, and automated quote functionality. These can appear in different places
on a solicitation, so I want to match your mental model exactly.
  3. Could you share 1–2 example solicitations (links or screenshots) that DO
     have the pricing you care about, and 1 that does NOT? That single artifact
     removes most of the guesswork.
  4. Of the fields you listed (item description, part number, NSN, quantity,
     online price, CAGE code, vendor pricing links) — which are must-haves for
     v1 vs nice-to-have later?

Scope, phasing & rate:
The full ask (retrieval + filters + pricing detection + CSV/Excel/Sheets +
daily/hourly scheduling + extensibility to new portals) is very doable, but I'd
suggest shipping it in phases so you get value fast and we don't over-build
before the data is proven. My instinct:
  - Phase 1: DIBBS retrieval + FSC/PSC / date / delivery filters + core fields → CSV/Excel
  - Phase 2: online-pricing detection (using your examples) + links
  - Phase 3: Google Sheets output + scheduled daily/hourly runs + docs
  5. Does a phased approach work for you, and is there a target rate range for
     this engagement? [OPTIONAL: "My rate is $[RATE]/hr." ]

One clarification:
  6. Your required skills list "Document AI" as mandatory, but the scope reads
     like structured scraping/parsing. Is there a PDF/scanned-document or OCR
     component I should plan for (e.g. solicitation attachments), or was that
     tag more of a general data-extraction signal?

I'm [LOCATION/US_ELIGIBILITY_STATEMENT] and available [HOURS_PER_WEEK] hrs/week,
which fits your under-30 / 1–3 month framing with room to continue as an ongoing
project.

If it's easier, a quick [CALL_LENGTH — e.g. "15-minute"] call works too — or send
the example solicitation and I'll come back with a concrete Phase 1 outline.

Thanks,
[YOUR_NAME]
[OPTIONAL_CONTACT_OR_PROFILE_LINK]
```

---

## Fill-in guide

| Field | What to put |
|-------|-------------|
| `[CLIENT_NAME]` | Client's name/handle if known; else drop to just "Hi," |
| `[ONE_LINE_RELEVANT_EXPERIENCE]` | One true, specific line — scraping/data-extraction/scheduled-export work. Never invent. |
| `[RATE]` | Only include the optional rate line if you want to anchor; match/omit based on strategy |
| `[LOCATION/US_ELIGIBILITY_STATEMENT]` | e.g. "US-based (Austin, TX)" — must be honest; post is US-only |
| `[HOURS_PER_WEEK]` | Sustainable weekly hours (≤30 to match post) |
| `[CALL_LENGTH]` | e.g. "15-minute" |
| `[YOUR_NAME]` | Name as on Upwork |
| `[OPTIONAL_CONTACT_OR_PROFILE_LINK]` | Optional |

## Concerns → questions traceability

Each question maps to a condition/risk in `intake/01-qualification.md`:

| Concern (from qualification) | Covered by question |
|------------------------------|----------------------|
| Federal portal ToS / auth / anonymous-scrape risk | Q1, Q2 |
| "Online pricing" undefined | Q3, Q4 |
| Scope-creep / unbounded multi-portal & pricing intel | Q5 (phasing) |
| Unknown rate band | Q5 (rate) |
| Document AI mandatory vs scope mismatch | Q6 |
| US-only hard filter | Location/eligibility line |
| Duration/hours fit | Hours line |

## Send checklist

- [ ] All `[FIELD]`s replaced (or deliberately removed)
- [ ] US eligibility stated honestly
- [ ] Experience line is true and specific
- [ ] Rate line kept or cut on purpose
- [ ] Reads like a person, not a form — trim any question you already know the answer to
- [ ] Attach/point to nothing you haven't verified you can do
