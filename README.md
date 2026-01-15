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

This visualization uses H3 hexagons to reveal spatial patterns of play in Pokémon GO:
- Hexagons represent areas of roughly equal size (set by H3 resolution).
- Darker hexes indicate a higher number of spins in that area.
- Tooltips show:
  - Number of spins (spin_count)
  - Number of unique PokéStops (pokestop_count)
  - Spins per stop (spins_per_stop)

Even at this level of abstraction:
- Long-term home locations become obvious
- Daily routines emerge immediately
- Travel history across cities and countries is trivial to reconstruct


## Timeline animations (Notebook 03)

This notebook explores temporal dynamics of Pokémon GO activity, moving beyond static hex maps to show when and where PokéStops were visited.

### Basic Timeline Animation

Each PokéStop visit is plotted as a dot on a map in the order it occurred and using Folium’s TimestampedGeoJson plugin for interactive playback. It allows viewers to see the flow of movement over time and demonstrates how even a short window of data can reveal travel patterns and routines.

Features:
- Play/pause and slider to explore time
- Auto-centering on the map based on mean player location
- Optional tooltips with visit timestamp

### Enhanced Timeline Animation with Hex Heatmap Overlay

- Combines the raw timeline dots with H3 hex heatmaps, showing cumulative activity per hex.
- Hex intensity darkens as more spins accumulate.
- Unique PokéStops per hex are displayed as numbers inside the hexagon.
- Trail of points fades quickly to avoid clutter and emphasize recent activity.
- Fixed map extents and aspect ratio ensure the map doesn’t zoom in/out between frames.
- Map tiles provide spatial context without distorting scale.

How it works:

1. Convert raw data into a GeoDataFrame in Web Mercator (EPSG:3857) for compatibility with contextily tiles.
2. Precompute fixed map bounds to prevent zooming during animation.
3. Maintain a cumulative dictionary of hex info:
 - count: total spins per hex
 - forts: set of unique PokéStops visited
4. For each frame:
  - Draw hex polygons with alpha proportional to spin count
  - Annotate hexes with number of unique PokéStops
  - Plot a fading trail of recent raw visits
  - Overlay map tiles for context
5. Export all frames as a GIF for easy sharing.

Key visualization benefits:
- Shows both spatial density (hex heatmap) and temporal sequence (raw dot trail)
- Highlights how even aggregated data can reveal patterns of movement and sensitive locations
- Demonstrates the power of location data with minimal input
- Tip: Adjust trail_length, H3 resolution, or colormap to explore different temporal and spatial scales.


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
