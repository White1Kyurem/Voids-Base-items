# Void’s Base Items — Server Owner Wiki

Use this guide to install VBI, understand its server settings and configure storage, raiding, electricity and air conditioning.

| I want to… | Read |
| --- | --- |
| Find my settings and apply changes | [Getting started](Getting-Started.md) |
| Understand every main JSON field, default and limit | [Main settings reference](Main-Settings.md) |
| Configure raids, dismantling or supported locks | [Raiding and CodeLocks](Raiding-and-CodeLocks.md) |
| Balance solar generation, lamps and refrigerators | [Power and refrigeration](Power-and-Refrigeration.md) |
| Hide displayed equipment on selected storage | [Attachment visibility](Attachment-Visibility.md) |
| Set AC temperature, range and energy use | [Air conditioners](Air-Conditioners.md) |
| Copy a complete valid configuration or a focused example | [Configuration examples](Configuration-Examples.md) |
| Look up an attachment display classname | [Display classnames](Display-Classnames.md) |
| Diagnose a setting that does not seem to work | [Troubleshooting](Troubleshooting.md) |

## Two configuration files

Both paths are relative to the server’s active **profiles folder**, selected by its `-profiles` launch argument:

| File | Controls |
| --- | --- |
| `Voids_Base_Items/settings.json` | Main VBI settings: raids, dismantling, solar generation, supported appliance consumption, refrigeration and attachment display. |
| `VoidsBaseItems/AirConditioners.json` | Separate wall/portable AC cooling radius, target temperature and energy consumption. |

The folder names differ: the main folder uses underscores; the AC folder does not. These are two separate files.

**Stop the server, back up the existing JSON, edit it, validate it and restart.** Settings are cached; saving a file while the server is running does not reload it.

The reference covers main schema **`ConfigVersion: 2`** and AC schema **`ConfigVersion: 1`**. See [reference scope](Reference-Scope.md) before applying examples to a different build.

## Official release and contact

- [Void’s Base Items on Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3830491338)
- Discord: `_void_______`
- [Documentation issues](https://github.com/White1Kyurem/Voids-Base-items/issues)

For additional CodeLock support, include the exact Workshop link to the lock mod. For bugs, provide the item classname, reproduction steps and relevant logs.
