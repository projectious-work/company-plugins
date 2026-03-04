# /latex-document — Create a LaTeX document

Creates a LaTeX document, compiles it, and validates the result visually.

## Usage

```
/latex-document <description of the document to create>
```

## Behaviour

1. **Read the skill** — Load `.claude/skills/latex/SKILL.md` for compilation workflow,
   typography rules, color palette, and design principles. For cheatsheets, also
   load `.claude/skills/latex-cheatsheet/SKILL.md`.

2. **Select agent** — Choose based on content:
   - Technical documentation, architecture docs → Developer
   - Project plans, status reports, proposals → Project Manager
   - Marketing materials, client-facing docs → Marketing Strategist

3. **Determine document type** — standard document or cheatsheet. If the user
   mentions "cheatsheet", "reference sheet", "summary sheet", or "cheat sheet",
   use the cheatsheet skill. Otherwise use the base latex skill.

4. **Write** — Create `.tex` files following the skill rules. Split content into
   separate files via `\input{}`. Save to `docs/<doc-name>/`.

5. **Compile** — Run:
   ```bash
   cd docs/<doc-name> && latexmk -lualatex -output-directory=out main.tex
   ```

6. **Validate** — Extract pages and inspect:
   ```bash
   pdftoppm -png -r 200 out/main.pdf out/preview
   ```
   Read each preview PNG. Check for layout issues, overfull boxes, broken
   references, readability. Fix and recompile until clean.

7. **Report** — Show preview images to the principal. State where files are saved.

## Context

Before writing, read:
- `context/DECISIONS.md` — for relevant prior decisions
- The appropriate skill SKILL.md — for rules and patterns

## Examples

```
/latex-document A project proposal for a cloud migration engagement
```

```
/latex-document A cheatsheet summarising our agent plugin architecture
```
