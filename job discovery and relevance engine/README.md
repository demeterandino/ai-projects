# Job Discovery & Relevance Engine

AI-powered job discovery and relevance scoring to reduce time spent browsing job boards.

**Key ideas**
- Source-agnostic ingestion (job postings can come from any feed/link/text)
- Hard constraints (location, language, role exclusions)
- Multi-dimensional AI scoring (fit based on responsibilities, not just job titles)
- Human-in-the-loop decision (the system shortlists; the user decides)

---

## Problem

Job discovery is noisy and time-consuming:
- Many postings look relevant by title, but are not a real fit after reading.
- Platform “top applicant” signals don’t reliably translate to interview callbacks.
- The biggest cost is not writing applications, but continuously searching and filtering.

---

## Goal

Automatically produce a **shortlist of high-fit jobs** for:
- **Vienna**
- **Remote EU**

…while excluding roles that are not a match (e.g., pure engineering / DS roles) and filtering out German fluency requirements.

---

## What this system does

Given job postings, the assistant:
1. Applies hard constraints (location, German requirement, role exclusions)
2. Scores fit **1–5** based on responsibilities and context
3. Returns **only jobs scored >= 4**, including:
   - Title / Company / Location
   - Score
   - 3 reasons for fit
   - 1–2 potential gaps

---

## What this system intentionally does NOT do (v1)

- Does not auto-apply to jobs
- Does not message recruiters
- Does not scrape or automate LinkedIn browsing

---

## Candidate profile (high-level)

The scoring is optimized for a candidate with strengths in:
- Product Ownership in complex platform transformations
- Operations & Technology Transformation
- Finance / Business Support Operations leadership
- Cross-functional collaboration (business–product–engineering)
- UAT, enablement, adoption, and process redesign
- People leadership (multi-country teams)

---

## Hard constraints

**Location**
- Accept only: Vienna or Remote EU

**Language**
- Exclude if fluent/C1 German is required
- Accept if B1/B2 is required or language is not mentioned
- If the job ad is written in German and no language level is stated → treat as fluent German required → exclude

**Role exclusions**
- Exclude roles primarily focused on coding, modeling, or research (e.g., software engineering, data science / ML engineering, research)

---

## Architecture (v1)

High-level flow:

1. **Google Alerts** collects new job postings based on role/title queries  
2. Alerts arrive to Gmail  
3. **Make** reads the alert emails and extracts job items  
4. The assistant evaluates postings using the prompt in this repository  
5. Output is a shortlist (e.g., Google Sheet / email / Notion)

> See `/screenshots` for the Google Alerts setup.

---

## Prompt

The core scoring prompt is stored here:
- `prompt.md` (or the file you created in this project folder)

It contains:
- Candidate profile
- Search role families
- Hard constraints
- Scoring rules
- Output format (shortlist only)

---

## Example output (format)

- **AI Product Owner – Company X (Vienna)** — Score: 4  
  - Strong match on product ownership + cross-functional delivery  
  - Platform / transformation context aligns with candidate background  
  - Clear operational impact metrics and stakeholder exposure  
  - Gap: explicit AI lifecycle ownership not clearly stated

---

## Status

- [x] Relevance prompt (v1)
- [x] Google Alerts configured (v1)
- [ ] Make scenario: ingest + score + shortlist (in progress)
- [ ] Output destination finalized (Sheet / Notion / email)

---

## Next improvements

- Add duplicate detection (same job posted on multiple sites)
- Add confidence score + “why excluded” debug mode (optional)
- Add role-family weights (core vs adjacent titles)
- Connect shortlisted jobs to an “Application Assistant” (tailored CV bullets + cover letter)
