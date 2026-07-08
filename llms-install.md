# Cline Install Notes

Use this repository to connect Cline to the hosted VedaVisor Vedic Astrology MCP server.

## Requirements

- Node.js with `npx`
- A VedaVisor MCP API key from https://vedavisor.com/auth?mode=signup&redirect=%2Fmcp-api-keys

## Cline MCP Configuration

Add this server configuration in Cline's MCP Servers settings:

```json
{
  "mcpServers": {
    "vedavisor": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.vedavisor.com/mcp/",
        "--header",
        "Authorization: Bearer vv_live_YOUR_KEY"
      ]
    }
  }
}
```

Replace `vv_live_YOUR_KEY` with your actual VedaVisor MCP API key. Do not commit real API keys.

## Verification

After saving the configuration, open Cline's MCP Servers view and confirm that `vedavisor` appears as a configured server. The hosted endpoint is:

```text
https://mcp.vedavisor.com/mcp/
```

The server uses Streamable HTTP and bearer-token authentication supporting both API keys (`vv_live_` / `vv_test_` prefix) and Supabase OAuth JWTs.
