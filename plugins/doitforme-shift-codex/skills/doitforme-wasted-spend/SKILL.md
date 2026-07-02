---
name: doitforme-wasted-spend
description: Diagnose and reduce wasted media spend across connected providers. Use when the user asks where budget is leaking or wants optimization recommendations.
---

Run a wasted spend analysis for `$ARGUMENTS` (default: all connected ad providers).

## Workflow

1. Verify connected providers using `get_connection_status`.
2. Pull waste indicators:
- Meta: `meta_get_performance` (focus on spend, CTR, CPC, conversions by campaign/adset/ad)
- Google Ads: `gads_execute_gaql` (segments + search term/campaign-level inefficiency views)
- Mailchimp (if relevant): `mailchimp_get_campaign_report` for low engagement sends
3. Identify waste sources:
- high spend + low conversion
- poor CTR/click quality
- budget concentration in low-return entities
- broken tracking/readiness issues
4. Quantify potential savings where possible.
5. Propose concrete fixes and expected outcome.

## Guardrails

- Do not execute destructive changes without explicit user confirmation.
- If data quality is weak (missing conversion signals/tracking), report that as blocker #1.
