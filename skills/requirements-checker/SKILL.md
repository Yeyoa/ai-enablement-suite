---
name: requirements-checker
description: >
  Verifies that all deliverables in a task are completed before closing.
  Use this skill whenever a task has multiple outputs, entregables, or steps
  that must all be produced. Triggers on: "check my requirements", "did you
  complete everything", "verify deliverables", "what did you miss", "checklist
  before sending", "verifica entregables", "checklist de requerimientos",
  "revisa que este completo", "no omitas nada", "asegurate de completar todo".
  Also triggers automatically at the START of any multi-deliverable task when
  a TASK.md contract exists in the project — read it and show the checklist
  before executing. And triggers at the END of any task execution to verify
  all items were produced. Never skip this skill when a task has 2+ outputs.
---

# Requirements Checker

Prevents deliverable omission by making the task contract explicit before
execution and verifying completion after.

Two modes:
- **PRE-EXECUTION**: Extract deliverables, show checklist, get confirmation
- **POST-EXECUTION**: Verify each deliverable was produced, flag gaps

---

## When to Run Each Mode

| Situation | Mode |
|---|---|
| User gives a multi-deliverable task | PRE → then POST after execution |
| TASK.md exists in project | PRE (read it) → POST after execution |
| User says "did you miss anything" | POST only |
| Skill-creator approves an output | POST (generates TASK.md — see below) |
| Any task with 2+ files, slides, or documents | PRE + POST |

**Default**: when in doubt, run both. PRE costs one confirmation step.
POST costs nothing and catches omissions before they reach the user.

---

## PRE-EXECUTION Protocol

### Step 1 — Extract deliverables

Read the user's request carefully. Extract every item that must be produced.
Sources to check:
- Explicit file names mentioned ("the .pptx", "el diagrama", "el BPMN")
- Implied outputs from the skill being used (read that skill's Output Summary)
- TASK.md if present in the project (always takes precedence)
- Branding or format constraints mentioned anywhere in the conversation

### Step 2 — Show the checklist

Present this block BEFORE executing anything:

```
CHECKLIST DE ENTREGABLES — [Task name]
Fuente: [user request / TASK.md / skill contract]

□ 1. [Entregable] | Formato: [x] | Branding: [x si aplica]
□ 2. [Entregable] | Formato: [x] | Branding: [x si aplica]
□ 3. [Entregable] | Formato: [x] | Branding: [x si aplica]

Restricciones adicionales:
- [branding, idioma, destinatario, path de guardado]

¿Confirmas este checklist antes de ejecutar?
```

### Step 3 — Wait for confirmation

Do not execute until the user confirms or amends the checklist.
If the user adds or removes items, update the checklist and show it again.
Only then proceed.

---

## POST-EXECUTION Protocol

### Step 1 — Reconstruct the checklist

Use the confirmed PRE checklist, or if PRE was skipped,
reconstruct from the task request and any TASK.md present.

### Step 2 — Verify each item

For each deliverable, check:
- Was a file produced? (check /mnt/user-data/outputs/ or stated path)
- Does the format match what was specified?
- Was branding applied if required?
- Was the human checkpoint respected (no irreversible action without approval)?

### Step 3 — Report

```
VERIFICACION DE ENTREGABLES — [Task name]

✅ 1. [Entregable] — Producido | [path]
✅ 2. [Entregable] — Producido | [path]
❌ 3. [Entregable] — OMITIDO
⚠️  4. [Entregable] — Producido pero sin branding correcto

RESULTADO: [N]/[Total] entregables completos
```

If any item is ❌ or ⚠️: fix it immediately before presenting outputs to user.
Never present incomplete work. Never ask the user to accept a partial delivery.

---

## TASK.md — Contract File

When the skill-creator approves an output, or when a user confirms a PRE
checklist as the canonical standard, generate a TASK.md contract.

This file becomes the ground truth for all future runs of this skill.
It lives in the skill's folder or the Cowork project folder.

### TASK.md template

```markdown
# Task Contract: [skill-name or process-name]
Aprobado: [YYYY-MM-DD]
Aprobado por: [user name]
Version: 1.0

## Entregables obligatorios
| # | Entregable | Formato | Path | Branding |
|---|-----------|---------|------|----------|
| 1 | [nombre] | [.pptx/.xlsx/.docx/.bpmn/.md] | [/path/] | [si/no] |
| 2 | [nombre] | [formato] | [path] | [si/no] |

## Restricciones
- Idioma: [español/inglés]
- Branding: [nombre del archivo brand-X.md si aplica]
- Naming convention: [YYYY-MM-DD_nombre_v1.0]
- Human checkpoints: [acción que requiere aprobación]

## Orden de ejecución
1. [paso]
2. [paso]

## Criterio de completitud
La tarea NO está completa hasta que todos los entregables
de la tabla anterior existan en su path con el formato correcto.

## Golden output reference
/golden-output/ — ejemplo aprobado del primer run exitoso
```

### When to show TASK.md before saving

Always show the generated TASK.md to the user before writing it.
Say: "Este es el contrato de entregables generado del output aprobado.
¿Lo confirmas como norma para futuras ejecuciones?"

Only write the file after explicit confirmation.

---

## Integration with skill-creator

When skill-creator reaches the packaging step AND the user has approved
an output during evaluation:

1. Run POST-EXECUTION verification on the approved output
2. Generate TASK.md from the verified deliverables list
3. Show TASK.md to user for confirmation
4. On confirmation: write TASK.md to the skill folder
5. Copy approved output to golden-output/ inside the skill folder
6. Include both in the .skill package

The TASK.md and golden-output/ are then available to any future run
of requirements-checker within that skill's context.

---

## Design principles

1. **PRE always before POST.** Never execute a multi-deliverable task
   without showing the checklist first. This is the human-in-the-loop
   for task definition, not just for task output.

2. **TASK.md beats conversation.** If a TASK.md exists, it overrides
   what the user said in the current message. Show any conflict explicitly.

3. **No partial delivery.** If POST finds gaps, fix them silently and
   re-verify before presenting. Do not tell the user "I missed X, here it is"
   as a separate message — just include it in the complete delivery.

4. **Branding is a deliverable attribute, not a bonus.** If a brand
   context file exists and a deliverable has no branding applied,
   that deliverable is ⚠️ incomplete, not ✅ complete.

5. **The golden output is not a template.** It is a reference for
   structure, format, and completeness. It does not constrain content.
