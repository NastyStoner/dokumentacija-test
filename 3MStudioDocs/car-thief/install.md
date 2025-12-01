---
title: Installation
parent: Car Thief
nav_order: 1
---

# Installation

## Requirements
- **ESX 1.12.4**
- **ox_lib**
- **oxmysql**
- **lb-tablet** (for tablet app)
- Optional: **ox_target**, **ox_inventory**, **okokNotify**/**brutal_notify**, **qs-vehiclekeys** or **wasabi_carlock**

## Files
- `data/vehicle_prices.json` — per‑model base prices (lowercase keys)
- `html/` — classic NUI garage
- `ui/dist/` — LB‑Tablet app bundle

## server.cfg
```
ensure ox_lib
ensure lb-tablet
ensure automafija   # (your resource folder name)
```

> Make sure `oxmysql` is started before scripts that use it.

## First run
1. Upload the resource to `resources/[3m]/automafija`  
2. Add `ensure automafija` to `server.cfg`  
3. Start the server. The script will automatically:
   - Load `data/vehicle_prices.json`
   - Create `ukradena_vozila` table mirroring `owned_vehicles` (+ `ukradeno_at`)
