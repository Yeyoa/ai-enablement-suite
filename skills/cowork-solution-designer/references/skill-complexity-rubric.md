# Skill Complexity Rubric

## Purpose
Score each skill in the solution design as Low / Medium / High build effort.
Be honest. Do not underestimate to make the solution look easier.

## Scoring Criteria

| Factor | Low | Medium | High |
|---|---|---|---|
| Connectors required | 0–1 | 2 | 3+ |
| Input type | Single file or text | Multiple files or structured form | Unknown system / image / legacy export |
| Output type | Single file (md, xlsx, pptx) | Multiple files or system write | Multi-step with conditional branches |
| Human checkpoint complexity | 1 clear approval step | 2 approval steps | Approval depends on content of output |
| Process variability | Fixed, predictable steps | Some conditional logic | High variability, many edge cases |
| System integration risk | System has Cowork connector | System needs manual export workaround | System is unknown or behind firewall |

## Scoring Rule
Score each factor independently. Take the highest score across all factors.
If 2+ factors score High, flag it as "High — requires scoping session before build."

## Build Effort Estimates (for stakeholder communication)

| Complexity | Estimated build time | Who can build it |
|---|---|---|
| Low | 1–2 hours | Analyst with Cowork access |
| Medium | Half day | AI Enablement lead or trained analyst |
| High | 1–3 days | AI Enablement lead + system owner |

## Notes
- "Build" means writing and testing the SKILL.md, not deploying infrastructure.
- A skill that reads a known file type and writes a known output is always Low,
  regardless of how complex the content transformation is.
- A skill that must write back to a system (CRM update, ERP entry, email send)
  is always at least Medium.
- Never score a skill as Low if the output system is unknown.
