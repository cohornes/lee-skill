# QC Findings Tracker — Baseline Spec

Context: Mortgage due diligence (TPR) firm. Quality Diligence Manager oversees 14 QC analysts reviewing 200 underwriters. Clients report errors by email; management compiles them into Excel reports. No centralized system exists yet to track trends.

## Error Taxonomy (Starter — Mortgage TPR)

Map every raw client finding to ONE category + ONE sub-category. If a finding spans multiple categories, default to the ROOT CAUSE category.

| Category | Sub-Categories | Definition |
|---|---|---|
| INCOME | Calculation, Documentation, Stability, DTI | Errors in income verification or qualification math |
| ASSET | Sourcing, Documentation, Reserves | Errors in asset verification or required reserves |
| CREDIT | Disputed Accounts, Authorized User, Inquiry | Errors in credit report analysis or related party credit |
| COLLATERAL | Appraisal Review, Value, Property Type | Errors in collateral evaluation or appraisal review |
| COMPLIANCE | TRID, RESPA, TILA, State Law | Regulatory or disclosure errors |
| DOCUMENTATION | Missing Docs, Expired Docs, NINA | Errors in required documentation completeness |
| DECISIONING | Eligibility, AUS, Manual UW | Errors in the final credit decision or guideline application |

### Severity Levels

- **Critical** — Material defect affecting loan saleability or compliance
- **Major** — Guideline violation requiring rework or re-underwrite
- **Minor** — Documentation gap or clerical error with no material impact
- **Observation** — Best practice miss; no immediate risk

This taxonomy is a starting point — refine categories/sub-categories once real historical findings are mapped against it, and note recurring "doesn't fit" findings as candidates for a new sub-category rather than forcing them into "Other."

## Core Schema: Raw Findings Log (One Row = One Finding)

| Field Name | Type | Required | Source | Notes |
|---|---|---|---|---|
| finding_id | Auto-number | Yes | System | YYYYMM-### format |
| date_received | Date | Yes | Client email | Date client sent feedback |
| client_name | Dropdown | Yes | Master list | From controlled client list |
| loan_id | Text | Yes | Client email | Must match LOS loan number |
| file_reference | Text | No | Internal | Internal file or batch ID |
| underwriter_name | Dropdown | Yes | HR roster | From controlled underwriter list |
| reviewer_name | Dropdown | Yes | QC team | Which of the 14 analysts reviewed |
| error_category | Dropdown | Yes | Taxonomy | From Category list above |
| error_subcategory | Dropdown | Yes | Taxonomy | From Sub-Category list above |
| severity | Dropdown | Yes | Taxonomy | Critical/Major/Minor/Observation |
| client_description | Text | Yes | Client email | Verbatim client feedback |
| root_cause | Dropdown | Yes | Analysis | Process gap / Training gap / System gap / Human error / Client instruction unclear |
| resolution_status | Dropdown | Yes | Workflow | Open / Under Review / Resolved - No Action / Resolved - Coaching / Resolved - Process Change |
| resolution_date | Date | No | System | Date status moved to Resolved |
| coaching_assigned | Dropdown | No | Manager | Underwriter or Reviewer requiring coaching |
| notes | Text | No | Freeform | Manager narrative |

When building the actual Excel/Sheets tracker, every "Dropdown" field should be a real data-validation dropdown sourced from a controlled list tab (client list, underwriter roster, taxonomy categories/sub-categories, etc.) — not free text — so trend analysis stays clean.

## Governance Rules

- **Master Lists:** Maintain separate "Config" sheets for: Client Names, Underwriter Roster, Reviewer Roster. Never hardcode dropdowns.
- **New Entries:** If a client or underwriter isn't in the master list, flag it in `notes` and add to Config within 24 hours. Never type free-text names into the main log.
- **Data Retention:** Keep 24 months rolling. Archive by quarter to a separate "Archive" sheet or file.
- **Access:** One owner (the manager) has write access to Config. 14 analysts have write access to Raw Log. Management has read access to Dashboard.

## Dashboard Views (Summary Sheet)

1. **Trend by Month:** Count of findings by month, stacked by severity
2. **Underwriter Heatmap:** Underwriter (rows) × Error Category (columns), color-coded by volume
3. **Reviewer Catch Rate:** % of findings by reviewer (to identify if certain reviewers are missing specific error types)
4. **Client Concentration:** Findings by client (are we failing one client's expectations?)
5. **Resolution Aging:** Open findings > 14 days (escalation trigger)
6. **Root Cause Trend:** % breakdown of root causes over trailing 6 months

## SOP: Logging New Client Feedback

When a client email arrives:
1. Forward to shared QC inbox (or designated intake email)
2. Within 48 hours, assigned reviewer creates ONE row per finding in Raw Log
3. Map client verbatim to `client_description`
4. Use taxonomy to assign category, subcategory, severity
5. If severity = Critical, immediately email manager + flag in `notes`
6. If root cause is unclear, set status to "Under Review" and schedule a 15-min huddle
7. Update `resolution_status` within 5 business days of logging
8. If coaching is assigned, link to coaching log (separate tracker) in `notes`

## Decisions Required Before Build

Answer these before generating the actual tracker file (separate from the Phase 1 discovery questions — these are build-specific):

1. **Tool:** Excel desktop, Excel 365 (co-authoring), or Google Sheets?
2. **Access:** Will all 14 analysts input simultaneously, or will one person log on behalf of the team?
3. **Client List:** Is there a static client list, or does it change monthly?
4. **Underwriter Roster:** Track by name, ID, or both? Do underwriters ever change teams?
5. **Severity Authority:** Who decides severity — the manager, the client, or a defined rubric?
6. **Coaching Linkage:** Should this tracker *trigger* coaching assignments, or just *reference* them?
7. **Output Format:** A downloadable Excel file with formulas, or a text schema to build manually?

## Constraints & Anti-Patterns

- Do NOT suggest SQL databases, Airtable, or Power BI unless explicitly asked.
- Do NOT create more than 6 dashboard views in the first build. Start simple.
- Do NOT auto-calculate a "reviewer quality score" from this data — it creates perverse incentives.
- Do NOT build email parsing automation. Manual logging is the correct MVP.
- Do NOT merge multiple findings into one row. One finding = one row, always.

## Process for logging new findings (high-level)

1. Management's Excel report of client feedback arrives
2. Each finding gets logged as a new row in the Raw Findings Log, mapped to one category + one sub-category from the taxonomy, with severity and root cause assigned
3. Dashboard auto-updates via pivot tables/charts referencing the log
4. Periodic review (e.g. monthly) to spot trends and flag coaching needs

(For the detailed step-by-step version with timing/escalation rules, see the SOP above.)
