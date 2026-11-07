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

Pulsar is the compact 1×1 introductory `ItemTurret` reference. It targets ground and air units without power, coolant, or a booster. Its 150-unit range, 30-tick reload (2 shots/s), 220 health, and 20 internal-ammo capacity intentionally sit below vanilla Duo's 160 range, 20-tick reload (3 shots/s), and 250 health. The extra ammunition choices provide flexibility, not a higher baseline damage rate.

| Ammo | Bullet role | Damage and effect | Range from `speed × lifetime` | `ammoMultiplier` | Reload behavior | Base output at 2 shots/s |
|---|---|---|---:|---:|---|---:|
| Copper | Basic kinetic | 10 direct damage | 150 | 3 | Normal | 20 direct DPS |
| Lead | Heavy kinetic | 15 direct damage; 1.2 armor multiplier | 135 | 2 | Normal | 30 direct DPS |
| Scrap | Cheap flak | 4 direct plus 8 splash in a 16-unit radius | 140 | 4 | 0.85 reload multiplier; 1.7 shots/s | 6.8 direct DPS plus area damage |
| Sand | Suppression | 6 direct; two 3-damage fragments | 150 | 4 | Normal | 12 direct DPS plus fragments |

Common-ammo efficiency stays within the guide's 2–4 range. Lead earns its lower multiplier through stronger single-target damage; scrap and sand exchange raw damage for affordable area/suppression behavior. None combines high damage, high capacity, and a major firing-rate advantage. The sand fragments are a small positional bonus, not a reliable single-target DPS increase.

Pulsar construction is 35 copper, 20 lead, and 15 graphite. Research is exactly five times each construction material (175/100/75), following the repository convention. It is visible and usable on both Serpulo and Erekir through `shownPlanets`, but it has only the existing single Cosmic research node under `cosmic-i-drill`. Copper and lead are not native Erekir progression materials; Erekir construction therefore still requires a supplied source of those items.

### Pulsar sprite requirements

Place art in `sprites/blocks/defense/`.

| Filename | Required | Purpose | Expected dimensions |
|---|---|---|---|
| `pulsar.png` | Yes | The rotating turret body and primary block icon. | 32×32 px for a 1×1 block |
| `pulsar-base.png` | Optional | A dedicated stationary base. If absent, v160.7 `DrawTurret` falls back to `vanilla-cosmic-block-1`. | 32×32 px |
| `pulsar-preview.png` | Optional | The placement-preview body; if absent, the main `pulsar.png` is used. | 32×32 px |
| `pulsar-top.png` | Optional | A rotating layer rendered above the main turret body. | 32×32 px |
| `pulsar-heat.png` | Optional | Additive firing-heat overlay, visible only while the turret has heat. | 32×32 px |

`pulsar-outline.png` is only needed if future draw parts are added; the current simple turret has none. No liquid, rotator, bottom, or glow region is used by the current definition.

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
