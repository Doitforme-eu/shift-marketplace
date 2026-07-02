---
name: doitforme-google-ads-pmax-playbook
description: Performance Max campaign construction and verification guide for Doitforme Google Ads operations. Use when building or optimizing PMax structures with safe defaults and completeness checks.
---

# Doitforme Google Ads PMax Playbook

Use this skill when the user asks to build or optimize a high-quality Performance Max campaign structure.

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
- Business name: at least 1
- Marketing images: at least 1 (prefer multiple)
- Square images: at least 1 (prefer multiple)
- Logo: at least 1 (prefer multiple)
- Portrait images/video/CTA when available

4. Handle EU/brand requirements:
- set required political declaration when needed.
- if account enforces brand guidelines, ensure business name + logo links exist.

5. Always verify after mutation:
- campaign delivery state
- targeting criteria
- asset counts by field type

## Recommended execution order

1. Create/update campaign in `PAUSED`.
2. Add geo + language targeting.
3. Build asset group with minimum required assets in one valid flow.
4. Enrich assets up to limits where possible.
5. Return concise changelog with IDs and counts.

## Output format

Return:
1. Campaign ID + name + status.
2. Asset group ID + status.
3. Targeting summary (locations/language).
4. Asset inventory by type.
5. Any limitations (e.g., missing YouTube assets).
