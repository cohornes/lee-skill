# /lee

A Claude Skill with two modes:

1. **4Ds Prompt Refiner** — turns a rough prompt into a structured prompt using the Diligence / Description / Discernment / Delegation framework, either as segmented sections or one combined prompt.
2. **QC Findings Tracker** — builds/extends a centralized tracker for mortgage due diligence (TPR) QC findings: client-reported errors, underwriter trends, and a dashboard.

## Install

Copy the `lee/` folder into your Claude Skills directory (or upload `lee.skill` in claude.ai / Claude Code) so it can be triggered by typing `/lee` or describing either use case.

## Structure

```
lee/
├── SKILL.md                       # Skill definition and instructions
└── references/
    └── qc-tracker-spec.md         # Baseline schema/taxonomy for the QC tracker
```
