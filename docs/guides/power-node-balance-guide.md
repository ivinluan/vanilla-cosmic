# Vanilla Cosmic — Power Node Balance Guide (Mindustry v160.7)

This guide covers native `PowerNode` blocks. A power node distributes an existing graph; it does not generate, consume, or buffer power unless another block class or consumer is explicitly configured.

## Native behavior and configuration

`laserRange` is measured in tiles. `maxNodes` is the maximum number of configured links stored by that node. With the native defaults retained here, `autolink` is enabled, `drawRange` is enabled, `laserScale` is `0.25`, `useLod` is enabled, and manual configuration can add or remove valid links. Native link validation also honors the endpoint node's own range and its available link capacity.

Both Synchrotron Nodes retain the native `PowerNode` defaults: `crushFragile: false` and `schematicPriority: -10`. Neither block has a power consumer, generator output, or battery capacity.

## Series balance

| Block | Size | Range | Links | Construction | Research | Health / armor | Parent |
|---|---:|---:|---:|---|---|---:|---|
| Synchrotron Node | 1×1 | 30 tiles | 25 | copper 75, lead 75, silicon 60, titanium 60, thorium 40, surge alloy 25 | 5× construction | 50 / 1 | `cosmic-i-drill` |
| Synchrotron Node II | 2×2 | 60 tiles | 5 | copper 140, lead 120, silicon 120, titanium 100, thorium 80, plastanium 60, phase fabric 40, surge alloy 40 | 5× construction | 200 / 2 | `synchrotron-node` |

Vanilla Power Node is 1×1 with 6 range and 10 links; Large Power Node is 2×2 with 15 range and 15 links; Surge Tower is 2×2 with 40 range and 2 links. Synchrotron Node's 30-range/25-link combination is a deliberately expensive dense-network hub. Node II raises range to 60, but its fixed 5-link maximum means it cannot replace the compact node where many local connections are needed.

The recipe progression uses only Serpulo-obtainable construction and research items, starts with copper/lead, then adds conductive silicon, titanium, thorium, and surge alloy. Node II adds plastanium and phase fabric only for the meaningful 60-tile transmission upgrade. The standard 5× research-cost rule applies to every material. The [durability guide](block-durability-balance-guide.md) adds the Power nodes and transmission relays role (`RH 1.00`, `RA 0.80`); utility range and connections do not grant wall-level durability.

## Cross-planet scope

Both definitions use `shownPlanets: [serpulo, erekir]`, so they are visible and applicable on both planets after unlock. They retain only one Serpulo-rooted Cosmic research node each; the list does not create an Erekir node or make Serpulo construction materials native to Erekir.

Native `PowerNode` links operate through normal compatible connected-power blocks. Erekir `BeamNode` uses separate directional linking behavior, so direct BeamNode-to-Synchrotron-Node interoperability is not promised and requires in-game testing.

## Sprite checklist

Create sprites under `sprites/blocks/power/power-node/`.

| Sprite | Required | Dimensions | Purpose |
|---|---|---:|---|
| `synchrotron-node.png` | Yes | 32×32 px | Main 1×1 Synchrotron Node region. |
| `synchrotron-node-ii.png` | Yes | 64×64 px | Main 2×2 Synchrotron Node II region. |
| `synchrotron-node-laser.png` | Optional | 4×48 px | Overrides the native power-link beam; otherwise v160.7 falls back to `laser`. |
| `synchrotron-node-laser-end.png` | Optional | 72×72 px | Overrides the native beam endcap; otherwise falls back to `laser-end`. |
| `synchrotron-node-ii-laser.png` | Optional | 4×48 px | Node II beam override; native fallback is used if absent. |
| `synchrotron-node-ii-laser-end.png` | Optional | 72×72 px | Node II beam-end override; native fallback is used if absent. |

The specified `laserColor1` and `laserColor2` tint the native link drawing. No custom beam sprites are required for the selected implementation.

## Sources and validation scope

- [PowerNode.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/PowerNode.java)
- [PowerBlock.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/power/PowerBlock.java)
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java)
- [Block Durability Balance Guide](block-durability-balance-guide.md)

Static review verifies fields, arithmetic, and source behavior. It does not replace in-game tests of autolinking, manual links, insulated obstacles, mixed Serpulo/Erekir power networks, or sprite packing.
