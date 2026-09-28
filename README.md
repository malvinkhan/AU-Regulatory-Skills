# Regulatory Skills

A collection of Claude Skills for pharmaceutical regulatory affairs work in the Australian (TGA) context. Each skill is self-contained and designed to be used with Claude to analyse regulatory documents and produce structured outputs.

These skills encode general regulatory frameworks and workflows — they do not contain any company-specific, sponsor-specific, or product-specific data.

## Skills

| Skill | Purpose | Status |
|---|---|---|
| [`tga-regulatory-analysis`](./tga-regulatory-analysis) | Classifies pharmaceutical changes (CCRs, stability data) against TGA variation types (Notification / SAR / Category 3 / Category 1) | ✅ Available |
| [`pi-cmi-gap-analysis`](./pi-cmi-gap-analysis) | Section-by-section gap analysis of PI/CMI documents — reference comparison, multi-brand consistency, PI↔CMI consistency, or structural/quality audit | ✅ Available |
| [`artwork-labelling-review`](./artwork-labelling-review) | Checklist-driven PASS/FLAG/FAIL review of label artwork against TGO 91 (prescription) and TGO 92 (non-prescription), plus PI/CMI/RASML cross-checks | ✅ Available |

## Usage

Each skill folder contains its own `SKILL.md` with the skill definition, and a `references/` folder for any supporting documents. Where a reference document is copyrighted government material (e.g. TGA guidance), it is **not bundled in this repo** — see the relevant skill's `references/README.md` for a link to the official source and instructions to add your own local copy.

## Templates

| Template | Purpose |
|---|---|
| [`ccr-task-tracker`](./ccr-task-tracker) | Notion database template for tracking Change Control Requests through the regulatory lifecycle |

## Disclaimer

These skills support regulatory decision-making but are not a substitute for formal TGA advice or review by a qualified Regulatory Affairs professional. Always verify outputs against current official guidance.
