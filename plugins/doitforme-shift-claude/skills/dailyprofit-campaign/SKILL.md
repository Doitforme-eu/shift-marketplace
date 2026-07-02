---
name: dailyprofit-campaign
description: Meta Ads campaign build and troubleshooting playbook for Doitforme workflows. Use when the user asks to create, update, validate, or debug Meta campaign structures and creatives.
---

# Doitforme Meta Campaign Playbook

Use this skill when the user asks to create, update, or troubleshoot Meta ads campaigns, ad sets, ads, or creatives.

## Prerequisites

- If the user wants Meta operations, first verify provider readiness with `get_connection_status` and `meta_health_check`.
- A valid Meta launch structure requires at minimum: account + campaign + ad set + ad + creative.
- Before creation, confirm:
  - target Meta ad account id (`act_...`)
  - page/identity details needed by the selected creative flow

## Creation defaults

- For net-new delivery objects, default to paused state first (`PAUSED`) unless the user explicitly requests publish.
- After successful creation, ask if the user wants to activate.

## Confirmation policy

Always ask explicit confirmation before destructive or high-impact tools:
- `meta_delete_campaign`
- `meta_delete_audience`
- `meta_delete_creative`
- `meta_update_campaign`
- `meta_update_custom_audience`
- `meta_update_creative`

## Error handling strategy

- Prefer user-friendly error summaries over raw traces.
- Check `error_user_title` and `error_user_msg` fields when present.
- If campaign/ad-set/ad creation fails repeatedly, compare payload assumptions with existing working entities from:
  - `meta_list_campaigns`
  - `meta_list_ad_sets`
  - `meta_list_ads`
- Include correlation id in final troubleshooting output.
