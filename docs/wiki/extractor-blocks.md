# Cosmic Vanilla Extractors

This catalogue covers blocks that take materials directly from the map: tile drills, wall bores, wall crafters, and liquid pumps. `Input` means ongoing operating input. Construction and research costs are documented in the block HJSON and balance guides, not duplicated here.

Every drill-family picture is a documentation preview composited from the block’s existing layers in Mindustry draw order. Standard drills use base, rotator, and top; WallCrafters additionally use `-rotator-bottom` beneath the base. These previews are not game sprites, do not alter any source artwork, and intentionally omit glow layers.

Rates assume full valid ore or wall coverage. Standard drills and mini drills output the mineable ore below their footprint; beam bores output the mineable ore in their valid wall-facing beams. Their liquid booster is optional and increases speed by the configured boost intensity.

## Drill — standard Cosmic I–VIII line

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="images/extractors/cosmic-i-drill.png" alt="Cosmic Drill" width="96"></div> | Cosmic Drill (I) | Optional water 4.8/s; 1.65× liquid boost. | Mineable floor ore, 0.80 items/s. |
| <div align="center"><img src="images/extractors/cosmic-ii-drill.png" alt="Cosmic II Drill" width="96"></div> | Cosmic II Drill | Optional water 6/s; 1.75× liquid boost. | Mineable floor ore, 1.00 items/s. |
| <div align="center"><img src="images/extractors/cosmic-iii-drill.png" alt="Cosmic III Drill" width="96"></div> | Cosmic III Drill | Optional water 7.2/s; 1.85× liquid boost. | Mineable floor ore, 1.26 items/s. |
| <div align="center"><img src="images/extractors/cosmic-iv-drill.png" alt="Cosmic IV Drill" width="96"></div> | Cosmic IV Drill | Optional water 8.4/s; 1.95× liquid boost. | Mineable floor ore, 1.60 items/s. |
| <div align="center"><img src="images/extractors/cosmic-v-drill.png" alt="Cosmic V Drill" width="96"></div> | Cosmic V Drill | Optional water 9.6/s; 2.05× liquid boost. | Mineable floor ore, 2.00 items/s. |
| <div align="center"><img src="images/extractors/cosmic-vi-drill.png" alt="Cosmic VI Drill" width="96"></div> | Cosmic VI Drill | Optional water 10.8/s; 2.15× liquid boost. | Mineable floor ore, 2.40 items/s. |
| <div align="center"><img src="images/extractors/cosmic-vii-drill.png" alt="Cosmic VII Drill" width="96"></div> | Cosmic VII Drill | Optional water 12/s; 2.25× liquid boost. | Mineable floor ore, 3.00 items/s. |
| <div align="center"><img src="images/extractors/cosmic-viii-drill.png" alt="Cosmic VIII Drill" width="96"></div> | Cosmic VIII Drill | Optional water 13.2/s; 2.35× liquid boost. | Mineable floor ore, 3.69 items/s. |

## Drill — mini Cosmic I–VIII line

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="images/extractors/cosmic-i-drill-mini.png" alt="mini Cosmic Drill" width="64"></div> | mini Cosmic Drill (I) | Optional water 4.8/s; 1.65× liquid boost. | Mineable floor ore, 0.27 items/s. |
| <div align="center"><img src="images/extractors/cosmic-ii-drill-mini.png" alt="mini Cosmic II Drill" width="64"></div> | mini Cosmic II Drill | Optional water 6/s; 1.75× liquid boost. | Mineable floor ore, 0.34 items/s. |
| <div align="center"><img src="images/extractors/cosmic-iii-drill-mini.png" alt="mini Cosmic III Drill" width="64"></div> | mini Cosmic III Drill | Optional water 7.2/s; 1.85× liquid boost. | Mineable floor ore, 0.43 items/s. |
| <div align="center"><img src="images/extractors/cosmic-iv-drill-mini.png" alt="mini Cosmic IV Drill" width="64"></div> | mini Cosmic IV Drill | Optional water 8.4/s; 1.95× liquid boost. | Mineable floor ore, 0.55 items/s. |
| <div align="center"><img src="images/extractors/cosmic-v-drill-mini.png" alt="mini Cosmic V Drill" width="64"></div> | mini Cosmic V Drill | Optional water 9.6/s; 2.05× liquid boost. | Mineable floor ore, 0.67 items/s. |
| <div align="center"><img src="images/extractors/cosmic-vi-drill-mini.png" alt="mini Cosmic VI Drill" width="64"></div> | mini Cosmic VI Drill | Optional water 10.8/s; 2.15× liquid boost. | Mineable floor ore, 0.80 items/s. |
| <div align="center"><img src="images/extractors/cosmic-vii-drill-mini.png" alt="mini Cosmic VII Drill" width="64"></div> | mini Cosmic VII Drill | Optional water 12/s; 2.25× liquid boost. | Mineable floor ore, 1.00 items/s. |
| <div align="center"><img src="images/extractors/cosmic-viii-drill-mini.png" alt="mini Cosmic VIII Drill" width="64"></div> | mini Cosmic VIII Drill | Optional water 13.2/s; 2.35× liquid boost. | Mineable floor ore, 1.20 items/s. |

## Drill and BurstDrill — advanced floor extraction

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="images/extractors/cosmic-drill-pro.png" alt="Cosmic Drill Pro" width="128"></div> | Cosmic Drill Pro | Power 18/s; optional cryofluid 14.4/s; 2.45× liquid boost. | Mineable floor ore, 10.38 items/s. |
| <div align="center"><img src="images/extractors/cosmic-drill-pro-max.png" alt="Cosmic Drill Pro Max" width="160"></div> | Cosmic Drill Pro Max | Power 25.8/s; optional cryofluid 15.6/s; 2.55× liquid boost. | Mineable floor ore, 22.86 items/s. |
| <div align="center"><img src="images/extractors/supernova-drill.png" alt="Supernova Drill" width="192"></div> | Supernova Drill | Power 18/s; cryofluid 36/s; slag 36/s. | Mineable floor ore, 20.53 items/s. |

## BeamDrill — wall bores

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="images/extractors/cosmic-prism-bore.png" alt="Cosmic Prism Bore" width="96"></div> | Cosmic Prism Bore | Power 48/s; optional neoplasm 7.8/s; 1.5× liquid boost. | Mineable ore on valid wall frontage, 1.50 items/s. |
| <div align="center"><img src="images/extractors/cosmic-radiant-bore.png" alt="Cosmic Radiant Bore" width="128"></div> | Cosmic Radiant Bore | Power 60/s; optional neoplasm 9.6/s; 1.88× liquid boost. | Mineable ore on valid wall frontage, 2.81 items/s. |
| <div align="center"><img src="images/extractors/cosmic-spectral-bore.png" alt="Cosmic Spectral Bore" width="160"></div> | Cosmic Spectral Bore | Power 75/s; optional neoplasm 12/s; 2.35× liquid boost. | Mineable ore on valid wall frontage, 4.71 items/s. |
| <div align="center"><img src="images/extractors/cosmic-photon-bore.png" alt="Cosmic Photon Bore" width="192"></div> | Cosmic Photon Bore | Power 93.6/s; optional neoplasm 15/s; 2.94× liquid boost. | Mineable ore on valid wall frontage, 7.32 items/s. |

## WallCrafter — Rift Fracturers

All Rift Fracturers require a valid sand-attribute wall on their working frontage. Their output is fixed by the block; the wall does not determine whether titanium, thorium, beryllium, or tungsten is produced.

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="images/extractors/titanium-rift-fracturer.png" alt="Titanium Rift Fracturer" width="160"></div> | Titanium Rift Fracturer | Power 45/s; optional water 14.4/s; 1.6× liquid boost. | Titanium, 4.36 items/s. |
| <div align="center"><img src="images/extractors/thorium-rift-fracturer.png" alt="Thorium Rift Fracturer" width="160"></div> | Thorium Rift Fracturer | Power 51/s; optional water 14.4/s; 1.6× liquid boost. | Thorium, 4.00 items/s. |
| <div align="center"><img src="images/extractors/beryllium-rift-fracturer.png" alt="Beryllium Rift Fracturer" width="160"></div> | Beryllium Rift Fracturer | Power 54/s; optional water 15.6/s; 1.6× liquid boost. | Beryllium, 4.36 items/s. |
| <div align="center"><img src="images/extractors/tungsten-rift-fracturer.png" alt="Tungsten Rift Fracturer" width="160"></div> | Tungsten Rift Fracturer | Power 66/s; optional water 16.8/s; 1.6× liquid boost. | Tungsten, 4.80 items/s. |

## Pump — liquid extraction

Pumps do not have a predetermined liquid output: they extract the valid liquid below their footprint.

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/liquid/pulsar-liquid-pump.png" alt="Pulsar Liquid Pump" width="64"></div> | Pulsar Liquid Pump | No ongoing resource input; must cover valid liquid tiles. | Liquid under each valid tile, 25 liquid/s per tile. |
| <div align="center"><img src="../../sprites/blocks/liquid/quasar-liquid-pump.png" alt="Quasar Liquid Pump" width="128"></div> | Quasar Liquid Pump | No ongoing resource input; must cover valid liquid tiles. | Liquid under each valid tile, 100 liquid/s per tile. |

