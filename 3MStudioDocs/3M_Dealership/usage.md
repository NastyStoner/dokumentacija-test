---
title: Usage & Gameplay
parent: 3M_Dealership
nav_order: 3
---


# Usage & Gameplay

This page explains how players and staff interact with 3M Dealership in-game.

---

## Player flow – main dealership

### Opening the dealership

1. Go to one of the configured dealership locations (`Config.Dealerships`).
2. Approach the *salesman* NPC.
3. Interact via **ox_target** (if enabled) or the default help text prompt.
4. The NUI dealership app opens.

### Browsing vehicles

Inside the app UI, players can:

- Browse categories (compact, sedans, SUVs, sports, etc.).
- View vehicle name, brand and price.
- Rotate and zoom the preview vehicle via the on-screen hints.
- See special badges (e.g. popular vehicles affected by the dynamic market).

The list of vehicles comes from `shared/vehicle.prices.lua` and `data/prices.json`.

### Test drives

From any vehicle entry, players can start a **test drive** if enabled:

- The vehicle is spawned at the configured test drive spawn (per dealer or global).
- The script gives temporary keys using your configured keys system.
- A timer is shown – when it reaches zero, the test drive ends.
- By default, leaving the vehicle early will also end the test drive.

Anti-exploit behaviour (if enabled in `Config.TestDrive.AntiExploit`):

- Test drive vehicles cannot be stored in garages.
- Mod shops are blocked for the test drive vehicle.
- Leaving the area despawns the vehicle.

When the test drive ends, the player is teleported back to their starting position.

### Buying vehicles (cash / bank)

Players can purchase the currently selected vehicle:

- **Buy (Bank)** – immediate purchase using the configured money account.
- The script:
  - Calculates the price using the dynamic market.
  - Removes money from the player.
  - Creates an ownership record in your `owned_vehicles` / `player_vehicles` table.
  - Creates a **pending keys** entry to be picked up at the key NPC.

### Buying on finance

If `Config.Finance.Enabled` is true, the UI shows a **Finance** option:

- The player selects a number of installments within allowed bounds.
- The script:
  - Calculates down payment and installment size.
  - Charges the down payment immediately.
  - Creates a `vehicle_loans` row with:

    - total principal
    - markup percentage
    - total installments
    - next-due instalment based on `DuePerPlaytimeHours`

The player receives the vehicle and keys just like a cash purchase.

#### Paying installments

- The script tracks **in-game playtime** and, once enough time has passed, marks an installment as due.
- If the player misses more installments than `Config.Finance.GraceMisses`, the vehicle becomes eligible for repossession.
- If `Config.Finance.Repo.DespawnOnRepo` is true, repossessed vehicles are removed from the ownership table and treated as returned stock.

You can optionally add your own interfaces (commands, NUI) for players to check outstanding loans or pay early – the backend functions already exist in `server/finance.lua`.

---

## Dynamic market

The player-facing effect of the **dynamic market** is:

- Popular vehicles (sold many times in the last 48h) receive a price **increase** badge.
- Unpopular vehicles can become slightly cheaper up to `CapDown` percent.
- The NUI reflects the adjusted prices automatically; you do not need to edit any database values manually.

This keeps your dealership economy reactive and reduces the need for constant manual balancing.

---

## Used car consignment

### Listing a vehicle for sale

1. Go to the consignment NPC (`Config.Consignment.NPC`).
2. Sit in the vehicle you want to list (or follow your server-specific instructions).
3. Interact with the NPC and choose the “Sell used vehicle” option.
4. Enter:

   - Asking price (validated against price fallbacks).
   - Optional description (length limited by `Config.Consignment.MaxDescLen`).
   - Optional flags (mileage, serviced) if enabled in config.

5. If all checks pass, the script:

   - Removes the vehicle from your ownership table.
   - Inserts a row into `vehicle_consignment`.
   - Spawns the vehicle on a free `LotSlot` position as a preview.

Per-player active listing limit is controlled via `Config.Consignment.MaxPerPlayer`.

### Browsing & buying used cars

Players can:

- Approach any preview vehicle on the lot and open the consignment UI.
- See:

  - Model, plate and seller name.
  - Price.
  - Description.
  - Extra metadata (bulletproof, mileage, serviced) depending on config.

If they purchase:

- The buyer is charged the full price.
- The seller receives their cut (minus `Config.HouseCut` commission).
- Ownership is transferred back to `owned_vehicles` / `player_vehicles` using the auto-detected schema.
- A webhook can be fired if configured.

### Collecting payouts

If a seller is online when a consigned vehicle is sold, they can be notified and directed to the payout NPC (`Config.PayoutNpc`).

The script also supports an `payouts_offline` queue table for implementing your own delayed payout logic if needed.


