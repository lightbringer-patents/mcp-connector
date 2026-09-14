# Coordinating the innovation-tool release

This connector change is a release candidate. Keep its PR in draft until the corresponding Phaenix and Altair changes have reached production and the matching plugin packages are ready. Updating this repository does not deploy the MCP server or publish a registry entry.

The immediate annotation correction is independent: [connector PR #2](https://github.com/lightbringer-patents/mcp-connector/pull/2) corrects the existing release's documentation and can merge first.

## Dependencies and order

1. Deploy [Phaenix PR #4124](https://github.com/phaenixorg/phaenix/pull/4124), including registration validation, the `AutomatedTask`/`AutomatedTaskJob` (type `INNOVATION_FEEDBACK`) schema and task routes. Confirm the matching MCP consent/auth changes are deployed too.
2. Deploy [Altair PR #155](https://github.com/phaenixorg/altair/pull/155) through the normal release flow. Confirm the new tool and task contracts against that deployment with a dedicated test account and synthetic material.
3. Release the matching [agent-plugin PR #1](https://github.com/lightbringer-patents/agent-plugin/pull/1) and [claude-plugin PR #1](https://github.com/lightbringer-patents/claude-plugin/pull/1) packages in coordination with the server. Both candidate packages are version 1.1.0. Follow the canonical plugin distribution guide for host submission and installation checks.
4. Confirm this README matches the deployed catalog, update the release-status notice with the verified release/date, and merge this connector update. The candidate MCP registry metadata version is 1.1.0, independently versioned from the server and plugins. Publish it through the normal registry workflow, then verify the registry entry before changing the publication wording. Never infer publication from a GitHub merge.

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

- Discover the deployed tools and annotations with both read and read/write consent. Expect 20 tools with developer feedback configured, or 19 without it, for the full production catalog. Consent can reduce the visible set further.
- Verify registration succeeds with a saved ID/link and warnings when appropriate; invalid payloads do not create records. Do not call registration to perform a validation-only request.
- Start automated feedback and follow the same task ID through success, partial success and failure. Verify that successful findings survive sibling failures. Both tools must return readable `title`/`description` findings and public analysis names, including cached results; deploy Phaenix rich-text rendering before Altair normalization. Unsupported results must produce `findings_error`, never raw editor JSON or an empty-success fallback.
- Request preparation for an explicitly selected innovation. Check both `requested` and `already_requested`; do not interpret either as completed filing or poll the innovation ID as a task.
- Build the portable and Claude packages using agent-plugin's release script; verify the complete skills trees are identical and that the skills are available in each target host.
- Compare registry and plugin endpoint, service positioning, tool names and scope guidance with the deployed server. Read-only hints and consent scopes describe different properties.

These are release acceptance checks, not claims that deployment, host approval or registry publication has occurred.

Task retention and recovery: deploy the task `createdAt` index, expiry checks, user-scrub cleanup, task listing/deletion routes, deletion token scope and daily authenticated cleanup job before Altair. Verify `list_tasks` recovery and `delete_task` with write consent; ownership and current subject access apply to listing/reads, while an owner may delete their own history after losing subject access. Reads expire at 30 days from creation; the capped daily sweep physically deletes expired parents and child results. Aurora execution records retain their existing separate lifecycle.
