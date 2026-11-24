# Troubleshooting

[Home](Home.md) · [Main settings](Main-Settings.md) · [Air conditioners](Air-Conditioners.md)

## My changes do not apply

1. Confirm the server’s actual `-profiles` directory.
2. Check the exact file name and the two different VBI folder names.
3. Stop the server, edit the file and validate the JSON.
4. Restart and rejoin; settings are cached, not hot-reloaded.
5. Check the startup script log for parsing, schema and classname messages.
6. Check whether the value is being clamped or overridden by a class-specific display rule.

Main settings belong in `Voids_Base_Items/settings.json`; AC settings belong in `VoidsBaseItems/AirConditioners.json`. Editing one does not configure the other.

## Main JSON errors

| Log fragment | Meaning and next step |
| --- | --- |
| `Invalid settings.json; raids disabled. File preserved.` | The main JSON could not be parsed. Fix JSON syntax/types in the preserved file or restore your backup. VBI raids are disabled for that run. |
| `Unsupported settings ConfigVersion; raids disabled.` | Keep the documented main schema at `2`; it is not the Workshop version number. |
| `Unknown RaidItemClassname; raids disabled:` | Use a real loaded `CfgVehicles` classname, not a display name or an unavailable mod item. |
| `Cannot create settings.json:` | Check the profiles path, directory existence and write permissions. |
| `Legacy settings import rejected` | The old `PB_BaseStorage` file failed validation. The original is preserved; correct it before expecting an import. |
| `Duplicate AttachmentDisplay entry; last entry wins:` | Remove or reconcile duplicate rules so the effective value is clear. |
| `Ignoring empty AttachmentDisplay entry.` | Fill in a valid effective placed-object classname or remove the empty entry. |

On invalid main JSON, the loader retains the file and disables VBI raids. Do not assume your custom non-raid values were loaded; defaults remain in use for that run. Fix the file and restart.

## AC JSON errors

`Cannot read AirConditioners.json; using defaults without changing the file.` means the AC file could not be loaded. Correct the JSON and restart. Existing malformed text is preserved; the units use defaults for that run.

The main and AC files have different schema markers: main `2`, AC `1`.

## No raid action

Check that raids are enabled, the configured tool’s exact classname matches the item in hand, the tool is not ruined, and the target is closed and locked with one recognized lock. Check startup detection and relevant client/server mods. Another mod’s raid action is independent of `RaidEnabled`.

## No dismantle action

For the documented generic action, use a screwdriver, open the eligible storage, empty its cargo, remove all attachments and remove its lock. Use the required crouched stance. A duration setting does not bypass those conditions. Loose crates/decorations use separate movement rules.

## Lock integration unavailable

Check the original lock mod, dependencies, load order and matching server/client versions. Search the log for `Lock integration ACTIVE:` or `Lock integration unavailable:`. Optional absent providers are expected when you do not use them. Do not add a nonexistent provider-priority field to the main JSON.

## Attachments are still visible, or unexpectedly hidden

- An explicit per-class rule overrides the global default.
- Built-in missing rules can be added back as true.
- Duplicate rules use the last value.
- Colored single/triple lockers use their shared base settings classname.
- Use a placed classname, not `_Kit`.
- Closed-storage hiding can additionally apply.
- Display settings cannot repair a missing proxy, reposition a weapon or create a missing slot.

See [attachment visibility](Attachment-Visibility.md) for working examples.

## Solar panel does not charge

Check for a working attached battery, clear sky above the check point, a positive peak rate, an in-game time strictly inside the configured window and a non-night game state. A full battery has no spare capacity. With consumers running, generation may be lower than their combined drain, so net charge can still fall during daylight.

## Refrigerator does not preserve contents

Check actual upstream power, that the fridge is not ruined and that the door is closed. `FridgeDecayMultiplier: 1` deliberately leaves decay at normal progression. Existing spoiled food is not repaired. The camping cooler is not controlled by these settings.

## AC has no display, sound or cooling

Confirm that the unit is placed, not ruined, switched on and actually supplied with energy. Both switch and supply are required. Check cable reach, source charge and the correct `Wall`/`Floor` group. The display shows the configured target; it is not a current-room-temperature sensor.

Cooling does not warm air already below the target, and body temperature still depends on the normal simulation. Vehicles and water contact are excluded. Overlapping units use the coldest applicable target rather than stacking temperature reductions.

## Waste-bin timing does not follow TrashConfirmSeconds

In the referenced implementation this field is loaded but unused by the action. The hold is fixed at 5 seconds with a 4.8-second server guard. Do not treat a changed JSON number as proof of changed action timing. See [reference scope](Reference-Scope.md).

## Missing actions or untranslated STR_VBI names

Confirm that server and clients use the same complete mod build, including the common scripts and localization data. A configuration change does not repair missing runtime files. Check the current server script log and RPT before changing persistence data.

## Reporting a problem

Send Discord `_void_______` or open a [documentation issue](https://github.com/White1Kyurem/Voids-Base-items/issues). Include the mod version, affected classname, exact setting, relevant JSON section, expected/actual behavior and reproduction steps. Add the startup RPT/script-log excerpts when relevant.

For CodeLock compatibility requests, include the exact Workshop link to the lock mod. Remove private server paths, player identifiers, passwords and unrelated logs before posting publicly.
