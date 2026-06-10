# Cowork Connectors Reference

Source: sidbharath.com/blog/ultimate-guide-claude-cowork/ (April 2026) + Anthropic documentation.

## Available Connectors (38+ as of April 2026)

### Productivity
| Connector | Key capabilities |
|---|---|
| Microsoft 365 | Read/write Word, Excel, PowerPoint, OneDrive files |
| Google Drive | Read/write Docs, Sheets, Slides, Drive files |
| Notion | Read/write pages, databases, blocks |

### CRM
| Connector | Key capabilities |
|---|---|
| HubSpot | Contacts, deals, pipeline, emails, properties |
| Salesforce | Accounts, opportunities, leads, cases |
| Close | Leads, calls, emails, pipeline |

### Project Management
| Connector | Key capabilities |
|---|---|
| Jira | Issues, sprints, projects, comments |
| Asana | Tasks, projects, teams |
| Linear | Issues, cycles, roadmap |

### Communication
| Connector | Key capabilities |
|---|---|
| Slack | Read channels, search messages, post (with approval) |
| Gmail | Read, search, draft, send (with approval) |
| Zoom | Transcripts, recordings, meeting data |

### Data
| Connector | Key capabilities |
|---|---|
| Snowflake | Query, read tables |
| BigQuery | Query, read tables |
| Databricks | Query, read notebooks |

### Other
| Connector | Key capabilities |
|---|---|
| DocuSign | Send, track, retrieve signed envelopes |
| Apollo | Prospect search, contact enrichment |
| Clay | Data enrichment, sequencing |
| WordPress | Read/write posts and pages |

## Pharma-Specific Gap Analysis
The following systems commonly used in pharma/regulated industries are NOT
currently in the Cowork connector list. Use manual file export workaround:

| System | Workaround |
|---|---|
| SAP (ERP, QM, MM modules) | Export to Excel/CSV → skill reads file |
| Veeva Vault | Export documents → skill reads PDF/Word |
| MES systems (manufacturing) | Export to CSV → skill reads file |
| LIMS (lab information) | Export to Excel → skill reads file |
| TrackWise / Pilgrim | Export to Excel → skill reads file |
| SuccessFactors | Export to CSV → skill reads file |

## Manual Input Pattern (for unavailable connectors)
When a required system has no connector, design the skill to accept:
- An Excel/CSV export as input
- Document the exact export steps in the skill's context file
- Mark connector status as `[MANUAL INPUT — connector TBD]`
- Flag in the Unknowns log for future connector development

## Data Routing Note
All connector data routes through Anthropic's cloud infrastructure.
In regulated environments (GxP, HIPAA, financial compliance), verify
data classification before activating connectors for sensitive records.
For GxP systems: connector use for non-GxP data only unless validated.
