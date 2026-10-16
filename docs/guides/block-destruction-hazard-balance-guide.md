# Vanilla Cosmic — Block Destruction Hazard Balance Guide (Mindustry v160.7)

This guide controls the destruction hazard contributed by items or liquids stored inside a block. It does not change normal production, transfer speed, health, armor, or a block's basic destruction behavior.

## Scale convention

| Value | Meaning |
|---:|---|
| `0` | Stored contents add no fire or explosion contribution. |
| `0.25` | Stored contents contribute one quarter of the normal amount. |
| `0.5` | Stored contents contribute half of the normal amount. |
| `1` | Native, full contribution. |

`flammabilityScale` controls the fire-related contribution and `explosivenessScale` controls the explosion-strength contribution. Neither value is a tile radius.

## Naming-based policy

| Theme / block family | Flammability | Explosiveness | Reason |
|---|---:|---:|---|
| Void (Void Conduit, Voidstream Hyperlane, Void Conveyor) | 0 | 0 | Void containment fully isolates stored contents. |
| Celestial | 0 | 0.5 | Sealed transport cannot ignite but may rupture in a controlled way. |
| Interstellar | 0 | 0.25 | Vacuum transport strongly suppresses a stored-content rupture. |
| Cosmic drills, bores, routers, and junctions | 0.5 | 0.5 | Cosmic containment reduces, but does not eliminate, operational risk. |
| Solar, Stellar, Pulsar, Quasar, Supernova, and Rift machinery | 0 | 1 | The machinery does not ignite from contents, but a loaded block retains a serious full-strength rupture. |

Apply both properties explicitly to each new block, choosing the family that best matches its name and intended containment technology. Use a different value only when the block's gameplay role clearly requires it, and document the exception.

## Current exceptions

- Cosmic Liquid Router and Cosmic Liquid Junction are Cosmic containment blocks and use `0.5` / `0.5`.
- Celestial Aqueduct is a Celestial transport block and uses `0` / `0.5`.
- Solar and Supernova Conduits deliberately use `0` / `1`, matching the stellar machinery rule.

## Engine note

Mindustry v160.7 declares both properties on the base `Block` class with a default value of `1`. Therefore they are supported by all current block definitions, but only have a meaningful effect when a destroyed block holds contents that can contribute flammability or explosiveness.

## Source

- [Block.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/Block.java)
