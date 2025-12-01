---
title: Configuration
parent: 3M_Dealership
nav_order: 2
---


# Configuration

All configuration is done in **`config.lua`**. This page explains the most important sections and how they affect gameplay.

---

## Framework & core settings

```lua
Config.Framework   = 'esx'   -- 'esx' or 'qb' / 'qbcore'
Config.Locale      = 'en'    -- 'en' or 'hr'
Config.Currency    = 'bank'  -- which account is charged (ESX/QB)
Config.UseNUIOnly  = true    -- true = full NUI flow, false = also show helptext
Config.UseOxTarget = true    -- use ox_target for interactions
```

- **Framework** – tells the script which framework helpers to prefer. Actual detection also happens in `server/utils.lua`.
- **Locale** – which dictionary from `locales/*.lua` to use (`en.lua` / `hr.lua`).
- **Currency** – which money account is used for vehicle purchases.
- **UseNUIOnly** – if `true`, the script relies on NUI prompts instead of 3D helptext.
- **UseOxTarget** – if `true`, spawns ox_target zones around NPCs and vehicles.

---

## Webhooks

```lua
Config.Webhooks = {
  Purchase = '',
  Finance  = '',
  Repo     = '',
  Consign  = ''
}
```

These are optional webhook URLs (e.g. Discord) that receive events when:

- A vehicle is purchased.
- A finance contract is created or paid.
- A vehicle is repossessed.
- A vehicle is consigned or sold from the used lot.

If you do not want webhooks, leave them empty.

---

## Notification bridge

```lua
Config.Notification = {
  provider = 'okok',          -- 'ox', 'esx', 'okok', 'brutal'
  title    = '3M Dealership',
}
```

The script never calls notification exports directly. Instead, it:

1. Triggers `3m:notify` with a key (e.g. `purchased_ok`).
2. Uses `client/notify.lua` to map keys to localized text and dispatch to the configured provider.

Supported providers out of the box:

- `ox` – `ox_lib` notifications.
- `esx` – `ESX.ShowNotification`.
- `okok` – `okokNotify`.
- `brutal` – placeholder for your own notification export.

You can also use the exported `Notify` function from `client/notify.lua` and `server/notify.lua` inside your own scripts if you wish to unify notifications.

---

## Keys system

```lua
Config.Keys = {
  System = 'wasabi', -- 'wasabi' | 'qs' | 'custom'
  GiveKeyExport = {
    wasabi = { resource = 'wasabi_carlock', export = 'GiveKey' },
    qs     = { resource = 'qs-vehiclekeys', export = 'GiveKeys' },
    custom = { resource = '', export = '' }
  },
  RemoveKeyExport = {
    wasabi = { resource = 'wasabi_carlock', export = 'RemoveKey' },
    qs     = { resource = 'qs-vehiclekeys', export = 'RemoveKeys' },
    custom = { resource = '', export = '' }
  }
}
```

- **System** – which key system you are using.
- **GiveKeyExport / RemoveKeyExport** – how to call that system to give/remove keys.

This configuration is used for:

- Delivering keys after purchase.
- Issuing test drive keys.
- Handling repossession and consignment transfers.

If you have a custom keys resource, set `System = 'custom'` and fill in the export info.

---

## Dealerships (multiple locations)

```lua
Config.Dealerships = {
  {
    id       = 'town',
    label    = 'Town Cardealer',
    pedModel = 'a_m_y_bevhills_01',

    salesman = vec4(-50.99, -1099.96, 25.42, 338.55),
    keyNpc   = vec4(-56.39, -1098.71, 25.42, 24.77),

    blip = {
      enabled    = true,
      sprite     = 225,
      color      = 1,
      scale      = 0.6,
      shortRange = true,
      name       = 'Town Cardealer',
    },

    preview = {
      vehicle = vec4(-7.49, -1068.99, 37.15, 270.82),
      camera  = vec4(6.68, -1074.78, 42.23, 57.05),
    },

    delivery = vec4(-59.92, -1072.88, 26.21, 87.99),
    testdrive = { spawn = vec4(-71.91, -1088.01, 26.65, 340.48) },

    showroom = {
      { model = 'sultan',  coords = vec4(-48.37, -1093.97, 25.42, 179.23) },
      { model = 'kuruma',  coords = vec4(-43.25, -1095.33, 25.42, 173.74) },
      -- ...
    },

    keyNpcWindowMinutes = 15,
    useCinematic        = true,
  },

  -- more dealerships...
}
```

Each dealership defines:

- **Salesman NPC** – opens the NUI.
- **Key NPC** – hands over keys and starts the cinematic delivery.
- **Blip** – map icon for the dealership.
- **Preview** – where the preview vehicle and NUI camera are positioned.
- **Delivery** – where purchased vehicles are delivered.
- **Test drive** – spawn for test drive vehicles.
- **Showroom** – world-placed static vehicles shown in the NUI list.
- **Key NPC window** – time window the player has to pick up keys for purchased vehicles.

If you only want a single dealership, keep just one entry in this table.

For backwards compatibility, `Config.Dealer` can be converted into a single `Config.Dealerships` entry automatically.

---

## Test drive settings

```lua
Config.TestDrive = {
  Enabled        = true,
  DurationSec    = 120,
  Mode           = 'normal', -- reserved for future modes
  ExitEnds       = true,     -- leaving the vehicle ends the test drive
  CleanReturnBonus = 0,      -- reserved for future update
  Spawn          = vec4(-71.91, -1088.01, 26.65, 340.48),
  AntiExploit = {
    BlockGarageStore = true,
    BlockModShop     = true,
    DespawnOnLeave   = true,
  }
}
```

Key behaviour:

- Starts from a configured spawn point (per-dealer spawn overrides this global one).
- Uses a timer visible in the UI.
- Optionally ends the drive when the player leaves the vehicle.
- Can block garage saving, upgrades and despawn the car if the player tries to leave the area.

---

## Dynamic market (48h window)

```lua
Config.Market = {
  Enabled             = true,
  WindowHours         = 48,
  TriggerSoldThreshold = 5,
  StepPercent         = 5,
  CapUp               = 30,
  CapDown             = 20,
  RecomputeIntervalMin = 1,
}
```

- **WindowHours** – rolling window for counting sales.
- **TriggerSoldThreshold** – how many sales are needed before the price moves.
- **StepPercent** – how much the price moves per step.
- **CapUp / CapDown** – maximum total movement above/below base price.
- **RecomputeIntervalMin** – soft interval for recalculating the market table.

Prices are always based on:

1. `shared/vehicle.prices.lua` (base price per model).
2. `Config.PriceFallbackByClass` if a model has no explicit entry.

---

## Finance (credit)

```lua
Config.Finance = {
  Enabled             = true,
  DownPaymentPct      = 20,
  FinanceMarkupPct    = 5,
  MinInstallments     = 1,
  MaxInstallments     = 12,
  DuePerPlaytimeHours = 1,
  GraceMisses         = 1,
  Repo = {
    ScanIntervalSec   = 60,
    DespawnOnRepo     = true,
  },
  EarlyPayoffDiscountPct = 0,
}
Config.Finance.Debug = true
```

- **DownPaymentPct** – upfront payment percentage (e.g. 20%).
- **FinanceMarkupPct** – increase over cash price when buying on finance.
- **Min/MaxInstallments** – allowed range of installments.
- **DuePerPlaytimeHours** – how often a new installment becomes due, based on in-game playtime.
- **GraceMisses** – how many missed installments are allowed before repo.
- **Repo.ScanIntervalSec** – how often repossession checks run.
- **Repo.DespawnOnRepo** – whether repossessed vehicles are removed from the ownership table.
- **EarlyPayoffDiscountPct** – discount if a loan is paid early.

If `Config.Finance.Enabled` is false, the finance tab is hidden from the NUI.

---

## Consignment (used vehicles) & payouts

```lua
Config.Consignment = {
  Enabled      = true,
  NPC          = vec4(-1600.91, -872.85, 9.81, 138.01),
  LotSlots     = {
    vec4(-1602.66, -867.61, 10.03, 142.90),
    -- more slot positions...
  },
  MaxPerPlayer = 5,
  MaxDescLen   = 300,
  ShowBulletproof = true,
  AllowMileage    = true,
  AllowService    = true,
}

Config.ConsignTable  = 'consign_stock' -- logical name used internally
Config.HouseCut      = 0.10            -- 10% commission
Config.PayoutAsCash  = true            -- pay as cash instead of bank
Config.MinClaimNotify = 0              -- minimal payout to notify
Config.PayoutNpc     = vec4(-1602.04, -871.32, 9.86, 135.54)
```

- **NPC** – main consignment NPC for listing and managing used cars.
- **LotSlots** – world positions used to spawn listed vehicles as previews.
- **MaxPerPlayer** – per-player limit of active listings.
- **MaxDescLen** – max length of description text.
- **ShowBulletproof / AllowMileage / AllowService** – extra metadata flags shown in the UI (optional).

The consignment system uses `vehicle_consignment` table and, on purchase, transfers ownership back into your configured owned-vehicles schema.

The payout NPC is used to pay the seller their cut if they are online; otherwise, you can adapt the `payouts_offline` table for queued payouts.

---

## Price data & fallbacks

```lua
Config.PriceData = 'data/prices.json'

Config.PriceFallbackByClass = {
  compacts    = { min = 20000,  max = 50000 },
  sedans      = { min = 30000,  max = 80000 },
  suvs        = { min = 50000,  max = 120000 },
  coupes      = { min = 60000,  max = 140000 },
  muscle      = { min = 50000,  max = 130000 },
  sports      = { min = 120000, max = 350000 },
  super       = { min = 250000, max = 500000 },
  motorcycles = { min = 15000,  max = 70000 },
  offroad     = { min = 50000,  max = 150000 },
  vans        = { min = 30000,  max = 90000 },
}
```

- **PriceData** – JSON snapshot used by the UI (`data/prices.json`).
- **PriceFallbackByClass** – if a model has no explicit price, its vehicle class is mapped into this table to determine a min/max range.

To adjust prices:

1. Edit `shared/vehicle.prices.lua` for base prices and categories.
2. (Optionally) regenerate or edit `data/prices.json` to match your server’s economy.
