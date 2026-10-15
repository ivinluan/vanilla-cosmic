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
| Drills, beam bores, BurstDrills, WallCrafters | 1.20 | 1.00 |
| Mass drivers | 1.25 | 1.20 |
| Defensive walls | 1.50 | 2.00 |

No defensive-wall multiplier is used in the current audit: the repository has no defensive wall HJSON files.

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
