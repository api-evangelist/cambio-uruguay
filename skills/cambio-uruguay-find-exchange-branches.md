---
name: cambio-uruguay-find-exchange-branches
description: Locate physical exchange-house branches in Uruguay — addresses, coordinates, departments — and find the nearest one.
api: Cambio Uruguay API
generated: '2026-09-07'
method: generated
source: openapi/cambio-uruguay-openapi.json
operations:
  - GET /exchanges/{origin}
  - GET /exchanges/{origin}/{location}
  - POST /geocoding
  - GET /distances
  - GET /localData
---

# Find exchange-house branches

1. **List a house's branches.** `GET /exchanges/{origin}` returns `Location` objects —
   `{name, address, latitude, longitude, department, city}`. Narrow to one department
   or city with `GET /exchanges/{origin}/{location}`.
2. **Resolve the user's position.** `POST /geocoding` forward-geocodes a free-text
   address into `{lat, lon, display_name}`.
3. **Rank by proximity.** `GET /distances` computes distances so you can recommend
   the nearest branch; `Location.distance` carries the result.
4. **House metadata** (website, departments served) lives in `GET /localData`.

Combine with the best-exchange-rate skill: filter to houses with a branch near the
user FIRST, then rank those by rate — the best on-paper rate is useless if the house
only has a counter in another department.

All calls are anonymous. Invalid `origin` or `location` values return HTTP 400 with
`validValues` to retry from.
