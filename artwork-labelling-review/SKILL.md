[SKILL.md](https://github.com/user-attachments/files/32721415/SKILL.md)
---
name: artwork-labelling-review
description: >
  Perform a structured TGA artwork and labelling review for Australian medicines. Produces a
  checklist-driven pass/flag/fail assessment against TGO 91 (prescription labels, S4/S8),
  TGO 92 (non-prescription labels including CHI, S2/S3/OTC/Listed), PI, and CMI requirements.
  Always use this skill whenever the user shares or describes label artwork, carton artwork,
  blister/foil artwork, a PI, a CMI, or any labelling material for an Australian medicine —
  even if they don't use the words "artwork review" or "TGO". Trigger for: "check this label",
  "review this artwork", "does this PI look right", "check the CMI", "artwork for approval",
  "annotate the label", "is this TGO 91 compliant", or any mention of label or artwork changes
  in a regulatory context. Also trigger when asked whether a label meets TGA requirements or
  to identify labelling issues before submission or print approval.
---

# Artwork & Labelling Review Skill

## Purpose

Perform a structured, checklist-driven review of Australian medicine labelling materials against TGA regulatory requirements. Produce a structured output with a **PASS / FLAG / FAIL** assessment for each checklist item, a summary, and recommended actions.

---

## Critical: TGO 91 vs TGO 92 Scope

This is the single most important determination before selecting a checklist.

| Order | Applies To | Scheduling |
|---|---|---|
| **TGO 91** | **Prescription and related medicines** | Schedule 4 (Prescription Only) and Schedule 8 (Controlled Drug) |
| **TGO 92** | **Non-prescription medicines** | Schedule 2, Schedule 3 (Pharmacist Only), Unscheduled (OTC), Listed (AUST L) |

If the product is a Schedule 4 or Schedule 8 medicine → use `references/tgo91_prescription_checklist.md`.
If the product is Schedule 2, Schedule 3, unscheduled, or listed → use `references/tgo92_nonprescription_checklist.md`.

---

## Reference Files

Load the appropriate checklist file(s) **before starting any review**. Only load files relevant to the review scope.

| Review Type | Reference File |
|---|---|
| Prescription medicine label (S4/S8) | `references/tgo91_prescription_checklist.md` |
| Non-prescription medicine label (S2/S3/OTC/Listed) | `references/tgo92_nonprescription_checklist.md` |
| Verifying warning statements on any OTC label | `references/rasml_advisory_statements.md` |
| Product Information (PI) | `references/pi_checklist.md` |
| Consumer Medicine Information (CMI) | `references/cmi_checklist.md` |
| Full pack review | Load all relevant files |

> **RASML rule:** For any TGO 92 review, always load `rasml_advisory_statements.md` alongside the TGO 92 checklist. Check each active ingredient against the RASML substance index. If the substance is listed, verify its required statements are present in the Warnings section of the CHI table.

> **Always read the relevant checklist file(s) before producing the review.** Do not rely on memory for individual checklist items.

---

## Scope Detection

Identify what the user has provided and which checklist(s) apply:

| Input | Review Scope |
|---|---|
| Label artwork for S4/S8 medicine (prescription) | TGO 91 checklist |
| Label artwork for OTC/complementary/listed medicine | TGO 92 checklist |
| PI document | PI checklist |
| CMI document | CMI checklist |
| Full artwork pack | All relevant checklists + cross-document consistency checks |
| User describes changes to labelling | Apply relevant checklist(s) to the changes described |
| Blister foil only | Relevant order's blister section only |
| Artwork not specifying schedule | Ask user or infer from product name/indication; flag assumption |

**If the user has not explicitly stated whether the medicine is prescription (S4/S8) or non-prescription (OTC/listed/complementary), STOP and ask before proceeding. Do not assume or infer — the checklist applied depends entirely on this, and using the wrong one produces an invalid review.**

---

## Workflow

### Step 1 — Determine TGO Order and Review Scope
The user must explicitly state whether the medicine is prescription (S4/S8 → TGO 91) or non-prescription (OTC/listed/complementary → TGO 92). If they have not stated this, ask before doing anything else. Once confirmed, load the appropriate reference file(s).

### Step 2 — Extract Key Product Details
Before running the checklist, extract and state:
- Product / brand name
- Active ingredient(s) and strength(s)
- Dosage form
- Schedule (S4, S8, S2, S3, AUST L, AUST R, unscheduled)
- ARTG number (if visible/stated)
- Sponsor name
- Document type(s) reviewed (main label, carton, blister, container label, PI, CMI)
- Panels visible (note which panels you can and cannot see)

State these at the top of the review output. If any are missing from the input, note "not provided/visible" — this may itself be a finding.

### Step 3 — Run the Checklist

Work through every section of the loaded checklist(s). For each item:
- Assign **PASS**, **FLAG**, **FAIL**, or **N/A**
- Add a **note** for FLAG and FAIL items explaining the specific issue

**Rating definitions:**

| Rating | Meaning |
|---|---|
| **PASS** | Requirement is clearly met |
| **FLAG** | Potentially met but with ambiguity, borderline compliance, or something requiring confirmation |
| **FAIL** | Requirement is not met — mandatory element absent, incorrect, or inconsistent |
| **N/A** | Not applicable to this product type, dosage form, or packaging configuration |

> **When reviewing an uploaded artwork file or image:** Be specific about which panels are visible. Note where an assessment cannot be completed due to limited visibility. Do not assume compliance for unverifiable elements.

### Step 4 — Produce the Review Output

---

## Output Format

```
ARTWORK & LABELLING REVIEW
──────────────────────────────────────────────────────

Product:            [Brand name / INN / strength / dosage form]
Schedule:           [S4 / S8 / S2 / S3 / AUST L / Unscheduled]
TGO Order:          [TGO 91 (Prescription) / TGO 92 (Non-Prescription)]
ARTG No.:           [AUST R xxxxxx / AUST L xxxxxx — or "Not visible"]
Sponsor:            [Sponsor name — or "Not visible"]
Review Type:        [e.g., TGO 91 Full Pack / TGO 92 Label + CHI / PI only]
Documents Reviewed: [Description — e.g., "Outer carton (front, back, spine) + blister foil"]
Review Date:        [Date]

──────────────────────────────────────────────────────
SECTION-BY-SECTION FINDINGS
──────────────────────────────────────────────────────

[For each checklist section relevant to this product:]

### [Section Name — e.g., "A1 — Schedule Entry / Cautionary Statement"]

| # | Requirement | TGO Ref | Result | Notes |
|---|---|---|---|---|
| A1.1 | [Requirement text] | [s.X] | PASS / FLAG / FAIL / N/A | [Note if not PASS] |
...

[Repeat for all sections]

──────────────────────────────────────────────────────
SUMMARY
──────────────────────────────────────────────────────

  PASS:    [n] items
  FLAG:    [n] items
  FAIL:    [n] items
  N/A:     [n] items

Overall Assessment:
  [ ] COMPLIANT — No issues identified; artwork recommended for approval
  [ ] MINOR ISSUES — FLAG items only; confirm before approval
  [ ] ISSUES IDENTIFIED — FAIL items present; artwork must be corrected before approval
  [ ] SIGNIFICANT ISSUES — Multiple FAILs or a critical FAIL (e.g., wrong schedule entry,
       wrong AUST R, missing active ingredient); do not approve — return for revision

──────────────────────────────────────────────────────
CRITICAL FINDINGS (FAIL items only)
──────────────────────────────────────────────────────

[List each FAIL item: item number, requirement, specific finding.
If no FAILs: "No critical findings."]

──────────────────────────────────────────────────────
FLAGS FOR REVIEW (FLAG items only)
──────────────────────────────────────────────────────

[List each FLAG item: item number, requirement, what needs confirmation.
If no FLAGs: "No flags."]

──────────────────────────────────────────────────────
RECOMMENDED ACTIONS
──────────────────────────────────────────────────────

1. [Mandatory corrections — FAIL items, in priority order]
2. [Items for confirmation — FLAG items]
3. [Advisory notes or good-practice recommendations]

──────────────────────────────────────────────────────
LIMITATIONS OF THIS REVIEW
──────────────────────────────────────────────────────

[Note items that could not be fully assessed due to:
  - Panels not visible in the artwork provided
  - Formulation/excipient list not provided (limits Schedule 1 assessment)
  - Items requiring cross-check with ARTG record or approved PI/CMI
  - State/territory-specific requirements (outside scope of this checklist)
  - Font size measurements (cannot be precisely measured from digital artwork alone)]

──────────────────────────────────────────────────────
```

---

## Quick Reference: Key Text Size Requirements (TGO 91 & 92)

| Element | Min Text Size | Reference |
|---|---|---|
| All mandatory text (general) | 1.5 mm cap height | s.7(2)(d) |
| AUST R / AUST L number | 1.0 mm (exception) | TG Regs 1990, s.15 |
| Active ingredient name + quantity (registered medicine, 1–3 AIs) | 3.0 mm | s.9(5) |
| Active ingredient name + quantity (4+ AIs, side/rear panel) | 2.5 mm | guidance |
| Active ingredient (medium container, non-prescription) | 2.5 mm on container | s.9(3) |
| Active ingredient (medium container, carton) | 3.0 mm on carton | s.9(5) |
| Ophthalmic: multidose statement on container | 2.0 mm | s.10(11)(b)(c) |
| Ophthalmic: batch number on container | 1.5 mm | s.10(11)(b)(i) |
| Ophthalmic: expiry date on container | 1.5 mm | s.10(11)(b)(j) |
| Very small containers (≤3 mL, prescription): all mandatory text on container | 1.0 mm | s.10(5) & s.10(12) |

---

## Important Caveats

- This review **supports the artwork approval process** but is not a substitute for QA sign-off, regulatory affairs sign-off, or TGA consultation.
- **TGO 91 / TGO 92 requirements are mandatory** — a non-compliant item is a FAIL regardless of how minor it appears visually.
- **Excipient warnings** require knowledge of the approved formulation. If the PI or full excipient list is not provided, all Schedule 1-related items must be flagged as unable to verify.
- **Cross-document consistency** (PI ↔ label ↔ CMI) is critical — inconsistencies between documents are always at minimum a FLAG.
- **Font size measurement** cannot be done precisely from digital artwork alone without a scale reference — flag borderline sizes for verification against print-ready files.
- **State/territory-specific requirements** (e.g., additional Poisons Act wording) are outside the scope of this checklist — note this limitation.
- **RASML warnings** change regularly — confirm the current RASML/MASS version when assessing warning statement compliance for non-prescription medicines.
- The TGA guidance document used to build this skill is *Labelling medicines to comply with TGO 91 and TGO 92*, Version 2.5, December 2024.
