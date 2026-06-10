---
name: adoption-playbook
description: >
  Generates a step-by-step user adoption guide for a newly built Cowork skill.
  Use this skill whenever a skill has been built and approved and the next step
  is enabling colleagues to use it in Cowork. Triggers on: "genera la guia de
  adopcion", "generate adoption guide", "como le explico esto a mi equipo",
  "how do they use this skill", "onboarding para el skill", "adoption playbook",
  "guia de usuario", "user guide for the skill", "como lo programo en schedule",
  "como hago que sea repetible para mis colegas". Also triggers automatically
  when cowork-solution-designer output contains a handoff flag pointing to
  adoption-playbook. Produces a PowerPoint adoption deck with branding applied,
  covering: what the skill does, when to run it, step-by-step activation guide,
  schedule setup, human checkpoints, and troubleshooting.
---

# Adoption Playbook

Generates a user-facing adoption guide for a Cowork skill.
Takes the approved SKILL.md and TASK.md as inputs and produces
a .pptx deck that a non-technical colleague can follow independently.

This is Step 4 of 4 in the AI Enablement consulting workflow:
1. Process Discovery
2. Process Mapping
3. Solution Design → cowork-solution-designer skill
4. **Adoption Playbook ← this skill**

---

## Language — Mandatory First Step

**Ask this before anything else:**

"¿La guía de adopción debe estar en **español** o **inglés**?
/ Should the adoption guide be in **Spanish** or **English**?"

Apply consistently across all slide text, step labels, and callouts.
Do not mix languages.

---

## Required Inputs

| Input | Source | Required |
|---|---|---|
| SKILL.md of the new skill | skill-creator output | Yes |
| TASK.md contract | skill-creator output (post-approval) | Yes |
| Branding context file | Project context folder | If exists |
| Colleague profile | Who will use this daily | Ask if missing |

If TASK.md is missing, ask the user to provide the deliverables list
before proceeding. Do not infer it from the SKILL.md alone.

---

## Execution Protocol

### Step 1 — Extract skill profile

From SKILL.md and TASK.md, extract:
- Skill name and one-line purpose
- Trigger phrase(s) the colleague will say
- Input(s) the colleague must provide
- Output(s) they will receive
- Human checkpoints (steps requiring their approval)
- Schedule eligibility (can this run automatically?)
- Connectors required (what must be connected first)

### Step 2 — Read Cowork setup reference

Read `references/cowork-setup-guide.md` before generating any slides.
It contains the exact steps for: opening Cowork, creating a Project,
adding context files, activating connectors, and setting up /schedule.

### Step 3 — Read PPT spec

Read `references/pptx-adoption-deck.md` before generating the deck.
It contains the slide-by-slide layout, branding spec, and pptxgenjs template.

### Step 4 — Generate the deck

Produce a .pptx with the structure defined in references/pptx-adoption-deck.md.
Apply branding from the project's brand context file if present.
Default branding if no context file: white background, navy headers, no color.

Save as: `/mnt/user-data/outputs/[skill-name]_adoption_guide.pptx`

### Step 5 — Run POST-EXECUTION verification

Use requirements-checker to verify all deliverables before presenting.

### Step 6 — Present

Present the file with present_files.
End with the handoff note:
"Guía lista. Próximo paso: instalar el skill en Cowork
y compartir este deck con los colegas que lo operarán."

---

## Deck Structure (7 slides)

| Slide | Título | Contenido |
|---|---|---|
| 1 | Portada | Nombre del skill, propósito, fecha, autor |
| 2 | ¿Qué hace este skill? | Propósito, trigger phrase, input → output visual |
| 3 | Antes de empezar | Requisitos: plan, connectors, carpeta, context files |
| 4 | Paso a paso: cómo activarlo | Numbered steps con screenshots placeholders |
| 5 | Human checkpoints | Qué requiere tu aprobación y por qué |
| 6 | Programarlo en Schedule | Cómo usar /schedule, frecuencia recomendada, limitaciones |
| 7 | Qué hacer si algo falla | Troubleshooting básico + a quién contactar |

---

## Design Principles

1. **Escrito para el colega, no para el constructor.** Ninguna jerga
   técnica sin explicación. Si el step dice "activa el connector de Gmail",
   el deck explica dónde está ese botón.

2. **Cada slide tiene una sola acción.** No acumular instrucciones.
   Una slide = un paso = una decisión.

3. **Los human checkpoints se explican con el porqué.** No "Claude te
   pedirá aprobación". Sino "Claude te mostrará el borrador del email
   antes de enviarlo — este es tu momento de revisarlo."

4. **Schedule se explica con la limitación honesta.** El deck debe
   mencionar que el schedule requiere que la computadora esté encendida
   y Cowork abierto. No prometer automatización sin esa condición.

5. **Branding del cliente siempre aplicado.** Si existe brand context
   file en el proyecto, todos los slides lo usan. No mezclar paletas.

### Step 6 — Audit Log (Paso C)

```bash
python references/audit-log-writer.py --append \
  --path "[audit_log path from Global Instructions]" \
  --skill "adoption-playbook" \
  --proyecto "[active Cowork project]" \
  --trigger "[exact phrase user typed]" \
  --idioma "[español/inglés]" \
  --entregables "[skill-name]_adoption_guide.pptx" \
  --duracion "[estimated minutes]" \
  --usuario "[user name if known]" \
  --notas "[any exception]"
```

If `audit_log` not found in Global Instructions, run first-run setup:
```bash
python references/audit-log-writer.py --init \
  --path ~/Claude-Workspace/audit/skill-audit-log.xlsx
```
Do not block delivery on this step.

---

## References

- `references/cowork-setup-guide.md` — Pasos de configuración de Cowork
- `references/pptx-adoption-deck.md` — Layout de slides y template pptxgenjs
- `references/audit-log-writer.py` — Writes one row to centralized audit Excel log
