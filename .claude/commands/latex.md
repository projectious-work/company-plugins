# /latex — Create a LaTeX document

Creates a professional PDF document using the LaTeX skill, compiles it with
LuaLaTeX, and validates the result visually.

## Usage

```
/latex <description of the document to create>
```

## Behaviour

1. **Read the skill** — Load `.claude/skills/latex/SKILL.md` for compilation
   workflow, typography rules, color palette, and design principles.

2. **Select agent** — Choose based on content:
   - Technical documentation, architecture docs → Developer
   - Project plans, status reports, proposals → Project Manager
   - Marketing materials, client-facing docs → Marketing Strategist

3. **Write** — Create `.tex` files following the skill rules. Split content into
   separate files via `\input{}`. Save to `docs/<doc-name>/`.

4. **Compile** — Run:
   ```bash
   cd docs/<doc-name> && latexmk -lualatex -output-directory=out main.tex
   ```

5. **Validate** — Extract pages and inspect:
   ```bash
   pdftoppm -png -r 200 out/main.pdf out/preview
   ```
   Read each preview PNG. Check for layout issues, overfull boxes, broken
   references, readability. Fix and recompile until clean.

6. **Report** — Show preview images to the principal. State where files are saved.

## Context

Before writing, read:
- `context/DECISIONS.md` — for relevant prior decisions
- `.claude/skills/latex/SKILL.md` — for all rules and patterns

## Example

```
/latex A project proposal for a cloud migration engagement
```
