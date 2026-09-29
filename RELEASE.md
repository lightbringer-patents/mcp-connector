# Connector release and compatibility

Public discovery on 2026-09-28 returned service version 4.10.0, the 20-tool catalog and four prompts. The patent search, import and family-update tools were not listed. OAuth resource metadata, package builds and manifest validation were checked in an earlier review on 2026-09-15. These checks do not establish that authenticated production workflows or host installation acceptance cases have passed.

The connector metadata in `server.json` is prepared at version 4.10.0, matching the service's `serverInfo.version` observed on 2026-09-28. The registry schema defines `version` as the equivalent of the MCP `Implementation.version`, so a service release that changes the reported version is followed by a matching `server.json` update and registry publication. The [portable plugin](https://github.com/lightbringer-patents/agent-plugin) and [Claude plugin](https://github.com/lightbringer-patents/claude-plugin) are versioned separately; consult their manifests and release notes for package versions. Updating these repositories does not publish an MCP registry entry or establish host-directory availability.

## Package and registry publication

1. **Verify compatibility:** confirm the public tools, prompts, OAuth metadata and supported workflows against the acceptance checks below. Use a dedicated test account and synthetic material for authenticated checks, and record what was actually tested.
2. **Prepare the workflow packages:** keep the complete `skills/` trees identical between the portable and Claude packages. Follow the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for package validation, host submission and installation checks.
3. **Check connector metadata:** ensure `server.json` and the README describe the verified public interface, and that `version` equals the `serverInfo.version` returned by the service on `initialize`.
4. **Publish and verify each channel:** publish the connector metadata to the MCP registry and verify the resulting entry. Complete each host's separate submission and publication flow for the plugin packages. Never infer publication or host approval from a GitHub merge.

Connector metadata 4.9.0 was published on 2026-09-22 and remained the registry's latest entry on 2026-09-28. Publication of matching 4.10.0 metadata is outstanding. Verify both the service and registry again before publishing.

## Portfolio workflow compatibility

Package version 1.2.0 is being prepared with a `patent-portfolio` skill in [agent-plugin #6](https://github.com/lightbringer-patents/agent-plugin/pull/6) and [claude-plugin #5](https://github.com/lightbringer-patents/claude-plugin/pull/5). The skill requires `search_public_patents` for discovery, `import_patent` for saving publications, and `refresh_patent_family` for family updates. Public discovery on 2026-09-28 did not advertise these tools. Package availability and service capability must be checked separately.

The next workflow-package release, 1.3.0, adds read-only saved-portfolio review and verification after authorised updates. Structured saved-family overviews in `search` and `fetch` require separate verification. The dated discovery above does not verify these response fields. Acceptance must cover read-only portfolio pagination, member visibility, priority provenance, distinct counts, missing-link assessments, and readback after authorised updates.

Before documenting portfolio operations as available:

- Discover these tools and their schemas and annotations under the relevant consent scopes. Recount the catalog and update the README's annotation summary; the portfolio tools use external patent sources and are expected to advertise `openWorldHint: true`.
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

- Discover the deployed tools and annotations with both read and read/write consent. The catalog observed on 2026-09-28 had 20 tools with developer feedback configured: 9 read-only and 11 non-read-only; without it, expect 19 tools: 9 read-only and 10 non-read-only. `get_task_status` is non-read-only because it persists refreshed status and findings, but remains available under read consent. That catalog advertised `openWorldHint: false` throughout; verify annotations again when tools are added. Consent can reduce the visible set further.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and documented analysis names, including previously completed results. Unavailable findings must be reported through `findings_error`, rather than presented as an empty successful result.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.
- Verify `list_tasks` recovery and `delete_task` with write consent. Task reads require ownership and current access to the associated innovation; owners can delete their own task history after losing access to the innovation. Tasks and findings expire after 30 days, and reads do not extend retention. Deletion does not cancel analysis or withdraw a patent-preparation request.

These are release acceptance checks. The dated verification above records what has been checked; it does not establish host approval or registry publication.
