# Vanilla Cosmic — Wall Crafter Balance (Mindustry Build 160.7 target)

## Role and vanilla baseline

Cosmic Rift Shatterer is a `WallCrafter` that turns sand-bearing solid walls into sand. Its vanilla predecessor is the Cliff Crusher: a size 2 `WallCrafter` with `attribute: sand`, `output: sand`, `drillTime: 110`, `power: 11 / 60`, and `fogRadius: 2`.

The requested goal is **200% more actual output** than Cliff Crusher. `WallCrafter` output depends on both `drillTime` and the block's size, because every valid wall tile along its facing edge contributes efficiency.

```
output rate = 60 / drillTime × size × valid-wall efficiency
```

Cliff Crusher's unboosted, full-frontage rate is `60 / 110 × 2 = 1.09` items/second. The Shatterer's target is `1.09 × 3 = 3.27` items/second. At size 4, `drillTime: 73` gives `60 / 73 × 4 = 3.29` items/second, which is 201.4% higher after whole-frame rounding.

## Balanced definition

| Stat | Cliff Crusher | Cosmic Rift Shatterer | Rationale |
|---|---:|---:|---|
| Size | 2 | 4 | Existing Shatterer footprint retained. |
| `drillTime` | 110 | 73 | Produces the requested 200% actual-rate increase after its larger frontage is included. |
| Power | 0.183 | 0.55 | 3× the vanilla power draw, rounded. |
| Item capacity | default | 20 | Practical buffer for a size 4 wall processor. |
| Fog radius | 2 | 4 | Larger footprint and upgraded visibility. |
| Health / armor | engine default / 0 | 700 / 60 | Custom Cosmic durability without the former excessive values. |
| Build multiplier | default | 1.0 | Normal construction speed. |

## Construction and research costs

The existing material list is retained. The requirement amounts use the shared baseline plus the drill guide's processed-material entry rule: copper 150, lead 150, graphite 60, silicon 40, plastanium 40, and phase fabric 40. Every research-cost entry is exactly 5× its own requirement.

## What `attribute: sand` and `output: sand` mean

`WallCrafter` is not limited to sand: a definition may use another supported wall attribute and set another fixed output item. However, `attribute: sand` checks the sand attribute of the solid wall tiles immediately in front of the block, while `output: sand` is a fixed item field. Therefore this Shatterer definition **always produces sand**; it does not inspect a wall ore and cannot produce copper, lead, or another ore through HJSON configuration alone.

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

- `attribute: sand` and `output: sand` remain intentionally fixed.
- The full-frontage output calculation is approximately 200% above Cliff Crusher.
- Each research-cost entry is 5× the matching construction requirement.
- The custom block uses only existing `WallCrafter` HJSON fields.
