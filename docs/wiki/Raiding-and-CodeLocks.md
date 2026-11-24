# Raiding, dismantling and CodeLocks

[Home](Home.md) · [All settings](Main-Settings.md) · [Examples](Configuration-Examples.md)

## Automatic CodeLock integration

VBI checks loaded addon classes at startup and enables supported integrations. Install the desired original lock mod and its dependencies, load it before VBI, and use the same relevant mods on server and clients.

| System | Integration |
| --- | --- |
| Code Lock by Room Service | Uses its CodeLock item, code entry and permissions. |
| DayZ Expansion BaseBuilding Code Lock | Uses Expansion’s code entry and lock handling. |
| LBmaster Better Codelock | Uses LBmaster’s extension of the digital lock and its access/management permissions. |
| Vanilla digital lock | Available when the game provides `DigitalCodeLock` and the matching slot is offered. This does not mean the mechanical three-/four-dial combination locks. |

Detection uses internal loaded mod/config classes. Renaming an `@mod` folder does not select a provider. The installed item and its provider determine code entry, management and access.

The documented main JSON has **no manual CodeLock enable switch or provider-priority field**. Do not add invented settings. Where a foreign lock system is present, the new vanilla digital slot can be hidden; an existing digital lock keeps its applicable provider. LBmaster is treated as the digital-lock extension.

VBI supports a maximum of **one installed lock per storage object** in this implementation. Multiple animated doors or drawers do not imply separate independent locks.

### Check detection in the log

Search the server script log for:

```text
Startup lock detection cached
Lock integration ACTIVE:
Lock integration unavailable:
Optional lock mod absent:
```

“Optional lock mod absent” is informational when you do not use that provider. “Integration unavailable” means an expected dependency, config class or compiled integration is missing. Check the original lock mod, dependencies, load order and matching client/server installation.

## VBI raid settings

| Setting | Default | Purpose |
| --- | --- | --- |
| `RaidEnabled` | `true` | Enables the VBI raid action. |
| `RaidItemClassname` | `"Lockpick"` | Exact tool classname. |
| `RaidDurationSeconds` | `300.0` | 5-minute action; range 10–3600 seconds. |
| `RaidToolDamagePercent` | `25.0` | Removes 25% of maximum tool health after success. |
| `ConsumeRaidItem` | `false` | If enabled, deletes the tool after success instead of applying the configured health loss. |

A raid requires eligible storage that is closed and locked, exactly one recognized lock, a living player in reach, and the non-ruined configured tool in the player’s hands. The action rechecks its conditions while running. Tool consumption/damage occurs after successful provider-side raid handling.

If a tool has 100 maximum health, `25` removes 25 health after success. It is not 25% of the tool’s remaining health. A partially damaged tool may therefore be ruined by a successful raid.

To disable VBI raids, edit the existing file:

```json
{
  "RaidEnabled": false
}
```

This is a **field-change example**, not a complete replacement configuration. VBI does not use this value to disable another mod’s separate raid actions.

For a ten-minute raid that consumes its tool:

```json
{
  "RaidEnabled": true,
  "RaidItemClassname": "Lockpick",
  "RaidDurationSeconds": 600.0,
  "ConsumeRaidItem": true
}
```

## Dismantling is separate from raiding

`DismantleDurationSeconds` defaults to `10.0`, with a 1–300 second range. The documented supported dismantle action requires a screwdriver, open and empty storage, no installed lock and no remaining attachments. The player must be in reach and use the action’s required crouched stance.

On success, eligible storage is replaced by its matching kit, carrying over its health fraction. Loose crates and directly carried decorations use their own movement/placement behavior instead of this generic dismantling path.

Changing the duration does not bypass these requirements or create a kit for an object that does not support one.
