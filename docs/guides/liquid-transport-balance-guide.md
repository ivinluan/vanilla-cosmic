# Vanilla Cosmic — Liquid Transport Balance Guide (Mindustry v160.7)

This guide covers native Conduit blocks. It does not equate liquid transport with conveyor item throughput: conduit capacity and pressure control liquid storage and transfer behavior, not a fixed displayed liquids-per-second value.

## Current progression

| Block | Capacity | Pressure | Build requirements | Research parent | Research cost |
|---|---:|---:|---|---|---|
| Solar Conduit | 30 | 1.25 | copper 2, lead 2, metaglass 2 | conduit | copper 10, lead 10, metaglass 10 |
| Supernova Conduit | 60 | 1.75 | lead 2, metaglass 3, titanium 2, silicon 2 | solar-conduit | lead 10, metaglass 15, titanium 10, silicon 10 |
| Void Conduit | 100 | 2.50 | metaglass 4, titanium 3, silicon 3, thorium 2, plastanium 2, surge alloy 1 | supernova-conduit | metaglass 20, titanium 15, silicon 15, thorium 10, plastanium 10, surge alloy 5 |

Vanilla Conduit → Solar Conduit → Supernova Conduit → Void Conduit

Research costs are exactly 5× their matching construction requirement, following the repository-wide resource rule.

## Liquid distribution branch

| Block | Class | Capacity | Pressure | Build requirements | Research parent | HP / armor |
|---|---|---:|---:|---|---|---:|
| Cosmic Liquid Router | LiquidRouter | 300 | Not used by this class | beryllium 35, tungsten 25, carbide 12, metaglass 20 | solar-conduit | 50 / 2 |
| Cosmic Liquid Junction | LiquidJunction | No liquid buffer | Not used by this class | beryllium 40, tungsten 30, carbide 15, metaglass 20 | cosmic-liquid-router | 50 / 2 |

Solar Conduit → Cosmic Liquid Router → Cosmic Liquid Junction

The router has 300 liquid capacity, exceeding Void Conduit's 100, so it can act as an advanced routing buffer. The junction has no internal liquid module: it sends a liquid source straight through the opposite side when that destination accepts it, avoiding a shared buffer and preserving the two crossing paths.

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

## Sources

- [Conduit.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/Conduit.java)
- [LiquidBlock.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidBlock.java)
- [BuildingComp.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/entities/comp/BuildingComp.java)
- [LiquidRouter.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidRouter.java)
- [LiquidJunction.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/liquid/LiquidJunction.java)
- [Vanilla conduit definitions](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
