# Example: Error Logs To Incident Ticket

## User Prompt

```text
Use build-ai-workflow to design this workflow:

Scan production error logs every hour.
Create an incident ticket when a new error pattern crosses a threshold.
Avoid duplicate tickets for the same incident.
```

## Workflow Check Spec

Purpose:
Create incident tickets for new production error patterns that cross the alert threshold.

Start condition:
Before scanning logs, confirm the downstream incident search and ticket creation tools are available. Log access can be validated by the first targeted fetch. If incident search or creation is unavailable, abort before scanning logs.

Source scope:
Fetch production errors from the last hour. Group errors by service, normalized error signature, environment, and time window. Exclude non-production logs.

Action decision:
For each error group, check whether it crosses the threshold. Before creating a ticket, search open incidents for the same normalized signature, service, and environment. If a likely match exists, update or report the existing ticket instead of creating a new one.

Stable marker:
Use service name, environment, normalized error signature, and first-seen timestamp bucket in UTC. Do not rely only on raw log message text because variable IDs and timestamps can make the same error look different.

Create/update rules:
Each incident ticket must include service, environment, error signature, count, first seen, last seen, sample log lines, dashboard link, runbook link, and search query used to find the logs.

Write-back:
If the log or alerting system supports annotations, record the incident ticket ID against the alert group. If not, include the stable marker in the ticket so future runs can find it.

End-state report:
Report tickets created, existing incidents matched, groups below threshold, failed lookups, and any groups skipped because the agent could not decide confidently.

Open questions:
What exact threshold defines an incident?
Should matching existing incidents update the ticket, add a comment, or only report the match?
