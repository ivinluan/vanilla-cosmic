# Vanilla Cosmic — Drill Rebalance (Mindustry Build 160.7 target)

This archive contains proposed balanced replacements for the 16 original HJSON drill definitions from the supplied `drills.zip`, plus the two later Pro variants. It does not modify the live GitHub repository.

## Rules used

- Standard drill health: 400 at tier I, then +80 per tier.
- Standard drill armor: 120 per tier; mini armor is exactly 50%.
- Standard item capacity: 60 at tier I, then +20 per tier; mini capacity is exactly 50%.
- Standard liquid capacity: 20 at tier I, then +10 per tier; mini capacity is exactly 50%.
- Mini health, armor, item/liquid capacity, build requirements, and research costs are exactly half of the matching standard drill.
- Water consumption and liquid boost intensity are identical for matching standard/mini tiers, per the requested rule. Water use increases gently by tier.
- `researchCost` is 5 times the corresponding construction `requirements`, per material; this gives a repeatable cost formula.
- `buildCostMultiplier` is standardized to `1.0` (normal construction speed) instead of `0.25` / `0.23`. In Mindustry this is a build-speed multiplier, separate from the item counts in `requirements`; the previous values made construction slower than normal.
- `drillTime` is in frames. Lower values drill faster; actual output also depends on ore hardness, ore coverage under the footprint, and liquid boosting. The mini times are independently tuned rather than simply halving standard times.
- Existing research links, tier limits, effects, and mini `drawMineItem: false` settings are preserved. Two inaccurate descriptions are corrected.
- Table tiers describe the Cosmic upgrade sequence. The HJSON `tier` field is the maximum ore hardness and remains `5` for the Pro variants.

## Balancing table

| Tier | Standard drillTime | Mini drillTime | Standard health | Mini health | Standard armor | Mini armor | Standard item cap | Mini item cap | Standard liquid cap | Mini liquid cap | Water amount (both) | Boost intensity (both) |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| I | 300 | 220 | 400 | 200 | 120 | 60 | 60 | 30 | 20 | 10 | 0.08 | 1.65 |
| II | 240 | 175 | 480 | 240 | 240 | 120 | 80 | 40 | 30 | 15 | 0.1 | 1.75 |
| III | 190 | 140 | 560 | 280 | 360 | 180 | 100 | 50 | 40 | 20 | 0.12 | 1.85 |
| IV | 150 | 110 | 640 | 320 | 480 | 240 | 120 | 60 | 50 | 25 | 0.14 | 1.95 |
| V | 120 | 90 | 720 | 360 | 600 | 300 | 140 | 70 | 60 | 30 | 0.16 | 2.05 |
| VI | 100 | 75 | 800 | 400 | 720 | 360 | 160 | 80 | 70 | 35 | 0.18 | 2.15 |
| VII | 80 | 60 | 880 | 440 | 840 | 420 | 180 | 90 | 80 | 40 | 0.2 | 2.25 |
| VIII | 65 | 50 | 960 | 480 | 960 | 480 | 200 | 100 | 90 | 45 | 0.22 | 2.35 |
| IX — Cosmic Drill Pro | 52 | — | 1040 | — | 1080 | — | 220 | — | 100 | — | 0.24 cryofluid | 2.45 |
| X — Cosmic Drill Pro Max | 42 | — | 1120 | — | 1200 | — | 240 | — | 110 | — | 0.26 cryofluid | 2.55 |

## Cost formula

### Construction requirements

- Preserve the block's existing ingredient list. Balance changes adjust counts only; they do not add, remove, or substitute materials.
- Within a footprint class, increase the count of every retained material from one Cosmic tier to the next. Keep counts rounded to practical multiples of 10 where possible.
- When a larger footprint begins a new tier, scale the prior standard drill's shared-material counts for the larger footprint, then round to practical tens. Cosmic Drill Pro (size 3) uses approximately 1.5× Cosmic VIII's shared-material costs. Cosmic Drill Pro Max (size 4) uses approximately 1.45× Pro's shared-material costs. A material that is not shared keeps its established recipe quantity.
- A mini drill's construction requirement for every material is exactly 50% of its matching standard drill's requirement. Therefore, choose even standard counts whenever a new standard/mini pair is added.

### New-material entry rule

Use this rule only when a material appears for the first time in a drill's recipe. Do not reset the other existing requirements.

| Material role | Starting requirement on a standard drill | Examples |
|---|---|---|
| Core/common | Continue the prior tier's count, or apply the footprint multiplier | copper, lead, graphite, silicon |
| Structural ore | 50% of that tier's copper requirement, rounded up to 10 | titanium, thorium, beryllium, tungsten |
| Processed advanced material | 25% of that tier's copper requirement, rounded up to 10 | plastanium, phase fabric |
| Rare alloy/catalyst | 5–10% of that tier's copper requirement, rounded up to 10, with a minimum of 10 | surge alloy, carbide |

After introduction, a material that remains in the next tier increases with the normal standard-drill progression. If it disappears from a recipe and returns later, treat the return as a new introduction and use this table again. The matching mini always uses exactly half of the resulting standard amount; do not create a separate mini baseline.

### Research costs

For every block and every material in its `requirements` list, set `researchCost` to exactly 5× that block's construction requirement. Do not use a separate multiplier for copper, lead, or any other individual material. This applies to standards, minis, Pro, and Pro Max.

The Pro and Pro Max drills retain their existing ingredient lists, size, ore tier, power use, cryofluid coolant, research chain, and effects. Their requirement counts and research costs follow the established IX/X size progression and universal 5× rule.

## Vanilla Pneumatic Drill baseline

The official Mindustry `Blocks.java` definition sets the Pneumatic Drill to size 2, tier 3, `drillTime: 400`, and boosted water consumption of `3.5 / 60` liquid units per tick. Cosmic I therefore starts faster than the vanilla Pneumatic Drill, and each later tier improves gradually. This is a balancing proposal; test in-game throughput on the same ore and with/without water before treating the numerical output as benchmarked.

## Validation notes

- HJSON files were updated by scripted field replacement and consistency checks.
- Every mini health/armor/itemCapacity/liquidCapacity/build requirement/research cost was checked against exactly half of its standard counterpart.
- Each standard/mini pair has the same water amount and boost intensity.
- Cosmic Drill Pro and Cosmic Drill Pro Max are documented as tiers IX and X. They have no mini variants; their cryofluid amounts and boost intensities continue the standard-tier progression.
- Pro and Pro Max retain their material lists and established progressively larger construction requirements. Every one of their research-cost entries is 5× its own requirement.
- This archive has not been launched in Mindustry here; runtime validation remains for the user to perform.
