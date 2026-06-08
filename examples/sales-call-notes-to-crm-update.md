# Example: Sales Call Notes To CRM Update

## User Prompt

```text
Use build-ai-workflow to design this workflow:

After each sales call, summarize the call notes and update the CRM opportunity.
If there are next steps, create follow-up tasks for the account owner.
Do not overwrite important CRM fields accidentally.
```

## Workflow Check Spec

Purpose:
Update CRM opportunities from sales call notes and create follow-up tasks when required.

Start condition:
Before processing the call notes, confirm the downstream side-effect tools are available: CRM update and task creation. Source note access can be validated by reading the note. If the CRM opportunity cannot be identified confidently, stop and ask for clarification.

Source scope:
Use only the call notes from the completed call and the matching CRM opportunity. Do not scan unrelated opportunities unless the source opportunity is missing or ambiguous.

Action decision:
For every proposed CRM update, decide whether to update, skip, or flag for review. Do not overwrite existing fields unless the workflow defines that field as safe to update. Create follow-up tasks only when the notes contain a clear owner, action, and due date or due-date rule.

Stable marker:
Use the call ID plus CRM opportunity ID. For follow-up tasks, include the call ID and action text normalized for casing and whitespace.

Create/update rules:
CRM updates must include call summary, customer objections, next step, owner, and timestamp. Follow-up tasks must include the source call link, account, opportunity, owner, due date, and specific action.

Write-back:
Record which CRM fields were updated and which task IDs were created. If task creation succeeds but CRM write-back fails, report the partial state.

End-state report:
Report CRM fields updated, tasks created, skipped updates, and fields requiring human review.

Open questions:
Which CRM fields are safe for the agent to update directly?
What due-date rule should apply when the call notes do not include a date?
