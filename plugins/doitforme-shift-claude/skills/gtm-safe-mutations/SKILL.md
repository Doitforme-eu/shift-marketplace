# gtm-safe-mutations

Purpose: Apply Google Tag Manager changes safely across workspaces without ID mismatch, duplicate-name conflicts, or broken trigger bindings.

## When to use
- Any GTM change (create/update/pause/delete/publish)
- Any request mentioning workspace, version, tag firing, conversion tags, or sync errors

## Core rules
1. Resolve live baseline first.
2. Never reuse `tagId`/`triggerId` across workspaces.
3. Always run preflight checks before mutate.
4. Prefer name-based resolution, then capture IDs in the target workspace.
5. If duplicate-name conflict appears, update existing entity instead of creating a new one.
6. For toggle actions, mutate only `paused` state unless full payload is required.
7. If workspace is out-of-date or has sync conflicts, stop and resolve sync first.
8. Verify final state before asking for publish.

## Workflow

### 1) Baseline
- Identify account/container/workspace explicitly.
- Confirm whether changes must be based on current Live version.
- If user references a specific version, anchor changes to that baseline first.

### 2) Preflight
- Check workspace status (editable/submitted/out-of-date).
- Detect sync conflicts (name collisions, stale workspace notices).
- Resolve entity IDs by **name in target workspace** (not from another workspace/session).

### 3) Mutations
- Create only missing entities.
- If create returns duplicate-name error: switch to update path on existing entity.
- For tag trigger binding, ensure referenced trigger exists in same workspace.
- For pause/unpause, prefer minimal update to `paused` and preserve existing bindings.

### 4) Safety controls
- Disable or pause problematic Custom HTML tags before publish if they can render raw JS to DOM.
- Keep one active conversion tag per objective unless user explicitly wants multi-tag setup.

### 5) Verification
- Validate expected ON/OFF state for each target tag.
- Validate trigger conditions match requested URL/event.
- Confirm no unexpected router/debug tags are active.
- Provide concise “ready to publish” checklist.

## Error handling map
- `404 Not found or permission denied`: wrong workspace ID/entity ID scope; re-resolve by name in target workspace.
- `unknown trigger` / `enablingTriggerId`: trigger does not exist in that workspace; create/resolve trigger first.
- `duplicate name`: entity exists; switch to update existing.
- `workspace submitted` / `out of date`: create/fix in fresh editable workspace or re-sync first.

## Publish checklist
- Correct workspace selected
- Sync conflicts resolved
- Target tags/trigger states verified
- Preview on exact target URL/event
- Submit + Publish
