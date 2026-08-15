# Hoenn Region — Official Routes & Dungeons

Real Hoenn, as it appears in Pokémon Emerald — towns, routes, and dungeons all match the source game. Structural changes from canon, matching Calli's and Neo Kanto's conventions: every HM-gated path (Surf, Dive, Rock Smash, etc.) and the one-way current on Routes 132–134 are simplified to plain, freely-traversable connections; the dungeon count is trimmed to a manageable 12 (see Notes); and this map was hand-built as a true grid (orthogonal adjacency generates the connections, per the standard build process) rather than a stylized layout. Gym leaders can also be challenged in any order, per the project's standard house rule — see `Hoenn_Gym_Leaders.md`.

## Route List

| Route | Connects | Dungeon |
|---|---|---|
| 101 | Littleroot Town ↔ Oldale Town | |
| 102 | Oldale Town ↔ Petalburg City | |
| 103 | Oldale Town ↔ Route 110 | |
| 104 | Petalburg City ↔ Route 105 (via Petalburg Woods) | **Petalburg Woods** |
| 105 | Route 104 ↔ Route 106 | |
| 106 | Route 105 ↔ Dewford Town | |
| 107 | Dewford Town ↔ Route 108 | |
| 108 | Route 107 ↔ Route 109 | |
| 109 | Route 108 ↔ Slateport City | |
| 110 | Slateport City ↔ Mauville City (also meets Route 103) | |
| 111 | Mauville City ↔ Route 112 (also meets Route 113) | |
| 112 | Route 111 ↔ Lavaridge Town | **Mt. Chimney** (combo with Lavaridge) |
| 113 | Fallarbor Town ↔ Route 111 | |
| 114 | Fallarbor Town ↔ Meteor Falls | |
| 115 | Meteor Falls ↔ Rustboro City | |
| 116 | Rustboro City ↔ Rusturf Tunnel | |
| 117 | Mauville City ↔ Verdanturf Town | |
| 118 | Mauville City ↔ Route 119 (also meets Route 123) | |
| 119 | Fortree City ↔ Route 118 | |
| 120 | Fortree City ↔ Route 121 | |
| 121 | Lilycove City ↔ Route 120 (also meets Route 122, Safari Zone) | |
| 122 | Mt. Pyre ↔ Route 121 (also meets Route 123) | |
| 123 | Route 118 ↔ Route 122 | |
| 124 | Lilycove City ↔ Mossdeep City (also meets Route 125) | |
| 125 | Mossdeep City ↔ Route 124 (also meets Shoal Cave) | |
| 126 | Route 127 ↔ Route 128 (also meets Sootopolis City) | |
| 127 | Mossdeep City ↔ Route 126 | |
| 128 | Route 126 ↔ Route 129 (also meets Seafloor Cavern, Victory Road) | |
| 129 | Route 128 ↔ Route 130 | |
| 130 | Route 129 ↔ Route 131 (also meets Seafloor Cavern) | |
| 131 | Pacifidlog Town ↔ Route 130 (also meets Sky Pillar) | |
| 132 | Pacifidlog Town ↔ Route 133 | |
| 133 | Route 132 ↔ Route 134 | |
| 134 | Route 133 ↔ Slateport City | |

*(34 routes total. Routes needing more than one map "spot" along their length use lettered segments — e.g. Route 105 is `105a`/`105b` — but all segments of one route number share the same encounter table.)*

## Dungeon List (12 total)

| # | Dungeon | Location | Description | Legendary/Mythical |
|---|---|---|---|---|
| 1 | Petalburg Woods | Route 104 | Dense forest between Petalburg and Rustboro, old-growth and shaded | — |
| 2 | Rusturf Tunnel | Route 116 ↔ Verdanturf Town | Excavated tunnel, a staple early-game dungeon | — |
| 3 | Granite Cave | Dewford Town | Multi-level cave, pitch dark below the entrance | — |
| 4 | Meteor Falls | Route 114 ↔ Route 115 | Cathedral-scale waterfall cave, meteorite fragments in the deepest chamber | **Latias & Latios** |
| 5 | Mt. Chimney | Combo with Lavaridge Town | Active volcano crater with a cable-car summit | — |
| 6 | New Mauville | Combo with Mauville City | Flooded underground power facility beneath the city | — |
| 7 | Seafloor Cavern | Route 128 ↔ Route 130 (also meets Sootopolis City) | Deep-sea cave; site of the region's ancient-legend awakening | **Kyogre & Groudon** |
| 8 | Sky Pillar | Route 131 | Crumbling ancient tower spiraling above the clouds | **Rayquaza** |
| 9 | Victory Road | Route 128 (also meets Ever Grande City) | The region's final gauntlet | — |
| 10 | Safari Zone | Route 121 | Fenced wildlife preserve with its own rules | — |
| 11 | Mt. Pyre | Route 122 | Ancestral graveyard mountain | — |
| 12 | Shoal Cave | Route 125 | Tide-dependent sea cave north of Mossdeep | — |

## Notes

- **Trimmed from the source game's fuller dungeon list**, matching Calli's and Neo Kanto's roughly-a-dozen scale. Cut entirely: Desert Ruins, Mirage Tower, Island Cave, Ancient Tomb, Jagged Pass, Fiery Path, Trick House, Weather Institute, Scorched Slab, Abandoned Ship, Sealed Chamber, and Cave of Origin. Rusturf Tunnel — cut in an earlier pass — is back in this version, as a staple early dungeon worth keeping for flavor.
- **Two true combo nodes** (town + dungeon sharing one map spot): Lavaridge Town/Mt. Chimney, and Mauville City/New Mauville. Sootopolis City and Ever Grande City are plain towns in this version — each sits *adjacent* to a dungeon (Seafloor Cavern and Victory Road respectively) rather than merged with one.
- **Legendary/Mythical cuts:** Jirachi, Deoxys, Regirock, Regice, and Registeel are **not part of this adaptation**. Jirachi's and Deoxys's only possible homes were cut dungeons and neither is obtainable without a real-game event anyway; the three Regis lost their ruin dungeons in this pass and were cut along with them rather than given an artificial new home.
- **Baby Pokémon cut:** Azurill, Igglybuff, and Pichu are also not part of this adaptation — their evolved lines (Marill, Jigglypuff, Pikachu) remain and are simply treated as base species. The Hoenn dex here is **194 species**, not 202 — every remaining species keeps a real obtain path (verified programmatically: no unreachable species, no evolution family split across unrelated tables where avoidable).
- **Fully connected network confirmed programmatically:** every one of the 79 map nodes (14 standalone towns + 2 combo towns, 53 route segments across 34 route numbers, 10 standalone dungeons + the Mt. Chimney/New Mauville combo dungeons) is reachable from Littleroot Town via breadth-first search over the exact edge list used in `index.html`, honoring every explicit non-adjacency the map calls for (e.g. Route 111 never touches Route 119 despite running parallel to it; Lilycove doesn't directly touch Mt. Pyre; Sky Pillar doesn't directly touch Seafloor Cavern).
- **Redundant entry points:** the Lilycove/Mossdeep/Sootopolis cluster is reachable via the Fortree land route (119→Fortree→120→121) and separately via the Route 124–127 sea loop; Rustboro is reachable via Verdanturf/Rusturf Tunnel and separately via Fallarbor/Meteor Falls. This supports the region's free gym-order design the same way Calli's and Neo Kanto's redundant loops do.
- **One-way currents (Routes 132–134):** in the original game these only flow west (Pacifidlog → Slateport); this project treats them as ordinary two-way routes, with the current kept only as flavor text.
- **Kyogre & Groudon** both are obtainable in this adaptation (Emerald's own plot has both awaken, unlike Ruby/Sapphire's version-exclusive single encounter), sharing Seafloor Cavern as their signature location.
- **Latias & Latios** are roaming Pokémon in the source game with no single fixed wild-encounter location. This project gives them a shared signature sum-12 slot at Meteor Falls instead, matching the convention used for Calli's and Neo Kanto's Legendaries.
- **Shared encounter tables:** Routes 124–127 (the Mossdeep-area sea loop) share one encounter table with a dive/undersea lean, and Routes 128–134 (the long Pacifidlog/current stretch back to Slateport) share a second one — see `Hoenn_Encounter_Tables.md`. Every other route and dungeon has its own distinct table.
