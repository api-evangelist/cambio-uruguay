---
name: cambio-uruguay-best-exchange-rate
description: Find the best place in Uruguay to buy or sell a currency right now, comparing 40+ exchange houses and banks.
api: Cambio Uruguay API
generated: '2026-09-07'
method: generated
source: openapi/cambio-uruguay-openapi.json + mcp/cambio-uruguay-mcp-tools.json
operations:
  - GET /
  - GET /parameters/all
  - GET /exchange/{origin}/{code}
---

# Find the best exchange rate in Uruguay

No API key or signup is needed — every call below is anonymous.

1. **Know the vocabulary first.** `GET /parameters/all` returns every valid `origin`
   (exchange-house slug like `brou`, `la_favorita`), `currency` code (ISO 4217 plus
   `XAU` for gold), and quote `type` (`BILLETE`, `CABLE`, `TRANSFERENCIA`, `eBROU`,
   `INTERBANCARIO`). Cache it — it changes rarely.
2. **Pull the full market.** `GET /` returns every current quote as a flat array of
   `{origin, code, type, buy, sell, date, name}`.
3. **Filter and rank.** Keep rows matching your currency code. To BUY the currency,
   the best house has the LOWEST `sell`; to SELL it, the best house has the HIGHEST
   `buy`. Exclude `origin: bcu` and `type: INTERBANCARIO` — those are reference rates
   not available to the public.
4. **Drill into one house if needed** with `GET /exchange/{origin}/{code}`.

Recovery: a bad `origin` or `code` returns HTTP 400 with `validValues` listing every
acceptable value and a `suggestion` — read it and retry, don't guess. Rates refresh
about every 10 minutes; responses are edge-cached for ~5 seconds.

MCP alternative: the hosted server at `https://mcp.cambio-uruguay.com/mcp` wraps this
flow as the `get_rates` and `best_house` tools.
