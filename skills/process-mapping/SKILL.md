---
name: process-mapping
description: >
  Maps and documents operational processes from any input: verbal description,
  numbered steps, image, diagram, or conversation. Use to document processes,
  create flow diagrams, structure automations, or generate consulting deliverables
  with BPMN 2.0 notation. Triggers on: "map this process", "draw the flow",
  "document how we do X", "step by step", "process sketch", "need the BPMN file",
  "how long does this process take", "what systems are used", "PowerPoint diagram",
  "swim lanes", "Bizagi", "Camunda", "draw.io", "waiting times", "cycle time",
  "mapea este proceso", "dibuja el flujo", "documenta como hacemos X",
  "dibujito de proceso", "archivo BPMN", "cuanto tarda", "que sistemas usan".
  Produces: process sheet with systems and times per step, Mermaid diagram,
  exportable .bpmn file, and PowerPoint swim-lane slide.
---

# Process Mapping

Maps and documents operational processes in visual and written format.
Accepts any input type and produces documentation ready for non-technical teams
and AI agent design.

---

## Language — Mandatory First Step

**Before doing anything else — including reading the process, structuring steps, or generating any output — ask exactly this and wait for a confirmed answer:**

"¿El output debe estar en **español** o **inglés**? / Should the output be in **Spanish** or **English**?"

Do not proceed until the user replies. Do not infer language from the language of their message or the process content.

Once confirmed, apply that language consistently across: the process sheet, diagram labels, BPMN task names, PowerPoint slide text, and all section headers. Do not mix languages in a single output. XML element IDs in the .bpmn file stay in English regardless (technical requirement).

---

## Accepted Input Types

| Input | How to Process |
|---|---|
| Verbal description / free text | Extract steps, actors, decisions, and systems. Ask for what's missing. |
| Numbered step list | Structure directly, identify implicit decisions and systems |
| Uploaded image or diagram | Read `references/visual-input.md` first |
| Conversation / interview | Extract process from responses, confirm with user |
| Partial or incomplete process | Flag gaps explicitly, ask before filling in |

---

## Mandatory Capture Per Step

For each step, obtain:

| Field | Description | Ask If Missing |
|---|---|---|
| **Actor** | Who executes it (specific role, not generic title) | Always |
| **System** | What tool or app they use | Always |
| **Execution time** | Active working time (min) | Always — accept estimates |
| **Wait time** | Time waiting before next step (min/h) | Always — accept estimates |
| **Task type** | Manual / Rule-based / AI-automatable | Infer if obvious |

**Times are mandatory. Follow this protocol in order:**

1. Ask the user before building any output. Frame it as: "Approximate times are fine. For each step, roughly how long does the person actively work on it, and how long does it sit waiting before the next step picks it up?"
2. If the user provides estimates, use them as-is.
3. If the user says they don't know for a specific step, apply these defaults based on task type and mark with `*`:

| Task type | Execution default | Wait default |
|---|---|---|
| Manual data entry / form fill | 10 min* | 0 min* |
| Email send | 5 min* | 4 h* |
| Email receive / response wait | 2 min* | 24 h* |
| System approval (automated) | 2 min* | 0 min* |
| Physical delivery / courier | 15 min* (prep) | 3 days* |
| Human review / approval | 15 min* | 1 day* |
| System lookup / catalog check | 5 min* | 0 min* |
| Invoice / GR / financial processing | 10 min* | 2 days* |

Never leave times as N/A or blank in the final output.
Calculate at the end: **Cycle Time = Σ(execution) + Σ(wait)**. Flag derived totals with `*`.

**Task type classification (from BPMN element):**

| BPMN Element | Task Type | Automation Path |
|---|---|---|
| User Task | Manual | Human stays; flag for partial automation |
| Service Task | AI-automatable | Replace with agent or script |
| Exclusive Gateway (XOR) | Rule-based | Replace with rule engine or ML classifier |
| Data Object | AI-automatable | Replace with data transformation script |
| Start / End Event | Rule-based | Trigger via scheduler or API call |

---

## Output Levels

### Level 1 — Quick diagram (Mermaid)
For: fast visualization in chat, meeting presentation, doc insert.
Read `references/mermaid-guide.md` before generating.

### Level 2 — Structured documentation
For: formal record, agent design input, team onboarding.
Includes process sheet with systems and times per step.

### Level 3 — Full deliverable with artifacts (default)
For: sponsor proposal, consulting deliverable, initiative-tracker input.
Combines:
- Process sheet (text) with systems and times
- Mermaid diagram (chat visualization)
- Exportable `.bpmn` file (downloadable artifact) → read `references/bpmn-guide.md`
- PowerPoint swim-lane slide (downloadable artifact) → read `references/pptx-process-diagram.md`

**If the user does not specify a level, generate Level 3 by default.**

### Notation selection

| If the user asks for... | Use |
|---|---|
| "quick diagram", "for a meeting", "visualización rápida" | Mermaid |
| "BPMN", "file for Bizagi/Camunda/draw.io", "notación estándar" | BPMN 2.0 + .bpmn file |
| "presentation", "PowerPoint", "executive slide" | PPTX swim-lane |
| Not specified (Level 3) | All three formats |

---

## Mapping Principles

1. One step = one action. Do not combine "review and approve" into one step.
   These are two steps with potentially different actors.

2. Name actors specifically, not generically.
   "Commercial Manager" not "the boss". This matters for assigning AI agent responsibilities.

3. Make decisions explicit.
   If there is a "it depends" in the process, there is a decision diamond in the diagram.
   Never leave it implicit.

4. Tag systems on every step.
   Each step must show what tool or system the actor uses.
   This defines what integrations the agent needs and where bottlenecks live.

5. Capture real times.
   Distinguish execution time (active work) from wait time (queue, approval).
   Wait time is typically 5-10x larger and is where automation opportunity lives.

6. Identify the human in the loop.
   Mark with [HUMAN REVIEW] / [REVISION HUMANA] any step where a person must approve
   before continuing. This is never removed when automating.

---

## Output Format — Process Sheet

```
PROCESS / PROCESO: [name]
AREA: [name]            VERSION: 1.0       DATE: [date]
OWNER: [role]           FREQUENCY: [daily/weekly/monthly]
EXECUTION TIME TOTAL:   [N min/hours]
WAIT TIME TOTAL:        [N min/hours]
CYCLE TIME TOTAL:       [sum of both]

OBJECTIVE:
[what result this process produces — one line]

TRIGGER:
[what event or condition starts the process]

ACTORS:
- [Actor 1]: [role in process]
- [Actor 2]: [role in process]

SYSTEMS USED:
- [System 1]: [steps where used]
- [System 2]: [steps where used]

STEPS:
| # | Action | Actor | System | Exec. Time | Wait Time | Task Type |
|---|--------|-------|--------|------------|-----------|-----------|
| 1 | [action] | [actor] | [system] | [N min] | [N min] | [Manual/Rule-based/AI-automatable] |
| 2 | [action] | [actor] | [system] | [N min] | [N min] | [Manual/Rule-based/AI-automatable] |

DIAGRAM (Mermaid):
[mermaid diagram here]

EXCEPTIONS AND SPECIAL CASES:
- [Exception 1]: [what happens and who resolves]

PAIN POINTS IDENTIFIED:
- [step N]: [problem description — specify if execution or wait time issue]

AUTOMATION OPPORTUNITIES:
- [step N]: [task type: AI-automatable / Rule-based] — [what an agent could do here]
- Agent pattern: [from ai-agent-builder skill]
- Systems / integrations required: [list]
- Recoverable time estimate: [N min per occurrence]
- Note: each automatable task maps to one agent or API endpoint

RECOMMENDED NEXT STEP:
[one concrete action]
```

---

## Artifacts to Generate (Level 3)

After completing the process sheet, generate automatically:

1. **`.bpmn` file** — BPMN 2.0 XML with all tasks, gateways, and system/time metadata
   in `<documentation>` per element. Task names use the output language chosen.
   Read `references/bpmn-guide.md`. Save as `/mnt/user-data/outputs/[process].bpmn`.

2. **PowerPoint slide** — Swim-lane diagram with actors, systems, times, and legend.
   Slide text uses the output language chosen.
   Read `references/pptx-process-diagram.md`. Save as `/mnt/user-data/outputs/[process]_diagram.pptx`.

Present both files with `present_files` when done.

---

## Final Step — Audit Log

After presenting files, run:

```bash
python references/audit-log-writer.py --append \
  --path "[audit_log path from Global Instructions]" \
  --skill "process-mapping" \
  --proyecto "[active Cowork project]" \
  --trigger "[exact phrase user typed]" \
  --idioma "[español/inglés]" \
  --entregables "[comma-separated output filenames]" \
  --duracion "[estimated minutes]" \
  --usuario "[user name if known]" \
  --notas "[any exception or variation, or empty]"
```

If `audit_log` path is not in Global Instructions, run first-run setup:
```bash
python references/audit-log-writer.py --init \
  --path ~/Claude-Workspace/audit/skill-audit-log.xlsx
```
Then prompt user to add `audit_log: ~/Claude-Workspace/audit/skill-audit-log.xlsx`
to their Global Instructions. Do not block delivery on this step.

---

## References

- `references/mermaid-guide.md` — syntax and patterns for flow diagrams
- `references/bpmn-guide.md` — BPMN 2.0 notation, exportable XML, tool compatibility
- `references/pptx-process-diagram.md` — swim-lane diagram in PowerPoint with pptxgenjs
- `references/visual-input.md` — how to process uploaded images and process sketches
- `references/audit-log-writer.py` — writes one row to centralized audit Excel log
