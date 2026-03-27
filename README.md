# Custom Copilot Agents

## Requirements Interrogator

**Invoke:** `@requirements-interrogator` in Copilot Chat (VS Code, JetBrains, or github.com)

**What it does:** Walks you through a structured 7-phase interrogation to extract comprehensive, testable requirements from a vague idea. It won't just take your answers — it will challenge assumptions, surface gaps, and push for specifics.

**When to use it:**
- You have a feature idea but haven't written requirements yet
- You need to turn a stakeholder request into engineering-ready specs
- You want to stress-test your thinking before writing a PRD or creating tickets
- You're scoping an epic and need to define boundaries

**What to expect:**
- ~15-25 minutes of back-and-forth conversation
- 7 phases: Scope → Context → Assumptions → Boundaries → Acceptance Criteria → Gaps → Output
- The agent will push back on vague answers — that's the point
- At the end, you get either a PRD document or structured user stories (your choice)

**How to start:**
```
@requirements-interrogator I need to spec out [your feature/change here]
```

**Tips for best results:**
- Have any existing docs, tickets, or context nearby — the agent can read files if you point it to them
- Be honest when you don't know the answer — "I don't know" is a valid response and surfaces real gaps
- If you want to skip ahead, say "skip to output" and it will produce the best doc it can from what you've discussed
- Say "challenge that" at any point to trigger extra pushback on your last answer

**Output formats:**
- **PRD Sections** — problem statement, scope, requirements matrix, risks. Good for stakeholder reviews.
- **User Stories + ACs** — Given/When/Then acceptance criteria, INVEST validation. Good for engineering handoff.
