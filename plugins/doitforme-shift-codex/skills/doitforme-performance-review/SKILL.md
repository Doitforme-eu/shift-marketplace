---
name: doitforme-performance-review
description: Run a practical cross-provider performance review for this project. Use when the user asks for weekly/monthly review, KPI tracking, or account health summary.
---

Run a performance review for `$ARGUMENTS` (default: last 30 days).

## Workflow

1. Check connectivity with `get_connection_status`.
2. Pull only from connected providers:
- Meta: `meta_get_campaign_performance` or `meta_get_performance`
- Google Ads: `gads_execute_gaql` (campaign metrics query)
- Google Analytics: `ga_run_report`
- Mailchimp: `mailchimp_list_campaigns` + `mailchimp_get_campaign_report`
3. Normalize output into one scorecard (provider-separated).
4. Compare actuals vs stated KPI targets (if present in project docs).
5. Highlight:
- top performers
- underperformers
- suspected blockers
6. Recommend top 3 next actions with expected impact.

## Output format

Include:
- timeframe used
- per-provider scorecard table
- key findings
- action list ordered by impact and confidence
