# Pokémon Dice

A dice-driven Pokémon region toolkit with a full regional dex, gym leaders, Elite Four, trainer classes, evolution rules on a 1–20 level scale, and an interactive HTML tool for rolling wild encounters, trainer battles, gym leader teams, and browsing the region map and Pokédex.

Two regions are playable, switchable from a dropdown in the tool itself:

- **Calli** — an original region based on California.
- **Neo Kanto** — the original Kanto region, thirty years later: a mix of the classic 151 with a wave of migrated species, some gyms changed type or city, and Team Rocket's old haunts repurposed. See `docs/NeoKanto_Regional_Dex.md` for the premise.

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

- **`index.html`** — the interactive tool (Wild Encounter Roller, Trainer Battle, Gym Leader / Elite Four / Champion, Region Map, Pokémon Data browser). A region dropdown in the masthead switches all of it between Calli and Neo Kanto, each backed by its own self-contained data set — there's no cross-region bleed in dex, map, or badge progress.
- **`Dice_Region_Ruleset.md`** — the generalized framework extracted from the Calli project: region-agnostic rules for building a new region (map structure, dex construction, type balance, the core dice mechanic, encounter tables, trainers, gyms, Elite Four, leveling, evolution, stats). Use this as the starting point for any future region.
- **`docs/`** — the reference documents behind the tool, 8 per region (`Calli_*.md` / `NeoKanto_*.md`): Regional Dex (species list, types, habitat, and die-face stats in one place), Dungeons (route/map graph), Evolution Guide, Encounter Tables, Pokémon Locations (reverse index of Encounter Tables), Trainer Classes, Gym Leaders, and Elite Four. None of these are loaded by `index.html` at runtime — they're the source material its data was transcribed from, kept as the reference to build against and to check the tool's data against later.
