# The Dice Region Ruleset
### A generalized framework for building Pokémon regions, extracted from the Calli project and proven again end-to-end on Neo Kanto

This document strips out everything region-specific and keeps only the *rules* — so it can be reused to build a new region from scratch, or to add one to a tool that already has another region in it. Wherever a rule references a number (like "8 gyms" or "209 Pokémon"), treat it as a tunable default, not a hard requirement, unless marked "fixed." §§15–17 are new since Calli — they cover the reference-doc set, the actual data schema a digital tool should use, and the build process that caught real bugs building Neo Kanto. §14 is new since Hoenn — it's the actual 1v1 combat-resolution math (dice-vs-stat rolls, type-effectiveness dice swaps, STAB), which everything up through §13 assumes exists but never actually defines.

---

## 1. Region Map Structure

- Design the map as a **node graph**, not a literal geographic map. Build it on a grid (rows × columns) where most cells are empty and a minority hold nodes.
- **Node types and IDs:**
  - **Towns/Cities** — 3-letter abbreviation, larger/primary nodes.
  - **Dungeons** — 2-letter abbreviation, secondary nodes (optional side content, not required for critical path).
  - **Routes** — numbered (1, 2, 3...); if a route needs more than one "spot" along its length (e.g., it passes a dungeon, or several other routes branch off it), suffix with a letter (`9a`, `9b`, `9c`...).
- **Connectivity rule:** two nodes are connected if and only if they are orthogonally adjacent on the grid (up/down/left/right — never diagonal). This can be computed automatically from the grid rather than hand-drawn, which avoids human transcription errors at scale.
- **Explicit exclusions:** allow specific adjacent pairs to be *not* connected, when the fiction calls for it (e.g., a cliff or river blocking an otherwise-adjacent path). Keep a small exception list and apply it after the automatic adjacency pass.
- **Co-located nodes:** when a dungeon sits inside/under a town (a mine under the capital, a sewer under a city), don't give it a separate grid cell — mark it as co-located with the town's node instead, using a fourth node kind, `combo`: `{id, kind:'combo', row, col, townId, townName, gymLeader, dungeonId, dungeonName, dungeonLocationId}`. Render it as one marker split by a divider (e.g. a gym-colored rectangle on top, a dungeon-colored triangle below) instead of two overlapping shapes, and give it a single click target whose info panel shows both halves (gym-leader link, dungeon encounter-table link, encounter preview) at once.
- **Verify full connectivity:** after building the graph, confirm every node is reachable from the starting town, and ideally that important clusters have more than one path in — this is what makes "gyms in any order" actually work without soft-locking the player.

---

## 2. Regional Dex Construction

- **Target size:** ~200 Pokémon, give or take 5–10 (190–210 is a comfortable range). This roughly matches the scale of most mainline regional dexes.
- **Starters:** 3 lines (Grass/Fire/Water is traditional), each fully evolved (3 stages each = 9 Pokémon), thematically fitted to the region's identity. Starters are **never** placed in wild encounter tables — they're Professor-gift-only.
- **Legendaries/Mythicals:** a "normal" region has roughly 5–8 Legendary/Mythical Pokémon. Tie each one thematically to a specific location (ideally a postgame-gated dungeon) rather than leaving them generic.
- **Completeness rule:** if a Pokémon is in the dex, **every one of its non-regional-variant evolutions must also be in the dex** (both directions — don't include a mid-evolution without its later stage, and don't include a final stage without checking whether its earlier stages should also be represented). Branch evolutions (e.g., a Pokémon with two alternate final forms) count as part of the same family, not extra dex slots.
- **Habitat organization:** group the dex by which town/route/dungeon each Pokémon is thematically associated with — makes both design and later cross-referencing much easier.
- **Audit for genuinely uncatchable species before finalizing.** A species with no evolution to fall back on (a true single-stage) *and* no wild-table appearance is a dead end, not a stylistic choice — nothing in the game ever gives the player a way to obtain it. Species reachable only by evolving an already-placed lower stage (via level, stone, or trade) are fine to leave out of wild tables entirely; this audit is specifically for species with nothing below them to evolve from.

---

## 3. Type Balance

- **Don't eyeball it — count it.** After drafting the dex, tally primary-type counts and combined (primary+secondary) counts, then compare against real regional dex patterns:
  - **Water, Normal, and Flying are *supposed* to be the most common types** in almost every real region — this isn't a flaw to fix, it's the norm (Flying is usually almost entirely secondary, rarely primary).
  - **Ghost and Dragon are the types real regions chronically under-supply.** Don't let your dex fall into that trap — make sure both have genuine presence, not just 1–2 tokens.
  - Watch for a **single type spiking abnormally high** relative to the rest (in Calli's case, Ground and Grass both ran hot from over-theming certain habitats) — if one type is 2–3× the size of its neighbors for no thematic reason, trim it.
  - It's fine — expected, even — for a couple of types to run higher than the National Dex baseline if the region's *geography* justifies it (e.g., a desert-heavy region will run Ground-heavy; an icy region will run Ice-heavy). Just don't let it get so extreme it crowds out variety.
- **Re-audit after every major edit.** Cuts and additions made for other reasons (evolution completeness, encounter-table needs, gym-team needs) will shift the balance — recheck the numbers each time, don't just check it once at the start.

---

## 4. Evolution-Stage Distribution

- Real regional dexes have roughly this shape: **two-stage lines are the single most common category** (usually just over half of all lines), **three-stage lines are the backbone** (~30%, including all starters and any pseudo-legendary-style lines), and **one-stage lines are the minority** (~15%, but a good chunk of those are Legendaries/Mythicals that don't evolve, not ordinary Pokémon).
- **Ordinary (non-Legendary) one-stage "oddities"** — Pokémon that just don't evolve, no lore reason needed — should still be roughly ~10% of the dex. If cuts have pushed that number down to near-zero, add a few back; these "oddity" Pokémon (a Tauros, a Lapras, a Farfetch'd-type pick) are part of what makes a dex feel textured rather than mechanically pure. Adding new evolution stages to previously-single-stage species (§11) shrinks this number fast — re-check it every time you unlock a new evolution, not just once.

---

## 5. Core Dice Mechanic

This is the engine underneath everything else — wild encounters, trainer battles, and gym teams all key off it.

- **Roll 2d6.** Any die showing above the **current threshold** gets rerolled (that single die only, not both).
- **Threshold scales with badge count:**

| Badges | Threshold | Dice show | Max possible sum | Most common sum |
|---|---|---|---|---|
| 0 | 2 | 1–2 | 4 | 3 |
| 1–2 | 3 | 1–3 | 6 | 4 |
| 3–4 | 4 | 1–4 | 8 | 5 |
| 5–6 | 5 | 1–5 | 10 | 6 |
| 7–8 | 6 | 1–6 | 12 | 7 |

- **Why this matters:** early-game, only low sums are even *reachable* — there's no need to gate content by badge count separately, the dice do it automatically. High-value slots (11, 12) become reachable only once the player has enough badges to roll a 6 on both dice.
- **Rarity is symmetric around 7.** At full threshold (7–8 badges), the exact odds are: 7 (16.7%) > 6/8 (13.9% each) > 5/9 (11.1%) > 4/10 (8.3%) > 3/11 (5.6%) > 2/12 (2.8%, tied for rarest). **2 is just as rare as 12** — don't build a table (or a trainer-class list) that treats low numbers as "common" by default; that's backwards.
- **If the region count (gyms, etc.) isn't fixed at 8**, derive the threshold tiers from the actual count rather than hardcoding 5 bands of size ~2 — e.g. five buckets spanning `0..gymCount` at the ¼/½/¾ marks reproduces the original table exactly when the count is 8, and degrades sanely for any other count.

---

## 6. Wild Encounter Tables

- One table per route/dungeon, covering sums 2–12 (11 slots).
- **Low slots = common/base-stage. High slots = rare/evolved.** But this needs to be checked *across the whole table*, not just within one species line — an evolved Pokémon from Line A should never occupy a lower (more common) slot than an unrelated base-stage Pokémon from Line B in that same table. (This is an easy bug to introduce when hand-editing tables piecemeal — audit for it programmatically if possible.) Single-stage species with no evolution at all are the one exception worth naming explicitly: their rarity is about scarcity, not stage, so a rare one-stage species sitting above an unrelated evolved Pokémon isn't the same bug.
- **Default to ~4–5 evolution families per table, not 11 distinct species.** The naive-looking "maximally varied" table — 11 completely unrelated single-species slots — is actually the wrong default: it reads as random rather than authored, and it's the single most common mistake in a first pass at this doc. Instead, pick a handful of lines the location's dex pool supports and show *multiple stages of the same line* across several slots — e.g. four families sized roughly 3/3/3/2 (three two-or-three-stage lines shown in full, one more at two slots) fills all 11 slots on its own, no unrelated one-off species needed. A couple of true standalone singles (no evolution, or a genuinely scattered species) mixed in is fine and often unavoidable, but they should be the minority, not the whole table.
- **Within that family, low slots are the earlier stage(s) and high slots are the later stage(s) — always, no exceptions per line.** A base-stage Pokémon must never sit at a sum equal to or higher than its own evolution's sum in the same table. Cross-check this *across the whole table* too, not just within one line — an evolved Pokémon from Line A should never occupy a lower slot than an unrelated base-stage Pokémon from Line B. (This is an easy bug to introduce when hand-editing tables piecemeal — audit it programmatically: for every evolution step where both stages appear in a table, assert `max(slots of the earlier stage) < min(slots of the later stage)`.) Single-stage species with no evolution at all are the one exception worth naming explicitly: their rarity is about scarcity, not stage, so a rare one-stage species sitting above an unrelated evolved Pokémon isn't the same bug.
- **Reserve the rarest slot (sum 12) for the location's signature rare find** — a Legendary/Mythical if the location has one, otherwise the location's best "trophy" catch. Since sum 12 is only reachable at max badge count, this is naturally a postgame-flavored reward with zero extra flagging required. If a signature find should require an additional condition beyond just rolling 12 (e.g. a second Legendary that only appears after some other postgame milestone), say so explicitly — don't let two Legendaries silently share one slot.
- **Sanity-check reachability:** nothing should be *permanently* locked behind an impossible combination (e.g., don't put a Pokémon whose evolution requires a level higher than the location's level cap ever allows at a slot that implies it should be evolved there).
- **Prefer keeping an evolution family together in one table**, not just in the dex overall — rather than scattering its stages across unrelated locations, place the base and its evolution(s) in the *same* table wherever the local dex pool supports it (this is the natural consequence of the ~4–5-families rule above, not a separate step). This isn't a hard requirement (a genuinely rare/scattered species is fine to split, and it's not always possible), but a table where every line's second half lives in a different table reads as unauthored the same way an all-singles table does. Audit this the same way as reachability: for every evolution step where both stages are placed *somewhere* in the region, check they share at least one table — and when redesigning a table, watch for orphaning a species that used to be there (removing a base-stage's only appearance because its slot got reassigned, while its evolution is still shown elsewhere with nothing under it).
- **Every species needs at least one path into the player's hands.** Cross-check the full dex against every encounter table: a species that's evolution-only (reachable by leveling/stone/trading an already-placed lower stage) is fine to leave out; a species that's neither in any wild table *nor* reachable by evolving something that is, is a dead end. This is easy to miss on one species out of ~200 — check it with a script, not by eye (build the reverse index, §15 item 5, and see what's missing).
- **Encounter table slots store the literal target species**, already at whatever evolutionary stage that slot's rarity implies — a high slot can name an evolved form directly, and a low-level encounter against that slot downgrades via `predecessor`/`minLevel` (§16), not by storing the base species and resolving up. This is the opposite convention from trainer-class/gym/Elite Four tables (§7–9), which store the *base* species and resolve the displayed stage from level at reveal time. Don't mix the two.

---

## 7. Route Trainers (Generic Trainer Classes)

- Design ~10–12 recurring trainer archetypes that could plausibly appear on **any** route in the region, regardless of terrain.
  - **Reject classes that require specific terrain to make sense as a concept** — a Swimmer or Fisherman literally cannot exist on a landlocked desert route; a Hiker, Camper, Cyclist, or Backpacker can exist anywhere. This is a stricter bar than "does this Pokémon type fit here" — it's about whether the *trainer's identity* is terrain-independent.
- **Map each class to one number on the 2–12 wheel** (plain 2d6 roll, no reroll-above-threshold — trainer *type* doesn't need to scale with badges the way encounter rarity does). Assign the numbers so that **the most generic/everyday class sits at 7** (the most probable roll) and the most unusual/elite class(es) sit at the 2/12 extremes — matching flavor to actual frequency, not the reverse.
- **Give each class its own 11-slot table of Pokémon lines** (one per sum 2–12), with **no repeated line within a single class's own table** (reuse across *different* classes is fine and realistic). Order each class's own table the same way: the class's single most "typical" pick at 7, its most surprising/rare pick at the 2/12 extremes.
- **Store the base (root) species per slot, not a pre-resolved stage.** This table only ever names the unevolved species; the actual battle stage is resolved from the trainer's level at reveal time (§11). It's easy to get backwards — writing the evolved name directly *looks* more informative, but then a low-level trainer would show something already evolved that shouldn't be reachable yet, and it silently duplicates a line that's actually the same family as another slot's base form.
- **Team size scales with badge tier** (same tiering shape as the core threshold table: 1 Pokémon at 0 badges, 2 at 1–2, 3 at 3–4, 4 at 5–6, 5 at 7–8).
- **Level is a flat formula, not tiered:** typically `base + exact badge count` (not bucketed the way team size is) — so level increases every single badge, even within a team-size tier.
- **Level-appropriate evolution:** once a line is rolled, resolve it to whichever stage the trainer's actual level supports (see §11) — don't just hand the roll a random stage.

---

## 8. Gym Leaders

- **8 gyms is the traditional count** — spread them across **geographically distinct towns**, not clustered near each other, so "any order" play actually produces varied early routes depending on player choice.
- **Type coverage:** give each gym a distinct type. It's fine — good, even — if this requires reshuffling which towns host gyms partway through design; town identity (its dex habitat) doesn't have to dictate gym type if the fiction can flex (e.g., a leader can be described as importing Pokémon from elsewhere in the region).
- **Any order means no story gate, on any gym, including a "final" one.** If the source material gates one gym behind other progress (an antagonist team's storyline, etc.), that gate either needs an in-fiction resolution before this region's story even starts, or needs to just not exist — a gym that's mechanically gated by badge count anyway (§5) doesn't need a *second*, story-based gate on top.
- **5-slot team structure, with fixed send-order and per-slot rules:**
  - Slots **1 and 2** are always present (these are the leader's "core" — slot 1 is conventionally the Ace).
  - Slot **3** unlocks once the player has **2+ badges already claimed** (before this fight).
  - Slot **4** unlocks at **4+ badges claimed**.
  - Slot **5** unlocks at **6+ badges claimed**.
  - **Send-out order is 5, 2, 3, 4, 1** — the newest/weakest-feeling addition (slot 5) goes out first, the Ace (slot 1) is saved for last.
  - **Levels:** slots 1 and 4 = `badges + 5`; slots 2 and 3 = `badges + 4`; slot 5 = `badges + 3`. (Levels use the *exact* badge count, not a tier — same principle as trainer levels.)
- **Attack type is a fixed rule per slot, not a random roll**, once you want gym battles to feel authored rather than randomized:
  - Slots 1 and 5 → the Pokémon's unmodified **primary type**.
  - Slot 2 → the Pokémon's **secondary type** — falling back to primary if the Pokémon is monotype.
  - Slot 3 → **one step clockwise** on the 18-type wheel from primary (see §13).
  - Slot 4 → **one step counterclockwise** from primary.
- Assign each leader **one signature evolution line per slot** (5 lines total), not a randomized species pool — gym leaders should feel authored and consistent every time you fight them, unlike route trainers. Same base-species-per-slot storage convention as §7; a slot's stored value can be a `[species, forcedBranch]` pair (§11) when the leader's type identity clearly wants one specific evolution out of a branching line.

---

## 9. Elite Four & Champion

- **4 Elite Four members + 1 Champion**, based at the region's final city.
- **Cover the types the 8 gyms don't.** If you have leftover "retired" concepts (e.g., you originally planned a gym for a type that got reassigned), consider promoting them to Elite Four members instead of inventing new characters from scratch — keeps continuity. If a member's specialty type stops being covered elsewhere for real in-fiction reasons (a gym's type changed), that's a good, citable reason to move the member to whatever type just opened up.
- **Elite Four members: 5-slot roster** (numbered slots 1–4, plus a separate Ace) — same fixed-level, fixed-type-per-slot logic as gyms (slot 2 = secondary type [primary if monotype], slot 3 = clockwise shift, slot 4 = counterclockwise, everything else = primary). Same base-species-per-slot storage convention as §7–8.
- **Champion: 6-slot roster** (slots 1–5, plus Ace) — same rules extended by one slot. The Champion doesn't need a fixed type at all — a roster that draws one flagship species per major region cluster, rather than doubling up on any Elite Four member's line, reads better than forcing a theme.
- **Fixed levels, no badge-gating** — once the player has beaten all 8 gyms, the full Elite Four/Champion roster is always available exactly as designed, unlike gym teams which scale with progress.
- **Sanity-check the level ceiling.** If your level formula caps out at some maximum (e.g., "3 + badges" caps at 11 once badges max at 8), don't assign an Elite Four/Champion Pokémon a final evolution that requires a level *above* that ceiling — it'll never actually appear. Pick a line whose evolution requirement fits within reach, even if that means passing on a flashier pseudo-legendary. (It's fine, and can be a deliberate narrative beat, for a *gym* leader's own line to fall just short of a level a *Champion* slot can reach — the Champion's roster finishing an evolution arc a gym leader's own team never quite completes is a nice piece of texture, not a bug, as long as it's intentional.)
- **Once all gyms are cleared, remove them from re-selection** — a player who's beaten all 8 gyms shouldn't be able to re-fight them from the same menu that offers the Elite Four; separate "cleared" state from "available" state.

---

## 10. The 1–20 Level Scale

If you want a lower-fidelity level system instead of 1–100, build an explicit bucket table and use it everywhere — never eyeball a conversion.

| New Lv | Old Lv range | New Lv | Old Lv range |
|---|---|---|---|
| 1 | 1–3 | 11 | 41–45 |
| 2 | 4–6 | 12 | 46–50 |
| 3 | 7–9 | 13 | 51–55 |
| 4 | 10–13 | 14 | 56–61 |
| 5 | 14–17 | 15 | 62–67 |
| 6 | 18–21 | 16 | 68–73 |
| 7 | 22–25 | 17 | 74–79 |
| 8 | 26–30 | 18 | 80–86 |
| 9 | 31–35 | 19 | 87–93 |
| 10 | 36–40 | 20 | 94–100 |

Buckets don't need to be perfectly even — they're deliberately compressed at the low end (fast early leveling) and stretched at the high end (grindier endgame), matching how mainline leveling curves actually feel.

---

## 11. Evolution Rules on the Compressed Scale

- **Level-based evolutions:** convert the mainline level directly through the bucket table above.
- **Non-level evolutions** (stone, trade, friendship, special conditions) have no inherent level in the source games — you have to invent a "proper level" for them, or two problems occur: (a) they can appear absurdly early in a level-gated system, and (b) if your system ever needs to pick "the best stage reachable at level X," a non-level evolution with the *same* threshold as its pre-evolution will instantly and permanently obsolete that pre-evolution, which is both wrong and boring.
- **Friendship evolutions have a concrete trigger, not a hidden stat.** This game tracks no friendship value, so "high friendship" instead means: the Pokémon evolves the first time it levels up to at least 4 levels above the level it was obtained at (caught, received as a starter, hatched, or traded in) — a simple, player-trackable stand-in for bonding over time. Note the level the Pokémon was obtained at when you get it.
- **The fix — a buffer rule:** a non-level evolution's proper level = its pre-evolution's proper level **+ a fixed buffer**. A buffer that scales (e.g., +6 if the pre-evolution is level 1, +3 for anything higher) reads more naturally than one flat number, since a Pokémon evolving from its very first stage should get more runway than one evolving from something already mid-progression.
- **Split evolutions with one level-based sibling are the exception to the buffer.** When a pre-evolution branches into two (or more) evolutions and *at least one* branch is a normal level-up evolution, give every non-level sibling in that same split the **level-based sibling's own level** instead of computing a buffer — don't make same-tier siblings appear at different levels than the branch that got there by leveling normally, for no in-fiction reason. Only fall back to the scaling buffer when *no* branch in the split is level-based (Eevee's stone/friendship evolutions, for instance, have no level-up sibling to borrow from, so all of them use the buffer).
- **Chain these calculations in dependency order** when a family has back-to-back non-level steps (e.g., base → non-level stage 2 → non-level stage 3) — the second buffer must be computed from the *already-buffered* first result, not from the original mainline data, or you'll under-count.
- **Keep this buffered ruleset separate from your "true" wild-encounter evolution data** if the two systems have different needs — wild encounters may only need a simple downgrade-if-too-low check (which never breaks even with zero buffer), while a trainer/gym system that always shows "the best reachable stage" absolutely needs the buffer. Don't let a fix for one system silently corrupt the other. (Concretely: this is `minLevel` vs. `trainerMinLevel`, §16 — a non-level evolution's `minLevel` just inherits its predecessor's own `minLevel` unchanged, no buffer, while its `trainerMinLevel` gets the buffer or the borrowed sibling level above.)
- **Branching evolutions need an explicit resolution rule for trainer/gym rosters**, not just a data note about which species exist. Default to a **random pick among whichever branches are level-eligible** at reveal time — checked *per branch option*, not once for the whole group, since branches can clear their threshold at different points (a trade-buffered sibling and a level-based sibling won't always share one, even after the rule above; check each option's own threshold independently). Reserve a **forced-branch override** for the specific roster slots where one evolution is clearly the intended pick (a Water-specialist gym leader's mid-evolution should probably become the pure-Water branch, not a coin flip that might hand them an off-type Pokémon) — implement it as a `[species, forcedBranch]` pair on that one slot only, so every *other* reference to the same species elsewhere in the region stays randomized rather than hardcoding the branch at the species level.

---

## 12. Stat System ("Die Faces")

- Convert each Pokémon's six base stats (HP/Atk/Def/SpA/SpD/Spe or your own six) into abstracted "die faces": **divide by a fixed divisor (17.5 worked well for a 1–20 level range) and round to the nearest integer.**
- **Zero is a legal result** — don't floor it to 1. A very low stat producing a literal 0-face is thematically appropriate for a few outlier Pokémon and doesn't break anything.
- **Level-based stat growth, without traditional stat formulas:** at a given level, distribute that many bonus points across the six dice by **rank order — highest die first, then second-highest, cycling back through all six once exhausted.** Break ties randomly (don't always favor the same stat position). This gives believable, non-linear growth without needing real stat-growth math.
- **Wild encounters vs. trainer/gym Pokémon can present this differently:** for a single showcased Pokémon, it's nice to show the base number and the bonus separately (transparency); for a roster of many Pokémon at once, just show the pre-combined final number plus a total, or the UI gets too noisy.

---

## 13. Attack-Type Wheel (Optional Flavor Mechanic)

A way to add combat-flavor randomness without inventing a full type-effectiveness engine:

- Arrange all 18 types in a fixed ring, e.g.: `dragon → flying → normal → fighting → dark → fire → ghost → psychic → fairy → grass → poison → bug → electric → steel → rock → ground → ice → water → (back to dragon)`.
- **Wild/trainer Pokémon:** pick a random "base type" from the Pokémon's actual type(s) (a 50/50 if dual-typed), then roll 2d6 and shift around the wheel: 2 → 2 steps back, 3–4 → 1 step back, 5–9 → stay, 10–11 → 1 step forward, 12 → 2 steps forward.
- **Authored Pokémon (gym leaders, Elite Four) can use a fixed version of the same idea instead of rolling**, if you want their battles to be deterministic and repeatable rather than randomized — e.g., "this slot always uses primary type, that slot always shifts one step clockwise."
- Always keep a Pokémon's **true type(s)** visibly separate from its rolled/assigned **attack type** in any display — conflating the two is confusing, especially when they happen to coincide.

---

## 14. Battle Resolution System (1v1 Combat)

A lightweight, single-roll combat model for resolving "which of these two Pokémon wins this exchange" — not a full HP/damage engine. Built entirely from pieces §12–13 already established: die-face stats plus their level bonus points, and the true-type/attack-type split.

- **Combat stat = the six die-face numbers, level bonus already included** (§12) — the same six numbers already shown for the Pokémon elsewhere in the tool. No separate combat-only stat block.
- **Each side rolls 1d6 to pick which of its own six stats is in play** — 1→HP, 2→Atk, 3→Def, 4→SpA, 5→SpD, 6→Spe (the fixed die-face order from §12). The side's **result is the *value* of the picked stat, not the raw 1–6 roll** — a Pokémon with die-faces `[6,7,3,5,4,7]` rolling a 6 contributes a result of 7 (its Speed die-face), not 6.
- **Type effectiveness changes how many of these picks are rolled and whether the best or worst is kept.** This needs a real 18-type effectiveness chart (the attacker's attack type against every one of the defender's 1–2 true types, multiplied together the normal way) — the §13 wheel only ever *picks* an attack type and was never meant to resolve effectiveness, so don't reuse it for this.

  | Effectiveness | Picks rolled | Keep |
  |---|---|---|
  | 4× (doubly super effective) | 3 | best |
  | 2× (super effective) | 2 | best |
  | 1× (normal) | 1 | — |
  | 0.5× (not very effective) | 2 | worst |
  | 0.25× (doubly resisted) | 3 | worst |
  | 0× (immune) | 3 | worst |

  Each "pick" is a full independent stat-selection roll (previous bullet), not a plain 1–6 number — "2 picks, keep best" means rolling which-stat twice, independently (repeats allowed), and keeping the higher of the two resulting stat *values*.
- **STAB (Same-Type Attack Bonus): +1 to the final result** if the Pokémon's attack type matches one of its own true types. This is exactly why §13 insists on keeping true type and rolled/assigned attack type visibly distinct — STAB is the one place that distinction has a mechanical payoff, not just a display nicety.
- **Resolution: higher final result wins the exchange. A tie means both sides reroll** (same stats, same dice config, same STAB — just roll again) rather than counting as a draw, so **there is effectively no such thing as a true tie** in the reported odds. Because rerolls are independent and identically distributed, don't actually simulate repeated rounds to get this — the post-reroll win odds are just the one-shot P(A wins) and P(B wins), each renormalized by `1 / (1 − P(tie))`. The only case that genuinely never resolves is a **perpetual tie**: every possible pairing of outcomes ties (P(tie) = 1), which only happens when both sides' results are deterministically identical on *every* conceivable roll — e.g., two Pokémon with all six stats equal to the same number, same effectiveness, same STAB. That's the one case worth a special "this can never be decided" message instead of a 0%/100% split. Because every input is a small number of d6-scale picks, the full outcome space is always small enough to enumerate exactly rather than simulate — worst case (3 picks vs. 3 picks) is only 6³ × 6³ = 46,656 pairings, so exact odds are cheap to compute by brute force.
- **This is a single opposed roll, not a multi-round battle with HP loss** — deliberately so, matching the granularity everything else in this ruleset already works at (one roll decides one outcome). A region wanting a full turn-based battle system needs a different mechanic; this one only answers "who wins this exchange."
- **Implement the battle simulator as another tab in the same file, not a separate page.** It doesn't need a region loaded to be useful — a species picker backed by a fixed base-stat table (real Pokémon, or a region's own `types`/`dice` data, §16) lets the user select any two Pokémon, assign bonus points per stat (their sum *is* the Pokémon's effective level, §10, §12 — don't make the user set level and bonus points separately, one derives the other), assign each an attack type, and see exact win/loss/tie percentages computed by enumeration rather than by sampling. **This was first built as its own file, cross-linked via `?param=...` URL round-tripping (serialize the roller's state into the URL, read it back on load) so "Send to Battle Sim" could hand over a specific Pokémon and "back" could restore where the user left off — it worked, but the round-trip machinery (`captureState`/`restoreState`, URL-encoding every field, re-parsing on load) was pure overhead that a same-page tab needs none of:** a "Send to Battle Sim" button becomes a plain function call (fill the fields, switch tabs) instead of a navigation, and the roller's state simply persists because the DOM was never torn down. Reach for a separate file only if the simulator genuinely needs to be usable with no region loaded *and* linked to from outside the tool; inside one tool, tabs win.
- **A "caught Pokémon" collection (localStorage, one shared key/shape across every tab) is the natural bridge between the roller and the battle simulator.** A "Catch!" button next to every rolled **wild** Pokémon and a "Save to [collection]" button on each battle-sim side both write to the same list; a "Load from [collection]" picker on each battle-sim side and "send to side A/B" buttons on each collection entry both read from it. Bonus points (EVs) and attack type should stay editable per saved entry (a player's plans for a caught Pokémon change), but base stats shouldn't (same rule as everywhere else, §12) — lock those inputs by the same real-species check used elsewhere. A wild catch should disable itself after use (catching the same specific encounter twice makes no sense).
- **Trainer and gym/Elite Four/Champion Pokémon don't get a "Catch!" button at all** — you can't catch another trainer's own Pokémon, same as mainline. Those rows only get "Send to Battle Sim"; a player who wants to save that exact rolled statline anyway can still route it through the battle simulator's "Save to My Pokemon," which is a general-purpose "bank this build" action rather than a claim that the Pokémon was caught.
- **An "Evolve" button on each collection entry, gated only where the app can actually check the gate.** Parse the evolution data's method text for an explicit level (e.g. "Level 9", or a stone/trade method's own buffered "→ Level N" where a region embeds one); if the entry's level (bonus-point sum) meets it, show a real "Evolve" button, otherwise show a disabled hint ("→ Vileplume at Lv 9") instead of hiding the option entirely. Item, trade, and friendship conditions the app has no way to verify (does the player actually have a Razor Claw? did they trade?) get their button shown unconditionally rather than blocked — same trust-the-player stance as EVs and attack type. **Branching evolutions need every eligible branch offered as its own button, not just the first one** — a species with two evolutions (one level-gated, one item-gated) will often have only one branch ready at a time; show whichever are ready and hint at the rest, rather than picking one for the player. On evolve: update name/types/base stats from the region's own `types`/`dice` tables (not the real-species lookup table used for the add form), keep the entry's id/EVs/attack type/note/caught-date unchanged — evolution doesn't reset progress. **Offer, don't force, an attack-type reroll on evolve:** after the evolution itself is confirmed, a second confirmation asks whether to reroll the attack type for the new species' true type(s) using the same 2d6 wheel spin as everywhere else (§13) — decline and the old attack type (which may no longer even be one of the new types) is left alone, since a player may want to keep it intentionally.
- **Attack-type dropdowns (anywhere the player picks one directly — collection entries, battle simulator sides, the manual-add form) are ordered around the wheel itself (Dragon → ... → Water, §13's `WHEEL` array), not alphabetically** — the dropdown then visually matches the mechanic that moves through it, unlike a species' *true* type1/type2 selects, which stay alphabetical since there's no wheel relationship to preserve there.
- **Offer an "auto-distribute by level" shortcut alongside manual per-stat bonus-point entry.** Given a target level, apply the same rank-order rule §12 already defines for wild/trainer Pokémon (highest stat gets the first point, then the next-highest, cycling through all six once exhausted, ties broken randomly) rather than inventing a second distribution rule just for the simulator — the whole point of the simulator is to reflect how bonus points actually land on a real roster Pokémon, and manual entry should stay available for testing hypothetical builds a real rolled Pokémon couldn't have.

---

## 15. Reference Doc Set

Author these per region as the human-readable spec — they're what the digital tool's data gets built *from*, not something the tool loads at runtime. Build them roughly in this order, since later docs key off earlier ones:

1. **Regional Dex** — every species, numbered, typed, habitat-grouped, with die-face stats (§2–4, §12) *all in one doc and one table*. This is the canonical ID/name source everything else keys off — build it first. Don't split this into a numbered dex, a separate narrative habitat write-up, and a separate stats doc — that's three copies of the same ~200 rows, and they will drift out of sync the first time the dex changes (this happened during Neo Kanto's build: a species-count mismatch between two supposedly-identical docs). One table, with a one-line italic flavor blurb per habitat section if you want the narrative texture.
2. **Dungeons** — the route/dungeon list and connectivity graph (§1). Needed before Encounter Tables, since tables are organized per location.
3. **Evolution Guide** — every evolution's level/method on the compressed scale (§10–11), grouped the same way as the Regional Dex.
4. **Encounter Tables** — the 11-slot table per route/dungeon (§6). The biggest doc; build it in geographic passes rather than all at once, and re-verify the document's own section structure (routes vs. dungeons) once it's fully assembled — appending in passes is exactly how a table can end up nested under the wrong heading.
5. **Pokémon Locations** — reverse index of Encounter Tables (species → every location/slot it appears in). **Generate this from the finished Encounter Tables programmatically** rather than hand-compiling it — it's mechanically derived data, and this is also the natural place the "every species obtainable" audit (§6) falls out for free: anything with no entry here and no evolution to fall back on is the bug to fix.
6. **Trainer Classes** — the terrain-neutral roster tables (§7).
7. **Gym Leaders** — the 8 rosters (§8).
8. **Elite Four & Champion** — the top-tier rosters (§9).

Don't build a separate "stats for gym Pokémon only" doc — it's a strict subset of the Regional Dex with no unique data of its own (this was cut from both Calli and Neo Kanto's doc sets for exactly this reason). A one-line pointer from Gym Leaders / Elite Four ("stats: see Regional Dex") does the same job with nothing left to keep in sync.

**Encounter Tables and Pokémon Locations are the one pair that looks like the same redundancy but isn't** — they answer opposite queries (what's at this place vs. where's this species), both genuinely useful, and merging them makes both harder to scan. Split docs are the right call when they serve different lookup directions; merge only when one is a strict subset or reformatting of the other.

---

## 16. Digital Companion Tool: Data Schema

If the region feeds an interactive tool, this is the data shape that's been built and proven across two regions in the same tool — use it rather than inventing a new one per region.

**One region = one object**, with these top-level keys:

- `locations[]` — `{id, name, kind:'route'|'dungeon', subtitle, flavor, table:[11 species]}`, one entry per Encounter Tables location. `table[i]` is indexed by `sum-2` (sum 2 → index 0 … sum 12 → index 10) and holds the *literal target species* for that slot (§6).
- `types` — `species -> [type1, type2|null]`.
- `dice` — `species -> [6 numbers]`, the die-face stat conversion (§12).
- `predecessor` — `species -> its immediate pre-evolution species, or null for a base`. Used to downgrade a wild encounter: if a table names an evolved species but the encountering trainer is under-level, step back through `predecessor` (checking `minLevel` at each stage) until you land on one the level actually supports.
- `minLevel` — `species -> the level it's valid from`, for that wild-encounter downgrade. **A non-level evolution's entry here just inherits its predecessor's own `minLevel` unchanged — no buffer.** Wild encounters don't need one; a plain downgrade-if-too-low check never breaks even at zero buffer (§11).
- `trainerMinLevel` — same shape as `minLevel`, but this is where the full buffer rule (§11) applies: base+6, prior-stage+3, or a level-based sibling's own level for splits. **Keep this a genuinely separate map from `minLevel`**, even though most entries end up numerically identical — the two systems' needs diverge exactly on non-level evolutions, and a shared table with exceptions bolted on will drift the first time someone edits one without the other.
- `trainerChain` — `root species -> ordered array of stages from base to final`, used to resolve a trainer/gym roster slot (which always names a *base* species, §7–9) to the correct stage at a given level. **A stage can itself be an array** at a branch point — pick randomly among whichever branch options are individually level-eligible (checked against `trainerMinLevel` per option, not once for the group), unless the slot supplies a forced override (below).
- `trainerClassBySum` — `'2'..'12' (string) -> class name` (§7).
- `trainerTables` — `class name -> 11-slot array of base species` (§7). A slot's value is normally a plain species string; it can also be a `[species, forcedBranch]` pair to force one specific evolution branch for that single occurrence, without touching the random default anywhere else that species is referenced.
- `gymLeaders` — `leader name -> {town, type, slots:{'1'..'5': base species or [species, forcedBranch]}}` (§8). No explicit level field — level is computed live from the player's current badge count via the §8 formula, not stored.
- `eliteFour[]` — `{id, name, type, slots:{'1'..'4','ace': base species or pair}, levels:{'1'..'4','ace': number}}` (§9) — levels *are* stored here, since Elite Four rosters are fixed rather than badge-derived.
- `champion` — same shape as one `eliteFour` entry, but slots `'1'..'5','ace'`.
- `mapNodes[]` — `{id, kind:'town'|'dungeon'|'route'|'combo', row, col, name, gymLeader?, locationId?}`, plus the combo-specific fields (§1) when `kind==='combo'`. Route nodes that share one named route (e.g. `9a`/`9b`) all point their `locationId` at the same `locations[]` entry.
- `mapEdges[]` — `[nodeIdA, nodeIdB]` pairs, generated from grid adjacency plus explicit exclusions (§1) rather than hand-listed — hand-listing ~80 edges is exactly where a typo produces a silently-disconnected node.
- `evoInfo` — `species -> {evolvesFrom, evolvesInto:[{species, method}]}`, for display only (a Pokédex-entry-style "evolves into X via Y" line). Unlike `trainerChain`, this should list *every* branch a species has, not just the one `trainerChain`'s resolution path happens to follow.

**Multiple regions in one tool:** wrap regions as `REGIONS = { regionKey: { label, data: {...above shape...} } }`, with one `DATA` variable reassigned to `REGIONS[currentKey].data` on switch — every function that already reads `DATA.whatever` keeps working unchanged. Derive anything that assumes a fixed count (gym-count-driven badge-pip loops, threshold-tier boundaries, the Elite-Four-unlock gate) from `Object.keys(DATA.gymLeaders).length` rather than a hardcoded number — cheap to do up front, and each region can then genuinely have a different gym count later without a second code path. On region switch: reset per-region UI state (badges, location/gym selection, search text, map node selection) but keep the active tab; explicitly clear anything rendered from the old region's data (map SVG contents, dex list) before rebuilding, rather than assuming a fresh build call will overwrite it — an SVG that's only ever been built once in the app's history won't have a clear-before-redraw step unless you add one, and it'll silently duplicate every node the second time it runs.

**Two implementation traps worth naming, since both caused real bugs:** binding a DOM event listener inside a function that can now run more than once (once per region switch) stacks duplicate listeners — bind search/filter listeners once at load instead, reading fresh state each time they fire. And SVG presentation-attribute transforms don't combine with a CSS `transform` on the same element — the CSS one silently wins; if a shape needs both a static rotation and a hover/selection effect, put them on different layers.

---

## 17. Build Process & Validation

The order that actually worked, end to end, building a second region into an existing tool:

1. **Worldbuilding proposal first, as its own reviewable document** — premise, dex composition (what's kept/cut/added and why), town/gym changes, Elite Four. Get this read and locked before building any mechanical data on top of it; re-deriving a whole dex because a premise detail changed later is expensive. Expect more than one review round.
2. **Map next**, once the premise is settled — town/gym locations depend on premise decisions, so translating the node graph (§1) into an actual grid earlier just means redoing it.
3. **Reference docs in dependency order** (§15).
4. **Implement by parsing the docs into the schema (§16) programmatically, not by hand-transcribing.** A script that reads the Regional Dex table and Encounter Tables and emits the region's data object is slower to write once than typing the same thing by hand, but it can't introduce a transcription typo, and re-running it after a doc edit is free. Hand-transcribing ~200 species across a dozen tables *will* introduce at least one silent mismatch — this is precisely how a "confirm both docs agree" bug (§15 item 1) happens in the first place.
5. **Validate with scripts before trusting the result, specifically:**
   - Every species named in every encounter table / trainer table / gym / Elite Four / Champion roster exists in `types` and `dice`.
   - Every `locationId` referenced by a map node resolves to a real `locations[]` entry, and vice versa.
   - The map graph is fully connected from the starting town (walk `mapEdges` from the start node; anything unvisited is orphaned).
   - No species is both absent from every encounter table *and* unreachable by evolving anything that is placed (§6) — the one bug class that silently makes a dex entry permanently uncatchable, and the easiest one to miss by eye across ~200 species.
   - Total dex count matches the sum of its parts (kept + added + legendary, etc.) — recompute this after every edit that adds or removes a species; it drifts fast when checked by eye instead of arithmetic.
6. **When a region is based on real Pokémon (an official dex, not an invented one), source base stats from one consolidated table page — PokémonDB's or Bulbapedia's complete base-stats table — not by fetching each species' individual page.** A per-species page fetch for ~200 species is enormously slower (and burns far more research budget) than one table load followed by lookups within it. Evolution methods/levels are the exception: those aren't reliably consolidated into one table across ~200 species, so per-family lookups (or a per-family Bulbapedia/Serebii page) are still the right tool there.
7. **Test the shipped tool in an actual browser, not just by reading the code.** Load it, switch regions if applicable, roll a wild encounter, roll a trainer/gym battle at a few different badge counts (including enough to trigger a branch point, §11), click through the map. A logic error in level-resolution or branch-selection code often only shows up when the code path actually runs — reading it rarely catches what running it does. Where possible, drive this with an automated script (fill inputs, click, read the resulting DOM) rather than only eyeballing screenshots, so the same check can be re-run after the next change for free.

---

## 18. Playable Game Mode (Optional)

A "Field Guide" (roll encounters, browse the dex, simulate battles, track a collection — everything §1-17 describe) is a reference/practice tool: every roll is a stateless "what would happen if," with no turns, no board position, no win/loss consequence. A **playable game** layers real turn-by-turn structure on top: a starting choice, movement across the region map, and encounters with actual stakes (catch-or-defeat, win-or-lose, experience and leveling).

- **Build it as its own file, not a tab in the Field Guide.** Unlike the battle simulator (§14, which benefited from being a same-page tab since it shares live state with the roller), a playable game is a fundamentally different *mode* with its own persistent save state, turn loop, and win/loss stakes — bolting that onto the reference tool risks the tool itself, and a player who just wants to look something up shouldn't have to load an entire game engine to do it. Link the two pages to each other in their mastheads.
- **Give the game its own Pokémon collection, separate from the Field Guide's.** It's tempting to share the Field Guide's "My Pokemon" `localStorage` key verbatim — one collection, editable/evolvable from either page for free — but a real playthrough and a reference tool's practice collection are different things conceptually: the Field Guide's list is scratch space for testing builds and shouldn't accumulate a specific playthrough's roster, and a *new* game needs to start with a clean team, not whatever the Field Guide happened to have saved. Give the game its own collection key, and clear it (not the Field Guide's) whenever a new game actually begins. It still needs its own *separate* key for board/turn state too (current position, badges/gyms beaten, whether a starter's been chosen) — the Field Guide's own `badges` counter is a throwaway in-memory variable, not something a real save should depend on.
- **Data duplication vs. a shared file is a real tradeoff, not a default.** The game needs the same region `DATA` (dex, encounter tables, trainer/gym logic) and small pure helper functions (wheel shift, dice-roll resolution, level-bonus distribution, stage-for-level resolution) the Field Guide already has. Extracting these into one external file both pages load is the cleaner long-term structure, but touches the Field Guide's working code to do it. Duplicating them into the new file is zero-risk to the tool that's already been tested, at the cost of two places to update on a future data edit — reasonable for a first version, worth revisiting once the game concept is proven out.
- **Movement is a die roll against the region map's own graph (§1), not a track** — after rolling N, compute every node within 1..N steps by shortest path (a simple BFS from the current node) and highlight all of them on the actual region map (reusing the Field Guide's own SVG rendering, §"Region Map" — a player-position ring plus a "can move here" glow), rather than a text list of place names. **The player's own current tile is never one of the offered destinations** — a turn has to go somewhere, so distance-0 is excluded from the reachable set entirely. **Unused movement becomes a resource, not a penalty**: picking a tile closer than the full roll forfeits the difference, which the player can then trade for a same-turn advantage elsewhere (this project's Calli game lets forfeited spaces shift a route's wild-vs-trainer roll in either direction) rather than just losing it.
- **Clicking a map node should open an info panel, not immediately act on it** — a player exploring the map wants to check *any* node (not just ones currently reachable) for what it has: gym leader and whether already beaten, co-located dungeon, evolution items, and the actual wild encounter table for a route/dungeon (the Field Guide's own `buildEncounterPreviewHtml`, keyed off `locationId`/`dungeonLocationId`, drops in verbatim). Reserve the actual move for an explicit button *inside* that panel, shown only when the panel's node is currently reachable — this cleanly separates "let me look at this" from "commit my whole turn to this," and means the same click handler and panel serve every node on the map, not just the glowing ones.
- **Tile-type resolution should read the map node's own fields, not hardcode assumptions**: a town/city offers whichever of {dungeon encounter, evolution item, gym battle} it actually has (a `combo` node's co-located dungeon, `evoItems[townId]` being non-empty, `gymLeader` being set) rather than assuming every town has all three; a plain town with none of those is a legitimate no-op tile.
- **Reuse the Field Guide's existing single-roll battle resolution (§14) for every individual matchup**, wild or trainer or gym — don't invent a second combat system. A wild encounter, a trainer's roster, and a gym leader's roster are all "a sequence of one-or-more opposing Pokémon, resolved one single-roll exchange at a time," so one encounter engine handles all three; only team generation and the post-win outcome (catch-or-defeat vs. automatic XP-and-advance) differ per type. **Watch the type-effectiveness wiring specifically**: each side's dice count/keep-best-or-worst depends on how effective *that side's attack type* is against the *other* side's defending types — `diceConfig(effectiveness(atkA, typesB[0], typesB[1]))`, not `typesA`. Copying the battle math without copying this cross-reference correctly is an easy, silent mistake (both sides still "battle," results just come out wrong) — this project's own first pass at it got exactly this backwards and it took a deliberate statistical test (identical stats, an engineered 4x-vs-1x matchup, dozens of trials, checking the win rate landed well above 50%) to catch, since reading the code alone didn't make the swap obvious.
- **A town's evolution item should offer to *evolve a Pokémon that needs it*, not "receive the item" as an inert flavor action.** For each of the player's Pokémon, check whether any of its evolution branches' method text names an item the town actually has (same substring match the Field Guide uses to link an item to its town, §"caught Pokémon collection" evolve pattern above); only surface the action at all when at least one such match exists, and let the player choose which specific (Pokémon, branch) pair to resolve if more than one qualifies. This reuses the same evolve mechanics as the collection's own Evolve button (species/types/base update, keep id/EVs/attack type/XP, optional attack-type reroll) rather than inventing a separate one.
- **Give the player a persistent, always-available way to check their own roster, separate from any one encounter's "who do I send out" picker.** A collapsible "My Pokemon" section (closed by default, a simple toggle) listing every owned Pokémon's types, attack type, full stat-by-stat breakdown, total, and current XP progress serves this — and the *same* per-Pokémon card markup should back both that roster view and the in-encounter picker, so there's exactly one place that knows how to render "one Pokémon's full detail," not two that can drift apart. Show every stat directly on the card rather than behind a select-then-reveal step — a native `<select>` can't render more than plain text per option anyway, and a plain list of full cards is both simpler to build and easier to compare at a glance than a dropdown.
- **Level-up (and friendship) evolution triggers automatically right after the XP grant that causes it, not through a player-facing button.** Check every XP recipient immediately after `grantXP` — a plain-level branch just needs `level >= evoRequiredLevel(method)`; a friendship branch (§11's "level ≥ level obtained at + 4") needs the Pokémon's level at catch/creation time recorded up front (`catchLevel`, set once, never touched again — including through later evolutions) so the check has something to compare against. Loop rather than checking once: a single big XP grant can cross more than one evolution's threshold in a row (e.g. hitting a plain-level branch's requirement can immediately also satisfy the *next* stage's friendship requirement), and each stage should resolve before the next is checked, same as a real cascading evolution would. The only player-facing choice here is the existing attack-type reroll (roll first, only ask if it actually differs) — the evolution itself is not optional, matching mainline. Surface what happened through whatever's already visible (the encounter log mid-battle, the outcome message otherwise) rather than a separate screen.
- **A defeated Pokémon's stat total (base + bonus points) is the experience it grants** — a wild one grants that total as-is, a trainer/gym one grants it ×1.5 rounded down (defeating someone else's trained Pokémon is worth more). When more than one of the player's Pokémon fought that *specific* opponent (because earlier ones lost first), **split the XP as evenly as possible, giving any remainder to whichever Pokémon were sent out later** (`base = floor(total/n)`, then the last `remainder` entries in send order each get `+1`) — this rewards the Pokémon that actually landed the final blow slightly more without shutting out the ones that chipped in first.
- **Leveling up needs no new state if level is already defined as the bonus-point sum (§10, §12)**: give the Pokémon `xp` (a single leftover-progress counter) and level up in a loop — `while(xp >= level*10){ xp -= level*10; award a bonus point; level += 1; }` — so a single large XP grant correctly cascades through multiple levels in one pass. The stat that gets the level-up's bonus point is the *defeated* Pokémon's own highest final stat (§"pick highest, ties random" pattern used elsewhere) — every level gained from one grant attributes to that same defeated Pokémon, since it's one battle's worth of experience, not several.
- **Every UI action that changes `view`/game state must re-render before returning — no exceptions, even on a branch that also happens to end the encounter.** The one real bug hit building this: two exit paths (a trainer/gym team's *final* member being defeated, and the player running out of usable Pokémon on a loss) updated the state variables and then `return`ed without calling the render function, leaving the last on-screen button wired to state that no longer existed — clicking it threw on the very next interaction instead of failing where the mistake was made. Prefer **one unconditional render call at the end of the function** over scattering `render(); return;` through every branch — a single call that always runs is much harder to accidentally skip than remembering it on each new early exit.

---

## Quick-Reference Checklist for a New Region

- [ ] Map: grid-based, orthogonal adjacency, verified fully connected, multiple entry paths into major clusters, co-located town/dungeon pairs merged into `combo` nodes
- [ ] Regional dex: ~200 Pokémon, 3 starters, 5–8 Legendaries/Mythicals, full evolution-line completeness, every species has at least one path into the player's hands
- [ ] Type balance audited against National Dex norms after every major edit
- [ ] Evolution-stage distribution checked (two-stage majority, ~30% three-stage, ~10% ordinary one-stage) — re-checked after unlocking any new evolution stages
- [ ] Core 2d6 + reroll-above-threshold mechanic defined, with badge-based threshold table (derived from the actual gym count, not hardcoded)
- [ ] Wild encounter tables per route/dungeon: ~4–5 evolution families per table (not 11 unrelated singles), each family's stages strictly low→high slot, rarity-consistent, sum-12 reserved for signature rare finds, literal target species stored per slot
- [ ] 10–12 terrain-neutral generic trainer classes, mapped to the wheel by actual frequency, base species (not pre-resolved stages) stored per slot
- [ ] 8 gym leaders, geographically spread, type-covered, 5-slot fixed-order roster rule, no story-based gate beyond the badge-count one, base species per slot
- [ ] Elite Four + Champion covering leftover types, fixed roster/levels, level-ceiling sanity-checked, base species per slot
- [ ] 1–20 (or other compressed) level scale with an explicit bucket-conversion table
- [ ] Non-level evolutions given a buffered "proper level" — or a level-based sibling's own level, for splits with one — computed in dependency order, kept separate from the wild-encounter (`minLevel`) version
- [ ] Branching evolutions resolve randomly among level-eligible options by default (checked per branch), with forced overrides only on the specific slots where flavor clearly demands one
- [ ] Stat-to-die conversion formula fixed, level-based bonus distribution rule defined
- [ ] (Optional) attack-type wheel mechanic for combat flavor
- [ ] (Optional) 1v1 battle resolution system defined (dice-vs-stat picks, type-effectiveness dice-count/best-worst table, STAB), backed by a real 18-type effectiveness chart rather than the attack-type wheel; battle simulator built as another tab in the same file (not a separate page + URL round-trip)
- [ ] (Optional) a "caught Pokémon" collection (shared localStorage key/shape), fed by a "Catch!" button on wild encounters only (no "Catch!" on trainer/gym/Elite Four/Champion rows — those only get "Send to Battle Sim") and a "Save" button on either battle-sim side, readable back into either battle-sim side; EVs and attack type editable per entry, base stats locked same as elsewhere
- [ ] (Optional) an "Evolve" button per collection entry — level-gated only where the evolution data has a checkable level, unconditionally offered where it doesn't (item/trade/friendship), every eligible branch of a split evolution shown as its own button
- [ ] (Optional) a playable game mode as its own separate page (not a Field Guide tab), with its own Pokémon-collection key (cleared on every new game) and its own save-state key, both separate from the Field Guide's; movement via BFS-reachable-tiles-highlighted-on-the-actual-map (never offering the player's own current tile) with a forfeit-for-advantage option; clicking any map node (reachable or not) opens an info panel with the encounter table, gym/dungeon/item info, and a "Move Here" button only when that node is currently reachable; tile resolution driven by the map node's own fields; a town's evolution item(s) offer to evolve an owned Pokémon that needs one, only when one exists; a collapsible "My Pokemon" roster and the in-encounter send-out picker share one full-detail card renderer; every matchup resolved through the existing single-roll battle system with dice count wired attacker-vs-*opponent's*-types (not its own); XP = defeated Pokémon's stat total (×1.5 for trainer/gym), split across multiple participants with remainder to the latest-sent; leveling as an `xp` counter plus a `while(xp >= level*10)` loop; level-up/friendship evolution checked automatically after every XP grant (a recorded `catchLevel` backs the friendship threshold), looping to catch multi-stage cascades, with only the attack-type reroll left as a player choice; one unconditional render call at the end of every state-changing function, not scattered before each early return
- [ ] Reference doc set built in dependency order (dex → dungeons → evolution guide → encounter tables → locations → trainer classes → gyms → Elite Four), one merged dex/stats doc, no gym-only stats doc
- [ ] Digital tool data built by parsing the docs programmatically, then validated (species existence, `locationId` resolution, graph connectivity, obtainability, dex-count arithmetic) before trusting it
- [ ] Shipped tool tested by actually driving it — region switch (if applicable), wild roll, trainer/gym roll at multiple badge counts, map interaction — not just read through
