# PPTX — Diagramas de proceso con swim lanes

Guía para generar un slide de PowerPoint con diagrama de proceso tipo swim-lane usando pptxgenjs.
Leer en conjunto con el skill `pptx` (pptxgenjs.md) para instrucciones de setup del entorno.

---

## Cuándo usar este output

- El cliente o equipo no tiene software BPMN instalado
- El entregable es una presentación ejecutiva o de consultoría
- El proceso se presentará ante un comité directivo o sponsor
- Se necesita un visual atractivo y editable en PowerPoint

---

## Estructura del slide de proceso

### Layout recomendado

```
┌─────────────────────────────────────────────────────────────┐
│  NOMBRE DEL PROCESO          Área: X  Fecha: YYYY-MM-DD     │
│  Tiempo total: Xh Xmin  |  Tiempo espera: Xh Xmin           │
├──────────┬──────────────────────────────────────────────────┤
│          │  ┌──────┐    ┌──────┐    ◇──────    ┌──────┐    │
│ Actor 1  │  │Task A│───▶│Task B│───▶           │Task E│    │
│ (Sistema)│  └──────┘    └──────┘    │  Sí      └──────┘    │
├──────────┤                          │                        │
│          │                          ▼ No                     │
│ Actor 2  │                      ┌──────┐                    │
│ (Sistema)│                      │Task C│                    │
│          │                      └──────┘                    │
├──────────┤                                                   │
│ Sistema  │              [Nombre Sistema]                     │
│ / IA     │              🤖 Automatizable                     │
└──────────┴──────────────────────────────────────────────────┘
│  LEYENDA: ▭ User Task  ◇ Decisión  🤖 Automatizable  👤 Human│
└─────────────────────────────────────────────────────────────┘
```

---

## Implementación en pptxgenjs

### Configuración del slide

```javascript
const pptx = new PptxGenJS();
const slide = pptx.addSlide();

// Fondo
slide.background = { color: "FFFFFF" };

// Paleta de colores
const COLORS = {
  headerBg:    "1B3A6B",   // azul oscuro
  headerText:  "FFFFFF",
  laneBg1:     "EBF3FF",   // azul muy claro (lane 1)
  laneBg2:     "F5F5F5",   // gris claro (lane 2)
  laneBg3:     "FFF8E7",   // amarillo muy claro (lane sistema/IA)
  laneLabel:   "1B3A6B",
  taskFill:    "FFFFFF",
  taskBorder:  "1B3A6B",
  taskText:    "1A1A2E",
  systemTask:  "E8F4FD",   // celeste para service tasks
  humanReview: "FFF3CD",   // amarillo para revisión humana
  gatewayFill: "FFF3CD",
  arrowColor:  "555555",
  timeText:    "888888",
};
```

### Header del proceso

```javascript
// Título del proceso
slide.addText("NOMBRE DEL PROCESO", {
  x: 0.3, y: 0.1, w: 9.4, h: 0.35,
  fontSize: 14, bold: true, color: COLORS.headerText,
  fill: { color: COLORS.headerBg },
  align: "left", valign: "middle"
});

// Métricas de tiempo
slide.addText("Área: Operaciones  |  Tiempo total: 2h 30min  |  Tiempo espera: 1h 45min  |  Fecha: 2026-06-10", {
  x: 0.3, y: 0.48, w: 9.4, h: 0.22,
  fontSize: 8, color: "555555", italic: true
});
```

### Swim lanes

```javascript
// Dimensiones base
const LANE_HEIGHT = 1.4;  // pulgadas por lane
const LANE_LABEL_W = 1.2;
const LANE_START_X = 0.3;
const LANE_START_Y = 0.75;
const SLIDE_W = 9.7;

const lanes = [
  { name: "Solicitante", system: "Email / Forms", color: COLORS.laneBg1 },
  { name: "Gerente / Aprobador", system: "SAP / Outlook", color: COLORS.laneBg2 },
  { name: "Sistema / IA", system: "Automatizado", color: COLORS.laneBg3 }
];

lanes.forEach((lane, i) => {
  const y = LANE_START_Y + (i * LANE_HEIGHT);

  // Fondo del lane
  slide.addShape(pptx.ShapeType.rect, {
    x: LANE_START_X, y: y, w: SLIDE_W, h: LANE_HEIGHT,
    fill: { color: lane.color },
    line: { color: "CCCCCC", width: 0.5 }
  });

  // Etiqueta del lane (rotada 90°)
  slide.addText(`${lane.name}\n${lane.system}`, {
    x: LANE_START_X, y: y, w: LANE_LABEL_W, h: LANE_HEIGHT,
    fontSize: 8, bold: true, color: COLORS.laneLabel,
    align: "center", valign: "middle",
    rotate: 0  // No rotar; texto en dos líneas
  });

  // Línea divisoria de etiqueta
  slide.addShape(pptx.ShapeType.line, {
    x: LANE_START_X + LANE_LABEL_W, y: y,
    w: 0, h: LANE_HEIGHT,
    line: { color: "AAAAAA", width: 1 }
  });
});
```

### Tasks (cajas de proceso)

```javascript
// Función para agregar una tarea
function addTask(slide, opts) {
  const { x, y, w = 1.3, h = 0.7, name, system, time, isAutomated = false, isHuman = false } = opts;

  const fillColor = isAutomated ? COLORS.systemTask
                  : isHuman    ? COLORS.humanReview
                  :              COLORS.taskFill;

  // Caja principal
  slide.addShape(pptx.ShapeType.rect, {
    x, y, w, h,
    fill: { color: fillColor },
    line: { color: COLORS.taskBorder, width: 1.5 }
  });

  // Nombre de la tarea
  slide.addText(name, {
    x: x + 0.05, y: y + 0.02, w: w - 0.1, h: h * 0.55,
    fontSize: 7, bold: true, color: COLORS.taskText,
    align: "center", valign: "middle", wrap: true
  });

  // Sistema y tiempo
  if (system || time) {
    slide.addText(`${system || ""}${time ? "  " + time : ""}`, {
      x: x + 0.05, y: y + (h * 0.6), w: w - 0.1, h: h * 0.35,
      fontSize: 6, color: COLORS.timeText, italic: true,
      align: "center", valign: "middle"
    });
  }

  // Ícono si aplica
  if (isAutomated) {
    slide.addText("🤖", { x: x + w - 0.22, y: y + 0.02, w: 0.2, h: 0.2, fontSize: 8 });
  }
  if (isHuman) {
    slide.addText("👤", { x: x + w - 0.22, y: y + 0.02, w: 0.2, h: 0.2, fontSize: 8 });
  }
}
```

### Gateway (decisión)

```javascript
function addGateway(slide, x, y, label) {
  // Rombo
  slide.addShape(pptx.ShapeType.diamond, {
    x, y, w: 0.6, h: 0.6,
    fill: { color: COLORS.gatewayFill },
    line: { color: COLORS.taskBorder, width: 1.5 }
  });

  // Pregunta encima
  slide.addText(label, {
    x: x - 0.3, y: y - 0.28, w: 1.2, h: 0.25,
    fontSize: 6, color: COLORS.taskText,
    align: "center", italic: true
  });
}
```

### Flechas de conexión

```javascript
function addArrow(slide, x1, y1, x2, y2, label = "") {
  slide.addShape(pptx.ShapeType.line, {
    x: x1, y: y1, w: x2 - x1, h: y2 - y1,
    line: { color: COLORS.arrowColor, width: 1.5, endArrowType: "arrow" }
  });

  if (label) {
    slide.addText(label, {
      x: (x1 + x2) / 2 - 0.15, y: Math.min(y1, y2) - 0.18, w: 0.3, h: 0.18,
      fontSize: 6, color: "777777", align: "center"
    });
  }
}
```

### Leyenda

```javascript
slide.addText("LEYENDA:", {
  x: 0.3, y: 6.85, w: 0.8, h: 0.2, fontSize: 7, bold: true, color: "333333"
});

const legend = [
  { color: COLORS.taskFill,    border: COLORS.taskBorder, label: "User Task" },
  { color: COLORS.systemTask,  border: COLORS.taskBorder, label: "🤖 Automatizable" },
  { color: COLORS.humanReview, border: COLORS.taskBorder, label: "👤 Revisión Humana" },
  { color: COLORS.gatewayFill, border: COLORS.taskBorder, label: "◇ Decisión" },
];

legend.forEach((item, i) => {
  const lx = 1.2 + (i * 1.6);
  slide.addShape(pptx.ShapeType.rect, {
    x: lx, y: 6.85, w: 0.22, h: 0.2,
    fill: { color: item.color }, line: { color: item.border, width: 1 }
  });
  slide.addText(item.label, {
    x: lx + 0.25, y: 6.85, w: 1.3, h: 0.2, fontSize: 6.5, color: "444444"
  });
});
```

---

## Checklist antes de generar el slide

- [ ] ¿Tengo todos los pasos con actor, sistema, y tiempo asignado?
- [ ] ¿Identifiqué las decisiones (gateways) y sus condiciones?
- [ ] ¿Marqué los pasos automatizables con 🤖 y los de revisión humana con 👤?
- [ ] ¿El número de lanes coincide con los actores del proceso?
- [ ] ¿Las métricas de tiempo total son la suma de ejecución + espera?

---

## Output esperado

- Un archivo `.pptx` con un slide de swim-lane diagram
- El slide incluye: header con nombre y métricas de tiempo, lanes por actor, tasks con sistema y tiempo, gateways, leyenda
- Guardado en `/mnt/user-data/outputs/[nombre_proceso]_diagrama.pptx`
- Presentado con `present_files`
