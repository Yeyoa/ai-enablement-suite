# PPT Adoption Deck — Spec & pptxgenjs Template
# Branding: Silanes (default) | #CC2222 red · #1B3A6B navy · #FFFFFF white

## Overview

7-slide deck. One action per slide. Written for a non-technical colleague.
All slides use the same header/footer pattern. Content varies per slide.

---

## Branding

```javascript
const C = {
  red:       "CC2222",   // Silanes rojo — headers, accents, CTAs
  navy:      "1B3A6B",   // Silanes navy — backgrounds, table headers
  white:     "FFFFFF",
  offwhite:  "F4F7FB",   // slide background
  navyLight: "EBF0FA",   // alternating row / info box fill
  redLight:  "FDECEA",   // warning / human checkpoint fill
  gray:      "64748B",   // captions, secondary text
  lgray:     "E2E8F0",   // borders, dividers
  gold:      "F59E0B",   // human checkpoint badges
  teal:      "0D9488",   // automated step badges
  green:     "16A34A",   // success / complete badges
  textDark:  "1A1A2E",   // body text
};
```

If a different brand context file is active, replace C.red and C.navy
with the values from that file. All other colors remain.

---

## Shared Components

### Header function
```javascript
function header(slide, title, subtitle) {
  slide.addShape(pres.shapes.RECTANGLE, {
    x: 0, y: 0, w: 13.3, h: 1.05,
    fill: { color: C.navy }, line: { type: "none" }
  });
  slide.addShape(pres.shapes.RECTANGLE, {
    x: 0, y: 0, w: 0.18, h: 1.05,
    fill: { color: C.red }, line: { type: "none" }
  });
  slide.addText(title, {
    x: 0.35, y: 0.1, w: 10.5, h: 0.52,
    fontSize: 20, bold: true, color: C.white, fontFace: "Calibri", margin: 0
  });
  if (subtitle) {
    slide.addText(subtitle, {
      x: 0.35, y: 0.62, w: 11, h: 0.3,
      fontSize: 10, color: "CADCFC", fontFace: "Calibri", italic: true, margin: 0
    });
  }
  slide.addText("LABORATORIOS SILANES", {
    x: 10.5, y: 0.28, w: 2.6, h: 0.35,
    fontSize: 8, bold: true, color: C.red, align: "right", fontFace: "Calibri", margin: 0
  });
}
```

### Footer function
```javascript
function footer(slide, pageNum, total) {
  slide.addShape(pres.shapes.RECTANGLE, {
    x: 0, y: 7.2, w: 13.3, h: 0.3,
    fill: { color: C.offwhite }, line: { type: "none" }
  });
  slide.addText("Guía de Adopción · Claude Cowork · [skill-name]", {
    x: 0.3, y: 7.22, w: 10, h: 0.22,
    fontSize: 7.5, color: C.gray, fontFace: "Calibri", margin: 0
  });
  slide.addText(`${pageNum} / ${total}`, {
    x: 12.7, y: 7.22, w: 0.45, h: 0.22,
    fontSize: 7.5, color: C.gray, align: "right", fontFace: "Calibri", margin: 0
  });
}
```

---

## Slide Specifications

### Slide 1 — Cover (Portada)

Dark full-bleed. Same pattern as Silanes cover slides.

```
Background: C.navy full slide
Left accent stripe: C.red 0.35" wide, full height
Decorative circle: top-right, large, semi-transparent navy-lighter

Content:
- Supra-label (small caps, C.red tint): "GUÍA DE ADOPCIÓN · CLAUDE COWORK"
- Title (large, white, Cambria): [Skill name]
- Subtitle (italic, light blue): [One-line purpose of the skill]
- Divider line: C.red, thin
- Meta line: "Skill construido por: [name]  |  Fecha: [date]  |  Version: 1.0"
- Phase label: "Paso 4 de 4: Adopción"

KPI boxes (bottom row, 3 boxes):
- Box 1: "[N] pasos para activar"
- Box 2: "[trigger phrase]" (the phrase they type)
- Box 3: "Schedule: [Sí/No]"
```

### Slide 2 — ¿Qué hace este skill?

Light background. Input → Output visual flow.

```
Header: "¿Qué hace este skill?" | subtitle: "Propósito · Trigger · Resultado"

Left column (navy card, 5.5" wide):
  Title: "Para qué sirve"
  Body: [1-paragraph plain language description from SKILL.md]
  Tag: trigger phrase in a red rounded badge

Right column (two stacked cards):
  Top card (teal): "Tú proporcionas (Input)"
    - [list of inputs the colleague provides]
  Bottom card (green): "Claude entrega (Output)"
    - [list of outputs from TASK.md]

Center: arrow connecting left to right cards
```

### Slide 3 — Antes de empezar

Checklist format. What must be set up before the first run.

```
Header: "Antes de empezar" | subtitle: "Requisitos de configuración en Cowork"

4 requirement cards in a 2x2 grid:

Card 1 — Plan requerido (navy):
  "Plan Pro ($20/mes) o superior"
  "Descarga Claude Desktop desde claude.ai"

Card 2 — Folder Permissions (teal):
  "Settings > Cowork > Folder Permissions"
  "Comparte: ~/Claude-Workspace/[project]/"

Card 3 — Connectors (gold):
  "[List connectors the skill needs]"
  "Settings > Cowork > Connectors → Autenticar"

Card 4 — Context Files (navy):
  "[List .md files to create]"
  "Coloca en /[project]/context/"

Footer note (small, gray):
"Si alguno de estos pasos ya está completo, puedes saltarlo."
```

### Slide 4 — Paso a paso: cómo activarlo

Numbered steps. One action per row.

```
Header: "Cómo activar el skill" | subtitle: "Sigue estos pasos en orden"

Step rows (one per action, alternating white/navyLight background):

| # | Acción | Dónde | Resultado esperado |
|---|--------|-------|-------------------|
| 1 | Abre Claude Desktop y ve a la pestaña Cowork | App | Ves el área de Cowork |
| 2 | Selecciona el proyecto [nombre] | Sidebar izquierdo | Proyecto activo |
| 3 | Escribe: "[trigger phrase]" | Campo de texto | Claude muestra plan |
| 4 | Revisa el plan y confirma | Pantalla de plan | Claude ejecuta |
| 5 | [Human checkpoint — ver Slide 5] | — | — |
| 6 | Encuentra el output en /outputs/ | Carpeta del proyecto | Archivo listo |

Step numbers in colored circles (C.navy for normal, C.gold for human checkpoint)
Row with human checkpoint highlighted in redLight fill.
```

### Slide 5 — Human Checkpoints

What requires human approval and why.

```
Header: "Tus puntos de aprobación" | subtitle: "Claude se detiene aquí y espera tu decisión"

For each human checkpoint in TASK.md, one card:

Card layout:
  Top strip: C.gold
  Icon: 👤
  Title: "Checkpoint [N]: [nombre del paso]"
  Body: "Claude te mostrará [qué te muestra]. Aquí puedes:
         ✓ Confirmar para continuar
         ✗ Cancelar y editar
         ✎ Pedir cambios antes de continuar"
  Why box (small, light): "Por qué existe este checkpoint: [razón]"

Bottom callout (full width, navy):
"Estos pasos NUNCA se ejecutan automáticamente.
Claude siempre espera tu aprobación antes de [acción irreversible]."
```

### Slide 6 — Programarlo en Schedule

How to automate with /schedule.

```
Header: "Ejecutarlo automáticamente" | subtitle: "Configura /schedule para que corra solo"

Left column — Cómo configurarlo (step list):
  1. Escribe /schedule en el campo de Cowork
  2. Nombre: "[suggested schedule name]"
  3. Frecuencia: [recommended: daily/weekly/custom]
  4. Hora: [recommended time]
  5. Prompt: "[exact prompt to use — copy-paste ready]"

Right column — Limitación importante (red warning card):
  Title: "⚠️ Requisito para que funcione"
  Body: "Tu computadora debe estar ENCENDIDA y
         Claude Desktop debe estar ABIERTO
         en el momento programado.
         Si la computadora está apagada o
         en reposo, la tarea se omite.
         Las tareas omitidas NO se recuperan."

Bottom tip (teal bar):
"Tip: Usa una computadora de escritorio para schedules críticos.
Configura tu OS para no entrar en reposo durante el horario programado."
```

### Slide 7 — Qué hacer si algo falla

Troubleshooting. Keep it simple and actionable.

```
Header: "¿Algo no funcionó?" | subtitle: "Guía de solución rápida"

Table (3 columns: Síntoma | Causa probable | Acción):

| Síntoma | Causa probable | Acción |
|---------|---------------|--------|
| El skill no se activa | Frase de trigger incorrecta | Usa exactamente: "[trigger phrase]" |
| Connector no conecta | Sesión expirada | Settings > Connectors > Re-autenticar |
| Output guardado en lugar incorrecto | Falta regla en Global Instructions | Agrega: "Guardar en /[path]/" |
| Schedule no corrió | Computadora apagada o Cowork cerrado | Verifica que la máquina esté encendida |
| Claude omitió un entregable | Sin TASK.md | Escribe: "verifica entregables" |
| Output sin branding | brand context file faltante | Agrega brand-[name].md a /context/ |

Bottom contact box (navy):
"¿No encuentras la solución? Contacta a:
[Nombre del AI Enablement Lead]  |  [email]"
```

---

## Output

Save as: `/mnt/user-data/outputs/[skill-name]_adoption_guide.pptx`
Present with present_files after POST-EXECUTION verification.
