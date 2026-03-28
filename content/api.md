---
title: "API Reference"
description: "Terminator Airline API — x402 micropayments, OpenSky flight data, solar ephemeris"
url: "/api/"
layout: "page"
---

The Terminator Airline API scores flights by solar terminator proximity. **Paid endpoints use x402 micropayments** — 0.001 USDC on Base (~$0.001 per call). Designed for AI agents.

## Base URL

```
https://api.terminator-airline.com
```

> Self-host: `docker-compose up` — [github.com/alx/terminator-airline-api](https://github.com/alx/terminator-airline-api)

---

## Free Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /` | API info, pricing, links |
| `GET /airports` | 18 covered SEA airports + coordinates |
| `GET /terminator?dt=` | Terminator line as GeoJSON (default: now) |

---

## Paid Endpoints (x402 — 0.001 USDC / call)

### `GET /flights/score`

Flights for an airport on a date, ranked by terminator score.

```
GET /flights/score?airport=BKK&date=2026-03-28&direction=departures
X-Payment: <proof>
```

**Response:**
```json
{
  "flights": [{
    "callsign": "TG201",
    "from": "BKK", "to": "SIN",
    "terminator_score": 87,
    "score_label": "🌅 Golden hour",
    "golden_zone_minutes": 22,
    "seat": {
      "side": "starboard",
      "seats": "D/E/F (right side)",
      "tip": "Sit on the starboard side to face the terminator light."
    }
  }]
}
```

### `GET /flights/{icao24}/seat`

Detailed seat recommendation for a specific flight by ICAO24.

---

## x402 Payment Flow

1. Request endpoint → receive `402 Payment Required`
2. Read `X-Payment-Required` header (amount, address, network)
3. Pay 0.001 USDC on **Base** to the specified address
4. Retry with `X-Payment: <tx_proof>` header
5. Receive flight data

```python
import httpx

resp = httpx.get("/flights/score?airport=BKK&date=2026-03-28")
if resp.status_code == 402:
    req = resp.json()
    # pay req["payTo"] req["maxAmountRequired"] USDC on Base
    proof = pay_usdc(req)
    resp = httpx.get("/flights/score?...", headers={"X-Payment": proof})
```

---

## AI Agent Usage

```
llms.txt: https://alx.github.io/terminator-airline/llms.txt
```

The `llms.txt` at the site root describes all endpoints for LLM agents — compatible with Claude, GPT, and any agent that reads llms.txt.
