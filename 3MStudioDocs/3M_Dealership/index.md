---
title: 3M_Dealership
has_children: true
nav_order: 7
---



# 3M Dealership

Modern, cinematic vehicle dealership system for ESX / QB-based FiveM servers.

3M Dealership replaces the classic “open a menu and buy a car” flow with a full dealership experience:

- NPC salesman with ox_target or key prompts
- Cinematic delivery with a key-handover NPC
- Safe test drives with automatic return and anti-exploit checks
- Dynamic market that raises/lowers vehicle prices based on sales
- In-depth finance system driven by **in‑game playtime**
- Used car consignment lot with visual previews
- Multiple dealership locations from a single config
- Built-in localization (EN/HR) and notification bridge

> This documentation is meant for GitHub Pages. Use the links below to navigate.

## Documentation

- [Installation](installation.md)
- [Configuration](configuration.md)
- [Usage & Gameplay](usage.md)

## Core Features

### Cinematic dealership flow

- Talk to a configured salesman ped to open the NUI.
- Browse vehicles with categories, labels and brands from `shared/vehicle.prices.lua`.
- Preview vehicles on the showroom floor (per-slot model + coords in `Config.Dealerships`).
- Purchase flow hands the vehicle off to a **key NPC** with a timed pickup window and an optional cinematic camera.

### Test drives (with anti-exploit)

- Configurable test drive duration and spawn position (`Config.TestDrive`).
- Player is returned to their original position when the test drive ends or they leave the vehicle.
- Optional anti-exploit: block storing the test drive vehicle in garages, block mod shops, and despawn the vehicle if the player leaves the area.

### Dynamic 48h market

- Table `vehicle_market` tracks the last 48 hours of sales.
- Once a vehicle crosses a configurable sold threshold, its price moves in steps (e.g. +5% per step).
- Price changes are capped separately for increases and decreases.
- All prices are resolved through `shared/vehicle.prices.lua` and `Config.PriceFallbackByClass` so every model has a sensible range.

### Finance system (loans by playtime)

- Table `vehicle_loans` stores active loans.
- Down payment, markup percentage, minimum/maximum installments and grace period are configurable via `Config.Finance`.
- Installments are accrued based on **in-game playtime**, not real-life time.
- Missed installments beyond the grace threshold automatically trigger repossession.
- Optional early payoff discount and automatic despawn of repossessed vehicles.
- Webhook support for created/paid/repossessed loans.

### Used car consignment lot

- Players can list their own vehicles for sale on a configured consignment lot.
- One NPC handles listing, cancelling and managing consigned vehicles.
- Each slot on the lot spawns a preview vehicle for browsing.
- Per-player limit for active listings, maximum description length, and house cut/commission are configurable.
- Payouts are processed via `Config.PayoutNpc` and can be paid as cash or bank (by config).
- SQL schema supports offline payouts via `payouts_offline` if you ever implement a delayed payout flow.

### Multi-dealer support

- `Config.Dealerships` defines as many dealerships as you want:
  - Unique `id` and label
  - Salesman and key NPC positions
  - Blip settings
  - Preview camera + vehicle spawn
  - Delivery position
  - Showroom slots (model + coords)
  - Per-dealer test drive spawn, key NPC window and cinematic toggle

### Framework & inventory agnostic design

- Primary target: **ESX** (1.2+).
- Framework auto-detection in `server/utils.lua` supports **QB**-style schemas (`player_vehicles`) as well.
- Keys are bridged via `Config.Keys` to:
  - `wasabi_carlock`
  - `qs-vehiclekeys`
  - or a fully custom export
- Notification bridge supports:
  - `ox_lib` notifications
  - ESX `showNotification`
  - `okokNotify`
  - A custom “brutal” provider slot
- Locales available out of the box: **English** and **Croatian**.

## Database schema

All required tables are provided in `tables.sql`:

- `vehicle_market` – dynamic market (48h sales window and current delta).
- `vehicle_loans` – finance contracts.
- `vehicle_consignment` – used car listings.
- `payouts_offline` – optional offline payout queue (hook-ready).

Import this file before starting the resource for the first time.
