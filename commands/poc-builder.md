Run the 3Cloud AI Engineering PoC Factory for: $ARGUMENTS

This is the SINGLE canonical entry point. It supersedes `/fleet-generate` and
`/fleet-orchestrate` (both retired). Follow the methodology spec exactly:

1. Read `_System/skills/poc-builder.md` — the authoritative orchestration process. Follow it.
2. Read `INDEX.md` first to resolve all vault paths (spec §8a — never hardcode paths).

## The flow you orchestrate (4 human-approval gates, held in CHAT)

You drive four stages. Three are Dynamic Workflows invoked via the **Workflow tool**;
one (DESIGN) is an interactive chat loop. A workflow CANNOT pause for approval mid-run,
so YOU hold each approval gate in chat BETWEEN workflow invocations.

> Calling the Workflow tool is explicitly authorized here: the user invoked `/poc-builder`,
> a slash command whose instructions tell you to call Workflow.

- **Stage 1 — FRAME:** Call Workflow `name: "poc-frame"` with args
  `{ clientName, useCase, sowPath?, notesPath?, sampleDataPath?, vaultRoot }`. Await completion.
  Present the Architecture Spec (+ 3-option adversarial comparison summary), the cost estimate,
  and the derived roster (which specialists were auto-selected, why, and their grounding). If the
  return has `roster.noMatch`, ASK whether to author each proposed specialist.
  ★ GATE 1 — WAIT for human approval of architecture + roster (override allowed). Do not proceed until "approved".

- **Stage 2 — DESIGN (interactive, NOT a workflow):** As the Design Engineer, present 3 aesthetic
  directions (no component code until one is chosen), then a per-component preview/approve loop.
  Write tokens + components to `output/<name>-<scope>/` and `Design-System.md` to the vault.
  ★ GATE 2 — WAIT for human approval of the design (frozen).

- **Stage 3 — BUILD:** Call Workflow `name: "poc-build"` with args
  `{ outputDir, clientName, scope, architectureSpec, roster, designTokensPath, designComponentsPath, vaultRoot }`.
  Await completion. If the return is `blocked: true` (api-contract incomplete), surface `missing` and stop.
  Otherwise start nothing yourself — the workflow already started the app natively; present the local URL(s).
  ★ GATE 3 — WAIT for human approval of the running app at localhost.

- **Stage 4 — HARDEN+HARVEST:** Call Workflow `name: "poc-harden-harvest"` with args
  `{ phase: "qa", outputDir, architectureSpec, scope, clientName, prdPath, vaultRoot }`. Await completion.
  Present the quality report (gates, adversarial-verify findings, visual regression, verdict).
  ★ GATE 4 — WAIT for human approval of quality. If verdict is `blocked` (unresolved must-fix), do NOT proceed.
  On approval, call Workflow `name: "poc-harden-harvest"` AGAIN with args
  `{ phase: "harvest", outputDir, architectureSpec, scope, clientName, prdPath, vaultRoot, qualityReport, dateForIndex }`.
  This writes back to Obsidian (patterns draft / lessons / Engagements/_INDEX / new specialist) and,
  for POC scope, deploys to Azure Container Apps via `azd up`.

## Hard rules
- Never call the next stage's Workflow before the human approves the current gate.
- Pass each workflow's structured return into the next workflow's `args` (state flow); reference large
  artifacts by path on disk, never inline them.
- MAF for runtime orchestration — never Semantic Kernel. AGENTS.md in every generated project.
- Fresh architecture from the PRD every time — never replay a prior client's design.
- `output/<name>-<scope>/` is the canonical code output location. The vault (`Hari_Work/`) is the working dir.
