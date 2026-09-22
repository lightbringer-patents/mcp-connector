# Lightbringer MCP: Patent Management Inside ChatGPT, Claude, and Cursor

[Lightbringer](https://lightbringer.com) brings invention capture, patent strategy, and attorney collaboration directly into the AI tools deep tech companies already use to research, build, and commercialize breakthrough innovations — via the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP).

> **Note:** This is a metadata/documentation repository for a **remote, hosted** MCP server. The server is operated by Lightbringer at the endpoint below — there is no code to install or run locally.

## What it does

- **Invention documentation** — Create and refine patent disclosures through natural conversation with your AI assistant: summarize novelty, convert meeting notes into invention reports, and identify missing technical details.
- **Patent-focused analysis** — Unlike generic AI writing tools, Lightbringer analyzes your invention using patent-focused review criteria designed to improve clarity, completeness, technical depth, and overall disclosure quality.
- **Inventor–attorney collaboration** — Participate in patent workflows directly from your AI client: review attorney comments, answer questions, and manage discussions while attorneys work in Lightbringer.
- **Portfolio context** — Access organizational innovation context to identify related inventions across teams, surface prior work, and spot strategic gaps in patent coverage.

## Supported clients

ChatGPT · Claude · Cursor · Claude Code · and other MCP-compatible IDEs and AI agents.

## Connection

| | |
|---|---|
| **Endpoint** | `https://mcp.lightbringer.com/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 (Authorization Code + PKCE), with Dynamic Client Registration |
| **Scopes** | `mcp:read`, `mcp:write` |

Setup is a simple authorization flow that enables secure access to invention records within your organization's Lightbringer environment.

### Add to an MCP client

```json
{
  "mcpServers": {
    "lightbringer": {
      "type": "streamable-http",
      "url": "https://mcp.lightbringer.com/mcp"
    }
  }
}
```

## Tools

20 tools — 9 read-only, 11 write:

| Tool | Type | What it does |
|---|---|---|
| `whoami` | read | Returns the signed-in user and organisation |
| `search` | read | Searches the organisation's inventions, patent applications, and reviews |
| `fetch` | read | Returns the full content of a single document |
| `list_innovations` | read | Lists the innovations in the organisation |
| `get_innovation` | read | Returns an innovation record as markdown |
| `get_innovation_template` | read | Returns the current innovation registration template |
| `list_tasks` | read | Lists the user's automated feedback tasks |
| `list_reviews` | read | Lists reviews the user participates in |
| `get_review` | read | Returns a review with its document and comments |
| `register_innovation` | write | Registers a new innovation from a structured payload |
| `update_innovation` | write | Updates sections of an existing innovation |
| `request_patent_preparation` | write | Requests Lightbringer patent preparation for a registered innovation |
| `start_innovation_feedback` | write | Starts automated analysis of an innovation description |
| `get_task_status` | write | Returns the status, progress, and findings of an automated task |
| `delete_task` | write | Permanently deletes one of the user's automated tasks |
| `send_developer_feedback` | write | Sends a report about the tools to the Lightbringer engineering team |
| `respond_to_review` | write | Records an approve or request-changes decision on a review |
| `add_comment` | write | Adds a comment anchored to a passage of the reviewed document |
| `add_discussion_comment` | write | Posts a comment to the review's discussion feed |
| `reply_to_comment` | write | Replies to an existing comment thread |

Registering an innovation and requesting patent preparation are separate actions. Registration saves the innovation; a preparation request tells Lightbringer you want it prepared for patent filing. Automated feedback runs as a task: start it, then read its status and findings by task ID.

## Registry

Published to the official MCP registry as [`com.lightbringer/connector`](https://registry.modelcontextprotocol.io/v0/servers?search=com.lightbringer/connector).

## Links

- **Homepage:** https://lightbringer.com
- **Product announcement:** https://www.lightbringer.com/product-updates/lightbringer-mcp-patent-management-inside-chatgpt-claude-and-cursor
- **Privacy policy:** https://lightbringer.com/about/legal/privacy-policy
