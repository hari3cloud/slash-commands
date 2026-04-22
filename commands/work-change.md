Execute a Change Brief by routing tasks through the project's agent fleet.

If `$ARGUMENTS` is a file path: process that specific brief.
If `$ARGUMENTS` is a quoted string: quick-mode, treat as inline informal brief.
If `$ARGUMENTS` is empty: process the oldest open brief from `.claude/lifecycle.json`'s `changes.open` list.

This command also responds to the legacy invocation `/enhancement` for backward compatibility.

## Execution

Read the methodology skill at `${VAULT_ROOT:-$HOME/Library/CloudStorage/OneDrive-3Cloud/3Cloud}/Hari_Work/_System/skills/enhancement.md` and follow it exactly. That skill is the authoritative process for this command.

The skill walks through:

1. **Load lifecycle context** — read `.claude/lifecycle.json`, confirm phase is iteration or evolution
2. **Load the project's fleet** — read `.claude/agents/`, identify which specialists exist
3. **Load the Change Brief** — from path, from `lifecycle.json` open list, or from quoted argument
4. **Parse and route** — extract acceptance criteria, out-of-scope, dependencies; route to fleet members per the routing matrix
5. **Produce mini wave plan** — present the wave breakdown to the user in the canonical format
6. **WAIT for user approval** — never start Wave A without explicit "go"
7. **Execute waves** — Wave A parallel, Wave B after A (with ContractInject if api/contract.md changed), Wave 2.5 localhost check if UI changed, Wave C verification
8. **Mark done** — update brief status, lifecycle.json, append Resolution to brief

## Critical rules from the skill (don't violate)

- **Contract rule:** Backend Engineer updates `api/contract.md` BEFORE Frontend Engineer writes new fetch calls. Hard dependency in wave plan.
- **Approval gate:** Never start Wave A without explicit human approval of the wave plan.
- **Acceptance verification:** Test Engineer must verify EACH acceptance criterion explicitly. Not "tests pass" — per-criterion verification.
- **No status: done** without all criteria checked.
- **Project's actual fleet:** Route to agents that exist in this project's `.claude/agents/`, not a generic roster. If a needed specialist doesn't exist, route to closest available and flag for user approval.

## Quick mode (when $ARGUMENTS is a quoted string)

For trivial changes that don't need a structured brief:

1. Construct an in-memory minimal brief from the description
2. Ask user for acceptance criteria if not obvious
3. Skip the brief authoring; proceed to wave planning
4. Same approval gates apply

## Lifecycle state updates

Throughout execution, keep `.claude/lifecycle.json` accurate:
- On approval: brief moves from `changes.open` → `changes.in_progress`
- On completion: `changes.in_progress` → `changes.done`
- Brief frontmatter `status` field tracks the same transitions

## Reporting

When all waves complete and Test Engineer reports `AcceptanceVerified`:

```markdown
## Change Brief Complete

**Brief:** <id> — <title>
**Type:** <type>
**Waves run:** A (<agents>), B (<agents>), C (<agent>)
**Files changed:** <count> across <areas>
**Tests added:** <count>
**Acceptance criteria verified:** N/N

Status: done. Updated in lifecycle.json and brief frontmatter.
```

## Constraints

- Never start execution without user approval of wave plan
- Never proceed against a `status: draft` brief — require approved
- Never violate the contract rule (Backend writes contract first, Frontend reads)
- Never let an agent modify files outside its file scope
- Never mark done if any acceptance criterion unverified
