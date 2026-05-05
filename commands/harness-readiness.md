Run a five-dimension harness-readiness assessment on a target — engagement, advisory engagement, the practice itself, or a specific codebase. Diagnostic only. Produces a structured report with severity-tagged gaps and a single overall recommendation.

Target: $ARGUMENTS

Argument shapes:
- "{ClientName} {EngagementSlug}" — assess a client engagement (e.g., "Menards 2026-Governance")
- "practice" — assess the AI Engineering practice itself
- "--target=path/to/codebase" — assess an explicit codebase path
- (empty) — infer from current working directory; ask if ambiguous

Read first, in this order:
1. INDEX.md
2. _System/skills/harness-readiness.md (the underlying skill spec — read in full)
3. Knowledge/Practice/AI-Engineering/Harness-Engineering-Foundations.md (vocabulary anchor — load before scoring)
4. Knowledge/Practice/AI-Engineering/Harness-Readiness/Assessment-Rubric.md (the load-bearing scoring document)
5. Knowledge/Practice/AI-Engineering/Harness-Readiness/Topology-Catalog.md (for Dimension 5)
6. Knowledge/Practice/AI-Engineering/ENGINEERING.md and Quality-Gates/Quality-Gate-Framework.md
7. For engagement targets: Clients/{ClientName}/Client Context.md, Engagement Context.md, Engagement Brief.md, ADRs, deliverables
8. For practice targets: the full skill list and pattern catalogue

Then score each of the five dimensions (Maintainability harness, Architecture fitness harness, Behaviour harness, Harnessability, Topology fit) on the 0–4 scale defined in the Assessment-Rubric.

Key rules:
1. Every score must trace to specific evidence found in the read files. Absence of evidence is score 0, not score 1 or 2.
2. The skill is BOUND to the rubric — cannot improvise tiers, merge tiers, or score "between tiers"
3. Severity tagging follows the rubric, not the skill's intuition
4. Cross-dimension synthesis runs AFTER per-dimension scoring — surface root-cause patterns
5. Pick exactly ONE overall recommendation from the controlled vocabulary; never equivocate
6. Surface the sales-evidence gap (Dim 3 below client-facing claims) plainly when found — DO NOT soften
7. The skill is DIAGNOSTIC ONLY — does not generate fixes, fitness functions, ground truth corpora, or behaviour harnesses; names them as recommended actions

Output format is in the skill spec — follow it exactly. Save the report to:
- `Clients/{ClientName}/Engagements/{Slug}/Harness-Readiness-Report-{YYYY-MM-DD}.md` for engagement targets
- `~/AI-Projects/claude-code-skills/3Cloud-Practice/AI-Engineering/Harness-Readiness/reports/` for practice-target assessments

The report's structure:
- Cross-Dimension Synthesis (if patterns surfaced)
- Per-Dimension Scores with rationale + gaps + recommended actions
- Overall Recommendation (exactly one from controlled vocabulary)
- Open Questions (if any)
- Reading Notes (sources read, honest industry-comparison framing)

Final summary in chat must include:
- The five dimension scores with one-line rationale each
- Cross-dimension patterns surfaced (or "none")
- The overall recommendation
- The path where the report was saved
