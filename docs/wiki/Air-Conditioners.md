# Wall and portable air conditioners

[Home](Home.md) · [Power guide](Power-and-Refrigeration.md) · [JSON examples](Configuration-Examples.md)

**File:** `<profiles>/VoidsBaseItems/AirConditioners.json`  
**Schema marker:** `ConfigVersion: 1`  
**Apply changes:** restart the server.

The wall unit and portable floor unit share the same operating rules, with separate server configuration groups. `Wall` affects all wall units; `Floor` affects all portable units. The JSON does not provide per-instance settings.

## Complete default configuration

```json
{
  "ConfigVersion": 1,
  "Wall": {
    "RadiusMeters": 6.0,
    "TargetTemperatureC": 20,
    "EnergyUsagePerSecond": 0.05
  },
  "Floor": {
    "RadiusMeters": 4.0,
    "TargetTemperatureC": 20,
    "EnergyUsagePerSecond": 0.04
  }
}
```

## Every setting explained

| Field | Type | Wall default | Floor default | Runtime limit | Effect |
| --- | --- | --- | --- | --- | --- |
| `ConfigVersion` | integer | `1` (root field) | `1` (same root field) | Keep at `1` | Schema marker; it is not the mod release number or temperature. |
| `Wall` / `Floor` | object | Wall settings | Portable settings | Preserve the object structure | Selects the group of units being configured. |
| `RadiusMeters` | number | `6.0` | `4.0` | `0.5`–`50` meters | Radius of the cooling sphere centered at the outlet. It is a radius, not a diameter. Higher values cover a larger area. |
| `TargetTemperatureC` | integer | `20` | `20` | `14`–`25` °C | Target used by cooling and shown on the display. Use a whole number because it selects a matching temperature texture. |
| `EnergyUsagePerSecond` | number | `0.05` | `0.04` | `0.001`–`10` | DayZ energy consumed per real second. Higher values drain the source faster; zero is clamped to the positive minimum. |

Out-of-range numeric values are limited in memory. Existing AC files are not rewritten merely because a value was clamped. If JSON cannot be read, the loader logs an error, preserves the file and uses defaults for that run.

## Power and controls

Connect a placed unit to a compatible powered 220-V-style DayZ energy-system connection, for example through a cable reel. Each unit has a 10-meter connection cable. This is the game’s connection type; the consumption values above are not real-world watts.

Use the unit’s on/off action. A connection alone does not run it: the upstream source must provide energy and the unit must be switched on.

| Switch | Actual supply | Cooling | Display | Sound and outlet flaps |
| --- | --- | --- | --- | --- |
| Off | No | Off | Dark | Sound off, flaps closed |
| Off | Yes | Off | Dark | Sound off, flaps closed |
| On | No | Off | Dark | Sound off, flaps closed |
| On | Yes | Active | Configured target | Looping sound, flaps open |

If a unit remains switched on during a supply failure, it resumes operation when usable power returns. Switching off, unplugging or ruining it ends operation. Taking it into hands/inventory also stops it; carried units do not create cooling zones. Supply changes are checked at half-second intervals in addition to energy events; other clients follow normal network synchronization.

## Display and sound

Both models have VOID branding, animated outlet flaps and their own continuous spatial fan/compressor sound. The active display shows the **configured target**, not a measured current room temperature.

Example: set `Wall.TargetTemperatureC` to `18`; running wall units show **18°C** and use that same target for cooling. The display is dark while inactive.

## What cooling actually changes

Inside an active unit’s radius, VBI caps the local air-temperature value used by the player’s normal DayZ environment simulation at the target:

| Ambient value | Target | Value used in the affected calculation |
| --- | --- | --- |
| 38°C | 20°C | 20°C |
| 24°C | 18°C | 18°C |
| 12°C | 20°C | 12°C |

The units cool only; they do not heat colder air. Player body temperature continues responding through the normal simulation, including clothing and movement. It is not directly assigned the display’s temperature.

The zone is spherical and works **through walls within the configured radius**. No room seal or building scan is required. The check samples a point 0.9 meters above the player position, relative to the unit’s outlet.

When zones overlap, the lowest applicable target is used. Extra units do not stack additional temperature subtraction. Players in vehicles or with water contact are excluded from this additional cooling. The system does not change global weather or actively refrigerate ground items.

## Example: larger, cooler wall units

```json
{
  "ConfigVersion": 1,
  "Wall": {
    "RadiusMeters": 8.0,
    "TargetTemperatureC": 18,
    "EnergyUsagePerSecond": 0.07
  },
  "Floor": {
    "RadiusMeters": 4.0,
    "TargetTemperatureC": 20,
    "EnergyUsagePerSecond": 0.04
  }
}
```

This changes every wall unit to an 8-meter radius, an 18°C target and a 0.07 consumption rate. Portable units keep their defaults. Restart after saving.
