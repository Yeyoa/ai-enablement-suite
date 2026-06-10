# Visual Input — Processing Uploaded Images and Process Sketches

Guide for extracting structured process information from images, photos,
whiteboard shots, and hand-drawn diagrams. Use this reference whenever
the user uploads a visual instead of describing a process in text.

---

## When this reference applies

- User uploads a photo of a whiteboard with a process drawn on it
- User uploads a hand-drawn flowchart or sketch ("dibujito")
- User uploads a screenshot of a diagram from Visio, PowerPoint, or any other tool
- User uploads a printed process document that includes a flow diagram
- User says "here's what our process looks like" and attaches an image

---

## Step 1 — Describe what you see

Before extracting anything, describe the image content explicitly:

- How many steps or shapes are visible
- Whether there are labels, arrows, decision points
- The general flow direction (left-right, top-down, circular)
- How legible the text is (clear / partial / illegible)
- Whether swim lanes or actor separations are visible

Do not skip this step. It sets expectations with the user
and surfaces interpretation gaps before they compound.

---

## Step 2 — Transcription rules

Extract process elements in this order:

### 2a. Steps and actions
- Read each labeled box, rectangle, or shape as one step
- Preserve the original label text exactly, then normalize it
- If the label is ambiguous (e.g. "check"), ask the user to clarify what is being checked

### 2b. Actors
- If swim lanes exist: use the lane label as the actor for all steps in that lane
- If no swim lanes: infer the actor from context clues (icons, color coding, text notes)
- If no actor information is visible: flag as [ACTOR UNKNOWN] and ask

### 2c. Decisions
- Every diamond shape or branching arrow is a decision gateway
- Extract the condition text if visible; if not, label as [CONDITION UNKNOWN]
- Identify all possible paths out of the gateway (yes/no, approve/reject, etc.)

### 2d. Systems
- Look for system logos, icons, or labels on or near each step
- Common visual cues: envelope icon = email, cloud = cloud system, cylinder = database
- If no system is marked: flag as [SYSTEM UNKNOWN] — do not assume

### 2e. Times
- Times are rarely shown in hand-drawn diagrams; flag all as [TIME UNKNOWN]
- Ask the user for estimates after transcription is complete

### 2f. Arrows and flow
- Follow the direction of arrows to establish sequence
- If arrows are bidirectional or unclear, flag the ambiguity
- Loops and back-arrows are valid; mark them as such in the Mermaid diagram

---

## Step 3 — Handle ambiguity

Build an explicit list of questions before asking the user.
Do not ask one question at a time — batch all gaps into a single message.

Format:
```
Based on the image, I could extract the following:
[brief summary of what was readable]

I need to confirm a few things before producing the full output:
1. Step 3: the label says "validate" — validate what, and who does it?
2. Step 5: no system is marked — what tool does the actor use here?
3. The arrow between Step 4 and Step 6 bypasses Step 5 — is that intentional?
4. I count 3 actors but lane labels are partially cut off — can you confirm the names?
```

---

## Step 4 — What to do when the image is unclear

If the image is illegible or too partial to extract reliably:

1. Describe what IS interpretable, however partial
2. List specific doubts (not generic "the image is unclear")
3. Ask the user to supplement with text for the unreadable parts

Do not invent steps or assume logic that is not visible.
An honest incomplete transcription is better than a plausible but incorrect one.

---

## Step 5 — Produce the structured output

Once gaps are resolved, generate the standard Level 3 output:

1. **Process sheet** — with all fields populated, [UNKNOWN] replaced by confirmed values,
   and task type classification (Manual / Rule-based / AI-automatable) per step
2. **Mermaid diagram** — reflecting the visual structure from the image, corrected
   for any ambiguities resolved with the user
3. **BPMN file** — `.bpmn` XML generated from the transcribed and confirmed process
4. **PowerPoint slide** — swim-lane diagram matching the actor structure from the image

If the image already shows a BPMN diagram (from Visio, Bizagi, draw.io, etc.),
note the source tool and preserve the original element types when generating
the `.bpmn` output.

---

## Common visual input patterns and how to handle them

| What you see | How to interpret |
|---|---|
| Sticky notes on a wall | Each note is likely one step; color = actor or category |
| Whiteboard with boxes and arrows | Standard flowchart; transcribe as-is |
| Printed document with a Visio diagram | High fidelity; transcribe directly, infer system from Visio shape styles |
| Hand-drawn sketch on paper | Highest ambiguity; follow steps 1-4 carefully |
| Screenshot of a PowerPoint slide | Medium fidelity; text is usually readable; check for cut-off labels |
| Photo taken at an angle | Perspective distortion; flag any text that is unreadable due to angle |

---

## Quality check before producing output

- [ ] Every step has an actor (or is flagged)
- [ ] Every step has a system (or is flagged)
- [ ] All decision gateways have at least two labeled exit paths
- [ ] No steps were invented — only what was visible or confirmed
- [ ] User has confirmed all [UNKNOWN] fields before final output is generated
