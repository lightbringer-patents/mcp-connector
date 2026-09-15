# Coordinating the innovation-tool release

Phaenix 12.5.0 and Altair 4.7.0 have been released. Public production discovery on 2026-09-15 confirmed Altair 4.7.0, the 20-tool catalog, four prompts and OAuth resource metadata. The connector metadata and both plugin packages are version 1.1.0. Updating these repositories does not publish an MCP registry entry or establish host-directory availability.

This release supersedes [connector PR #2](https://github.com/lightbringer-patents/mcp-connector/pull/2), which documents Altair 4.6.0 and its former tool names and annotations. Close that PR as superseded when releasing [connector PR #3](https://github.com/lightbringer-patents/mcp-connector/pull/3); it is no longer a prerequisite to merge.

## Release status and remaining order

1. **Server releases complete:** [Phaenix PR #4124](https://github.com/phaenixorg/phaenix/pull/4124) and [Altair PR #155](https://github.com/phaenixorg/altair/pull/155) are included in the released backends. Public discovery and local contract tests confirm the documented surface; authenticated production acceptance cases below require a dedicated test account and synthetic material.
2. **Release the workflow packages:** merge [agent-plugin PR #1](https://github.com/lightbringer-patents/agent-plugin/pull/1) and [claude-plugin PR #1](https://github.com/lightbringer-patents/claude-plugin/pull/1), both version 1.1.0, with identical skills trees. Follow the canonical plugin distribution guide for host submission and installation checks.
3. **Release the connector documentation:** mark [connector PR #3](https://github.com/lightbringer-patents/mcp-connector/pull/3) ready and merge it with the dated catalog verification. Supersede PR #2 as described above.
4. **Publish and verify:** publish MCP registry metadata 1.1.0 through the normal registry workflow, then verify the entry before updating the publication wording. The registry still listed 1.0.4 as latest on 2026-09-15. Complete each host's separate submission/publish flow for the plugin packages. Never infer publication from a GitHub merge.

Package builds, manifest validation and 195 targeted Altair tests passed during the 2026-09-15 surface review. These checks do not establish that authenticated production workflows or host installation acceptance cases have passed.

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

The renamed tools have no compatibility aliases. Update installed skills, saved references and client tool discovery together with the server release. The status input is `task_id`, not a legacy ticket; one task represents the complete feedback run. Preparation returns `outcome: requested | already_requested` without a task ID. Keep historical exports as historical records.

## Verification before release

- Discover the deployed tools and annotations with both read and read/write consent. Expect 20 tools with developer feedback configured: 9 read-only and 11 non-read-only; without it, expect 19 tools: 9 read-only and 10 non-read-only. `get_task_status` is non-read-only because it persists refreshed status and findings, but remains available under read consent. All tools advertise `openWorldHint: false`. Consent can reduce the visible set further.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and public analysis names, including cached results; deploy Phaenix rich-text rendering before Altair normalization. Unsupported results must produce `findings_error`, never raw editor JSON or an empty-success fallback.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.

These are release acceptance checks. The dated verification above records what has been checked; it does not establish host approval or registry publication.

Deployment dependency reference: the Phaenix registration validation, rich-text rendering, `AutomatedTask`/`AutomatedTaskJob` schema (type `INNOVATION_FEEDBACK`), task `createdAt` index, expiry checks, user-scrub cleanup, task routes, deletion token scope and daily authenticated cleanup job must precede Altair in any new environment. Verify `list_tasks` recovery and `delete_task` with write consent; ownership and current subject access apply to listing/reads, while an owner may delete their own history after losing subject access. Reads expire at 30 days from creation; the capped daily sweep physically deletes expired parents and child results. Aurora execution records retain their existing separate lifecycle. Public discovery alone does not verify database migrations or cleanup execution.
