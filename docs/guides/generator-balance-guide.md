# Vanilla Cosmic — Generator Balance Guide (Mindustry v160.7)

This guide records balance decisions for native power generators. A `PowerNode` moves an existing network; a generator creates power.

## Native HJSON behavior and limitation

- `ConsumeGenerator.powerProduction` is power per tick. Its displayed output is `powerProduction × 60` per second.
- One fuel trigger removes one item. `itemDuration` is its active cycle in ticks; the next item is not consumed until that cycle ends.
- HJSON `consumes.itemList` maps to `ConsumeItemList`, which can accept an explicit arbitrary list of vanilla items. It scans stored items in ascending native content-ID order, so it selects the first eligible stored item. There is no native HJSON priority or manual fuel selection.
- The engine freezes the selected item’s `itemDurationMultiplier` when that cycle begins. This makes per-item duration and total energy reliable.
- The engine does **not** freeze a `ConsumeItemList.itemMultipliers` efficiency value: it reads the next eligible stored item every update. With mixed fuel storage, an HJSON power multiplier could change before the current cycle ends. Therefore native HJSON cannot reliably tie fuel-specific instantaneous output to the burned item.

The Entropy Furnace uses the smallest fully supported solution: all accepted items use a neutral efficiency multiplier, while `itemDurationMultipliers` defines each item’s total energy. It outputs a fixed 300 power/second during a cycle; better fuels sustain it longer. A custom Java `ConsumeGeneratorBuild` would be required for reliable fuel-specific *instantaneous* power, and is deliberately not introduced into this HJSON-only mod.

## Entropy Furnace

**Role:** a universal 2×2 solid-fuel generator. It accepts all 20 ordinary visible vanilla items from Serpulo and Erekir. Its advantages are cross-planet versatility and simple single-item fuel cycles. Its constraints are that its peak output remains below a Steam Generator, it has no fuel-priority control, and it needs continuous slag removal.

### Configuration and balance

| Property | Value | Reason |
|---|---:|---|
| Output while active | 5 power/tick = 300/second | Below vanilla Steam Generator’s 330/second. |
| Base duration | 120 ticks / 2 seconds | The highest-energy fuel gets a complete, visible cycle. |
| Fuel buffer | 60 items | Operating convenience only; does not increase rate. |
| Slag output | 0.06666667/tick = 4/second | A sustained but manageable liquid-export requirement. |
| Slag buffer | 120 | Fills from empty in 30 seconds if output is blocked. |
| Full-buffer outcome | Native self-destruction; radius 4, 300 damage | A local logistics failure, not reactor-scale area denial. |

Every fuel produces the same 300 power/second and 4 slag/second **while active**. Its multiplier changes the 120-tick cycle length and therefore the total energy and slag per item.

`explodeOnFull` always destroys the furnace when its slag buffer reaches capacity. The configured radius/damage blast is emitted through `PowerGenerator.onDestroyed()` only when the active game rules allow reactor explosions; this is native v160.7 behavior. The block’s destruction itself does not depend on that rule.

| Fuel | Duration multiplier | Cycle | Power/second | Total energy/item | Slag/item |
|---|---:|---:|---:|---:|---:|
| Scrap | 0.12 | 14.4 ticks / 0.24 s | 300 | 72 | 0.96 |
| Sand | 0.10 | 12 ticks / 0.20 s | 300 | 60 | 0.80 |
| Copper | 0.20 | 24 ticks / 0.40 s | 300 | 120 | 1.60 |
| Lead | 0.20 | 24 ticks / 0.40 s | 300 | 120 | 1.60 |
| Coal | 0.50 | 60 ticks / 1 s | 300 | 300 | 4.00 |
| Graphite | 0.60 | 72 ticks / 1.2 s | 300 | 360 | 4.80 |
| Silicon | 0.45 | 54 ticks / 0.9 s | 300 | 270 | 3.60 |
| Metaglass | 0.35 | 42 ticks / 0.7 s | 300 | 210 | 2.80 |
| Titanium | 0.65 | 78 ticks / 1.3 s | 300 | 390 | 5.20 |
| Thorium | 0.75 | 90 ticks / 1.5 s | 300 | 450 | 6.00 |
| Plastanium | 0.65 | 78 ticks / 1.3 s | 300 | 390 | 5.20 |
| Phase Fabric | 0.80 | 96 ticks / 1.6 s | 300 | 480 | 6.40 |
| Surge Alloy | 1.00 | 120 ticks / 2 s | 300 | 600 | 8.00 |
| Spore Pod | 0.50 | 60 ticks / 1 s | 300 | 300 | 4.00 |
| Pyratite | 0.80 | 96 ticks / 1.6 s | 300 | 480 | 6.40 |
| Blast Compound | 0.90 | 108 ticks / 1.8 s | 300 | 540 | 7.20 |
| Beryllium | 0.60 | 72 ticks / 1.2 s | 300 | 360 | 4.80 |
| Tungsten | 0.80 | 96 ticks / 1.6 s | 300 | 480 | 6.40 |
| Oxide | 0.75 | 90 ticks / 1.5 s | 300 | 450 | 6.00 |
| Carbide | 0.90 | 108 ticks / 1.8 s | 300 | 540 | 7.20 |

`fissile-matter` and `dormant-cyst` are intentionally excluded because they are hidden/internal items. Common surplus supplies have the shortest cycles; refined, structural, and energetic materials give longer cycles without making them compulsory construction ingredients.

### Vanilla comparison

| Generator | Size | Output/second | Fuel and tradeoff |
|---|---:|---:|---|
| Combustion Generator | 1×1 | 60 | Flammable item only; no mandatory waste line. |
| Steam Generator | 2×2 | 330 | Flammable item plus water; no slag output. |
| Differential Generator | 3×3 | 1080 | Pyratite and cryofluid; specialized high output. |
| RTG Generator | 2×2 | 270 | Radioactive fuel only; very long fuel cycles. |
| Entropy Furnace | 2×2 | 300 while active | Any ordinary item; item value is total cycle energy, and slag must be exported. |

The Entropy Furnace does not replace Steam: its active output is lower, it creates mandatory waste, and low-value materials need frequent delivery. Future specialized Cosmic generators should exceed it only through their intended fuel and infrastructure requirements.

### Construction, research, and durability

Construction: copper 120, lead 100, graphite 60, silicon 70, titanium 50, thorium 30. The recipe represents ordinary generator structure, conductive control, and heat containment without requiring the furnace’s advanced possible fuels. Research is exactly 5× each construction cost and is parented to `cosmic-i-drill`, so it remains payable in the Serpulo-rooted Cosmic tree despite being shown on both planets.

The documented **Power generators** durability role yields `MH = 1.105`, `MA = 0.223`, `RH = 1.10`, `RA = 0.90`, then `health = 190` and `armor = 1`. The pressure hazard does not justify defensive-wall stats.

## Sprite checklist

| Sprite | Required | Dimensions | Purpose |
|---|---|---:|---|
| `sprites/blocks/power/consume-generator/entropy-furnace.png` | Yes | 64×64 px | Native default main region for the 2×2 furnace. |
| `sprites/blocks/power/consume-generator/entropy-furnace-glow.png` | Yes | 64×64 px | Active-only `DrawGlowRegion` overlay; it is visible while the furnace is generating power. |
| `sprites/blocks/general/cosmic-liquid-bottom.png` | Existing shared asset | 64×64 px | Structural layer shared through the explicit `vanilla-cosmic-cosmic-liquid-bottom` atlas name. |

The drawer order is shared bottom → native tiled slag fill → main region → active glow. `DrawLiquidTile` uses native liquid frames and needs no block-specific `-liquid` sprite.

## Sources and validation scope

- [ConsumeGenerator.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/ConsumeGenerator.java)
- [ConsumeItemList.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItemList.java)
- [ConsumeItemFilter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItemFilter.java)
- [ContentParser.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/mod/ContentParser.java)
- [PowerGenerator.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/PowerGenerator.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)

Static validation covers field support, fuel eligibility, the 5× research ratio, durability arithmetic, and native fuel/waste behavior. In-game testing remains needed for fuel-delivery behavior, actual slag export, full-buffer destruction and area damage, research availability, and the owner-created sprite.
