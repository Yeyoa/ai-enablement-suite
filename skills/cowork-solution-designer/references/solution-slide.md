# Solution Slide Reference

## Purpose
Produce a 4-slide PowerPoint deck summarizing the Cowork solution architecture.
Designed for a sponsor, evaluator, or committee audience.
All visuals are in PowerPoint — no HTML artifacts.

---

## Deck Structure

### Slide 1 — Architecture Overview
**Title:** "[Process Name] — AI Solution in Cowork"
**Subtitle:** "Step 3 of 4: Solution Design | [Date]"

Three-column body layout:
- Col 1 (amber): Global Instructions — bulleted rules (max 5)
- Col 2 (blue): Project — context files list + connectors list
- Col 3 (green): Skills — one line per skill (name + trigger phrase)

Footer bar (dark): Three KPI boxes:
- "[N] Skills designed"
- "~[N] min/week recoverable"
- "[N] Connectors needed"

---

### Slide 2 — Skill Cards
**Title:** "Skills — What Colleagues Will Trigger"

One card per skill. Card layout:
- Skill name (bold, large)
- Trigger phrase (italic, quoted)
- Input → Output (one line, arrow between)
- Complexity badge (Low/green, Medium/amber, High/red)
- Time saved badge (blue)
- Human checkpoint note (small, orange)

Arrange cards in a grid: 2 columns, up to 3 rows per slide.
If more than 6 skills, add a Slide 2b.

---

### Slide 3 — Artifact Map
**Title:** "What Each Skill Produces"

Table with columns:
| Skill | Input | Output Format | Saved To | Rationale |

One row per skill.
Highlight [OUTPUT TBD] rows in orange.
Highlight [MANUAL INPUT] connector rows in yellow.

Footer note: "Output formats inferred from process type and downstream consumer."

---

### Slide 4 — Unknowns & Next Steps
**Title:** "Open Items & Handoff"

Left column — Unknowns (red accent):
- [UNKNOWN SYSTEM] items
- [OUTPUT TBD] items
- High-complexity skills needing scoping
- [MANUAL INPUT] connectors to resolve

Right column — Next Step (green accent):
- "Step 4: Adoption Playbook"
- Colleagues who will operate this: [list]
- Skills requiring training: [list]
- Estimated onboarding effort: [Low / Medium / High]

---

## pptxgenjs Implementation Notes

```javascript
const pptx = new PptxGenJS();
pptx.layout = 'LAYOUT_WIDE'; // 13.3 x 7.5 inches

// Slide 1 — Architecture Overview
const s1 = pptx.addSlide();
s1.background = { color: 'FFFFFF' };

// Header
s1.addText('[Process Name] — AI Solution in Cowork', {
  x: 0.3, y: 0.15, w: 12.7, h: 0.5,
  fontSize: 24, bold: true, color: '1E293B'
});
s1.addText('Step 3 of 4: Solution Design  |  [Date]', {
  x: 0.3, y: 0.65, w: 12.7, h: 0.3,
  fontSize: 13, color: '64748B', italic: true
});

// Three columns (y: 1.1, h: 5.0)
// Col 1 — Global Instructions (amber)
s1.addShape(pptx.ShapeType.rect, {
  x: 0.2, y: 1.1, w: 3.9, h: 5.0,
  fill: { color: 'FFF8E1' }, line: { color: 'F59E0B', width: 1 }
});
s1.addText('⚙️ Global Instructions', {
  x: 0.35, y: 1.2, w: 3.6, h: 0.4,
  fontSize: 12, bold: true, color: '92400E'
});
// Add rules as bullet text

// Col 2 — Project (blue)
s1.addShape(pptx.ShapeType.rect, {
  x: 4.4, y: 1.1, w: 3.9, h: 5.0,
  fill: { color: 'EFF6FF' }, line: { color: '3B82F6', width: 1 }
});
s1.addText('📁 Project: [Name]', {
  x: 4.55, y: 1.2, w: 3.6, h: 0.4,
  fontSize: 12, bold: true, color: '1E40AF'
});
// Context files section, then connectors section

// Col 3 — Skills (green)
s1.addShape(pptx.ShapeType.rect, {
  x: 8.6, y: 1.1, w: 4.5, h: 5.0,
  fill: { color: 'F0FDF4' }, line: { color: '22C55E', width: 1 }
});
s1.addText('🧩 Skills ([N] total)', {
  x: 8.75, y: 1.2, w: 4.2, h: 0.4,
  fontSize: 12, bold: true, color: '166534'
});
// One line per skill: name + trigger phrase

// Footer KPI bar
s1.addShape(pptx.ShapeType.rect, {
  x: 0.2, y: 6.3, w: 12.9, h: 0.9,
  fill: { color: '1E293B' }
});
// Three KPI text blocks inside footer

// --- Slide 2 — Skill Cards ---
// --- Slide 3 — Artifact Map ---
// --- Slide 4 — Unknowns & Next Steps ---
// (follow same pattern — header, body, footer)

pptx.writeFile({ fileName: '[process-name]_solution.pptx' });
```

## Save Location
`/mnt/user-data/outputs/[process-name]_solution.pptx`

## Slide Design Rules
- Max 5 bullets per column on Slide 1
- Trigger phrases always italic and quoted
- Complexity badges: Low (green #16A34A), Medium (amber #D97706), High (red #DC2626)
- [OUTPUT TBD] cells: orange fill (#FED7AA)
- [MANUAL INPUT] cells: yellow fill (#FEF9C3)
- [UNKNOWN SYSTEM] cells: red fill (#FEE2E2)
- Footer KPIs must be real numbers, never placeholders
