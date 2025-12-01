---
title : Usage
parent: Blackmarket
nav_order: 3
---

# Usage

This page explains how the Blackmarket behaves in-game for both **players** and **admins**.

---

## Player flow

### 1. Opening the Blackmarket

Depending on your configuration, players can open the UI in two ways:

#### a) NPC interaction

If `Config.NPC.Enabled = true`:

- Go to `Config.NPC.Coords`.
- Use your target key (ox_target / qb-target) on the NPC.
- Select the configured option (default: **"Otvori Blackmarket"**).
- The Blackmarket UI will open.

#### b) Standalone command

If **lb-tablet is disabled** (`Config.LBTablet = false`) and `Config.CommandStandalone` is set:

- Type the command in chat, for example:

  ```text
  /blackmarket
  ```

- The standalone NUI version of the Blackmarket will open.

#### c) Tablet app

If **lb-tablet integration** is enabled (`Config.LBTablet = true`):

- Open your lb-tablet.
- Select the app with identifier `Config.Identifier` (default: `blackmarket-app`).
- The UI loads inside the tablet.

---

## 2. Browsing the shop

Inside the UI:

- Items are grouped into categories:
  - Pistols, SMG, Rifles, Ammo, Melee, Misc.
- Stock & price per item are shown.
- Toggling categories filters the visible list.

Players can:

- Add items to their cart with a quantity selector.
- See a running total (sum of all items in cart).
- Remove items or change quantities before confirming.

---

## 3. Checking out

When the player presses **Checkout**:

1. The client sends the cart to the server.
2. The server:
   - Validates all item IDs.
   - Validates stock availability.
   - Summarizes total price.
   - Checks the player's black money balance (`Framework.GetBlackMoney`).
3. If any validation fails, the player receives a localized error notification.

If everything is valid:

- Stock is decremented from memory & persisted.
- Black money is removed from the player.
- A random **drop point** is chosen from `Config.DropPoints`.
- A 6-digit **PIN code** is generated.
- A new row is inserted into `bm_orders` and `bm_order_items`.
- A **stash** is registered at the drop location and filled with items.
- A runtime delivery entry is stored for that player.

The player receives:

- A **success notification**.
- An **info notification** with the PIN code (localized string `delivery_assigned`).
- A waypoint and optional blip to the drop location (depending on config).

---

## 4. Collecting the order

At the drop location:

1. The player uses target on the drop interaction.
2. A PIN prompt opens
3. The player enters their PIN code.

The server checks:

- The player identifier matches the delivery.
- The drop index matches.
- The code is correct.
- The order is not expired.

If valid:

- Player gets a success notification (`opened_stash`).
- The stash is opened through the inventory integration.
- The post-open expiry timer is scheduled (if configured).

If invalid:

- Wrong code → `wrong_code`.
- No delivery at that point → `no_order_here`.
- Expired → `delivery_expired` and stash is cleared/removed.

### Stash cleanup

For inventories that support it (`Bridge.Inventory.CanWatchStash()`):

- A background loop checks if the stash is empty.
- When it becomes empty:
  - The stash is cleared and removed.
  - The delivery entry is deleted from memory.

---

## 5. Expiry behaviour

### Pre-open expiry

If `Config.Delivery.UseExpiry = true`:

- When an order is created, `scheduleExpiry` is called.
- After `Config.Delivery.ExpiryMinutes`:
  - If the stash was never opened:
    - The stash is cleared & removed.
    - The delivery is removed from memory.
    - The player (if online) gets a `delivery_expired` notification.

### Post-open expiry

When the stash is opened:

- `schedulePostOpenExpiry` is called.
- After `Config.Delivery.ExpiryMinutes`:
  - If the delivery is still active:
    - The stash is cleared & removed.
    - The player (if online) gets a `delivery_expired` notification.

---

## 6. Admin commands

The following commands require admin permissions.  
The exact logic is implemented in your framework bridge; by default, ESX uses group checks (e.g. `Config.AdminGroup`) and QBCore uses permissions.


### `/syncblackitems`

- Rebuilds item index from `Config.Items`.
- Persists stock to `blackmarket_stock.json`.
- Synchronizes `Config.Items` to the `bm_items` MySQL table.
- Runs initial restock logic for `AutoRestock.Mode = 'restart'`.


---

## 7. Locales & customization

### Locales

- All notification strings, UI labels and prompts are defined in `locales/en.lua`, `locales/hr.lua`, etc.
- To add a new language:
  1. Copy `locales/en.lua` → `locales/de.lua`.
  2. Translate values.
  3. Set `Config.Locale = 'de'` in `config.lua`.

### Images

- Item images are referenced by `image` in `Config.Items`.
- Place PNG files in the UI assets path used by the NUI (commonly `ui/dist/images/`).
- Make sure file names match exactly (case-sensitive for some hosts).

### NPC & drop locations

- Adjust `Config.NPC.Coords` to move the dealer.
- Add/remove entries in `Config.DropPoints` to change potential delivery spots.

---

With this setup, you can run a fully-featured illegal market that feels immersive and configurable, while keeping the code cleanly separated between framework, inventory and target layers.
