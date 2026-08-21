# Region Map Generator — Rules

## Setup

- Grid of square spaces, 4 cardinal directions: N, S, E, W.
- Central City ("AAA") starts at the origin.
- **Hard cap:** the map's bounding box (width × height) may never exceed **160 spaces**. Before placing any tile outside the current bounding box, check what the box's area *would become* — if placing the tile would push it past 160, don't place it (see cutoff rules below).
- Town/city names: AAA, BBB, CCC... (each letter tripled, one town per letter, in creation order).
- Dungeon names: AA, BB, CC... (each letter doubled, incrementing across the whole map).

## 1. Build the route/town tree

**From Central City:** place one route in each of the 4 directions (N, S, E, W) — always all four, no roll needed.

**For every route** (from Central City or from a town's outgoing rolls below):

1. **Length:** roll a d6 → length = (1,1,2,2,3,3) for rolls (1,2,3,4,5,6).
2. **Dungeon?** roll a d6 → 5 or 6 = yes. If yes, pick one random insertion slot out of `length + 1` possible slots (before tile 1, between any two tiles, or after the last tile) — the dungeon occupies that slot as an *extra* tile (so total tiles walked = length, or length + 1 if a dungeon is present).
3. **Walk the route tile by tile**, in a straight line, one direction only:
   - If a tile would land on an already-occupied space (route, dungeon, or town) → stop immediately; draw a connector line into that existing space; **no town** is created at the end of this route.
   - If placing a tile would push the bounding box over 160 spaces → stop; **force the last tile placed to become a dungeon** (overwrite it, even if it was going to be a plain route tile); **no town**.
   - Otherwise place the tile (route or dungeon per the slot from step 2).
4. **If the walk completes fully** (no cutoff above), a town goes on the next space past the last tile:
   - If that space is already occupied → draw a connector line there instead; no town.
   - If placing it would exceed the 160-space cap → force the last route tile into a dungeon instead; no town.
   - **If that space is orthogonally adjacent to any other existing town** → don't place a town; instead extend the route by one more (plain route) tile onto that space, and draw connector lines from it to every adjacent town. No town.
   - Otherwise: place the new town (next name in sequence).
5. **If a town was placed:** roll a d6 → on a 6, this town also has a dungeon on its own tile (shown as a downward dungeon triangle attached under the town marker).
6. **New routes out of the town:** for each of the 3 directions *other than* the one this route arrived from, roll a d6 → 4, 5, or 6 means a new route is queued in that direction (go to step 1 for it).

Repeat until no routes remain to process.

## 2. Final connector ("spur") pass

After the tree is fully built, go through **every town, in the order it was created**. For each of its 4 directions:

- If the adjacent space is occupied: if no connector line already links them, draw one (no new tiles). Otherwise skip.
- If the adjacent space is empty: scan straight outward (within the map's existing bounding box) for the nearest occupied space.
  - Nothing found before the edge of the map → do nothing.
  - Found, but the gap is **more than 4 spaces** → skip (too far — spur routes cap out at 4 spaces).
  - Otherwise: fill the gap with route tiles (a new route). Roll a d6 for a dungeon (5–6 = yes) — if yes, one of the gap tiles (at random) becomes the dungeon *instead of* a route tile (no extra tile added this time, unlike step 1.2). Connect the far end to the node you found.

## 3. Renumber every route

Routes so far only need to be tracked as *groups* of tiles (which tiles belong to the same route) — don't worry about numbering them yet.

1. Randomly pick one direction from {N, S} and one from {E, W} (e.g. "South" + "East"). Together they name a starting corner (Southeast) and a sweep order: the direction picked *first* is the outer sweep (go edge-to-edge along its axis), the one picked *second* is the inner sweep (go edge-to-edge along its axis, within each outer step).
2. Walk **every space** in the bounding box, filled or empty, in that nested order, starting from the named corner.
3. The first time you reach any tile of a not-yet-numbered route, that route gets the next number (starting at 1), and all of its tiles get lettered a, b, c... in the order the sweep reaches them.
4. **If a route ends up with only one tile, drop the letter** (just "3", not "3a"). Dungeon tiles keep their own AA/BB labels and are untouched by this step.

This guarantees the highest route number equals the total number of routes, with no gaps.

## 4. Accept or regenerate

Check the finished region:

- Total towns/cities (Central City included) must be **10–16**.
- Total dungeons (standalone + town-shared) must be **8–14**.

If either fails, throw the whole region out and start over from step 1 with fresh rolls. Repeat until both pass.

## 5. Gyms

Randomly choose **8** of the region's towns/cities (Central City is eligible) to have a gym.

## Legend

| Symbol | Meaning |
|---|---|
| Blank circle | Route tile |
| Green square | Town/city, no gym |
| Gold square + star | Town/city with a gym |
| Red upward triangle | Standalone dungeon |
| Thin colored rectangle + red downward triangle attached below | Town/city (colored by gym status) sharing its space with a dungeon |
| Dashed outline | Central City marker |
