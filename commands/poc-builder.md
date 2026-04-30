Activate the 3Cloud AI Engineering PoC Factory for the client: $ARGUMENTS

Step 1 — Load practice context
Read INDEX.md first.
Read Knowledge/Practice/AI-Engineering/ENGINEERING.md — master practice context.
Read Knowledge/Practice/AI-Engineering/Agent-Factory/Master-Prompt.md — 10-step pipeline.

Step 2 — Load client context
Check Clients/$ARGUMENTS/ for advisory context (Client Context.md, roadmaps, etc.)
If no advisory context exists, note: "No advisory context — all decisions from PRD + customer."

Step 3 — Find the PRD
Look for SPEC.md or *-prd.md in the current working directory or ask the user where the PRD is.

Step 4 — Execute the 10-step pipeline
Follow Master-Prompt.md exactly:
1. Read the PRD
2. Check advisory context (done in Step 2)
3. Classify scope (Demo or PoC) via Scope-Gate.md
4. Run PRD Intake Pipeline (PRD-Intake-Pipeline.md)
5. Run Architecture Selector (Architecture-Selector.md)
6. Produce Architecture Spec + cost estimate
7. ★ HUMAN CHECKPOINT 1 — present and wait for approval
8. Scaffold + assign tasks (Wave-Planner.md + Agent-Roster.md)
9. Execute waves autonomously
10. ★ HUMAN CHECKPOINT 2 — present quality gates and wait

## Key Rules
- MAF for runtime orchestration — NEVER Semantic Kernel
- AGENTS.md in every generated project
- Architecture approval BEFORE any code generation
- Every client gets a fresh architecture from the PRD — never replay a previous client's design
