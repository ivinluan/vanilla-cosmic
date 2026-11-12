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

## Pyroclast Dynamo and Exergonic Reactor

Both use native `ConsumeItems`, which requires every listed item before a new cycle can begin and removes all listed stacks together. Once paid, a `ConsumeGenerator` remains active until `itemDuration` elapses even if a later delivery runs out; it then stops until both inputs are available again.

| Block | Required items per cycle | Cycle | Output | Fuel rate | Energy/cycle | Parent |
|---|---|---:|---:|---:|---:|---|
| Pyroclast Dynamo | 2 pyratite, 1 graphite | 90 ticks / 1.5 s | 6/tick = 360/s | 1.333 pyratite/s, 0.667 graphite/s; 80/40 per minute | 540 | `entropy-furnace` |
| Exergonic Reactor | 1 graphite, 1 oxide | 120 ticks / 2 s | 7/tick = 420/s | 0.5 graphite/s and 0.5 oxide/s; 30/30 per minute | 840 | `magnetar-oxidation-reactor` |

Pyroclast is the earlier solid-fuel workhorse: its processed pyratite requirement gives it 360/s, above Steam Generator’s 330/s, while its 1.5-second cycle creates ongoing delivery demand. Exergonic adds only 60/s, not a universal replacement; its longer, more efficient cycle depends on oxide, an advanced material. Both remain below vanilla Chemical Combustion Chamber’s 550/s and far below Differential Generator’s 1080/s.

### Cross-planet fuel routes

- **Pyroclast Dynamo:** Serpulo has vanilla pyratite and graphite. The current Cosmic content has graphite production on both planets, but no pyratite or coal-production route on Erekir; the existing production catalogue explicitly identifies this gap. The block is shown and can be constructed on both planets, but it is not self-sufficient on Erekir until a future pyratite supply route is added. This implementation does not add an unrelated factory.
- **Exergonic Reactor:** Erekir has graphite and its normal oxide progression. On Serpulo, the existing `magnetar-oxidation-reactor` produces oxide from beryllium and ozone, and its prerequisite chain provides the needed infrastructure. Its research parent therefore establishes the intended oxide route without using oxide as an inaccessible research cost.

### Construction and durability

| Block | Construction | Research | MH / MA | Health / armor |
|---|---|---|---:|---:|
| Pyroclast Dynamo | copper 100, lead 80, graphite 55, silicon 50, titanium 40 | 5× every construction item | 1.078 / 0.165 | 190 / 1 |
| Exergonic Reactor | copper 140, lead 120, graphite 90, silicon 100, titanium 75, thorium 45, plastanium 30, surge alloy 40 | 5× every construction item | 1.163 / 0.342 | 200 / 2 |

Both apply the established **Power generators** role (`RH 1.10`, `RA 0.90`). The advanced reactor gains modest reinforcement from thorium, plastanium, and surge alloy rather than defensive-wall durability.

## Nullstar Reactor

**Role:** a 4×4 `ImpactReactor` for late-game, high-output generation. It is intentionally a demanding network component, not a universal replacement for ordinary fuel generators: it needs an already powered grid, two solid fuel items, and two continuous containment liquids.

| Block | Required items per cycle | Continuous inputs | Startup / warmup | Gross output | Continuous power draw | Net output at full warmup | Parent |
|---|---|---|---:|---:|---:|---:|---|
| Nullstar Reactor | 2 thorium + 2 blast compound every 180 ticks / 3 seconds | cryofluid 30/s; nitrogen 18/s | 100% supplied external power; `warmupSpeed: 0.002` | 390/tick = 23,400/s | 100/tick = 6,000/s | 17,400/s | `void-atmospheric-harvester` |

Vanilla Impact Reactor produces 130/tick = 7,800/s gross and consumes 25/tick = 1,500/s. Nullstar's 390/tick gross production is exactly **3×** vanilla Impact Reactor (the specified 200% advantage), while its 6,000/s power requirement is 4× the native operating draw. This leaves 17,400/s net at full warmup, compared with vanilla's 6,300/s net.

Native `ImpactReactor` behavior requires consumer efficiency and at least 99% powered status before warmup rises. With the configured `0.002` warmup speed, it reaches power break-even at approximately 12 seconds, 90% warmup in about 19 seconds, and 99% warmup in about 38 seconds. Losing sufficient incoming power or any mandatory item/liquid input makes it cool down; the block does not operate on a partial fuel recipe. Its native reactor explosion behavior remains unchanged rather than being multiplied with output.

### Cross-planet supply constraint

`shownPlanets` makes the block discoverable on both Serpulo and Erekir. Cryofluid and nitrogen have existing Cosmic production routes on both worlds, but blast compound has no native Erekir source and Vanilla Cosmic does not currently produce it. Therefore Erekir players cannot self-supply Nullstar without a future blast-compound route, another mod, or a sandbox supply. This guide records the gap instead of adding an unrelated factory.

### Construction, research, and durability

Construction: copper 500, lead 450, graphite 300, silicon 400, titanium 300, thorium 250, plastanium 180, phase fabric 125, surge alloy 150. The added titanium, phase fabric, and surge alloy represent cryogenic containment, field control, and high-current infrastructure. Research is exactly 5× every construction quantity, preserving the project's standard research convention and using materials available through the Serpulo-rooted Cosmic tree.

Nullstar uses the **High-output reactors** durability role (`RH 1.20`, `RA 1.00`). Its material values yield `MH = 1.196` and `MA = 0.386`; the 4×4 calculation is `health = 40 × 16 × 1.196 × 1.20 = 919`, rounded to **920**, and `armor = (2 + 1.5 × 4) × 0.386 × 1.00 = 3.1`, rounded to **3**. This is reinforced processing containment, not defensive-wall protection.

### Construction, research, and durability

Construction: copper 120, lead 100, graphite 60, silicon 70, titanium 50, thorium 30. The recipe represents ordinary generator structure, conductive control, and heat containment without requiring the furnace’s advanced possible fuels. Research is exactly 5× each construction cost and is parented to `cosmic-i-drill`, so it remains payable in the Serpulo-rooted Cosmic tree despite being shown on both planets.

The documented **Power generators** durability role yields `MH = 1.105`, `MA = 0.223`, `RH = 1.10`, `RA = 0.90`, then `health = 190` and `armor = 1`. The pressure hazard does not justify defensive-wall stats.

## Sprite checklist

| Sprite | Required | Dimensions | Purpose |
|---|---|---:|---|
| `sprites/blocks/power/consume-generator/entropy-furnace.png` | Yes | 64×64 px | Native default main region for the 2×2 furnace. |
| `sprites/blocks/power/consume-generator/entropy-furnace-glow.png` | Yes | 64×64 px | Active-only `DrawGlowRegion` overlay; it is visible while the furnace is generating power. |
| `sprites/blocks/general/cosmic-liquid-bottom.png` | Existing shared asset | 64×64 px | Structural layer shared through the explicit `vanilla-cosmic-cosmic-liquid-bottom` atlas name. |
| `sprites/blocks/power/consume-generator/pyroclast-dynamo.png` | Yes | 64×64 px | Native default main region for the 2×2 Pyroclast Dynamo. |
| `sprites/blocks/power/consume-generator/pyroclast-dynamo-glow.png` | Yes | 64×64 px | Active-only glow overlay for the Pyroclast Dynamo. |
| `sprites/blocks/power/consume-generator/exergonic-reactor.png` | Yes | 64×64 px | Native default main region for the 2×2 Exergonic Reactor. |
| `sprites/blocks/power/consume-generator/exergonic-reactor-glow.png` | Yes | 64×64 px | Active-only glow overlay for the Exergonic Reactor. |
| `sprites/blocks/general/cosmic-liquid-bottom-xlarge.png` | Yes, shared | 128×128 px | Explicit atlas region `vanilla-cosmic-cosmic-liquid-bottom-xlarge` used as Nullstar's containment base. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor.png` | Yes | 128×128 px | Default 4×4 reactor body. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor-plasma-0.png` | Yes | 128×128 px | First native `DrawPlasma` layer. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor-plasma-1.png` | Yes | 128×128 px | Second native `DrawPlasma` layer. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor-plasma-2.png` | Yes | 128×128 px | Third native `DrawPlasma` layer. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor-plasma-3.png` | Yes | 128×128 px | Fourth native `DrawPlasma` layer. |
| `sprites/blocks/power/impact-reactor/nullstar-reactor-glow.png` | Yes | 128×128 px | Warmup-driven glow overlay. |

Entropy Furnace uses shared bottom → native tiled slag fill → main region → active glow. Pyroclast Dynamo and Exergonic Reactor use main region → active glow only: neither requires a `-bottom` or `-liquid` sprite. Nullstar uses the shared xlarge containment base → four native plasma layers → main body → warmup glow; it does **not** use `nullstar-reactor-bottom.png` or `nullstar-reactor-liquid.png`.

## Sources and validation scope

- [ConsumeGenerator.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/ConsumeGenerator.java)
- [ConsumeItemList.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItemList.java)
- [ConsumeItemFilter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItemFilter.java)
- [ImpactReactor.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/ImpactReactor.java)
- [ConsumeItems.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeItems.java)
- [ConsumeLiquids.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/consumers/ConsumeLiquids.java)
- [DrawPlasma.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/draw/DrawPlasma.java)
- [DrawGlowRegion.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/draw/DrawGlowRegion.java)
- [ContentParser.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/mod/ContentParser.java)
- [PowerGenerator.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/PowerGenerator.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)

Static validation covers field support, fuel eligibility, the 5× research ratio, durability arithmetic, and native fuel/waste behavior. In-game testing remains needed for fuel-delivery behavior, actual slag export, full-buffer destruction and area damage, research availability, and the owner-created sprite.
