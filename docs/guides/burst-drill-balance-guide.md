# Vanilla Cosmic — Burst Drill Balance (Mindustry Build 160.7 target)

## Vanilla baseline and output formula

Vanilla Blast Drill is a size 4, tier 5 `Drill` with `drillTime: 280`, `itemCapacity: 20`, `power: 3`, and optional water boosting. A normal drill's unboosted full-coverage output is determined by every dominant ore tile under its footprint:

```
output rate = 60 / drillTime × size²
```

Blast Drill's full-coverage rate is `60 / 280 × 4² = 3.43` items/second. The Supernova Drill target is 500% higher actual output, or six times the Blast Drill rate: `3.43 × 6 = 20.57` items/second. Supernova is size 8, so `drillTime: 187` gives `60 / 187 × 8² = 20.53` items/second: 498.9% higher after whole-frame rounding.

## Supernova Drill balance

| Stat | Blast Drill | Supernova Drill | Rationale |
|---|---:|---:|---|
| Size | 4 | 8 | Existing Supernova footprint retained. |
| Ore tier | 5 | 7 | Retains access to the highest vanilla ore hardness. |
| `drillTime` | 280 | 187 | Six times the actual full-coverage output after the 8×8 footprint is included. |
| Power | 3 | 18 | Six times the vanilla demand, matching the output target. |
| Coolant | water 0.10 | cryofluid 0.60 + slag 0.60 | Existing two-liquid identity retained at a proportional sustained cost. Neither liquid is a speed booster. |
| Health / armor | engine default / 0 | 3200 / 240 | Size-appropriate endgame durability without the former excessive armor. |
| Item capacity | 20 | 640 | Ten full 8×8 production bursts. |
| Liquid capacity | default | 720 | Ten seconds of the combined mandatory coolant demand. |
| Build multiplier | default | 1.0 | Normal construction speed. |

## Construction and research costs

Requirements retain Supernova's existing material list while using the established new-material entry rules. The common materials scale to the six-times-output target; plastanium and phase fabric use roughly 25% of copper, and surge alloy uses roughly 10%. Every `researchCost` entry is exactly 5× the matching construction requirement.

| Material | Requirement | Research cost |
|---|---:|---:|
| Copper | 390 | 1950 |
| Lead | 390 | 1950 |
| Graphite | 180 | 900 |
| Silicon | 360 | 1800 |
| Plastanium | 100 | 500 |
| Phase fabric | 100 | 500 |
| Surge alloy | 40 | 200 |

## Burst Drill implementation notes

`BurstDrill` inherits normal ore selection from `Drill` but emits all dominant ore tiles at once when its charge reaches `drillTime`. It uses the same full-coverage `size²` throughput calculation for its average output. Its burst effect, arrows, glow, shake, and `invertedTime` are presentation settings; they do not increase item output.

## Validation checklist

- Full-coverage unboosted output is approximately 500% higher than Blast Drill.
- `drillTime` has an inline HJSON comment with the calculated items-per-second rate.
- Every research-cost item equals 5× its construction requirement.
- Both required liquids are preserved as cooling costs, not marked as optional speed boosters.
