Build one of the operator's OWN Azure-deployed products (decoupled from 3Cloud): $ARGUMENTS

This is the product sibling of /poc-builder. Read the full skill spec first:
`_System/skills/product-builder.md` — follow it exactly. It reuses the SAME three workflow
scripts (poc-frame, poc-build, poc-harden-harvest) with `profile: "product"` (not a fork).

Step 1 — Load context
Read INDEX.md first.
Read Knowledge/Practice/AI-Engineering/ENGINEERING.md and
Knowledge/Practice/AI-Engineering/workflows/README.md (workflow args/return contracts).
Set profile = "product", ProductName = $ARGUMENTS. Do NOT read Clients/ and do NOT look for a SOW.

Step 2 — Find the Project Brief (PRD)
Look for SPEC.md / docs/PRD.md / *-prd.md in the product repo, or ask the user.
If none exists, STOP — direct the user to author a project-brief.md product-PRD first.

Step 3 — Run the five gates (held in chat between workflows)
★ Gate 0  SET PRODUCT GOAL — distill + approve product-goal.md (frozen after approval).
★ Gate 1  FRAME — architecture + roster (poc-frame, profile:product, productGoalPath).
★ Gate 2  DESIGN — frozen design system (interactive, not a workflow).
★ Gate 3  BUILD — running app at localhost (poc-build, profile:product; Azure via CI/CD).
★ Gate 4  HARDEN+HARVEST — quality report + Goal-Traceability matrix; on approval, harvest
          to product repo docs + Products/_INDEX, deploy via the product Azure CI/CD template
          (GitHub Actions + OIDC), then emit the Phase-1→Phase-2 lifecycle handoff.

## Key Rules
- profile:"product" on every workflow call — never fork the scripts.
- productGoalPath threads into every workflow; the Goal Contract is read first by every agent.
- Deploy via .github/workflows/deploy.yml (OIDC) to your OWN Azure sub — NEVER azd up to a demo sub.
- Write back to the product repo + Products/_INDEX — NEVER Clients/Engagements.
- No Gate-4 pass while any success_criterion is GAP/UNVERIFIABLE without a recorded waiver.
- MAF for runtime orchestration — NEVER Semantic Kernel. Next.js frontend — never Streamlit.
