---
name: doitforme-write-ad-copy
description: Generate performance-oriented ad copy aligned to brand context and live account signals. Use when the user asks for headlines, descriptions, hooks, or campaign messaging.
---

Write ad copy for `$ARGUMENTS`.

## Workflow

1. Read local brand context first (`AGENTS.md` and any project briefs if present).
2. Pull live context from connected providers:
- Google Ads: `gads_execute_gaql` for current keyword/campaign signals
- Meta: `meta_get_campaign_performance` for winning themes
- GA: `ga_run_report` for landing-page and conversion context (if available)
3. Build copy options by intent:
- acquisition angle
- trust/proof angle
- urgency/value angle
4. Validate against platform constraints before finalizing.
5. Present options clearly and ask which variant to apply.

## Output requirements

Return at least:
- 5 headline options
- 5 description/body options
- 3 CTA options
- short rationale tied to observed performance signals

If no reliable live data is available, state that clearly and switch to hypothesis-based copy.
