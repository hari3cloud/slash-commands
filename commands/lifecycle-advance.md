Advance the current project to the next lifecycle phase.

If `$ARGUMENTS` is a phase name (`ideation | greenfield | iteration | evolution | sunset`): transition to that phase.
If `$ARGUMENTS` is empty: ask which phase to transition to, based on current state.

## Execution

1. Read `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/_System/skills/project-lifecycle.md` for the canonical 5-phase model and transition gate questions.

2. Read current state from `./.claude/lifecycle.json`. Identify current phase.

3. If target phase wasn't specified, present options based on what's possible from current state. The 5 phases:
   - **0 ideation** — drafting the Project Brief
   - **1 greenfield** — building from PRD (4-wave model)
   - **2 iteration** — agile features/fixes (Change Briefs)
   - **3 evolution** — pivots / refactors / capability expansion
   - **4 sunset** — wind-down

   Show valid forward transitions and the reverse `3 → 2` if applicable.

4. Run the **transition gate questions** from the lifecycle skill for the target phase. Examples:

   For **greenfield → iteration:**
   - Is the product deployed or otherwise usable? (yes/no)
   - Is at least one user (even internal) interacting with it? (yes/no)
   - Are tests passing? (yes/no)
   - Is there a backlog of work that's not part of original PRD scope? (yes/no)

   For **iteration → evolution:**
   - Is the change being contemplated significantly larger than typical Change Briefs? (yes/no)
   - Does it require new agents or substantial fleet update? (yes/no)
   - Does it touch architecture, stack, or business model? (yes/no)

   For each "no" or "uncertain" answer, ask the user if they want to proceed anyway with the transition. Phase transitions are explicit and human-approved — there's no "the system says you're not ready."

5. On user confirmation, update `lifecycle.json`:
   - Add an exit timestamp to the current phase in `phase_history`
   - Add new phase entry with start timestamp
   - Update top-level `phase` and `phase_entered`
   - Preserve all other state (changes lists, fleet_version, brief_path)

6. Print confirmation:

```markdown
## Phase Transition Complete

**Project:** <project name>
**Previous phase:** <old phase> (entered <date>, duration <duration>)
**New phase:** <new phase> (entered today)

**What this means:**
<one-paragraph explanation of what tools and patterns now apply, drawn from the lifecycle skill>

**Suggested next actions:**
<2-3 things appropriate for the new phase>
```

## Edge cases

**Skipping phases:** If user wants to jump (e.g., 0 → 2 because there was no formal greenfield), allow it but ask for confirmation. Add a phase_history entry noting the skip.

**Reverting:** Going back from iteration → greenfield is unusual but allowed (e.g., decided to rebuild). Confirm with user; preserve full history.

**Sunset is terminal:** No transitions out of sunset. If user wants to revive, that's effectively a new project (or evolution-from-sunset, treat as edge case requiring manual lifecycle.json edit).

## Constraints

- Never transition without user confirmation
- Never modify lifecycle.json beyond the phase fields (don't touch changes lists, brief_path, etc.)
- Always preserve full phase_history (audit trail)
- If `lifecycle.json` doesn't exist, this command can't run — direct user to `/fleet-generate` first to initialize
