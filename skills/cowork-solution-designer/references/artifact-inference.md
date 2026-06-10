# Artifact Inference Reference

## Purpose
Determine the correct output file format for each skill based on what the
process naturally produces, who consumes it, and what happens to it next.

Never default to a fixed format. Read the process, infer the artifact.

---

## Primary Inference Table

| Process type / output destination | Inferred artifact | Rationale |
|---|---|---|
| Regulatory submission, SOP, procedure | .docx | Word is the standard for controlled documents in pharma/regulated environments |
| Batch record, deviation report, CAPA | .docx | Structured narrative with signatures — Word or PDF |
| Management / sponsor presentation | .pptx | Slides for live review meetings |
| Metrics dashboard, KPI report | .xlsx | Data needs to be filtered, sorted, charted downstream |
| Consolidation from multiple sources | .xlsx | Tabular aggregation with formulas |
| Audit trail, log, register | .xlsx | Row-based records, filterable |
| Customer/partner communication | Draft text (no file) | Goes into email or system directly |
| System upload (ERP, LIMS, CRM) | .xlsx or .csv | System import format |
| Internal briefing for decision-making | .docx or .pptx | Depends on audience (operational = doc, executive = slides) |
| Training material | .pptx or .docx | Slides for delivery, Word for reference manual |
| Financial model, forecast | .xlsx | Formulas, scenarios, charts |
| Project status update | .pptx | Slide for steering committee |
| Certificate, label, formal record | .pdf | Fixed layout, non-editable |
| Unknown / unclear | Flag as [OUTPUT TBD] | Ask once, then flag and continue |

---

## Secondary Signals — Override Primary If Present

| Signal in process | Override to |
|---|---|
| "Subject to approval / sign-off" | .docx or .pdf (supports tracked changes or wet signature) |
| "Goes into [named system]" | Match that system's import format — check with user |
| "Presented to committee / management" | .pptx regardless of content type |
| "Filed / archived for compliance" | .pdf (fixed, non-editable) |
| "Used by analyst for further work" | .xlsx (editable, formula-ready) |
| "Sent externally (client, authority)" | .pdf (professional, layout-locked) |

---

## Multi-Output Skills

Some skills produce more than one artifact. This is valid when the same data
serves two different audiences or purposes.

Examples:
- Batch analysis → .xlsx (analyst data) + .docx (QC summary for manager)
- Monthly metrics → .xlsx (raw data) + .pptx (executive update)
- Deviation investigation → .docx (narrative report) + .xlsx (trend log)

When designing a multi-output skill:
- List both outputs in the Skill card
- Both get their own save path
- The human checkpoint covers both outputs before either is distributed

---

## Pharma-Specific Notes

| Document type | Standard format | Notes |
|---|---|---|
| SOP / Work Instruction | .docx | Controlled document — include version, effective date, approver fields |
| Batch Record | .docx or system form | If system form: design as text draft for copy-paste |
| CAPA / Deviation | .docx | Narrative structure, root cause, action items |
| Change Control | .docx | Includes impact assessment section |
| Stability Report | .xlsx + .docx | Data in Excel, narrative in Word |
| Protocol / Report | .docx | GxP: protocol and report are separate documents |
| Training Record | .xlsx | Roster with completion dates, trainer, version trained on |

---

## When Output Is Truly Unknown

If the process step says "uploads to system" or "sends to external party" but
the system or format is unspecified:

1. Flag the skill output as `[OUTPUT TBD]`
2. Note in the Unknowns log: "Step N — output destination unclear. Ask [actor] what system receives this."
3. Design the skill to produce a generic .docx or .xlsx as a placeholder
4. Mark the Skill card: `Output format: [OUTPUT TBD — placeholder: .docx]`

Do not block. Produce something and flag it.
