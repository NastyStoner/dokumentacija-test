---
title: Troubleshooting
parent: Car Thief
nav_order: 6
---

# Troubleshooting

- **No such export StartVehicleCoding** — Ensure the Vehicle Coding resource is started and export name matches (`exports['cardecode']:StartVehicleCoding`).  
- **Garage full** — You reached `Config.MaxStolenVehicles` in `ukradena_vozila`.
- **ESX nil/xPlayer nil** — Start order wrong; ensure `es_extended` before this resource.
- **Owner names not shown** — `users` table must have `identifier, firstname, lastname` columns or adjust query.
- **No keys after return** — Set `Config.KeysGiveOnReturn = true` and have QS/Wasabi resource running (or use `none`).
- **NPC unlock not saving** — Ensure table `automafija_npc` exists; script inserts automatically on purchase.
- **Vehicle price defaulting** — Add/adjust entry in `data/vehicle_prices.json` (lowercase model name), or use `/setvehprice`.
