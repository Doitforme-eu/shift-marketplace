# Doitforme Shift for Codex

This private Codex plugin bundles Shift skills and a remote MCP server definition.

## User setup

1. Install the marketplace and plugin:

```bash
codex plugin marketplace add <path-or-repo-to-distribution/codex-plugin>
codex plugin add doitforme-shift@doitforme-shift-marketplace
```

2. When Codex prompts you to authenticate Shift, complete that step in the Codex UI.

3. If your Codex installation does not automatically wire the MCP server, add the `doitforme_shift` entry from [`.mcp.json`](./.mcp.json) to your Codex MCP configuration.

## Notes

- Codex should handle authentication during install or first use instead of requiring a shell environment variable.
- This plugin keeps using the shared Shift remote MCP backend at `https://backend.shift.doitforme.eu/mcp`.
