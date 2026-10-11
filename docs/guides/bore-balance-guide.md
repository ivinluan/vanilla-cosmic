# Vanilla Cosmic — Beam Bore Balance (Mindustry Build 160.7 target)

This guide balances the Cosmic beam-bore line in this order: Cosmic Prism Bore, Cosmic Radiant Bore, Cosmic Spectral Bore, then Cosmic Photon Bore.

## Vanilla baseline

Vanilla's Large Plasma Bore is the predecessor and research parent of Cosmic Prism Bore. In the current official source it is a size 3, tier 5 `BeamDrill` with `drillTime: 100`, `range: 6`, `fogRadius: 4`, `laserWidth: 0.7`, `itemCapacity: 20`, and `power: 0.8`. Its hydrogen and nitrogen consumption establish that powered beam bores can have normal and boost liquid consumers.

## Progression rules

- Prism starts at a 25% drilling-throughput improvement over Large Plasma Bore: `drillTime` is 80 frames instead of 100.
- Each subsequent Cosmic bore is 25% faster than its predecessor. Because a lower `drillTime` is faster, calculate it as `previous drillTime / 1.25`, then round to a whole frame.
- Health, armor, item capacity, liquid capacity, power consumption, neoplasm boost consumption, boost intensity, range, fog radius, and laser width each increase by approximately 25% per bore. Values are rounded to useful whole units or two decimal places.
- `tier: 5` is preserved across the line so every bore matches the Large Plasma Bore's maximum ore hardness.
- `buildCostMultiplier` is `1.0` for normal construction speed.
- The existing requirement material lists are preserved. For every material retained by the next bore, construction requirements increase by 25%, rounded to a practical multiple of 5. This applies to the shared copper, lead, graphite, silicon, titanium, and thorium entries.
- When a bore introduces a new material, use an entry amount appropriate to its material class, then apply the same 25% progression while that material remains in the line. The current entry points are 100 titanium and 50 phase fabric for Radiant, 125 thorium and 15 surge alloy for Spectral, and 160 beryllium and 20 carbide for Photon.
- Every `researchCost` item is exactly 5× its own construction requirement. This applies to copper and lead as well as every other material.
- Visual-only fields such as colors, sparks, and spread are not balance statistics and remain part of each bore's identity.

## Balancing table

| Bore | Size | Drill time | Health | Armor | Item cap | Liquid cap | Power | Neoplasm boost | Boost | Range | Fog | Laser width |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Large Plasma Bore (vanilla baseline) | 3 | 100 | — | — | 20 | — | 0.8 | 0.05 nitrogen | default | 6 | 4 | 0.7 |
| Cosmic Prism Bore | 2 | 80 | 400 | 120 | 70 | 30 | 0.8 | 0.13 | 1.50 | 6 | 5 | 0.75 |
| Cosmic Radiant Bore | 3 | 64 | 500 | 150 | 90 | 40 | 1.00 | 0.16 | 1.88 | 8 | 6 | 0.95 |
| Cosmic Spectral Bore | 4 | 51 | 630 | 190 | 110 | 50 | 1.25 | 0.20 | 2.35 | 10 | 8 | 1.20 |
| Cosmic Photon Bore | 5 | 41 | 790 | 240 | 140 | 60 | 1.56 | 0.25 | 2.94 | 13 | 10 | 1.50 |

`drillTime` is the key throughput value. Wall ore availability, hardness, range, and active liquid boosting still affect practical output, so validate the sequence in-game on the same wall ore before finalizing release balance.

## Construction requirement progression

| Bore | Copper | Lead | Graphite | Silicon | Titanium | Thorium | Other materials |
|---|---:|---:|---:|---:|---:|---:|---|
| Cosmic Prism Bore | 150 | 150 | 60 | 40 | — | — | plastanium 40 |
| Cosmic Radiant Bore | 190 | 190 | 75 | 50 | 100 | — | phase fabric 50 |
| Cosmic Spectral Bore | 240 | 240 | 95 | 65 | 125 | 125 | surge alloy 15 |
| Cosmic Photon Bore | 300 | 300 | 120 | 80 | 160 | 160 | beryllium 160; carbide 20 |

## New-material entry rule

When an item appears for the first time in a bore recipe, calculate it from that bore's copper requirement. Do not reset the retained material counts.

| Material role | Starting requirement | Examples |
|---|---|---|
| Core/common | Continue from the prior bore at 1.25×, rounded to 5 | copper, lead, graphite, silicon |
| Structural ore | About 50% of copper, rounded to 5 | titanium, thorium, beryllium |
| Processed advanced material | About 25% of copper, rounded to 5 | plastanium, phase fabric |
| Rare alloy/catalyst | 5–10% of copper, rounded to 5, minimum 5 | surge alloy, carbide |

If an item is removed and later used again, it starts over under this rule rather than inheriting the amount from its earlier appearance. Once an item remains in two adjacent recipes, increase it by the normal 25% step.

## Validation checklist

- Each bore's research parent matches the previous bore; Prism follows `large-plasma-bore`.
- Each bore uses only supported `BeamDrill` fields already present in the project definitions.
- Each research-cost entry equals 5× the matching requirement entry.
- No construction ingredient was added, removed, or substituted during balancing.
