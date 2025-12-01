---
title: Events & Callbacks
parent: beekeeper
nav_order: 3
---

## Client --> Server
```
--Place beehivebox where players is standing (server check item and limit: spawns prop)
TriggerServerEvent('beekeeping:createHive', x, y, z, heading)

-- Beehive Status  → server to client 'beekeeping:showStatus'
TriggerServerEvent('beekeeping:getStatus', hiveId)

-- Add queen ('bee' or 'bumblebee') – uses apropriate queen item
TriggerServerEvent('beekeeping:insertQueen', hiveId, 'bee' | 'bumbar')

-- add workers ('bee' or 'bumblebee') – spends worker items, respects limit
TriggerServerEvent('beekeeping:addWorkers', hiveId, 'bee' | 'bumbar')

-- Maintain the hive (the server itself consumes items and captures gain/limit 0–100)
TriggerServerEvent('beekeeping:feedHive',  hiveId)
TriggerServerEvent('beekeeping:waterHive', hiveId)
TriggerServerEvent('beekeeping:cleanHive', hiveId)
TriggerServerEvent('beekeeping:healHive',  hiveId)

-- Collection (standard / special) – requires empty jars
TriggerServerEvent('beekeeping:collectHoney',        hiveId)   -- pretvara product -> honey_jar/propolis_jar
TriggerServerEvent('beekeeping:collectDarkHoney',    hiveId)   -- dark_honey / royal_jelly
TriggerServerEvent('beekeeping:collectGoldenHoney',  hiveId)   -- golden_honey / bee_venom

```

## Server --> Client
```
-- Spawn prop hive + ox_target interactions
RegisterNetEvent('beekeeping:spawnHiveProp', function(hiveData) ... end)

-- Bulk loading of hives (on playerLoaded)
RegisterNetEvent('beekeeping:loadHives', function(hives) ... end)

-- Status payload for NUI/panel
RegisterNetEvent('beekeeping:showStatus', function(hive) ... end)

-- Collection animations (client at the end of the trigger finishCollect*)
RegisterNetEvent('beekeeping:playCollectAnim', function(hiveId) ... end)
RegisterNetEvent('beekeeping:playCollectAnimSpecial', function(hiveId, 'golden'|'dark') ... end)

-- Type change (e.g. after inserting a queen): specific channel by ID
RegisterNetEvent(('beekeeping:updateHiveType:%s'):format(hiveId), function(newType) ... end)

-- Admin NUI
RegisterNetEvent('beekeeping:openHiveManager', function(hives) ... end)
RegisterNetEvent('beekeeping:deleteHive', function(hiveId) ... end)
RegisterNetEvent('beekeeping:moveHive', function(data) ... end)
```

## Admin 
```
TriggerServerEvent('beekeeping:adminDeleteHive', hiveId)
TriggerServerEvent('beekeeping:adminTeleportHiveToMe', hiveId)
TriggerServerEvent('beekeeping:adminSetMaxBees', hiveId) -- instant fill on Config.MaxBeesPerHive
```


## Examples

- add beehive from another resource
```
local ped = PlayerPedId()
local pos = GetOffsetFromEntityInWorldCoords(ped, 0.0, 0.5, 0.0)
TriggerServerEvent('beekeeping:createHive', pos.x, pos.y, pos.z, GetEntityHeading(ped))
```

Insert the bumblebee queen
```
TriggerServerEvent('beekeeping:insertQueen', hiveId, 'bumbar')
```

Add  worker bees (if player has them in inventory)
```
TriggerServerEvent('beekeeping:addWorkers', hiveId, 'bee')
```

Feed and collect honey
```
TriggerServerEvent('beekeeping:feedHive', hiveId)
TriggerServerEvent('beekeeping:collectHoney', hiveId)
```
