---
title: Installation
parent: 3M_Dealership
nav_order: 1
---


# Installation

## Requirements

Minimal recommended stack:

- FiveM server build **5181+** (`/server:5181` is declared in `fxmanifest.lua`)
- **oxmysql**
- **ox_lib**
- **ox_target**
- **ESX** (1.2+). QBCore-style schemas are supported by autodetection, but ESX is the primary target.

Optional integrations:

- `okokNotify` – notification provider
- `wasabi_carlock` – vehicle keys
- `qs-vehiclekeys` – alternative keys
- `ox_inventory` – only if you adapt interactions, not required by default

## 1. Download & place the resource

1. Download the resource and extract it.
2. Place the folder in your server resources directory, for example:

   ```text
   resources/[3m]/3m_dealership
   ```

3. Make sure the folder name matches the one in `fxmanifest.lua` (`name '3M Dealership'`), usually `3m_dealership`.

## 2. Import SQL tables

1. Open `3m_dealership/tables.sql`.
2. Import the file into your game database (phpMyAdmin, HeidiSQL, CLI, etc.).
3. This will create the following tables:

   - `vehicle_market`
   - `vehicle_loans`
   - `vehicle_consignment`
   - `payouts_offline`

> **Note:** The script also autodetects your vehicle ownership schema (ESX `owned_vehicles` or QB `player_vehicles`). Ensure at least one of these exists with standard columns.

## 3. Configure the resource

Open `config.lua` and adjust at least:

- `Config.Framework` – set to `'esx'` (recommended) or `'qb'` / `'qbcore'` if you run QB.
- `Config.Locale` – `'en'` or `'hr'`.
- `Config.Currency` – which account is used for purchases (`'bank'` by default).
- `Config.UseNUIOnly` – whether to force NUI prompts or allow 3D help text.
- `Config.UseOxTarget` – enable or disable ox_target interaction zones.
- `Config.Webhooks` – optional webhook URLs for purchases, finance, repos and consignment.
- `Config.Notification.provider` – choose your notification provider (`'ox'`, `'esx'`, `'okok'`, `'brutal'`).

Then review:

- `Config.Keys` – how keys are given/removed (`wasabi_carlock`, `qs-vehiclekeys` or your own export).
- `Config.Dealerships` – positions, blips, showroom slots and delivery/test drive points.
- `Config.TestDrive` – duration, spawn position and anti-exploit options.
- `Config.Market` – dynamic market behaviour (window, thresholds, caps).
- `Config.Finance` – down payment, markup, installment rules and repo settings.
- `Config.Consignment` & payout settings.
- `Config.PriceData` and `Config.PriceFallbackByClass` – price source and fallback ranges.

See [Configuration](configuration.md) for a more detailed breakdown.

## 4. Start order

In your `server.cfg`, ensure the dependencies are started before `3m_dealership`:

```cfg
ensure oxmysql
ensure ox_lib
ensure ox_target

ensure 3m_dealership
```

If you use `okokNotify`, `wasabi_carlock` or `qs-vehiclekeys`, make sure those resources are started **before** `3m_dealership` as well.

## 5. First run & basic checks

1. Start the server.
2. Check your server console:
   - You should see logs from 3M Dealership about framework detection and owned-vehicles schema.
   - No errors should appear from `3m_dealership` during startup.
3. Join the server and move to a configured dealership location.
4. Interact with the salesman NPC (ox_target or prompt) and verify the NUI opens.

If anything fails, double-check:

- SQL was imported.
- Dependencies are started.
- `config.lua` is valid (no missing commas or syntax errors).
