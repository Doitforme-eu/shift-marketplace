# Doitforme Shift Marketplace

<img src="./plugins/doitforme-shift-codex/assets/icon.png" alt="Doitforme Shift" width="96" height="96">

Run marketing operations from **Codex** or **Claude Code** with Doitforme Shift: **Meta Ads, Google Ads, Google Analytics 4 (GA4), and Google Tag Manager (GTM)** in one MCP workflow.

[Shift website](https://shift.doitforme.eu/) · [Connect your accounts](https://shift.doitforme.eu/el/ads-mcp/) · [Setup guide](https://shift.doitforme.eu/el/setup-guide/)

## What you can do

| Platform | Workflows |
| --- | --- |
| Meta Ads / Facebook & Instagram Ads | Review campaign performance, investigate wasted spend, prepare campaign structures, troubleshoot creatives, and write ad copy. |
| Google Ads | Analyze campaign and search-term performance, investigate wasted spend, and build and verify Performance Max campaigns. |
| Google Analytics 4 | Analyze traffic acquisition, engagement, ecommerce and key events; compare periods and explain changes using connected property data. |
| Google Tag Manager | Inspect containers and workspaces, prepare tags and triggers, validate conversion tracking changes, and run the e-shop setup playbook. |

Available actions depend on the tools exposed by Shift, your connected providers, and your account permissions. The plugin checks the relevant account, property, or workspace before working. New campaigns default to paused; destructive or high-impact changes require confirmation. GA4 analysis is read-only unless you separately request a change.

## Install for Codex

```bash
codex plugin marketplace add Doitforme-eu/shift-marketplace
codex plugin add doitforme-shift@doitforme-shift-marketplace
```

For desktop clients, follow the [Codex plugin guide](https://developers.openai.com/plugins/build/plugins) for the supported marketplace installation flow. The native package is in [`plugins/doitforme-shift-codex`](./plugins/doitforme-shift-codex/).

## Install for Claude Code

```bash
claude plugin marketplace add Doitforme-eu/shift-marketplace
claude plugin install doitforme-shift@doitforme-shift-marketplace
```

The Claude package is in [`plugins/doitforme-shift-claude`](./plugins/doitforme-shift-claude/). These commands target Claude Code; other Claude clients have their own supported plugin or connector setup.

## Connect Shift

1. Sign in at [Shift](https://shift.doitforme.eu/) and connect the providers you want to use: Meta Ads and/or Google Ads, Google Analytics and Google Tag Manager.
2. Install the plugin for your client and complete Shift authentication when prompted. The remote server advertises OAuth discovery.
3. Ask Shift to check the connection status before your first provider task. If authentication is unavailable in your client, use the current [Shift setup guide](https://shift.doitforme.eu/el/setup-guide/) for its manual connection instructions.

The MCP endpoint is `https://backend.shift.doitforme.eu/mcp`. Each user authenticates their own connection. Never add API keys, access tokens, or provider credentials to this repository or a shared plugin package.

## Try these requests

- “Review Meta Ads and Google Ads performance for the last 30 days and suggest the three highest-priority actions.”
- “Analyze GA4 acquisition and ecommerce performance against the previous period. Show the property, dates and limitations used.”
- “Inspect my GTM workspace and check the GA4 and Google Ads conversion tags before publishing any changes.”
- “Find wasted spend across my connected ad accounts and explain the evidence.”

## Bundled skills

Both client packages include the same ten skills:

| Skill | Use it for |
| --- | --- |
| `doitforme-shift` | Shift connection checks and shared marketing workflow instructions |
| `dailyprofit-campaign` | Meta Ads campaign creation and troubleshooting |
| `doitforme-google-ads-pmax-playbook` | Google Ads Performance Max construction and verification |
| `doitforme-ga4-analysis` | Google Analytics 4 reporting and analysis |
| `doitforme-performance-review` | Cross-provider performance reviews |
| `doitforme-wasted-spend` | Wasted media spend investigation |
| `doitforme-write-ad-copy` | Performance ad copy |
| `gtm-safe-mutations` | Google Tag Manager workspace changes and verification |
| `gtm-eshop-quickstart` | GTM setup for an e-shop |
| `mailchimp-template-to-sections` | Email template preparation; live Mailchimp operations require available provider support |

## Public availability

This repository is a public, independently hosted marketplace. Anyone who can reach it can add it using the commands above. Adding a repository marketplace does **not** automatically list the plugin in the universal OpenAI directory or Anthropic's directory; those catalogs have separate submission and review processes. Installing the plugin also does not connect a user's advertising or analytics accounts automatically.

## Help

Use the [Shift setup guide](https://shift.doitforme.eu/el/setup-guide/) for account and connection help. Report package or installation problems in [GitHub Issues](https://github.com/Doitforme-eu/shift-marketplace/issues). Include the client version and error message, with credentials and private account data removed.

[Privacy policy](https://launchpad.doitforme.eu/en/data-protection-policy/) · [Terms of service](https://launchpad.doitforme.eu/en/terms-and-conditions/)
