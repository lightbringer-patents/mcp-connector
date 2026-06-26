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

17 tools — 11 read-only, 6 write:

| Tool | Type |
|---|---|
| `whoami` | read |
| `search` | read |
| `fetch` | read |
| `list_inventions` | read |
| `get_invention` | read |
| `get_invention_template` | read |
| `validate_invention` | read |
| `get_invention_feedback` | read |
| `list_reviews` | read |
| `get_review` | read |
| `check_task_status` | read |
| `create_invention` | write |
| `update_invention` | write |
| `submit_invention` | write |
| `respond_to_review` | write |
| `add_comment` | write |
| `reply_to_comment` | write |

## Registry

Published to the official MCP registry as [`com.lightbringer/connector`](https://registry.modelcontextprotocol.io/v0/servers?search=com.lightbringer/connector).

## Links

- **Homepage:** https://lightbringer.com
- **Product announcement:** https://www.lightbringer.com/product-updates/lightbringer-mcp-patent-management-inside-chatgpt-claude-and-cursor
- **Privacy policy:** https://lightbringer.com/about/legal/privacy-policy
