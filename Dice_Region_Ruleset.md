# The Dice Region Ruleset
### A generalized framework for building Pokémon regions, extracted from the Calli project

This document strips out everything Calli-specific and keeps only the *rules* — so it can be reused to build a new region from scratch. Wherever a rule references a number (like "8 gyms" or "209 Pokémon"), treat it as a tunable default, not a hard requirement, unless marked "fixed."

---

## 1. Region Map Structure

- Design the map as a **node graph**, not a literal geographic map. Build it on a grid (rows × columns) where most cells are empty and a minority hold nodes.
- **Node types and IDs:**
  - **Towns/Cities** — 3-letter abbreviation, larger/primary nodes.
  - **Dungeons** — 2-letter abbreviation, secondary nodes (optional side content, not required for critical path).
  - **Routes** — numbered (1, 2, 3...); if a route needs more than one "spot" along its length (e.g., it passes a dungeon, or several other routes branch off it), suffix with a letter (`9a`, `9b`, `9c`...).
- **Connectivity rule:** two nodes are connected if and only if they are orthogonally adjacent on the grid (up/down/left/right — never diagonal). This can be computed automatically from the grid rather than hand-drawn, which avoids human transcription errors at scale.
- **Explicit exclusions:** allow specific adjacent pairs to be *not* connected, when the fiction calls for it (e.g., a cliff or river blocking an otherwise-adjacent path). Keep a small exception list and apply it after the automatic adjacency pass.
- **Co-located nodes:** when a dungeon sits inside/under a town (a mine under the capital, a sewer under a city), don't give it a separate grid cell — mark it as co-located with the town's node instead. Merge these visually into a single combined marker (see §14) rather than two overlapping markers.
- **Verify full connectivity:** after building the graph, confirm every node is reachable from the starting town, and ideally that important clusters have more than one path in — this is what makes "gyms in any order" actually work without soft-locking the player.

---

## 2. Regional Dex Construction

- **Target size:** ~200 Pokémon, give or take 5–10 (190–210 is a comfortable range). This roughly matches the scale of most mainline regional dexes.
- **Starters:** 3 lines (Grass/Fire/Water is traditional), each fully evolved (3 stages each = 9 Pokémon), thematically fitted to the region's identity. Starters are **never** placed in wild encounter tables — they're Professor-gift-only.
- **Legendaries/Mythicals:** a "normal" region has roughly 5–8 Legendary/Mythical Pokémon. Tie each one thematically to a specific location (ideally a postgame-gated dungeon) rather than leaving them generic.
- **Completeness rule:** if a Pokémon is in the dex, **every one of its non-regional-variant evolutions must also be in the dex** (both directions — don't include a mid-evolution without its later stage, and don't include a final stage without checking whether its earlier stages should also be represented). Branch evolutions (e.g., a Pokémon with two alternate final forms) count as part of the same family, not extra dex slots.
- **Habitat organization:** group the dex by which town/route/dungeon each Pokémon is thematically associated with — makes both design and later cross-referencing much easier.

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
- **Ordinary (non-Legendary) one-stage "oddities"** — Pokémon that just don't evolve, no lore reason needed — should still be roughly ~10% of the dex. If cuts have pushed that number down to near-zero, add a few back; these "oddity" Pokémon (a Tauros, a Lapras, a Farfetch'd-type pick) are part of what makes a dex feel textured rather than mechanically pure.

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

---

## 6. Wild Encounter Tables

- One table per route/dungeon, covering sums 2–12 (11 slots).
- **Low slots = common/base-stage. High slots = rare/evolved.** But this needs to be checked *across the whole table*, not just within one species line — an evolved Pokémon from Line A should never occupy a lower (more common) slot than an unrelated base-stage Pokémon from Line B in that same table. (This is an easy bug to introduce when hand-editing tables piecemeal — audit for it programmatically if possible.)
- **Repeats within a table are fine and often desirable** for genuinely common species, and it's fine for some low-diversity locations (a narrow-habitat dungeon) to repeat more than a rich, varied route. Not every table needs full variety — but if you *do* want variety, vary deliberately (e.g., make sure slot 2 and slot 4 aren't reflexively identical everywhere).
- **Reserve the rarest slot (sum 12) for the location's signature rare find** — a Legendary/Mythical if the location has one, otherwise the location's best "trophy" catch. Since sum 12 is only reachable at max badge count, this is naturally a postgame-flavored reward with zero extra flagging required.
- **Sanity-check reachability:** nothing should be *permanently* locked behind an impossible combination (e.g., don't put a Pokémon whose evolution requires a level higher than the location's level cap ever allows at a slot that implies it should be evolved there).

---

## 7. Route Trainers (Generic Trainer Classes)

- Design ~10–12 recurring trainer archetypes that could plausibly appear on **any** route in the region, regardless of terrain.
  - **Reject classes that require specific terrain to make sense as a concept** — a Swimmer or Fisherman literally cannot exist on a landlocked desert route; a Hiker, Camper, Cyclist, or Backpacker can exist anywhere. This is a stricter bar than "does this Pokémon type fit here" — it's about whether the *trainer's identity* is terrain-independent.
- **Map each class to one number on the 2–12 wheel** (plain 2d6 roll, no reroll-above-threshold — trainer *type* doesn't need to scale with badges the way encounter rarity does). Assign the numbers so that **the most generic/everyday class sits at 7** (the most probable roll) and the most unusual/elite class(es) sit at the 2/12 extremes — matching flavor to actual frequency, not the reverse.
- **Give each class its own 11-slot table of Pokémon lines** (one per sum 2–12), with **no repeated line within a single class's own table** (reuse across *different* classes is fine and realistic). Order each class's own table the same way: the class's single most "typical" pick at 7, its most surprising/rare pick at the 2/12 extremes.
- **Team size scales with badge tier** (same tiering shape as the core threshold table: 1 Pokémon at 0 badges, 2 at 1–2, 3 at 3–4, 4 at 5–6, 5 at 7–8).
- **Level is a flat formula, not tiered:** typically `base + exact badge count` (not bucketed the way team size is) — so level increases every single badge, even within a team-size tier.
- **Level-appropriate evolution:** once a line is rolled, resolve it to whichever stage the trainer's actual level supports (see §11) — don't just hand the roll a random stage.

---

## 8. Gym Leaders

- **8 gyms is the traditional count** — spread them across **geographically distinct towns**, not clustered near each other, so "any order" play actually produces varied early routes depending on player choice.
- **Type coverage:** give each gym a distinct type. It's fine — good, even — if this requires reshuffling which towns host gyms partway through design; town identity (its dex habitat) doesn't have to dictate gym type if the fiction can flex (e.g., a leader can be described as importing Pokémon from elsewhere in the region).
- **5-slot team structure, with fixed send-order and per-slot rules:**
  - Slots **1 and 2** are always present (these are the leader's "core" — slot 1 is conventionally the Ace).
  - Slot **3** unlocks once the player has **2+ badges already claimed** (before this fight).
  - Slot **4** unlocks at **4+ badges claimed**.
  - Slot **5** unlocks at **6+ badges claimed**.
  - **Send-out order is 5, 2, 3, 4, 1** — the newest/weakest-feeling addition (slot 5) goes out first, the Ace (slot 1) is saved for last.
  - **Levels:** slots 1 and 4 = `badges + 5`; slots 2 and 3 = `badges + 4`; slot 5 = `badges + 3`. (Levels use the *exact* badge count, not a tier — same principle as trainer levels.)
- **Attack type is a fixed rule per slot, not a random roll**, once you want gym battles to feel authored rather than randomized:
  - Slots 1, 2, and 5 → the Pokémon's unmodified **primary type**.
  - Slot 3 → **one step clockwise** on the 18-type wheel from primary (see §13).
  - Slot 4 → **one step counterclockwise** from primary.
- Assign each leader **one signature evolution line per slot** (5 lines total), not a randomized species pool — gym leaders should feel authored and consistent every time you fight them, unlike route trainers.

---

## 9. Elite Four & Champion

- **4 Elite Four members + 1 Champion**, based at the region's final city.
- **Cover the types the 8 gyms don't.** If you have leftover "retired" concepts (e.g., you originally planned a gym for a type that got reassigned), consider promoting them to Elite Four members instead of inventing new characters from scratch — keeps continuity.
- **Elite Four members: 5-slot roster** (numbered slots 1–4, plus a separate Ace) — same fixed-level, fixed-type-per-slot logic as gyms (slot 3 = clockwise shift, slot 4 = counterclockwise, everything else = primary).
- **Champion: 6-slot roster** (slots 1–5, plus Ace) — same rules extended by one slot.
- **Fixed levels, no badge-gating** — once the player has beaten all 8 gyms, the full Elite Four/Champion roster is always available exactly as designed, unlike gym teams which scale with progress.
- **Sanity-check the level ceiling.** If your level formula caps out at some maximum (e.g., "3 + badges" caps at 11 once badges max at 8), don't assign an Elite Four/Champion Pokémon a final evolution that requires a level *above* that ceiling — it'll never actually appear. Pick a line whose evolution requirement fits within reach, even if that means passing on a flashier pseudo-legendary.
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
- **The fix — a buffer rule:** a non-level evolution's proper level = its pre-evolution's proper level **+ a fixed buffer**. A buffer that scales (e.g., +6 if the pre-evolution is level 1, +3 for anything higher) reads more naturally than one flat number, since a Pokémon evolving from its very first stage should get more runway than one evolving from something already mid-progression.
- **Split evolutions with one level-based sibling are the exception to the buffer.** When a pre-evolution branches into two (or more) evolutions and *at least one* branch is a normal level-up evolution, give every non-level sibling in that same split the **level-based sibling's own level** instead of computing a buffer — don't make Fairy-Politoed-Bellossom-style siblings appear at a different level than the branch that got there by leveling normally, for no in-fiction reason. Only fall back to the scaling buffer when *no* branch in the split is level-based (Eevee's five stone/friendship evolutions, for instance, have no level-up sibling to borrow from, so all five use the buffer).
- **Chain these calculations in dependency order** when a family has back-to-back non-level steps (e.g., base → non-level stage 2 → non-level stage 3) — the second buffer must be computed from the *already-buffered* first result, not from the original mainline data, or you'll under-count.
- **Keep this buffered ruleset separate from your "true" wild-encounter evolution data** if the two systems have different needs — wild encounters may only need a simple downgrade-if-too-low check (which never breaks even with zero buffer), while a trainer/gym system that always shows "the best reachable stage" absolutely needs the buffer. Don't let a fix for one system silently corrupt the other.

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

## 14. If You Build a Digital Companion Tool

Lessons that generalize beyond Calli specifically:

- **Separate "true" data from "derived/rolled" data** in your data model from the start (e.g., a Pokémon's real typing vs. its rolled attack type) — trying to bolt this distinction on later is painful.
- **When two systems need different versions of the same underlying rule** (e.g., wild-encounter evolution thresholds vs. trainer-team evolution thresholds), keep them as genuinely separate datasets rather than one shared table with exceptions — shared tables silently break the first time one system's needs diverge from the other's.
- **Case sensitivity and lookup-table mismatches are a top source of silent bugs** in this kind of project — if one part of your data stores `"Water"` and a lookup table expects `"water"`, the failure won't throw an error, it'll just silently return the wrong (but plausible-looking) result. Normalize casing at the boundary, every time.
- **CSS transforms and SVG presentation-attribute transforms don't combine — the CSS one wins and replaces the other.** If a shape needs a static rotation *and* a hover/selection effect, apply them to different layers (or use non-transform properties like stroke-width/shadow for the interactive state) rather than fighting over the same `transform` property.
- **Test the actual shipped logic, not a hand-simplified re-implementation of it** — running the real functions end-to-end (with a proper DOM, if it's a web tool) catches bugs that re-deriving the math on paper won't, especially around scope, timing, and data-format mismatches.
- **When a probability curve is central to the design** (like the 2d6 threshold system here), audit your content placement against the *actual* math, not intuition — it's very easy to place "common-feeling" content at low numbers out of habit, even when low numbers are statistically rare.

---

## Quick-Reference Checklist for a New Region

- [ ] Map: grid-based, orthogonal adjacency, verified fully connected, multiple entry paths into major clusters
- [ ] Regional dex: ~200 Pokémon, 3 starters, 5–8 Legendaries/Mythicals, full evolution-line completeness
- [ ] Type balance audited against National Dex norms after every major edit
- [ ] Evolution-stage distribution checked (two-stage majority, ~30% three-stage, ~10% ordinary one-stage)
- [ ] Core 2d6 + reroll-above-threshold mechanic defined, with badge-based threshold table
- [ ] Wild encounter tables per route/dungeon, rarity-consistent, sum-12 reserved for signature rare finds
- [ ] 10–12 terrain-neutral generic trainer classes, mapped to the wheel by actual frequency
- [ ] 8 gym leaders, geographically spread, type-covered, 5-slot fixed-order roster rule
- [ ] Elite Four + Champion covering leftover types, fixed roster/levels, level-ceiling sanity-checked
- [ ] 1–20 (or other compressed) level scale with an explicit bucket-conversion table
- [ ] Non-level evolutions given a buffered "proper level," computed in dependency order
- [ ] Stat-to-die conversion formula fixed, level-based bonus distribution rule defined
- [ ] (Optional) attack-type wheel mechanic for combat flavor
- [ ] (Optional) digital tool: true-vs-derived data kept separate, casing normalized, real end-to-end testing before shipping changes
