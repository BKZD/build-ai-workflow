# Build AI Workflow

A portable Agent Skill for designing and reviewing AI workflows that touch real systems.

Use it when an agent workflow creates, updates, skips, routes, sends, or writes back records where the wrong action has cost: duplicate records, missing context, bad routing, incorrect status, untraceable changes, or partial execution.

The core idea:

> The happy path proves the agent can act.
> The checks around the action determine whether the workflow is safe to run again.

## What This Skill Helps With

- Turning a prototype automation into a repeatable workflow spec.
- Reviewing an existing agent command, skill, prompt, or runbook for side-effect risk.
- Defining when a workflow should create, update, skip, or fail.
- Preventing duplicate records across repeated runs.
- Making sure created records include enough context for a human to act.
- Writing back a destination key, URL, or processing marker to the original source.
- Reporting created, updated, skipped, and failed records clearly at the end of a run.

## Good Fits

- Support conversation -> follow-up task.
- Sales call notes -> CRM update.
- Error logs -> incident ticket.
- User feedback -> product issue.
- Email -> sent reply or status update.
- Meeting transcript -> assigned action items.
- Document review -> comments, approvals, or status changes.

## Not A Good Fit

This is not meant for workflows that only brainstorm, summarize, research, analyze, or draft text.

Use it when the workflow creates, updates, routes, sends, marks, links, or writes back external records.

## Install

### Codex

```bash
git clone https://github.com/BKZD/build-ai-workflow.git ~/.codex/skills/build-ai-workflow
```

Restart Codex after installing a new skill.

### Claude Code

```bash
git clone https://github.com/BKZD/build-ai-workflow.git ~/.claude/skills/build-ai-workflow
```

Claude Code personal skills live under `~/.claude/skills/<skill-name>/SKILL.md`.

### Hermes

For a direct single-skill install:

```bash
hermes skills install https://raw.githubusercontent.com/BKZD/build-ai-workflow/main/SKILL.md
```

For a local full-folder install:

```bash
git clone https://github.com/BKZD/build-ai-workflow.git ~/.hermes/skills/build-ai-workflow
```

### OpenClaw

For a personal skill available across workspaces:

```bash
git clone https://github.com/BKZD/build-ai-workflow.git ~/.agents/skills/build-ai-workflow
```

For one workspace only, clone it into that workspace's `skills/` directory:

```bash
git clone https://github.com/BKZD/build-ai-workflow.git ./skills/build-ai-workflow
```

## Usage

Ask your agent to use the skill when designing or reviewing a side-effecting workflow:

```text
Use build-ai-workflow to turn this automation into a workflow check spec:

Every morning, scan support conversations from the last 24 hours.
Create follow-up tasks for unresolved billing issues.
Link each task back to the original conversation.
Skip anything already processed.
```

Or ask it to review an existing workflow:

```text
Use build-ai-workflow to review this agent command before it runs in production.
Focus on duplicate records, missing context, write-back, and partial failures.
```

Expected output:

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

## Examples

See:

- [Support conversation to follow-up task](examples/support-conversation-to-follow-up-task.md)
- [Sales call notes to CRM update](examples/sales-call-notes-to-crm-update.md)
- [Error logs to incident ticket](examples/error-logs-to-incident-ticket.md)

## Why It Is Portable

This skill follows the Agent Skills pattern: a folder containing `SKILL.md` with YAML frontmatter and markdown instructions. It has no scripts, no runtime dependencies, no API keys, and no tool-specific logic.

## Roadmap: Multi-Skill Stack

Build AI Workflow is currently distributed as a standalone skill.

The next packaging step is to move related workflow skills into a stack layout:

```text
ai-workflow-stack/
├── SKILL.md                  # root/router skill
├── agents/openai.yaml        # optional Codex/OpenAI metadata
├── setup                     # host-aware installer
├── skills/
│   └── build-ai-workflow/
│       ├── SKILL.md
│       └── agents/openai.yaml
└── README.md
```

A stack package will make it possible to install several related skills from one repo. The setup script will handle host-specific install paths for Codex, Claude Code, Hermes, OpenClaw, and similar harnesses while keeping each skill's `SKILL.md` as the portable core artifact.

## References

- [Agent Skills open standard](https://github.com/agentskills/agentskills)
- [OpenAI skills catalog for Codex](https://github.com/openai/skills)
- [Claude Code skills documentation](https://code.claude.com/docs/en/skills)
- [Hermes skills system](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/features/skills.md)
- [OpenClaw skills documentation](https://open-claw.bot/docs/tools/skills/)

## License

MIT
