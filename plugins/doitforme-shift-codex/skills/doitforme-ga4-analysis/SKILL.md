---
name: doitforme-ga4-analysis
description: Analyze Google Analytics 4 (GA4) acquisition, engagement, key events, and ecommerce through Shift MCP. Use for Google Analytics reports, traffic or revenue changes, landing page analysis, channel comparisons, and campaign measurement.
---

# Google Analytics 4 analysis

Use the connected Shift MCP tools for read-only Google Analytics analysis. Do not create or change properties, data streams, key events, audiences, or GTM tags as part of an analysis request.

## Resolve the scope

1. Run `get_connection_status` and verify Google Analytics is connected. If it is unavailable, explain how to connect it in Shift and stop provider calls.
2. Identify the property using available discovery tools. If several properties fit, ask which one to use. Never guess a property ID.
3. Use the user's requested dates and comparison. If omitted, use the last 30 complete days and the preceding 30 complete days. State the actual dates and the property timezone when available.
4. Identify the business question and any campaign, channel, country, device, landing page, or event filters. Keep user acquisition and session acquisition distinct.

## Query and analyze

1. Discover available reporting tools and supported dimensions and metrics. Prefer `ga_run_report`; only use `ga_execute_api_read` after confirming that a normal tool cannot cover the request. Use actual tool schemas, not invented parameters.
2. Select compatible dimensions and metrics for the question: acquisition and sessions, engagement, relevant key events, or ecommerce purchases and revenue where tracked. Start with a small aggregate report, then break down the largest changes.
3. Compare like-for-like periods. Show absolute changes and percentage changes; if the baseline is zero, mark percentage change as unavailable. Recompute rates from their numerator and denominator rather than averaging row percentages.
4. Respect report pagination and row limits. State whether totals cover all rows or only returned rows; do not rank a truncated set as the whole property.
5. Report any tool-provided sampling, thresholding, freshness, attribution, consent, or missing-data limitations. Do not infer that an absent event or empty response proves zero activity or broken tracking.
6. Keep GA4 key events and revenue distinct from ad-platform conversions and attributed revenue. Compare them only with clearly stated attribution, timezone, currency, and date differences. Ask before reading a different provider if that would expand the requested scope.
7. Separate observed changes from hypotheses. Recommend focused follow-up checks without claiming causation from a correlation.

## Return

- Property and filters used, dates, comparison, timezone and currency when available.
- A compact KPI table with current and prior values and changes.
- The most significant channel, campaign, landing page or device findings supported by the retrieved data.
- Up to three prioritized recommendations, with evidence and limitations.
- Any unavailable reports or tool errors; never fabricate data to fill a gap.

Do not print credentials or include personal user-level data in the report. A request to repair tracking or change configuration is a separate mutation workflow; read the current state, explain the proposed change, obtain any required confirmation and verify the saved result.
