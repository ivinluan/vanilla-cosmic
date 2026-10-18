# Vanilla Cosmic — Serpulo Transmuter Balance Guide (Mindustry v160.7)

This guide covers GenericCrafter machines that produce an existing Erekir item from resources obtained through Serpulo progression. The Beryllium and Tungsten Transmuters are intentionally Serpulo-facing: both explicitly use `shownPlanets: [serpulo]`. This presentation/progression setting does not make an item native to Serpulo, unlock Erekir buildings, or impose an environment-placement restriction.

## Technical conventions

- Use `type: GenericCrafter`, `category: crafting`, and `outputItem: item/amount` for a fixed vanilla item output. `output` is a different field used by blocks such as WallCrafter. Do not create duplicate custom items.
- `craftTime` is ticks. At full efficiency, `items/minute = output amount x 3600 / craftTime`.
- `consumes.power` and mandatory liquid `amount` are **per tick**. Divide a desired power-per-second value by 60. Multiply a liquid-per-tick value by 60 for liquid/second and by `craftTime` for liquid/craft.
- Set `itemCapacity` and, where needed, `liquidCapacity` as buffers only; neither is a production-rate multiplier.
- All input items, liquids, construction requirements, and research costs must be Serpulo obtainable. Never require the Erekir item that the first machine introduces.
- Apply the Production / GenericCrafter durability role (`RH 1.10`, `RA 0.90`) from the [Block Durability Balance Guide](block-durability-balance-guide.md). Add a row to the durability audit for every new machine.

## Recipe evaluation

### Beryllium Transmuter

| Approach | Assessment |
|---|---|
| A — item-only | **Selected.** Titanium, thorium, and phase fabric are late Serpulo materials with a clear synthesis theme. The single phase-fabric input caps beryllium output behind an established advanced factory without adding pipe infrastructure to the first cross-planet machine. |
| B — item + liquid | Water or oil would add a line but would not improve the intended early cross-planet progression. Cryofluid would duplicate the more advanced tungsten machine's complexity. |
| C — liquid-intensive | Rejected. A large liquid cost would make the first transmuter awkward to site and disproportionally dependent on pumps/conduits rather than its material inputs. |

Final recipe: titanium x3, thorium x2, and phase fabric x1 produce beryllium x1 every 60 ticks. That is 60 beryllium/minute (1/second) at full efficiency, with 2.5 power/second and no liquid input.

### Tungsten Transmuter

| Approach | Assessment |
|---|---|
| A — item-only | Viable, but it would make the higher-tier machine differ only by higher item counts and power, offering little operational distinction from Beryllium Transmuter. |
| B — item + liquid | **Selected.** Thorium, surge alloy, and phase fabric establish a costly late-Serpulo material basis; continuous cryofluid adds meaningful cooling and pipe infrastructure while remaining readily producible on Serpulo. |
| C — liquid-intensive | Rejected. Large cryofluid consumption would make coolant generation, rather than rare items and power, the dominant constraint. |

Final recipe: thorium x4, surge alloy x1, phase fabric x1, and cryofluid at 0.1/tick produce tungsten x1 every 120 ticks. That is 30 tungsten/minute (0.5/second) at full efficiency, with 5 power/second, 6 cryofluid/second, and 12 cryofluid/craft.

## Progression and costs

| Property | Beryllium Transmuter | Tungsten Transmuter |
|---|---|---|
| Size | 2x2 | 3x3 |
| Research parent | `phase-weaver` | `beryllium-transmuter` |
| Construction | copper 120, lead 100, titanium 80, thorium 50, silicon 50, phase fabric 15 | copper 220, lead 180, titanium 150, thorium 120, silicon 120, plastanium 60, phase fabric 40, surge alloy 30 |
| Research cost | 5x its construction recipe | 5x its construction recipe |
| Output | beryllium x1 / 60 ticks | tungsten x1 / 120 ticks |
| Output rate | 60/minute (1/second) | 30/minute (0.5/second) |
| Input per craft | titanium 3, thorium 2, phase fabric 1 | thorium 4, surge alloy 1, phase fabric 1, cryofluid 12 |
| Power | 2.5/second | 5/second |
| Liquid | none | cryofluid 6/second |
| Internal buffers | 50 items | 50 items, 100 liquid |

The Tungsten Transmuter follows Beryllium Transmuter in research because it represents the next transmutation tier, but it does **not** consume, require, or research-cost beryllium. The resulting chain is linear in machine knowledge while remaining free of a resource-production deadlock.

Both output rates are deliberate fixed-factory rates, not a claim of mining parity: native Erekir extraction depends on ore layout, drill type, boosters, and operating frontage. These machines trade a modest, reliable Serpulo output for expensive refined inputs, power, and—at the tungsten tier—continuous cryofluid.

## Durability calculations

### Beryllium Transmuter

Construction total: 415.

```text
MH = 1 + ((100 x 0.05) + (80 x 0.30) + (50 x 0.45) + (50 x 0.10) + (15 x 0.50)) / 415 = 1.154
HP = roundNearest10(40 x 2^2 x 1.154 x 1.10) = 200

MA = ((100 x 0.20) + (80 x 0.80) + (50 x 1.00) + (15 x 0.50)) / 415 = 0.341
Armor = roundNearestWhole((2 + 1.5 x 2) x 0.341 x 0.90) = 2
```

### Tungsten Transmuter

Construction total: 920.

```text
MH = 1 + ((180 x 0.05) + (150 x 0.30) + (120 x 0.45) + (120 x 0.10) + (60 x 0.35) + (40 x 0.50) + (30 x 0.70)) / 920 = 1.198
HP = roundNearest10(40 x 3^2 x 1.198 x 1.10) = 470

MA = ((180 x 0.20) + (150 x 0.80) + (120 x 1.00) + (60 x 0.70) + (40 x 0.50) + (30 x 1.50)) / 920 = 0.416
Armor = roundNearestWhole((2 + 1.5 x 3) x 0.416 x 0.90) = 2
```

## Sprite requirements

Each transmuter uses `DrawMulti` to layer the native default main region and a white, additive `DrawGlowRegion` overlay. `DrawGlowRegion` is visible only while the machine has crafting warmup (power and all required inputs are being consumed); it is intentionally absent while idle. No bottom, liquid, rotator, or top region is required.

When configuring a drawer in HJSON, use a separate multiline object for any region that has more than `type`; HJSON does not treat spaces between inline object fields as separators. The transmuters therefore declare `type`, `suffix`, `color`, `alpha`, and `glowIntensity` on separate lines for `DrawGlowRegion`.

| Block | Main sprite | Recommended dimensions |
|---|---|---|
| Beryllium Transmuter | `beryllium-transmuter.png`, `beryllium-transmuter-glow.png` | 64x64 pixels each (2x2 at 32 pixels/tile) |
| Tungsten Transmuter | `tungsten-transmuter.png`, `tungsten-transmuter-glow.png` | 96x96 pixels each (3x3 at 32 pixels/tile) |

No sprite files are generated by this implementation.

## Sources and validation scope

- [GenericCrafter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/production/GenericCrafter.java)
- [ConsumeItems.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItems.java)
- [ConsumeLiquid.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquid.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
- [Block Durability Balance Guide](block-durability-balance-guide.md)

Static validation confirms declared content IDs, item/liquid names, Serpulo-only cost sets, research-parent IDs, arithmetic, and supported GenericCrafter fields. It does not replace in-game testing of load order, tech-tree placement, sprite packing, or final gameplay balance.
