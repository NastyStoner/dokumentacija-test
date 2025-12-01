---
title: Usage & Gameplay
parent: Car Thief
nav_order: 3
---

# Usage & Gameplay

## Flow
1. **Approach a vehicle** (distance ≤ `Config.Lockpick.MinDistance`), press **E** to attempt coding/lockpick.
2. **Server checks** job & inventory (`lockpick`). If allowed, client starts the **Vehicle Coding** minigame via `exports['cardecode']:StartVehicleCoding(...)`.
3. On **success**, player gets the **keys** (QS/Wasabi if present) and the vehicle doors unlock.
4. **Drive to a drop‑off NPC** (unlocked for your job). Interact to **store** the car to the stolen pool.
5. From the **Mafia Garage NPC** or **LB‑Tablet app**, view stolen cars: **return** to owner (spawns to player) or **destroy for cash** (payout depends on `vehicle_prices.json` × `Config.VehicleScrapPercent`).

## Tablet
- LB‑Tablet app is auto‑registered; opening it loads current stolen vehicles and shows owner name, plate and model.
- Deleting from tablet = scrapping for cash.

## Classic garage UI
- Interact with Garage NPC → opens NUI list.
- Actions: Return / Delete for cash.
