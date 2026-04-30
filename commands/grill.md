Run an adversarial alignment grilling session on the topic in $ARGUMENTS (or, if empty, on the plan/document active in the current conversation).

Read first:
- INDEX.md
- Read the underlying skill spec: _System/skills/grill.md (in the vault, this is `~/AI-Projects/claude-code-skills/_System/skills/grill.md` via symlink)

Then follow grill.md exactly. Key rules:
1. ONE question per turn
2. Always provide a recommended answer with brief reasoning
3. Walk dependencies in order — foundational decisions first
4. Surface conflicts with existing context (Client Context, prior advisory, ADRs, practice standards)
5. When done: summarize decisions reached, open questions deferred, what changed about the original plan

Do NOT:
- Generate the deliverable
- Update CONTEXT.md or ADRs (use /grill-with-context for that)
- Soften disagreement to be polite
