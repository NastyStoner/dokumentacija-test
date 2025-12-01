---
title: Configuration
parent: beekeeper
nav_order: 2
---

# Configuration

`Config.Locale` == Main language for notifications and NUI labels. (e.g., 'hr', 'en').

`Config.BeehiveItem` = "beehive_box" == Inventory item required to place a hive.. Player must have it.

`Config.BeehiveBox` = `3m_beebox`  == Ensure the model assets are loaded via some started resource.

`Config.MaxHivesPerPlayer` = 50 ==  -- Maximum number of hives a single player can own (DB limit).

## Consumable items for maintenance
```
Config.FoodItem  = "bee_food"
Config.WaterItem = "water_bottle"
Config.CleanItem = "cleaning_agent"
Config.HealItem  = "bee_heal"
```

`Config.JarItem` = "glass_jar"  == Empty jar required to collect products (honey/propolis/specials).

##  Output items when collecting:
```
Config.FullJarItem         = "honey_jar"     -- honey (bee)
Config.FullJarItemDark     = "dark_honey"    -- dark honey (bee special)
Config.FullJarItemGold     = "golden_honey"  -- golden honey (bee special)
Config.FullJarItemPropolis = "propolis_jar"  -- propolis (bumblebee)
```
# Bumblebee specials
```
Config.RoyalJellyItem = "royal_jelly"  -- bumbar "dark" channel
Config.BeeVenomItem   = "bee_venom"    -- bumbar "golden" channel
```
# Client animation duration when collecting
-- Client animation duration when collecting (milliseconds).

-- Normal: used when taking from "product" (honey/propolis).

-- Special: used for special pickups (dark/golden/royal/venom).
```
Config.CollectTimeNormal  = 3500  -- ms (propolis/honey)
Config.CollectTimeSpecial = 3500  -- ms (royal/venom, dark/golden)
```
`Config.PropolisPerJar` = 50 == Amount of PROPOLIS needed per jar from a bumblebee hive


## ACTION DURATIONS
```
-- These are UX-friendly durations you can use in your NUI/progress bars.
Config.FeedTime      = 3  -- feeding action time
Config.WaterTime     = 3  -- watering action time
Config.CleanTime     = 3  -- cleaning action time
Config.HealTime      = 3  -- healing action time
Config.QueenTime     = 4  -- inserting queen (queen_bee/bumbar_queen)
Config.BeeTime       = 4  -- adding workers (worker_*)
Config.TakeHoneyTime = 3  -- take product action time
```

## RESOURCE CONSUMPTION PER TICK (server loop)
```
-- Every Config.DegradeTimer minutes the server reduces these bars.
-- Unit: percentage points on a 0–100 scale per field.
Config.ReduceFoodPerTick  = 10
Config.ReduceWaterPerTick = 10
Config.ReduceCleanPerTick = 10
```
## GAIN PER ITEM USE
```
-- When a player uses an item on a hive, increases the target bar
-- by this amount (capped at 100). Unit: percentage points.
Config.FoodGain  = 50
Config.WaterGain = 50
Config.CleanGain = 50
Config.HealGain  = 50
```

## Hive Settings
```
-- Maximum workers a hive can hold. Server rejects additions over this limit.
Config.MaxBeesPerHive = 100

-- Maximum HP caps (logical upper bounds). Server degrades by -1 per degrade tick.
Config.MaxHiveHP = 100
Config.MaxBeeHP  = 100

-- “Happy” thresholds. Production happens ONLY if all three are met
-- (food >= Food, water >= Water, clean >= Clean) AND a queen is present.
Config.HappyAt = {
    Food  = 40,
    Water = 40,
    Clean = 40,
}
```

## HONEY / PRODUCT LOGIC
```
-- Product produced per cycle per worker when the hive is “happy”.
-- Formula: product += HoneyPerCycle * bees
Config.HoneyPerCycle = 1

-- Product required to fill one jar for a normal bee hive (honey).
-- Bumble hives use Config.PropolisPerJar instead.
Config.HoneyPerJar = 20


---------------------------------------------------------------------
-- TICK TIMERS (server loop) — in minutes
---------------------------------------------------------------------
-- Every UpdateTimer minutes, server:
-- 1) checks all hives,
-- 2) if “happy” + queen present → increases product and rolls RNG for specials.
Config.UpdateTimer  = 1   -- minutes

-- Every DegradeTimer minutes, server:
-- - reduces food/water/clean by Reduce*PerTick,
-- - reduces bee_hp and hive_hp by 1; deletes hive if any HP hits 0.
Config.DegradeTimer = 15  -- minutes


---------------------------------------------------------------------
-- SPECIAL RNG CHANCES (percent per Update cycle)
---------------------------------------------------------------------
-- BEES (type = 'bee'): add to dark_honey / golden_honey counters (by +1 each on success).
Config.BeeGoldenChance = 20  -- % chance per cycle for +1 golden_honey
Config.BeeDarkChance   = 10  -- % chance per cycle for +1 dark_honey

-- BUMBAR (type = 'bumbar'): add to bee_venom / royal_jelly counters (by +1 each).
-- Internally mapped: “golden”→ venom, “dark”→ royal.
Config.BumbarVenomChance      = 20 -- % chance per cycle for +1 bee_venom
Config.BumbarRoyalJellyChance = 10 -- % chance per cycle for +1 royal_jelly

```
