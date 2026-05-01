Orchestrate a project's tailored agent fleet through a 5-wave end-to-end greenfield PoC build with mandatory adversarial verification.

Special flags in `$ARGUMENTS`:
- `--scope=<phase|feature>` — scope the build to a PRD phase or named feature instead of the full PoC
- `--adversarial=light` — reduce Wave 4 to security-review + property-based fuzz only (NOT permitted if the PRD has a compliance, risk, or safety-critical section)
- `--dry-run` — produce the master plan and adversarial-pass list, do not write any code
- `--resume` — read the latest `docs/poc-readiness-*.md` and continue from the must-fix resolution loop

## Execution

1. Read the methodology skill at `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/_System/skills/fleet-orchestration.md` and follow it exactly. That skill is the authoritative process for this command.

2. Run pre-flight checks (Step 1 of the methodology):
   - `<project>/.claude/agents/` exists and contains at minimum `lead-architect.md` and `test-engineer.md`
   - `<project>/.claude/lifecycle.json` exists with `phase: "greenfield"`
   - The brief at `lifecycle.json.brief_path` (default `docs/PRD.md`) has `status: approved` and a `type` field
   - Working tree is clean (or user confirms intentional dirty state)
   - If any check fails, stop with the methodology's remediation messages

3. Invoke the `lead-architect` subagent (via the `Agent` tool with `subagent_type: "lead-architect"`) with the brief, fleet roster, and scope. Receive a 5-wave master plan back. Use the template at `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/Knowledge/Practice/AI-Engineering/Templates/orchestration/master-plan.template.md` as the output format.

4. Present the master plan and **WAIT for explicit user approval.** The user may approve, edit, scope down, or cancel. Do not invoke any specialist until "go".

5. Execute waves 0 → 4 in order. For each wave:
   - Spawn ALL wave specialists in parallel via multiple `Agent` tool calls in one message
   - Collect WaveDone / ContractChange / Blocked / ScopeQuestion signals
   - Resolve signals before next wave (re-read `api/contract.md` on ContractChange — that's the contract rule)
   - After Wave 2: if any frontend file changed, run Wave 2.5 mini-loop (localhost smoke check); route fixes back before Wave 3

6. Wave 4 adversarial — run four passes in parallel (one message, four `Agent` calls):
   - Pass A: security review (invoke `/security-review` skill, or test-engineer with OWASP prompt as fallback)
   - Pass B: property-based fuzz on safety-critical paths (test-engineer + hypothesis)
   - Pass C: failure injection scenarios (test-engineer or relevant infra/agent specialist)
   - Pass D: compliance review against PRD's risk/compliance sections (lead-architect)
   - Synthesize findings; classify each as `must-fix | should-fix | accepted-risk`

7. If any `must-fix`: enter resolution loop (Step 6 of the methodology). Triage via lead-architect, get user approval on fix plan, spawn fix specialists, re-run failed adversarial check, repeat until clean or user explicitly accepts residual risk.

8. Synthesize a PoC readiness report at `<project>/docs/poc-readiness-{YYYY-MM-DD}.md` using the template at `${VAULT_ROOT:-/AI-Projects/3cloud-vault/Hari_Work}/Knowledge/Practice/AI-Engineering/Templates/orchestration/poc-readiness.template.md`. Verdict: `ready | ready-with-followups | blocked`.

9. Ask the user — do NOT auto-advance — whether to transition `lifecycle.json.phase` from `greenfield` → `iteration`. On approval, update `phase_history`.

10. Print final summary (see methodology Step 7 for format).

## Critical rules from the skill (don't violate)

- **Approval gate (master plan):** Never spawn specialists until the user explicitly approves the master plan from Step 2.
- **Approval gate (fix plan):** Never spawn fix specialists in the resolution loop without explicit user approval of the fix plan.
- **Subagent constraint:** Main Claude is the only `Agent`-tool caller. The `lead-architect` plans; main Claude executes the spawning. Specialists never spawn other specialists.
- **Contract rule:** Backend Engineer updates `api/contract.md` BEFORE Frontend Engineer writes any new fetch calls. Hard dependency in wave plan.
- **Adversarial mandatory:** All four passes run by default. `--adversarial=light` is only permitted if the PRD has no compliance/risk/safety-critical section.
- **must-fix blocks completion:** Never produce a `verdict: ready` readiness report with unresolved must-fix findings.
- **No silent lifecycle advance:** Phase transition requires explicit user confirmation.
- **File scope:** Specialists modify only files within their declared file scope per `.claude/agents/<role>.md`.

## Constraints

- Never run against a project that hasn't been fleet-generated (no `.claude/agents/`)
- Never run against a `phase != greenfield` project — direct to `/work-change` instead
- Never run against a `status: draft` brief — require `approved`
- Never overwrite files in scopes that don't belong to the spawning specialist
- Never proceed without master-plan approval
- Never advance lifecycle without explicit user confirmation
