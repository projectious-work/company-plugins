---
description: Standard artifact format for inter-agent communication and task handoffs
user-invocable: false
---

# Structured Handoff Format

This skill defines the standard artifact format for inter-agent communication at
projectious.work. Every agent uses this format when passing work to another agent.

## When to Use

- When one agent's output becomes another agent's input
- When the `/team` command decomposes a task into role-specific sub-tasks
- When an agent completes work that requires follow-up by a different role

## Handoff Artifact Template

```markdown
# Handoff: <brief title>

## From
<originating agent role>

## To
<target agent role>

## Backlog Reference
<BACK-NNN ID(s) this work relates to, or "None" if ad-hoc>

## Task Context
<What is the broader goal? Why is this handoff happening? Reference any relevant
prior decisions from context/DECISIONS.md.>

## Deliverable
<What has been produced or what needs to be produced? Include inline content,
file references, or summaries as appropriate.>

## Status
- **Completed:** <what is done>
- **Blockers:** <anything preventing progress, or "None">
- **Assumptions:** <any assumptions made that the receiving agent should validate>

## Next Actions
1. <specific, actionable next step with clear owner>
2. <...>
```

## Rules

- Every section is required. Use "None" or "N/A" if a section is not applicable.
- Keep each section concise — prefer bullet points over paragraphs.
- Reference `context/DECISIONS.md` entries by date when relevant.
- **Always include the `BACK-NNN` backlog ID** when the work relates to a
  tracked item. This enables the Project Manager to update the backlog.
- When reporting completion or status changes, the handoff to the PM must
  include enough detail for the PM to update `context/BACKLOG.md`.
- The **Next Actions** must be concrete and actionable, not vague ("improve
  quality" is bad; "run linter on src/ and fix all warnings" is good).
- If a handoff crosses more than one role boundary, create separate artifacts
  for each target agent.
