---
name: build-ai-workflow
description: >-
  Design, review, and codify checks for AI/agent workflows that create, update,
  skip, route, send, or write back records in real systems. Use when the user is
  building or refining side-effecting automations, agent commands, skills,
  runbooks, or workflow specs where a wrong action has cost: duplicate records,
  missing context, bad routing, incorrect status, untraceable changes, or partial
  execution. Do not use for pure brainstorming, summarization, research
  synthesis, writing drafts, or analysis workflows unless they also create or
  update external state.
---

# Build AI Workflow

## Overview

Use this skill to turn a prototype agent workflow into a repeatable workflow spec. The key distinction: the happy path proves the agent can act; the checks around the action determine whether the workflow is safe to run again.

This applies when an agent workflow touches real systems: creating tickets, updating CRM records, sending messages, writing notes back to a source system, changing statuses, routing work, or any other side effect.

## Boundary

Use this skill when the workflow has side effects that need to be safe to repeat.

Good fits:

- Support conversation -> follow-up task.
- Sales call notes -> CRM update.
- Error logs -> incident ticket.
- User feedback -> product issue.
- Email -> sent reply or status update.
- Meeting transcript -> assigned action items.
- Document review -> comments, approvals, or status changes.

Do not use this skill for workflows that only brainstorm, summarize, research, analyze, or draft text unless the workflow also creates, updates, routes, sends, marks, links, or writes back external records.

## Core Workflow

When designing or revising a workflow, produce a spec that answers these operational questions.

### 1. Start Condition

Define what must be available before the workflow begins.

- Required user inputs, source scope, time windows, projects, labels, config, or destination systems.
- Downstream side-effect tools that must work before source retrieval starts: create, update, search, route, send, or write-back tools.
- If a required downstream side-effect tool is unavailable, abort before fetching or processing source records. Say what is missing and what was not done.
- Source read access can usually be validated by the first targeted fetch. Do not add a separate source preflight unless source retrieval is expensive, slow, incomplete by default, or has side effects.

### 2. Source Scope

Define exactly what source records are in scope.

- What records to fetch.
- Which filters to apply.
- Which time window or status range to use.
- How to avoid expensive or incomplete scans when a more targeted query exists.

### 3. Action Decision

For every source item, define how the workflow decides create, update, skip, or fail.

- Check whether the item was already handled before creating anything.
- Do not infer that a batch is safe because a sample looked safe.
- Do not create containers, parents, groups, or summaries until at least one child/action is proven necessary.
- Define the exact lookup/search used before creation. If the destination search is fuzzy, indexed, or brittle, search broadly enough to find candidates and then exact-match in the workflow logic.
- When a lookup unexpectedly returns nothing, preserve enough proof to troubleshoot: query, filters, result count, and the action taken.
- Define what happens when the workflow cannot decide confidently.

### 4. Stable Marker

Define how the workflow recognizes the same item across runs.

- Use a marker that is generated the same way every time.
- Convert timestamps, timezones, casing, whitespace, and ID formats into one comparison format before comparing.
- Convert timestamp values from their provided timezone into the workflow's chosen comparison timezone; do not assume a timestamp is already in that timezone unless the source explicitly says so.
- Prefer source-system IDs plus a consistently formatted timestamp or version when one record can have multiple events.
- Avoid relying only on human-readable titles or summaries for duplicate checks.

### 5. Create Or Update Rules

Define what a correct created/updated record must contain.

- Where the record belongs.
- Required title/summary format.
- Required fields and metadata.
- Required timestamps and timezone.
- Required source links, conversation links, evidence links, or attachments.
- Required assignee, owner, project, type, status, priority, or labels.

### 6. Write-Back

Define what should be written back to the original source after successful action.

- Record the destination key, URL, status, or processing note in the source system when safe and allowed.
- Do not overwrite useful prior context unless the workflow explicitly requires cleanup.
- If write-back fails after creation succeeds, report the mismatch and provide repair steps.

### 7. End-State Report

Define what the workflow reports at the end.

- Created records.
- Updated records.
- Skipped records and why.
- Failed records and what remains unresolved.
- Source records that could not be checked.
- Any assumptions, partial results, or manual follow-ups.

### 8. Learning Loop

After a real run, inspect the output against the intended behavior. If a mismatch appears, update the command, skill, or runbook so the same mistake has fewer ways to repeat.

Do not describe this as the workflow "learning" or "improving itself" unless there is an actual automated learning mechanism. Say the command/skill/spec became more explicit because the human or agent updated it.

## Cleanup Mode

Use cleanup mode after a run creates incorrect side effects.

1. Identify the source marker that should have prevented the duplicate or wrong action.
2. Convert existing destination records to the same marker format the workflow should use now.
3. Group destination records by that marker or by the intended grouping rule.
4. For each duplicate group, choose the record to keep using a clear rule such as earliest-created, already-linked, or explicitly chosen by the user.
5. Mark later or incorrect records as duplicates, move them, close them, or rename them according to the destination system's safe cleanup practice.
6. Update the workflow spec so future runs perform the same-format check before creating records.
7. Report what was cleaned up, what remains ambiguous, and what was not changed.

## Review Mode

When reviewing an existing workflow, lead with gaps that can cause incorrect side effects.

Use this order:

1. Missing start/stop conditions.
2. Missing or weak duplicate checks.
3. Unstable markers or inconsistent timestamp handling.
4. Missing required context in created records.
5. Missing write-back to the source system.
6. Missing final report or unclear partial-failure handling.

For each finding, state:

- What can go wrong.
- Why the current workflow allows it.
- The concrete rule to add.

## Output Patterns

For a new workflow, return:

```text
Workflow Check Spec

Purpose:

Start condition:
Source scope:
Action decision:
Stable marker:
Create/update rules:
Write-back:
End-state report:
Open questions:
```

For a review, return:

```text
Findings

1. <risk>
What can go wrong:
Why this workflow allows it:
Rule to add:

Suggested spec changes:
```

## Portability Rules

- Keep the spec tied to the user's actual source systems, destination systems, and side effects.
- Do not assume a vendor, API, cadence, owner, status, priority, cleanup action, or write-back field unless the user provides it.
- Generalize checks by function, such as source scope, destination lookup, duplicate marker, create/update rule, write-back, and end-state report.
- When a workflow term is ambiguous, define it in the spec instead of relying on implicit meaning.
- Preserve the distinction between the action the agent performs and the checks around that action.
