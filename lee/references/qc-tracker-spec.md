# QC Findings Tracker — Baseline Spec

Context: Mortgage due diligence (TPR) firm. Quality Diligence Manager oversees 14 QC analysts reviewing 200 underwriters. Clients report errors by email; management compiles them into Excel reports. No centralized system exists yet to track trends.

## Raw Findings Log — suggested fields

| Field | Notes |
|---|---|
| Finding ID | Auto-incrementing unique ID |
| Date reported | Date client feedback received |
| Client | Client/lender name |
| Loan / File ID | Reference number for the file in question |
| Underwriter | Who made the error |
| QC Analyst | Which of the 14 analysts reviewed/missed it (if known) |
| Error Category | From standardized taxonomy (below) |
| Error Description | Free text, what went wrong |
| Severity | e.g. Critical / Major / Minor |
| Root Cause | e.g. Missed document, calculation error, policy misapplication, training gap |
| Resolution Status | Open / In Review / Resolved |
| Resolution Notes | Free text |

## Starter Error Taxonomy (adjust based on real data)

- Income Calculation
- Asset Verification
- Credit / Liability Review
- Compliance / Regulatory Flag
- Document Indexing / Missing Docs
- Appraisal / Valuation
- Loan Condition Clearing
- Other (specify)

Note: this taxonomy should be refined once real historical findings are categorized — treat it as a starting point, not final.

## Dashboard Views

- Findings by Error Category (trend over time — line or bar chart by month)
- Findings by Underwriter (ranked, to identify repeat issues)
- Findings by Severity
- Open vs. Resolved status counts

## Process for logging new findings

1. Management's Excel report of client feedback arrives
2. Each finding gets logged as a new row in the Raw Findings Log, categorized against the taxonomy
3. Dashboard auto-updates via pivot tables/charts referencing the log
4. Periodic review (e.g. monthly) to spot trends and flag coaching needs
