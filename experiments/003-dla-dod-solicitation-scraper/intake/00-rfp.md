# RFP — Python Web Scraping Specialist for DLA/DoD Solicitations

**Captured:** 2026-07-24T03:16:01Z  
**Source:** `c:\Users\softw\Downloads\Python Web Scraping Specialist for DLA_DoD Solicitations.txt`  
**Experiment:** `experiments/003-dla-dod-solicitation-scraper/`

## Structured extract

| Field | Value | Confidence |
|-------|-------|------------|
| Title / one-liner | Python automation to pull DLA/DoD solicitations (esp. DIBBS) and filter for online pricing | H |
| Budget / rate | Hourly; no rate band stated | H (unknown rate) |
| Hours / duration | &lt;30 hrs/week; 1–3 months; Intermediate; Ongoing project | H |
| Location constraints | **US-based freelancers only** (hard filter) | H |
| Stated stack | Python; Document AI; Data Extraction; web scraping, APIs, parsing, automation | H |
| Integrations named | DIBBS / DLA Internet Bid Board System; CSV, Excel, Google Sheets; optional “new portals or pricing sources” | H |
| Deliverables | Automated solicitation retrieval + filters; online-pricing detection/extraction; CSV/Excel/Sheets output; daily/hourly schedule; runbooks for filters and portal extension | H |
| Vague-ask signals | “Online pricing / competitive historical / NSN price history / automated quote” detection criteria undefined; Document AI listed as mandatory but unused in scope text; no sample NSNs/FSCs; no ToS/auth posture | H |
| Red flags | Federal portal scraping may violate ToS / require accounts / CAPTCHA; legal/compliance risk; scope spans scrape + pricing intelligence + multi-export + scheduler + extensibility in 1–3 months; rate unknown vs Intermediate mix-of-value framing | H |
| Opportunity signals | Clear primary portal (DIBBS/DLA); concrete field list; automation + docs valued; ongoing project may extend past initial build; domain familiarity (DIBBS/DLA) is differentiator | M |

## Raw text

```
Python Web Scraping Specialist for DLA/DoD Solicitations
Posted 12 hours ago
Only freelancers located in the U.S. may apply.
Summary
I’m looking for an experienced Python developer who can build an automated workflow that pulls government solicitations and filters them based on specific criteria, especially solicitations that have online pricing available.


This role requires someone who understands web scraping, APIs, data parsing, and automation, ideally with someone who understands DIBBS, DLA, or other federal procurement portals.


Scope of Work


1. Build Automated Solicitation Retrieval
Create a Python script that automatically pulls new solicitations from:
DIBBS / DLA Internet Bid Board System


Allow filtering by:
FSC/PSC codes
Delivery 
Date posted


2. Identify Solicitations With Online Pricing
Build logic to detect solicitations that:
Include online pricing
Include competitive historical pricing
Include NSN‑level price history
Include automated quote functionality


Extract and store:
Item description
Part Number
NSN
Quantity
Online price
Cage Code
Vendor pricing links


3. Data Output & Organization
Deliver results in:
CSV
Excel
Google Sheets


Include fields such as:
Solicitation number
Agency
Deadline
NSN
Online price
Link to solicitation
Link to pricing


4. Automation & Scheduling
Set up automatic daily or hourly pulls


5. Documentation
Provide clear instructions on:
How to run the script
How to update filters
How to add new portals or pricing sources
* Less than 30 hrs/week
Hourly
* 1 to 3 months
Duration
* Intermediate
I am looking for a mix of experience and value
   * Project Type: Ongoing project
Skills and Expertise
Mandatory skills
Document AI
Python
Data Extraction
```
