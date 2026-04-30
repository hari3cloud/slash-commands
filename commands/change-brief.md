Author a new Change Brief for the current project.

If `$ARGUMENTS` is empty: walk the user through type selection and brief authoring interactively.

If `$ARGUMENTS` starts with `--type <feature|enhancement|bugfix|refactor|tech-debt>`: skip type selection.

If `$ARGUMENTS` is a quoted string: treat as the change title; default to type `feature`; abbreviated authoring.

Special flags:
- `--import-from <path>` — convert items from an existing flat backlog (e.g., `ENHANCEMENTS.md`) into individual Change Briefs

## Execution

1. Read the methodology skill at `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/_System/skills/change-brief.md` to know what makes a good brief.

2. Confirm we're in a project repo (look for `.claude/lifecycle.json`). If not, ask which project directory the brief is for.

3. **Type selection** (if not specified):
   ```
   What kind of change is this?
   1. feature — new capability not present today
   2. enhancement — improving existing capability
   3. bugfix — fix a defect
   4. refactor — internal restructure, no behavior change
   5. tech-debt — security/deps/performance/observability
   ```

4. Read the corresponding template from `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/3Cloud-Practice/AI-Engineering/Templates/change-brief/<type>.template.md`

5. **Interactive authoring** — walk through sections in order:
   - Title (one line)
   - Context (why this change, source)
   - Type-specific problem framing (problem/opportunity, current behavior + improvement, repro steps, etc.)
   - Acceptance criteria (testable items)
   - Out of scope (push for explicit answers)
   - Dependencies (other briefs, external systems)
   - Estimated waves (your best guess; explain the heuristic)
   
   For each section: show the template's prompt text, accept user's answer, validate it's not empty/vague, optionally suggest improvements (e.g., "this acceptance criterion isn't testable — try framing it as observable behavior").

6. Generate `id` field: `YYYY-MM-DD-short-kebab-slug` from today's date and a slug derived from the title.

7. Write the brief to `<project>/docs/changes/<id>.md` with `status: draft`.

8. Display the completed brief and ask: "Ready to approve this brief? Reply 'yes' to set status: approved, or 'edit' to revise."

9. If approved:
   - Update brief frontmatter to `status: approved`
   - Add brief path to `lifecycle.json` `changes.open` list
   - Print: "Brief approved. Run `/work-change docs/changes/<id>.md` when ready to execute."

## Quick mode (when $ARGUMENTS is a quoted string)

If user passed something like `/change-brief "fix typo in landing page"`:

1. Skip type selection (default `feature` for quick mode unless words like "fix"/"bug" suggest `bugfix`)
2. Use the title as both `title` and the `Context` section seed
3. Ask only the essential questions:
   - One-line desired behavior
   - 1-3 acceptance criteria
   - Estimated waves (default 1)
4. Write brief with `status: draft`
5. Display and ask for approval as in step 8

## --import-from mode

If user passed `--import-from ENHANCEMENTS.md`:

1. Read the source file, parse each ENH-XXX item
2. For each PENDING item, generate an equivalent Change Brief:
   - Map the ENH item's title → brief title
   - Type defaults to `enhancement` unless the item's content strongly suggests otherwise
   - Map fields where possible (Problem → Context+Problem, Acceptance Criteria → Acceptance Criteria, etc.)
3. Write each brief individually to `docs/changes/`
4. Print a summary of imported briefs and ask user to review & approve each

## Constraints

- Never approve a brief without explicit user confirmation
- Never write outside `<project>/docs/changes/`
- Always validate that acceptance criteria are testable before allowing approval (push back if vague)
- Default `status: draft` — only approve on explicit user confirmation
