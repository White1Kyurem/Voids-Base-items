# Main settings reference

[Home](Home.md) · [Examples](Configuration-Examples.md) · [Troubleshooting](Troubleshooting.md)

**File:** `<profiles>/Voids_Base_Items/settings.json`  
**Schema:** `ConfigVersion: 2`  
**Apply changes:** restart the server.

Defaults below describe a newly generated configuration. Existing settings retain their saved values.

## Schema, raids and dismantling

| Setting | Type | Default | Accepted value / limit | Effect |
| --- | --- | --- | --- | --- |
| `ConfigVersion` | integer | `2` | Keep at `2`; recognized `1` is migrated. | Configuration schema marker, not the Workshop release number. Unsupported versions disable VBI raids. |
| `RaidEnabled` | boolean | `true` | `true` / `false` | Enables the VBI raid action on eligible closed, locked storage. `false` disables this action; it does not configure another mod’s raid system. |
| `RaidItemClassname` | string | `"Lockpick"` | An existing `CfgVehicles` classname. | Exact classname required in the raider’s hands. A subclass is not automatically accepted. An empty or unknown classname disables VBI raids. |
| `RaidDurationSeconds` | number | `300.0` | `10`–`3600` seconds | Continuous raid action duration: default 5 minutes. A higher value means a longer raid. |
| `RaidToolDamagePercent` | number | `25.0` | `0`–`100` percent | Health removed after a successful raid, measured against the tool’s **maximum** health. Used when the tool is not consumed. `0` removes this configured damage. |
| `ConsumeRaidItem` | boolean | `false` | `true` / `false` | `true` deletes the tool after a successful raid. This takes precedence over `RaidToolDamagePercent`. |
| `DismantleDurationSeconds` | number | `10.0` | `1`–`300` seconds | Time needed for the supported dismantle-to-kit action. It does not change raid duration or remove dismantling requirements. |

See [raiding and CodeLocks](Raiding-and-CodeLocks.md) for action requirements and examples.

## Solar generation and consumption

Energy rates are **DayZ energy units per real elapsed second**, not watts or battery percentage points.

| Setting | Type | Default | Runtime limit | Effect |
| --- | --- | --- | --- | --- |
| `SolarEnergyPerSecond` | number | `0.15` | `0`–`10` | Peak generation rate. Output follows the daylight curve, so this is not a constant all-day rate. `0` disables generation while stored battery energy can still supply consumers. |
| `SolarSunriseHour` | number | `6.0` | `0`–`23` | Start of the generation window in in-game hours. Generation is zero at the boundary. `6.5` means 06:30. |
| `SolarSunsetHour` | number | `18.0` | Sunrise + `0.1`, up to `24` | End of the generation window. Must be later than sunrise; overnight windows are not supported. Generation is zero at the boundary. |
| `WallLampEnergyPerSecond` | number | `0.005` | `0.0001`–`10` | Consumption of `VBI_WallLamp`. Higher values drain the source faster. |
| `FloorLampEnergyPerSecond` | number | `0.01` | `0.0001`–`10` | Consumption of `VBI_FloorLamp`. |
| `FridgeEnergyPerSecond` | number | `0.02` | `0.0001`–`10` | Consumption of `VBI_Fridge`. |

The lamp keys above do not provide a global multiplier for every light. The table lamp and both floodlight classes retain their item-configured consumption in the referenced implementation. AC consumption has its own settings.

See [power and refrigeration](Power-and-Refrigeration.md) for the generation curve, battery behavior and appliance mapping.

## Refrigeration

| Setting | Type | Default | Runtime limit | Effect |
| --- | --- | --- | --- | --- |
| `FridgeTemperatureC` | number | `4.0` | `0`–`10` °C | Target used for temperature interpolation of eligible contents while the refrigerator is cooling. The value is approached over time. |
| `FridgeDecayMultiplier` | number | `0.2` | `0`–`1` | Multiplies food decay progression while cooling. `0.2` means 20% of normal progression; `1` means normal progression; `0` stops this decay progression while the effect applies. |

Cooling requires a non-ruined refrigerator with working power and a closed door. Opening it or losing power removes these VBI cooling benefits. This file does not configure the camping cooler or AC target temperature.

## Waste bin

| Setting | Type | Default | Loaded limit | Effect in the referenced implementation |
| --- | --- | --- | --- | --- |
| `TrashConfirmSeconds` | number | `10.0` | `3`–`60` seconds | Present in the schema and clamped by the loader, but **not used by the empty-bin action**. That action uses a fixed 5-second hold with a 4.8-second server guard. Changing this field alone does not change that hold time. |

This distinction documents actual behavior rather than promising an effect from the field name. See [reference scope](Reference-Scope.md) if your release has changed the action.

## Attachment display

| Setting | Type | Default | Effect |
| --- | --- | --- | --- |
| `ShowAttachmentsByDefault` | boolean | `true` | Fallback visibility for classes with no matching display rule. Explicit class rules take precedence. |
| `AttachmentDisplay` | array of objects | Built-in class rules, initially `true` | Per-class display rules. Keep the generated list and edit selected entries. |
| `AttachmentDisplay[].ItemClassname` | string | Class-specific | Effective settings classname of the **placed object**, not its kit. Matching is case-insensitive; some variants share a base settings classname. |
| `AttachmentDisplay[].ShowAttachments` | boolean | `true` | Whether normal attachment models on that class may be displayed. It does not change slots, capacity or permissions. |

For duplicate class rules, the last entry wins. Built-in rules are replenished when missing, so a global `false` alone will not hide classes whose explicit rule is still `true`.

See [attachment visibility](Attachment-Visibility.md) for color variants and complete examples.

## Runtime limits and invalid values

Values outside numeric limits are clamped when loaded. Keep your JSON within those limits; the on-disk text is not guaranteed to be rewritten to show the effective value. For booleans, strings and arrays, use the correct JSON types rather than relying on conversion.

Invalid main JSON, unsupported schema versions and invalid raid classnames disable VBI raiding. Consult the startup log and [troubleshooting](Troubleshooting.md) instead of assuming a successful server start proves the configuration was accepted.
