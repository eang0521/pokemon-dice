# National Evolution Data Reference

Every evolution relationship among the base forms listed in [Pokemon_Base_Stats.md](Pokemon_Base_Stats.md). **Level-up evolutions** are converted from their mainline level to the **1–20 compressed scale** (ruleset §10) — original mainline levels are not shown anywhere in this doc, only the converted result. **Non-level evolutions** (item, friendship, trade, attack-type, or another unique trigger) have no inherent level, so their method is shown on its own with no level attached — a computed "proper level" (ruleset §11's buffer rule) still exists under the hood to keep split-evolution siblings consistent with each other, it's just not printed here.

**Trade-with-item evolutions convert to that item** (e.g. trade holding Dragon Scale → **Dragon Scale**). **Pure trade evolutions convert to Linking Cord.** Species not listed here don't evolve at all (and aren't evolved from anything) — cross-check [Pokemon_Base_Stats.md](Pokemon_Base_Stats.md) for the full species list.

**Day/night and gender conditions are not modeled and have been removed entirely.** Where that qualifier used to be the only thing distinguishing two different evolution targets from the same species, and both targets are still kept in this doc (Eevee → Espeon/Umbreon, Burmy → Mothim/Wormadam), both are now reachable from the same condition and marked **(random)** — a 50/50 coin flip, same convention as Wurmple → Silcoon/Cascoon. Where it was just a flavor qualifier on a single target (no second target to flip against, or the other target was later simplified away entirely — Rockruff → Lycanroc, Espurr → Meowstic, Lechonk → Oinkologne), it's simply dropped.

**Move-learned evolution triggers** (Ancient Power, Rollout, etc.) convert to **"[Type] attack type"** — leveling up while that's the Pokémon's current attack type, using the move's real in-game type — same convention as Piloswine → Mamoswine. Two brand-new counters cover the remaining odd cases: **Battles Won** (Primeape → Annihilape, Bisharp → Kingambit — 10 all-time battle wins) and **Steps** (Pawmo → Pawmot, Bramblin → Brambleghast, Rellor → Rabsca, Gimmighoul → Gholdengo — 25 spaces moved on the map).

| Evolution | Method |
|---|---|
| Bulbasaur → Ivysaur | Level 5 |
| Ivysaur → Venusaur | Level 9 |
| Charmander → Charmeleon | Level 5 |
| Charmeleon → Charizard | Level 10 |
| Squirtle → Wartortle | Level 5 |
| Wartortle → Blastoise | Level 10 |
| Caterpie → Metapod | Level 3 |
| Metapod → Butterfree | Level 4 |
| Weedle → Kakuna | Level 3 |
| Kakuna → Beedrill | Level 4 |
| Pidgey → Pidgeotto | Level 6 |
| Pidgeotto → Pidgeot | Level 10 |
| Rattata → Raticate | Level 6 |
| Spearow → Fearow | Level 6 |
| Ekans → Arbok | Level 7 |
| Pikachu → Raichu | Thunder Stone |
| Sandshrew → Sandslash | Level 7 |
| Nidoran♀ → Nidorina | Level 5 |
| Nidorina → Nidoqueen | Moon Stone |
| Nidoran♂ → Nidorino | Level 5 |
| Nidorino → Nidoking | Moon Stone |
| Clefairy → Clefable | Moon Stone |
| Vulpix → Ninetales | Fire Stone |
| Jigglypuff → Wigglytuff | Moon Stone |
| Zubat → Golbat | Level 7 |
| Golbat → Crobat | Friendship |
| Oddish → Gloom | Level 6 |
| Gloom → Vileplume | Leaf Stone |
| Gloom → Bellossom | Sun Stone |
| Paras → Parasect | Level 7 |
| Venonat → Venomoth | Level 9 |
| Diglett → Dugtrio | Level 8 |
| Meowth → Persian | Level 8 |
| Psyduck → Golduck | Level 9 |
| Mankey → Primeape | Level 8 |
| Primeape → Annihilape | 10 Battles Won |
| Growlithe → Arcanine | Fire Stone |
| Poliwag → Poliwhirl | Level 7 |
| Poliwhirl → Poliwrath | Water Stone |
| Poliwhirl → Politoed | King's Rock |
| Abra → Kadabra | Level 5 |
| Kadabra → Alakazam | Linking Cord |
| Machop → Machoke | Level 8 |
| Machoke → Machamp | Linking Cord |
| Bellsprout → Weepinbell | Level 6 |
| Weepinbell → Victreebel | Leaf Stone |
| Tentacool → Tentacruel | Level 8 |
| Geodude → Graveler | Level 7 |
| Graveler → Golem | Linking Cord |
| Ponyta → Rapidash | Level 10 |
| Slowpoke → Slowbro | Level 10 |
| Slowpoke → Slowking | King's Rock |
| Magnemite → Magneton | Level 8 |
| Magneton → Magnezone | Thunder Stone |
| Doduo → Dodrio | Level 9 |
| Seel → Dewgong | Level 9 |
| Grimer → Muk | Level 10 |
| Shellder → Cloyster | Water Stone |
| Gastly → Haunter | Level 7 |
| Haunter → Gengar | Linking Cord |
| Onix → Steelix | Metal Coat |
| Drowzee → Hypno | Level 8 |
| Krabby → Kingler | Level 8 |
| Voltorb → Electrode | Level 8 |
| Exeggcute → Exeggutor | Leaf Stone |
| Cubone → Marowak | Level 8 |
| Lickitung → Lickilicky | Rock attack type |
| Koffing → Weezing | Level 9 |
| Rhyhorn → Rhydon | Level 11 |
| Rhydon → Rhyperior | Protector |
| Chansey → Blissey | Friendship |
| Tangela → Tangrowth | Rock attack type |
| Horsea → Seadra | Level 9 |
| Seadra → Kingdra | Dragon Scale |
| Goldeen → Seaking | Level 9 |
| Staryu → Starmie | Water Stone |
| Scyther → Scizor | Metal Coat |
| Electabuzz → Electivire | Electirizer |
| Magmar → Magmortar | Magmarizer |
| Magikarp → Gyarados | Level 6 |
| Eevee → Vaporeon | Water Stone |
| Eevee → Jolteon | Thunder Stone |
| Eevee → Flareon | Fire Stone |
| Eevee → Espeon | Friendship (random) |
| Eevee → Umbreon | Friendship (random) |
| Eevee → Leafeon | Leaf Stone |
| Eevee → Glaceon | Ice Stone |
| Eevee → Sylveon | Friendship (and know a Fairy-type move) |
| Porygon → Porygon2 | Upgrade |
| Porygon2 → Porygon-Z | Dubious Disc |
| Omanyte → Omastar | Level 10 |
| Kabuto → Kabutops | Level 10 |
| Dratini → Dragonair | Level 8 |
| Dragonair → Dragonite | Level 13 |
| Chikorita → Bayleef | Level 5 |
| Bayleef → Meganium | Level 9 |
| Cyndaquil → Quilava | Level 5 |
| Quilava → Typhlosion | Level 10 |
| Totodile → Croconaw | Level 6 |
| Croconaw → Feraligatr | Level 8 |
| Sentret → Furret | Level 5 |
| Hoothoot → Noctowl | Level 6 |
| Ledyba → Ledian | Level 6 |
| Spinarak → Ariados | Level 7 |
| Chinchou → Lanturn | Level 8 |
| Togepi → Togetic | Friendship |
| Togetic → Togekiss | Shiny Stone |
| Natu → Xatu | Level 7 |
| Mareep → Flaaffy | Level 5 |
| Flaaffy → Ampharos | Level 8 |
| Marill → Azumarill | Level 6 |
| Hoppip → Skiploom | Level 6 |
| Skiploom → Jumpluff | Level 8 |
| Aipom → Ambipom | Normal attack type |
| Sunkern → Sunflora | Sun Stone |
| Yanma → Yanmega | Rock attack type |
| Wooper → Quagsire | Level 6 |
| Murkrow → Honchkrow | Dusk Stone |
| Misdreavus → Mismagius | Dusk Stone |
| Girafarig → Farigiraf | Psychic attack type |
| Pineco → Forretress | Level 9 |
| Dunsparce → Dudunsparce | Normal attack type |
| Gligar → Gliscor | Razor Fang |
| Snubbull → Granbull | Level 7 |
| Sneasel → Weavile | Razor Claw |
| Teddiursa → Ursaring | Level 8 |
| Slugma → Magcargo | Level 10 |
| Swinub → Piloswine | Level 9 |
| Piloswine → Mamoswine | Rock attack type |
| Remoraid → Octillery | Level 7 |
| Houndour → Houndoom | Level 7 |
| Phanpy → Donphan | Level 7 |
| Larvitar → Pupitar | Level 8 |
| Pupitar → Tyranitar | Level 13 |
| Treecko → Grovyle | Level 5 |
| Grovyle → Sceptile | Level 10 |
| Torchic → Combusken | Level 5 |
| Combusken → Blaziken | Level 10 |
| Mudkip → Marshtomp | Level 5 |
| Marshtomp → Swampert | Level 10 |
| Poochyena → Mightyena | Level 6 |
| Zigzagoon → Linoone | Level 6 |
| Wurmple → Silcoon | Level 3 (random) |
| Silcoon → Beautifly | Level 4 |
| Wurmple → Cascoon | Level 3 (random) |
| Cascoon → Dustox | Level 4 |
| Lotad → Lombre | Level 5 |
| Lombre → Ludicolo | Water Stone |
| Seedot → Nuzleaf | Level 5 |
| Nuzleaf → Shiftry | Leaf Stone |
| Taillow → Swellow | Level 7 |
| Wingull → Pelipper | Level 7 |
| Ralts → Kirlia | Level 6 |
| Kirlia → Gardevoir | Level 8 |
| Kirlia → Gallade | Dawn Stone |
| Surskit → Masquerain | Level 7 |
| Shroomish → Breloom | Level 7 |
| Slakoth → Vigoroth | Level 6 |
| Vigoroth → Slaking | Level 10 |
| Nincada → Ninjask | Level 6 |
| Nincada → Shedinja | Automatic (appears alongside Ninjask, same Level/EVs) |
| Whismur → Loudred | Level 6 |
| Loudred → Exploud | Level 10 |
| Makuhita → Hariyama | Level 7 |
| Nosepass → Probopass | Thunder Stone |
| Skitty → Delcatty | Moon Stone |
| Aron → Lairon | Level 9 |
| Lairon → Aggron | Level 11 |
| Meditite → Medicham | Level 10 |
| Electrike → Manectric | Level 8 |
| Roselia → Roserade | Shiny Stone |
| Gulpin → Swalot | Level 8 |
| Carvanha → Sharpedo | Level 8 |
| Wailmer → Wailord | Level 10 |
| Numel → Camerupt | Level 9 |
| Spoink → Grumpig | Level 9 |
| Trapinch → Vibrava | Level 9 |
| Vibrava → Flygon | Level 11 |
| Cacnea → Cacturne | Level 9 |
| Swablu → Altaria | Level 9 |
| Barboach → Whiscash | Level 8 |
| Corphish → Crawdaunt | Level 8 |
| Baltoy → Claydol | Level 10 |
| Lileep → Cradily | Level 10 |
| Anorith → Armaldo | Level 10 |
| Feebas → Milotic | Prism Scale |
| Shuppet → Banette | Level 10 |
| Duskull → Dusclops | Level 10 |
| Dusclops → Dusknoir | Reaper Cloth |
| Snorunt → Glalie | Level 11 |
| Snorunt → Froslass | Dawn Stone |
| Spheal → Sealeo | Level 9 |
| Sealeo → Walrein | Level 11 |
| Clamperl → Huntail | Deep Sea Tooth |
| Clamperl → Gorebyss | Deep Sea Scale |
| Bagon → Shelgon | Level 8 |
| Shelgon → Salamence | Level 12 |
| Beldum → Metang | Level 6 |
| Metang → Metagross | Level 11 |
| Turtwig → Grotle | Level 6 |
| Grotle → Torterra | Level 9 |
| Chimchar → Monferno | Level 5 |
| Monferno → Infernape | Level 10 |
| Piplup → Prinplup | Level 5 |
| Prinplup → Empoleon | Level 10 |
| Starly → Staravia | Level 5 |
| Staravia → Staraptor | Level 9 |
| Bidoof → Bibarel | Level 5 |
| Kricketot → Kricketune | Level 4 |
| Shinx → Luxio | Level 5 |
| Luxio → Luxray | Level 8 |
| Cranidos → Rampardos | Level 8 |
| Shieldon → Bastiodon | Level 8 |
| Burmy → Wormadam | Level 6 (random) |
| Burmy → Mothim | Level 6 (random) |
| Combee → Vespiquen | Level 6 |
| Buizel → Floatzel | Level 8 |
| Cherubi → Cherrim | Level 7 |
| Shellos → Gastrodon | Level 8 |
| Drifloon → Drifblim | Level 8 |
| Buneary → Lopunny | Friendship |
| Glameow → Purugly | Level 10 |
| Stunky → Skuntank | Level 9 |
| Bronzor → Bronzong | Level 9 |
| Gible → Gabite | Level 7 |
| Gabite → Garchomp | Level 12 |
| Riolu → Lucario | Friendship |
| Hippopotas → Hippowdon | Level 9 |
| Skorupi → Drapion | Level 10 |
| Croagunk → Toxicroak | Level 10 |
| Finneon → Lumineon | Level 9 |
| Snover → Abomasnow | Level 10 |
| Snivy → Servine | Level 5 |
| Servine → Serperior | Level 10 |
| Tepig → Pignite | Level 5 |
| Pignite → Emboar | Level 10 |
| Oshawott → Dewott | Level 5 |
| Dewott → Samurott | Level 10 |
| Patrat → Watchog | Level 6 |
| Lillipup → Herdier | Level 5 |
| Herdier → Stoutland | Level 9 |
| Purrloin → Liepard | Level 6 |
| Pansage → Simisage | Leaf Stone |
| Pansear → Simisear | Fire Stone |
| Panpour → Simipour | Water Stone |
| Munna → Musharna | Moon Stone |
| Pidove → Tranquill | Level 6 |
| Tranquill → Unfezant | Level 9 |
| Blitzle → Zebstrika | Level 8 |
| Roggenrola → Boldore | Level 7 |
| Boldore → Gigalith | Linking Cord |
| Woobat → Swoobat | Friendship |
| Drilbur → Excadrill | Level 9 |
| Timburr → Gurdurr | Level 7 |
| Gurdurr → Conkeldurr | Linking Cord |
| Tympole → Palpitoad | Level 7 |
| Palpitoad → Seismitoad | Level 10 |
| Sewaddle → Swadloon | Level 6 |
| Swadloon → Leavanny | Friendship |
| Venipede → Whirlipede | Level 7 |
| Whirlipede → Scolipede | Level 8 |
| Cottonee → Whimsicott | Sun Stone |
| Petilil → Lilligant | Sun Stone |
| Sandile → Krokorok | Level 8 |
| Krokorok → Krookodile | Level 10 |
| Darumaka → Darmanitan | Level 9 |
| Dwebble → Crustle | Level 9 |
| Scraggy → Scrafty | Level 10 |
| Yamask → Cofagrigus | Level 9 |
| Tirtouga → Carracosta | Level 10 |
| Archen → Archeops | Level 10 |
| Trubbish → Garbodor | Level 10 |
| Zorua → Zoroark | Level 8 |
| Minccino → Cinccino | Shiny Stone |
| Gothita → Gothorita | Level 9 |
| Gothorita → Gothitelle | Level 11 |
| Solosis → Duosion | Level 9 |
| Duosion → Reuniclus | Level 11 |
| Ducklett → Swanna | Level 9 |
| Vanillite → Vanillish | Level 9 |
| Vanillish → Vanilluxe | Level 12 |
| Deerling → Sawsbuck | Level 9 |
| Karrablast → Escavalier | Linking Cord |
| Foongus → Amoonguss | Level 10 |
| Frillish → Jellicent | Level 10 |
| Joltik → Galvantula | Level 10 |
| Ferroseed → Ferrothorn | Level 10 |
| Klink → Klang | Level 10 |
| Klang → Klinklang | Level 12 |
| Tynamo → Eelektrik | Level 10 |
| Eelektrik → Eelektross | Thunder Stone |
| Elgyem → Beheeyem | Level 11 |
| Litwick → Lampent | Level 11 |
| Lampent → Chandelure | Dusk Stone |
| Axew → Fraxure | Level 10 |
| Fraxure → Haxorus | Level 12 |
| Cubchoo → Beartic | Level 10 |
| Shelmet → Accelgor | Linking Cord |
| Mienfoo → Mienshao | Level 12 |
| Golett → Golurk | Level 11 |
| Pawniard → Bisharp | Level 13 |
| Bisharp → Kingambit | 10 Battles Won |
| Rufflet → Braviary | Level 13 |
| Vullaby → Mandibuzz | Level 13 |
| Deino → Zweilous | Level 12 |
| Zweilous → Hydreigon | Level 15 |
| Larvesta → Volcarona | Level 14 |
| Chespin → Quilladin | Level 5 |
| Quilladin → Chesnaught | Level 10 |
| Fennekin → Braixen | Level 5 |
| Braixen → Delphox | Level 10 |
| Froakie → Frogadier | Level 5 |
| Frogadier → Greninja | Level 10 |
| Bunnelby → Diggersby | Level 6 |
| Fletchling → Fletchinder | Level 5 |
| Fletchinder → Talonflame | Level 9 |
| Scatterbug → Spewpa | Level 3 |
| Spewpa → Vivillon | Level 4 |
| Litleo → Pyroar | Level 9 |
| Flabébé → Floette | Level 6 |
| Floette → Florges | Shiny Stone |
| Skiddo → Gogoat | Level 9 |
| Pancham → Pangoro | Level 9 (own a Dark-type Pokémon) |
| Espurr → Meowstic | Level 7 |
| Honedge → Doublade | Level 9 |
| Doublade → Aegislash | Dusk Stone |
| Spritzee → Aromatisse | Sachet |
| Swirlix → Slurpuff | Whipped Dream |
| Inkay → Malamar | Level 8 |
| Binacle → Barbaracle | Level 10 |
| Skrelp → Dragalge | Level 12 |
| Clauncher → Clawitzer | Level 10 |
| Helioptile → Heliolisk | Sun Stone |
| Tyrunt → Tyrantrum | Level 10 |
| Amaura → Aurorus | Level 10 |
| Goomy → Sliggoo | Level 10 |
| Sliggoo → Goodra | Level 12 |
| Phantump → Trevenant | Linking Cord |
| Pumpkaboo → Gourgeist | Linking Cord |
| Bergmite → Avalugg | Level 10 |
| Noibat → Noivern | Level 12 |
| Rowlet → Dartrix | Level 5 |
| Dartrix → Decidueye | Level 9 |
| Litten → Torracat | Level 5 |
| Torracat → Incineroar | Level 9 |
| Popplio → Brionne | Level 5 |
| Brionne → Primarina | Level 9 |
| Pikipek → Trumbeak | Level 5 |
| Trumbeak → Toucannon | Level 8 |
| Yungoos → Gumshoos | Level 6 |
| Grubbin → Charjabug | Level 6 |
| Charjabug → Vikavolt | Thunder Stone |
| Crabrawler → Crabominable | Ice Stone |
| Cutiefly → Ribombee | Level 7 |
| Rockruff → Lycanroc | Level 7 |
| Mareanie → Toxapex | Level 10 |
| Mudbray → Mudsdale | Level 8 |
| Dewpider → Araquanid | Level 7 |
| Fomantis → Lurantis | Level 9 |
| Morelull → Shiinotic | Level 7 |
| Salandit → Salazzle | Level 9 |
| Stufful → Bewear | Level 8 |
| Bounsweet → Steenee | Level 6 |
| Steenee → Tsareena | Normal attack type |
| Wimpod → Golisopod | Level 8 |
| Sandygast → Palossand | Level 11 |
| Type: Null → Silvally | Friendship |
| Jangmo-o → Hakamo-o | Level 9 |
| Hakamo-o → Kommo-o | Level 11 |
| Cosmog → Cosmoem | Level 11 |
| Cosmoem → Solgaleo | Level 13 (random) |
| Cosmoem → Lunala | Level 13 (random) |
| Poipole → Naganadel | Dragon attack type |
| Meltan → Melmetal | Friendship |
| Grookey → Thwackey | Level 5 |
| Thwackey → Rillaboom | Level 9 |
| Scorbunny → Raboot | Level 5 |
| Raboot → Cinderace | Level 9 |
| Sobble → Drizzile | Level 5 |
| Drizzile → Inteleon | Level 9 |
| Skwovet → Greedent | Level 7 |
| Rookidee → Corvisquire | Level 6 |
| Corvisquire → Corviknight | Level 10 |
| Blipbug → Dottler | Level 4 |
| Dottler → Orbeetle | Level 8 |
| Nickit → Thievul | Level 6 |
| Gossifleur → Eldegoss | Level 6 |
| Wooloo → Dubwool | Level 7 |
| Chewtle → Drednaw | Level 7 |
| Yamper → Boltund | Level 7 |
| Rolycoly → Carkol | Level 6 |
| Carkol → Coalossal | Level 9 |
| Applin → Flapple | Tart Apple |
| Applin → Appletun | Sweet Apple |
| Applin → Dipplin | Syrupy Apple |
| Dipplin → Hydrapple | Dragon attack type |
| Silicobra → Sandaconda | Level 10 |
| Arrokuda → Barraskewda | Level 8 |
| Toxel → Toxtricity | Level 8 |
| Sizzlipede → Centiskorch | Level 8 |
| Clobbopus → Grapploct | Dark attack type |
| Sinistea → Polteageist | Cracked Pot |
| Hatenna → Hattrem | Level 9 |
| Hattrem → Hatterene | Level 11 |
| Impidimp → Morgrem | Level 9 |
| Morgrem → Grimmsnarl | Level 11 |
| Milcery → Alcremie | Sweet |
| Snom → Frosmoth | Friendship |
| Cufant → Copperajah | Level 9 |
| Duraludon → Archaludon | Metal Alloy |
| Dreepy → Drakloak | Level 12 |
| Drakloak → Dragapult | Level 14 |
| Kubfu → Urshifu | Scroll of Darkness |
| Sprigatito → Floragato | Level 5 |
| Floragato → Meowscarada | Level 10 |
| Fuecoco → Crocalor | Level 5 |
| Crocalor → Skeledirge | Level 10 |
| Quaxly → Quaxwell | Level 5 |
| Quaxwell → Quaquaval | Level 10 |
| Lechonk → Oinkologne | Level 6 |
| Tarountula → Spidops | Level 5 |
| Nymble → Lokix | Level 7 |
| Pawmi → Pawmo | Level 6 |
| Pawmo → Pawmot | 25 Steps |
| Tandemaus → Maushold | Level 7 |
| Fidough → Dachsbun | Level 8 |
| Smoliv → Dolliv | Level 7 |
| Dolliv → Arboliva | Level 9 |
| Nacli → Naclstack | Level 7 |
| Naclstack → Garganacl | Level 10 |
| Charcadet → Armarouge | Auspicious Armor |
| Charcadet → Ceruledge | Malicious Armor |
| Tadbulb → Bellibolt | Thunder Stone |
| Wattrel → Kilowattrel | Level 7 |
| Maschiff → Mabosstiff | Level 8 |
| Shroodle → Grafaiai | Level 8 |
| Bramblin → Brambleghast | 25 Steps |
| Toedscool → Toedscruel | Level 8 |
| Capsakid → Scovillain | Fire Stone |
| Rellor → Rabsca | 25 Steps |
| Flittle → Espathra | Level 9 |
| Tinkatink → Tinkatuff | Level 7 |
| Tinkatuff → Tinkaton | Level 10 |
| Wiglett → Wugtrio | Level 8 |
| Finizen → Palafin | Level 10 |
| Varoom → Revavroom | Level 10 |
| Glimmet → Glimmora | Level 9 |
| Greavard → Houndstone | Level 8 |
| Cetoddle → Cetitan | Ice Stone |
| Frigibax → Arctibax | Level 9 |
| Arctibax → Baxcalibur | Level 13 |
| Gimmighoul → Gholdengo | 25 Steps |
| Poltchageist → Sinistcha | Unremarkable Teacup |
