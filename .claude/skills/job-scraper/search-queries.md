# Search Queries for Job Scraper

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (CLI skills, auto-discovered by `/scrape`):
- **linkedin.com/jobs** - `linkedin-search` CLI (India: Pune, Bengaluru, Delhi NCR, remote; overseas target countries)
- **freehire** - `freehire-search` CLI

Indian and remote boards (WebSearch `site:` fallback until a CLI is added with `/add-portal`):
- **naukri.com** - largest Indian general board
- **foundit.in** - (formerly Monster India)
- **instahyre.com** - curated tech roles, product companies
- **hirist.tech** - tech-only board (QA/SDET category)
- **wellfound.com** - startups (formerly AngelList Talent)
- **himalayas.app** - remote-first roles, filterable by hiring country (India)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

Danish demo portals (Jobindex, Jobbank, Jobdanmark, Jobnet) stay **disabled** - not a target market.

## Query Categories

Queries are grouped by priority. Candidate languages are English and Hindi; postings in these markets are written in English, so queries are English-only (a Hindi variant adds no results for tech roles). Combine each query with the location terms in the Location Filter below.

**Geographic priority (applies to every category):** Pune and Bengaluru first, then overseas sponsored markets, then remote and Delhi NCR. Run each category's India queries for `Pune` and `Bengaluru` (also `Bangalore`) before anything else.

**Organize by function, not job title.** The same underlying work carries different titles across companies and markets; each category lists several title variants.

### Priority 0 (always run, every scrape): Mobile Automation Engineer / Mobile Testing

User rule (2026-10-27): **always** search for mobile automation engineer and mobile testing roles on every run, including default and focus-area runs, and mark every such posting **High** fit in Step 3, ranked above all other High rows. A posting counts when its title or core duties are mobile test automation or mobile app testing (Appium, XCUITest, Espresso, Maestro, iOS/Android testing), at any seniority. The override does not bypass the hard gates: an undeclared required language (Language Gate) or a failed Eligibility/Location gate still makes it Low.

```
site:linkedin.com/jobs "Mobile Automation Engineer" OR "Mobile Test Automation Engineer" India
site:linkedin.com/jobs "Mobile QA Engineer" OR "Mobile Testing" Pune OR Bengaluru OR Gurugram OR Noida
site:naukri.com "Mobile Automation" Appium OR XCUITest
site:linkedin.com/jobs "Mobile QA" OR "Mobile Test Automation" (Dubai OR London OR Amsterdam OR Berlin OR Dublin)
```

Title variants: Mobile Automation Engineer, Mobile Test Automation Engineer, Mobile QA Engineer, Mobile Tester, Mobile App Test Engineer, QA Engineer (Mobile), SDET (Mobile), iOS/Android Test Automation Engineer

### Priority 1: Senior/Lead mobile test automation (SDET track)

Owning mobile quality and automation frameworks at Staff/Lead level. Product companies preferred.

```
site:linkedin.com/jobs "Lead SDET" mobile Pune OR Bengaluru
site:linkedin.com/jobs "Staff SDET" OR "Senior SDET" Appium Bengaluru
site:naukri.com "SDET" Appium Python Pune
site:naukri.com "Lead SDET" OR "SDET Lead" Bengaluru
site:instahyre.com "SDET" mobile automation Bengaluru
site:hirist.tech "SDET" Appium Pune OR Bengaluru
site:foundit.in "Mobile Automation Lead" XCUITest OR Appium
site:linkedin.com/jobs "Mobile QA Lead" OR "Mobile Test Lead" Pune OR Bengaluru
site:linkedin.com/jobs "Senior SDET" OR "Mobile QA Lead" Dubai
```

Title variants: Lead SDET, Staff SDET, Senior SDET, SDET III, Principal SDET, Mobile QA Lead, Mobile Test Automation Lead, Senior QA Automation Engineer (Mobile), Senior Software Engineer in Test

### Priority 2: Test automation architecture / quality engineering leadership

Designing frameworks and quality strategy across teams; IC, manager, or hybrid.

```
site:linkedin.com/jobs "Test Automation Architect" mobile Pune OR Bengaluru
site:linkedin.com/jobs "Quality Engineering Lead" OR "QE Lead" Bengaluru
site:naukri.com "Automation Architect" Appium OR XCUITest
site:instahyre.com "QA Lead" OR "Quality Engineering Manager" Bengaluru OR Pune
site:hirist.tech "Test Architect" OR "QA Architect" mobile
site:linkedin.com/jobs "QA Automation Lead" OR "Test Automation Architect" (Dubai OR London OR Amsterdam OR Berlin OR Dublin)
site:linkedin.com/jobs "Senior SDET" mobile "visa sponsorship" (UK OR Germany OR Netherlands OR Ireland OR Europe)
```

Title variants: Test Automation Architect, QA Architect, Quality Engineering Lead, QE Manager, Engineering Manager - Quality, Head of QA (smaller product companies)

### Priority 3: AI / LLM quality engineering

Testing and evaluating AI features; building AI-assisted QA workflows.

```
site:linkedin.com/jobs "AI Quality Engineer" OR "LLM QA" OR "AI Test Engineer"
site:linkedin.com/jobs "SDET" "LLM" OR "generative AI" Bengaluru OR Pune
site:wellfound.com "QA" OR "SDET" AI LLM India
site:himalayas.app "QA Engineer" AI remote India
site:naukri.com "AI testing" OR "GenAI testing" SDET
site:linkedin.com/jobs "AI Evaluation Engineer" OR "LLM Evaluation" remote
```

Title variants: AI Quality Engineer, AI/ML Test Engineer, LLM QA Engineer, AI Evaluation Engineer, QA Engineer - AI Products, SDET (GenAI)

### Priority 4: Last-resort directions (wider net)

Only surface these when Priorities 1-3 are thin. Score Career Alignment lower.

```
site:linkedin.com/jobs "Developer Productivity Engineer" OR "AI Tooling Engineer" Bengaluru
site:wellfound.com "AI engineer" agents MCP India
site:linkedin.com/jobs "iOS Developer" Swift Pune OR Bengaluru
site:naukri.com "React Native Developer" Pune OR Bengaluru
site:himalayas.app "QA Automation Engineer" remote India
```

## Location Filter

Home base: Delhi NCR, India (currently remote). Flexible on location; relocation within India welcome; relocation abroad only with visa sponsorship.
- **Ideal:** Pune, Bengaluru (on-site, hybrid, or remote)
- **Ideal:** Remote (India-based employer, or international employer hiring in India / via EOR)
- **Acceptable:** Dubai (UAE) with employer-provided work visa
- **Acceptable:** Europe (incl. UK) with visa sponsorship - e.g. UK, Germany, Netherlands, Ireland
- **Out of scope (since 2026-10-25):** rest of the Middle East (Abu Dhabi, Saudi Arabia, Qatar, etc.), Singapore, Canada, Australia, USA
- **Acceptable:** Delhi NCR (Gurugram, Noida, Delhi) - home base
- **Borderline:** Other Indian cities (Hyderabad, Mumbai, Chennai, etc.)
- **Too far / exclude:** USA; any overseas role that says no sponsorship or requires citizenship/PR; night shifts or permanent US-hours overlap

## Salary Filter

India-based roles: exclude when the posted range tops out below **17 LPA INR**. If no salary is posted, include and mark "salary unverified". Overseas: no fixed floor.

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
