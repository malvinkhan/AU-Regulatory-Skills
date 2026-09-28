[SKILL.md](https://github.com/user-attachments/files/32718444/SKILL.md)
---
name: tga-regulatory-analysis
description: >
  Analyse pharmaceutical changes and produce a full TGA Regulatory Impact Summary,
  including variation classification (Notification, SAR, Category 3, or Category 1),
  variation type code, rationale, and relevant TGA guidance references. Always use this
  skill whenever the user uploads or describes a Change Control Report (CCR), stability
  data, or any document describing a pharmaceutical change — even if they don't use the
  words "variation" or "TGA". Trigger for change types including manufacturing site
  changes, formulation or excipient changes, specification changes, and CEP (Certificate
  of Suitability) updates. Always use this skill when the user asks anything about
  regulatory impact, variation type code, TGA submission pathway, or whether a change
  is major or minor.
---

# TGA Regulatory Analysis Skill

## Purpose

Analyse pharmaceutical change documents and produce a structured **TGA Regulatory Impact Summary**, covering:

- The correct variation classification and TGA variation type code
- Rationale for the classification
- Reference to the applicable section of the official TGA variation types table
- A grouping flag where obviously warranted

---

## Authoritative Reference Document

This skill relies on the TGA's official publication:

> *Variations to prescription medicines – excluding variations requiring evaluation of clinical or bioequivalence data; Appendix 1: Variation types – chemical entities* (Version 3.1, January 2020)
> Published by the Therapeutic Goods Administration, © Commonwealth of Australia.
> Source: https://www.tga.gov.au/resources/guidance/variations-prescription-medicines-excluding-variations-requiring-evaluation-clinical-or-bioequivalence-data-chemical-entities

**This document is not bundled in this repo** (it is Commonwealth copyright material — see `references/README.md`). To use this skill fully:

1. Download the current version from the TGA link above.
2. Save it locally as `references/tga_variation_types_appendix1.pdf` (or `.txt` if you convert it) — this path is gitignored and stays local to your machine.
3. Always consult that file before classifying any variation. The index of all variation type codes is near the front of the document; detailed conditions for each type are in the body.

If the reference document isn't present locally, flag this to the user and ask them to supply it or confirm the classification against the current TGA guidance directly.

---

## TGA Variation Pathway Overview

| Pathway | Description | TGA Approval Required Before Implementation? |
|---|---|---|
| **No reporting required** | Very low-risk changes; implement freely | No |
| **Notification** | Low risk; TGA approval granted automatically on lodgement | No (implement before/after lodgement per conditions) |
| **Self-Assessable Request (SAR)** | Sponsor self-assesses data; TGA verifies | No, but must notify TGA |
| **Category 3** | Quality-data evaluation required by TGA | Yes — prior approval required |
| **Category 1** | Full dataset evaluation (quality, nonclinical, clinical, or bioequivalence) — **not covered by this guidance** | Yes — registration process applies |

> **Important:** This skill covers Notifications, SARs, and Category 3 requests. If a change appears to require clinical or bioequivalence data, flag it as a potential Category 1 application and recommend the user consult the TGA's prescription medicine registration process guidance.

---

## Common Change Types & Likely Variation Codes

Use these as a starting point, but always verify against the reference document.

### API / CEP Changes
| Change | Likely Code | Category |
|---|---|---|
| CEP revision for non-sterile API (not synthetic polypeptide/fermentation) | ACEP | Notification |
| API site of manufacture — transfer/addition to existing manufacturer | AMTA | Notification |
| API site of manufacture — new site (other cases) | AMAS | Category 3 |
| API specification narrowing | ASNL | Notification |
| API specification — change to test methods or widening | ASCS | Category 3 |
| API retest period — decrease / more restrictive storage | ASDR | Notification |
| API retest period — change to retest period (extension) | ADSL | Category 3 |

### Drug Product — Site of Manufacture
| Change | Likely Code | Category |
|---|---|---|
| Site of labelling/primary packaging (non-sterile) | DMPL | Notification |
| Site of secondary manufacture/packaging | DMSL | Notification |
| New/additional site of manufacture (non-sterile, non-modified release) | DMSA | Notification |
| New/additional site of manufacture (sterile or modified release) | DMST | Category 3 |
| Cessation of a site | DMDM | Notification |

### Drug Product — Formulation / Excipient
| Change | Likely Code | Category |
|---|---|---|
| Qualitative/quantitative formulation change creating separate & distinct good — minor | DFCI | SAR |
| Qualitative/quantitative formulation change — more significant | DFCF / DFNA | Category 3 |
| Excipient specification narrowing | Verify in index | Notification |
| Excipient change — grade or source (same function) | Verify in index | Notification or SAR |
| Addition or removal of excipient | Verify in index | Likely Category 3 |

### Drug Product — Specifications / Test Changes
| Change | Likely Code | Category |
|---|---|---|
| Narrowing of specification limits | DSNL | Notification |
| Addition of new test and limit | DSNT | Notification |
| Change to test method (non-substantial) | DSAM | Notification |
| Widening or deletion of specification limits | DSCS | Category 3 |

### Packaging Changes
| Change | Likely Code | Category |
|---|---|---|
| Change to container/closure system (non-sterile, like-for-like) | CCLS | Notification |
| New container/closure type | CCNS | SAR or Category 3 (check conditions) |
| Pack size change | PSAD (add) / PSDE (delete) | SAR or Category 3 |

> These codes are illustrative — always verify exact code and conditions in the reference document.

---

## Workflow: How to Analyse a Document

When the user provides a CCR, stability report, or change description:

### Step 1 — Read the Reference Document
Open the local copy of `references/tga_variation_types_appendix1.pdf` (or `.txt`). Locate the relevant section using the index and read the detailed conditions for the specific variation type.

### Step 2 — Extract the Change
Clearly state:
- What is changing (the "before" and "after")
- Which product(s) and ARTG number(s) are affected (if stated)
- The reason for the change as documented

### Step 3 — Match to a Variation Type Code
Using the reference document index and detailed sections:
- Identify the variation type code (e.g., ACEP, DMSA)
- Confirm the conditions listed in the document are satisfied by the change described
- If conditions are only partially met, flag this explicitly

### Step 4 — Classify
Assign the variation category: No reporting / Notification / SAR / Category 3 / Possible Category 1.

If ambiguous between two codes or categories, present both and explain the determining factor.

### Step 5 — Grouping Check (only if obvious)
Flag if:
- The CCR describes multiple interdependent changes, OR
- The change clearly cannot stand alone (e.g., a manufacturing site change bundled with a process change)

### Step 6 — Produce the Regulatory Impact Summary

---

## Output Format: Regulatory Impact Summary

```
REGULATORY IMPACT SUMMARY
──────────────────────────────────────────────────────

Product:            [Product name(s) and ARTG number if available]
Change Description: [Plain English before → after]
Document(s):        [CCR number / stability report / etc.]

CLASSIFICATION
  Variation Code:   [e.g., ACEP / DMSA / DFCI]
  Pathway:          [Notification / SAR / Category 3 / Category 1 / No reporting required]
  Impact Level:     [Major (Cat 1) / Moderate (Cat 3) / Minor (Notification or SAR) / Nil]

RATIONALE
  [2–4 sentences explaining why this classification applies, citing the specific
  conditions from the TGA variation types table (Appendix 1) that apply or are
  met by this change. Reference the relevant section number or page if possible.]

TGA GUIDANCE REFERENCE
  Document: Variations to prescription medicines – Appendix 1: Variation types –
            chemical entities (V3.1, January 2020)
  Section:  [e.g., Section 1.1.2 — Notifications / Section 1.3.3 — Category 3]
  URL:      https://www.tga.gov.au/resources/guidance/variations-prescription-medicines-excluding-variations-requiring-evaluation-clinical-or-bioequivalence-data-chemical-entities

GROUPING FLAG (only if applicable)
  [Flag if changes are obviously interdependent and should be grouped. Note the
  additional codes that would apply.]

STABILITY DATA NOTE (if stability data provided)
  [Comment on whether the data supports the change — e.g., shelf life data
  adequate, data gaps identified, bracketing/matrixing approach noted.]

RECOMMENDATIONS / NEXT STEPS
  [Any caveats, items requiring confirmation, or recommended escalation.]
──────────────────────────────────────────────────────
```

---

## Important Caveats

- This analysis supports regulatory decision-making but is not a substitute for TGA advice or review by a senior RA professional.
- Always use Version 3.1 (January 2020) of Appendix 1 as the primary reference, or a more recent version if the user provides one.
- For novel, complex, or borderline changes, recommend a formal TGA query or escalation.
- If stability data is provided, assess whether it satisfies the data requirements listed under the relevant variation type in the reference document.
