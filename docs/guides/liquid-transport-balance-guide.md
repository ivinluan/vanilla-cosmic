# Vanilla Cosmic — Liquid Transport Balance Guide (Mindustry v160.7)

This guide covers Vanilla Cosmic liquid transport and storage blocks. It does not equate liquid transport with conveyor item throughput: capacity and pressure control liquid storage and transfer behavior, not a fixed displayed liquids-per-second value.

## Current progression

| Block | Capacity | Pressure | Build requirements | Research parent | Research cost |
|---|---:|---:|---|---|---|
| Solar Conduit | 30 | 1.25 | copper 2, lead 2, metaglass 2 | conduit | copper 10, lead 10, metaglass 10 |
| Supernova Conduit | 60 | 1.75 | lead 2, metaglass 3, titanium 2, silicon 2 | solar-conduit | lead 10, metaglass 15, titanium 10, silicon 10 |
| Void Conduit | 100 | 2.50 | metaglass 4, titanium 3, silicon 3, thorium 2, plastanium 2, surge alloy 1 | supernova-conduit | metaglass 20, titanium 15, silicon 15, thorium 10, plastanium 10, surge alloy 5 |

Vanilla Conduit → Solar Conduit → Supernova Conduit → Void Conduit

Research costs are exactly 5× their matching construction requirement, following the repository-wide resource rule.

## Liquid storage

| Block | Class | Size | Capacity | Power | Build requirements | Research parent | HP / armor |
|---|---|---:|---:|---|---|---|---:|
| Celestial Reservoir | LiquidRouter | 4×4 | 13,500 | None | metaglass 150, beryllium 250, tungsten 200, carbide 25 | reinforced-liquid-tank | 1,390 / 7 |

Celestial Reservoir is a dedicated storage block using native `LiquidRouter` behavior: it stores one liquid type and distributes that liquid to valid neighboring blocks when they can accept it. Its 13,500 capacity is exactly 5× vanilla Reinforced Liquid Tank's verified 2,700 capacity. It has no power requirement and leaves `liquidPressure` at the native default; capacity is not treated as a transfer-speed stat.

## Liquid distribution branch

| Block | Class | Capacity | Pressure | Build requirements | Research parent | HP / armor |
|---|---|---:|---:|---|---|---:|
| Cosmic Liquid Router | LiquidRouter | 300 | Not used by this class | beryllium 35, tungsten 25, carbide 12, metaglass 20 | solar-conduit | 50 / 2 |
| Cosmic Liquid Junction | LiquidJunction | No liquid buffer | Not used by this class | beryllium 40, tungsten 30, carbide 15, metaglass 20 | cosmic-liquid-router | 50 / 2 |

Solar Conduit → Cosmic Liquid Router → Cosmic Liquid Junction

The router has 300 liquid capacity, exceeding Void Conduit's 100, so it can act as an advanced routing buffer. The junction has no internal liquid module: it sends a liquid source straight through the opposite side when that destination accepts it, avoiding a shared buffer and preserving the two crossing paths.

## Long-range liquid bridges

| Block | Class | Range | Capacity | Pressure | Power | Research parent | HP / armor |
|---|---|---:|---:|---:|---:|---|---:|
| Celestial Aqueduct | LiquidBridge | 25 | 150 | 1.10 | None | supernova-conduit | 50 / 1 |
| Voidstream Hyperlane | LiquidBridge | 50 | 300 | 1.20 | 0.55 power/sec | void-conduit | 50 / 1 |

Celestial Aqueduct mirrors Celestial Bridge's 25-tile unpowered range. Voidstream Hyperlane mirrors Interstellar Hyperlane's 50-tile range and 0.55 power/sec requirement. Both use LiquidBridge's native linking behavior and only link with their own block type by default.

The original pressure proposals of 1.5 and 2.5 were intentionally reduced to 1.10 and 1.20. LiquidBridge forwards liquid through the standard liquid movement calculation, so large pressure values can create excessively large one-tick transfers from a full internal buffer. Capacity 150/300 already provides substantial buffering.

## Liquid bridge mechanics

- LiquidBridge inherits ItemBridge link/range mechanics, but disables item storage and moves its current liquid through the target bridge after native warmup.
- liquidCapacity and inherited liquidPressure are supported. Liquid pressure affects the source fill fraction used by liquid transfer; neither field establishes a fixed liquid-per-second throughput.
- LiquidBridge overrides ItemBridge transport and does not use item transportTime. It calls liquid movement once per tick after warmup, so item-bridge speeds cannot be numerically matched with supported HJSON.
- At a full source and empty same-capacity target, the first transfer can be up to the source capacity per tick: 150 liquid/tick (9,000 liquid/sec theoretical burst) for Celestial Aqueduct and 300 liquid/tick (18,000 liquid/sec theoretical burst) for Voidstream. This is a burst ceiling, not sustained network throughput.
- Celestial Aqueduct follows vanilla Bridge Conduit's unpowered visual and disabled-acceptance settings. Voidstream follows Phase Conduit's pulse and arrow behavior, but uses the approved 0.55 power/sec custom requirement.

## Destruction hazard progression

`flammabilityScale` and `explosivenessScale` only modify the destruction effect contributed by stored liquid; they do not alter normal liquid transport. A value of `1` is the native full contribution, `0.5` is half, and `0` removes that contribution.

| Blocks | Flammability scale | Explosiveness scale | Intended result |
|---|---:|---:|---|
| Solar Conduit, Supernova Conduit | 0 | 1 | Cannot ignite from stored liquid, but a loaded explosive-liquid line ruptures at full strength. |
| Void Conduit, Voidstream Hyperlane | 0 | 0 | Void containment prevents stored liquids from adding fire or explosion effects. |
| Cosmic Liquid Router, Cosmic Liquid Junction | 0.5 | 0.5 | Reinforced routing hardware halves, but does not erase, the risk of a loaded hub. |
| Celestial Aqueduct | 0 | 0.5 | Sealed transport prevents fire but keeps a controlled rupture risk for stored explosive liquid. |

## Liquid router and junction mechanics

- LiquidRouter accepts an incoming liquid only when empty or already holding that same liquid. It distributes its current liquid to valid neighboring liquid blocks using native fill balancing. It has no item-style speed setting and does not use liquidPressure.
- LiquidJunction does not store or mix liquids. Its native destination lookup routes an input to the opposite side; it remains blocked when the destination cannot accept the liquid. It has no capacity, pressure, or speed field to tune in HJSON.
- Cosmic Liquid Router uses solid: false and underBullets: true, matching vanilla Liquid Router's practical placement behavior. Cosmic Liquid Junction uses solid: false, matching vanilla Liquid Junction.

## Verified flow behavior

- A conduit updates forward liquid movement each tick while it contains liquid.
- The source fill fraction is source amount / source capacity × liquidPressure. Increasing pressure therefore favors forward movement into a less-filled destination.
- The transfer calculation is bounded by the source's stored liquid, its capacity, the destination's free capacity, liquid acceptance, and relative fill levels. Capacity and pressure do not define an independently reliable liquid-per-second number.
- Native Conduit has leaks: true, padCorners: true, and is unpowered by default. These blocks preserve those values; no power consumer or hasPower field is added.
- liquidPadding is not a v160.7 Conduit property. Do not add it to HJSON. padCorners is the supported related visual setting and is left at its native default.

## Durability

Conduits use the approved liquid-transport role multiplier: health RH = 1.00, armor RA = 0.80.

| Block | Health calculation result | Armor calculation result | Implemented values |
|---|---:|---:|---|
| Solar Conduit | 40 | 0 | 40 HP, 0 armor |
| Supernova Conduit | 50 | 1 | 50 HP, 1 armor |
| Void Conduit | 50 | 1 | 50 HP, 1 armor |

The calculations use the formulas and material coefficients in [Block Durability Balance Guide](block-durability-balance-guide.md). They intentionally keep inexpensive 1×1 liquid transport from becoming wall-like.

## Vanilla comparison and limits

Vanilla Conduit uses 20 capacity; Pulse Conduit uses 40 capacity and 1.025 pressure; Plated Conduit uses 50 capacity and 1.025 pressure. The cosmic sequence increases both capacity and pressure, while retaining the native Conduit class's leakage and directional behavior.

Exact sustained throughput must be tested in game with realistic source, destination, pipe length, branching, and liquid-network conditions. No fixed throughput claim should be inferred from these fields alone.

## Asset requirements

No sprites were created or modified. Conduit loads five top variants, five bottom variants, and a cap region. For each new ID, provide:

- block-id-top-0.png through block-id-top-4.png
- block-id-bottom-0.png through block-id-bottom-4.png (the vanilla conduit-bottom fallback exists, but custom artwork is recommended)
- block-id-cap.png

For example, Solar Conduit uses solar-conduit-top-0.png through solar-conduit-top-4.png, matching bottom variants, and solar-conduit-cap.png.

Liquid Router needs cosmic-liquid-router.png and cosmic-liquid-router-bottom.png. Liquid Junction needs cosmic-liquid-junction.png. No artwork was created or modified.

Celestial Reservoir also uses the native LiquidRouter renderer and requires `celestial-reservoir.png` and `celestial-reservoir-bottom.png`. No artwork was created or modified.

Each LiquidBridge needs four regions: block-id.png, block-id-end.png, block-id-bridge.png, and block-id-arrow.png. Therefore create celestial-aqueduct.png, celestial-aqueduct-end.png, celestial-aqueduct-bridge.png, celestial-aqueduct-arrow.png, and the corresponding voidstream-hyperlane files.

## Sources

- [Conduit.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/Conduit.java)
- [LiquidBlock.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidBlock.java)
- [BuildingComp.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/entities/comp/BuildingComp.java)
- [LiquidRouter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidRouter.java)
- [LiquidJunction.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidJunction.java)
- [LiquidBridge.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidBridge.java)
- [ItemBridge.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/distribution/ItemBridge.java)
- [Vanilla conduit definitions](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
