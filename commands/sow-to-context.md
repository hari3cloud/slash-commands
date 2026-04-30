Extract client context from a 3Cloud Statement of Work for: $ARGUMENTS

Step 1 — Load the extraction skill
Read _System/skills/sow-to-context.md — this contains the full SOW → Client Context field mapping, extraction protocol, and template.

Step 2 — Find the SOW
Look for a .docx or .pdf file in the current directory, uploads, or ask the user to specify the SOW file path.

Step 3 — Read the SOW
Extract text from the SOW document. Focus on:
- Executive Summary (Section 1) — richest context
- Scope of Work (Sections 2-5) — what we're doing
- Deliverables (Section 6) — contractual outputs
- Out of Scope (Section 7) — critical boundaries
- Project Team (Section 10) — 3Cloud roles
- Responsibilities & Assumptions (Section 14) — what Client must provide
- Schedule (Section 15) — timeline
- Commercial (Sections 16-17) — pricing and funding
- Model/AI/GenAI clause (if present) — regulatory requirements

Step 4 — Generate Client Context.md
Follow the template in sow-to-context.md. Mark all fields that can't be extracted from the SOW with ⚠️ for enrichment during kickoff.

Step 5 — Generate Open Questions for Kickoff
For every ⚠️ field, create a specific question grouped by the workshop where it'll naturally be answered.

Step 6 — Save
Save to: Clients/$ARGUMENTS/Client Context.md
Report what was extracted and what needs enrichment.
