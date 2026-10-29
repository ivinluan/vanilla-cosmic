# Vanilla Cosmic — Cross-Planet Production Balance Guide (Mindustry v160.7)

This guide covers GenericCrafter machines that manufacture an existing item native to the opposite campaign from locally obtainable resources. Stellar Spallation Chamber (`stellar-spallation-chamber`) and Pulsar Nucleoforge (`pulsar-nucleoforge`) provide Erekir items from Serpulo progression; Nebular Alchemy Reactor (`nebular-alchemy-reactor`), Neutron Flux Forge (`neutron-flux-forge`), and Quasar Plasma Crucible (`quasar-plasma-crucible`) provide Serpulo items from Erekir progression. All are available on both planets through `shownPlanets: [serpulo, erekir]`. This controls content applicability; it does not create a second tech-tree node, make an item native to another planet, unlock vanilla buildings, or impose an environment-placement restriction.

## Technical conventions

- Use `type: GenericCrafter`, `category: crafting`, and `outputItem: item/amount` for a fixed vanilla item output. `output` is a different field used by blocks such as WallCrafter. Do not create duplicate custom items.
- `craftTime` is ticks. At full efficiency, `items/minute = output amount x 3600 / craftTime`.
- `consumes.power` and mandatory liquid `amount` are **per tick**. Divide a desired power-per-second value by 60. Multiply a liquid-per-tick value by 60 for liquid/second and by `craftTime` for liquid/craft.
- Set `itemCapacity` and, where needed, `liquidCapacity` as buffers only; neither is a production-rate multiplier.
- Use ingredients, construction requirements, and research costs obtainable on the source planet. Never require the target item that the machine introduces.
- Multiple mandatory liquids/gases must use one `consumes.liquids` array. v160.7 `ConsumeLiquids` accepts, checks, and removes every declared `LiquidStack` each tick. Gases such as ozone, hydrogen, and nitrogen use the same `Liquid` system.
- Apply the Production / GenericCrafter durability role (`RH 1.10`, `RA 0.90`) from the [Block Durability Balance Guide](block-durability-balance-guide.md). Add a row to the durability audit for every new machine.

## Terrain-attribute production bonuses

Four existing Cosmic producers use the v160.7 `AttributeCrafter` subclass. They all set `baseEfficiency: 1` and `minEfficiency: -1`, so their listed ordinary-terrain rates remain available everywhere and terrain never blocks placement. `maxBoost: 0.25` caps the floor-derived bonus at +25%; it is deliberately not a 2x multiplier for the already high-throughput liquid/gas machines.

| Block | Attribute | `boostScale` | Full-footprint intended maximum | Baseline rate | Terrain-capped rate |
|---|---|---:|---:|---:|---:|
| Nebula Condenser | `spores` | 0.20833334 | Dense 2x2 spore-floor coverage | 100 arkycite/s | 125 arkycite/s |
| Eventide Bioreactor | `spores` | 0.10 | Dense 3x3 spore-floor coverage | 100 neoplasm/s | 125 neoplasm/s |
| Accretion Carbothermal Forge | `steam` | 0.0625 | Four Erekir steam-vent tiles | 2 carbide/s | 2.5 carbide/s |
| Void Atmospheric Harvester | `steam` | 0.0625 | Four Erekir steam-vent tiles | 100 nitrogen/s | 125 nitrogen/s |

`AttributeCrafter` sums the chosen floor attribute beneath the complete footprint. In v160.7, common spore floors contribute 0.15 or 0.30 each and Erekir steam vents contribute 1 each. The four selected blocks set `scaleLiquidConsumption: true`; the engine applies their terrain efficiency scale to standard update consumers, so increased output also raises their continuous liquid and power demand rather than creating a free production multiplier. Item inputs continue to scale naturally because faster crafting completes more item-consuming craft cycles.

`Attribute.heat` is intentionally not used. Erekir has an environmental heat value, which would give an ambient planet-wide bonus rather than a reward for special floor placement. `spores` is reserved for the two biological/spore-themed machines; `steam` is reserved for the two thermal/atmospheric machines. Portable transmuters, water/gas conversion, synthetic oil, cryofluid, and ozone oxidation remain ordinary `GenericCrafter` blocks because their recipes—not terrain—define their intended progression.

## Recipe evaluation

### Stellar Spallation Chamber (`stellar-spallation-chamber`)

| Approach | Assessment |
|---|---|
| A — item-only | **Selected.** Titanium, thorium, and phase fabric are late Serpulo materials with a clear synthesis theme. The single phase-fabric input caps beryllium output behind an established advanced factory without adding pipe infrastructure to the first cross-planet machine. |
| B — item + liquid | Water or oil would add a line but would not improve the intended early cross-planet progression. Cryofluid would duplicate the more advanced tungsten machine's complexity. |
| C — liquid-intensive | Rejected. A large liquid cost would make the first transmuter awkward to site and disproportionally dependent on pumps/conduits rather than its material inputs. |

Final recipe: titanium x3, thorium x2, and phase fabric x1 produce beryllium x1 every 60 ticks. That is 60 beryllium/minute (1/second) at full efficiency, with 2.5 power/second and no liquid input.

### Pulsar Nucleoforge (`pulsar-nucleoforge`)

| Approach | Assessment |
|---|---|
| A — item-only | Viable, but it would make the higher-tier machine differ only by higher item counts and power, offering little operational distinction from the Stellar Spallation Chamber. |
| B — item + liquid | **Selected.** Thorium, surge alloy, and phase fabric establish a costly late-Serpulo material basis; continuous cryofluid adds meaningful cooling and pipe infrastructure while remaining readily producible on Serpulo. |
| C — liquid-intensive | Rejected. Large cryofluid consumption would make coolant generation, rather than rare items and power, the dominant constraint. |

Final recipe: thorium x4, surge alloy x1, phase fabric x1, and cryofluid at 0.1/tick produce tungsten x1 every 120 ticks. That is 30 tungsten/minute (0.5/second) at full efficiency, with 5 power/second, 6 cryofluid/second, and 12 cryofluid/craft.

## Erekir-to-Serpulo recipes

These machines preserve the requested size rule: count every distinct consumed resource, including liquids and gases. `liquidCapacity: 60` is a per-liquid buffer, so each required liquid can be accepted up to 60 units while the native `ConsumeLiquids` consumer removes both rates together.

| Property | Nebular Alchemy Reactor | Neutron Flux Forge | Quasar Plasma Crucible |
|---|---|---|---|
| Item inputs per craft | beryllium 2 | tungsten 1 | tungsten 2, oxide 1 |
| Liquid/gas inputs per craft | slag 12, ozone 6 | arkycite 12, nitrogen 8 | hydrogen 12, ozone 6 |
| Output per craft | copper 5 | lead 5 | titanium 6 |
| Craft time | 300 ticks / 5 seconds | 300 ticks / 5 seconds | 360 ticks / 6 seconds |
| Output rate | 60/minute | 60/minute | 60/minute |
| Size | 3x3 | 3x3 | 4x4 |
| Buffers | 50 items, 60 per liquid | 50 items, 60 per liquid | 50 items, 60 per liquid |
| Power | 3/second | 4/second | 5/second |
| Liquid/gas rate | slag 2.4/s, ozone 1.2/s | arkycite 2.4/s, nitrogen 1.6/s | hydrogen 2/s, ozone 1/s |
| Construction | beryllium 80, tungsten 40, oxide 20 | beryllium 75, tungsten 75, oxide 30, carbide 20 | beryllium 150, tungsten 150, oxide 80, carbide 50 |
| Research cost | beryllium 400, tungsten 200, oxide 100 | beryllium 375, tungsten 375, oxide 150, carbide 100 | beryllium 750, tungsten 750, oxide 400, carbide 250 |
| Research parent | `pulsar-nucleoforge` | `nebular-alchemy-reactor` | `neutron-flux-forge` |

The construction and research values contain only Erekir-obtainable items and follow the project’s standard 5x construction-to-research multiplier.

## Serpulo-to-Erekir advanced materials

Magnetar Oxidation Reactor and Accretion Carbothermal Forge are 2x2 `GenericCrafter` blocks. Their two physical inputs set their footprint; power is infrastructure, not a physical recipe input. Both make existing Erekir items, retain `shownPlanets: [serpulo, erekir]`, and use costs that the Serpulo-rooted Cosmic research tree can pay. Their output quantities are fixed item crafts, not the separate 100/second liquid-output convention.

| Block (ID) | Inputs per 60-tick craft | Output | Output rate | Power | Buffers | Size |
|---|---|---|---:|---:|---|---:|
| Magnetar Oxidation Reactor (`magnetar-oxidation-reactor`) | beryllium 2; ozone 6 | oxide 1 | 1/s (60/minute) | 3/s | 50 items, 60 liquid | 2x2 |
| Accretion Carbothermal Forge (`accretion-carbothermal-forge`) | silicon 2; graphite 3 | carbide 2 | 2/s (120/minute) | 4/s | 50 items | 2x2 |

The existing Cosmic item-machine rates are 1 beryllium/second (Stellar Spallation Chamber), 0.5 tungsten/second (Pulsar Nucleoforge), 1 copper/second (Nebular Alchemy Reactor), 1 lead/second (Neutron Flux Forge), and 1 titanium/second (Quasar Plasma Crucible). Oxide stays at the established 1/second standard. Carbide is 2/second because its two-item output requires both refined silicon and graphite, while still remaining a compact late-tier factory rather than a liquid-output-scale producer.

Vanilla v160.7's 3x3 Oxidation Chamber makes 1 oxide every 120 ticks (0.5/s) from 1 beryllium and ozone at 2/s, with 0.5 power/s plus heat. Magnetar is 2x its item rate, but needs 2 beryllium and 6 ozone per craft, 3 power/s, a 2x2 Cosmic construction tier, and separately established ozone infrastructure. Vanilla's 3x3 Carbide Crucible makes 1 carbide every 33.75 ticks (about 1.78/s) from 2 tungsten and 3 graphite, with 2 power/s plus heat. Accretion makes 2/s (12.5% faster) but substitutes accessible silicon for tungsten, consumes 4 power/s, and is gated behind the preceding oxide tier. Neither removes meaningful infrastructure or makes vanilla progression irrelevant.

### Construction, research, and tree placement

| Block | Construction | Research cost | Parent | Placement reason |
|---|---|---|---|---|
| Magnetar Oxidation Reactor | copper 160, lead 130, graphite 100, silicon 120, titanium 100, thorium 60, plastanium 20 | 5x construction | `ionosphere-reactor` | Ionosphere Reactor establishes the mandatory ozone input. Beryllium is supplied by the separately unlocked Stellar Spallation Chamber; one HJSON node can have only one research parent. |
| Accretion Carbothermal Forge | copper 190, lead 160, graphite 120, silicon 160, titanium 130, thorium 80, plastanium 50, phase fabric 30 | 5x construction | `magnetar-oxidation-reactor` | The research sequence establishes the advanced Erekir-material branch. Oxide is not an ingredient or a research cost, so the parent link is not a production loop. |

Both construction lists add meaningful late-Serpulo structural materials without requiring their own outputs. Research costs are exactly five times construction, preserving the current Cosmic convention and remaining payable by the existing Serpulo root.

### Durability calculations

Both use the Production / GenericCrafter role (`RH 1.10`, `RA 0.90`).

| Block | Material health `MH` | Material armor `MA` | Formula result: HP / armor |
|---|---:|---:|---:|
| Magnetar Oxidation Reactor | 1.131 | 0.275 | 200 / 1 |
| Accretion Carbothermal Forge | 1.153 | 0.302 | 200 / 1 |

## Serpulo liquid and gas production

These six `GenericCrafter` blocks create existing Erekir-associated liquids and gases from Serpulo-progression resources. `outputLiquid` is a **per-tick** continuous rate in v160.7: every block uses `1.6666667`, which is approximately 100 liquid/second or 6000/minute at full efficiency. Unlike item output, the liquid rate is not multiplied by `craftTime`; craft time controls item-input consumption and the active-production cycle.

| Block (ID) | Physical inputs | Output | Output rate | Craft time | Input rates / craft item | Power | Size | Liquid capacity |
|---|---|---|---:|---:|---|---:|---:|---:|
| Nebula Condenser (`nebula-condenser`) | oil, spore pod | arkycite | 100/s | 120 ticks | oil 6/s; spore pod 1/craft | 2/s | 2x2 | 60 |
| Ionosphere Reactor (`ionosphere-reactor`) | water, silicon | ozone | 100/s | 90 ticks | water 6/s; silicon 1/craft | 2/s | 2x2 | 60 |
| Protostar Extractor (`protostar-extractor`) | water | hydrogen | 100/s | 60 ticks | water 10/s | 1.5/s | 1x1 | 60 |
| Void Atmospheric Harvester (`void-atmospheric-harvester`) | water, cryofluid | nitrogen | 100/s | 120 ticks | water 6/s; cryofluid 3/s | 4/s | 2x2 | 60 |
| Astral Catalyzer (`astral-catalyzer`) | arkycite, graphite | cyanogen | 100/s | 180 ticks | arkycite 12/s; graphite 1/craft | 5/s | 2x2 | 60 |
| Eventide Bioreactor (`eventide-bioreactor`) | arkycite, water, phase fabric | neoplasm | 100/s | 240 ticks | arkycite 12/s; water 18/s; phase fabric 1/craft | 7/s | 3x3 | 100 |

All six blocks deliberately output approximately 100 liquid/second. This is far above vanilla factory benchmarks—cyanogen is about 8.3x vanilla Cyanogen Synthesizer's 12/second, and neoplasm is 5x vanilla Neoplasia Reactor's 20/second—so continuous high-capacity liquid transport is required. Neoplasm is a hostile spreading `CellLiquid`, so players must keep its output line contained and test its in-game behavior before relying on it in normal logistics.

## Serpulo liquid synthesis

These two 2x2 `GenericCrafter` blocks manufacture native Serpulo liquids from existing infrastructure. They follow the same 100/second Cosmic liquid-output standard. Their output buffers are intentionally 200, giving a short two-second reserve while a connected liquid network drains the 100/second continuous output.

| Block (ID) | Physical inputs | Output | Output rate | Craft time | Input rates / craft item | Power | Size | Liquid capacity |
|---|---|---|---:|---:|---|---:|---:|---:|
| Stellar Petroforge (`stellar-petroforge`) | graphite, hydrogen | oil | 100/s | 120 ticks | graphite 1.5/s; hydrogen 20/s | 5/s | 2x2 | 200 |
| Nebular Cryoforge (`nebular-cryoforge`) | water, titanium | cryofluid | 100/s | 120 ticks | water 100/s; titanium 5/s | 8.33/s | 2x2 | 200 |

Vanilla Oil Extractor is an attribute-dependent 3x3 Fracker with a displayed base rate of 15 oil/second (`pumpAmount: 0.25`); it needs an oil-bearing floor, water, sand, and 3 power/second. Stellar Petroforge trades that terrain restriction for the existing 100/second Cosmic-output standard, but requires the mod's hydrogen production branch, graphite, and higher power. Vanilla Cryofluid Mixer outputs 12 cryofluid/second from 12 water/second, 1 titanium per 120 ticks, and 1 power/second. Nebular Cryoforge is deliberately 8.33x faster and scales its water, titanium, and power demand accordingly.

### Construction, research, and tree placement

| Block | Construction | Research cost | Parent | Placement reason |
|---|---|---|---|---|
| Stellar Petroforge | copper 150, lead 130, graphite 100, silicon 100, titanium 80, thorium 40, plastanium 20 | 5x construction | `protostar-extractor` | Hydrogen is its required gas and is introduced by Protostar Extractor, preventing an oil or research loop. |
| Nebular Cryoforge | copper 160, lead 140, graphite 90, silicon 130, titanium 120, thorium 60, plastanium 30 | 5x construction | `solar-conduit` | A water-and-titanium Serpulo recipe becomes available after the mod's liquid-transport foundation. |

Both costs are payable through the Serpulo-rooted Cosmic tree and neither contains its produced liquid. Both explicitly use `shownPlanets: [serpulo, erekir]`, which permits use on either planet once supplied but does not create a second research node.

### Durability calculations

The two synthesizers use the Production / GenericCrafter role (`RH 1.10`, `RA 0.90`).

| Block | Material health `MH` | Material armor `MA` | Formula result: HP / armor |
|---|---:|---:|---:|
| Stellar Petroforge | 1.118 | 0.248 | 200 / 1 |
| Nebular Cryoforge | 1.138 | 0.293 | 200 / 1 |

### Construction, research, and tree placement

| Block | Construction | Research cost | Parent | Placement reason |
|---|---|---|---|---|
| Nebula Condenser | copper 110, lead 90, graphite 60, silicon 50, titanium 40 | 5x construction | `solar-conduit` | First arkycite source needs established liquid transport. |
| Ionosphere Reactor | copper 130, lead 110, graphite 70, silicon 100, titanium 70 | 5x construction | `solar-conduit` | First gas branch also requires established liquid transport. |
| Protostar Extractor | copper 70, lead 55, graphite 35, silicon 45, titanium 35 | 5x construction | `ionosphere-reactor` | Follows the first controlled gas process with a simpler single-fluid recipe. |
| Void Atmospheric Harvester | copper 150, lead 125, graphite 80, silicon 100, titanium 110, thorium 60, plastanium 40, surge alloy 20 | 5x construction | `protostar-extractor` | Cryogenic separation belongs after the hydrogen branch and late-Serpulo cooling. |
| Astral Catalyzer | copper 180, lead 150, graphite 110, silicon 130, titanium 110, thorium 90, plastanium 60, phase fabric 60, surge alloy 60 | 5x construction | `nebula-condenser` | Requires the mod's arkycite source before cyanogen synthesis. |
| Eventide Bioreactor | copper 240, lead 200, graphite 130, silicon 170, titanium 160, thorium 140, plastanium 100, phase fabric 100, surge alloy 100 | 5x construction | `astral-catalyzer` | Final hazardous biological branch follows arkycite and exotic-process infrastructure. |

All construction and research costs are payable from the existing Serpulo-rooted Cosmic research inventory. The blocks remain buildable on both planets through `shownPlanets: [serpulo, erekir]`; that list does not make a second tech-tree node.

### Durability calculations

All six use the Production / GenericCrafter role (`RH 1.10`, `RA 0.90`).

| Block | Material health `MH` | Material armor `MA` | Formula result: HP / armor |
|---|---:|---:|---:|
| Nebula Condenser | 1.075 | 0.160 | 190 / 1 |
| Ionosphere Reactor | 1.088 | 0.177 | 190 / 1 |
| Protostar Extractor | 1.086 | 0.177 | 50 / 1 |
| Void Atmospheric Harvester | 1.162 | 0.349 | 200 / 2 |
| Astral Catalyzer | 1.175 | 0.369 | 210 / 2 |
| Eventide Bioreactor | 1.189 | 0.404 | 470 / 2 |

### Research accessibility limitation

The current `Vanilla Cosmic` root is explicitly assigned to Serpulo. In v160.7, all child nodes inherit that root’s planet and research inventory source. Therefore these Erekir-only research costs cannot currently be paid from the Cosmic tree, even though the blocks are shown on both planets. The HJSON lane is valid and non-circular—Pulsar Nucleoforge → Nebular Alchemy Reactor → Neutron Flux Forge → Quasar Plasma Crucible—but it is deliberately future-gated until the mod adopts a custom shared-inventory research system, adds a separate Erekir-side progression design, or revises the root and cost model. `shownPlanets` does not solve this engine limitation.

## Progression and costs

| Property | Stellar Spallation Chamber | Pulsar Nucleoforge |
|---|---|---|
| Size | 2x2 | 3x3 |
| Research parent | `titanium-rift-fracturer` | `stellar-spallation-chamber` |
| Construction | copper 120, lead 100, titanium 80, thorium 50, silicon 50, phase fabric 15 | copper 220, lead 180, titanium 150, thorium 120, silicon 120, plastanium 60, phase fabric 40, surge alloy 30 |
| Research cost | 5x its construction recipe | 5x its construction recipe |
| Output | beryllium x1 / 60 ticks | tungsten x1 / 120 ticks |
| Output rate | 60/minute (1/second) | 30/minute (0.5/second) |
| Input per craft | titanium 3, thorium 2, phase fabric 1 | thorium 4, surge alloy 1, phase fabric 1, cryofluid 12 |
| Power | 2.5/second | 5/second |
| Liquid | none | cryofluid 6/second |
| Internal buffers | 50 items | 50 items, 100 liquid |

The Pulsar Nucleoforge follows the Stellar Spallation Chamber in research because it represents the next transmutation tier, but it does **not** consume, require, or research-cost beryllium. The resulting chain is linear in machine knowledge while remaining free of a resource-production deadlock.

Both output rates are deliberate fixed-factory rates, not a claim of mining parity: native Erekir extraction depends on ore layout, drill type, boosters, and operating frontage. These machines trade a modest, reliable Serpulo output for expensive refined inputs, power, and—at the tungsten tier—continuous cryofluid.

## Durability calculations

### Stellar Spallation Chamber (`stellar-spallation-chamber`)

Construction total: 415.

```text
MH = 1 + ((100 x 0.05) + (80 x 0.30) + (50 x 0.45) + (50 x 0.10) + (15 x 0.50)) / 415 = 1.154
HP = roundNearest10(40 x 2^2 x 1.154 x 1.10) = 200

MA = ((100 x 0.20) + (80 x 0.80) + (50 x 1.00) + (15 x 0.50)) / 415 = 0.341
Armor = roundNearestWhole((2 + 1.5 x 2) x 0.341 x 0.90) = 2
```

### Pulsar Nucleoforge (`pulsar-nucleoforge`)

Construction total: 920.

```text
MH = 1 + ((180 x 0.05) + (150 x 0.30) + (120 x 0.45) + (120 x 0.10) + (60 x 0.35) + (40 x 0.50) + (30 x 0.70)) / 920 = 1.198
HP = roundNearest10(40 x 3^2 x 1.198 x 1.10) = 470

MA = ((180 x 0.20) + (150 x 0.80) + (120 x 1.00) + (60 x 0.70) + (40 x 0.50) + (30 x 1.50)) / 920 = 0.416
Armor = roundNearestWhole((2 + 1.5 x 3) x 0.416 x 0.90) = 2
```

### Nebular Alchemy Reactor (`nebular-alchemy-reactor`)

Construction total: 140.

```text
MH = 1 + ((80 x 0.15) + (40 x 0.40) + (20 x 0.45)) / 140 = 1.264
HP = roundNearest10(40 x 3^2 x 1.264 x 1.10) = 500

MA = ((80 x 0.30) + (40 x 1.20) + (20 x 0.70)) / 140 = 0.614
Armor = roundNearestWhole((2 + 1.5 x 3) x 0.614 x 0.90) = 4
```

### Neutron Flux Forge (`neutron-flux-forge`)

Construction total: 200.

```text
MH = 1 + ((75 x 0.15) + (75 x 0.40) + (30 x 0.45) + (20 x 0.65)) / 200 = 1.339
HP = roundNearest10(40 x 3^2 x 1.339 x 1.10) = 530

MA = ((75 x 0.30) + (75 x 1.20) + (30 x 0.70) + (20 x 1.40)) / 200 = 0.808
Armor = roundNearestWhole((2 + 1.5 x 3) x 0.808 x 0.90) = 5
```

### Quasar Plasma Crucible (`quasar-plasma-crucible`)

Construction total: 430.

```text
MH = 1 + ((150 x 0.15) + (150 x 0.40) + (80 x 0.45) + (50 x 0.65)) / 430 = 1.351
HP = roundNearest10(40 x 4^2 x 1.351 x 1.10) = 950

MA = ((150 x 0.30) + (150 x 1.20) + (80 x 0.70) + (50 x 1.40)) / 430 = 0.816
Armor = roundNearestWhole((2 + 1.5 x 4) x 0.816 x 0.90) = 6
```

## Sprite requirements

Most listed machines use `DrawMulti` to layer the native default main region and a white, additive `DrawGlowRegion` overlay. `DrawGlowRegion` is visible only while the machine has crafting warmup (power and all required inputs are being consumed); it is intentionally absent while idle. No bottom, liquid, rotator, or top region is required unless a row states otherwise.

The liquid/gas producers layer a bottom region, a fixed input-liquid `DrawLiquidTile`, the main region, and an active glow. `DrawLiquidTile` uses native tiled liquid frames and needs no `-liquid.png`; fixed input liquids avoid an ambiguous shaped overlay for machines that store multiple inputs and outputs. These liquid tiles deliberately omit padding, allowing their fill layer to span each block's full footprint beneath the main sprite. Seven 2x2 machines share `cosmic-liquid-bottom.png` through an explicit `DrawRegion.name`; Magnetar adds its ozone `DrawLiquidTile`. Accretion has no bottom layer because it has no liquid/gas chamber. Protostar Extractor and Eventide Bioreactor retain their correctly sized individual bottom regions.

When configuring a drawer in HJSON, use a separate multiline object for any region that has more than `type`; HJSON does not treat spaces between inline object fields as separators. The transmuters therefore declare `type`, `suffix`, `color`, `alpha`, and `glowIntensity` on separate lines for `DrawGlowRegion`.

| Block | Main sprite | Recommended dimensions |
|---|---|---|
| Stellar Spallation Chamber (`stellar-spallation-chamber`) | `stellar-spallation-chamber.png`, `stellar-spallation-chamber-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Pulsar Nucleoforge (`pulsar-nucleoforge`) | `pulsar-nucleoforge.png`, `pulsar-nucleoforge-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Nebular Alchemy Reactor (`nebular-alchemy-reactor`) | `nebular-alchemy-reactor.png`, `nebular-alchemy-reactor-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Neutron Flux Forge (`neutron-flux-forge`) | `neutron-flux-forge.png`, `neutron-flux-forge-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Quasar Plasma Crucible (`quasar-plasma-crucible`) | `quasar-plasma-crucible.png`, `quasar-plasma-crucible-glow.png` | 128x128 pixels each (4x4 at 32 pixels/tile) |
| Nebula Condenser (`nebula-condenser`) | `nebula-condenser.png`, shared `cosmic-liquid-bottom.png`, `nebula-condenser-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Ionosphere Reactor (`ionosphere-reactor`) | `ionosphere-reactor.png`, shared `cosmic-liquid-bottom.png`, `ionosphere-reactor-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Protostar Extractor (`protostar-extractor`) | `protostar-extractor.png`, `protostar-extractor-bottom.png`, `protostar-extractor-glow.png` | 32x32 pixels each (1x1 at 32 pixels/tile) |
| Void Atmospheric Harvester (`void-atmospheric-harvester`) | `void-atmospheric-harvester.png`, shared `cosmic-liquid-bottom.png`, `void-atmospheric-harvester-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Astral Catalyzer (`astral-catalyzer`) | `astral-catalyzer.png`, shared `cosmic-liquid-bottom.png`, `astral-catalyzer-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Eventide Bioreactor (`eventide-bioreactor`) | `eventide-bioreactor.png`, `eventide-bioreactor-bottom.png`, `eventide-bioreactor-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Stellar Petroforge (`stellar-petroforge`) | `stellar-petroforge.png`, shared `cosmic-liquid-bottom.png`, `stellar-petroforge-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Nebular Cryoforge (`nebular-cryoforge`) | `nebular-cryoforge.png`, shared `cosmic-liquid-bottom.png`, `nebular-cryoforge-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Magnetar Oxidation Reactor (`magnetar-oxidation-reactor`) | `magnetar-oxidation-reactor.png`, shared `cosmic-liquid-bottom.png`, `magnetar-oxidation-reactor-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Accretion Carbothermal Forge (`accretion-carbothermal-forge`) | `accretion-carbothermal-forge.png`, `accretion-carbothermal-forge-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |

No sprite files are generated by this implementation.

## Sources and validation scope

- [GenericCrafter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/production/GenericCrafter.java)
- [ConsumeItems.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItems.java)
- [ConsumeLiquid.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquid.java)
- [ConsumeLiquids.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquids.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
- [Block Durability Balance Guide](block-durability-balance-guide.md)

Static validation confirms declared content IDs, item/liquid names, source-planet cost sets, research-parent IDs, arithmetic, and supported GenericCrafter fields. It does not replace in-game testing of load order, tech-tree placement, sprite packing, or final gameplay balance.
