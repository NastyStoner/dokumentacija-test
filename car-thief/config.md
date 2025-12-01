---
title: Configuration
parent: Car Thief
nav_order: 2
---

# Configuration

Edit `config.lua`.

## Core
| Key | Type | Default | Notes |
|---|---|---|---|
| `Locale` | string | `"hr"` | UI/notify language. |
| `NotifyType` | string | `"okok"` | `okok`, `brutal`, `esx/chat` fallback. |
| `JobName` | string | `"automafija"` | Required job to interact. |
| `DatabaseName` | string | `"s67536_hugo"` | Used when cloning `owned_vehicles` schema. |
| `MaxStolenVehicles` | number | `15` | Global capacity of stolen garage. |
| `KeySystem` | string | `"auto"` | `auto` / `qs` / `wasabi` / `none`. |
| `KeysGiveOnReturn` | bool | `true` | Give player keys upon returning car. |
| `Webhook` | string | `""` | Discord webhook for logs. |
| `VehicleScrapPercent` | number | `0.30` | Scrap payout percent of model price. |

## Lockpick
```lua
Config.Lockpick = {
  Enabled = true,
  JobOnly = true,
  PromptKey = 38,      -- E
  MinDistance = 2.5,
  CooldownMs = 5000,
  ConsumeOnStart = false,
  ItemName = 'lockpick',
}
```

## Tablet app
| Key | Default | Notes |
|---|---|---|
| `Identifier` | `"automafija-app"` | LB‑Tablet app id |
| `DefaultApp` | `false` | Show on device by default |
| `Name` | `"HotWheels"` | App name |
| `Description` | `"Tajni registar crnog trzista..."` | App description |
| `Developer` | `"Matic i Vuco d.o.o"` | App developer |
| `images` | `ui/dist/screenshot-*.webp` | Used in tablet app listing |

## Garage & NPCs
```lua
Config.GarageLocation = vec3(-1161.1333, -2013.6637, 13.1803)
Config.GarageSpawn = { coords = vec3(-1156.9354, -2009.1191, 13.1803), heading = 319.0 }
Config.GarageNpcModel = 'g_m_m_chicold_01'

Config.NpcDropoffs = {
  { id='dock', label='Docks', coords=vec3(235.4201,-3110.7344,5.7903),  heading=86.0,  model='g_m_m_mexboss_01',   price=25000 },
  { id='murietta_oilfield', label='Oil Field', coords=vec3(1383.294,-2078.184,51.9985), heading=4.0,   model='g_m_y_ballaeast_01', price=300000 },
  { id='paleto', label='Paleto Bay', coords=vec3(11.7509,6493.8779,31.4683), heading=224.0, model='g_m_y_lost_03', price=22000 },
  { id='vespucci', label='Vespucci', coords=vec3(-1072.919,-1049.486,2.1503), heading=32.0, model='g_m_y_famdnf_01', price=27000 },
  { id='sandy', label='Sandy Shores', coords=vec3(1721.0782,3304.0823,41.2235), heading=288.0, model='g_m_m_chicold_01', price=20000 },
}
```
