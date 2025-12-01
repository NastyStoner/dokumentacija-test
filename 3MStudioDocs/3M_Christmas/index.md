---
title: 3M Christmas Event
has_children: true
nav_order: 4
---


# 3M Christmas Event

Full-featured seasonal event for FiveM with:

- Global Christmas tree progression (shared XP for whole server)
- Daily Advent Calendar (1–25) with configurable rewards
- Santa Spots with carryable gifts and random ornament drops
- Santa delivery quest (NPC + van + 5 deliveries)
- Placeable toys (e.g. snowball → snowman prop)
- Shoulder plush toys (non-consumable, toggle on/off)
- Optional automatic Christmas weather lock

The resource is framework-aware (ESX / QBCore) and bridges most common inventories, keys and notify systems.

---

## Documentation

- [Installation](Installation.md)
- [Configuration](Config.md)
- [Items & Integration](Items.md)
- [Usage (Gameplay Flow)](Usage.md)
- [Developer API (Exports & Events)](Developers.md)

---

## Requirements (Summary)

- **Framework**: ESX (`es_extended`) or QBCore (`qb-core`)
- **Database**: `oxmysql`
- **Inventory** (any of):
  - ESX default inventory
  - QBCore default inventory
  - `ox_inventory`
  - `qb-inventory`
- **Optional**:
  - `ox_target` / `qb-target` for interactions
  - `wasabi_carlock`, `qs-vehiclekeys`, `qb-vehiclekeys` or `brutal_keys` for quest van keys
  - `ox_lib`, `okokNotify`, `wasabi_notify` for notifications

For detailed setup, see [Installation](Installation.md).
