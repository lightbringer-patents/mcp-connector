# Connector release and compatibility

Public discovery on 2026-10-09 returned service version 4.15.0, 33 tools and five prompts, including the three company-context tools. The `fetch` schema advertises `include`, `mode` and `cursor`, with `retrieval` in its output. Startup instructions mention company context and focused document reads. These observations verify anonymous discovery only; authenticated results, permissions and host installation remain separate acceptance checks.

The connector metadata in `server.json` is prepared at version 4.15.0; registry publication is pending. Its `version` must equal the `serverInfo.version` the service reports. The registry schema defines `version` as the equivalent of the MCP `Implementation.version`, so a service release that changes the reported version is followed by a matching `server.json` update and registry publication. The [portable plugin](https://github.com/lightbringer-patents/agent-plugin) and [Claude plugin](https://github.com/lightbringer-patents/claude-plugin) are versioned separately; consult their manifests and release notes for package versions. Updating these repositories does not publish an MCP registry entry or establish host-directory availability.

## Service 4.15.0 compatibility

- `get_company_context` and `get_company_context_template` use read consent. `update_company_context` requires write consent and moderator rights, accepts selected fields with the latest revision, and rejects stale revisions without changes. Notes replace the complete shared text; omitted fields remain unchanged and empty strings clear fields. Legal applicant-name updates do not rename established workspaces.
- `fetch` supports outline discovery and selected claims, description and abstract reads. Filtered pages provide source/revision attribution and completion information; continue with the same ID and selection until `has_more` is false. Stale cursors require restarting. Omitting options preserves the legacy full-content contract and available family metadata.
- Startup guidance distinguishes account identity from a shared company brief and describes focused reads. Transport, endpoint and OAuth configuration are unchanged.
- Portable 5.2.0 and Claude 1.5.0 are prepared separately with the company-context skill and focused-read guidance. Their review and publication status must be checked in the relevant host; live server tools do not establish skill installation.

## Service 4.14.0 compatibility

- General service instructions are available before sign-in. Clients can recover current identity and activity through tools after authorization; startup snapshots are optional. `whoami` supports optional organisation country, state and website, and an optional bounded member roster under read consent.
- Review schemas distinguish Strategy, report and document targets. `list_reviews` covers participant and creator reviews; `respond_to_review` accepts an optional message of up to 2,000 characters.
- `send_developer_feedback` requires write consent and explicit approval of the report fields and recipient disclosure. Reports contain only approved text, category and optional tool name, without automatic account or client metadata.
- Preparation, review-notification and developer-feedback tools advertise `openWorldHint: true`, alongside the three portfolio tools that use external patent sources. The README lists the complete annotation sets.
- MCP prompts are optional task starters. Startup instructions, individual tool contracts, live capture guides and installed skills have distinct roles; prompt titles make capture-and-save intent explicit.

## Package and registry publication

1. **Verify compatibility:** confirm the public tools, prompts, OAuth metadata and supported workflows against the acceptance checks below. Use a dedicated test account and synthetic material for authenticated checks, and record what was actually tested.
2. **Prepare the workflow packages:** keep the complete `skills/` trees identical between the portable and Claude packages. Follow the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for package validation, host submission and installation checks.
3. **Check connector metadata:** ensure `server.json` and the README describe the verified public interface, and that `version` equals the `serverInfo.version` returned by the service on `initialize`.
4. **Publish and verify each channel:** publish the connector metadata to the MCP registry and verify the resulting entry. Complete each host's separate submission and publication flow for the plugin packages. Never infer publication or host approval from a GitHub merge.

Connector metadata 4.14.0 was published on 2026-10-06 at 19:39:48 UTC after a production version check. Both the [exact-version entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.lightbringer%2Fconnector/versions/4.14.0) and the [latest entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.lightbringer%2Fconnector/versions/latest) returned version 4.14.0 with active status and `isLatest: true`; their complete server metadata matched the merged `server.json`. This confirms MCP registry publication; host-directory publication remains separate.

Connector metadata 4.13.1 was published on 2026-10-01 after registry validation and a production version check. On that date, the [latest registry entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.lightbringer%2Fconnector/versions/latest) independently returned version 4.13.1 with active status and `isLatest: true`; its server metadata matched the 4.13.1 release. The registry recorded publication at 15:15:21 UTC. A concurrent exact-version read timed out; the successful latest-entry read confirmed publication, so no repeat publication was needed.

Before 4.13.1, version 4.11.0 was published on 2026-09-29. Registry publication is separate from plugin host approval and publication.

## Portfolio workflow compatibility

The workflow packages include IP strategy authoring and saved-portfolio/family review; consult each package's release notes for its version and publication status. The portfolio workflow uses `search` and `fetch` for saved records, `search_public_patents` for public discovery, and authorised `import_patent` and `refresh_patent_family` writes. Structured family data is optional. Test pagination, member visibility, priority provenance, distinct counts, missing-link assessments and readback after updates before claiming authenticated acceptance.

From service version 4.12.0, the plain-text restriction on `search_public_patents` text inputs is enforced when the tool is called instead of being advertised as a JSON Schema `pattern`, so clients that do not support Unicode property escapes in schema patterns can load the tool. Accepted input is unchanged: provider query syntax is still rejected with a validation error.

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

- Discover the deployed tools and annotations with both read and read/write consent. The anonymous catalog observed on 2026-10-09 had 33 tools with developer feedback configured: 15 read-only and 18 non-read-only; without it, expect 32 tools: 15 read-only and 17 non-read-only. `get_task_status` is non-read-only because it persists refreshed status and findings, but remains available under read consent. Nine tools advertise `openWorldHint: true`, as listed in the README. Verify annotations again when tools change. Consent can reduce the visible set further.
- Verify company-context reads with read consent; update-tool absence under read-only consent; moderator checks; stale/current revision handling; complete notes replacement; omitted and empty fields; and applicant-name behavior. Use synthetic notes and verify readback after authorised changes. Strategy-only requests must not change shared context.
- Verify focused fetch on a seeded saved patent: outline without bodies, claims-only pages through completion, source attribution, continued blocks, stale cursors, empty/unavailable/unknown sections and import warnings. Content/cursor without `include` and outline with a cursor must fail. Oversized responses must not silently truncate. Preserve entity-prefixed IDs and verify unfiltered legacy content and family readback.
- Verify developer feedback is unavailable under read-only consent and requires explicit approval of its exact report fields and engineering-channel recipient disclosure. Check that account and client metadata are not added automatically, and that confidential content is excluded.
- Check review discovery as a participant and creator, distinct Strategy/report/document targets, and the 2,000-character response-message limit. Verify notification behavior and existing review permissions.
- Exercise Strategy capture and populated draft creation, revision conflicts and direct section edits, rejected edits touching pending review changes, and explicit publication/unpublication/deletion. Verify management permissions and that read-only or draft-only requests do not publish or perform unrelated writes.
- Search accessible meetings with `category: "meeting"` and fetch the returned `meeting:<id>`. Check canvas/transcript snapshots, speaker and timestamp information, unavailable content, and access restrictions.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and documented analysis names, including previously completed results. Unavailable findings must be reported through `findings_error`, rather than presented as an empty successful result.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.
- Verify `list_tasks` recovery and `delete_task` with write consent. Task reads require ownership and current access to the associated innovation; owners can delete their own task history after losing access to the innovation. Tasks and findings expire after 30 days, and reads do not extend retention. Deletion does not cancel analysis or withdraw a patent-preparation request.

These are release acceptance checks. The dated verification above records what has been checked; it does not establish host approval or registry publication.
