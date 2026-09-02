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

## Dashboard Views

- Findings by Error Category / Sub-Category (trend over time — line or bar chart by month)
- Findings by Underwriter (ranked, to identify repeat issues)
- Findings by Severity
- Findings by Root Cause
- Open vs. Resolved status counts

## Process for logging new findings

1. Management's Excel report of client feedback arrives
2. Each finding gets logged as a new row in the Raw Findings Log, mapped to one category + one sub-category from the taxonomy, with severity and root cause assigned
3. Dashboard auto-updates via pivot tables/charts referencing the log
4. Periodic review (e.g. monthly) to spot trends and flag coaching needs
