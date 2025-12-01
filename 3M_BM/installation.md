---
title : Installation
parent: Blackmarket
nav_order: 2
---

# Installation

This guide explains how to install and run **3M Blackmarket** on your FiveM server.

---

## 1. Requirements

### Core

- **FiveM** (latest recommended build)
- **MySQL** server (MariaDB / MySQL)
- **oxmysql** resource (for DB access)

### Framework

- **ESX** (1.2+ / legacy) **or**
- **QBCore**

The script uses a shared `Framework` bridge. In most setups you can leave `Config.Framework = 'auto'` and it will detect ESX / QBCore. If you have your own global bridge, make sure it is loaded before this resource.

### Inventory

- **ox_inventory** or **qb-inventory**

Set via `Config.Inventory` in `config.lua`. When set to `"auto"`, the script will try to detect which inventory you use.

### Target

- **ox_target** or **qb-target**

Set via `Config.Target` in `config.lua`.

### Optional

- **lb-tablet** – if you want the Blackmarket UI as a tablet app.
- `okokNotify` or another notification system depending on your `Config.NotifyType`.

---

## 2. Adding the resource

1. Download and unzip the resource.
2. Place the folder in your server resources directory, for example:

   ```text
   resources/[3m]/3M_Blackmarket
   ```

3. Make sure the folder name matches what you want to use (e.g. `3M_Blackmarket`).  
4. In your `server.cfg` add:

   ```cfg
   ensure oxmysql
   ensure ox_lib        
   ensure ox_inventory  # or qb-inventory
   ensure ox_target     # or qb-target
   ensure lb-tablet     # only if you use the tablet
   ensure 3M_Blackmarket
   ```

Adjust the order so that frameworks, inventory, target and oxmysql start **before** `3M_Blackmarket`.

---

## 3. Database setup

The resource creates all necessary tables automatically if they do not exist:

- `bm_items`
- `bm_meta`
- `bm_orders`
- `bm_order_items`

You can also import `blackmarket.sql` manually:

1. Open your MySQL client (HeidiSQL / phpMyAdmin / DBeaver / CLI).
2. Select your game database.
3. Run the contents of `blackmarket.sql`.

This is optional, but recommended for clean structure and indexes.

---

## 4. Configuration

Open `config.lua` and review all options:

- Framework / Inventory / Target mode
- Notification provider
- Black money account names
- Drop points & delivery settings
- NPC position & model
- Item list / stock / prices

See the full [Configuration](config.md) reference for details.

---

## 5. First run & syncing items

1. Start your server.
2. Watch the console for `[blackmarket]` messages:
   - It should log tables creation, item sync and auto-restock mode.
3. In-game, use the admin sync command once (with appropriate permission):

   ```text
   /syncblackitems
   ```

This will:

- Rebuild the item index from `Config.Items`
- Persist stock to `blackmarket_stock.json`
- Sync items to the `bm_items` MySQL table
- Optionally restock all items to `maxStock` (depending on `Config.AutoRestock.Mode`)

---

## 6. Testing in-game

1. Go to the configured NPC location (`Config.NPC.Coords`) **or** use the standalone command if enabled (`Config.CommandStandalone`).
2. Open the Blackmarket UI.
3. Add a few items to the cart and confirm the order.
4. You should:
   - Lose black money balance.
   - Get a notification with a **PIN code** and a drop location.
   - See a waypoint/blip if enabled.
5. Travel to the drop, interact with the target and enter the PIN.
6. The stash should open with your items.

If this works, you are ready to configure prices and integrate it fully in your city.
