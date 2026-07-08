# VedaVisor Vedic Astrology MCP

Professional-grade Vedic astrology via Model Context Protocol. 22 tools across 5 tiers: birth charts, dashas, yogas, compatibility, panchanga, muhurta, prashna, and chart-aware Cosmic Advisor interpretation. Free tier with no credit card required. The only MCP server that reads your chart like a real astrologer, not a template.

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
| GitHub URL | https://github.com/dchhill/vedavisor-vedic-astrology-mcp |

## Server Config

Remote MCP clients (Claude Desktop, Cursor, VS Code, any MCP-compatible client):

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

Cline / stdio MCP clients (via `mcp-remote` bridge):

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

Replace `vv_live_YOUR_KEY` with your actual MCP API key from the VedaVisor dashboard.

## 22 Tools Across 5 Tiers

### Sadhaka (Free, No Credit Card) — 7 tools
`calculate_birth_chart`, `calculate_daily_panchanga`, `get_daily_rashifal`, `calculate_full_numerology`, `calculate_vimsottari_dasha`, `resolve_place`, `get_weekly_transit_events`
- 10 req/min, 1,000/day (2x VedAstro)

### Jyotishi ($1/mo) — 7 additional tools (14 total)
`find_auspicious_times`, `calculate_yogas`, `calculate_compatibility`, `calculate_ashtakavarga`, `recommend_remedies`, `calculate_transits`, `calculate_tajaka`
- 15 req/min, 500/day. Same price as VedAstro, 3x the tools.

### Acharya ($9/mo) — 2 additional tools (16 total)
`calculate_varga_charts`, `calculate_all_dashas`
- 30 req/min, 1,000/day. All 16 divisional charts plus every dasha system.

### Siddha ($29/mo) — 6 additional tools (22 total), includes Cosmic Advisor interpretation
`ask_cosmic_advisor`, `daily_horoscope`, `get_weekly_horoscope`, `calculate_varshaphala`, `ask_prashna`, `calculate_karakas`
- 60 req/min, 5,000/day, 300 generation credits/month (~100 Cosmic Advisor calls or 300 daily horoscopes).

### Jyotish Guru ($99/mo) — All 22 tools + white-label
- 600 req/min, 100,000/day, 2,000 generation credits/month, unattributed output, dedicated support + SLA.

## Generation Credits

Advisor-backed tools consume credits from a monthly pool (per subscription, shared across all API keys):

| Tool | Credits per call |
|------|-----------------|
| `ask_cosmic_advisor` | 3 |
| `get_weekly_horoscope` | 2 |
| `daily_horoscope` | 1 |

Credits reset on the 1st of each month. Exhausted credits return a ToolError with upgrade guidance.

### Credit Packs (Sadhaka/Jyotishi users)
One-time purchase to access Cosmic Advisor tools without upgrading your subscription tier. Credits expire at end of calendar month.

| Calls | Price |
|-------|-------|
| 10 (~10 Advisor or 30 daily horoscopes) | $4.99 |
| 25 (~25 Advisor or 75 daily horoscopes) | $9.99 |
| 100 (~100 Advisor or 300 daily horoscopes) | $29.99 |

Siddha and Jyotish Guru subscribers already include monthly credits, so no packs are needed.

## Authentication

Two auth methods are supported:

1. **API Keys** (primary): `vv_live_` (production) or `vv_test_` (development) prefixed keys generated from the VedaVisor dashboard. Pass via `Authorization: Bearer vv_live_YOUR_KEY`.

2. **OAuth 2.1 + PKCE** (Phase 4): Supabase-issued JWTs are verified via OAuth metadata endpoints at `/.well-known/oauth-authorization-server` and `/.well-known/oauth-protected-resource`. Non-`vv_` Bearer tokens are verified as JWTs and mapped to the user's MCP subscription tier.

## Idempotency

Generation-credit tools support `x-idempotency-key` header. Duplicate keys within 1 hour return cached responses with zero credit deduction. Result caching: same birth details + same question returns cached result for 24 hours.

## Connection

Transport: HTTP (Streamable HTTP, stateless)

Auth: Bearer token with `vv_live_` / `vv_test_` prefixed API keys, or Supabase OAuth JWT.

Rate limits: Tiered. Free supports 10 req/min and 1,000/day; Enterprise supports 600 req/min and 100,000/day.

All astrological data is computed in real time using a professional-grade sidereal Lahiri calculation engine. Advisor interpretations are multi-paragraph, chart-specific responses grounded in classical Jyotish, not templates.

## Discovery Endpoints

| Endpoint | Purpose |
|----------|---------|
| `/.well-known/agents.json` | Machine-readable server metadata for MCP clients |
| `/.well-known/server-card.json` | MCP server metadata for directories |
| `/llms.txt` | Human + LLM-readable tool catalog |
| `/llms-full.txt` | Full tool documentation for LLM context loading |
| `/.well-known/oauth-authorization-server` | OAuth 2.1 authorization server metadata (RFC 8414) |
| `/.well-known/oauth-protected-resource` | OAuth 2.1 protected resource metadata (RFC 9728) |

## Links

- Website: https://mcp.vedavisor.com
- MCP endpoint: https://mcp.vedavisor.com/mcp/
- API docs: https://mcp.vedavisor.com/docs
- Signup: https://vedavisor.com/auth?mode=signup&redirect=%2Fmcp-api-keys
- Dashboard: https://mcp.vedavisor.com/dashboard

## Security Note

This repository contains public listing information and example MCP client configurations only. API keys are prefix-distinguished from OAuth tokens. Per-key, per-tool authorization is enforced at dispatch time. All tool invocations are logged with request IDs. HTTPS is enforced.

