# Vanilla Cosmic — Distribution Balance Guide (Mindustry Build 160.7 target)

This guide defines how Vanilla Cosmic balances item-distribution blocks. It covers construction costs, research costs, progression, and throughput terminology. It does not prescribe a universal speed value: each Mindustry transport class interprets its speed fields differently.

## Core cost rule

- `researchCost` is exactly **5×** each corresponding construction `requirements` entry.
- Preserve a block's existing material list when rebalancing. Adjust counts only when its role, range, capacity, or rated throughput materially changes.
- Start with the appropriate planet/progression resource family. Do not add advanced materials only to make a recipe expensive.
- For an upgraded block with the same role, increase retained-material counts in practical whole-number steps. Add a new advanced material only when it represents a real new capability such as long range, high-capacity stacks, crossing streams, or powered transport.

## Material roles for distribution blocks

| Role | Typical materials | Use in a recipe |
|---|---|---|
| Early transport | copper, lead, graphite | Standard Serpulo belts and basic distribution. |
| Advanced Serpulo transport | titanium, thorium, silicon, plastanium, phase fabric | Faster belts, longer unpowered links, or refined transport behavior. |
| Advanced Erekir transport | beryllium, tungsten, carbide, oxide | High-end Erekir belts and routing. |
| Premium conductive / cosmic component | surge alloy | High-throughput powered stack transport and exceptional distribution capabilities. |

## Throughput terminology

Do not copy a numeric speed from one transport class to another.

| Block class | Important fields | How to compare throughput |
|---|---|---|
| `Conveyor` | `speed`, `displayedSpeed` | Use `displayedSpeed` as the in-game comparison value; conveyor movement is not linearly represented by the raw `speed`. |
| `Router` | `speed` | It transfers one item per update interval. Whole-frame updates mean its practical rate is quantized. |
| `Junction` | `speed`, `capacity`, `displayedSpeed` | `speed` is the crossing delay; capacity is per side. Use `displayedSpeed` for the shown comparison, then account for the per-side path. |
| `BufferedItemBridge` | `range`, `speed`, `bufferCapacity`, `displayedSpeed` | `speed` is buffer travel delay. The native dispatch gate also caps unboosted output, so it cannot be inferred from `speed` alone. |
| `ItemBridge` | `range`, `transportTime` | Approximate rated rate with `60 / transportTime`. Range affects placement, not this rate. |
| `StackConveyor` | `itemCapacity`, `speed` | Rated rate is `itemCapacity × speed × 60`; actual movement also follows native stack-line efficiency behavior. |
| `StackRouter` | `itemCapacity`, `speed` | It fills one item type, waits for its charge delay, then unloads the full stack in a burst. `speed` is a charge delay, not an items-per-second value. |
| `MassDriver` | `range`, `reload`, `itemCapacity`, `minDistribute`, `bulletSpeed`, `bulletLifetime` | Shipment size is limited by source/target item capacity. Rated maximum shipment rate is `itemCapacity × 60 / reload`; projectile travel must fit within its lifetime. |
| `LaunchPad` / `LandingPad` | `itemCapacity`, `launchTime`, `arrivalDuration`, `cooldownTime` | Campaign-only sector logistics. A Launch Pad launches only when full. A Landing Pad's underlying import rate remains the limiting source; its capacity sets batch size and arrival/cooldown set its maximum receive cadence. |

## Current distribution progression

This table records the current definitions. It is a cost and progression reference, not a request to change their speed, range, or research parent.

| Block | Class | Primary capability                          | Construction materials | Research parent |
|---|---|---------------------------------------------|---|---|
| Solar Conveyor | `Conveyor` | Early high-speed belt                       | copper 20, lead 20, graphite 10 | `titanium-conveyor` |
| Supernova Conveyor | `Conveyor` | Advanced Serpulo belt                       | titanium 20, thorium 20, silicon 10 | `solar-conveyor` |
| Void Conveyor | `Conveyor` | Advanced Erekir belt                        | beryllium 30, tungsten 20, carbide 10 | `supernova-conveyor` |
| Cosmic Router | `Router` | Fast distribution                           | beryllium 35, tungsten 25, carbide 12 | `void-conveyor` |
| Cosmic Junction | `Junction` | Fast non-mixing crossings                   | beryllium 40, tungsten 30, carbide 15 | `void-conveyor` |
| Celestial Bridge | `BufferedItemBridge` | Long unpowered item link, 25-item buffer    | titanium 30, thorium 30, silicon 15, graphite 10 | `supernova-conveyor` |
| Interstellar Hyperlane | `ItemBridge` | Long powered item link, 50-item buffer      | beryllium 50, tungsten 40, carbide 25, phase fabric 30, silicon 40 | `void-conveyor` |
| Stellar Stack Conveyor | `StackConveyor` | 25-item stacks / 100 rated items per second | beryllium 100, tungsten 80, carbide 40, surge alloy 40 | `surge-conveyor` |
| Stellar Stack Router | `StackRouter` | 25-item burst distribution | beryllium 110, tungsten 90, carbide 45, surge alloy 50 | `stellar-stack-conveyor` |
| Quasar Mass Driver | `MassDriver` | 250-item long-range shipment | titanium 300, silicon 200, lead 300, thorium 150, plastanium 100, phase fabric 80, surge alloy 50 | `mass-driver` |
| Astral Departure Nexus | `LaunchPad` | 200-item mixed campaign export every 10 seconds | copper 500, lead 450, silicon 350, titanium 350, thorium 250, plastanium 150, phase fabric 75, surge alloy 40, beryllium 100, tungsten 75 | `celestial-bridge` |
| Astral Arrival Nexus | `LandingPad` | 400-item one-type campaign import per eligible landing | copper 550, lead 500, silicon 400, titanium 450, thorium 300, plastanium 200, phase fabric 100, surge alloy 60, beryllium 125, tungsten 100, carbide 40 | `astral-departure-nexus` |

## Campaign logistics pads

Campaign pads are a separate transport family. They use native campaign export/import statistics rather than a local link, projectile, or conveyor path.

- `Astral Departure Nexus` launches only when its 200-item inventory is full. Its 600-tick interval gives a nominal full-batch export capacity of **20 items/s**. It continuously consumes 720 power/s and 6 cryofluid/s during the charge interval; its 120-liquid buffer covers 20 seconds of cryofluid use.
- `Astral Arrival Nexus` accepts no local input and is configured for one item type. When campaign imports for that item have accumulated and water is available, it receives a full 400-item batch. Its required 180-tick arrival plus 300-tick cooldown caps repeated full deliveries at **400 / 8 = 50 items/s**; lower campaign export rates remain lower.
- The Arrival Nexus consumes 6,000 water per landing (15 water per delivered item) and stores exactly one full landing's cost. Its delivery batch does not create items or raise the campaign import rate.
- Both pads retain Serpulo-rooted research while using a mixed material family: their beryllium and tungsten are available from the Stellar Spallation Chamber and Pulsar Nucleoforge, and Arrival's carbide is available from the Accretion Carbothermal Forge. `shownPlanets` exposes them to both planet scopes after unlock, but native campaign pads only operate where that planet's campaign rules and compatible sector import/export state are available.

## When adding a distribution block

1. Choose the native transport class first; it determines behavior and which fields control throughput.
2. Compare output rate only with the same class formula or the block's displayed in-game rate.
3. Select construction materials from the relevant planet and technology tier.
4. Scale costs for the new capability—range, per-path crossing capacity, routing, stack capacity, or power requirement—not merely for a larger raw speed number.
5. Set each research material to exactly 5× its construction amount.
6. Keep the research parent reachable before the new recipe's materials are expected to be used.
