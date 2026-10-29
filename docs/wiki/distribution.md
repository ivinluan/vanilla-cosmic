# Cosmic Vanilla Distribution

This page lists the Cosmic Vanilla blocks that move, route, bridge, or store items and liquids. `Input` describes ongoing operating resources or what the block accepts; it does not repeat construction or research costs.

Only each block's default sprite is shown. Conveyor connection frames, bridge endpoints and arrows, stack overlays, liquid fills, glow layers, and router bottoms are deliberately omitted. Conduit entries use their default `-top-0` connection frame.

## Conveyor and stack transport

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/distribution/conveyors/solar-conveyor-0-0.png" alt="Solar Conveyor" width="48"></div> | Solar Conveyor | Items from its back; no power. | Moves items forward at 16 items/s. |
| <div align="center"><img src="../../sprites/blocks/distribution/conveyors/supernova-conveyor-0-0.png" alt="Supernova Conveyor" width="48"></div> | Supernova Conveyor | Items from its back; no power. | Moves items forward at 21 items/s. |
| <div align="center"><img src="../../sprites/blocks/distribution/conveyors/void-conveyor-0-0.png" alt="Void Conveyor" width="48"></div> | Void Conveyor | Items from its back; no power. | Moves items forward at 26 items/s. |
| <div align="center"><img src="../../sprites/blocks/distribution/stack-conveyors/stellar-stack-conveyor.png" alt="Stellar Stack Conveyor" width="48"></div> | Stellar Stack Conveyor | Items from its back; power 4/s. | Moves 25-item stacks; rated 100 items/s. |
| <div align="center"><img src="../../sprites/blocks/distribution/ducts/stellar-stack-router.png" alt="Stellar Stack Router" width="48"></div> | Stellar Stack Router | Item stacks; power 6/s. | Distributes a full 25-item stack in a burst to connected outputs. |

## Item routing and crossing

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/distribution/cosmic-router.png" alt="Cosmic Router" width="48"></div> | Cosmic Router | Items from connected sides; no power. | Evenly distributes items among eligible outputs. |
| <div align="center"><img src="../../sprites/blocks/distribution/cosmic-junction.png" alt="Cosmic Junction" width="48"></div> | Cosmic Junction | Two intersecting item streams; no power. | Passes streams across without mixing; displayed speed 26 items/s. |

## Item bridges and long-range launch

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/distribution/celestial-bridge.png" alt="Celestial Bridge" width="48"></div> | Celestial Bridge | Linked bridge; items; no power. | Bridges items up to 25 tiles; 30-item buffer; displayed speed 15 items/s. |
| <div align="center"><img src="../../sprites/blocks/distribution/interstellar-hyperlane.png" alt="Interstellar Hyperlane" width="48"></div> | Interstellar Hyperlane | Linked hyperlane; items; power 33/s. | Bridges items up to 50 tiles; 50-item capacity; 2.3-tick transport interval. |
| <div align="center"><img src="images/distribution/quasar-mass-driver.png" alt="Quasar Mass Driver" width="160"></div> | Quasar Mass Driver | Items; power 360/s; a receiving Quasar Mass Driver in range. | Launches shipments of at least 50 items, up to 250 stored, across 1,320 tiles. |

## Liquid conduits

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/liquid/conduits/solar-conduit-top-0.png" alt="Solar Conduit default top frame" width="48"></div> | Solar Conduit | Any liquid from the previous conduit; no power. | Sends liquid forward; 30 liquid capacity; pressure 1.25. |
| <div align="center"><img src="../../sprites/blocks/liquid/conduits/supernova-conduit-top-0.png" alt="Supernova Conduit default top frame" width="48"></div> | Supernova Conduit | Any liquid from the previous conduit; no power. | Sends liquid forward; 60 liquid capacity; pressure 1.75. |
| <div align="center"><img src="../../sprites/blocks/liquid/conduits/void-conduit-top-0.png" alt="Void Conduit default top frame" width="48"></div> | Void Conduit | Any liquid from the previous conduit; no power. | Sends liquid forward; 100 liquid capacity; pressure 2.5. |

## Liquid routing and storage

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/liquid/cosmic-liquid-router.png" alt="Cosmic Liquid Router" width="48"></div> | Cosmic Liquid Router | Any connected liquid stream; no power. | Evenly distributes liquid among valid outputs; 300 liquid capacity. |
| <div align="center"><img src="../../sprites/blocks/liquid/cosmic-liquid-junction.png" alt="Cosmic Liquid Junction" width="48"></div> | Cosmic Liquid Junction | Two intersecting liquid streams; no power. | Passes streams across without interference. |
| <div align="center"><img src="../../sprites/blocks/liquid/celestial-reservoir.png" alt="Celestial Reservoir" width="160"></div> | Celestial Reservoir | Any connected liquid stream; no power. | Stores and routes up to 13,500 liquid. |

## Liquid bridges

| Picture | Name | Input | Output |
| --- | --- | --- | --- |
| <div align="center"><img src="../../sprites/blocks/liquid/celestial-aqueduct.png" alt="Celestial Aqueduct" width="48"></div> | Celestial Aqueduct | Linked aqueduct; any liquid; no power. | Bridges liquid up to 25 tiles; 150 liquid capacity; pressure 1.1. |
| <div align="center"><img src="../../sprites/blocks/liquid/voidstream-hyperlane.png" alt="Voidstream Hyperlane" width="48"></div> | Voidstream Hyperlane | Linked hyperlane; any liquid; power 33/s. | Bridges liquid up to 50 tiles; 300 liquid capacity; pressure 1.2. |

