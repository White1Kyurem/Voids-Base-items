# Solar power, lighting and refrigeration

[Home](Home.md) · [Main settings](Main-Settings.md) · [Air conditioners](Air-Conditioners.md)

## Solar setup

Use a non-ruined car **or** truck battery in the panel; one battery is supported at a time. Place the panel where its sky-check point has a clear path upward. Switch the output off before removing the battery.

The attached physical battery is the energy store. Generation charges it, and connected appliances draw from it. Removing and reinserting the battery does not refill it. At night or with the sky blocked, consumers can continue using the remaining stored energy.

| Setting | Default | Meaning |
| --- | --- | --- |
| `SolarEnergyPerSecond` | `0.15` | Peak energy units generated per real elapsed second. |
| `SolarSunriseHour` | `6.0` | Beginning of the in-game generation window. |
| `SolarSunsetHour` | `18.0` | End of the in-game generation window. |

Generation also requires the game’s night flag to be false, a clear sky check, a usable panel and attached battery capacity. Extending the configured window does not force generation during the game’s night state.

### Daylight curve

Between sunrise and sunset, generation follows a sine curve: zero at either boundary and the configured peak at the midpoint.

```text
rate = SolarEnergyPerSecond × sin(π × (hour − sunrise) / (sunset − sunrise))
```

With defaults and other conditions met:

| In-game time | Approximate generation rate |
| --- | --- |
| 06:00 | 0 |
| 09:00 | 0.1061 energy units/second |
| 12:00 | 0.15 energy units/second |
| 15:00 | 0.1061 energy units/second |
| 18:00 | 0 |

The panel updates about every five real seconds. Its rate uses elapsed real seconds; the curve position uses in-game time. Changing server time acceleration changes how quickly the panel moves through its daylight window.

`SolarEnergyPerSecond: 0` stops solar charging. It does not delete stored battery energy. Use a sunrise earlier than sunset; this implementation does not support a window crossing midnight.

## Which appliance uses which setting?

| Appliance | Configuration source | Default consumption |
| --- | --- | --- |
| `VBI_WallLamp` | `WallLampEnergyPerSecond` in main JSON | 0.005 |
| `VBI_FloorLamp` | `FloorLampEnergyPerSecond` in main JSON | 0.01 |
| `VBI_Fridge` | `FridgeEnergyPerSecond` in main JSON | 0.02 |
| `VBI_DecorTableLamp` | Item configuration; no corresponding main JSON field | 0.005 |
| `VBI_Floodlight` | Item configuration; no corresponding main JSON field | 0.02 |
| `VBI_WallFloodlight` | Item configuration; no corresponding main JSON field | 0.02 |
| Wall AC | `Wall.EnergyUsagePerSecond` in AC JSON | 0.05 |
| Portable AC | `Floor.EnergyUsagePerSecond` in AC JSON | 0.04 |

All rates are DayZ energy units per real second. Higher consumption drains the source faster. The main lamp/fridge rates have a positive minimum of `0.0001`; setting them to zero is clamped rather than making them free.

The four-outlet power strip distributes energy from its upstream source. It does not generate energy or create a separate battery reserve.

### Example energy budget

One wall lamp (`0.005`), one floor lamp (`0.01`), a fridge (`0.02`), a wall AC (`0.05`) and a portable AC (`0.04`) total **0.125 units/second** while operating.

A default panel can exceed that load near its peak (`0.15`), but generates less than the load at 09:00 (`~0.1061`) and nothing at night. The battery covers the deficit until empty. This is a rate example, not a guarantee of all-night runtime; capacity, charge, daylight duration and other consumers matter.

## Refrigerator settings

| Setting | Default | Effect |
| --- | --- | --- |
| `FridgeEnergyPerSecond` | `0.02` | Energy consumption. |
| `FridgeTemperatureC` | `4.0` | Target for eligible contents, limited to 0–10°C. |
| `FridgeDecayMultiplier` | `0.2` | Scales food decay progression, limited to 0–1. |

Refrigeration applies while the fridge is **closed, non-ruined and receiving working power**. The fridge switches itself on when plugged in; an upstream source still needs to supply actual energy. Opening the door or losing power removes its cooling/decay benefit.

At `0.2`, the affected food-decay progression runs at 20% of normal. At `1`, it progresses normally; at `0`, this specific progression stops while refrigeration applies. This does not repair spoiled food or reset elapsed decay.

The refrigerator accepts compatible edible items and drink containers, with liquid checks. It is not unrestricted general-purpose cargo. Temperature processing applies to eligible contents; the camping cooler is not controlled by these refrigerator settings.
