Run an engagement-scoped grilling session that captures glossary terms in `Engagement Context.md` and hard-to-reverse decisions in ADRs. Promotes terms to `Client Context.md` automatically when they apply to the client beyond just this engagement.

Engagement target: $ARGUMENTS
Format expected: "{ClientName} {EngagementSlug}" — e.g., "Menards 2026-Governance" or "Simon Property Group Lease-Document-Intelligence"
If $ARGUMENTS is empty, infer from current working directory or ask.

Read first:
- INDEX.md
- _System/skills/grill-with-context.md (the underlying skill spec — read in full)
- Clients/{ClientName}/Client Context.md
- Clients/{ClientName}/Engagements/{EngagementSlug}/Engagement Brief.md (or any *Engagement Brief.md with date prefix; if it exists)
- Clients/{ClientName}/Engagements/{EngagementSlug}/Engagement Context.md (if exists)
- Clients/{ClientName}/Engagements/{EngagementSlug}/docs/adr/*.md (if any exist)
- Any roadmap, business case, or deliverable in the engagement folder
- Practice material relevant to engagement type:
  - Advisory: Knowledge/Practice/AI-Strategic-Advisory/ADVISORY.md
  - Engineering: Knowledge/Practice/AI-Engineering/ENGINEERING.md

Then follow grill-with-context.md exactly. Key rules:
1. ONE question per turn, always with a recommended answer
2. Update `Engagement Context.md` INLINE the moment a term resolves — don't batch
3. AUTO-PROPOSE promotion to `Client Context.md` when a term is forever-true about the client (not engagement-specific)
4. Create `Engagement Context.md` and `docs/adr/` LAZILY — only when first entry is needed
5. ADR offered ONLY when ALL THREE: hard-to-reverse + surprising-without-context + real-trade-off
6. ADRs always engagement-scoped — never written to client level
7. Surface conflicts with existing Client Context, Engagement Context, ADRs, and practice standards
8. DO NOT treat calendar gaps in pre-sales engagements as staleness signals — pre-SOW pursuits naturally have multi-week file-level quiet periods

File locations:
- `Engagement Context.md` → `Clients/{ClientName}/Engagements/{EngagementSlug}/Engagement Context.md`
- ADRs → `Clients/{ClientName}/Engagements/{EngagementSlug}/docs/adr/{NNNN}-{slug}.md`
- Promoted terms → appropriate existing section in `Clients/{ClientName}/Client Context.md`

Format examples for all three are in `_System/skills/grill-with-context.md` — read them.

Final summary must include:
- Decisions reached
- Terms added/updated in `Engagement Context.md`
- Terms PROMOTED to `Client Context.md`
- ADRs created (with numbers)
- Any glossary debt — terms used but not resolved
