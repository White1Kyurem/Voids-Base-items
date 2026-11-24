# Attachment visibility

[Home](Home.md) · [Classname lookup](Display-Classnames.md) · [Examples](Configuration-Examples.md)

**File:** `<profiles>/Voids_Base_Items/settings.json`  
**Apply changes:** restart the server.

These settings control the visible models of normal attached equipment. They preserve the items, attachment slots, inventory capacity and access permissions. Cargo items and geometry built into a furniture model are not created or removed by these switches.

## How a rule is selected

1. VBI finds the placed object’s effective settings classname. Normally this is its classname; some variants explicitly use a shared base classname.
2. A matching `AttachmentDisplay` entry takes precedence over `ShowAttachmentsByDefault`.
3. Class matching is case-insensitive. If a class appears more than once, the **last entry wins**.
4. If no rule matches, the global default is used.
5. A cabinet’s existing hide-when-closed behavior can additionally hide attachments. Setting visibility to `true` does not force them through a closed door.

CodeLock model visibility remains handled by the existing lock design. This switch does not show a lock that is otherwise inventory-only.

## Hide the tool wall while showing weapons on the gun rack

Edit the corresponding entries in the existing array:

```json
{
  "ShowAttachmentsByDefault": true,
  "AttachmentDisplay": [
    { "ItemClassname": "VBI_ToolWall", "ShowAttachments": false },
    { "ItemClassname": "VBI_GunRack", "ShowAttachments": true },
    { "ItemClassname": "VBI_GunWall", "ShowAttachments": true }
  ]
}
```

This is a focused example. Preserve your other settings and rules when applying it. Setting `false` hides the displayed equipment; it does not disable the wall or destroy stored tools.

## Why does a global false not hide everything?

A newly generated file contains explicit `true` rules for built-in item classes. Those rules override the global fallback. VBI also replenishes missing built-in rules, initially as `true`.

To hide attachments on all generated classes, set `ShowAttachmentsByDefault` to `false` **and set the existing built-in `ShowAttachments` values to `false`**. Do not rely on replacing the array with an empty array: missing built-in rules can be added again.

## Colored lockers share settings

Colored single lockers use `VBI_SettingsClass = "VBI_Locker"`. Colored triple lockers use `VBI_SettingsClass = "VBI_TripleLocker"`.

| Placed example | Rule to edit |
| --- | --- |
| `VBI_Locker_Red` or `VBI_Locker_Black` | `VBI_Locker` |
| `VBI_TripleLocker_Green` or `VBI_TripleLocker_Violet` | `VBI_TripleLocker` |

Adding a `VBI_Locker_Red` rule does not override that shared lookup in this implementation. Independent visibility for each locker color would require a mod change, not another JSON entry.

Use the **placed item’s effective class**, never an `_Kit` classname. The [classname reference](Display-Classnames.md) lists generated base rules.

## Limits of the setting

- It does not move or rotate a proxy.
- It does not create a missing model proxy or equipment slot.
- It does not change cargo dimensions or compatibility rules.
- It does not make decorative parts into functional attachments.
- It does not select individual slots within one item; the rule applies to that class’s normal displayed attachments.

If equipment remains invisible with its rule set to `true`, check that the storage is open where required, the item uses a supported slot and server/client mod versions match.
