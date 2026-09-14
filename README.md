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
| **Auth** | OAuth 2.1 (Authorization Code + PKCE, S256), with Dynamic Client Registration |
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

The full production catalog in Altair 4.6.0 contains 19 tools — 10 annotated read-only and 9 annotated non-read-only. The table uses `readOnlyHint`, not OAuth scope: actual availability depends on consented scopes and server configuration. `send_developer_feedback` is optional; without its configured delivery channel, the catalog contains 18 tools.

| Tool | Type |
|---|---|
| `whoami` | read |
| `search` | read |
| `fetch` | read |
| `list_inventions` | read |
| `get_invention` | read |
| `get_invention_template` | read |
| `validate_invention` | read |
| `list_reviews` | read |
| `get_review` | read |
| `check_task_status` | read |
| `create_invention` | write |
| `update_invention` | write |
| `submit_invention` | write |
| `get_invention_feedback` | write¹ |
| `respond_to_review` | write |
| `add_comment` | write |
| `reply_to_comment` | write |
| `add_discussion_comment` | write |
| `send_developer_feedback` | write² |

¹ Available under read consent; annotated non-read-only because it dispatches background patent analysis. It never edits content.
² Available under read consent when its delivery channel is configured. Sends feedback about the tools themselves to the Lightbringer engineering team through Slack.

The following tools carry `destructiveHint: true`: `update_invention`, `submit_invention`, `respond_to_review`, `add_comment`, `reply_to_comment`, `add_discussion_comment`, and `send_developer_feedback`. All of those except `update_invention` also carry `openWorldHint: true`, reflecting outgoing notifications or feedback delivery. Other tools advertise both hints as false.

These annotations describe tool behavior; they do not grant access. The consented scopes and the user's existing permissions determine what the connection can do.

## Registry

Published to the official MCP registry as [`com.lightbringer/connector`](https://registry.modelcontextprotocol.io/v0/servers?search=com.lightbringer/connector).

## Links

- **Homepage:** https://lightbringer.com
- **Product announcement:** https://www.lightbringer.com/product-updates/lightbringer-mcp-patent-management-inside-chatgpt-claude-and-cursor
- **Privacy policy:** https://lightbringer.com/about/legal/privacy-policy
