Compose a defensible Azure reference architecture (diagram + decision record) using 3Cloud's Architecture-Selector and Microsoft's official Azure icons: $ARGUMENTS

Step 1 — Load the methodology
Read _System/skills/reference-architecture.md and follow its Step-by-Step Instructions exactly. That file is the authoritative specification for this command.

Step 2 — Detect mode
Based on $ARGUMENTS and filesystem state:
- If $ARGUMENTS contains --evolve, run in Evolve mode
- If $ARGUMENTS contains --review, run in Review mode (read-only)
- If $ARGUMENTS is a path to a PRD, use that as the brief
- If $ARGUMENTS contains --client followed by a client name, read Clients/<name>/Client Context.md
- If $ARGUMENTS is empty, look for docs/PRD.md in the current directory or enter Interactive mode

Announce the detected mode clearly at the start of the response. Never silently switch modes.

Step 3 — Load framework context
Read these files from the vault:
- Knowledge/Practice/AI-Engineering/ENGINEERING.md
- Knowledge/Practice/AI-Engineering/Agent-Factory/Architecture-Selector.md
- Knowledge/Practice/AI-Engineering/Agent-Factory/Azure-Icon-Map.md
- Knowledge/Practice/AI-Engineering/Agent-Factory/Layer-Canonical-Components.md
- Knowledge/Practice/AI-Engineering/Templates/Reference-Architecture.drawio
- Knowledge/Practice/AI-Engineering/Templates/Architecture-Decision-Record.template.md
- Knowledge/Practice/AI-Engineering/Templates/Evolution-Notes.template.md
- Knowledge/Practice/AI-Engineering/Templates/Component-Inventory.template.json

Step 4 — Ask Tier 1 strategic questions
The 5 anchor questions per the methodology. If the brief answers any of them, confirm the extracted answer rather than re-asking.

Step 5 — Run Architecture-Selector
Traverse all 10 decision trees with extracted requirements + Tier 1 answers. Capture the decision trail.

Step 6 — Ask Tier 2 disambiguation questions only where Architecture-Selector shows genuine equivalence
Never ask more than 4 Tier 2 questions. If more ambiguity exists, the brief isn't sharp enough and you should say so.

Step 7 — Build Component Decision Records
One CDR per component. Every CDR must have: poc_variant, prod_variant, production_gate (triggers + migration approach + rollback), at least 2 alternatives considered, at least 3 defense talking points, scale limits, when-not-to-use, Microsoft doc URLs with retrieved-on dates, regulatory flags, dependencies.

Step 8 — Resolve icons
Look up each component in Azure-Icon-Map. Base64-embed the SVG into the drawio mxCell. If a service isn't mapped, record icon_missing: true and use a placeholder; do not block.

Step 9 — Assemble artifacts
Clone Reference-Architecture.drawio and populate. Clone Architecture-Decision-Record.template.md and populate. Clone Component-Inventory.template.json and populate. In Evolve mode, also produce Evolution-Notes.

Step 10 — Write outputs
Write to <project>/architecture/:
- reference-architecture.drawio (Create) or reference-architecture-v{N}.drawio (Evolve; previous versions retained)
- reference-architecture.png (if drawio CLI available; graceful fallback to deferred if not)
- architecture-decision-record.md or architecture-decision-record-v{N}.md
- component-inventory.json or component-inventory-v{N}.json
- evolution-notes-v{FROM}-to-v{TO}.md (Evolve mode only)
- architecture-lifecycle.json (always current, never versioned)

Step 11 — Present summary and wait for acceptance
List produced artifacts. Summarize the architecture. List top decisions, deferred decisions, accepted risks, and any unmapped icons. Then ask for approval or revision.

Step 12 — On acceptance
If user says "accept", update architecture-lifecycle.json's current_version_approved to today's date. Remind them about --evolve and --review for future work.

Constraints
- Never generate architecture without running Architecture-Selector first
- Never overwrite previous version files in Evolve mode
- Never skip Tier 1 questions
- Never ask Tier 2 questions that Architecture-Selector already decides
- Never produce a CDR with fewer than 3 defense talking points
- Never produce a CDR without production_gate fields in poc_scoped stage
- Never replace functioning non-Microsoft stacks unless client explicitly requested replacement (multi-cloud principle)
- Never mention competing consulting firms in client-facing output
- Never hallucinate costs or SLAs — cite retrieved-on sources or mark "unknown"
- Announce the mode at the start of the response; do not switch modes silently
