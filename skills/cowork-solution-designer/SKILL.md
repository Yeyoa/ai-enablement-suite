---
name: cowork-solution-designer
description: >
  Designs AI workflow solutions in Claude Cowork from a mapped process with identified
  automation opportunities. Use whenever a process has been analyzed and the next step
  is designing how colleagues will operate it using Cowork. Triggers on: "diseña la
  solución", "design the cowork setup", "how should we build this in Cowork", "define
  the skills needed", "design the AI workflow", "qué skills necesitamos", "cómo lo
  implementamos en Cowork", "architecture for this process", "build the agent setup",
  "define global instructions", "qué va en el proyecto", "design the automation",
  "solution design", "diseño de solución". Also triggers when process-mapping output
  contains AUTOMATION OPPORTUNITIES and user asks "what now" or "next step".
  Produces: PowerPoint solution architecture deck, Global Instructions draft, Project
  definition, Skill specification cards per automation opportunity.
---

# Cowork Solution Designer

Designs the AI workflow architecture for a mapped process. Takes AUTOMATION
OPPORTUNITIES as input and produces a complete, ready-to-present Cowork
implementation design: Global Instructions, Project definition, Skill
specifications, and a PowerPoint deck.

This is Step 3 of 4 in the AI Enablement consulting workflow:
1. Process Discovery (identify AI-susceptible steps)
2. Process Mapping → process-mapping skill
3. **Solution Design ← this skill**
4. Adoption Playbook → adoption-playbook skill

---

## Language — Mandatory First Step

**Ask this before anything else:**

"¿El output debe estar en **español** o **inglés**? / Should the output be in **Spanish** or **English**?"

Apply consistently across all outputs. Do not mix languages.

---

## Accepted Inputs

| Input | How to handle |
|---|---|
| process-mapping output (full) | Use AUTOMATION OPPORTUNITIES section directly |
| Pasted process steps with AI tags | Extract automation candidates, ask for missing context |
| Free description of a process | Run a light capture (actor, system, task type per step) before designing |
| Uploaded image / diagram | Read process, identify automatable steps, confirm before designing |

**Minimum required to proceed:**
- List of steps marked as AI-automatable or Rule-based
- Actor(s) who will operate the solution (colleagues)
- Systems involved (even if partial)

If missing, ask once. Do not block on unknowns — flag them and proceed with
assumptions marked `[ASSUMPTION]`.

---

## Step 0 — Infer Process Artifacts

Before designing anything, read the process steps and identify what file types
the process naturally produces or consumes. This determines what each skill
should output.

Read `references/artifact-inference.md` for the full inference table.

**Quick rules:**

| If the process involves... | Likely artifact |
|---|---|
| Reports, summaries, regulatory docs | .docx (Word / SOP) |
| Data consolidation, metrics, tracking | .xlsx (Excel) |
| Presentations to management or sponsors | .pptx (PowerPoint) |
| Structured records, logs, audit trails | .xlsx or .pdf |
| Emails or communications | Draft text (no file) |
| Mixed: document + data | Both .docx and .xlsx |
| Unknown output destination | Flag as [OUTPUT TBD] |

Populate this table before proceeding — one row per skill:

```
ARTIFACT INFERENCE TABLE
| Skill | Input type | Output type | Rationale |
|-------|-----------|-------------|-----------|
| [name] | [file/text/system] | [.xlsx/.docx/.pptx/draft] | [why] |
```

Show this table to the user and confirm before building skill cards.
If the user corrects any row, update before proceeding.

---

## Design Framework — Three Cowork Levers

Every solution is composed of exactly three structural elements. Design all three.

### 1. Global Instructions

Rules that apply to every session, every colleague, every task.
Behavioral constraints only — not domain knowledge.

**Design criteria:**
- Safety guardrails (what Claude must never do without approval)
- Output conventions (file naming, save locations)
- Language and tone for this team
- Human-in-the-loop rules (which steps always require a person to approve)

**Template:**
```
GLOBAL INSTRUCTIONS — [Process Name]

Always:
- [rule]

Never:
- [rule]

Default save location: /[folder]/
File naming: YYYY-MM-DD_[descriptor]_[version]
Language: [language]
Human approval required before: [actions — e.g. sending, uploading, deleting]
```

### 2. Project Definition

An isolated Cowork workspace scoped to this process.

**Template:**
```
PROJECT: [Name]
Purpose: [one line]

Context files:
- about-process.md: [what to include — SOPs, regulatory refs, team roles]
- output-templates.md: [links or paths to template files]
- [other].md: [purpose]

Connectors needed:
- [System]: [what Claude uses it for] — [Available / MANUAL INPUT]

Folder structure:
/[project-root]/
  /inputs/     ← [source files colleagues drop here]
  /outputs/    ← [what Claude produces]
  /templates/  ← [Excel, Word, PPT templates Claude uses]
  /archive/    ← [processed inputs, versioned outputs]
```

**Connector availability:** Check `references/cowork-connectors.md`.
For any system not in the connector list, mark as `[MANUAL INPUT — connector TBD]`
and design the skill to accept a file export instead.

### 3. Skill Cards

One skill per distinct automation. Each is a reusable module triggered by a
natural language phrase.

**Card template:**
```
SKILL: [name]
Priority: [1 = build first]
Trigger phrase: "[what a colleague would say to start this]"

Input:
  - Type: [file upload / text / nothing — colleague provides nothing]
  - Format: [.xlsx / .pdf / .docx / free text / n/a]
  - Source: [where the colleague gets it — system export, email, shared folder]

What Claude does:
  1. [action]
  2. [action]
  3. [action — include [HUMAN REVIEW] tag where approval is required]

Output:
  - File: [filename pattern]
  - Format: [.xlsx / .docx / .pptx / draft text]
  - Saved to: /[folder]/
  - Rationale: [why this format — from artifact inference table]

Connectors: [list — or MANUAL INPUT]
Human checkpoint: [step N — what the person reviews and approves]
Estimated time saved: [N min per occurrence]
Complexity: [Low / Medium / High — read references/skill-complexity-rubric.md]
```

---

## Execution Protocol

### Step 1 — Parse Automation Opportunities
Extract from process-mapping AUTOMATION OPPORTUNITIES: step number, task type,
systems, recoverable time. If input is free-form, do a light capture first.

### Step 2 — Run Artifact Inference (Step 0 above)
Build the inference table. Confirm with user before proceeding.

### Step 3 — Classify by Cowork Lever

| If the opportunity is... | Design it as... |
|---|---|
| A safety boundary or approval rule | Global Instruction |
| Knowledge the agent needs every session | Context file in Project |
| A recurring task a colleague triggers | Skill |
| A connection to an external system | Connector in Project |
| A repeating background task | Scheduled task — note in Skill card |

### Step 4 — Draft Text Deliverables
Produce in order:
1. Global Instructions (copy-paste ready)
2. Project definition sheet
3. Skill cards (one per automation, output format from inference table)

### Step 5 — Generate PowerPoint Deck
Read `references/solution-slide.md` before generating.

Deck structure (one slide per section):
- **Slide 1 — Architecture Overview:** Process name, 3-column layout
  (Global Instructions / Project + Connectors / Skills summary),
  footer KPIs (skills count, time saved, connectors needed)
- **Slide 2 — Skill Cards:** One card per skill with trigger phrase,
  input→output, complexity badge, time saved
- **Slide 3 — Artifact Map:** Table showing each skill, its output format,
  and the rationale (from inference table)
- **Slide 4 — Unknowns & Next Steps:** Open items, handoff to adoption-playbook

Save as: `/mnt/user-data/outputs/[process-name]_solution.pptx`

### Step 6 — Surface Unknowns
List explicitly after outputs:
- Systems not identified (`[UNKNOWN SYSTEM]`)
- Output destinations unclear (`[OUTPUT TBD]`)
- Skills marked High complexity needing scoping
- Connectors not available in Cowork (`[MANUAL INPUT]`)

### Step 7 — Handoff Flag (Paso A completo)
Always end Paso A with:
```
NEXT STEP → Paso B: Skill Specifications
Colleagues who will operate this: [list]
Skills requiring training: [list — with complexity score]
Estimated onboarding effort: [Low / Medium / High]
```

---

## Paso B — Skill Specifications

For each skill card produced in Paso A, generate a full SKILL.md draft.
This is the build input a developer or AI Enablement lead uses to
construct the actual skill in Cowork.

### For each skill card, produce:

```markdown
---
name: [skill-name]
description: >
  [Trigger phrases from the skill card. Include natural language
   variants the colleague would actually say. Be pushy — list at
   least 5 trigger phrases in Spanish and English.]
---

# [Skill Name]

[One paragraph: what it does, for whom, in what context.]

## Language
Ask: "¿Español o inglés?" before generating any output.

## Input
[What the colleague provides — file, text, or nothing.]

## Execution Protocol
[Step-by-step, extracted from the skill card's "What Claude does" list.
 Include [HUMAN REVIEW] tags where approval is required.]

## Output
[File name pattern, format, save path — from artifact inference table.]

## Final Step — Audit Log
[Standard audit block — see below.]

## References
[Any reference files this skill needs.]
```

**Standard audit block to append to every skill generated:**
```
### Final Step — Audit Log
Run references/audit-log-writer.py --append with:
- skill: [this skill's name]
- trigger: [phrase the user typed]
- entregables: [files produced]
- notas: [any exception]
Read SKILL.md of skill-audit-logger for full parameter list.
```

One SKILL.md draft per skill card. Present all drafts inline for review
before the user takes them to skill-creator for full construction.

---

## Paso C — Audit Log

After completing Paso A and Paso B, run:

```bash
python references/audit-log-writer.py --append \
  --path "[audit_log path from Global Instructions]" \
  --skill "cowork-solution-designer" \
  --proyecto "[active Cowork project]" \
  --trigger "[exact phrase user typed]" \
  --idioma "[español/inglés]" \
  --entregables "[solution deck filename], [skill drafts count] skill drafts" \
  --duracion "[estimated minutes]" \
  --usuario "[user name if known]" \
  --notas "[any exception or variation]"
```

If `audit_log` not found in Global Instructions, run first-run setup:
```bash
python references/audit-log-writer.py --init \
  --path ~/Claude-Workspace/audit/skill-audit-log.xlsx
```
Prompt user to add `audit_log: ~/Claude-Workspace/audit/skill-audit-log.xlsx`
to Global Instructions. Do not block delivery on this step.

---

## Output Summary

| Deliverable | Paso | Format | Purpose |
|---|---|---|---|
| Artifact Inference Table | A | Text | Grounds output formats |
| Global Instructions draft | A | Text block | Copy into Cowork settings |
| Project definition sheet | A | Text block | Setup guide |
| Solution deck | A | .pptx (4 slides) | Sponsor / interview deliverable |
| Unknowns log | A | Text list | Follow-up items |
| Skill SKILL.md drafts | B | Text blocks | Build input for skill-creator |
| Audit log entry | C | Excel row | Usage record |

---

## Design Principles

1. **Output format follows the process, not a template.** A QC process that
   ends with a batch record gets a .docx SOP. A metrics process gets a .xlsx.
   A management update gets a .pptx. Never default to a fixed format.

2. **One skill per distinct trigger.** Do not bundle unrelated automations.
   A colleague says one phrase, gets one outcome.

3. **Every skill has a human checkpoint.** At minimum one approval step per
   skill. Never design fully autonomous execution in a regulated or operational
   context.

4. **Unknown systems are not blockers.** No Cowork connector → design for
   manual file export input. Mark it, flag it, move on.

5. **Global Instructions are guardrails, not instructions.** Domain knowledge
   goes in context files. Rules go in Global Instructions. Never mix them.

6. **Complexity is honest.** Read the rubric. A skill writing back to a system
   is always at least Medium. Do not understate build effort.

7. **Design for colleagues, not for yourself.** Trigger phrases must be natural
   for the person doing the job daily. "Genera el informe de lote" beats
   "execute batch record automation pipeline."

---

## References

- `references/artifact-inference.md` — Full inference table for output format by process type
- `references/solution-slide.md` — PowerPoint deck layout and pptxgenjs template (4 slides)
- `references/cowork-connectors.md` — Available Cowork connectors and manual input workarounds
- `references/skill-complexity-rubric.md` — Low / Medium / High scoring criteria
- `references/audit-log-writer.py` — Writes one row to centralized audit Excel log
