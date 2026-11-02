# Vanilla Cosmic — Campaign Logistics Balance Guide (Mindustry v160.7)

This guide covers campaign-sector launch and landing pads. They are not local item bridges: native campaign export statistics determine what can arrive, while pad capacity and timers determine batch behavior.

## Native mechanics

- A `LaunchPad` launches only when full. It records its payload as an export to the selected destination sector after the native launch payload finishes.
- `acceptMultipleItems: true` allows a launch payload containing every stored item type. It does not change the destination-selection rules.
- A `LandingPad` is configured for one receiving item type. An eligible landing fills the entire `itemCapacity`; it neither creates items nor raises the export rate supplied by other sectors.
- Launch destination selection is same-planet only. Landing imports are only available in campaign sectors using the modern, non-legacy launch-pad rules.
- `shownPlanets` is content visibility/applicability metadata. It does not create a second technology node or add campaign-sector logistics to a planet that lacks the relevant native campaign state.

## Timing model

Use ticks at 60 per second.

| Measure | Formula | Meaning |
|---|---|---|
| Launch capacity | `itemCapacity / launchTime × 60` | Maximum full-batch export rate, assuming the pad can remain full. |
| Arrival cycle capacity | `itemCapacity / (arrivalDuration + cooldownTime) × 60` | Maximum repeated landing rate, assuming campaign imports accumulate this quickly. |
| Actual import rate | `min(campaign import rate, arrival cycle capacity)` | A Landing Pad cannot exceed exports available from other sectors. |

## Astral Logistics Pad targets

| Block | Capacity | Timing | Maximum full-batch rate | Operating cost |
|---|---:|---|---:|---|
| Astral Departure Nexus | 200 items | 600 ticks / 10 s launch interval | 20 items/s | 720 power/s and 6 cryofluid/s while charging |
| Astral Arrival Nexus | 400 items | 180 ticks / 3 s landing + 300 ticks / 5 s cooldown | 50 items/s | 6,000 water per 400-item landing (15 water/item) |

The Arrival Nexus uses a 400-item batch over an 8-second total cycle. This gives it a 50-item/s maximum cadence—2.5 times vanilla Landing Pad's 100-item / 5-second maximum—while campaign export availability remains the hard limit.

## Cost and research rules

- Use copper and lead as the structural base; add silicon for campaign-control hardware, titanium and thorium for a high-tier chassis, and modest plastanium, phase fabric, and surge alloy for exceptional capability.
- Keep every material researchable from the standalone Serpulo-rooted Cosmic tree. Beryllium and tungsten are appropriate only after the Stellar Spallation Chamber and Pulsar Nucleoforge; carbide is appropriate only after the Accretion Carbothermal Forge. Do not add an Erekir material merely because the block is visible on Erekir.
- Research cost is exactly five times every construction requirement. This applies to both pads.
- Treat a 5×5 campaign logistics pad as heavy infrastructure for durability. Use the documented Mass Driver role multipliers (`RH 1.25`, `RA 1.20`) until the durability guide gains a dedicated campaign-logistics role.

## Sprite and liquid conventions

- A Launch Pad can draw an optional `-light` region, an optional `-pod` with a `launchpod` fallback, and an optional `-preview` with a main-region fallback.
- A Landing Pad can draw an optional `-pod` with an `advanced-launch-pad-pod` fallback. It uses the normal main block region and native water fill drawing.
- Keep a departure liquid buffer at least one full launch interval plus reasonable connection slack. The Astral Departure Nexus holds 120 cryofluid, covering 20 seconds at 6/s.
- Landing Pad liquid capacity must hold one complete landing cost. The Astral Arrival Nexus holds exactly its 6,000-water cost.

## Validation checklist

1. Confirm the launch pad only starts at full capacity and supports a mixed payload.
2. Confirm a landing pad has one selected item type and fills its full configured capacity.
3. Verify source-sector exports, destination selection, and arrival behavior in a non-legacy campaign sector.
4. Verify no interplanetary claim or custom transport bypass was introduced.
5. Check Erekir's actual campaign availability separately; visibility alone is not proof of sector logistics support.
