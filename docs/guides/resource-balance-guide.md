# Vanilla Cosmic — Resource Balance Guide (Mindustry Build 160.7 target)

This guide groups vanilla resources by **practical progression rarity** for Vanilla Cosmic balance decisions. Mindustry does not assign official rarity labels to items or liquids: map generation, planet, campaign sector, production chain, and hazards determine availability. Use these groups to choose sensible construction and research ingredients; do not treat them as hard engine rules.

## Item rarity groups

| Group | Vanilla items | Typical role in a block recipe |
|---|---|---|
| Common base resources | `copper`, `lead`, `sand`, `scrap`, `coal` | Early construction; copper and lead are the normal recipe backbone. Sand, scrap, and coal should be used only when their production or environmental theme matters. |
| Refined basic resources | `graphite`, `silicon`, `metaglass` | Midgame processing materials. Use in modest counts alongside copper and lead. |
| Mid-progression mined ores | `titanium`, `thorium`, `beryllium` | Structural upgrades that mark a stronger block or a planet-specific recipe. |
| Advanced mined ore | `tungsten` | Late Erekir structural material; reserve for powerful or late-game content. |
| Advanced refined materials | `plastanium`, `phase-fabric`, `oxide` | High-tech components. Add only after the block's core materials already justify its tier. |
| Rare alloys and catalysts | `surge-alloy`, `carbide` | Endgame ingredients for exceptional stats, energy handling, or durability. Keep counts lower than core materials. |
| Specialized crafted materials | `spore-pod`, `pyratite`, `blast-compound` | Use when a block specifically relates to biomass, combustion, ammunition, or explosives—not merely to make a recipe more expensive. |
| Internal / do not use in ordinary recipes | `fissile-matter`, `dormant-cyst` | Hidden vanilla items. Do not use these in normal requirements, research, or fixed production outputs. |

## Item recipe rules

- Start a construction recipe with copper and lead unless the block is deliberately planet-specific.
- Add graphite, silicon, or metaglass for processing complexity.
- Add a mined progression ore when the block's tier rises; do not introduce several advanced ores merely to inflate cost.
- Treat plastanium, phase fabric, oxide, surge alloy, and carbide as upgrades rather than bulk material. Their counts should remain below the copper and lead counts in most recipes.
- Keep a block's existing ingredient list when rebalancing it. Use the entry rules in the drill, bore, burst-drill, and wall-crafter guides only when creating a new recipe or introducing a material for the first time.
- Set `researchCost` to 5× every corresponding construction requirement unless a block-specific guide explicitly establishes another rule.

## Cross-planet transmutation on Serpulo

- A Serpulo-facing transmuter may output an existing Erekir item when its HJSON uses that vanilla item as its fixed output. This does not create a duplicate item and does not change the native planet association of the item or unlock Erekir blocks.
- Every input, construction requirement, and research cost for a Serpulo transmuter must remain obtainable through Serpulo progression. Do not use Erekir-exclusive materials such as beryllium, tungsten, oxide, carbide, arkycite, hydrogen, nitrogen, ozone, or cyanogen as costs for the machine that introduces them.
- Use `shownPlanets: [serpulo]` deliberately for a Serpulo-facing transmuter. It controls content presentation/progression visibility; it is not an environment-placement restriction.
- Prefer one meaningful infrastructure tradeoff—advanced item inputs, power, or a liquid line—over a long chain of arbitrary ingredients. Keep a slower, higher-tier output meaningfully more expensive than an earlier cross-planet output.

## Liquid and gas rarity groups

Mindustry stores both liquids and gases in the `Liquid` content type. In HJSON, use their content names exactly as shown below.

| Group | Vanilla liquids / gases | Typical balance use |
|---|---|---|
| Common environmental liquid | `water` | Default coolant, drill booster, or utility liquid. It should be the cheapest and safest liquid requirement. |
| Common industrial liquids | `oil`, `slag` | Oil is combustible; slag is hot and hazardous. Use them for fuel, processing, heat, or risk-themed mechanics rather than generic cooling. |
| Advanced processed coolant | `cryofluid` | Premium cooling. Appropriate for high-tier drills, reactors, and advanced production. |
| Erekir industrial liquid | `arkycite` | Planet-specific fuel/process liquid. Use only where its Erekir availability fits the block. |
| Erekir process gases | `hydrogen`, `nitrogen`, `ozone` | Advanced gas-processing inputs. Hydrogen and ozone are flammable; nitrogen is the safest general-purpose gas in this group. |
| Endgame reactive gas | `cyanogen` | High-risk, high-power gas. Reserve for exceptional late-game content. |
| Hazard / special-case liquid | `neoplasm` | Hostile spreading liquid. Do not use as a routine coolant or ordinary recipe ingredient unless the block is expressly built around its hazard. |
| Internal / do not use in ordinary content | `gallium` | Hidden vanilla liquid. Avoid it in normal requirements, research, and standard blocks. |

## Liquid recipe rules

- Prefer water for early liquid boosting and low-tier cooling.
- Move to cryofluid only when a block's output, power use, or tier merits an advanced coolant.
- Use oil, slag, arkycite, and gases for a specific mechanical or thematic reason; do not add liquids just to raise cost.
- A liquid consumption amount is a rate, not a one-time construction cost. Check whether it is mandatory or a `booster` before comparing two blocks.
- Test liquid-consuming blocks in-game: map availability and piping capacity can make a theoretically balanced rate impractical.

## Reference scope

This list covers the vanilla item and liquid registries available to the mod target. Other mods can register additional resources, but those should be documented in their own compatibility section before Vanilla Cosmic uses them.
