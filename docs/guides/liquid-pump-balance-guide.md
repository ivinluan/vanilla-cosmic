# Vanilla Cosmic — Liquid Pump Balance Guide (Mindustry v160.7)

This guide balances terrain liquid pumps. It applies to the `Pump` block type and distinguishes its raw engine rate from the output shown in-game.

## Engine rate rule

`pumpAmount` is liquid **per tick per valid liquid tile**, not liquid per second. At 60 ticks per second:

```text
per-tile liquid/sec = pumpAmount × 60 × floor liquidMultiplier
total liquid/sec = per-tile rate × number of valid tiles
```

A multiblock pump can use every valid tile under its footprint. It must cover one liquid type only; mixed liquid floors make placement invalid. The liquid network and the block's internal capacity can still limit sustained output.

## Current progression

| Block | Size | `pumpAmount` | Nominal per-tile output | Full-footprint output on normal liquid | Capacity | Power | Research parent |
|---|---:|---:|---:|---:|---:|---|---|
| Pulsar Liquid Pump | 1×1 | 0.41666667 | 25 liquid/sec | 25 liquid/sec | 100 | None | `rotary-pump` |
| Quasar Liquid Pump | 2×2 | 1.66666667 | 100 liquid/sec | 400 liquid/sec | 500 | None | `pulsar-liquid-pump` |

The Quasar has four times Pulsar's per-tile rate and can cover four liquid tiles, giving it a theoretical 16× total output advantage on a normal 2×2 liquid area.

## Cost and research rules

- Use common liquid infrastructure materials—copper and metaglass—as the recipe foundation.
- Add titanium and silicon for faster, advanced pumps. Add thorium and plastanium only for the highest-capacity cosmic tier.
- Keep these pumps unpowered only when their construction cost and progression position compensate for that benefit.
- `researchCost` is exactly **5×** each corresponding construction requirement, matching the repository-wide resource and distribution guides.
- Do not use hidden resources or materials unavailable before the intended research parent.

## HJSON and sprite checklist

- Use `type: Pump`, `category: liquid`, `size`, `pumpAmount`, and `liquidCapacity`.
- Do not add `consumes.power` or `hasPower` to an unpowered pump.
- `Pump` inherits the liquid storage behavior needed for its internal liquid buffer.
- Provide `block-id.png` and `block-id-liquid.png`: respectively, the main block artwork and liquid-overlay region. For these blocks: `pulsar-liquid-pump.png`, `pulsar-liquid-pump-liquid.png`, `quasar-liquid-pump.png`, and `quasar-liquid-pump-liquid.png`.
- Test final sprites and terrain output in-game. This guide's rate calculations assume normal liquid multiplier and an unconstrained connected liquid network.
