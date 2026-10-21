# Doitforme Shift for Claude Code

Marketing operations for Meta Ads, Google Ads, Google Analytics 4 (GA4), and Google Tag Manager (GTM), with ten bundled skills and the Shift remote MCP connection.

## Install

```bash
claude plugin marketplace add Doitforme-eu/shift-marketplace
claude plugin install doitforme-shift@doitforme-shift-marketplace
```

These commands target Claude Code. Other Claude clients have their own supported plugin or connector installation flow.

## Connect

1. Sign in to [Shift](https://shift.doitforme.eu/) and connect the providers you want to use.
2. Complete Shift authentication when your client prompts you. The server advertises OAuth discovery; each user authenticates their own account.
3. Ask Shift to check `get_connection_status` before using provider data.

The bundled [MCP configuration](./.mcp.json) points to `https://backend.shift.doitforme.eu/mcp`. If your client needs a manual connection, follow the current [Shift setup guide](https://shift.doitforme.eu/el/setup-guide/). Keep keys and tokens in your client's secure connection settings, outside the repository.

## Examples

- Review Meta Ads and Google Ads performance and identify wasted spend.
- Analyze GA4 acquisition, engagement, key events and ecommerce performance.
- Inspect a GTM workspace and validate conversion tags before publishing.

See the [marketplace README](../../README.md) for the full skill inventory, platform coverage, support, and public directory limitations. Available operations depend on connected providers, exposed MCP tools and account permissions.

## Publisher links

[Privacy policy](https://launchpad.doitforme.eu/en/data-protection-policy/) · [Terms of service](https://launchpad.doitforme.eu/en/terms-and-conditions/) · [Package support](https://github.com/Doitforme-eu/shift-marketplace/issues)

These are the published policy links used by the Shift website. The bundled icon is the website's [512 px app icon](https://shift.doitforme.eu/logo512.png).
