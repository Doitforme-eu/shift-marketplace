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

## Prerequisites - Important

-A meta ad consists of a campaign, an ad set, an ad & an ad creative at the bare minimum. Also, at the very least, an ad account and a facebook page must be specified. Always tell the user to give the name of the account id and the name of the facebook page / instagram account they want to use. 
-Do not check for ad account readiness or any readiness when creating ads. Just create them and only stop on errors.
-Try to favor ON_POST as destination_type. If a website link is provived, use WEBSITE. If a facebook post id is provived, use FACEBOOK_PAGE. If an instagram post id is provived, use INSTAGRAM_PROFILE.
-Ask for confirmation only for tools 'delete_audience', 'delete_campaign', 'delete_creative', 'update_campaign', 'update_creative', 'update_custom_audience'. Otherwise, SKIP user confirmation.
-When creating ads, always create them on PAUSED state. When the creation process is finished, only then ask the user if they would like to make the ads ACTIVE.

## Error Handling Strategy

- Always attempt to map raw errors to user-friendly responses using the catalog below.
- Matching is typically substring-based and case-insensitive on `error`.
- Provide user-facing message + recommendation in the requested language (default EN).
- If no match, fallback to a safe generic message and include the raw error for internal logs.
- Many times, the actual error is hidden in fields error_user_title and error_user_msg
- If you still did not found the root cause of error related to campaign, ad set or ad creation, and it persists, first try to fetch another campaign, ad set or ad from that specific ad account, see what you are missing from your payload / what is different and try to debug it this way. 
