# Client Onboarding Automation — n8n

A fully automated client onboarding system built for professional services firms — accounting, legal, and consulting businesses. Zero manual work after the client submits the intake form.

---

## The Problem

Most service businesses onboard clients manually. Someone has to create the CRM record, send the welcome email, build the task list, notify the team, and remember to follow up if documents don't arrive. That process takes 2–3 hours per client and things fall through the gaps when the team is busy.

---

## The Solution

When a new client submits the intake form, this workflow handles everything automatically in under 30 seconds.

---

## What Happens Automatically

```
Client submits Tally form
        ↓
Client record created in Airtable CRM
        ↓
Personalised welcome email sent via Gmail
        ↓
Onboarding task list created in ClickUp
        ↓
Team notified on Slack with client summary
        ↓
Wait 48 hours
        ↓
Check if documents received
        ↓
YES → Workflow ends, no action needed
NO  → Reminder email sent to client automatically
```

---

## Results

- Onboarding admin reduced from **2.5 hours to under 2 minutes**
- Zero manual steps after form submission
- No leads or documents fall through the gaps
- Team always notified instantly with full client context
- Follow-up reminders fire automatically — nothing depends on someone remembering

---

## Tools and Stack

| Tool | Purpose |
|---|---|
| n8n | Workflow automation engine |
| Tally | Client intake form |
| Airtable | CRM — client records and status tracking |
| Gmail | Welcome email and follow-up reminders |
| ClickUp | Onboarding task list per client |
| Slack | Team notification on new client |

---

## Workflow Nodes

1. **Webhook Trigger** — Receives form submission from Tally
2. **Set Node** — Maps and cleans form fields into variables
3. **Airtable Node** — Creates client record with status: Awaiting Documents
4. **Gmail Node** — Sends personalised welcome email
5. **ClickUp Node (x5)** — Creates 5 onboarding tasks with due dates and assignees
6. **Slack Node** — Posts client summary card to #new-clients channel
7. **Wait Node** — Pauses workflow for 48 hours
8. **Airtable Get Node** — Fetches current document status for this client
9. **IF Node** — Checks if documents have been received
10. **Gmail Node** — Sends reminder email if documents not yet received
11. **Error Handler** — Logs failures to Google Sheet and sends Slack alert

---

## How to Import and Use

**Step 1 — Import the workflow**
- Open your n8n instance
- Go to Workflows → Import from file
- Upload `client_onboarding_workflow.json`

**Step 2 — Connect your credentials**
- Airtable API key
- Gmail OAuth
- ClickUp API key
- Slack OAuth

**Step 3 — Update these values**
- Replace `YOUR_LIST_ID` in ClickUp nodes with your actual List ID
- Replace `YOUR_AIRTABLE_BASE_ID` with your Airtable base ID
- Replace `#new-clients` with your Slack channel name

**Step 4 — Set up your Tally form**
- Create the intake form at tally.so
- Go to Integrations → add your n8n webhook URL

**Step 5 — Test**
- Submit a test form entry
- Confirm Airtable record created
- Confirm welcome email received
- Confirm ClickUp tasks created
- Confirm Slack notification posted
- Manually trigger the IF node — test both paths

---

## Airtable CRM Structure

The workflow writes to a `Clients` table with these fields:

| Field | Type |
|---|---|
| Client Name | Text |
| Email | Email |
| Phone | Phone |
| Business Name | Text |
| Service | Single select |
| Status | Single select |
| Documents Received | Checkbox |
| Date Onboarded | Date |
| Assigned To | Text |
| Notes | Long text |

---

## Error Handling

Every node is covered by an error workflow that:
- Logs the failure to a Google Sheet with timestamp, node name, and error message
- Sends a Slack DM alert with what broke and which client was affected
- Ensures no failure goes unnoticed

---

## Customisation

This workflow is designed to be adapted. Common modifications:

- **Swap Airtable for HubSpot** — replace the Airtable nodes with HubSpot CRM nodes
- **Swap ClickUp for Notion** — replace task creation nodes with Notion database entries
- **Add document upload link** — include a file upload URL in the welcome email
- **Add e-signature step** — trigger a DocuSign or PandaDoc request after the welcome email
- **Extend follow-up sequence** — add a second reminder at 72 hours via the Wait node

---

## About This Project

Built as part of a professional automation portfolio targeting professional services firms. Designed to show end-to-end workflow architecture — trigger, data mapping, multi-tool integration, conditional logic, and error handling.

**Tools demonstrated:** n8n · Airtable · Gmail · ClickUp · Slack · Tally · Webhook · IF logic · Wait node · Error workflows
