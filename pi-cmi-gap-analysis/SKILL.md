[SKILL.md](https://github.com/user-attachments/files/32719517/SKILL.md)
---
name: pi-cmi-gap-analysis
description: >
  Perform a structured section-by-section gap analysis of a Product Information (PI) or
  Consumer Medicine Information (CMI) document for an Australian medicine. Produces a
  MATCH / GAP / MINOR finding for each section against a reference document or TGA
  structural requirements. Always use this skill whenever the user uploads or describes
  a PI or CMI and wants to: compare it against a reference or approved PI; check
  consistency between PI and CMI; compare PIs across multiple brands of the same molecule;
  or audit a PI/CMI for grammar, spelling, or structural errors. Trigger for: "PI gap
  analysis", "CMI review", "compare PI to reference", "check PI across brands",
  "compare PI to approved", "PI audit", "CMI audit", "check PI against innovator",
  "does the CMI match the PI", "PI inconsistencies", "Milestone 5", "S31 PI comparison",
  or any request to identify differences between two or more PI/CMI documents.
---

# PI / CMI Gap Analysis Skill

## Purpose

Perform a structured, section-by-section gap analysis of Australian medicine PI and CMI documents. Produce a formatted Gap Report with a **MATCH / GAP / MINOR** finding for each section, a summary table, and prioritised action items.

---

## Scope — Four Analysis Modes

Determine which mode applies before proceeding:

| Mode | When to Use | User Provides |
|---|---|---|
| **A — Reference Comparison** | Comparing a sponsor's PI against a TGA-approved reference PI (innovator or approved generic) | Sponsor PI + Reference PI |
| **B — Multi-Brand Consistency** | Comparing PIs or CMIs across two or more brands of the same molecule | All brand PIs and/or CMIs |
| **C — PI ↔ CMI Consistency** | Checking that the CMI accurately reflects the approved PI | PI + CMI (same product) |
| **D — Structural / Quality Audit** | Reviewing a single PI or CMI for required sections, grammar, spelling, formatting | Single PI or CMI |

> If unclear which mode applies, ask the user before proceeding. State the mode selected at the top of the output.

---

## Reference Files

Load the appropriate file(s) before starting any review:

| Analysis Scope | Reference File |
|---|---|
| Any PI review (Modes A, B, D) | `references/pi_required_sections.md` |
| Any CMI review (Modes B, C, D) | `references/cmi_required_sections.md` |
| PI ↔ CMI consistency (Mode C) | Both files above |
| Multi-brand PI + CMI (Mode B) | Both files above |

> **Always read the relevant reference file(s) before running the analysis.** Do not rely on memory for required sections or consistency rules.

---

## Workflow

### Step 1 — Determine Mode and Load References

Select the analysis mode from the table above. Load the required reference file(s). State the mode, documents received, and reference file(s) used at the top of the output.

### Step 2 — Extract Document Metadata

For each document provided, extract and record:
- Product/brand name
- Active ingredient(s) and strength(s)
- Dosage form
- Sponsor name
- ARTG number (AUST R / AUST L — if visible)
- Date of most recent amendment (PI) or date of preparation (CMI)
- Whether this is the sponsor document or the reference document

If any of these are missing or unclear, note "not visible/not provided."

### Step 3 — Run the Section-by-Section Analysis

Work through each major section of the PI or CMI. For each section:

**Mode A (Reference Comparison):**
- Compare the sponsor PI section against the corresponding reference PI section
- Assign MATCH / GAP / MINOR for each section
- For GAPs: state what is present in the reference but absent or materially different in the sponsor PI
- For MINORs: state the specific wording issue (grammar, spelling, formatting, minor phrasing difference)

**Mode B (Multi-Brand Consistency):**
- Compare corresponding sections across all brands side-by-side
- Flag any section where brands differ meaningfully
- Flag any section where one brand is missing content present in another

**Mode C (PI ↔ CMI Consistency):**
- For each CMI section, identify the corresponding PI section
- Confirm that the CMI content is consistent with (and does not extend beyond) the PI
- Flag contradictions, missing key content, or outdated information in the CMI

**Mode D (Structural / Quality Audit):**
- Check for the presence of all required sections per the reference file
- Flag missing sections as GAP
- Flag grammar, spelling, and formatting issues as MINOR
- Identify any content that appears outdated, inconsistent, or ambiguous

### Step 4 — Assign Findings

| Rating | Meaning |
|---|---|
| **MATCH** | Section is present and consistent with the reference or requirements |
| **GAP** | Substantive content is missing, materially incorrect, or inconsistent — requires correction |
| **MINOR** | Present and substantially correct, but has grammar/spelling/formatting issues or minor wording differences |
| **N/A** | Section not applicable to this dosage form or product type |
| **UNABLE TO ASSESS** | Section could not be reviewed due to missing document, redacted content, or unclear scope |

### Step 5 — Produce the Gap Report

---

## Output Format

```
PI / CMI GAP ANALYSIS REPORT
──────────────────────────────────────────────────────

Analysis Mode:       [A / B / C / D — with description]
Active Ingredient:   [INN / AAN]
Dosage Form:         [e.g., Tablets, 10 mg]

Documents Reviewed:
  [Sponsor/Brand 1]  — [Brand name, AUST R, Amendment date]
  [Reference / Brand 2]  — [Brand name, AUST R, Amendment date]
  [Additional brands if applicable]

Reference Files Used:
  [pi_required_sections.md / cmi_required_sections.md]

Review Date:         [Date]

──────────────────────────────────────────────────────
SECTION-BY-SECTION FINDINGS
──────────────────────────────────────────────────────

[For each major PI or CMI section:]

### [Section Number and Name — e.g., "3. Pharmacology"]

| Finding | Detail |
|---|---|
| Result | MATCH / GAP / MINOR / N/A / UNABLE TO ASSESS |
| Issue | [Description of the gap or discrepancy — leave blank if MATCH] |
| Reference | [What the reference PI/CMI states — for Mode A only] |
| Action Required | [Yes (mandatory) / Yes (recommended) / No] |

[Repeat for all sections]

──────────────────────────────────────────────────────
SUMMARY TABLE
──────────────────────────────────────────────────────

| Section | Result | Action Required |
|---|---|---|
| 1. Name of the Medicine | MATCH / GAP / MINOR | Yes / No |
| 2. Description | ... | ... |
| 3. Pharmacology | ... | ... |
| ... | ... | ... |

  MATCH:              [n] sections
  GAP:                [n] sections — mandatory corrections
  MINOR:              [n] sections — recommended corrections
  N/A:                [n] sections
  UNABLE TO ASSESS:   [n] sections

──────────────────────────────────────────────────────
OVERALL ASSESSMENT
──────────────────────────────────────────────────────

  [ ] CONSISTENT — No substantive gaps identified; minor issues only (if any)
  [ ] MINOR ISSUES ONLY — MINOR findings only; corrections recommended before next submission
  [ ] GAPS IDENTIFIED — One or more GAP findings; PI/CMI must be updated
  [ ] SIGNIFICANT GAPS — Multiple or critical GAPs (e.g., missing safety information,
       inconsistent indication, missing CMI overdose guidance); high priority for correction

──────────────────────────────────────────────────────
GAPS — MANDATORY CORRECTIONS
──────────────────────────────────────────────────────

[List each GAP finding in priority order:]

1. Section [X]: [What is missing or inconsistent, and what must be added/corrected]
   Reference text: [Relevant content from reference PI or required section — paraphrased]

[If no GAPs: "No mandatory corrections identified."]

──────────────────────────────────────────────────────
MINOR ISSUES — RECOMMENDED CORRECTIONS
──────────────────────────────────────────────────────

[List each MINOR finding:]

1. Section [X]: [Grammar/spelling/formatting issue and suggested correction]

[If no MINORs: "No minor issues identified."]

──────────────────────────────────────────────────────
PRIORITISED ACTION PLAN
──────────────────────────────────────────────────────

Priority 1 — Mandatory (submit PI/CMI update to TGA):
  [List GAPs that require regulatory submission, with suggested variation pathway if known]

Priority 2 — Recommended (include in next scheduled update):
  [List MINORs and low-priority GAPs]

Priority 3 — Advisory (monitor or confirm):
  [Any sections requiring cross-check with ARTG record, sponsor confirmation, or TGA query]

──────────────────────────────────────────────────────
LIMITATIONS OF THIS ANALYSIS
──────────────────────────────────────────────────────

[Note what could not be fully assessed:]
  - Sections not provided or redacted
  - Inability to cross-check against ARTG record directly
  - Version of the reference PI used (date, amendment number)
  - Any TGA-approved variations not yet reflected in provided documents
  - Clinical content requiring medical/pharmacological expertise to fully evaluate

──────────────────────────────────────────────────────
```

---

## Important Caveats

- This analysis supports regulatory document review but is not a substitute for TGA advice or sign-off by a senior RA professional.
- For generic medicines, the sponsor PI must reflect the safety information in the reference PI — deletion or material downgrading of safety warnings is not acceptable.
- For PI updates triggered by an S31 or SAR, confirm whether the PI amendment requires a Category 1, Category 3, or Notification submission depending on the nature of the change.
- A PI/CMI discrepancy identified here may itself require a separate regulatory action (e.g., ARTG variation) if the approved PI does not yet reflect current clinical evidence.
- Always verify the version/amendment date of both the sponsor and reference documents — comparing against an outdated reference PI may produce false GAPs.
- Cross-document consistency (PI ↔ CMI ↔ label artwork ↔ ARTG record) is the responsibility of the sponsor; flag any suspected misalignments for further investigation.
