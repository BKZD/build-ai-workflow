# Example: Support Conversation To Follow-Up Task

## User Prompt

```text
Use build-ai-workflow to design this workflow:

Every morning, review support conversations from the last 24 hours.
Create a follow-up task for unresolved billing issues.
Link the task back to the original conversation.
Skip anything already processed.
```

## Workflow Check Spec

Purpose:
Create follow-up tasks for unresolved billing issues found in recent support conversations.

Start condition:
Before fetching conversations, confirm the downstream side-effect tools are available: task creation and source write-back. Source read access can be validated by the first targeted fetch. If task creation or write-back is unavailable, abort before fetching conversations.

Source scope:
Fetch support conversations from the last 24 hours. Only include conversations that are unresolved and mention a billing issue. Exclude conversations already marked with a destination task ID or processing note.

Action decision:
For each conversation, check whether it already has a linked task or processing note before creating anything. If the conversation is ambiguous, skip it and report why. Do not create a task based on a sampled subset.

Stable marker:
Use the source conversation ID plus the billing issue timestamp, normalized to UTC. Do not rely only on the conversation title or customer name.

Create/update rules:
Each task must include the customer identifier, source conversation link, short issue summary, relevant timestamps, and the reason it needs follow-up. Put it in the correct billing follow-up queue.

Write-back:
After successful task creation, write the task ID or URL back to the source conversation. If the task is created but write-back fails, report the mismatch and include the repair action.

End-state report:
Report created tasks, skipped conversations with reasons, failures, and any conversations that could not be checked.

Open questions:
What exact field or note marks a conversation as already processed?
Which billing queue should receive the tasks?
