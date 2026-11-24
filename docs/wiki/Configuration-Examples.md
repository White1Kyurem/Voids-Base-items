# Configuration examples

[Home](Home.md) · [Main reference](Main-Settings.md) · [AC reference](Air-Conditioners.md)

Back up the generated files before editing. Complete downloadable examples are in [the examples folder](../examples/). They are baseline examples, not instructions to overwrite a customized server configuration.

| File | Use |
| --- | --- |
| [settings.default.json](../examples/settings.default.json) | Complete main configuration, including every generated display rule from the documented schema. |
| [settings.pve.json](../examples/settings.pve.json) | Same complete configuration with VBI raids disabled. |
| [settings.hidden-displays.json](../examples/settings.hidden-displays.json) | Same complete configuration with the global display fallback and every built-in display rule set to false. |
| [AirConditioners.default.json](../examples/AirConditioners.default.json) | Complete default wall/portable AC configuration. |
| [AirConditioners.cooler-wall.json](../examples/AirConditioners.cooler-wall.json) | Wall units: 8-meter radius, 18°C, 0.07 energy/second; portable units unchanged. |

## Disable VBI raids

Change `RaidEnabled` to `false` in your existing main file. Preserve the other fields. The complete PvE example changes only this field from the documented defaults.

## Longer raids with a consumed tool

Change these fields in the existing main file:

```json
{
  "RaidEnabled": true,
  "RaidItemClassname": "Lockpick",
  "RaidDurationSeconds": 600.0,
  "ConsumeRaidItem": true
}
```

This means a ten-minute successful raid consumes the tool. `RaidToolDamagePercent` is not applied in addition to consuming it. This block is a fragment showing changes, not a full replacement file.

## Longer battery runtime

Lower the consumption values of the supported devices or reduce the number of operating devices. Increasing `SolarEnergyPerSecond` improves daytime generation but does not add night generation or increase battery capacity.

For example, halving `FridgeEnergyPerSecond` from `0.02` to `0.01` halves that refrigerator’s configured operating drain. It does not halve your entire base’s consumption.

## Adjust food preservation

- `FridgeDecayMultiplier: 0.2` — default, 20% progression while cooling.
- `FridgeDecayMultiplier: 0.5` — 50% progression, weaker preservation than default.
- `FridgeDecayMultiplier: 1.0` — normal progression.
- `FridgeDecayMultiplier: 0.0` — this decay progression stops while cooling applies.

Preserve working power and keep the door closed to apply the refrigerator benefit.

## Hide all configured attachment displays

Use the hidden-displays example as a baseline, or edit your existing file so `ShowAttachmentsByDefault` and every applicable `ShowAttachments` entry are `false`.

An empty array with a global false is insufficient because missing built-in rules can be reinserted as true. The example keeps those rules explicitly false. Inventory slots remain usable.

## Change AC models independently

Edit `Wall` for every wall unit and `Floor` for every portable unit. To show 18°C on wall units, set `Wall.TargetTemperatureC` to the integer `18`. The display and cooling use the same target.

Restart the server after every example change. For a minimal, controlled test, change one category at a time and verify its effect before adjusting another.
