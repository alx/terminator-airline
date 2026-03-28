---
title: "How it works"
description: "The science behind Terminator Airline"
url: "/how-it-works/"
layout: "page"
---

## The Terminator Line

The **solar terminator** is the moving line that separates Earth's sunlit side from its night side. At altitude, this line creates the most dramatic light conditions possible — a mix of deep indigo, burning orange, and electric pink that lasts only minutes from the ground but can stretch across an entire flight at 35,000 ft.

## The Score

We calculate the terminator's position every minute using precise solar ephemeris data. For each scheduled flight we compute:

1. **Flight path** — great circle route between origin and destination
2. **Sun position** — altitude and azimuth along the flight path over time
3. **Terminator proximity** — how close the flight passes to the terminator line
4. **Duration in the golden zone** — minutes spent within ±30 min of the terminator

These combine into a single **Terminator Score** from 0 to 100.

## Seat recommendation

Your seat matters. We calculate the **sun azimuth relative to the aircraft heading** at the moment of best light and tell you:

- Which side of the plane faces the terminator
- The exact window seat rows for the best view
- Whether to look forward, sideways, or behind

## Data sources

- ✈️ **Flight schedules** — OpenSky Network + airline schedules
- ☀️ **Solar ephemeris** — Astropy / SunCalc algorithms
- 🌩️ **Cloud cover** — Open-Meteo forecast API *(coming soon)*
