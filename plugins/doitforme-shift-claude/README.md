# Doitforme Shift for Claude

This private Claude plugin bundles Shift skills and a remote MCP server definition.

## User setup

1. Add the private marketplace and install the plugin:

```bash
claude plugin marketplace add <path-or-repo-to-distribution/claude-plugin>
claude plugin install doitforme-shift
```

2. Complete authentication in the Claude UI when Shift prompts for it.

3. If needed, import or merge the MCP configuration from [`.mcp.json`](./.mcp.json) into your project or managed Claude MCP settings.

## Notes

- Claude Code is the recommended Claude client for this plugin.
- Claude Desktop and managed Cowork setups can use the same MCP config, but the distribution flow is usually admin-led.
