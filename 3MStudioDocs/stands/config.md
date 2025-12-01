---
title: Configuration
parent: stands
nav_order: 2
---


# Configuration

All options live in `standovi/config.lua`.

```lua
Config = {}

-- Purchase & refund
Config.ShopPrice = 2000000  -- price to buy an unowned stand
Config.SellPrice = 1000000  -- refund when selling back to city

-- Items control
Config.BlacklistItems = { 'money' }  -- disallow selling these items
Config.BlockWeapons = true           -- block any item that starts with 'WEAPON_'

-- Notifications: 'okok' uses okokNotify, 'esx' uses ESX.ShowNotification
Config.Notify = 'okok'

-- Locale: 'en' or 'hr'
Config.Locale = 'en'

-- Target interaction label (visible on ox_target)
Config.TargetLabel = 'Open Stand'

-- Define all stands here (id must be unique)
Config.Stands = {
    { id = 1, coords = vector3(208.78, -3025.84, 5.87), heading = 278.75, targetCoords = vector3(208.78, -3025.84, 5.87) },
    { id = 2, coords = vector3(209.44, -3042.27, 5.83), heading = 266.02, targetCoords = vector3(209.44, -3042.27, 5.83) },
}
```

## Fields

### `ShopPrice`
Price (cash) required to purchase an unowned stand.

### `SellPrice`
Refund (cash) when selling the stand back to the city.

### `BlacklistItems`
Array of item names that cannot be stocked or sold. Use this to block currency-like items or anything you don't want on stands.

### `BlockWeapons`
When `true`, any item whose name starts with `WEAPON_` will be rejected server-side.

### `Notify`
- `okok` → uses `okokNotify`
- `esx` → uses `ESX.ShowNotification`

### `Locale`
Two locales are included: `en`, `hr`. Duplicate the file in `locales/` to add more.

### `TargetLabel`
Text for the ox_target prompt.

### `Stands`
List of placed stands:
- `id` — unique integer identifier
- `coords` — where the prop spawns
- `heading` — stand heading
- `targetCoords` — where the target sphere is centered
