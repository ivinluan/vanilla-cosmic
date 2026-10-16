# Vanilla Cosmic — Block Durability Audit (v160.7)

This audit records the pre-approval HJSON values and the formula recommendations calculated from their construction requirements. The formula values were approved and applied to every audited HJSON block on 2026-10-15. `Native` means Mindustry's calculated health when `health` was absent. `Default` armor is 0. Formula columns use the rules in [Block Durability Balance Guide](block-durability-balance-guide.md).

Vanilla comparison shorthand: `Conveyor` = titanium/plastanium conveyor; `Router` = router; `Junction` = junction; `Bridge` = buffered/phase conveyor; `Stack` = surge conveyor; `Drill` = pneumatic/laser/impact drill by tier; `Bore` = large plasma bore; `WallCrafter` = cliff crusher or large cliff crusher; `Pump` = mechanical/rotary/impulse pump; `Mass Driver` = mass driver.

## Distribution — 10 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| `celestial-bridge` | BufferedItemBridge, 1 | titanium 30, thorium 30, silicon 15, graphite 10 | 80 explicit / 0 default | 50 / 2 | Bridge | Decrease/review: formula keeps a small logistics footprint fragile. |
| `cosmic-junction` | Junction, 1 | beryllium 40, tungsten 30, carbide 15 | 110 explicit / 0 default | 50 / 1 | Junction | Decrease/review: present HP is high for a 1×1 junction. |
| `cosmic-router` | Router, 1 | beryllium 35, tungsten 25, carbide 12 | 100 explicit / 0 default | 40 / 1 | Router | Decrease/review: present HP is high for a 1×1 router. |
| `interstellar-hyperlane` | ItemBridge, 1 | beryllium 50, tungsten 40, carbide 25, phase-fabric 30, silicon 40 | 110 explicit / 0 default | 50 / 1 | Bridge | Decrease/review: formula gives advanced transport modest armor, not wall durability. |
| `quasar-mass-driver` | MassDriver, 4 | titanium 300, silicon 200, lead 300, thorium 150, plastanium 100, phase-fabric 80, surge-alloy 50 | 1296 explicit / 0 default | 1010 / 5 | Mass Driver | Review: existing 1296 HP was deliberately set to 3× vanilla Mass Driver; decide whether that prior target overrides the formula. |
| `solar-conveyor` | Conveyor, 1 | copper 20, lead 20, graphite 10 | 40 explicit / 0 default | 40 / 0 | Conveyor | Keep: health matches formula; zero armor rounds from a very small material score. |
| `stellar-stack-conveyor` | StackConveyor, 1 | beryllium 100, tungsten 80, carbide 40, surge-alloy 40 | 150 explicit / 0 default | 50 / 2 | Stack | Decrease/review: high throughput does not itself warrant high static durability. |
| `stellar-stack-router` | StackRouter, 1 | beryllium 110, tungsten 90, carbide 45, surge-alloy 50 | 160 explicit / 0 default | 50 / 2 | Stack | Decrease/review: same 1×1 logistics principle. |
| `supernova-conveyor` | Conveyor, 1 | titanium 20, thorium 20, silicon 10 | 60 explicit / 0 default | 40 / 1 | Conveyor | Decrease/review: one armor is a reasonable advanced-material distinction. |
| `void-conveyor` | Conveyor, 1 | beryllium 30, tungsten 20, carbide 10 | 80 explicit / 0 default | 40 / 1 | Conveyor | Decrease/review: advanced material composition alone should not make a belt wall-like. |

## Drills, bores, and WallCrafters — 27 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| `beryllium-rift-fracturer` | WallCrafter, 4 | beryllium 240, graphite 120, silicon 80 | 820 explicit / 80 explicit | 860 / 2 | WallCrafter | Review: HP aligns; armor 80 is far beyond formula and should be manually justified. |
| `cosmic-drill-pro-max` | Drill, 4 | copper 780, lead 780, graphite 330, silicon 250, phase-fabric 200, surge-alloy 40 | 1120 explicit / 1200 explicit | 840 / 1 | Drill | Review: armor 1200 nearly negates ordinary direct hits; formula flags it as an exceptional override. |
| `cosmic-drill-pro` | Drill, 3 | copper 540, lead 540, graphite 230, silicon 170, titanium 450, thorium 450 | 1040 explicit / 1080 explicit | 500 / 3 | Drill | Review: both values are intentional-tier candidates, but armor is dramatically above normal production durability. |
| `cosmic-i-drill-mini` | Drill, 1 | copper 75, graphite 30, silicon 20, titanium 65 | 200 explicit / 60 explicit | 50 / 1 | Drill | Review: mini footprint should not retain wall-level armor. |
| `cosmic-i-drill` | Drill, 2 | copper 150, graphite 60, silicon 40, titanium 130 | 400 explicit / 120 explicit | 220 / 1 | Drill | Review: current armor substantially exceeds material/role result. |
| `cosmic-ii-drill-mini` | Drill, 1 | copper 90, graphite 40, silicon 25, plastanium 25 | 240 explicit / 120 explicit | 50 / 0 | Drill | Review: formula rounds the small armor score to zero. |
| `cosmic-ii-drill` | Drill, 2 | copper 180, graphite 80, silicon 50, plastanium 50 | 480 explicit / 240 explicit | 210 / 1 | Drill | Review: current armor is a major exception. |
| `cosmic-iii-drill-mini` | Drill, 1 | copper 105, graphite 45, silicon 30, titanium 90, thorium 90 | 280 explicit / 180 explicit | 60 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-iii-drill` | Drill, 2 | copper 210, graphite 90, silicon 60, titanium 180, thorium 180 | 560 explicit / 360 explicit | 230 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-iv-drill-mini` | Drill, 1 | copper 120, graphite 50, silicon 35, titanium 100, phase-fabric 30 | 320 explicit / 240 explicit | 60 / 1 | Drill | Review: current armor is a major exception. |
| `cosmic-iv-drill` | Drill, 2 | copper 240, graphite 100, silicon 70, titanium 200, phase-fabric 60 | 640 explicit / 480 explicit | 220 / 1 | Drill | Review: current armor is a major exception. |
| `cosmic-photon-bore` | BeamDrill, 5 | copper 300, lead 300, graphite 120, silicon 80, titanium 160, thorium 160, beryllium 160, carbide 20 | 790 explicit / 240 explicit | 1380 / 3 | Bore | Review: formula supports a large HP increase, but not the present armor. |
| `cosmic-prism-bore` | BeamDrill, 2 | copper 150, lead 150, graphite 60, silicon 40, plastanium 40 | 400 explicit / 120 explicit | 210 / 1 | Bore | Review: current armor is a major exception. |
| `cosmic-radiant-bore` | BeamDrill, 3 | copper 190, lead 190, graphite 75, silicon 50, titanium 100, phase-fabric 50 | 500 explicit / 150 explicit | 480 / 1 | Bore | Review: HP closely matches; armor needs separate approval. |
| `cosmic-spectral-bore` | BeamDrill, 4 | copper 240, lead 240, graphite 95, silicon 65, titanium 125, thorium 125, surge-alloy 15 | 630 explicit / 190 explicit | 880 / 3 | Bore | Review: formula supports more HP but far less armor. |
| `cosmic-v-drill-mini` | Drill, 1 | copper 135, graphite 55, silicon 40, titanium 115, thorium 115, beryllium 115 | 360 explicit / 300 explicit | 60 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-v-drill` | Drill, 2 | copper 270, graphite 110, silicon 80, titanium 230, thorium 230, beryllium 230 | 720 explicit / 600 explicit | 230 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-vi-drill-mini` | Drill, 1 | copper 150, graphite 65, silicon 45, titanium 125, thorium 125, surge-alloy 10 | 400 explicit / 360 explicit | 60 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-vi-drill` | Drill, 2 | copper 300, graphite 130, silicon 90, titanium 250, thorium 250, surge-alloy 20 | 800 explicit / 720 explicit | 230 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-vii-drill-mini` | Drill, 1 | copper 165, graphite 70, silicon 50, titanium 140, thorium 140, beryllium 140, tungsten 110 | 440 explicit / 420 explicit | 60 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-vii-drill` | Drill, 2 | copper 330, graphite 140, silicon 100, titanium 280, thorium 280, beryllium 280, tungsten 220 | 880 explicit / 840 explicit | 230 / 3 | Drill | Review: current armor is a major exception. |
| `cosmic-viii-drill-mini` | Drill, 1 | copper 180, graphite 75, silicon 55, titanium 150, thorium 150, beryllium 150, carbide 30 | 480 explicit / 480 explicit | 60 / 2 | Drill | Review: current armor is a major exception. |
| `cosmic-viii-drill` | Drill, 2 | copper 360, graphite 150, silicon 110, titanium 300, thorium 300, beryllium 300, carbide 60 | 960 explicit / 960 explicit | 230 / 2 | Drill | Review: current armor is a major exception. |
| `supernova-drill` | BurstDrill, 8 | copper 390, lead 390, graphite 180, silicon 360, plastanium 100, phase-fabric 100, surge-alloy 40 | 3200 explicit / 240 explicit | 3430 / 2 | Drill | Review: large footprint supports the HP increase; armor needs separate approval. |
| `thorium-rift-fracturer` | WallCrafter, 4 | copper 180, lead 180, graphite 80, silicon 60, titanium 90, thorium 100, plastanium 60, phase-fabric 60 | 760 explicit / 70 explicit | 900 / 3 | WallCrafter | Review: formula supports HP increase but not the present armor. |
| `titanium-rift-fracturer` | WallCrafter, 4 | copper 150, lead 150, graphite 60, silicon 40, plastanium 40, phase-fabric 40, titanium 80 | 700 explicit / 60 explicit | 870 / 2 | WallCrafter | Review: formula supports HP increase but not the present armor. |
| `tungsten-rift-fracturer` | WallCrafter, 4 | beryllium 260, tungsten 180, graphite 140, silicon 110, oxide 60, phase-fabric 60, surge-alloy 30 | 900 explicit / 100 explicit | 960 / 4 | WallCrafter | Review: formula supports HP increase but not the present armor. |

## Liquid pumps — 2 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| `pulsar-liquid-pump` | Pump, 1 | copper 60, metaglass 50, titanium 35, silicon 25 | 40 Native / 0 Default | 40 / 0 | Pump | Keep: native HP already matches formula. |
| `quasar-liquid-pump` | Pump, 2 | copper 150, metaglass 180, titanium 150, silicon 120, thorium 100, plastanium 60 | 210 Native / 0 Default | 190 / 1 | Pump | Review: formula is a modest HP decrease and adds one armor. |

## Conduits — 3 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| solar-conduit | Conduit, 1 | copper 2, lead 2, metaglass 2 | New block | 40 / 0 | Conduit | Applied: entry liquid transport stays inexpensive and unarmored. |
| supernova-conduit | Conduit, 1 | lead 2, metaglass 3, titanium 2, silicon 2 | New block | 50 / 1 | Pulse Conduit | Applied: modest tier increase without wall-level durability. |
| void-conduit | Conduit, 1 | metaglass 4, titanium 3, silicon 3, thorium 2, plastanium 2, surge alloy 1 | New block | 50 / 1 | Plated Conduit | Applied: capability rises through capacity and pressure, not excessive HP or armor. |

## Liquid routing — 2 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| cosmic-liquid-router | LiquidRouter, 1 | beryllium 35, tungsten 25, carbide 12, metaglass 20 | New block | 50 / 2 | Liquid Router | Applied: high capacity supports routing without making the block wall-like. |
| cosmic-liquid-junction | LiquidJunction, 1 | beryllium 40, tungsten 30, carbide 15, metaglass 20 | New block | 50 / 2 | Liquid Junction | Applied: direct crossing has no stored liquid, but retains advanced utility-block durability. |

## Liquid bridges — 2 blocks

| Block ID | Type, size | Requirements | Current HP / armor | Formula HP / armor | Vanilla comparison | Recommendation and reason |
|---|---|---|---|---|---|---|
| celestial-aqueduct | LiquidBridge, 1 | titanium 30, thorium 30, silicon 15, graphite 10, metaglass 30 | New block | 50 / 1 | Bridge Conduit | Applied: advanced unpowered range and buffer without wall-level durability. |
| voidstream-hyperlane | LiquidBridge, 1 | beryllium 50, tungsten 40, carbide 25, phase fabric 30, silicon 40, metaglass 40 | New block | 50 / 1 | Phase Conduit | Applied: power, range, and buffer define the higher tier instead of extreme durability. |

## Audit outcome

- Blocks currently covered: **46**.
- Largest proposed HP increase: `cosmic-photon-bore`, 790 → 1380 (+590). The 5×5 footprint drives this result.
- Largest proposed HP decrease: `cosmic-viii-drill`, 960 → 230 (−730). This reflects the formula's refusal to multiply durability by total recipe quantity.
- Largest armor decrease: `cosmic-drill-pro-max`, 1200 → 1 (−1199). The existing drill armor series is the dominant manual-review concern.
- Largest armor increase: `quasar-mass-driver`, 0 → 5. This is still moderate and affects direct non-piercing bullet hits only.
- All current construction materials are covered by coefficients. There are no missing coefficients.

### Approval decisions needed

1. Decide whether the existing high armor values on the Cosmic drill/bore series are intentional. Under v160.7, they dramatically reduce ordinary direct bullet damage, although not all splash damage.
2. Decide whether Quasar Mass Driver's approved 3× vanilla health target (1296) should remain an explicit exception instead of using formula HP 1010.
3. Review logistics decreases before any implementation: the formula keeps 1×1 transport easy to destroy even when expensive.
4. Review the formula-driven HP increases for Photon Bore, Supernova Drill, and the three Rift Fracturers.

The formula values are now the implemented HJSON durability values. Re-audit when a block's size, requirements, or role changes.
