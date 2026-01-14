# Pokemon-GO-Personal-Data-Visualisation
Personal Pokémon GO data visualisation using H3 hex maps and timeline animations to explore the power and sensitivity of location data.

## Overview

Pokémon GO provides players with access to their personal gameplay logs, including timestamped location data for PokéStop interactions.
This project turns that raw export into a local analytics and visualization pipeline that:
- loads and cleans PokéStop spin data from Niantic CSV exports
- normalises timestamps and converts events into true local time
- deduplicates overlapping and multi-logged interactions
- enriches events with spatial metadata (distance, city, country)
- aggregates activity into H3 hexagonal cells for spatial analysis
- produces both static heatmaps and time-based map animations

The result is a set of reusable notebooks that allow players to explore their own gameplay history spatially and temporally, while also highlighting how quickly meaningful location patterns emerge from seemingly simple interaction data.


## Requesting Pokémon GO data from Niantic

Niantic allows players to request their personal data under GDPR / data access rights. I request a copy of my Pokémon GO data first in early 2022 and then in later 2025, on both occasions Niantic Support has come back to me within 2 weeks with a password-protected download link.

The multiple csv files include pokestop spin history, pokemon encounter logs, gym interactions, raid activity, friends list and much more.
This project only focuses on the Pokestop_spin data.

⚠️ Important: Thes original files are highly sensitive personal data that contain precise timestamps and GPS coordinates. Only a small, time-bounded demo slice (hokkaido.csv) is included in the repo. All full-history processing is intended to run locally only.

## Data loading & cleaning (Notebook 01)

The first notebook focuses on making the raw export usable and safe:
- Combine multiple CSV batches into a single dataset
- Normalize timestamps into UTC for consistency
- Deduplicate overlapping or multi-logged events
- Infer local timezone per event (DST-aware)
- Convert timestamps to true local time
- Enrich events with:
  - distance between player and PokéStop
  - stable PokéStop identifiers
  - city / country metadata (cached & rate-limited)


## Spatial aggregation with H3 hexes (Notebook 02)



## Timeline animation (Notebook 03)



## Privacy & GDPR considerations

- Location data does not need to be labeled “home” to reveal home
- Aggregation reduces risk, but does not eliminate it
- Timelines are often more sensitive than static heatmaps
- Even “harmless” game data qualifies as personal data

For this reason:
- no full-history raw data is shared
- demo datasets are intentionally short
- all analysis is designed to run locally
- If you use this repo, you are responsible for your own data handling.

## Try it yourself

Any Pokémon GO player can use this pipeline to visualize their own play history, explore travel patterns across years, and understand how location data accumulates over time.

## Why this project exists

Pokémon GO’s in-app maps are fun but limited.

This project started as a curiosity: “What would my Pokémon GO footprint look like if I controlled the data?”

location data is powerful, persistent, and deserves respect.
