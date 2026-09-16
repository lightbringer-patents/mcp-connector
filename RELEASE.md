# Connector release and compatibility

Public discovery on 2026-09-15 confirmed the Lightbringer MCP service's 20-tool catalog, four prompts and OAuth resource metadata. Package builds and manifest validation passed during that review. These checks do not establish that authenticated production workflows or host installation acceptance cases have passed.

The connector metadata in `server.json` is version 1.1.0. The [portable plugin](https://github.com/lightbringer-patents/agent-plugin) and [Claude plugin](https://github.com/lightbringer-patents/claude-plugin) are versioned separately; consult their manifests and release notes for package versions. Updating these repositories does not publish an MCP registry entry or establish host-directory availability.

## Package and registry publication

1. **Verify compatibility:** confirm the public tools, prompts, OAuth metadata and supported workflows against the acceptance checks below. Use a dedicated test account and synthetic material for authenticated checks, and record what was actually tested.
2. **Prepare the workflow packages:** keep the complete `skills/` trees identical between the portable and Claude packages. Follow the [distribution guide](https://github.com/lightbringer-patents/agent-plugin/blob/main/DISTRIBUTION.md) for package validation, host submission and installation checks.
3. **Check connector metadata:** ensure `server.json` and the README describe the verified public interface and intended connector version.
4. **Publish and verify each channel:** publish the connector metadata to the MCP registry and verify the resulting entry. Complete each host's separate submission and publication flow for the plugin packages. Never infer publication or host approval from a GitHub merge.

The registry listed 1.0.4 as latest during verification on 2026-09-15. This dated observation does not establish its current status; verify the listing before claiming that connector metadata 1.1.0 is published.

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

- Discover the deployed tools and annotations with both read and read/write consent. Expect 20 tools with developer feedback configured: 9 read-only and 11 non-read-only; without it, expect 19 tools: 9 read-only and 10 non-read-only. `get_task_status` is non-read-only because it persists refreshed status and findings, but remains available under read consent. All tools advertise `openWorldHint: false`. Consent can reduce the visible set further.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and documented analysis names, including previously completed results. Unavailable findings must be reported through `findings_error`, rather than presented as an empty successful result.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.
- Verify `list_tasks` recovery and `delete_task` with write consent. Task reads require ownership and current access to the associated innovation; owners can delete their own task history after losing access to the innovation. Tasks and findings expire after 30 days, and reads do not extend retention. Deletion does not cancel analysis or withdraw a patent-preparation request.

These are release acceptance checks. The dated verification above records what has been checked; it does not establish host approval or registry publication.
