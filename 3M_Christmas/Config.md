---
title : Configuration
parent: 3M Christmas Event
nav_order: 1
---

# Configuration

All options are in `config.lua`.  
Below is a structured overview of the main sections.

---

## Locale & Notifications

### `Config.Locale`

- **Type:** `string`
- **Default:** `'hr'`
- **Description:** Current language key used for `Locales[...]`.
  - Provided: `en`, `hr`
  - You can add more by creating `locales/<lang>.lua` and adding `Locales['<lang>'] = { ... }`.

---

### `Config.Notify`

```lua
Config.Notify = {
    provider = 'okok' -- 'esx', 'qb', 'ox', 'wasabi', 'okok'
}
```

- Selects which notify system `Notify()` will use.
- See [Developer API](Developers.md#notify-export) for details.

---

## Debug

```lua
Config.Debug = {
    present     = true,
    ForceActive = false
}
```

- `present`  
  - When `true`, daily limits for Santa spot rewards are disabled.  
  - Helpful for testing spots & drops.

- `ForceActive`  
  - When `true`, event is considered active regardless of `Config.TimeWindow`.  
  - Use this for testing outside the configured date range.

---

## Event Window & Calendar

### `Config.CalendarKeyItem`

```lua
Config.CalendarKeyItem = 'xmas_key'
```

- Name of the inventory item that unlocks the calendar window for the current day.

---

### `Config.TimeWindow`

```lua
Config.TimeWindow = {
    start  = { month = 11, day = 1 },
    finish = { month = 11, day = 25 }
}
```

- Controls which real-world dates count as “event days” (1..N).
- Used for:
  - Advent Calendar day index
  - Per-day quest completion
  - Key reward uniqueness

---

## Weather Lock

```lua
Config.ChangeWeather = {
    enabled        = true,
    provider       = 'auto',  -- 'auto' | 'cd_easytime' | 'qb-weathersync' | 'none'

    weatherType    = 'XMAS',  -- GTA weather type

    freezeTime     = false,
    dynamicWeather = false,
    blackout       = false,

    pollInterval   = 30000,   -- server: check event active
    clientReapply  = 30000    -- client: reapply weather/time
}
```

- `enabled`: master on/off for weather lock.
- `provider`:
  - `auto` – auto-detect `cd_easytime` or `qb-weathersync`, otherwise internal fallback.
  - `cd_easytime` – force that provider.
  - `qb-weathersync` – force that provider.
  - `none` – no automatic weather control.
- `weatherType`: weather name set during event (e.g. `XMAS`, `SNOW`).
- `freezeTime`, `dynamicWeather`, `blackout`: passed to provider when supported.
- `pollInterval`: how often the server re-checks whether the event should be active.
- `clientReapply`: how often clients reapply weather/time locally.

---

## Vehicle Keys for Quest Van

```lua
Config.Keys = {
    provider = 'wasabi',      -- 'none' | 'wasabi' | 'qs' | 'qb' | 'brutal'
    label    = 'Christmas Van'
}
```

- `provider`:
  - `none` – no external key system is used.
  - `wasabi` – `wasabi_carlock`.
  - `qs` – `qs-vehiclekeys`.
  - `qb` – `qb-vehiclekeys`.
  - `brutal` – `brutal_keys`.
- `label`: used only for some key systems (e.g. `brutal_keys`) as display name.

---

## Global Tree

```lua
Config.Tree = {
    coords = vector4(159.9889, -1005.0620, 29.4178, 0.0),

    stages = {
        { xp = 0,    model = 'borcina' },
        { xp = 250,  model = 'borcina' },
        { xp = 500,  model = 'borcina' },
        { xp = 750,  model = 'borcina' },
        { xp = 1000, model = 'xmas_tree_stage5' }
    }
}
```

- `coords`: position and heading of the **main** tree.
- `stages`: list of XP thresholds and tree models.
  - The highest `xp` that is `<=` current tree XP defines the active stage.
  - You can use different models for each stage.

---

## Donations & Santa Spots

```lua
Config.DonationsPerDay      = 2
Config.SantaSpotDailyLimit  = 5
Config.CarryGiftModel       = '3m_xmas_gift'
```

- `Config.DonationsPerDay`
  - Max donation actions per player per day at the main tree.
- `Config.SantaSpotDailyLimit`
  - Max number of **items rewarded** per player per day from Santa spots.
- `Config.CarryGiftModel`
  - Prop model used when carrying gifts picked from Santa spots.

---

### `Config.SantaSpots`

```lua
Config.SantaSpots = {
    {
        id   = 'legion_square',
        tree = {
            coords = vector4(133.0702, -988.5129, 29.3581, 0.0),
            model  = 'prop_xmas_tree_int'
        },
        gifts = {
            { coords = vector4(131.0391, -977.5129, 29.3582, 0.9), model = '3m_xmas_gift' },
            -- more gifts...
        }
    },
    -- more spots...
}
```

- Each entry defines one **Santa spot**:
  - `id`: unique identifier (string). If omitted, auto-generated.
  - `tree`: small decorative tree:
    - `coords`: position/heading.
    - `model`: object name.
  - `gifts`: list of gift props around the tree:
    - `coords`: where each gift sits.
    - `model`: usually `3m_xmas_gift`.

Players can pick up these gifts and carry them to the main tree to receive a random ornament.

---

## Toys (placeable props)

```lua
Config.Toys = {
    ['snowball'] = {
        prop          = 'xm3_prop_xm3_snowman_01b',
        spawnDistance = 1.0,
        faceAway      = true,
        headingOffset = 0.0,
        zOffset       = 0.0,
        interactive   = true
    }
}
```

- Map inventory item names to world props.
- Fields:
  - `prop`: model name to spawn.
  - `spawnDistance`: distance in front of player to spawn.
  - `faceAway`: if `true`, prop faces away from player.
  - `headingOffset`: rotation offset.
  - `zOffset`: vertical offset.
  - `interactive`:
    - `true` – use preview/placement mode (arrows + Q/R + PageUp/PageDown).
    - `false` – spawn directly.

You can add as many entries as you want.

---

## Ped Toys (shoulder plushes)

```lua
Config.PedToys = {
    ['xmas_plush_elf'] = {
        prop     = 'sum_prop_sum_arcade_plush_01a',
        bone     = 10706,
        offset   = vector3(0.08, -0.02, 0.05),
        rotation = vector3(0.0, 0.0, 190.0)
    },
    ['xmas_plush_elf2'] = { ... },
    ['xmas_plush_elf3'] = { ... },
    ['xmas_plush_elf5'] = { ... },
    ['xmas_plush_elf7'] = { ... }
}
```

- Each key is an **item name** which toggles a plush on the player’s shoulder.
- Fields:
  - `prop`: model name.
  - `bone`: ped bone index (e.g. 10706 = shoulder).
  - `offset`: `vector3` position offset.
  - `rotation`: `vector3` rotation.

Logic:

- No toy → use item → attach.
- Same toy active → use item → detach.
- Different toy active → show notification (no swap).

---

## Santa Delivery Quest

```lua
Config.Quest = {
    enabled = true,

    NPC = {
        model  = 's_m_m_mariachi_01',
        coords = vector4(160.7701, -1009.3114, 28.5505, 158.7634)
    },

    Vehicle = {
        model        = 'burrito3',
        coords       = vector4(159.5688, -1012.4126, 29.3917, 66.7903),
        returnCoords = vector4(159.5688, -1012.4126, 29.3917, 66.7903),
        returnRadius = 5.0
    },

    TreeXP = 20,

    RewardItems = {
        { item = 'xmas_star',    min = 1, max = 3 },
        { item = 'snowball',     min = 1, max = 2 },
        { item = 'santa_cookie', min = 1, max = 1 }
    },

    KeyReward = {
        item   = 'xmas_key',
        chance = 100
    },

    Deliveries = {
        { coords = vector3(125.0, -1045.0, 29.2), radius = 25.0 },
        -- ...
    }
}
```

- `enabled`: master on/off.
- `NPC`: quest giver ped.
- `Vehicle`:
  - `model`: van model.
  - `coords`: spawn position.
  - `returnCoords`, `returnRadius`: where the player must return the van.
- `TreeXP`: how much global tree XP to add when quest is completed.
- `RewardItems`: random item rewards for the player.
- `KeyReward`:
  - `item`: calendar key item.
  - `chance`: % chance to also give a key on completion.
- `Deliveries`: positions and radius for each drop-off.

---

## Donation Items

```lua
Config.DonationItems = {
    xmas_ball           = { xp = 2 },
    xmas_garland        = { xp = 4 },
    xmas_star           = { xp = 6 },
    santa_cookie        = { xp = 8 },
    snowball            = { xp = 10 },
    candy_cane          = { xp = 12 },
    exclusive_xmas_hat  = { xp = 16 }
}
```

- Keys are **item names**, values define how much XP they give to the global tree.
- Used when the player chooses *Donate ornaments* on the main tree.
- You can modify, add or remove entries freely.

---

## Calendar Rewards

```lua
Config.CalendarRewards = {
    [1]  = { items = { { name = 'xmas_plush_elf2', count = 2  } } },
    [2]  = { items = { { name = 'snowball',        count = 10 } } },
    [3]  = { items = { { name = 'candy_cane',      count = 3  } } },
    -- ...
    [24] = { items = { { name = 'xmas_star',       count = 1  } } },
    [25] = { items = { { name = 'money',           count = 24000 } } }
}
```

- Indexed by **event day** (`1` = first day of `Config.TimeWindow.start`).
- Each entry:
  - `items` – array of `{ name = <itemName>, count = <number> }`
  - The special `"money"` name pays cash (through `Framework.AddMoney`).

You can fully customize this table to match your economy and item set.
