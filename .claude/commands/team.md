# /team — Orchestrate a task across the virtual company

Takes a task description and decomposes it across the four agent roles
(Developer, Project Manager, Marketing Strategist, Coach), launches the
appropriate agents, and synthesizes the results.

## Usage

```
/team <task description>
```

## Behaviour

1. **Analyse the task** — Determine which roles are relevant. Not every task
   needs all four agents.

2. **Decompose** — Break the task into role-specific sub-tasks. Write a handoff
   artifact for each agent using the structured handoff format
   (`.claude/skills/handoff-format/`).

3. **Launch agents** — Spawn the relevant agents in parallel where possible:
   - Developer (`.claude/agents/developer.md`) — for coding, architecture, infra
   - Project Manager (`.claude/agents/project-manager.md`) — for planning, tracking, coordination
   - Marketing Strategist (`.claude/agents/marketing-strategist.md`) — for content, outreach, positioning
   - Coach (`.claude/agents/coach.md`) — for process review, quality assessment, backlog audits, retrospectives, team improvement, standup facilitation

4. **Collect results** — Gather each agent's output.

5. **Synthesize** — Present a unified summary to the principal with:
   - What was accomplished per role
   - Open items and blockers
   - Recommended next actions

## Context

Before decomposing, read:
- `context/OWNER.md` — to understand principal preferences and weaknesses
- `context/DECISIONS.md` — to respect prior decisions and avoid re-litigating

## Example

```
/team Draft a LinkedIn article about our Agentic AI consulting approach and
create a publishing schedule for the next 4 weeks
```

This would engage:
- **Marketing Strategist** — draft the article and content calendar
- **Project Manager** — create the schedule with dates and milestones
- **Developer** — skipped (no technical task)
- **Coach** — skipped (no process review task)

```
/team Review our backlog health and run a retrospective on last week's progress
```

This would engage:
- **Coach** — backlog audit and retrospective facilitation
- **Project Manager** — backlog status and scheduling input
- **Developer** — skipped (no technical task)
- **Marketing Strategist** — skipped (no content task)
