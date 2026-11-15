# Vanilla Cosmic — Turret Ammo Balance Guide (Mindustry v160.7)

This guide defines the design rules for future Vanilla Cosmic item, liquid, and gas turrets. It is a balance specification only; it does not add a turret or modify vanilla ammunition.

## Core rule: ammo is explicitly mapped

Mindustry does not give an item, liquid, or gas an automatic projectile. A turret maps each accepted resource to a `BulletType`; that bullet defines damage, range, splash, homing, piercing, status effects, fragments, lightning, and reload behavior.

- `ItemTurret` can map any registered item to a bullet type.
- `LiquidTurret` and `ContinuousLiquidTurret` can map any registered `Liquid`; Mindustry stores gases in this same content type.
- A universal turret may accept several resources, but it must map every accepted resource deliberately. Unmapped resources are rejected.
- Do not use hidden internal resources (`fissile-matter`, `dormant-cyst`, `gallium`) as ordinary ammunition.

## What “more efficient” means

Efficiency must not mean every Cosmic bullet has more damage than vanilla. Use a resource's value to grant a meaningful choice: improved ammo capacity, more reliable targeting, a role-specific effect, or higher damage—but not all of them at once.

### Item ammo efficiency

For `ItemTurret`, `ammoMultiplier` is internal ammo capacity gained per input item. One shot consumes `1 / ammoMultiplier` items. It does **not** multiply damage.

| Ammo band | Recommended `ammoMultiplier` | Intended tradeoff |
|---|---:|---|
| Common / utility | 2–4 | Cheap sustained fire with modest damage or utility. |
| Refined basic | 3–5 | Better damage, range, or accuracy without high-end effects. |
| Mid-progression ore | 2–4 | Stronger piercing, shrapnel, or controlled splash. |
| Advanced processed | 1–3 | Potent specialized effect; lower efficiency compensates. |
| Endgame alloy / catalyst | 1–2 | Exceptional effect such as lightning, heavy fragmentation, or missile payload; never also give extreme rate of fire. |

Use the lower end for rapid-fire turrets and the higher end only for slower, low-damage utility ammunition. `reloadMultiplier` is a separate firing-rate control and should normally stay between `0.75` and `1.25`; reserve stronger modifiers for a documented exception.

### Liquid and gas efficiency

`LiquidTurret` consumes `1 / ammoMultiplier` liquid per shot. `ContinuousLiquidTurret` consumes its configured liquid-per-tick amount adjusted by the ammunition multiplier. Liquid capacity must cover at least several shots or one sustained firing interval; never set it only to the exact one-shot cost.

For a spray turret, use `ammoMultiplier` around `0.4–1.5`. A lower value makes each shot consume more liquid. For a continuous turret, start from the desired liquid-per-second operating cost, then use a modest `0.8–1.5` multiplier. High-output gases should be balanced through consumption and power, not unlimited range or damage.

## Cosmic ammunition roles

The following are suggested roles. They are not automatic native effects and must be implemented in each bullet definition.

| Resource family | Suggested Cosmic role | Do not combine by default |
|---|---|---|
| Copper, lead, scrap, sand | Cheap kinetic, flak, or suppression ammunition | High damage with high ammo efficiency. |
| Graphite, metaglass, silicon | Accurate kinetic, fragmentation, or light homing | Strong splash plus strong homing. |
| Coal, pyratite, spore pods | Fire, area denial, or short-range burst | Long range and high fire chance. |
| Titanium, thorium | Piercing, shrapnel, or precision heavy rounds | Wide splash and unlimited piercing. |
| Blast compound, plastanium | Area damage or controlled fragments | High fragments, high splash, and high rate together. |
| Phase fabric, surge alloy | Utility displacement, special missile behavior, or lightning | Very high damage with major reload acceleration. |
| Beryllium, tungsten | Erekir precision piercing / anti-air progression | Strong anti-building and large area damage together. |
| Oxide, carbide | Late-game focused beam, artillery, or fragment payload | Cheap ammo multiplier or weak operating cost. |

## Liquid and gas roles

| Resource | Suggested Cosmic role | Native caution |
|---|---|---|
| Water | Wetting, knockback, extinguishing, light utility control | Avoid making it a high-DPS universal fuel. |
| Slag | Melting damage spray | Hot liquid; make range and damage modest. |
| Cryofluid | Freezing and fire suppression | Premium coolant; use consumption as the main cost. |
| Oil | Tarred area denial and flammable puddles | It does not ignite automatically. |
| Arkycite | Fuel-themed spray or volatile support round | Add fire/explosion behavior explicitly if used. |
| Ozone | Advanced continuous flame/energy stream | High-risk gas; do not give it both maximum range and DPS. |
| Hydrogen | Fast volatile beam or burst | Must receive an explicit custom bullet effect. |
| Nitrogen | Cooling, slow/control, or defensive utility stream | Prefer utility over raw damage. |
| Cyanogen | Endgame continuous beam or dangerous burst | Require substantial power and gas consumption. |
| Neoplasm | Excluded from ordinary ammunition | Deposited puddles can spread and damage buildings. |

## Sample ammo damage profiles

These are **starting examples**, not live Vanilla Cosmic block definitions. They show a coherent damage scale for a future 2×2 Cosmic item turret with one shot every 40 ticks (1.5 shots/s before coolant) and a future liquid/gas turret. Recalculate sustained DPS if the real turret uses burst shots, a different reload time, or multiple barrels.

### Example 2×2 Cosmic item turret

| Ammo | Bullet role | Direct damage per bullet | Extra effect | `ammoMultiplier` | Base single-target DPS at 1.5 shots/s |
|---|---|---:|---|---:|---:|
| Copper | Basic kinetic | 20 | None | 4 | 30 |
| Graphite | Precision kinetic | 36 | Slight range advantage | 3 | 54 |
| Silicon | Homing kinetic | 28 | Light homing; reduced raw damage pays for reliability | 3 | 42 |
| Pyratite | Incendiary round | 28 | `burning`, 12 splash damage, optional fire creation | 2 | 42 plus burn/splash |
| Titanium | Piercing round | 52 | Pierce one additional target | 2 | 78 per direct target before pierce value |
| Thorium | Heavy piercing round | 68 | Pierce two targets; slower or less efficient ammunition | 1.5 | 102 per direct target before pierce value |
| Surge alloy | Arc round | 48 | Two short lightning arcs, 12 damage each | 1 | 72 including both arcs if they connect |

The progression is intentional: graphite improves direct damage, silicon trades damage for hit reliability, pyratite trades efficiency for area/status pressure, titanium and thorium focus on piercing, and surge alloy gets a special effect at a high resource cost. Do not give one round both Thorium-level direct damage and Surge-level lightning.

### Example liquid/gas turret profiles

For a conventional spray turret, damage below is per projectile, not per second. For a continuous beam, use DPS because it applies damage continuously.

| Ammo | Turret style | Sample damage | Effect / constraint |
|---|---|---:|---|
| Water | Spray | 0.2 | `wet`, knockback, puddles, extinguishes fires; utility rather than DPS. |
| Slag | Spray | 5 | `melting`; a practical mid-tier damage spray. |
| Cryofluid | Spray | 0.2 | `freezing`, extinguishing, control utility. |
| Oil | Spray | 0.2 | `tarred` puddles; requires a separate ignition source for fire damage. |
| Ozone | Continuous beam | 55 DPS | Advanced flame/energy stream; require a meaningful gas rate. |
| Hydrogen | Continuous beam | 40 DPS | Volatile mid-tier stream; explicitly define its effect rather than relying on flammability. |
| Nitrogen | Continuous beam | 15 DPS | Low damage, slow/control or cooling utility. |
| Cyanogen | Continuous beam | 110 DPS | Endgame stream; pair with substantial power and gas consumption. |

These liquid and gas examples must still respect their chosen bullet class. `LiquidBulletType` naturally applies the liquid status and deposits puddles; a continuous flame/beam bullet needs its damage and status configured explicitly.

## Universal turret limits

Do not make an ordinary turret accept every resource. A large ammo map lets conveyor networks load weak ammo ahead of strong ammo, and the native item turret fires its queued ammo in order.

- Early Cosmic turret: 3–4 curated item choices.
- Mid-tier turret: 4–6 choices, optionally one liquid/gas variant only if its class supports it.
- Endgame universal turret: 6–10 deliberately tiered choices; every choice needs a distinct role and visible stats.
- Use separate item and liquid/gas turret blocks when both systems need deep choices. Native `ItemTurret` cannot accept liquid ammo, and native `LiquidTurret` cannot accept item ammo without custom Java.

## Pulsar reference implementation

Pulsar is the compact 1×1 introductory `ItemTurret` reference. It targets ground and air units without power, coolant, or a booster. Its 250-unit range, 25-tick reload (2.4 shots/s), 220 health, and 20 internal-ammo capacity give it a deliberate reach and convenience advantage over vanilla Duo's 160 range. Duo retains the faster 20-tick reload and 250 health. The extra ammunition choices provide flexibility, not an unrestricted damage increase.

| Ammo | Bullet role | Damage and effect | Range from `speed × lifetime` | `ammoMultiplier` | Reload behavior | Base output at 2.4 shots/s |
|---|---|---|---:|---:|---|---:|
| Copper | Basic kinetic | 13 direct damage | 252 | 4 | Normal | 31.2 direct DPS |
| Lead | Heavy kinetic | 17 direct damage; 1.2 armor multiplier | 251.1 | 3 | Normal | 40.8 direct DPS |
| Scrap | Cheap flak | 5 direct plus 10 splash in a 16-unit radius | 252 | 4 | 0.85 reload multiplier; 2.04 shots/s | 10.2 direct DPS plus area damage |
| Sand | Suppression | 8 direct; two 4-damage fragments | 252 | 4 | Normal | 19.2 direct DPS plus fragments |

Common-ammo efficiency stays within the guide's 2–4 range. Copper's 31.2 direct DPS is about 16% above Duo's 27 copper DPS, and Pulsar's 250-unit reach is its larger convenience advantage. Lead trades a small amount of projectile reach for stronger direct fire. Scrap and sand exchange raw single-target damage for affordable area/suppression behavior. None combines high damage, high capacity, and a major firing-rate advantage. The sand fragments deliberately retain their short 39-unit lifetime range as a local suppression effect, not an additional long-range projectile.

Pulsar construction is 35 copper, 20 lead, and 15 graphite. Research is exactly five times each construction material (175/100/75), following the repository convention. It is visible and usable on both Serpulo and Erekir through `shownPlanets`, but it has only the existing single Cosmic research node under `cosmic-i-drill`. Copper and lead are not native Erekir progression materials; Erekir construction therefore still requires a supplied source of those items.

### Pulsar sprite requirements

Place Pulsar art in `sprites/blocks/turret/pulsar/`.

| Filename | Required | Purpose | Expected dimensions |
|---|---|---|---|
| `pulsar.png` | Yes | The rotating turret body and primary block icon. | 32×32 px for a 1×1 block |
| `pulsar-barrel-r.png` | Yes | Right barrel overlay. It recoils on the first alternating muzzle shot. | 32×32 px |
| `pulsar-barrel-l.png` | Yes | Left barrel overlay. It recoils on the second alternating muzzle shot. | 32×32 px |
| `pulsar-base.png` | Optional | A dedicated stationary base. If absent, v160.7 `DrawTurret` falls back to `vanilla-cosmic-block-1`. | 32×32 px |
| `pulsar-preview.png` | Optional | The placement-preview body; if absent, the main `pulsar.png` is used. | 32×32 px |
| `pulsar-top.png` | Optional | A rotating layer rendered above the main turret body. | 32×32 px |
| `pulsar-heat.png` | Optional | Additive firing-heat overlay, visible only while the turret has heat. | 32×32 px |

`pulsar-outline.png` remains optional. Pulsar alternates one projectile between its 7-unit-spaced barrel overlays while retaining its existing 25-tick reload, damage, and ammo use. It also uses native `ShapePart` and `HaloPart` warmup visuals, which are procedural and need no sprite regions. No liquid, rotator, bottom, or glow region is used by the current definition.

## Shatterstar reference implementation

Shatterstar is a 1×1 `ItemTurret` with exactly 175 world-unit (21.875-tile) targeting range. It fires five immediate `ShrapnelBulletType` traces through native `ShootSpread` at -14°, -7°, 0°, 7°, and 14°, once per 70 ticks. That is 0.857 volleys per second. It targets both ground and air, has 20 internal ammo units, and has no power, coolant, liquid, gas, or heat-network requirement.

`ShrapnelBulletType` is an instantaneous line, not a traveling projectile. Shatterstar's 1×1 default muzzle is 4 units in front of its centre, so each trace uses `length: 171`: the centre ray ends at the turret's 175-unit target reach rather than unintentionally extending beyond it. `pierceCap` limits each trace to the stated count of collided units and/or buildings, and the ray ends at the capped target. The traces do not guarantee five hits on every target: the values below are the **theoretical full-overlap ceiling** for a sufficiently large or very close target. The full five-ray fan is roughly 83 units wide at its end, which makes it substantially less reliable against a small target at maximum range. The main visual widths are restrained to 7–9, with 2.25–2.75-wide serrations; this changes only the beam appearance, not range, damage, or collision.

| Ammo | Tactical role | Damage per trace | Theoretical five-trace volley | Nominal direct DPS ceiling | Penetration / armor interaction | `ammoMultiplier` | Item use at sustained fire |
|---|---|---:|---:|---:|---|---:|---:|
| Titanium | Efficient standard buckshot | 16 | 80 | 68.6 | Up to 2 eligible targets per trace; ordinary armor rules. | 4 | 0.214 titanium/s (12.86/min) |
| Thorium | Heavy kinetic burst | 24 | 120 | 102.9 | Up to 3 targets; direct-hit armor is multiplied by 0.6, so 40% is bypassed. | 2 | 0.429 thorium/s (25.71/min) |
| Tungsten | Armor-breaking flechettes | 20 | 100 | 85.7 | Up to 4 targets; `pierceArmor: true` bypasses ordinary direct-hit unit and building armor. | 2 | 0.429 tungsten/s (25.71/min) |
| Carbide | Premium hyperdense shrapnel | 28 | 140 | 120.0 | Up to 5 targets per trace; ordinary armor rules keep it from also being an unconditional armor-bypass round. | 1 | 0.857 carbide/s (51.43/min) |

One firing cycle consumes one internal ammo unit, not five: v160.7's default `consumeAmmoOnce: true` consumes once after the `ShootSpread` pattern produces its five traces. Consequently, `ammoMultiplier` is volleys per supplied item for this turret. Titanium supplies the best endurance; Thorium pays endurance for damage; Tungsten trades raw damage for the only full armor bypass; Carbide has the best burst and line penetration but consumes one carbide per volley. None uses a reload modifier, so every ammunition type keeps the 70-tick base cadence.

### Shatterstar balance comparison

| Turret | Size | Range | Reload | Burst model | Balance role |
|---|---:|---:|---:|---|---|
| Pulsar | 1×1 | 250 | 25 ticks | One conventional projectile | Flexible, long-reach introductory defense. |
| Shatterstar | 1×1 | 175 | 70 ticks | Five short-medium shrapnel traces | Position-dependent anti-armor and line-piercing burst; no coolant boost. |
| Fuse (vanilla) | 3×3 | 90 | 35 ticks | Three 20°-spaced shrapnel traces | Much larger dedicated close-range shotgun with considerably stronger titanium/thorium traces and optional coolant. |

Shatterstar does not replace Pulsar because it loses 75 range and sustained single-projectile reliability. It does not replace Fuse because it has a much lower per-trace damage, a slower base cycle, a smaller footprint, no coolant acceleration, and a narrow practical overlap window. It remains useful later only when premium ammunition, target geometry, and placement justify the associated resource cost.

### Shatterstar durability and construction

Construction is copper 45, lead 25, and graphite 20. Research is exactly 5× those requirements: copper 225, lead 125, and graphite 100. The basic-material recipe keeps it alongside Pulsar as a first-generation Cosmic defense; titanium, thorium, tungsten, and carbide are ammunition options rather than construction gates.

For the durability guide's new **Defensive turrets** role, use `RH = 5.25` and `RA = 1.00`. The higher health multiplier is a scoped exception for exposed combat blocks: the normal 40-HP 1×1 structural baseline is far below vanilla 1×1 turret durability. The armor multiplier remains conservative. For Shatterstar's recipe, `MH = 1.032` and `MA = 0.078`, yielding `roundNearest10(40 × 1² × 1.032 × 5.25) = 220 HP` and `roundNearestWhole(3.5 × 0.078 × 1.00) = 0 armor`. This matches Pulsar's 220 HP while preserving the absence of armor. Direct armor-piercing attacks still have no special protection to overcome.

### Shatterstar sprite requirements

Place sprites in `sprites/blocks/turret/shatterstar/`; the atlas uses their filenames, not the folder name.

| Filename | Required by the game | Purpose | Expected dimensions |
|---|---|---|---|
| `shatterstar.png` | Yes | Main rotating turret body and block icon. | 32×32 px |
| `shatterstar-mid.png` | Yes | Rotating mid-layer above the main body; drawn through `RegionPart`. | 32×32 px |
| `shatterstar-heat.png` | Yes for the requested firing-heat design | Additive firing overlay; native `DrawTurret` draws it only while firing heat remains. | 32×32 px |
| `shatterstar-base.png` | No | Dedicated stationary base; otherwise the native `vanilla-cosmic-block-1` fallback is used. | 32×32 px |
| `shatterstar-preview.png` | No | Placement-preview override; otherwise the main body is used. | 32×32 px |
| `shatterstar-outline.png` | No | Optional underlay for procedural parts; do not create it unless a separate outline is wanted. | 32×32 px |
| `shatterstar-top.png` | No | Optional rotating body overlay. | 32×32 px |

`ShapePart` and `HaloPart` are procedural native effects using `#FFD28A`; they require no image. `DrawTurret` tolerates every optional region being absent. Do not add a liquid, bottom, glow, barrel, or rotator sprite for this configuration.

## Aperture reference implementation

Aperture is a 1×1 `ItemTurret` long-range rail sniper. It has exactly 350 world units (43.75 tiles) of targeting range, targets both ground and air, and fires one precise `RailBulletType` shot per 100-tick cycle. Its native `ShootPattern` uses `firstShotDelay: 18`, which is a real charge delay before the rail is created; the steady-state cadence remains 0.6 shots per second because the turret reloads during that delay. It has no power, coolant, liquid, gas, or heat-network requirement and stores 12 internal ammo units.

Rail shots are instantaneous lines, not travelling projectiles. The default 1×1 muzzle sits 4 units forward of the turret centre, so Aperture uses `length: 346`: its centre line reaches the 350-unit targeting edge without unintentionally exceeding it. Every round has a finite `pierceCap`. When a rail pierces a target, its remaining damage is reduced by `min(current damage, target initial health × pierceDamageFactor)`; exact post-pierce damage therefore depends on the targets actually struck. Native rails do not expose a beam-width field; Aperture instead spaces its `instTrail` particles every 32 units and omits the large no-hit end burst, reducing visual density without changing combat behavior.

| Ammo | Tactical role | Initial damage | First-hit nominal DPS | Penetration | Armor / building interaction | `ammoMultiplier` | Sustained use |
|---|---|---:|---:|---|---|---:|---:|
| Graphite | Affordable precision slug | 80 | 48 | 2 targets; 20% target-health loss factor | Ordinary armor; 0.7× building damage. | 3 | 0.20 graphite/s (12/min) |
| Titanium | Accelerated aligned-target penetrator | 110 | 66 | 3 targets; 12% loss factor | Ordinary armor; 0.7× building damage. | 2 | 0.30 titanium/s (18/min) |
| Thorium | Heavy-density rail slug | 150 | 90 | 2 targets; 8% loss factor | Effective direct-hit armor is 0.65×; 0.8× building damage. | 1 | 0.60 thorium/s (36/min) |
| Tungsten | Hypervelocity armor penetrator | 125 | 75 | 4 targets; 5% loss factor | `pierceArmor: true` bypasses ordinary direct-hit armor; 0.7× building damage. | 1 | 0.60 tungsten/s (36/min) |

Graphite provides sustained early sniper fire. Titanium improves aligned-target pressure. Thorium has the highest initial impact and reduced armor effectiveness but only two penetrations. Tungsten is deliberately lower in raw initial damage than Thorium; its reason to exist is full direct-hit armor bypass, the widest finite penetration chain, and the strongest damage retention. No ammunition changes Aperture's reload speed.

### Aperture balance comparison

| Turret | Size | Range | Cycle | Operating model | Role |
|---|---:|---:|---:|---|---|
| Pulsar | 1×1 | 250 | 25 ticks | Conventional flexible rounds | Sustained general-purpose defense. |
| Shatterstar | 1×1 | 175 | 70 ticks | Five shrapnel traces | Close-medium anti-armor burst and lined-target pressure. |
| Aperture | 1×1 | 350 | 100 ticks + 18-tick charge | One precise finite-pierce rail | Long-range support against aligned durable targets. |
| Foreshadow (vanilla) | 4×4 | 500 | 200 ticks | 1,350-damage rail; five ammo units/shot; power and coolant | Dedicated expensive endgame railgun. |

Aperture trades rate of fire, crowd coverage, and reload economy for reach and direct precision. It cannot replace Pulsar against dispersed targets, Shatterstar at short range, or Foreshadow's high-damage endgame rail role. A large wave can overwhelm it because every cycle produces one line rather than an area effect or a multi-target fan.

### Aperture construction, durability, and sprites

Construction is copper 50, lead 30, graphite 35, and silicon 15. Research is exactly 5× each requirement: copper 250, lead 150, graphite 175, and silicon 75. This modest refined-material increase over Pulsar reflects its 350-unit precision hardware without requiring titanium, tungsten, or any endgame material. Aperture is a sibling of Pulsar and Shatterstar under `cosmic-i-drill`.

Using the Defensive turrets role, Aperture's recipe gives `MH = 1.045` and `MA = 0.073`. The calculated result is `roundNearest10(40 × 1² × 1.045 × 5.25) = 220 HP` and `roundNearestWhole(3.5 × 0.073 × 1.00) = 0 armor`. Long range does not add a durability bonus.

Place sprite assets in `sprites/blocks/turret/aperture/`.

| Filename | Required by the game | Purpose | Expected dimensions |
|---|---|---|---|
| `aperture.png` | Yes | Main rotating rail-turret body and block icon. | 32×32 px |
| `aperture-mid.png` | Yes | Rotating mid-layer above the main body, rendered through `RegionPart`. | 32×32 px |
| `aperture-heat.png` | Yes for the requested firing-heat design | Additive heat overlay, visible only while firing heat remains. | 32×32 px |
| `aperture-base.png` | No | Dedicated stationary base; otherwise `vanilla-cosmic-block-1` is used. | 32×32 px |
| `aperture-preview.png` | No | Placement-preview override; otherwise the main body is used. | 32×32 px |
| `aperture-outline.png` | No | Optional underlay for procedural parts. | 32×32 px |
| `aperture-top.png` | No | Optional rotating body overlay. | 32×32 px |

The `ShapePart` and `HaloPart` are copied from Pulsar's first-generation Cosmic style and recolored to `#FFABE5`; they require no sprite. Do not create liquid, bottom, glow, barrel, or rotator assets for the current configuration.

## Required balance checks for every future turret

1. Declare the native class first: `ItemTurret`, `LiquidTurret`, or `ContinuousLiquidTurret`.
2. List each accepted resource, bullet class, damage, range, status, splash, piercing/fragment count, and ammo efficiency in the block-specific guide.
3. Compare sustained damage per second and resource consumption against the closest vanilla turret of the same role.
4. Give high damage, splash, homing, lightning, piercing, and high firing rate separate costs; do not stack all of them in one ammo type.
5. For gases and high-tier ammunition, include a meaningful ongoing resource, power, or heat requirement.
6. Test one ammo type at a time, then test mixed-item queue behavior, liquid switching, target classes, and fire/extinguishing behavior in game.
7. Keep construction and research costs under the repository-wide 5× research-cost rule unless a future approved turret guide establishes an exception.

## Sources

- [Blocks.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/content/Blocks.java): vanilla mappings and reference turret behavior.
- [ItemTurret.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/defense/turrets/ItemTurret.java): item ammo storage and consumption.
- [LiquidTurret.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/defense/turrets/LiquidTurret.java) and [ContinuousLiquidTurret.java](https://github.com/Anuken/Mindustry/blob/v160.7/core/src/mindustry/world/blocks/defense/turrets/ContinuousLiquidTurret.java): liquid/gas ammo behavior.
