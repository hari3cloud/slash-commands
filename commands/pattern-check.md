Surface prior architectural decisions on the topic: $ARGUMENTS

Step 1 — Read INDEX.md to confirm where architecture decisions live across the vault.

Step 2 — Scan for matching prior decisions
Search these locations for files mentioning the topic ($ARGUMENTS):
- Clients/*/Architecture-Decisions/*.md
- Knowledge/Architecture/Patterns/*.md (if any)
- Knowledge/Practice/AI-Engineering/Patterns/*.md (if any)

For each matching file, extract:
- Client name (from path or frontmatter)
- Date of the decision
- The decision (what was chosen)
- The reason (why)
- The tradeoff accepted (what was given up)
- Source file path

Step 3 — Produce the output in this exact format

```
Found {N} prior decisions on {topic}:

1. {Client} ({Month YYYY}) — chose {decision}.
   Reason: {one-sentence reason}.
   Tradeoff: {one-sentence tradeoff}.
   Source: {relative path}

2. {Client} ({Month YYYY}) — chose {decision}.
   Reason: {one-sentence reason}.
   Tradeoff: {one-sentence tradeoff}.
   Source: {relative path}

[... additional prior decisions ...]

Suggested discovery questions for the new customer:
  • {question 1 — inferred from the deciding factor in prior decision 1}
  • {question 2 — inferred from the deciding factor in prior decision 2}
  • {question 3 — inferred from any common tradeoff across the prior decisions}
```

Step 4 — Quality rules for the suggested questions
- Each question must be inferred from an actual *tradeoff* or *deciding factor* in the prior decisions — not generic discovery boilerplate
- Questions should surface the factor that made the prior decisions diverge
- If two prior decisions chose opposite things, the question is essentially "which factor applies here?"
- Maximum 3 questions; quality over quantity

Step 5 — Honesty rules
- If no prior decisions found, say so plainly: "No prior decisions found on {topic} in the vault. The new conversation will start cold."
- If only one prior decision found, note: "Only one prior decision found — limited diversity for inference. Treat as a single data point, not a pattern."
- Never invent prior decisions or extrapolate beyond what's in the files. Surface what's there.

Step 6 — Do not save output to a file. Print to terminal only. This is a retrieval skill, not a writing skill.
