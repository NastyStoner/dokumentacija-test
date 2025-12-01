---
title: beekeeper
has_children: true
nav_order: 3
---

# Beekeeper - Overview

A fully-featured, production-ready beekeeping system for ESX servers. Players place physical beehives, maintain them (food, water, cleanliness, HP), add queens/workers, and harvest outputs. The system supports two hive archetypes—Bee (classic honey + rare dark/golden honey) and Bumblebee (propolis + rare royal jelly / bee venom)—each with distinct loot tables, progression, and balancing levers. Everything is database-backed, ox_inventory-native, with ox_target interactions and a minimal NUI panel for status/actions. Admins get a live manager to view, move, or delete hives.

## **HIGHLIGHTS**

- Two Hive Types, Distinct Rewards

- Bee: steady honey economy + rare dark/golden drops.

- Bumble: propolis core + rare royal/venom drops (stronger economy knobs).

- Strict, Config-Driven Balance

- Happy thresholds, per-tick consumption, per-cycle output, jar costs, and special RNG are all in config.lua, fully commented.

*Diegetic Interactions*

- Real world prop (3m_beebox) + ox_target actions (open panel, collect, maintain).

*Inventory-Native*

- Uses ox_inventory for items, consumption, and payouts; no ad-hoc item hacks.

- Resilient, DB-backed State

- MySQL tables persist hive stats; ticks and degradation are server-side and deterministic.

*Clean UX*

- Minimal NUI for live status; ESC close; progress animations via ox_lib.

*Admin Control*

- /beehives opens manager; teleport/move, delete, set max workers; permission-gated (owner).

*Scalable*

- Per-player hive limits; per-hive worker caps; periodic loops tuned for performance.

*Safe by Design*

- Server validates items, caps bars (0–100), checks ownership/limits, and guards rare resource drains.


**Links**

- [installation](install.md)
- [Configuration](config.md)
- [Usage](usage.md)
- [Events](api.md)
