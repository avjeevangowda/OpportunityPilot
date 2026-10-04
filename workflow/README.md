# Workflows

| File | What it does |
|------|--------------|
| `opportunitypilot.json` | Main workflow: Gmail trigger, AI extraction, filter, match score, Sheets, Calendar, draft email, Gmail draft with resume, Telegram alert |
| `digest.json` | Daily 8 AM digest of deadlines due in the next 7 days |

## Export (how these files were made)
In n8n: open the workflow, then **⋯ > Export JSON**.

## Import
In n8n: **⋯ > Import > From file**, choose the JSON, then reconnect your own credentials in each node (Gmail, Gemini, Google Sheets, Google Calendar, Google Drive, Telegram).

Credentials are not included in the exported files.
