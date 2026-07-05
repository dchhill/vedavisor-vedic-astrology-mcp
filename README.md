# VedaVisor Vedic Astrology MCP

Professional-grade Vedic astrology via Model Context Protocol. 22 tools: birth charts, dashas, yogas, compatibility, panchanga, muhurta, prashna, and chart-aware Cosmic Advisor interpretation. Free tier with no credit card required.

## Listing Details

| Field | Value |
|---|---|
| Name | `vedavisor-vedic-astrology-mcp` |
| Title | VedaVisor Vedic Astrology MCP |
| Status | created |
| Type | server |
| Tags | vedic-astrology, astrology, mcp-server, birth-chart, panchanga, dasha, muhurta, prashna, cosmology |
| Avatar URL | https://raw.githubusercontent.com/dchhill/vedavisor-vedic-astrology-mcp/main/logo.png |
| Author Name | VedaVisor |
| GitHub URL | [https://mcp.vedavisor.com](https://github.com/dchhill/vedavisor-vedic-astrology-mcp) |

## Server Config

Remote MCP clients:

```json
{
  "mcpServers": {
    "vedavisor": {
      "url": "https://mcp.vedavisor.com/mcp/",
      "headers": { "Authorization": "Bearer vv_live_YOUR_KEY" }
    }
  }
}
```

Cline / stdio MCP clients:

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
        "Authorization: Bearer ${VEDAVISOR_MCP_API_KEY}"
      ],
      "env": {
        "VEDAVISOR_MCP_API_KEY": "vv_live_YOUR_KEY"
      }
    }
  }
}
```

## About

VedaVisor brings classical Vedic astrology to AI agents through the Model Context Protocol. The server exposes 22 curated tools across 5 tiers, from basic birth chart calculation to in-depth Cosmic Advisor interpretation.

## 22 Tools

- Birth charts, daily panchanga, rashifal, numerology, Vimsottari dasha.
- Muhurta, yogas, compatibility (Ashtakoota Guna Milan), ashtakavarga, remedies, transits, Tajaka annual chart.
- All 16 varga charts (D-1 through D-60), all dasha systems.
- Cosmic Advisor, daily/weekly personalized horoscope, Varshaphala, Prashna horary, Karakas.
- White-label rights, high-volume rate limits, and dedicated support for enterprise use.

## Connection

Transport: HTTP Streamable HTTP (stateless)

Auth: Bearer token with `vv_live_` / `vv_test_` prefixed API keys

Rate limits: Tiered. Free supports 10 req/min and 1,000/day; Enterprise supports 600 req/min and 100k/day.

The only MCP server that reads your chart like a real astrologer, not a template.

## Links

- Website: https://mcp.vedavisor.com
- MCP endpoint: https://mcp.vedavisor.com/mcp/
- Signup: https://vedavisor.com/auth?mode=signup&redirect=%2Fmcp-api-keys

## Security Note

This repository contains public listing information and an example MCP client configuration only.
