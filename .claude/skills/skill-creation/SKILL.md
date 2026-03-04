---
description: Guide for creating new Claude Code skills — covers SKILL.md structure, frontmatter, commands, and quality patterns
user-invocable: false
---

# Skill Creation Skill

A meta-skill that codifies best practices for creating new Claude Code skills
and commands. Based on patterns from the projectious.work plugin (excalidraw-
diagram, latex, latex-cheatsheet, handoff-format) and official Claude Code
documentation.

## Skill Anatomy

A skill is a directory under `.claude/skills/` containing at minimum a
`SKILL.md` file:

```
.claude/skills/<skill-name>/
├── SKILL.md              # Required — main instructions
├── references/           # Optional — schemas, templates, scripts
│   ├── json-schema.md
│   ├── template.tex
│   └── helper.py
└── examples/             # Optional — sample outputs
    └── sample.md
```

## SKILL.md Structure

### 1. YAML Frontmatter (required)

```yaml
---
description: <what this skill does and when to use it — max 200 chars>
user-invocable: true|false
argument-hint: <argument placeholder shown in autocomplete>
---
```

- **`description`** — Claude uses this to decide when to auto-invoke the skill.
  Be specific and "pushy": state explicitly what triggers use. Include keywords
  Claude would encounter in relevant conversations.
- **`user-invocable: true`** — skill appears as a `/slash-command`. Set `false`
  for background knowledge skills (like this one or the handoff format).
- **`argument-hint`** — shown after the slash command in autocomplete. Use
  angle brackets: `<description of document>`, `<diagram subject>`.

### 2. Title and Purpose (1–2 lines)

```markdown
# Skill Name

One-sentence description of what this skill produces.
```

### 3. Workflow Section

Define the numbered steps an agent follows. This is the most critical section —
it determines execution quality.

```markdown
## Workflow

1. **Design** — decide X, Y, Z.
2. **Write/Generate** — create the artifact following rules below.
3. **Compile/Render** — produce the output.
4. **Validate** — inspect the output visually or programmatically, fix, repeat.
```

**Key pattern: always include a validation loop.** Every skill that produces
artifacts must define how to check the output and iterate until clean.

### 4. Rules and Constraints

Organize domain-specific rules into logical sections. Use tables for
comparative guidance (use vs. avoid):

```markdown
| Purpose | Use | Avoid |
|---|---|---|
| Tables | `tabularray` | `tabular` + `\hline` |
```

Rules should be:
- **Prescriptive** — "always do X", "never do Y" — not "consider doing X"
- **Concrete** — include code snippets, exact commands, specific values
- **Justified** — when a rule is non-obvious, add a brief rationale

### 5. Reference Material

For large reference content (schemas, templates, API docs), put them in
separate files under `references/` and link from SKILL.md:

```markdown
See `references/json-schema.md` for element types and properties.
```

Keep SKILL.md under ~250 lines. Move detailed reference material to separate
files. Claude loads referenced files when needed.

### 6. File Organisation Section

Document the expected output file structure:

```markdown
## File Structure

\```
docs/<name>/
├── main.tex
├── sections/
├── figures/
└── out/              # gitignored
\```
```

## Command Files

Commands live in `.claude/commands/` as standalone `.md` files. They are the
user-facing entry point that invokes one or more skills.

### Command Structure

```markdown
# /<command-name> — Short description

One-line summary of what this command does.

## Usage

\```
/<command-name> <arguments>
\```

## Behaviour

1. **Read the skill** — Load the relevant SKILL.md file(s).
2. **Select agent** — Route to the appropriate agent based on content type.
3. **Execute** — Follow the skill workflow.
4. **Validate** — Run the skill's validation loop.
5. **Report** — Show results to the principal.

## Context

Before starting, read:
- `context/DECISIONS.md` — for relevant prior decisions
- The skill's SKILL.md — for rules and patterns

## Example

\```
/<command-name> A concrete example of usage
\```
```

### Command vs. Skill Relationship

- **Skill** = the knowledge and rules (how to do something)
- **Command** = the trigger and workflow (when and in what order)
- One command may invoke multiple skills (e.g., `/latex-document` loads both
  the latex and latex-cheatsheet skills depending on document type)
- One skill may be invoked by multiple commands or by other skills
- Skills with `user-invocable: true` can also be invoked directly as
  `/skill-name` without a separate command file

## Quality Checklist for New Skills

Before considering a skill complete:

1. **Frontmatter** — `description`, `user-invocable`, and `argument-hint` (if
   user-invocable) are all set correctly.
2. **Workflow** — numbered steps cover the full lifecycle from design to
   validation.
3. **Validation loop** — the skill defines how to check output quality and
   iterate. Every artifact-producing skill must have this.
4. **Concrete rules** — rules include code snippets, exact commands, and
   specific values. No vague guidance.
5. **Modern tooling** — the skill recommends current tools and packages, with
   explicit "avoid" lists for deprecated alternatives.
6. **File structure** — the expected output layout is documented.
7. **Command file** — if `user-invocable: true`, a corresponding command file
   exists in `.claude/commands/` (or the skill itself serves as the command).
8. **Context size** — SKILL.md is under ~250 lines. Reference material is in
   separate files.
9. **Tested** — the skill has been invoked at least once and produced a valid
   output.
10. **DECISIONS.md** — the skill's creation is recorded in the decision log.

## Patterns from Existing Skills

### Excalidraw Diagram Skill
- **Lean context:** reduced reference material from 24KB to ~6KB by dropping
  verbose examples — Claude generates valid output from a concise schema.
- **External tooling:** uses Playwright for rendering, with a first-time setup
  section documenting exact commands.
- **Semantic design tokens:** defines a color palette with roles (primary,
  secondary, success, warning, danger, neutral).

### LaTeX Skill
- **Package load order:** specifies exact ordering to avoid conflicts — this
  level of precision prevents hard-to-debug issues.
- **Comparative tables:** "Use X / Avoid Y" format makes rules scannable and
  unambiguous.
- **Design principles:** encodes domain knowledge (CRAP, information hierarchy)
  as actionable rules rather than theory.

### LaTeX Cheatsheet Skill
- **Extends another skill:** starts with "Read the LaTeX skill first" — avoids
  duplicating shared rules.
- **Comparison table:** clearly shows what differs from the base skill.
- **Iterative workflow:** explicitly acknowledges that layout is trial-and-error
  and documents the iteration strategy.

### Handoff Format Skill
- **Non-invocable:** `user-invocable: false` — it's background knowledge that
  other skills and agents reference.
- **Template-driven:** provides a complete fill-in-the-blanks template that
  agents follow exactly.

## Anti-Patterns to Avoid

- **Vague instructions** — "write good code" vs. "use `tabularray` for tables,
  never `tabular` with `\hline`"
- **No validation loop** — producing output without checking it leads to
  quality issues
- **Monolithic SKILL.md** — files over 300 lines should be split; large
  reference content goes in `references/`
- **Duplicating shared rules** — if two skills share rules, one should extend
  the other (like latex-cheatsheet extends latex)
- **Missing frontmatter** — Claude cannot discover or auto-invoke skills
  without proper YAML frontmatter
- **Overly philosophical content** — Claude works better with concrete rules
  than with design philosophy essays; keep it actionable
- **No file structure documentation** — agents need to know where to put output
  files
