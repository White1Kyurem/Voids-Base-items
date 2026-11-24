# Installation and configuration files

[Home](Home.md) · [Main settings](Main-Settings.md) · [Examples](Configuration-Examples.md)

## Install and generate the files

1. Install the original [VBI Workshop release](https://steamcommunity.com/sharedfiles/filedetails/?id=3830491338) on both the server and clients. Use matching versions.
2. If using a supported CodeLock mod, install its dependencies and load the original lock mod before VBI. See [CodeLock integration](Raiding-and-CodeLocks.md#automatic-codelock-integration).
3. Start the server once. The mod creates missing configuration files in the active profiles folder.
4. Stop the server before making configuration changes.
5. Back up the generated files, edit only the desired values, validate the JSON and restart.

The GitHub documentation is not a replacement for installing the Workshop mod.

## Find the profiles folder

Use the directory passed to the server’s `-profiles` launch argument. A hosting panel may label this directory “profiles” or provide a separate file-manager location.

For example, with `-profiles=C:\DayZServer\profiles`, the configuration paths are:

```text
C:\DayZServer\profiles\Voids_Base_Items\settings.json
C:\DayZServer\profiles\VoidsBaseItems\AirConditioners.json
```

Do not edit a client profiles folder or an old server instance’s files. You do not need to unpack or modify a PBO to change these settings.

## JSON editing rules

- Keep setting names and nesting exactly as documented.
- Write booleans as `true` or `false`, without quotes.
- Use a decimal point: `0.15`, not `0,15`.
- Put classnames in double quotes.
- Separate entries with commas, with no trailing comma after the last entry.
- JSON does not support comments. The supplied example files contain valid JSON without comments.
- Keep existing fields and attachment rules when changing one setting.

All durations and energy rates use seconds. Solar sunrise/sunset values use **in-game hours**. They are independent of your computer’s local time.

## When changes take effect

Restart the server after editing either file. Main settings are loaded once and sent to clients through VBI’s settings synchronization. AC settings are also loaded once; each AC receives the applicable values on initialization.

Rejoin after the restart when checking client-side actions and displays. Existing placed objects read their settings again when loaded by the server. No character wipe is required for a normal configuration change.

## First creation and upgrades

The main file uses `ConfigVersion: 2`. A recognized version-1 file is upgraded with its loaded values retained and a uniquely named `.v1.bak` backup. Missing built-in attachment display rules can also be added, with a `.before-v1.bak` backup. Therefore, deliberately omitting a built-in display rule is not a reliable way to disable it; set its boolean explicitly.

If the current main file is absent, VBI can import a recognized legacy `PB_BaseStorage/settings.json`. The old file is preserved. An invalid legacy file is rejected instead of being silently replaced.

The AC file is created if missing. Existing AC JSON is read without being rewritten on each restart. See the separate failure behavior in [troubleshooting](Troubleshooting.md).
