# Neo Kanto Dungeons & Routes

*Canonical route/dungeon graph. Finalized against the node map you provided — this supersedes any earlier route description in the Neo Kanto Dossier. Encounter Tables, Trainer Classes, and the Gym Leaders/Elite Four docs all key off the location names below.*

## Route List

| Route | Connects | Terrain | Dungeon |
|---|---|---|---|
| 1 | Pallet Town ↔ Viridian City | Open grassland | — |
| 2 | Viridian City ↔ Pewter City | Forest (shrunk by development) | Viridian Forest |
| 3 | Pewter City ↔ Mt. Moon | Rocky foothills | Mt. Moon (west entrance) |
| 4 | Mt. Moon ↔ Cerulean City | Rocky foothills | Mt. Moon (east entrance); Cerulean Cave (segment nearest Cerulean) |
| 5 | Cerulean City ↔ Saffron City | Urban outskirts | — |
| 6 | Saffron City ↔ Vermilion City | Urban outskirts | — |
| 7 | Celadon City ↔ Saffron City | Urban outskirts | — |
| 8 | Saffron City ↔ Lavender Town | Urban outskirts | — |
| 9 | Cerulean City ↔ Power Plant | Rocky pass | — |
| 10 | Power Plant ↔ Rock Tunnel | Rocky pass / lakeside | Rock Tunnel |
| 11 | Vermilion City ↔ Route 12 | Coastal lowland | — |
| 12 | Lavender Town ↔ Route 11 junction ↔ Route 13 | Riverside | — |
| 13 | Route 12 ↔ Route 14 | Winding grassland | — |
| 14 | Route 13 ↔ Route 15 | Winding grassland | — |
| 15 | Route 14 ↔ Fuchsia City | Winding grassland | — |
| 16 | Celadon City ↔ Route 17 | Cycling Road (north end) | — |
| 17 | Route 16 ↔ Route 18 | Cycling Road | — |
| 18 | Route 17 ↔ Fuchsia City | Cycling Road (south end) | — |
| 19 | Fuchsia City ↔ Route 20 | Open water | — |
| 20 | Route 19 ↔ Cinnabar Island | Open water | Seafoam Islands |
| 21 | Cinnabar Island ↔ Pallet Town | Open water | — |
| 22 | Viridian City ↔ fork: Route 23 and Route 26 | Scrubland | — |
| 23 | Route 22 ↔ Indigo Plateau | Mountain trail | Victory Road |
| 24 | Cerulean City ↔ Route 25 | Nugget Bridge | — |
| 25 | Route 24 ↔ dead end | Coastal path | Bill's House (flavor only, not a mechanical dungeon) |
| 26 | Route 22 (fork) ↔ Icefall Pass | Alpine mountain pass | Icefall Pass |

## Dungeon List

| # | Dungeon | Location | Description | Legendary/Mythical |
|---|---|---|---|---|
| 1 | Viridian Forest | Within Route 2 | Shrunk to a fraction of its original size; surviving Bug/Grass habitat | — |
| 2 | Mt. Moon | Between Routes 3 & 4 | Classic meteorite cave | — |
| 3 | Cerulean Cave | Off Route 4, segment nearest Cerulean City | Deepened and expanded; high-level, home to the Dark/Dragon migrant wave | Mewtwo |
| 4 | Power Plant | Between Routes 9 & 10 | Modernized into a renewable-energy facility | Zapdos |
| 5 | Rock Tunnel | Between Route 10 and Lavender Town | Pitch-black cave, light source required | — |
| 6 | Pokémon Tower | Co-located with Lavender Town | Renovated Ghost gym site | — |
| 7 | Kanto Wildlife Preserve | Co-located with Fuchsia City | Formerly the Safari Zone; now a legally protected reserve | — |
| 8 | Seafoam Islands | Within Route 20 | Icy sea caves | Articuno |
| 9 | Cinnabar Mansion / Geothermal Complex | Co-located with Cinnabar Island | Rebuilt research lab after the volcanic reawakening | Moltres, Genesect (postgame) |
| 10 | Victory Road | Between Route 23 and Indigo Plateau | Elite-Four-gated final stretch | — |
| 11 | Icefall Pass | End of Route 26 | New alpine dungeon | — |

## Notes

- **Diglett's Cave has been removed entirely** — no node for it on the map. Diglett/Dugtrio relocated to the Rock Tunnel encounter table instead (see `NeoKanto_Regional_Dex_Numbered.md`).
- **Five explicit exclusions** override what plain grid adjacency would otherwise connect (per `Dice_Region_Ruleset.md` §1):
  - Cerulean Cave (CC) is *not* connected to Route 24 — it's reached from Route 4 instead.
  - Route 25 is *not* connected to Route 9 at either of its two adjacency points — keeps Route 25 a genuine dead end rather than a shortcut back into the Route 9/Power Plant corridor.
  - Route 11 is *not* connected directly to Route 14 at either of its two adjacency points — forces the loop through the Route 12/Lavender junction rather than a shortcut.
- **Full connectivity confirmed**: every node is reachable from Pallet Town. Multiple entry paths exist into every major cluster — Saffron is a genuine four-way hub (Routes 5/6/7/8), Cerulean has four approaches (Routes 4/5/9/24), and Fuchsia has three (Routes 15/18/19) — so no gym or region can be soft-locked regardless of the order they're tackled in.
- **Route 22 is the fork point** for the region's two dead-end postgame-flavor branches: Route 23 → Victory Road → Indigo Plateau (Elite Four, badge-gated) and Route 26 → Icefall Pass (freely explorable, no gate).
