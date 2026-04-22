Generate an AI roadmap for the client specified in $ARGUMENTS.

Step 1 — Read client context
Read Hari_Work/_System/INDEX.md first.
Then read Hari_Work/Clients/$ARGUMENTS/Client Context.md
If it doesn't exist, ask the user to provide: industry, AI maturity, key pain points, mission/vision, and constraints.

Step 2 — Select and read relevant knowledge
Based on the client context, read only the applicable notes:
- For strategy and portfolio: Hari_Work/Knowledge/AI-Strategy/Cornell/Notes/
- For governance and risk: Hari_Work/Knowledge/AI-Strategy/UPenn/Notes/
- For technical architecture: Hari_Work/Knowledge/Architecture/
Pull from multiple domains when the client needs both strategy and governance.

Step 3 — Generate the roadmap using this structure:
# {Client} — AI Roadmap
## Executive Summary
## Current State Assessment (use BCG/MIT 5 Org Behaviors as diagnostic)
## AI Portfolio Design (map initiatives to H1/H2/H3 and Defense/Offense)
## 12-Month Roadmap (Phase 1: Crawl 0-3mo, Phase 2: Walk 3-9mo, Phase 3: Run 9-18mo)
## Governance & Risk (apply risk taxonomy and three lines of defense)
## Capability Requirements (people, process, technology)
## Success Metrics
## Frameworks Applied
## Next Steps

Step 4 — Save the output
Save as: Hari_Work/Clients/$ARGUMENTS/{ClientName} AI Roadmap {YYYY-MM-DD}.md
Then update Hari_Work/_System/memory.md with the date and client name.

## Citation and Attribution Rules
IMPORTANT — these rules govern how frameworks are referenced throughout the roadmap:

- NEVER mention professor names, faculty names, or researcher names in the output
- NEVER say "According to Professor X" or "As Prof. Y argues"
- When citing a framework source, use institution name only: "Cornell framework", "UPenn framework", "BCG/MIT research", "Wharton research"
- In the Frameworks Applied section, list framework name + institution only — e.g. "Innovation Horizons Framework (Cornell)" not "Karan Girotra's Innovation Horizons"
- The roadmap is a 3Cloud client deliverable — it should read as 3Cloud's strategic thinking, informed by best-in-class frameworks, not as an academic paper

## Output Quality Rules
- Ground every recommendation in the client's actual context from Client Context.md
- Name specific stakeholders from the client context where relevant
- Keep executive summary to 3 sentences max — board-ready language
- Write for a CxO audience — no jargon, no academic hedging
- Flag any gaps in client context that would strengthen the roadmap
