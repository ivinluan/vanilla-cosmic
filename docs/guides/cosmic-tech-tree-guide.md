# Vanilla Cosmic — Technology Tree Guide (Mindustry v160.7)

## Root and scope

`cosmic-i-drill` is the `Vanilla Cosmic` root node. Its HJSON research object declares `root: true`, `name: vanilla-cosmic`, `planet: serpulo`, and `requiresUnlock: false`; this creates a selectable third research tree without modifying either vanilla tree. The root is deliberately Serpulo-based, so its research inventory is drawn from Serpulo campaign sectors.

All Cosmic blocks retain `shownPlanets: [serpulo, erekir]` where configured. That controls content applicability and build/database visibility after unlock; it does not duplicate a research node into the two native planet trees.

## Progression

```text
Vanilla Cosmic — Cosmic I Drill
├─ Drill chain: Cosmic I → II → III → IV → V → VI → VII → VIII → Pro → Pro Max → Supernova
│  └─ Mini chain: Cosmic I Mini → II Mini → III Mini → IV Mini → V Mini → VI Mini → VII Mini → VIII Mini
├─ Bores: Prism → Radiant → Spectral → Photon
├─ Rift extraction: Titanium → Thorium → Beryllium → Tungsten
│  ├─ Stellar Spallation Chamber → Pulsar Nucleoforge → Nebular Alchemy Reactor → Neutron Flux Forge → Quasar Plasma Crucible
│  ├─ Celestial Reservoir
│  └─ Event Horizon Wall → Large → Colossal
├─ Item distribution: Solar Conveyor → Supernova Conveyor → Void Conveyor
│  ├─ Cosmic Router → Cosmic Junction
│  ├─ Celestial Bridge
│  ├─ Interstellar Hyperlane
│  └─ Quasar Mass Driver
│  └─ Stellar Spallation Chamber → Stellar Stack Conveyor → Stellar Stack Router
├─ Cross-planet liquids: Solar Conduit → Nebula Condenser → Astral Catalyzer → Eventide Bioreactor
│                         └→ Ionosphere Reactor → Protostar Extractor → Void Atmospheric Harvester
│                                                             └→ Stellar Petroforge
├─ Serpulo coolant synthesis: Solar Conduit → Nebular Cryoforge
└─ Liquids: Pulsar Liquid Pump → Quasar Liquid Pump
   └─ Solar Conduit → Supernova Conduit → Void Conduit → Voidstream Hyperlane
      ├─ Celestial Aqueduct
      └─ Cosmic Liquid Router → Cosmic Liquid Junction
```

The diagram expresses prerequisite links, not an automatic guarantee that every displayed node can be paid for immediately. Each node preserves its existing `researchCost`.

## Cross-planet material gates

Beryllium and tungsten have Serpulo production through the two Transmuters. Copper, lead, and titanium now have Erekir-only construction and research costs in their reverse-transmutation lane. All nodes needing Erekir-only research items—including oxide or carbide—remain visible but cannot be paid for from the Serpulo-rooted Cosmic tree until the mod adopts a compatible cross-planet research solution. This is an explicit engine limitation, not an unsupported attempt to read Erekir sector inventories.

When adding a new cross-planet crafter, attach it to the earliest Cosmic prerequisite that the player can already research and make its own construction and research costs obtainable before its output is required. Do not move the Cosmic root to Erekir unless the entire tree’s research-cost model is rebalanced for Erekir inventories.

## Technical boundary

Mindustry HJSON creates one `TechNode` per content definition. A `shownPlanets` list cannot create duplicate nodes under both native trees. A truly shared research inventory or two independent native-tree nodes per block would require custom Java logic; this mod intentionally uses the supported HJSON-only standalone-root approach.
