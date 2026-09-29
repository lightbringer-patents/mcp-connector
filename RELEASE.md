# Connector release and compatibility

Public discovery on 2026-09-29 returned service version 4.11.0, 23 tools and four prompts, including patent search, import and family refresh. The search and fetch schemas also describe meeting discovery and canvas/transcript retrieval. Protected-resource metadata was checked. Both 1.2.0 plugin packages built with identical skills trees; release-builder tests and strict validation of the packaged Claude manifest, marketplace and skills passed. These checks do not establish that authenticated production workflows or host installation acceptance cases have passed.

The connector metadata in `server.json` is prepared at version 4.11.0, matching the service's `serverInfo.version` observed on 2026-09-29. The registry schema defines `version` as the equivalent of the MCP `Implementation.version`, so a service release that changes the reported version is followed by a matching `server.json` update and registry publication. The [portable plugin](https://github.com/lightbringer-patents/agent-plugin) and [Claude plugin](https://github.com/lightbringer-patents/claude-plugin) are versioned separately; consult their manifests and release notes for package versions. Updating these repositories does not publish an MCP registry entry or establish host-directory availability.

## Package and registry publication

1. **Verify compatibility:** confirm the public tools, prompts, OAuth metadata and supported workflows against the acceptance checks below. Use a dedicated test account and synthetic material for authenticated checks, and record what was actually tested.
2. **Prepare the workflow packages:** keep the complete `skills/` trees identical between the portable and Claude packages. Follow the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for package validation, host submission and installation checks.
3. **Check connector metadata:** ensure `server.json` and the README describe the verified public interface, and that `version` equals the `serverInfo.version` returned by the service on `initialize`.
4. **Publish and verify each channel:** publish the connector metadata to the MCP registry and verify the resulting entry. Complete each host's separate submission and publication flow for the plugin packages. Never infer publication or host approval from a GitHub merge.

Connector metadata 4.11.0 was published on 2026-09-29 after registry validation and a production version check. The [4.11.0 registry entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.lightbringer%2Fconnector/versions/4.11.0) was independently verified as active and marked latest. Registry reads were intermittently timing out; a read timeout is not a reason to republish.

## Portfolio workflow compatibility

Package version 1.2.0 is being prepared with a `patent-portfolio` skill in [agent-plugin #6](https://github.com/lightbringer-patents/agent-plugin/pull/6) and [claude-plugin #5](https://github.com/lightbringer-patents/claude-plugin/pull/5). The skill requires `search_public_patents` for discovery, `import_patent` for saving publications, and `refresh_patent_family` for family updates. Public discovery on 2026-09-29 advertised all three tools and their input/output schemas. Package availability and service capability must be checked separately.

Before claiming authenticated portfolio workflow or host acceptance:

- Verify these tools under the relevant consent scopes: `search_public_patents` uses `mcp:read`; `import_patent` and `refresh_patent_family` use `mcp:write` and require organisation import permissions. Public discovery confirmed `openWorldHint: true` for all three tools; it does not verify account access.
- Exercise assignee discovery, pagination and empty results; single and portfolio imports; repeated imports and conflicts; and family updates for saved own patents. Use the portfolio cases in the [plugin distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md).
- Verify that reported saves, warnings and family update outcomes agree with the returned results. Family updates should not be described as updates to patent text, assets or legal status.
- Match `server.json` to the service version actually reported at release time. The prepared metadata version does not reserve a version for portfolio tools or establish their availability.

## Contract migration

| Previous MCP tool | Replacement |
|---|---|
| `list_inventions` | `list_innovations` |
| `get_invention` | `get_innovation` |
| `get_invention_template` | `get_innovation_template` |
| `create_invention` | `register_innovation` |
| `update_invention` | `update_innovation` |
| `submit_invention` | `request_patent_preparation` |
| `get_invention_feedback` | `start_innovation_feedback` |
| `check_task_status` | `get_task_status` |
| `validate_invention` | Removed; registration validates and saves in one request. |

The renamed tools have no compatibility aliases. Update installed skills and saved references, and refresh client tool discovery when migrating to this interface. The status input is `task_id`, not a legacy ticket; one task represents the complete feedback run. Preparation returns `outcome: requested | already_requested` without a task ID.

## Verification before release

- Discover the deployed tools and annotations with both read and read/write consent. The catalog observed on 2026-09-29 had 23 tools with developer feedback configured: 10 read-only and 13 non-read-only; without it, expect 22 tools: 10 read-only and 12 non-read-only. `get_task_status` is non-read-only because it persists refreshed status and findings, but remains available under read consent. The three portfolio tools advertise `openWorldHint: true`; all other tools advertise false. Verify annotations again when tools are added. Consent can reduce the visible set further.
- Search accessible meetings with `category: "meeting"` and fetch the returned `meeting:<id>`. Check canvas/transcript snapshots, speaker and timestamp information, unavailable content, and access restrictions.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and documented analysis names, including previously completed results. Unavailable findings must be reported through `findings_error`, rather than presented as an empty successful result.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.
- Verify `list_tasks` recovery and `delete_task` with write consent. Task reads require ownership and current access to the associated innovation; owners can delete their own task history after losing access to the innovation. Tasks and findings expire after 30 days, and reads do not extend retention. Deletion does not cancel analysis or withdraw a patent-preparation request.

These are release acceptance checks. The dated verification above records what has been checked; it does not establish host approval or registry publication.
