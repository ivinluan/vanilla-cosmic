# Vanilla Cosmic — Block Durability Balance Guide (Mindustry v160.7)

This is a design-time system for choosing supported HJSON `health` and `armor` values. It does not run inside Mindustry, and this guide does not change any block values. The accompanying [audit](block-durability-audit.md) records the current and proposed values for every current Vanilla Cosmic block.

## Principles

- Durability follows footprint, material composition, and gameplay role—not raw recipe size or progression alone.
- Construction requirements determine the calculation. Research requirements never affect durability.
- Armor is damage reduction, not an extra health pool. Keep it modest on ordinary production and logistics blocks.
- The formula is a repeatable starting point. Existing deliberate overrides, enemy damage, and vanilla comparisons may require a documented exception.

## Verified vanilla behavior

- With no explicit `health`, `Block.init()` starts at 40 scaled health per tile, adds each requirement item's native `healthScaling` once, then multiplies by `size²` and rounds to the nearest 5. Requirement **quantities do not matter** to this native calculation.
- An explicit HJSON `health` replaces that calculation. `armor` defaults to 0 and may be set independently.
- On a direct bullet collision, armor reduces incoming building damage by the effective armor amount. The result is `max(damage − armor, damage × 0.10)`, so at least 10% of the original direct-hit damage remains. A bullet with `pierceArmor` skips this reduction; its armor multipliers can also alter the effective armor.
- Standard splash/tile damage reaches buildings through direct damage calls and does not use the direct-collision armor reduction path. Armor therefore is not universal protection against explosions or scripted direct damage.
- No class in this mod's current inventory changes those base health or armor rules. Block classes differ in function and footprint, which this guide handles through role multipliers.

## Vanilla Cosmic health formula

```text
HP = roundNearest10(40 × S² × MH × RH)
MH = 1 + Σ((quantityᵢ / total construction quantity) × healthCoefficientᵢ)
```

`S` is the block size, `MH` is weighted material health, and `RH` is the role multiplier. Use only construction requirements. The quantity weights are a Vanilla Cosmic custom rule and are intentionally different from vanilla native `healthScaling`.

## Vanilla Cosmic armor formula

```text
Armor = roundNearestWhole((2 + 1.5 × S) × MA × RA)
MA = Σ((quantityᵢ / total construction quantity) × armorCoefficientᵢ)
```

`MA` is weighted material armor and `RA` is the role multiplier. Clamp the final result to 0 or greater.

## Material coefficients

| Material | Health | Armor |
|---|---:|---:|
| Copper | 0.00 | 0.0 |
| Lead | 0.05 | 0.2 |
| Graphite | 0.08 | 0.1 |
| Silicon | 0.10 | 0.0 |
| Metaglass | 0.10 | 0.0 |
| Titanium | 0.30 | 0.8 |
| Thorium | 0.45 | 1.0 |
| Plastanium | 0.35 | 0.7 |
| Phase Fabric | 0.50 | 0.5 |
| Surge Alloy | 0.70 | 1.5 |
| Beryllium | 0.15 | 0.3 |
| Tungsten | 0.40 | 1.2 |
| Oxide | 0.45 | 0.7 |
| Carbide | 0.65 | 1.4 |

All construction items currently used by Vanilla Cosmic have coefficients. No missing-material default has been silently applied. Add a coefficient and record its rationale before using a new construction item.

## Role multipliers

| Actual role | Health `RH` | Armor `RA` |
|---|---:|---:|
| Conveyors, routers, junctions | 0.85 | 0.50 |
| Item bridges, stack transport | 0.95 | 0.70 |
| Pumps and liquid transport | 1.00 | 0.80 |
| Liquid storage | 1.75 | 1.50 |
| Item storage | 1.75 | 1.50 |
| Production / GenericCrafter | 1.10 | 0.90 |
| Drills, beam bores, BurstDrills, WallCrafters | 1.20 | 1.00 |
| Mass drivers | 1.25 | 1.20 |
| Defensive walls | 1.50 | 2.00 |
| Ultimate Defensive Walls | 13.50 | Fixed 40 armor |

The current audit applies the documented Ultimate Defensive Wall exception only to the Event Horizon Wall family. The ordinary Defensive walls multiplier remains available for future standard wall blocks.

### Production / GenericCrafter role

Use this role for stationary item-production machines whose primary function is transforming delivered inputs into a fixed output, including the Stellar Spallation Chamber (`stellar-spallation-chamber`) and Pulsar Nucleoforge (`pulsar-nucleoforge`). It sits below extraction machinery because it has no exposed mining head or wall-facing operating position, but above ordinary logistics because it represents a compact advanced processing structure. Do not apply it to pumps, drills, bores, WallCrafters, or storage blocks merely because they produce or hold a resource.

## Ultimate Defensive Wall exception

Event Horizon Walls are an intentional endgame exception to the ordinary **Defensive walls** role. This does not change the rules for ordinary walls.

- Use the normal material-weighting formulas first and record their result.
- Event Horizon Wall recipes preserve a fixed material ratio: tungsten 50, carbide 20, phase fabric 20, surge alloy 10 per 1×1 footprint. This gives `MH = 1.500` and `MA = 1.130` for every size.
- The ultimate tier uses `RH = 13.50`, exactly nine times the ordinary Defensive walls health role multiplier. With the fixed material ratio, this yields 810 HP at 1×1, 3,240 HP at 2×2, and 12,960 HP at 4×4: each is three times vanilla Carbide Wall health for the same footprint.
- Standard Defensive walls armor would be 8, 11, and 18 for the 1×1, 2×2, and 4×4 variants. The fixed **40 armor** is an explicit ultimate-defense exception, set to twice vanilla Reinforced Surge Wall armor and intentionally shared across sizes following vanilla wall conventions. It is not a result of the normal armor formula.
- Continue to apply the engine limitation: armor reduces ordinary non-piercing direct bullet damage, but it is neither a shield pool nor universal protection against splash/tile damage.

The three Event Horizon recipes and research costs scale exactly by footprint: 1×, 4×, and 16×. Do not apply a capacity, total-cost, or additional size multiplier beyond the formula and this documented tier exception.

### Native ShieldWall configuration

All Event Horizon variants use the same supported native defensive settings: `chanceDeflect = 30`, `flashHit = true`, `lightningChance = 0.15`, `lightningDamage = 90`, `lightningLength = 51`, `absorbLasers = true`, and `insulated = true`. Deflection is damage-dependent (`chanceDeflect / incoming bullet damage`), not a flat 30% chance. Lightning length is the native lightning-segment length, not a guaranteed straight-line tile range.

The shield values scale by footprint: 675 / 2,700 / 10,800 shield health and 1.5 / 6 / 24 regeneration per tick. Each uses the same 400-tick break cooldown. Shields require power: HJSON consumer values of 0.0375 / 0.15 / 0.6 power per tick equal 2.25 / 9 / 36 power per second. Power remains unchanged because shield capacity and regeneration are unchanged. When unpowered, the native shield is unavailable while the physical wall remains.

`emitLight`, `lightColor`, and `lightRadius` provide constant native block light; the colors and `glowColor` are visual settings, not additional shield mechanics.

## Storage Block Durability

Use the **Liquid storage** role for dedicated liquid containers, tanks, and reservoirs. Use the **Item storage** role for dedicated containers, vaults, and warehouses. Both roles use `RH = 1.75` and `RA = 1.50`.

- Classify by primary gameplay function, not Java class alone. A `LiquidRouter` built chiefly to store liquid, such as Celestial Reservoir, is Liquid storage; a router built to distribute liquid remains liquid transport.
- Do not apply the storage roles to pumps, conduits, liquid bridges, liquid junctions, ordinary liquid routers, conveyors, item routers, mass drivers, stack transport, or production buffers.
- Treat cores separately. Their base-infrastructure purpose means the Item storage multiplier does not automatically apply.
- Storage capacity does not add a second health or armor multiplier. The footprint, construction material weights, and storage role multipliers are the complete calculation.
- Do not assign armor solely because capacity is high. Stronger high-tier materials may improve weighted `MH` and `MA`, but no separate storage formula is needed.

### Celestial Reservoir worked example

Celestial Reservoir is 4×4 and requires metaglass ×150, beryllium ×250, tungsten ×200, and carbide ×25: total construction quantity = 625.

```text
Material weights = metaglass 0.24, beryllium 0.40, tungsten 0.32, carbide 0.04
MH = 1 + (0.24 × 0.10) + (0.40 × 0.15) + (0.32 × 0.40) + (0.04 × 0.65) = 1.238
HP = roundNearest10(40 × 4² × 1.238 × 1.75) = roundNearest10(1386.56) = 1390

MA = (0.24 × 0.0) + (0.40 × 0.3) + (0.32 × 1.2) + (0.04 × 1.4) = 0.560
Armor = roundNearestWhole((2 + 1.5 × 4) × 0.560 × 1.50) = roundNearestWhole(6.72) = 7
```

This is more durable than vanilla Reinforced Liquid Tank (900 HP, no explicit armor) because it is a larger dedicated storage block made with high-tier reinforcement materials. Its 13,500 liquid capacity does not independently increase its durability.

## Rounding and exceptions

- For positive health values, use `floor(value / 10 + 0.5) × 10`; an exact halfway value rounds upward.
- For positive armor values, use `floor(value + 0.5)`; an exact halfway value rounds upward.
- Health must be positive and armor must be nonnegative.
- Do not add a total-quantity bonus. A future capped bonus of at most 10% may be considered only after comparing same-size vanilla blocks.
- Treat a formula change of more than 20 armor, or a large reduction to an intentional explicit value, as **Review**, not an automatic edit.

## Worked example

For a 3×3 drill with titanium ×100, thorium ×60, and silicon ×40 (total 200):

```text
MH = 1 + (0.50 × 0.30) + (0.30 × 0.45) + (0.20 × 0.10) = 1.305
HP = roundNearest10(40 × 3² × 1.305 × 1.20) = roundNearest10(563.76) = 560

MA = (0.50 × 0.8) + (0.30 × 1.0) + (0.20 × 0.0) = 0.70
Armor = roundNearestWhole((2 + 1.5 × 3) × 0.70 × 1.00) = roundNearestWhole(4.55) = 5
```

## Applying the system to future blocks

1. Confirm the exact block type, size, construction requirements, and intended gameplay role.
2. Ensure every construction material has coefficients in this guide.
3. Calculate `MH`, `MA`, proposed HP, and proposed armor with the rules above.
4. Compare with an appropriate vanilla block of the same functional family and footprint.
5. Decide whether the result is safe to apply or requires an exception; document the reason.
6. After approval, write only numeric `health` and/or `armor` HJSON values and re-audit the block.

## Sources and approval checklist

- [Block.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/Block.java): native health initialization and armor field.
- [BuildingComp.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/entities/comp/BuildingComp.java): direct bullet collision handling.
- [Damage.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/entities/Damage.java) and [Vars.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/Vars.java): armor formula and 10% minimum direct damage.
- [Items.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Items.java): native item `healthScaling` values.
- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java): vanilla comparison definitions.

Before implementation, approve the formula values in the audit, explicitly resolve every `Review` recommendation, and test representative direct-hit and splash-damage scenarios in game. Do not treat the formula itself as gameplay testing.
