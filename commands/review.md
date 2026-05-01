Run a multi-lens review pass over a deliverable (code, advisory document, or spec). Each lens is a focused single-purpose review; output is a ranked list of concerns by severity and location.

Target: $ARGUMENTS
If $ARGUMENTS is empty:
- Look at the most recent file modifications in the current working directory
- Check `git log --oneline -n 5` for recent commits
- Ask the user which deliverable to review if ambiguous

Read first:
- INDEX.md
- _System/skills/review.md (the underlying skill spec — read in full)
- The target deliverable itself
- For code targets: relevant `Knowledge/Architecture/Patterns/` and `Knowledge/Practice/AI-Engineering/ENGINEERING.md`
- For advisory targets: `Knowledge/Practice/AI-Strategic-Advisory/ADVISORY.md`, the engagement's `Client Context.md`, `Engagement Context.md`, and any prior ADRs

Then run the six default lenses (or the subset specified via `--lenses=`):
1. Correctness — does it do what it should
2. Security — auth, secrets, injection, data egress
3. Performance & Cost — complexity, Azure cost, latency
4. Simplicity — abstractions earning their cost, YAGNI
5. Convention & Pattern Fit — matches established patterns and standards
6. Maintainability — names, magic numbers, implicit knowledge

Key rules:
1. Each concern reports severity (critical/high/medium/low), location (file:line or section), one-sentence concern, and recommended action for critical/high
2. Run each lens INDEPENDENTLY — don't cross-contaminate
3. Stay terse — dense not eloquent
4. Surface cross-lens patterns at the end (e.g., one root cause showing up under three lenses)
5. Recommend next action at the bottom: ship as-is / fix criticals then ship / re-architect / discard

Do NOT:
- Generate the fix itself (review only — user decides remediation)
- Comment on style preferences (that's lint, not review)
- Run all six lenses if user specified `--lenses=` subset
- Soften concerns with hedges — state plainly, severity does the softening
- Ignore the engagement's `Engagement Context.md` and ADRs — those are ground truth for what's deliberate

Output format is in the skill spec — follow it exactly. Use the markdown table format under each lens heading, and finish with a Summary block listing must-fix / should-consider / defer-or-accept counts plus a recommended next action.
