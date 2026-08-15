# Pokémon Dice

A dice-driven Pokémon region toolkit with a full regional dex, gym leaders, Elite Four, trainer classes, evolution rules on a 1–20 level scale, and an interactive HTML tool for rolling wild encounters, trainer battles, gym leader teams, and browsing the region map and Pokédex.

Three regions are playable, switchable from a dropdown in the tool itself:

- **Calli** — an original region based on California.
- **Neo Kanto** — the original Kanto region, thirty years later: a mix of the classic 151 with a wave of migrated species, some gyms changed type or city, and Team Rocket's old haunts repurposed. See `docs/NeoKanto_Regional_Dex.md` for the premise.
- **Hoenn** — real Hoenn from Pokémon Emerald: the official regional dex (194 species — a handful of legendaries and babies were cut, see `docs/Hoenn_Dungeons.md`), the actual eight gym leaders and Elite Four/Champion, and a hand-built region map, all adapted into this project's "any gym order" / 1–20 scale format.

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

- **`index.html`** — the interactive tool (Wild Encounter Roller, Trainer Battle, Gym Leader / Elite Four / Champion, Region Map, Pokémon Data browser). A region dropdown in the masthead switches all of it between Calli, Neo Kanto, and Hoenn, each backed by its own self-contained data set — there's no cross-region bleed in dex, map, or badge progress. Links to `battle_simulator.html` via a "Battle Sim" tab and `my_pokemon.html` via a "My Pokemon" tab. Every rolled Pokémon (wild, Trainer Battle, Gym Leader/Elite Four/Champion) has its own "Send to Battle Sim" button (opens the battle sim pre-filled with that exact Pokémon, level bonus included) and "Catch!" button (saves it to My Pokemon). The battle sim's back link returns to the field guide in the exact state it was left in — same tab, badges, and selections, with every rolled result redisplayed rather than re-rolled, so nothing re-randomizes in the round trip.
- **`battle_simulator.html`** — a standalone 1v1 combat-odds calculator (see `Dice_Region_Ruleset.md` §14), separate from the region tool and not tied to any one region. Pick any two Pokémon (autocompletes against the full real dex via `pokemon_base_stats.json`'s die-face conversion, type in a custom name to build one from scratch, load one straight from My Pokemon, or arrive here already filled in via a "Send to Battle Sim"/"Catch!" round trip), assign bonus points per stat (their sum becomes the Pokémon's effective level) and an attack type, and it computes exact win/loss/tie percentages by enumerating every dice outcome — type effectiveness (a real 18-type chart, not the §13 wheel) changes how many dice are rolled and whether the best or worst is kept, and STAB adds +1. A "Roll the Battle!" button plays out one real exchange instead of just the odds, rerolling automatically on a tie. Real Pokémon have their base stats locked (only EVs and attack type stay editable); a "Save to My Pokemon" button on each side banks the current build. Links back to `index.html` and `my_pokemon.html`.
- **`my_pokemon.html`** — your caught-Pokémon collection, stored in `localStorage` (shared across all three pages, including under `file://` — confirmed to persist whether the tool is opened by double-clicking the file or served over GitHub Pages). Populated via the "Catch!" buttons in the field guide or "Save to My Pokemon" in the battle sim; each entry can be sent straight into either side of the Battle Simulator or released. This is the fast path for "how does my caught team stack up against this wild Pokémon/trainer I just rolled."
- **`Dice_Region_Ruleset.md`** — the generalized framework extracted from the Calli project: region-agnostic rules for building a new region (map structure, dex construction, type balance, the core dice mechanic, encounter tables, trainers, gyms, Elite Four, leveling, evolution, stats, 1v1 battle resolution). Use this as the starting point for any future region.
- **`docs/`** — the reference documents behind the tool, 8 per region (`Calli_*.md` / `NeoKanto_*.md` / `Hoenn_*.md`): Regional Dex (species list, types, habitat, and die-face stats in one place), Dungeons (route/map graph), Evolution Guide, Encounter Tables, Pokémon Locations (reverse index of Encounter Tables), Trainer Classes, Gym Leaders, and Elite Four. None of these are loaded by `index.html` at runtime — they're the source material its data was transcribed from, kept as the reference to build against and to check the tool's data against later.
- **`pokemon_base_stats.json` / `.csv`** — current (latest-generation) base stats for every real Pokémon and alternate form (1,219 rows, scraped from [PokémonDB's full Pokédex table](https://pokemondb.net/pokedex/all)). A standing local cache so future region-building work (die-face stat conversion, ÷17.5 rounded — see `Dice_Region_Ruleset.md` §12) can look stats up directly instead of re-fetching them. Not loaded by `index.html` at runtime.
- **`pokemon_evolution_data.json` / `.csv`** — evolution relationships (833 species/forms across 340 families, scraped from [PokémonDB's evolution chart](https://pokemondb.net/evolution)): each record has `evolvesFrom`/`evolvesTo` with the verbatim condition text (level, item, trade, friendship, location, etc.), supporting branching evolutions (Eevee, Tyrogue, Wurmple). Species absent from this file don't evolve at all — cross-check against `pokemon_base_stats.json` for the full species list. A standing local cache so future region-building work (evolution guides, buffered non-level-evolution levels — see `Dice_Region_Ruleset.md` §11) can look this up directly instead of re-fetching per-species pages. Not loaded by `index.html` at runtime.
