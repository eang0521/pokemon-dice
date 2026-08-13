# Pokémon Dice — Calli Region

A dice-driven Pokémon region (Calli, based on California) with a full regional dex, gym leaders, Elite Four, trainer classes, evolution rules on a 1–20 level scale, and an interactive HTML tool for rolling wild encounters, trainer battles, gym leader teams, and browsing the region map and Pokédex.

## Live tool

Once GitHub Pages is turned on for this repo (see below), the tool will be live at:

```
https://eang0521.github.io/pokemon-dice/
```

`index.html` is the whole tool — self-contained, no build step, works offline once loaded.

## Enabling GitHub Pages

1. Push this folder to the `pokemon-dice` repo (root of the repo, not a subfolder).
2. On GitHub: **Settings → Pages**.
3. Under "Build and deployment", set **Source** to "Deploy from a branch".
4. Set **Branch** to `main` (or whichever branch you pushed to) and folder to `/ (root)`.
5. Save. GitHub will publish the site in a minute or two at the URL above.

## What's in here

- **`index.html`** — the interactive tool (Wild Encounter Roller, Trainer Battle, Gym Leader / Elite Four / Champion, Region Map, Pokémon Data browser).
- **`Dice_Region_Ruleset.md`** — the generalized framework extracted from the Calli project: region-agnostic rules for building a new region (map structure, dex construction, type balance, the core dice mechanic, encounter tables, trainers, gyms, Elite Four, leveling, evolution, stats). Use this as the starting point for any future region.
- **`docs/`** — the reference documents behind the tool:
  - `Calli_Regional_Dex.md` / `Calli_Regional_Dex_Numbered.md` — the 209-Pokémon regional dex
  - `Calli_Dungeons.md` — official routes and dungeon locations
  - `Calli_Encounter_Tables.md` — full wild encounter tables per route/dungeon
  - `Calli_Pokemon_Locations.md` — reverse lookup: where to find each Pokémon
  - `Calli_Evolution_Guide.md` — evolution levels/methods on the 1–20 scale
  - `Calli_Gym_Leaders.md` / `Calli_Elite_Four.md` — gym leader and Elite Four/Champion teams
  - `Calli_Trainer_Classes.md` — the 11 generic route trainer classes
  - `Calli_Gym_Pokemon_Stats.md` / `Calli_Full_Dex_Stats.md` — die-face stat conversions
