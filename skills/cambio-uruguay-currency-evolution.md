---
name: cambio-uruguay-currency-evolution
description: Chart how a currency's buy/sell rates evolved at a specific Uruguayan exchange house, with statistics.
api: Cambio Uruguay API
generated: '2026-09-07'
method: generated
source: openapi/cambio-uruguay-openapi.json + live response from GET /evolution/brou/USD (200, 2026-09-07)
operations:
  - GET /evolution/{origin}/{code}
  - GET /evolution/{origin}/{code}/{type}
  - GET /changes
  - GET /market-change
---

# Track a currency's evolution

1. **Pick the house and currency.** `origin` slugs and currency codes come from
   `GET /parameters/all` (e.g. `brou` + `USD`).
2. **Fetch the series.** `GET /evolution/{origin}/{code}` returns dated
   `{date, buy, sell, avg}` points plus a `statistics` block —
   `totalDataPoints`, `dateRange`, min/max/avg/current and change. A live probe of
   `/evolution/brou/USD` returned a 370-point daily series covering six months.
3. **Scope by quote channel when it matters** with
   `GET /evolution/{origin}/{code}/{type}` (`BILLETE` vs `CABLE` vs `TRANSFERENCIA`
   can diverge meaningfully).
4. **For market-wide movement** rather than one house, use `GET /changes` and
   `GET /market-change`.

Recovery: invalid path values return HTTP 400 with `validValues` and a `suggestion`.
All operations are reads — safe to retry freely.

MCP alternative: the `get_evolution` tool on `https://mcp.cambio-uruguay.com/mcp`
wraps this with a `period` (months) parameter.
