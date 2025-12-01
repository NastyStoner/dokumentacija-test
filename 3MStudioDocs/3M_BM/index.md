---
title: Blackmarket
has_children: true
nav_order: 6
---

# 3M Blackmarket

A configurable, framework-agnostic Black Market system for FiveM with support for ESX and QBCore, ox_inventory or qb-inventory, ox_target / qb-target and optional **lb-tablet** integration.

Players can browse a secret weapon & contraband shop, pay with black money, receive a **random drop location** with a PIN code, and loot their order from a timed stash. Stock, prices and categories are fully configurable in `config.lua`, and the resource keeps item stock persistent across restarts and can auto-restock on a schedule.

---

## Main Features

- **Multi‑framework support**
  - ESX & QBCore via a shared framework bridge (`Config.Framework = 'auto' | 'esx' | 'qbcore'`).
- **Inventory agnostic**
  - ox_inventory and qb-inventory support through `Config.Inventory`.
  - Uses stashes at hidden drop points to deliver orders.
- **Target integration**
  - Works with ox_target or qb-target (`Config.Target`).
  - NPC interaction for opening the market.
- **Optional lb-tablet app**
  - Run as a standalone NUI app (command-based) or as a full **lb-tablet** application.
- **Dynamic stock & pricing**
  - All items defined in `Config.Items`.
  - Per-item stock & maxStock, categories, and custom images.
- **Persistent stock storage**
  - Stock is stored in `blackmarket_stock.json` and synced to MySQL (`bm_items`).
  - Auto-save loop (every X minutes, configurable).
- **Delivery system with PIN code**
  - Orders are charged immediately and delivered to a **random drop point**.
  - Each order has a one-time PIN code which the player must enter at the drop.
  - Uses dedicated stashes per order.
- **Expiry & cleanup**
  - Optional pre-open expiry (order never opened).
  - Optional post-open expiry (stash open but not emptied).
  - Automatic cleanup of empty stashes for supported inventories.
- **Admin tools**
  - Commands to restock, sync config with DB, and add stock to individual items.

---

## Files Overview

```text
3M_Blackmarket/
  fxmanifest.lua
  config.lua
  blackmarket.sql
  client/
    client.lua
    functions.lua
  server/
    server.lua
    functions.lua
  shared/
    bridge.lua           (framework/inventory/target bridge – escrowed in Tebex build)
  locales/
    en.lua
    hr.lua
  ui/
    dist/                (compiled React / NUI app)
  blackmarket_stock.json (generated at runtime)
```

---


