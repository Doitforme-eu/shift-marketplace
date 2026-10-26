---
name: doitforme-google-ads-pmax-playbook
description: Performance Max campaign construction and verification guide for Doitforme Google Ads operations. Use when building or optimizing PMax structures with safe defaults and completeness checks.
---

# Doitforme Google Ads PMax Playbook

Use this skill when the user asks to build or optimize a high-quality Performance Max campaign structure.

## Catalog and schema preflight

- Use the actual authenticated tool catalog and discovered input schema, not a remembered signature. API-key and saved legacy catalogs retain older schemas and fallback gateways; the new OAuth public catalog uses named tools only.
- Select the public typed workflow below only when the advertised `gads_create_performance_max_structure` schema exposes `asset_groups`, `validate_only` and the other inputs you intend to use. If only the old schema is present, use the legacy sequential workflow instead.
- Prefer advertised named tools. Use a legacy gateway only if the exact fallback tool is advertised and no named tool covers the operation: `gads_execute_api_read` / `gads_execute_api_mutate`. Keep its existing schema and permission limits.
- Report unsupported operations and the exact gap if neither path is available. Never guess hidden tool names. Never use a browser, website UI or generic web navigation to bypass an MCP gap.

## Objective

Create robust, launch-ready PMax structures with:
1. Safe defaults (`PAUSED` unless explicitly requested otherwise).
2. Complete assets (up to platform limits).
3. Correct geo/language targeting.
4. Clear verification output after changes.

## Required operating rules

1. Always confirm:
- `customer_id`
- `login_customer_id` (if MCC)
- campaign objective (`MAXIMIZE_CONVERSIONS` or `MAXIMIZE_CONVERSION_VALUE`)
- budget
- target geos and language

2. Never enable by default:
- keep campaign and asset group in `PAUSED` unless user explicitly says to publish.

3. Build completeness:
- Headlines: target up to 15
- Long headlines: target up to 5
- Descriptions: target up to 5
- Business name: at least 1 at the campaign/group scope required by the discovered branding mode
- Marketing images: at least 1 (prefer multiple)
- Square images: at least 1 (prefer multiple)
- Logo: at least 1 (prefer multiple)
- Portrait images/video/CTA when available

4. Handle EU/brand requirements according to the advertised schema:
- For the public typed workflow, obtain the advertiser's explicit `contains_eu_political_advertising` declaration (`CONTAINS_EU_POLITICAL_ADVERTISING` or `DOES_NOT_CONTAIN_EU_POLITICAL_ADVERTISING`). Never infer the political declaration from geography or assume a default.
- With `brand_guidelines_enabled=true` (the public workflow default), use campaign-scoped business name and logos. Supply `business_name`, `logo_asset_ids` or supported typed brand assets as the schema permits; do not duplicate campaign brand links on each group.
- Only when `brand_guidelines_enabled=false` is explicitly selected and supported, use group-scoped branding with business-name/logo assets for each applicable group. Legacy schemas may use a different branding contract; follow the discovered schema instead of appending new fields.
- Respect the schema's retail-only/Merchant Center eligibility and required asset checks; do not assume a retail-only exception applies to a non-retail group.

5. Always verify after mutation:
- campaign delivery state
- targeting criteria
- asset counts by field type

## Recommended execution order

### Public typed workflow (only when its schema is advertised)

1. Collect the complete approved structure, explicit EU declaration, target geos/languages, asset inventory and bidding objective. Supply exactly one existing `budget_id` or a new `daily_budget_amount_micros`, according to the discovered input schema.
2. Build one atomic typed `gads_create_performance_max_structure` request for the dependent campaign, budget when needed, targeting, asset groups, text assets and links. Do not split those dependent creates into separate public calls. Upload image/video assets separately through advertised media tools before referencing their IDs; do not pass local paths or binary data to the structure tool.
3. Run the complete request with `validate_only=true` first and `partial_failure=false`. Successful validation means nothing was created or changed; it is not an applied campaign. Keep campaign and groups paused unless the user explicitly authorizes enabling them.
4. After successful validation and required user confirmation, apply the approved payload with `validate_only=false` and `partial_failure=false`. The server constructs one atomic GoogleAdsService batch; partial failure is not supported for this dependent structure.
5. Inspect `validated_only`, `applied`, `resource_names`, `operation_results`, `partial_failure_error` and returned `created` IDs before claiming success. Verify the returned campaign/group state, targeting and branding through advertised read tools. A completed tool call alone is not proof that resources were created or are serving.
6. Do not retry the entire workflow after partial or uncertain outcomes. Inspect returned IDs/errors and re-read current state first; earlier media uploads or legacy steps may already have succeeded even when the later structure did not. Report confirmed changes separately from failures and unverified state.

### Legacy sequential workflow (only when the discovered schema is legacy)

The legacy schema is authoritative: do not append new arguments such as `asset_groups`, `validate_only`, `partial_failure`, or public branding fields when that schema does not expose them. Use any validation option only when actually advertised; do not claim a legacy multi-call workflow is atomic.

1. Create/update campaign in `PAUSED`.
2. Add geo + language targeting.
3. Build asset group with minimum required assets in one valid flow.
4. Enrich assets up to limits where possible.
5. Return concise changelog with IDs and counts. Inspect each step's actual results and verify existing IDs before retrying a partial build; preserve supported legacy tools and conditional gateways.

For optimization of an existing campaign or group, prefer advertised named editing tools over recreating the structure. If the discovered schema cannot express a required declaration or branding setting, report that gap rather than silently dropping it.

## Output format

Return:
1. Campaign ID + name + status.
2. Asset group ID + status.
3. Targeting summary (locations/language).
4. Asset inventory by type.
5. Any limitations (e.g., missing YouTube assets).
6. Validation-only versus applied outcome, returned IDs/errors, any partial or uncertain earlier steps, and what was actually verified. Do not describe a paused or merely validated campaign as live.
