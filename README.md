# Lightbringer patent service through MCP

Work with [Lightbringer's patent service](https://lightbringer.com) from your AI assistant, where you research and build technical solutions. Register and enrich innovations, request patent preparation, and collaborate with the Lightbringer team on reports and patent drafts.

Lightbringer offers a full patent service with qualified patent attorneys on its team. Professional engagements include attorney advice, strategy assessment, novelty searches, freedom-to-operate (FTO) assessments, patent drafting, filing and prosecution. Lightbringer is the route to that professional work; automated analysis of an innovation description is not an attorney review, novelty search or FTO assessment.

This is a metadata and documentation repository for a remote, hosted MCP server. Lightbringer operates the server; there is no server code to install or run from this repository.

## What you can do

- **Capture innovations where you work.** Identify ideas from an inventor conversation or authorised technical sources. Search for existing records, register new innovations, and enrich them over time without creating duplicates.
- **Use existing context.** Retrieve accessible innovations, patents, strategy reports and meeting notes or transcripts to guide your work. Trade-secret classification is not exposed through the current tools.
- **Develop an IP strategy.** Follow the current capture guide, save a populated draft, and revise it using the current record revision. Publication and deletion are separate explicit actions and require appropriate permissions.
- **Refine an innovation description.** Start automated feedback, follow its progress and read the findings. This checks the description rather than establishing novelty or patentability.
- **Request patent preparation.** Ask Lightbringer to prepare a selected innovation for patent filing. Registration alone does not initiate this workflow. An explicit preparation request does not require an automated feedback or revision cycle first.
- **Collaborate with your patent team.** Read reviews, attorney comments and proposed amendments; add comments, reply in discussions and respond to reviews using your existing permissions.
- **Import portfolios and build patent families.** Find public publications by company name or publication number, import your own patents or third-party references, and group saved own patents into families.

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
| `patent-portfolio` | Review saved portfolios and families, discover and import publications, and verify authorised family updates. |
| `ip-strategy` | Build and revise an evidence-based Strategy using current capture guidance and revision-aware edits. |

See the [release and compatibility guide](RELEASE.md) for package status and verification results.

[agent-plugin](https://github.com/lightbringer-patents/agent-plugin) is the canonical skills package. [claude-plugin](https://github.com/lightbringer-patents/claude-plugin) mirrors its complete skills tree with Claude-specific metadata. These are separate distribution packages for the same service. See the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for supported installation routes, release dependencies and host verification. A repository or registry listing does not itself establish approval or availability in a host's plugin directory.

After connecting and installing the matching skills, try:

> Use Lightbringer's innovation-capture skill to identify potential innovations in the technical work we have discussed. Search for existing records, register new innovations and enrich relevant existing ones using supported facts. Preserve open questions and report anything that could not be saved. Do not request patent preparation; registration is the goal for now.

For a separate preparation request: “I want Lightbringer to prepare innovation X for patent filing.” For review work: “Help me respond to the patent team's review of X.”

For portfolio work: “Find publications filed under these company names and import our portfolio.” For family grouping: “Group these saved patents into families.”

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

The table describes each tool's purpose. Its read/write labels follow `readOnlyHint`, not OAuth scope. Availability depends on consented scopes, permissions and service configuration; clients should discover their available tools and schemas through `tools/list`.

| Tool | Type | Purpose |
|---|---|---|
| `whoami` | read | Identify the connected user and organisation, with optional country, state and website. Where supported, `include_members` also retrieves a bounded roster of fellow members with read consent. |
| `search` | read | Find accessible innovations, documents and meetings. |
| `fetch` | read | Retrieve a record, document or meeting snapshot found through search. |
| `search_public_patents` | read | Find public patent publications by assignee, keywords or complete publication number. |
| `list_innovations` | read | Find existing innovations. |
| `get_innovation` | read | Read an innovation's current content. |
| `get_strategy_template` | read | Retrieve the current Strategy capture guide, schema and example; requires Strategy management rights. |
| `list_strategies` | read | List accessible Strategies and their publication status. |
| `get_strategy` | read | Read Strategy sections and the current edit revision. |
| `create_strategy` | write | Save a populated draft Strategy. |
| `edit_strategy` | write | Apply direct section edits using the current revision, preserving unrelated content. |
| `set_strategy_publication` | write | Publish a Strategy or return it to draft when explicitly requested. |
| `delete_strategy` | write | Permanently delete a selected Strategy when explicitly requested. |
| `get_innovation_template` | read | Retrieve the structured registration template. |
| `list_reviews` | read | Locate report and patent-draft reviews. |
| `get_review` | read | Read review documents, comments and discussion. |
| `list_tasks` | read | Recover owned task IDs and recorded status in the connected organisation; optional innovation filter and pagination. |
| `delete_task` | write | Delete an owned task and cached findings in any state, without cancelling execution. |
| `register_innovation` | write | Validate and save an innovation in one request. |
| `import_patent` | write | Save a publication as an own patent or third-party reference. |
| `refresh_patent_family` | write | Update family grouping among saved own patents. |
| `update_innovation` | write | Replace selected sections of an existing innovation. |
| `request_patent_preparation` | write | Request Lightbringer's patent-preparation workflow. |
| `start_innovation_feedback` | write¹ | Dispatch automated description analysis and return a task. |
| `get_task_status` | write¹ | Refresh an automated feedback task using its `task_id` and save its status and available findings. |
| `respond_to_review` | write | Record an approve/request-changes response. |
| `add_comment` | write | Add a comment anchored to a review document passage. |
| `reply_to_comment` | write | Reply to an existing comment thread. |
| `add_discussion_comment` | write | Add a general review discussion comment. |
| `send_developer_feedback` | write² | Report tool problems or missing capabilities to Lightbringer. |

¹ Both tools are available under read consent and annotated non-read-only: `start_innovation_feedback` dispatches background analysis; `get_task_status` refreshes active analyses and saves their status and available findings. Neither edits the innovation description.

² Available under read consent when developer feedback is enabled. Sends tool feedback to the Lightbringer engineering team.

The following tools carry `destructiveHint: true`: `delete_strategy`, `edit_strategy`, `set_strategy_publication`, `delete_task`, `update_innovation`, `request_patent_preparation`, `respond_to_review`, `add_comment`, `reply_to_comment`, `add_discussion_comment`, and `send_developer_feedback`. Other tools advertise `destructiveHint: false`. `search_public_patents`, `import_patent` and `refresh_patent_family` advertise `openWorldHint: true` because they use external patent sources. The other tools advertise `openWorldHint: false`. Preparation and review actions can still send email or in-app notifications, and developer feedback is sent to Lightbringer's engineering team. Annotations describe behavior; they do not grant access.

## Workflow behavior

General service rules and workflow entry points are available before sign-in.
Startup identity and activity are optional snapshots: a client can finish
authorization without receiving another startup message. `whoami` retrieves
current identity and organisation details when needed; where its advertised schema
supports `include_members`, read consent also permits a roster of up to 20 fellow
members' names and roles, with a total count. A failed roster lookup is reported
separately from an empty roster. `list_innovations` and `list_reviews` retrieve
current accessible records. Detailed capture guidance comes from the template
tools, and installed skills provide fuller workflow practices.

**Registration saves a record.** `register_innovation` validates before saving. Validation errors mean nothing was registered; success returns the saved ID/link and any non-blocking warnings. Warnings do not mean registration failed. There is no separate MCP validation tool. Incomplete ideas that cannot satisfy the schema should remain explicitly pending registration rather than being filled with invented details.

**Automated feedback returns a task.** `start_innovation_feedback` returns one `task_id` for the complete feedback run, plus `status`, `progress` and per-analysis `results`. Use that same ID with `get_task_status`. Continue while `queued` or `running`; stop at `succeeded`, `partially_succeeded` or `failed`. Report successful findings alongside individual failures. The initial call returns after dispatch by default; ending a wait does not cancel the analysis.

**Patent preparation is a professional service request.** `request_patent_preparation` returns an innovation ID/link and `outcome: requested | already_requested`. It returns no task ID. The outcome confirms a new or existing preparation request, not completed preparation, a paid engagement, email delivery or a patent filing. `get_task_status` does not track preparation requests. Professional-service milestone tracking is not currently exposed through a dedicated MCP tool.

**Patent imports save publications.** Imports distinguish your own portfolio from third-party references and return saved-record links and processing warnings. Own-patent imports automatically attempt family grouping. A targeted family refresh can add missing relationships among saved own patents; it does not import more publications, remove existing relationships, or update patent text, assets or legal status.

**Saved-portfolio reviews use saved records.** Paginate `search` across application and patent categories and use `fetch` for selected records. Optional `family` overviews describe accessible saved members, jurisdictions, recorded statuses, priority provenance and coverage limits. Group by the current `groupingKey` and count families, applications and recorded publications separately, without counting one family's totals for each member. Keys can change, lists can be truncated and coverage remains unverified.

A per-record `family.refresh` assessment checks stored references without changing records. Known `missing_links` can justify an authorised refresh; unresolved references require investigation. Unknown data or old/null timestamps alone do not justify refresh. Review-only requests do not authorise writes. After authorised updates, fetch affected records again to verify the resulting family; if family data is absent, report that limit. See the [portfolio skill](https://github.com/lightbringer-patents/agent-plugin/tree/main/skills/patent-portfolio) for workflow details.

**Strategy dependencies follow the connected tools.** Start from organisation context and relevant saved records. `whoami` can supply missing organisation context; country, state and website are optional and may be absent on older connections. They do not classify the company or determine its first-filing office. Use relevant application-region metadata and the user's plans for filing context. Strategy capture depends on `list_strategies`, relevant `get_strategy` reads and `get_strategy_template`; the guide requires Strategy management rights. Saving additionally needs `create_strategy`, write consent and appropriate permissions. Missing tools or denied access are limitations, not evidence that no records exist. Retain proposed work and offer the platform when a required action is unavailable.

The [Strategy workflow dependency table](https://github.com/lightbringer-patents/agent-plugin/blob/main/skills/ip-strategy/references/mcp-workflow.md#tool-and-skill-dependencies) covers conditional handoffs to innovation capture, portfolio work and patent preparation. Those skills do not add server capabilities or authorise writes. The MCP registry entry supplies a connection, not installed skills; plugin availability and server tool availability must be checked separately.

**Strategy creation saves a draft.** Use `get_strategy_template` for capture and `get_strategy` before revision-aware edits. Replacements apply directly; conflicts require rereading and reconciliation, and edits intersecting pending review changes can be rejected. Creation and edits require Strategy management rights. Publication is a separate explicit action; multiple Strategies may be published at once. A saved Strategy does not itself import patents, request preparation or execute the actions written in it.

The five user-invoked MCP prompts are `draft-invention-disclosure`, `draft-strategy`, `start-innovation-feedback`, `request-patent-preparation` and `summarize-my-reviews`. These are optional task starters, not prerequisites for using tools. Capture prompt titles make their intent to save explicit. Startup instructions supply service-wide rules, tool descriptions define operation contracts, live capture guides own authoring procedures and schemas, and installed skills coordinate the wider workflow. Reading or making a targeted update to a saved Strategy does not require the capture guide.

Tasks and findings expire 30 days after creation; reading does not consume them or extend retention. Use `list_tasks`, optionally filtered by `invention_id`, to recover a lost task ID in the connected organisation. Follow `next_cursor` even if access filtering returns an empty page; listing reports recorded status without polling. `delete_task` permanently removes the user’s task and findings when requested, in any execution state, with write consent. Deletion does not cancel the analysis, delete the innovation or withdraw a service request.

## Registry and related repositories

The connector's MCP registry identity is [`com.lightbringer/connector`](https://registry.modelcontextprotocol.io/v0/servers?search=com.lightbringer/connector). `server.json` describes this remote connector; it does not bundle the workflow skills. Its `version` tracks the service's `serverInfo.version`, while plugin packages are versioned separately. Repository merges, registry publication and host-directory publication are separate steps. See the [release and compatibility guide](RELEASE.md) for version and publication status.

- [Canonical skills and portable plugin](https://github.com/lightbringer-patents/agent-plugin)
- [Claude plugin and marketplace](https://github.com/lightbringer-patents/claude-plugin)
- [Homepage](https://lightbringer.com)
- [Product announcement](https://www.lightbringer.com/product-updates/lightbringer-mcp-patent-management-inside-chatgpt-claude-and-cursor)
- [Privacy policy](https://lightbringer.com/about/legal/privacy-policy)
