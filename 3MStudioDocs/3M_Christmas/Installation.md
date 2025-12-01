---
title : Installation
parent: 3M Christmas Event
nav_order: 2
---

# Installation

## 1. Download & folder structure

1. Place the resource folder in your server resources, for example:

   ```text
   resources/[3m]/3M_ChristmasAddons
   resources/[3m]/3M_Christmas
   ```

2. Make sure that addon folder is started before script as there are assets that needs to load before script.


## 2. Dependencies

**Required**

- `es_extended` **or** `qb-core`
- `oxmysql`

**Supported inventories**

- ESX native inventory
- QBCore native inventory
- `ox_inventory`
- `qb-inventory`

**Optional but supported**

- Target:
  - `ox_target`
  - `qb-target`
- Vehicle keys for Santa quest:
  - `wasabi_carlock`
  - `qs-vehiclekeys`
  - `qb-vehiclekeys`
  - `brutal_keys`
- Notifications:
  - ESX native (`esx:showNotification`)
  - QBCore native (`QBCore:Notify`)
  - `ox_lib` notify
  - `wasabi_notify`
  - `okokNotify`

---

## 3. Database (oxmysql)

This resource uses `oxmysql` and includes:

- `@oxmysql/lib/MySQL.lua` in `fxmanifest.lua`
- Auto-creation of required tables on resource start

Tables created by the script:

```sql
CREATE TABLE IF NOT EXISTS xmas_tree_progress (
    id TINYINT NOT NULL PRIMARY KEY,
    xp INT NOT NULL DEFAULT 0,
    stage TINYINT NOT NULL DEFAULT 1
);

CREATE TABLE IF NOT EXISTS xmas_players (
    identifier VARCHAR(64) NOT NULL PRIMARY KEY,
    total_xp INT NOT NULL DEFAULT 0,
    donations_today INT NOT NULL DEFAULT 0,
    last_donation_date DATE DEFAULT NULL,
    last_spot_date DATE DEFAULT NULL,
    spot_count_today INT NOT NULL DEFAULT 0,
    last_daily_date DATE DEFAULT NULL,
    calendar_claims LONGTEXT DEFAULT NULL,
    last_quest_day DATE DEFAULT NULL,
    quest_completed TINYINT(1) NOT NULL DEFAULT 0,
    key_day TINYINT UNSIGNED DEFAULT NULL,
    key_used TINYINT(1) NOT NULL DEFAULT 0
);
```

You **do not** have to run these manually – they’re here for reference.

---

## 4. Items & props

You must create the needed items in your inventory system.  
See [Items & Integration](Items.md) for a full item list and examples.

Custom prop for carryable gift:

- Model/YDR: `3m_xmas_gift.ydr`
- YTYP: `3m_xmas_gift.ytyp`

The files are in the `stream/` folder. To enable them, uncomment the `data_file` line in `fxmanifest.lua`:

```lua
files {
    'html/index.html',
    'html/style.css',
    'html/app.js',
    'stream/*'
}

data_file 'DLC_ITYP_REQUEST' 'stream/3m_xmas_gift.ytyp'
```

---

## 5. Start order

In your `server.cfg`, ensure:

```cfg
ensure oxmysql
ensure es_extended      # or: ensure qb-core
# ensure ox_inventory   # if you use it
# ensure qb-inventory   # if you use it
# ensure ox_target      # optional
# ensure qb-target      # optional

ensure Christmas
```

Framework, inventory, target and key providers are auto-detected at runtime.

---

## 6. Basic configuration

Open `config.lua` and adjust:

- `Config.Locale` – language (`'en'` or `'hr'`)
- `Config.TimeWindow` – date range for the event
- `Config.Tree` – global tree location & XP stages
- `Config.SantaSpots` – Santa spots positions & props
- `Config.Quest` – Santa quest NPC, van and delivery points
- `Config.DonationItems` & `Config.CalendarRewards` – XP & rewards

See [Configuration](Config.md) for detailed reference.

---

## 7. Verifying the install

1. Restart the server or start the resource:

   ```cfg
   refresh
   ensure Christmas
   ```

2. Check the console for `[3M Xmas]` messages:
   - Framework detection (ESX / QBCore)
   - Weather bridge info (if enabled)
   - Tree XP load (e.g. `Loaded tree XP: 0 (stage 1)`)

3. Join the server:
   - The main Christmas tree should spawn at `Config.Tree.coords`.
   - Target options should appear on the tree and Santa spots (if a target system is installed).
   - Using configured items (`snowball`, `xmas_key`, ornaments, plush toys) should trigger the appropriate interactions.
