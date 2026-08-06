Draft or regenerate a 3Cloud T&M estimator for: $ARGUMENTS

Step 1 — Load the estimator skill
Read _System/skills/estimator.md — it contains the full pipeline (extract →
enrich → reason → review → render → verify), the framework paths, and the
guardrails. Follow it exactly; do not estimate from memory.

Step 2 — Read the framework contract
Read /Users/harit/AI-Projects/claude-code-skills/3Cloud-Practice/Estimator/framework/README.md
for the file contracts and reasoning rules. If this path is unreadable in the
current session mode, report that and stop.

Step 3 — Run the pipeline
Execute the skill's steps 1-7 for $ARGUMENTS (a client name or workbook path).
If $ARGUMENTS names a partial mode (e.g. "extract only", "render only"), run
through that step and stop — the skill's Example invocations define these.
Key gates: the TSL tabs must already be ATLAS-filled; sizing quantities need a
source or a user question; the user reviews the estimate.yaml summary before
anything is rendered to xlsx.

Step 4 — Report
Per-feature hours, largest lines, calibration verdict, capacity check, the
falsifier, and where the bundle + rendered workbook were saved.
