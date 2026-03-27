# Custom Copilot Agents

## Requirements Interrogator

**Invoke:** `@requirements-interrogator` in Copilot Chat (VS Code, JetBrains, or github.com)

**What it does:** Walks you through a structured 7-phase interrogation to extract comprehensive, testable requirements from a vague idea. It won't just take your answers — it will challenge assumptions, surface gaps, and push for specifics. At the end, it pushes the output directly to Azure DevOps and/or Confluence.

**When to use it:**
- You have a feature idea but haven't written requirements yet
- You need to turn a stakeholder request into engineering-ready specs
- You want to stress-test your thinking before writing a PRD or creating tickets
- You're scoping an epic and need to define boundaries

**What to expect:**
- ~15-25 minutes of back-and-forth conversation
- 7 phases: Scope → Context → Assumptions → Boundaries → Acceptance Criteria → Gaps → Delivery
- The agent will push back on vague answers — that's the point
- At the end, it delivers to your chosen destination:
  - **PRD → Confluence** — published as a page for stakeholder review
  - **User Stories → Azure DevOps** — created as work items with full ACs, linked to parent epics
  - **Both** — PRD to Confluence + stories to ADO, cross-linked

**How to start:**
```
@requirements-interrogator I need to spec out [your feature/change here]
```

**Tips for best results:**
- Have any existing docs, tickets, or context nearby — the agent can read files if you point it to them
- Be honest when you don't know the answer — "I don't know" is a valid response and surfaces real gaps
- If you want to skip ahead, say "skip to output" and it will produce the best doc it can from what you've discussed
- Say "challenge that" at any point to trigger extra pushback on your last answer

---

## Setup

### On github.com (no setup needed)
The MCP servers for Azure DevOps and Confluence are configured in the agent file. Set these repository variables and secrets:

**Repository variables** (Settings → Secrets and variables → Actions → Variables):
- `AZURE_DEVOPS_ORG` — your ADO organization name
- `AZURE_DEVOPS_PROJECT` — default project name
- `CONFLUENCE_URL` — e.g., `https://yourcompany.atlassian.net/wiki`
- `CONFLUENCE_USER` — your Confluence email
- `CONFLUENCE_SPACE_KEY` — default space key

**Repository secrets** (Settings → Secrets and variables → Actions → Secrets):
- `AZURE_DEVOPS_PAT` — Azure DevOps Personal Access Token (needs Work Items read/write scope)
- `CONFLUENCE_API_TOKEN` — Confluence API token (generate at https://id.atlassian.com/manage-profile/security/api-tokens)

### In VS Code (local MCP setup required)
The `mcp-servers` block in the agent file is ignored when running locally. You need to configure the MCP servers in your VS Code settings.

Add to your `.vscode/settings.json` or user settings:

```json
{
  "mcp": {
    "servers": {
      "azure-devops": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "@anthropic-ai/mcp-azure-devops"],
        "env": {
          "AZURE_DEVOPS_ORG": "your-org",
          "AZURE_DEVOPS_PROJECT": "your-project",
          "AZURE_DEVOPS_PAT": "your-pat"
        }
      },
      "confluence": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "@anthropic-ai/mcp-confluence"],
        "env": {
          "CONFLUENCE_URL": "https://yourcompany.atlassian.net/wiki",
          "CONFLUENCE_USER": "your-email",
          "CONFLUENCE_API_TOKEN": "your-token",
          "CONFLUENCE_SPACE_KEY": "your-space"
        }
      }
    }
  }
}
```

Once configured, the `azure-devops/*` and `confluence/*` tools will be available to the agent locally.

### Without MCP servers
The agent still works without ADO/Confluence configured — it will produce the full output in chat and offer to write it to a local file instead. The interrogation process is the same either way.
