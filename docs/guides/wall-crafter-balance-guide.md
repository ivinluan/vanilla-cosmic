# Vanilla Cosmic — Wall Crafter Balance (Mindustry Build 160.7 target)

## Role and vanilla baseline

Titanium Rift Fracturer is a `WallCrafter` that turns compatible sand-attributed solid walls into titanium. Its vanilla predecessor is the Cliff Crusher: a size 2 `WallCrafter` with `attribute: sand`, `output: sand`, `drillTime: 110`, `power: 11 / 60`, and `fogRadius: 2`.

The requested goal is **300% more actual output** than Cliff Crusher (4× its item rate). `WallCrafter` output depends on both `drillTime` and the block's size, because every valid wall tile along its facing edge contributes efficiency.

```
output rate = 60 / drillTime × size × valid-wall efficiency
```

Cliff Crusher's unboosted, full-frontage rate is `60 / 110 × 2 = 1.09` items/second. The Fracturer's target is `1.09 × 4 = 4.36` items/second. At size 4, `drillTime: 55` gives `60 / 55 × 4 = 4.36` items/second, exactly 300% higher.

## Balanced definition

| Stat | Cliff Crusher | Titanium Rift Fracturer | Rationale |
|---|---:|---:|---|
| Size | 2 | 4 | Existing Fracturer footprint retained. |
| `drillTime` | 110 | 55 | Produces the requested 300% actual-rate increase after its larger frontage is included. |
| Output | sand | titanium | Fixed titanium output from a compatible wall; it does not read the wall's mineral. |
| Power | 0.183 | 0.75 | Roughly 4× the vanilla power draw, rounded, matching the rate increase. |
| Optional water boost | none | 0.24 liquid/tick, 1.6× speed | Uses the stock `WallCrafter` liquid-booster behavior; water is optional, not a required operating input. |
| Item / liquid capacity | default / none | 50 / 60 | A practical output buffer and water reserve for the size 4 block. |
| Fog radius | 2 | 4 | Larger footprint and upgraded visibility. |
| Health / armor | engine default / 0 | 700 / 60 | Custom Cosmic durability without the former excessive values. |
| Build multiplier | default | 1.0 | Normal construction speed. |

## Construction and research costs

The existing material list is retained, and titanium is added as the new structural ore. Per the shared new-material rule, structural ore starts at 50% of copper: `150 × 0.5 = 75`, rounded up to 80. Requirements are therefore copper 150, lead 150, graphite 60, silicon 40, plastanium 40, phase fabric 40, and titanium 80. Every research-cost entry is exactly 5× its own requirement, so titanium research costs 400.

## Rift Fracturer progression

All four Fracturers remain size 4 and use the stock `WallCrafter` water-booster behavior. Each has a fixed output; output value does not change its item-rate calculation.

| Block | Output | `drillTime` | Full-frontage rate | Power | Water / boost | Health / armor | Research parent |
|---|---|---:|---:|---:|---|---|---|
| Titanium Rift Fracturer | titanium | 55 | 4.36/s | 0.75 | 0.24/tick, 1.6× | 700 / 60 | `cliff-crusher` |
| Thorium Rift Fracturer | thorium | 60 | 4.00/s | 0.85 | 0.24/tick, 1.6× | 760 / 70 | `blast-drill` |
| Beryllium Rift Fracturer | beryllium | 55 | 4.36/s | 0.90 | 0.26/tick, 1.6× | 820 / 80 | `cliff-crusher` |
| Tungsten Rift Fracturer | tungsten | 50 | 4.80/s | 1.10 | 0.28/tick, 1.6× | 900 / 100 | `large-cliff-crusher` |

Thorium branches from the Serpulo `blast-drill`, which already requires access to thorium mining. Beryllium branches from the Erekir `cliff-crusher`, keeping its recipe to early Erekir beryllium, graphite, and silicon. Tungsten branches from the later Erekir `large-cliff-crusher`, so its tungsten and advanced processing requirements are obtainable before it can be researched. Every new Fracturer uses the universal 5× research-cost rule.

## What `attribute: sand` and `output: titanium` mean

`WallCrafter` is not limited to sand: a definition may use another supported wall attribute and set another fixed output item. However, `attribute: sand` checks the sand attribute of the solid wall tiles immediately in front of the block, while `output: titanium` is a fixed item field. Therefore this Fracturer **always produces titanium**; it does not inspect a wall ore and cannot require or detect a titanium-bearing wall through HJSON configuration alone.

For a block that mines the item present in each wall, use `BeamDrill`. Vanilla's `BeamDrill` reads each wall's `wallDrop()` and adds that drop to its inventory, subject to the drill's ore `tier`. This is the behavior used by the Cosmic bore line.

For a general wall processor that changes output based on an arbitrary wall's attribute or type, create a custom Java block extending `WallCrafter` (or a purpose-built `Block`) and custom `Building`. It must choose the current output from the faced wall, store multiple item types safely, and update its placement/UI logic. That dynamic output behavior is not provided by the stock `WallCrafter` HJSON fields.

## Output and wall compatibility

The `output` field accepts an item content reference, so a `WallCrafter` can technically be configured to output any item registered by the game or this mod. The output is not linked to the wall: `attribute: sand` with `output: copper` would create copper from a sand-attributed wall.

Stock `WallCrafter` can only latch onto **solid** walls with a positive value for its configured attribute. In vanilla content, the practical production attribute for solid walls is `sand`. The compatible sand-attributed walls are stone wall, dune wall, regolith wall, yellow stone wall, rhyolite wall, carbon wall, ferric stone wall, beryllic stone wall, red stone wall, and sand wall. Their attribute strengths differ, so they do not all run at the same speed.

The vanilla attributes are heat, spores, water, oil, light, sand, and steam. Vanilla also assigns water, oil, spores, and steam to floors or vents, but the stock `WallCrafter` scans only solid walls; those attributes therefore do not create additional practical vanilla wall targets. A mod can add a custom solid wall with a positive attribute value, then target it with a fixed-output `WallCrafter`.

Use a `BeamDrill` when the desired result is the actual wall ore (copper, lead, titanium, and so on). Its output follows the mined wall's `wallDrop`, rather than a fixed `output` field.

### Valid vanilla `attribute` values

```hjson
heat
spores
water
oil
light
sand
steam
```

These are attributes, not wall names. In unmodified vanilla content, `sand` is the practical choice for a `WallCrafter`, because the stock production logic requires a solid wall with a positive matching attribute.

### Valid vanilla `output` item values

```hjson
scrap
copper
lead
graphite
coal
titanium
thorium
silicon
plastanium
phase-fabric
surge-alloy
spore-pod
sand
blast-compound
pyratite
metaglass
beryllium
tungsten
oxide
carbide
fissile-matter
dormant-cyst
```

`fissile-matter` and `dormant-cyst` are hidden/internal vanilla items, so do not use them as normal production outputs. This mod currently defines no custom items. Other loaded mods can add additional valid item names.

## Validation checklist

- `attribute: sand` and `output: titanium` remain intentionally fixed.
- The full-frontage output calculation is exactly 300% above Cliff Crusher.
- Each research-cost entry is 5× the matching construction requirement.
- The custom block uses only existing `WallCrafter` HJSON fields.
