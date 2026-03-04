# /latex-cheatsheet — Create a LaTeX cheatsheet

Creates a dense, visually structured reference sheet using the LaTeX cheatsheet
skill (tcbposter grid layouts), compiles it with LuaLaTeX, and validates the
result visually.

## Usage

```
/latex-cheatsheet <description of the cheatsheet to create>
```

## Behaviour

1. **Read the skills** — Load both `.claude/skills/latex/SKILL.md` (base rules)
   and `.claude/skills/latex-cheatsheet/SKILL.md` (cheatsheet-specific patterns).

2. **Select agent** — Choose based on content:
   - Technical reference, API docs, language cheatsheets → Developer
   - Process guides, methodology summaries → Project Manager
   - Service overviews, capability summaries → Marketing Strategist

3. **Write** — Create `.tex` files following both skill files. Use `tcbposter`
   with 12-column grid, `posterboxenv` with `cheatboxskin`, and split each
   box's content into its own file. Save to `docs/<cheatsheet-name>/`.

4. **Compile** — Run:
   ```bash
   cd docs/<cheatsheet-name> && latexmk -lualatex -output-directory=out main.tex
   ```

5. **Validate** — Extract pages and inspect:
   ```bash
   pdftoppm -png -r 200 out/main.pdf out/preview
   ```
   Read each preview PNG. Check for: dead space, overfull boxes, unreadable
   text, misaligned boxes, broken references. Rearrange boxes and recompile
   until pages are densely filled.

6. **Report** — Show preview images to the principal. State where files are saved.

## Context

Before writing, read:
- `context/DECISIONS.md` — for relevant prior decisions
- `.claude/skills/latex/SKILL.md` — base typography and compilation rules
- `.claude/skills/latex-cheatsheet/SKILL.md` — cheatsheet-specific patterns

## Example

```
/latex-cheatsheet A Docker commands reference sheet covering build, run,
compose, networking, and volumes
```
