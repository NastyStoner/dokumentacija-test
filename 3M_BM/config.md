---
title : Config
parent: Blackmarket
nav_order: 1
---

# Configuration

All configuration is done in **`config.lua`**.

```lua
Config = {}
Config.Framework = 'auto'   -- 'auto' | 'esx' | 'qbcore'
Config.Inventory = 'auto'   -- 'auto' | 'ox' | 'qb'
Config.Target    = 'auto'   -- 'auto' | 'ox' | 'qb'
```

---

## Framework & Inventory

### `Config.Framework`

Controls which framework bridge is used.

- `'auto'` – detects ESX or QBCore based on running resources.
- `'esx'`  – force ESX.
- `'qbcore'` – force QBCore.

The bridge provides helpers like:

- `Framework.GetIdentifier(src)`
- `Framework.GetBlackMoney(src)`
- `Framework.RemoveBlackMoney(src, amount, reason)`
- `Framework.AddBlackMoney(src, amount, reason)`


### `Config.Inventory`

Controls which inventory integration is used.

- `'auto'` – tries to detect ox_inventory or qb-inventory.
- `'ox'`   – force ox_inventory.
- `'qb'`   – force qb-inventory.

The bridge exposes:

- `Bridge.Inventory.RegisterStash(stashId, label, slots, weight, ownerIdentifier, extra, coords)`
- `Bridge.Inventory.AddItem(stashId, item, amount, metadata, slot)`
- `Bridge.Inventory.OpenStash(src, stashId, label, slots, weight)`
- `Bridge.Inventory.ClearStash(stashId)`
- `Bridge.Inventory.RemoveStash(stashId)`
- `Bridge.Inventory.CanWatchStash()` and `Bridge.Inventory.IsStashEmpty(stashId)` (optional, for auto-cleanup)

Make sure your inventory resource is started before the blackmarket.

### `Config.Target`

Controls which target resource is used for drop points and NPC interaction.

- `'auto'` – detect ox_target or qb-target.
- `'ox'`   – force ox_target.
- `'qb'`   – force qb-target.

The bridge provides:

- `Bridge.Target.AddDropPoint(index, coords, heading, label, icon, distance, callback)`
- `Bridge.Target.AddNpcTarget(ped, label, icon, distance, callback)`

---

## Black money account

```lua
Config.BlackMoneyAccount = {
    esx    = 'black_money',
    qbcore = 'cash',
}
```

- For **ESX**, the default is usually `'black_money'`.
- For **QBCore**, you can point this to whichever account represents illegal money in your economy (e.g. `'black_money'`, `'markedbills'` total, etc.), depending on your framework setup.

The bridge uses this map to implement `Framework.GetBlackMoney` and related operations.

---

## Tablet app (lb-tablet)

```lua
Config.LBTablet   = false
Config.Identifier = "blackmarket-app"
Config.DefaultApp = false
Config.Name       = "Blackmarket"
Config.Description = "Crno trziste."
Config.Developer   = "Matic"
```

- Set `Config.LBTablet = true` to enable **lb-tablet** mode.
- When enabled, the UI is loaded as a tablet app with:
  - Identifier: `Config.Identifier`
  - Display name: `Config.Name`
  - Description: `Config.Description`
  - App developer string: `Config.Developer`
  - `Config.DefaultApp` decides whether it appears pinned by default in the tablet.

When `Config.LBTablet = false`, the script runs in **standalone NUI** mode and uses:

- `Config.CommandStandalone` to open (`/blackmarket` by default).

---

## Locale & notifications

```lua
Config.Locale      = 'en'    -- 'en', 'hr', ...
Config.NotifyType  = 'okok'  -- 'esx', 'okok', 'brutal', 'ox'
Config.AdminGroup  = 'owner'
Config.CommandStandalone = 'blackmarket'
```

- `Config.Locale` selects which file from `locales/` is loaded on the client.  
  Add your own `locales/xx.lua` to support more languages.
- `Config.NotifyType` controls which notification provider is used by `NotifyPlayer`:
  - `'esx'`    – use ESX built-in notifications.
  - `'okok'`   – use `okokNotify`.
  - `'brutal'` – custom notification system (example).
  - `'ox'`     – use `ox_lib` notifications.
- `Config.AdminGroup` is used in ESX permissions to decide who can run admin commands.
  For QBCore the implementation is usually permission-based.

---

## Drop points & delivery

```lua
Config.DropPoints = {
    { x = -594.2508, y = -1144.3292, z = 22.1322, w = 359.020},
    { x = 443.6479,  y = -1491.7626, z = 29.3004, w = 19.6664},
    { x = 450.9764,  y = -767.7982,  z = 27.3578, w = 275.1093}
}
```

- Each entry defines a potential **drop location** for orders.
- `x, y, z` – coordinates.
- `w` – heading (optional, can be used by your own props if you spawn them).

A random drop point is chosen for each order when checking out.

### Delivery behaviour

```lua
Config.Delivery = {
    UseExpiry     = true,
    ExpiryMinutes = 30,
    StashSlots    = 80,
    StashWeight   = 200000,
    Label         = 'Black Market Paket',
    SetWaypoint   = true,
    ShowBlip      = true,
}
```

- `UseExpiry` – enable or disable expiry logic.
- `ExpiryMinutes` – how long the order is valid.
  - Pre-open expiry: if the stash is never opened, it will be removed and the order marked as expired.
  - Post-open expiry: once the stash is opened, a countdown starts; after the timer, it will be cleared & removed if still active.
- `StashSlots` & `StashWeight` – capacity of the stash in your inventory system.
- `Label` – visual label (stash name).
- `SetWaypoint` – whether to automatically place a waypoint on the player’s map.
- `ShowBlip` – whether to show a temporary blip around the drop.

---

## Auto restock & stock persistence

```lua
Config.AutoRestock = {
    Mode = 'daily' -- 'restart', 'daily', 'weekly', 'monthly', 'none'
}
```

- `Mode = 'restart'`
  - Restocks once on server/resource restart.
- `Mode = 'daily'`, `'weekly'`, `'monthly'`
  - Starts a background loop that checks elapsed time since last restock and refills items to `maxStock`.
- `Mode = 'none'`
  - No automatic restock; stock only changes on purchase or admin commands.

Stock itself is stored in:

- Memory (runtime `ItemsIndex`)
- `blackmarket_stock.json` in the resource folder
- MySQL `bm_items` table (for analytics / persistence)

---

## NPC configuration

```lua
Config.NPC = {
    Enabled     = true,
    Model       = 'csb_undercover',
    Coords      = { x = 978.3830, y = 2953.7490, z = 43.2853, w = 5.8851 },
    Scenario    = 'WORLD_HUMAN_STAND_IMPATIENT',
    Distance    = 2.0,
    TargetIcon  = 'fas fa-gun',
    TargetLabel = 'Otvori Blackmarket'
}
```

- `Enabled` – toggle NPC entirely.
- `Model` – ped model name (string).
- `Coords` – location & heading of the NPC.
- `Scenario` – optional scenario to play (idle animation).
- `Distance` – target interaction range.
- `TargetIcon` – icon shown by target resource.
- `TargetLabel` – text shown in the target menu.

Players can talk to this NPC to open the Blackmarket UI.

---

## Items configuration

Items are defined as a map under `Config.Items`:

```lua
Config.Items = {
    ['WEAPON_APPISTOL'] = {
        label    = "AP Pistolj",
        price    = 100000,
        image    = "WEAPON_APPISTOL.png",
        stock    = 50,
        maxStock = 50,
        category = "pistols"
    },
    -- ...
}
```

Each item supports:

- `label` – name shown in the UI.
- `price` – price per unit (in **black money**).
- `image` – file name for icon (relative to `ui/dist/images` or your chosen folder).
- `stock` – initial stock on first sync.
- `maxStock` – cap for restocks.
- `category` – one of:
  - `"pistols"`, `"smg"`, `"rifles"`, `"ammo"`, `"melee"`, `"misc"`

You may also use human-readable categories and let the script normalize them, e.g. `"pistolji"`, `"municija"`, `"puske"` are converted to internal categories.

After editing `Config.Items`, run:

```text
/syncblackitems
```

to rebuild the index and sync everything to DB.
