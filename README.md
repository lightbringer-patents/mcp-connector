# Lightbringer patent service through MCP

Work with [Lightbringer's patent service](https://lightbringer.com) from your AI assistant, where you research and build technical solutions. Register and enrich innovations, request patent preparation, and collaborate with the Lightbringer team on reports and patent drafts.

Lightbringer offers a full patent service with qualified patent attorneys on its team. Professional engagements include attorney advice, strategy assessment, novelty searches, freedom-to-operate (FTO) assessments, patent drafting, filing and prosecution. Lightbringer is the route to that professional work; automated analysis of an innovation description is not an attorney review, novelty search or FTO assessment.

> **Release status:** This branch documents the upcoming innovation-tool release. It depends on the matching [Phaenix task and registration changes](https://github.com/phaenixorg/phaenix/pull/4124), [Altair server changes](https://github.com/phaenixorg/altair/pull/155), [agent-plugin package](https://github.com/lightbringer-patents/agent-plugin/pull/1) and [claude-plugin package](https://github.com/lightbringer-patents/claude-plugin/pull/1). The new tool names are not a claim about the deployed endpoint. See [release coordination](RELEASE.md) before merging or publishing.

This is a metadata and documentation repository for a remote, hosted MCP server. Lightbringer operates the server; there is no server code to install or run from this repository.

## What you can do

- **Capture innovations where you work.** Identify ideas from an inventor conversation or authorised technical sources. Search for existing records, register new innovations, and enrich them over time without creating duplicates.
- **Use portfolio and strategy context.** Retrieve accessible records and strategy reports to guide exploration. Dedicated IP-strategy editing and trade-secret classification tools are not currently available; use the platform or continue with the Lightbringer team when needed.
- **Refine an innovation description.** Start automated feedback, follow its progress and read the findings. This checks the description rather than establishing novelty or patentability.
- **Request patent preparation.** Ask Lightbringer to prepare a selected innovation for patent filing. Registration alone does not initiate this workflow. An explicit preparation request does not require an automated feedback or revision cycle first.
- **Collaborate with your patent team.** Read reviews, attorney comments and proposed amendments; add comments, reply in discussions and respond to reviews using your existing permissions.

MCP access is available to Lightbringer platform subscribers, free or paid. Professional work is provided through separate paid engagements. The tools below define what can be initiated or consumed directly through MCP today; other service arrangements continue through the platform or Lightbringer team, including meetings where relevant. Agents cannot make payments.

## Connect and get started

Use a compatible MCP client such as ChatGPT, Claude, Claude Code or Cursor. Support for installing bundled skills varies by host; a connection to the MCP endpoint alone does not install skills.

| Setting | Value |
|---|---|
| Endpoint | `https://mcp.lightbringer.com/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 (Authorization Code + PKCE, S256), with Dynamic Client Registration |
| Scopes | `mcp:read`, `mcp:write` |

The server advertises its authorization server through RFC 9728 protected-resource metadata at `/.well-known/oauth-protected-resource`. Complete the host's sign-in flow and select the Lightbringer organisation to connect. Access is limited to that organisation and your existing permissions. Eligible users can enable MCP for an organisation during consent; other members can connect when MCP is already enabled. Follow the account or organisation remedy shown if connection is blocked. Never paste passwords or access tokens into chat.

### Install the skills with the connector

The MCP server supplies available tools, schemas and general service instructions. Skills guide the assistant through a workflow using those tools; they do not grant additional permissions or add server capabilities.

| Skill | How it uses MCP |
|---|---|
| `innovation-capture` | Explore authorised context, search for related innovations, register or update records, and optionally start automated feedback and follow its task. |
| `patent-preparation` | Resolve the selected innovation, preserve the user's explicit patenting intent, request preparation and explain the actual outcome. |
| `patent-review` | Read review artifacts, prepare sourced feedback, and post authorised comments or review responses. |

[agent-plugin](https://github.com/lightbringer-patents/agent-plugin) is the canonical skills package. [claude-plugin](https://github.com/lightbringer-patents/claude-plugin) mirrors its complete skills tree with Claude-specific metadata. These are separate distribution packages for the same service. See the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for supported installation routes, release dependencies and host verification. A repository or registry listing does not itself establish approval or availability in a host's plugin directory.

After connecting and installing the matching skills, try:

> Use Lightbringer's innovation-capture skill to identify potential innovations in the technical work we have discussed. Search for existing records, register new innovations and enrich relevant existing ones using supported facts. Preserve open questions and report anything that could not be saved. Do not request patent preparation; registration is the goal for now.

For a separate preparation request: “I want Lightbringer to prepare innovation X for patent filing.” For review work: “Help me respond to the patent team's review of X.”

### Connect an MCP client directly

For clients accepting this configuration format:

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

Other hosts use their own connector UI or transport label; follow the matching plugin package. General guidance is available through MCP when the workflow skills are not installed.

## Tools

The upcoming full production catalog contains 18 tools: 9 annotated read-only and 9 annotated non-read-only. The table uses `readOnlyHint`, not OAuth scope. Actual availability depends on consented scopes and configuration; without the optional developer-feedback delivery channel, there are 17 tools. Clients should discover their available tools through `tools/list`.

| Tool | Type | Purpose |
|---|---|---|
| `whoami` | read | Identify the connected user and organisation. |
| `search` | read | Find accessible innovation and document context. |
| `fetch` | read | Retrieve a record or document found through search. |
| `list_innovations` | read | Find existing innovations. |
| `get_innovation` | read | Read an innovation's current content. |
| `get_innovation_template` | read | Retrieve the structured registration template. |
| `list_reviews` | read | Locate report and patent-draft reviews. |
| `get_review` | read | Read review documents, comments and discussion. |
| `get_task_status` | read | Follow an automated feedback task using its `task_id`. |
| `list_tasks` | read | Recover owned task IDs and recorded status in the connected organisation; optional innovation filter and pagination. |
| `delete_task` | write | Delete an owned task and cached findings in any state, without cancelling execution. |
| `register_innovation` | write | Validate and save an innovation in one request. |
| `update_innovation` | write | Replace selected sections of an existing innovation. |
| `request_patent_preparation` | write | Request Lightbringer's patent-preparation workflow. |
| `start_innovation_feedback` | write¹ | Dispatch automated description analysis and return a task. |
| `respond_to_review` | write | Record an approve/request-changes response. |
| `add_comment` | write | Add a comment anchored to a review document passage. |
| `reply_to_comment` | write | Reply to an existing comment thread. |
| `add_discussion_comment` | write | Add a general review discussion comment. |
| `send_developer_feedback` | write² | Report tool problems or missing capabilities to Lightbringer. |

¹ Available under read consent; annotated non-read-only because it dispatches background analysis. It does not edit the innovation description.

² Available under read consent when its delivery channel is configured. Sends tool feedback to the Lightbringer engineering team through Slack.

The following tools carry `destructiveHint: true`: `delete_task`, `update_innovation`, `request_patent_preparation`, `respond_to_review`, `add_comment`, `reply_to_comment`, `add_discussion_comment`, and `send_developer_feedback`. All of those except `delete_task` and `update_innovation` also carry `openWorldHint: true`, reflecting outgoing notifications or feedback delivery. Other tools advertise both hints as false. Annotations describe behavior; they do not grant access.

## Registration, automated tasks and professional requests

**Registration saves a record.** `register_innovation` validates before saving. Validation errors mean nothing was registered; success returns the saved ID/link and any non-blocking warnings. Warnings do not mean registration failed. There is no separate MCP validation tool. Incomplete ideas that cannot satisfy the schema should remain explicitly pending registration rather than being filled with invented details.

**Automated feedback returns a task.** `start_innovation_feedback` returns one `task_id` for the complete feedback run, plus `status`, `progress` and per-analysis `results`. Use that same ID with `get_task_status`. Continue while `queued` or `running`; stop at `succeeded`, `partially_succeeded` or `failed`. Report successful findings alongside individual failures. The initial call returns after dispatch by default; ending a wait does not cancel the analysis.

**Patent preparation is a professional service request.** `request_patent_preparation` returns an innovation ID/link and `outcome: requested | already_requested`. It returns no task ID. The outcome confirms a new or existing preparation request, not completed preparation, a paid engagement, email delivery or a patent filing. `get_task_status` does not track preparation requests. Professional-service milestone tracking is not currently exposed through a dedicated MCP tool.

The four user-invoked MCP prompts are `draft-invention-disclosure`, `start-innovation-feedback`, `request-patent-preparation` and `summarize-my-reviews`. These are server-provided starting templates; the three plugin skills provide fuller workflow guidance.

Tasks and findings expire 30 days after creation; reading does not consume them or extend retention. Use `list_tasks`, optionally filtered by `invention_id`, to recover a lost task ID in the connected organisation. Follow `next_cursor` even if access filtering returns an empty page; listing reports recorded status without polling. `delete_task` permanently removes the user’s task and findings when requested, in any execution state, with write consent. Deletion does not cancel the analysis, delete the innovation or withdraw a service request.

## Registry and related repositories

The connector's MCP registry identity is [`com.lightbringer/connector`](https://registry.modelcontextprotocol.io/v0/servers?search=com.lightbringer/connector). `server.json` describes this remote connector; it does not bundle the workflow skills. Its version is separate from the plugin and server versions. The metadata on this branch is a release candidate, not evidence of registry publication.

- [Canonical skills and portable plugin](https://github.com/lightbringer-patents/agent-plugin)
- [Claude plugin and marketplace](https://github.com/lightbringer-patents/claude-plugin)
- [Homepage](https://lightbringer.com)
- [Product announcement](https://www.lightbringer.com/product-updates/lightbringer-mcp-patent-management-inside-chatgpt-claude-and-cursor)
- [Privacy policy](https://lightbringer.com/about/legal/privacy-policy)
