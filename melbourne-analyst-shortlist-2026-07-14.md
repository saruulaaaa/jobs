# Melbourne Analyst Shortlist — 14 July 2026

Ranked against the [job hunter profile](https://github.com/saruulaaaa/jobs/blob/claude/job-hunter-profile-h2a5po/job-hunter-profile.md) (Saruultuya Orkhontuul — FP&A / Power BI / commercial analytics, 3+ yrs experience + Master of Business Analytics).

**Method note:** the Apify job-scraper actor could not be run from this session — the environment has no Apify API token and its network policy blocks `api.apify.com` (and direct fetches to Seek/LinkedIn/Indeed). The shortlist below was instead compiled from live search-engine snapshots of Seek/Indeed/Glassdoor listings posted within roughly the last 7 days. Where the ad doesn't disclose a company or a direct job ID, the search link that surfaces the ad is given instead.

| # | Title | Company | Salary | Arrangement | Link | Why it fits |
|---|-------|---------|--------|-------------|------|-------------|
| 1 | FP&A Analyst | via Ethos BeathChapman (recruiter) | $120k–$140k p.a. | Melbourne, FT | [Seek – Ethos BeathChapman jobs](https://au.seek.com/Ethos-BeathChapman-jobs) | Exact title match — group-level planning, reporting and strategic decision support is precisely the Tok Tok FP&A remit; listed 7 days ago. |
| 2 | Data Analyst | UniSuper | Not disclosed + **17% super** | Melbourne CBD, hybrid | [Seek – UniSuper jobs Melbourne](https://au.seek.com/UniSuper-jobs/in-All-Melbourne-VIC/full-time) | Dashboards and reporting in **Power BI + SQL** — her two strongest tools — at a purpose-led financial services fund. |
| 3 | Finance Analyst (business-facing) | Undisclosed (Seek ad) | $106k + super | Melbourne, hybrid | [Seek – finance analyst Melbourne](https://au.seek.com/finance-analyst-jobs/in-All-Melbourne-VIC?daterange=7) | Insights, forecasting and reporting for business stakeholders — a one-to-one match with her sole-analyst FP&A experience. |
| 4 | Data Analyst — Supplier Strategy | Wesfarmers Health (Priceline group) | Not disclosed | Docklands, hybrid | [Seek – Wesfarmers Health jobs](https://au.seek.com/Wesfarmers-Health-jobs/in-Docklands-VIC-3008) | Driving supplier strategy through data at a major retail/health group — mirrors her procurement-decision forecasting (AUD 1M+) at Tok Tok. |
| 5 | Commercial Financial Analyst | Undisclosed (Seek ad) | Not disclosed | Melbourne, WFH flexibility | [Seek – financial analyst Melbourne](https://au.seek.com/financial-analyst-jobs/in-All-Melbourne-VIC?daterange=7) | Reporting, pricing and analysis with explicit **Power BI** exposure — plays directly to her commercial analytics differentiator. |
| 6 | Operations Analyst — Global Freight | High-growth freight-tech (Seek ad) | Not disclosed | West/Port Melbourne | [Seek – operations analyst Melbourne](https://au.seek.com/operations-analyst-jobs/in-Melbourne-VIC-3000?daterange=7) | Building dashboards and automations for logistics operations — a natural continuation of her delivery-company ops analytics. |
| 7 | Financial Analyst — Healthcare | Undisclosed (Seek ad) | $90k–$110k + super | Melbourne, hybrid (3-mo contract) | [Seek – financial analyst contract Melbourne](https://au.seek.com/financial-analyst-jobs/in-All-Melbourne-VIC/contract-temp) | She's open to contract roles — a fast route to local AU experience in a strong-fit healthcare finance function. |
| 8 | Market Research & Analysis Analyst | Colliers (Living & Residential Capital Markets) | Not disclosed | Melbourne, hybrid, FT | [Colliers careers](https://careers.colliers.com/jobs) / [Seek – market research Melbourne](https://au.seek.com/market-research-jobs/in-All-Melbourne-VIC) | Listed ~4 days ago; research + insight generation at a global property brand, welcoming early-career analysts. |
| 9 | Digital Product & Insights Graduate | RACV | $80k incl. super | Melbourne | [Seek – data analyst Melbourne 3000](https://au.seek.com/data-analyst-jobs/in-Melbourne-VIC-3000) | Graduate program using data and insights to improve digital channels — ideal bridge role for a fresh analytics Master's (applications close 13 Aug 2026). |
| 10 | Business Analyst (data & insights) | Good to Great Schools Australia | $80–$110/hr (contract) | **Fully remote** (AEST-friendly) | [Seek – business analyst Melbourne](https://au.seek.com/business-analyst-jobs/in-All-Melbourne-VIC?daterange=7) | Turning data into insights for smarter decisions, fully remote at a strong contract rate — fits her remote-open preference. |

## Near misses / honourable mentions

- **Graduate Finance Analyst — Rinnai Australia** (12-month contract, potential to go permanent) — good backup graduate-level entry: [Seek – Rinnai jobs](https://www.seek.com.au/Rinnai-jobs/in-All-Melbourne-VIC)
- **Data Business Analyst — via Gladwin Partners** ($900/day, 6-mo contract, 3 days WFH) — lucrative but the stack (AWS/Databricks engineering) skews more technical than her profile.
- **BI & Data Analyst — NW suburbs** (hybrid, 1 day WFH) — Power BI reporting build, solid fit but less flexibility.
- **ASIC — analyst (hybrid, from $127,641 + 15.4% super)** — attractive package; likely requires more senior/regulatory experience.

## Caveats

- Posting ages are as reported by search snapshots on 14 Jul 2026; a few could not be confirmed to the exact day. Roles known to be older than 7 days (e.g. Spark Foundry Media Analyst, 23 days) were excluded.
- Direct `seek.com.au/job/ID` links couldn't be captured because job boards block automated fetching from this environment; the links above land on the filtered search or company page where the ad appears.
- To re-run this properly via Apify (e.g. `bebity/linkedin-jobs-scraper` or a Seek scraper actor), add an `APIFY_TOKEN` to the environment and allow `api.apify.com` in the session's network policy.
