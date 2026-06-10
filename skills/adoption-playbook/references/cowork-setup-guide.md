# Cowork Setup Guide
# Source: sidbharath.com/blog/ultimate-guide-claude-cowork/ (April 2026)

## Prerequisites

| Requirement | Detail |
|---|---|
| App | Claude Desktop (macOS or Windows) — download from claude.ai |
| Plan | Pro ($20/mo), Max ($100-200/mo), Team, or Enterprise |
| Tab | Open Claude Desktop → click "Cowork" tab at the top |

---

## Step 1 — Folder permissions

Cowork reads and writes only in folders you explicitly share.

**Path:** Settings > Cowork > Folder Permissions

Recommended setup:
- Share: `~/Claude-Workspace/` (create this folder first)
- Share: Documents folder
- Do NOT share: home directory root, .ssh, any folder with credentials

Best practice: create `/Claude-Workspace/[project-name]/` per project with:
```
/inputs/     ← files the colleague drops here before running the skill
/outputs/    ← where Claude saves its deliverables
/templates/  ← .pptx, .xlsx, .docx templates Claude uses
/archive/    ← processed inputs and versioned outputs
/context/    ← .md context files Claude loads each session
```

---

## Step 2 — Global Instructions

Rules that apply to every session, every skill, every colleague.
Keep them short and behavioral.

**Path:** Settings > Cowork > Edit Global Instructions

Example rules:
```
Always ask for language (Spanish or English) before generating output.
Never send emails or messages without explicit approval.
Save all outputs to /Claude-Workspace/[project]/outputs/
Use naming convention: YYYY-MM-DD_[descriptor]_v1.pptx
Human approval required before: sending, uploading, deleting.
```

---

## Step 3 — Create a Project

Projects are isolated workspaces. One project per process or domain.

**Path:** Cowork sidebar → click "+" to create new project

Each project gets:
- Its own context files (loaded every session within that project)
- Its own connector activations
- Its own conversation history
- Its own scheduled tasks

Context files live in the project folder and supplement Global Instructions.
Create these as .md files and place them in the project's /context/ folder:

| File | Content |
|---|---|
| `about-process.md` | What this process does, who owns it, key dates |
| `team-roles.md` | Who does what, approver names, contact info |
| `brand-[name].md` | Colors, fonts, logo placement, naming conventions |
| `output-templates.md` | Paths to .pptx / .xlsx templates to use |

---

## Step 4 — Install a Skill

Skills are .skill files built with the skill-creator.

**Path:** Settings > Customize > Browse Plugins (for Anthropic skills)
For custom .skill files: Settings > Customize > Import Skill → select .skill file

After installing, the skill appears in the available_skills list.
Claude reads the skill description and decides when to invoke it automatically.
You can also trigger it manually by using the phrases in the skill description.

---

## Step 5 — Activate Connectors

Connectors give Claude access to external tools.

**Path:** Settings > Cowork > Connectors → find service → Authenticate (OAuth)

Key connectors for common workflows:

| Connector | What Claude can do |
|---|---|
| Gmail | Read, search, draft, send (with approval) |
| Google Drive | Read/write Docs, Sheets, Slides, Drive files |
| Microsoft 365 | Read/write Word, Excel, PowerPoint, OneDrive |
| Slack | Read channels, search messages, post (with approval) |
| HubSpot | Contacts, deals, pipeline |
| Notion | Read/write pages and databases |

**Important:** Connector data routes through Anthropic cloud.
In regulated environments (GxP, HIPAA), verify data classification
before activating connectors for sensitive records.

Systems without a Cowork connector (SAP, Veeva, LIMS, legacy ERPs):
design the skill to accept a manual file export as input instead.

---

## Step 6 — Run a Skill Manually

Open the project → type the trigger phrase in the Cowork input.

Example: "Genera el deck de esta semana del Proyecto Nahual"

Claude will:
1. Show an execution plan before doing anything
2. Ask for approval
3. Execute the skill steps
4. Show human checkpoints when they are reached (approval required)
5. Save outputs to the configured folder
6. Confirm completion

---

## Step 7 — Set Up a Schedule

Schedules run a skill automatically at a set frequency.

**How to create:** Type `/schedule` in Cowork. You will be prompted for:
- Name: descriptive label (e.g., "Actualización Semanal Nahual")
- Frequency: daily, weekly, or custom cron expression
- Time: when it should run (e.g., every Monday 8:00 AM)
- Task description: the exact prompt Claude will execute

**Example schedule prompt:**
```
Genera el deck de actualización semanal del Proyecto Nahual.
Usa el template en /Nahual/Semanal/templates/lss-weekly-template.pptx
Guarda el output en /Nahual/Semanal/outputs/
Deja el deck listo para mi revisión. No envíes nada sin mi aprobación.
```

**Critical limitation:** Scheduled tasks require your computer to be
awake with Claude Desktop running. If the machine is asleep or Cowork
is closed when the task fires, it is skipped. Missed runs do not queue.

To keep schedules reliable:
- Run schedules on a desktop machine, not a laptop set to sleep
- Set your OS to prevent sleep during scheduled hours
- Check the schedule log in Cowork sidebar after the task fires

---

## Step 8 — Dispatch (Control from Phone)

Dispatch lets you trigger Cowork tasks from the Claude mobile app.

**Setup:** Open Cowork on desktop → click Dispatch → scan QR code with Claude app

Use cases:
- Fire a task while away from desk
- Check on running tasks
- Approve a human checkpoint from your phone

Requires: computer awake, Claude Desktop open, Pro or Max plan.

---

## Troubleshooting Quick Reference

| Problem | First action |
|---|---|
| Skill not triggering | Check the skill description trigger phrases; rephrase your request |
| Connector not connecting | Re-authenticate in Settings > Connectors |
| Output saved to wrong folder | Check Global Instructions save path rule |
| Schedule didn't fire | Verify computer was awake; check schedule log |
| Claude skipped a step | Run requirements-checker: "verifica entregables" |
| Output has wrong branding | Check brand context file exists in project /context/ folder |
| Need to cancel a running task | Press Stop in the Cowork execution panel |
