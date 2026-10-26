---
name: gtm-safe-mutations
description: Apply Google Tag Manager (GTM) changes safely across containers and workspaces. Use for tags, triggers, conversion tracking, workspace sync errors, validation, and publishing checks.
---

# gtm-safe-mutations

Purpose: Apply Google Tag Manager changes safely across workspaces without ID mismatch, duplicate-name conflicts, or broken trigger bindings.

## When to use
- Any GTM change (create/update/pause/delete/publish)
- Any request mentioning workspace, version, tag firing, conversion tags, or sync errors

## Catalog and permission boundaries

- Use the actual authenticated tool catalog and discovered input schema before choosing a read or mutation. API-key and saved legacy catalogs retain fallback gateways; the new OAuth public catalog uses named tools only. Do not assume a legacy signature applies to a same-named public tool.
- Prefer advertised named GTM list/get/create/update/delete tools. Preserve advertised legacy recipes with their discovered schemas. Use legacy gateway paths only if the exact fallback tool is advertised and the named tools do not cover the operation: `gtm_execute_api_read` / `gtm_execute_api_mutate`. Follow the advertised schema and actual granted permissions.
- Report unsupported operations and missing permissions precisely. Never guess hidden tool names. Never use a browser, website UI or generic web navigation to bypass an MCP gap.

Google tracking permission rules:
- Google Analytics access remains read-only (`analytics.readonly`): do not perform GA editing or request new OAuth scopes, even through an advertised legacy gateway.
- `tagmanager.edit.containers` permits workspace edits, not create-version, quick-preview or publish. Create-version and quick-preview require `tagmanager.edit.containerversions`; publication requires `tagmanager.publish`. Standard Shift login does not request these extra GTM scopes; do not request new OAuth scopes or try alternate endpoints.
- Discover current GTM CRUD input schemas; read current state and fingerprint before updating, preserve omitted fields, and treat supplied arrays as replacements. Stop and re-read on a stale, changed or missing fingerprint.
- Never claim preview succeeded without supported, authorized execution evidence. Workspace read-back is not browser/event verification or proof of publication. An advertised tool is not proof of provider permission.

- When an advertised version/publication tool or legacy gateway exists, use it only if the required scopes are already granted and the user explicitly confirms the high-impact action. Availability alone does not establish authorization; do not request new OAuth scopes or claim an alternate endpoint can bypass a denial.

## Core rules
1. Resolve live baseline first.
2. Never reuse `tagId`/`triggerId` across workspaces.
3. Always run preflight checks before mutate.
4. Prefer name-based resolution, then capture IDs in the target workspace.
5. If duplicate-name conflict appears, update existing entity instead of creating a new one.
6. For toggle actions, mutate only `paused` state unless full payload is required.
7. If workspace is out-of-date or has sync conflicts, stop and resolve sync first.
8. Verify staged state before considering publication; publication remains conditional on advertised capability, granted scopes, explicit confirmation and successful verification.

## Workflow

### 1) Baseline
- Identify account/container/workspace explicitly.
- Confirm whether changes must be based on current Live version.
- If user references a specific version, anchor changes to that baseline first.

### 2) Preflight
- Check workspace status (editable/submitted/out-of-date).
- Detect sync conflicts (name collisions, stale workspace notices).
- Resolve entity IDs by **name in target workspace** (not from another workspace/session).
- When advertised, use `gtm_get_live_version` / `gtm_get_version` for the baseline and `gtm_get_workspace_changes` for pending changes/conflicts; follow all returned list pages before concluding an entity is absent. If the live baseline cannot be read, report that limitation rather than inventing it.
- Read the current entity configuration and `fingerprint` before an update. If the fingerprint is stale, changed or missing, stop and re-read instead of forcing the write. Use the current schema's fingerprint field/check and preserve the existing legacy fingerprint path when only that schema is advertised.

### 3) Mutations
- Create only missing entities.
- If create returns duplicate-name error: switch to update path on existing entity.
- For tag trigger binding, ensure referenced trigger exists in same workspace.
- For pause/unpause, prefer minimal update to `paused` and preserve existing bindings.
- Follow the current schema-driven CRUD contract. If advertised, use `gtm_get_tag`, `gtm_create_tag`, `gtm_update_tag` and `gtm_delete_tag` (and corresponding trigger/variable tools); public updates use typed `changes`, while legacy recipes/gateways may require a different or complete payload. Do not mix the schemas.
- Supplied arrays replace the corresponding arrays in public `changes`; preserve omitted arrays/fields and existing trigger bindings. Re-read and compare after each mutation. Staged workspace CRUD does not publish a live version.

### 4) Safety controls
- Disable or pause problematic Custom HTML tags before publish if they can render raw JS to DOM.
- Keep one active conversion tag per objective unless user explicitly wants multi-tag setup.

### 5) Verification
- Validate expected ON/OFF state for each target tag.
- Validate trigger conditions match requested URL/event.
- Confirm no unexpected router/debug tags are active.
- Report a concise staged-change checklist, separately listing publication/preview as verified or BLOCKED. Reading back workspace configuration is not proof of live tag firing.
- Never claim preview succeeded or say the change is ready to publish when preview is unsupported, unauthorized, not run or failed. Stop at the exact gap and ask the user for the next step; do not open a browser to perform missing verification.

## Error handling map
- `404 Not found or permission denied`: wrong workspace ID/entity ID scope; re-resolve by name in target workspace.
- `unknown trigger` / `enablingTriggerId`: trigger does not exist in that workspace; create/resolve trigger first.
- `duplicate name`: entity exists; switch to update existing.
- `workspace submitted` / `out of date`: create/fix in fresh editable workspace or re-sync first.

## Publish checklist (conditional, not an instruction to publish automatically)
- Correct workspace selected
- Sync conflicts resolved
- Target tags/trigger states verified through advertised MCP reads
- Preview on the exact target URL/event only through an advertised, authorized MCP capability with actual validation evidence; otherwise mark BLOCKED, not previewed.
- Submit + Publish only after explicit user confirmation, an advertised tool with a supported input schema, already-granted `tagmanager.edit.containerversions` / `tagmanager.publish` permissions, and successful required verification. Follow any advertised `confirm_publish` input.
- If these conditions are not met, leave edits staged and report that they are not live. Preserve legacy publication functionality when all conditions are met; never use a gateway, alternate route or browser as a permission bypass.

Scope references: [create-version](https://developers.google.com/tag-platform/tag-manager/api/reference/rest/v2/accounts.containers.workspaces/create_version), [quick-preview](https://developers.google.com/tag-platform/tag-manager/api/reference/rest/v2/accounts.containers.workspaces/quick_preview), [publish](https://developers.google.com/tag-platform/tag-manager/api/reference/rest/v2/accounts.containers.versions/publish), [tag update and fingerprint](https://developers.google.com/tag-platform/tag-manager/api/reference/rest/v2/accounts.containers.workspaces.tags/update).
