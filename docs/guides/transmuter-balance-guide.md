# Vanilla Cosmic — Cross-Planet Transmuter Balance Guide (Mindustry v160.7)

This guide covers GenericCrafter machines that manufacture an existing item native to the opposite campaign from locally obtainable resources. Stellar Spallation Chamber (`stellar-spallation-chamber`) and Pulsar Nucleoforge (`pulsar-nucleoforge`) provide Erekir items from Serpulo progression; Nebular Alchemy Reactor (`nebular-alchemy-reactor`), Neutron Flux Forge (`neutron-flux-forge`), and Quasar Plasma Crucible (`quasar-plasma-crucible`) provide Serpulo items from Erekir progression. All are available on both planets through `shownPlanets: [serpulo, erekir]`. This controls content applicability; it does not create a second tech-tree node, make an item native to another planet, unlock vanilla buildings, or impose an environment-placement restriction.

## Technical conventions

- Use `type: GenericCrafter`, `category: crafting`, and `outputItem: item/amount` for a fixed vanilla item output. `output` is a different field used by blocks such as WallCrafter. Do not create duplicate custom items.
- `craftTime` is ticks. At full efficiency, `items/minute = output amount x 3600 / craftTime`.
- `consumes.power` and mandatory liquid `amount` are **per tick**. Divide a desired power-per-second value by 60. Multiply a liquid-per-tick value by 60 for liquid/second and by `craftTime` for liquid/craft.
- Set `itemCapacity` and, where needed, `liquidCapacity` as buffers only; neither is a production-rate multiplier.
- Use ingredients, construction requirements, and research costs obtainable on the source planet. Never require the target item that the machine introduces.
- Multiple mandatory liquids/gases must use one `consumes.liquids` array. v160.7 `ConsumeLiquids` accepts, checks, and removes every declared `LiquidStack` each tick. Gases such as ozone, hydrogen, and nitrogen use the same `Liquid` system.
- Apply the Production / GenericCrafter durability role (`RH 1.10`, `RA 0.90`) from the [Block Durability Balance Guide](block-durability-balance-guide.md). Add a row to the durability audit for every new machine.

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

Each transmuter uses `DrawMulti` to layer the native default main region and a white, additive `DrawGlowRegion` overlay. `DrawGlowRegion` is visible only while the machine has crafting warmup (power and all required inputs are being consumed); it is intentionally absent while idle. No bottom, liquid, rotator, or top region is required.

When configuring a drawer in HJSON, use a separate multiline object for any region that has more than `type`; HJSON does not treat spaces between inline object fields as separators. The transmuters therefore declare `type`, `suffix`, `color`, `alpha`, and `glowIntensity` on separate lines for `DrawGlowRegion`.

| Block | Main sprite | Recommended dimensions |
|---|---|---|
| Stellar Spallation Chamber (`stellar-spallation-chamber`) | `stellar-spallation-chamber.png`, `stellar-spallation-chamber-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Pulsar Nucleoforge (`pulsar-nucleoforge`) | `pulsar-nucleoforge.png`, `pulsar-nucleoforge-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Nebular Alchemy Reactor (`nebular-alchemy-reactor`) | `nebular-alchemy-reactor.png`, `nebular-alchemy-reactor-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Neutron Flux Forge (`neutron-flux-forge`) | `neutron-flux-forge.png`, `neutron-flux-forge-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |
| Quasar Plasma Crucible (`quasar-plasma-crucible`) | `quasar-plasma-crucible.png`, `quasar-plasma-crucible-glow.png` | 128x128 pixels each (4x4 at 32 pixels/tile) |

No sprite files are generated by this implementation.

## Sources and validation scope

- [GenericCrafter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/production/GenericCrafter.java)
- [ConsumeItems.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItems.java)
- [ConsumeLiquid.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquid.java)
- [ConsumeLiquids.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquids.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
- [Block Durability Balance Guide](block-durability-balance-guide.md)

Static validation confirms declared content IDs, item/liquid names, source-planet cost sets, research-parent IDs, arithmetic, and supported GenericCrafter fields. It does not replace in-game testing of load order, tech-tree placement, sprite packing, or final gameplay balance.
