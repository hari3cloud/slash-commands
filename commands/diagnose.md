Diagnose a hard bug or performance regression using a disciplined six-phase loop.

Bug description: $ARGUMENTS
If $ARGUMENTS is empty, ask the user to describe the bug, expected behavior, and how to reproduce it (or admit they don't have a clean repro).

Read first:
- INDEX.md
- _System/skills/diagnose.md (the underlying skill spec)
- If working in a poc-builder workspace: read AGENTS.md, ENGINEERING.md context
- Knowledge/Practice/AI-Engineering/Quality-Gates/Self-Healing-Patterns.md

Then follow diagnose.md exactly. Six phases, in order:
1. Build a feedback loop (this is the skill — disproportionate effort here)
2. Reproduce — verify the loop captures the user's actual bug
3. Hypothesise — 3-5 ranked falsifiable hypotheses, show ranked list to user before testing
4. Instrument — one variable at a time, tagged debug logs ([DEBUG-xxxx])
5. Fix + regression test — write failing test first IF a correct seam exists; if not, document the absence
6. Cleanup + post-mortem — remove debug instrumentation, capture the lesson

Do NOT:
- Skip Phase 1 because "the bug is obvious"
- Generate a single hypothesis and start testing
- Leave [DEBUG-xxxx] instrumentation in the codebase
- Conflate "test passes now" with "bug is fixed" — re-run the original repro

When done, if the root cause is architectural, capture in:
- Knowledge/Architecture/Lessons-Learned/{Engagement}.md (append)
- An ADR in the engagement folder (if hard-to-reverse change is needed)
- Knowledge/Practice/AI-Engineering/Quality-Gates/Self-Healing-Patterns.md (if pattern is general)
