---
title: stands
has_children: true
nav_order: 5
---


# 3M Stands (standovi) — ESX Player-Owned Stalls

Lightweight, stable **player-owned street stands**. Players can **buy a stand**, **stock it with their items**, and **sell to others** via a clean NUI.  
Data persists to a JSON file (no SQL).

## Features
- **ESX** (tested on ESX 1.12+).
- World props: each stand is a placed `prop_hotdogstand_01` with heading.
- Buy / sell stands (prices configurable).
- Add items to stand stock, set **price** and **quantity**.
- Players purchase items from the **Shop** tab.
- Owner can **withdraw** earned cash.
- **Blacklist** + optional **weapon blocking**.
- Interaction via **ox_target**.
- JSON persistence: `standovi/standovi.json`.
- EN & HR locales, notify bridge (`okokNotify` or ESX).

## Requirements
- `es_extended`
- `ox_inventory`
- `ox_target` (or compatible target wrapper)
- (Optional) `okokNotify`

## Installation
1. Put the resource in `resources/` (folder name: `standovi`).  
2. Ensure dependencies are started.
3. Add to `server.cfg`:
   ```cfg
   ensure standovi
   ```
4. Configure `standovi/config.lua` — see **config.md**.
5. Restart the server.

## Quick Start
- Approach any stand and interact (**Open Stand**).  
- If unowned, purchase it (needs enough cash).  
- As owner, open **Manage** → add items from inventory, set **price**/**qty**.  
- Anyone can buy from **Shop**.  
- Owner can **withdraw** accumulated cash or sell the stand back to the city.

## Persistence
State is saved in `standovi/standovi.json` (owners, stock, balances).  
Back up this file periodically.

## Performance
Very low client CPU usage (prop + small target zone).

## Support
- Configurable through `config.lua` and `locale.lua`.
- UI files are editable in `html/`.
