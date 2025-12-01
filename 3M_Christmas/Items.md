---
title : Items
parent: 3M Christmas Event
nav_order: 3
---

# Items & Integration

This resource expects several items to exist in your inventory system.  
Below is a list of the important ones and how they are used.

---

## Overview

### Core gameplay items

- `xmas_key`  
  - Calendar key for the current day.  
  - Referenced by `Config.CalendarKeyItem`.  
  - Given as a quest reward (`Config.Quest.KeyReward`) or by your own systems.

- `xmas_ball`, `xmas_garland`, `xmas_star`, `santa_cookie`, `snowball`, `candy_cane`, `exclusive_xmas_hat`  
  - Donation items for the main tree.  
  - Defined in `Config.DonationItems` with XP values.

---

### Toys / Props

- `snowball`
  - **Two uses:**
    - Donation item (if defined in `Config.DonationItems`).
    - Placeable toy configured in `Config.Toys['snowball']` → spawns a `xm3_prop_xm3_snowman_01b` snowman.
  - On QBCore, registered as a usable item automatically.

- `3m_xmas_gift` (prop)
  - World prop model for carryable gifts at Santa spots and in the quest.
  - **Not** an inventory item by default.
  - Configured via:
    - `Config.CarryGiftModel` (carry animation)
    - `Config.SantaSpots[i].gifts[*].model`

---

### Shoulder Plush Toys

Configured in `Config.PedToys`:

- `xmas_plush_elf`
- `xmas_plush_elf2`
- `xmas_plush_elf3`
- `xmas_plush_elf5`
- `xmas_plush_elf7`

Usage:

- These items are **non-consumable toggles**:
  - Use item → attach plush to shoulder.
  - Use the same item again → detach.
  - With a different toy already attached → only notification.

On QBCore, `xmas_plush_elf` is automatically registered as usable; you should make the rest usable if you want to use them too.

---

### Food / Buff Items

- `santa_cookie`
- `candy_cane`

On QBCore they are registered as usable items and emit client events:

- `xmas:food:UseSantaCookie`
- `xmas:food:UseCandyCane`

This resource **does not** implement the visual/buff effects – it only emits the events.  
You can handle them in your own script (see [Developer API](Developers.md#food-events) for an example).

---

## Example item definitions

### ESX (items table)

Example SQL (adjust fields to your schema):

```sql
INSERT INTO items (name, label, weight, rare, can_remove) VALUES
  ('xmas_key',          'Christmas Key', 1, 0, 1),
  ('xmas_ball',         'Christmas Bauble', 1, 0, 1),
  ('xmas_garland',      'Christmas Garland', 1, 0, 1),
  ('xmas_star',         'Tree Star', 1, 0, 1),
  ('santa_cookie',      'Santa Cookie', 1, 0, 1),
  ('candy_cane',        'Candy Cane', 1, 0, 1),
  ('snowball',          'Snowball', 1, 0, 1),
  ('exclusive_xmas_hat','Exclusive Xmas Hat', 1, 0, 1),
  ('xmas_plush_elf',    'Xmas Plush Elf', 1, 0, 1),
  ('xmas_plush_elf2',   'Xmas Plush Elf 2', 1, 0, 1),
  ('xmas_plush_elf3',   'Xmas Plush Elf 3', 1, 0, 1),
  ('xmas_plush_elf5',   'Xmas Plush Elf 5', 1, 0, 1),
  ('xmas_plush_elf7',   'Xmas Plush Elf 7', 1, 0, 1);
```

---

### QBCore (items.lua)

Example entries:

```lua
['xmas_key'] = {
    name = 'xmas_key',
    label = 'Christmas Key',
    weight = 0,
    type = 'item',
    image = 'xmas_key.png',
    unique = false,
    useable = true,
    shouldClose = true
},

['santa_cookie'] = {
    name = 'santa_cookie',
    label = 'Santa Cookie',
    weight = 0,
    type = 'item',
    image = 'santa_cookie.png',
    unique = false,
    useable = true,
    shouldClose = true
},

['candy_cane'] = {
    name = 'candy_cane',
    label = 'Candy Cane',
    weight = 0,
    type = 'item',
    image = 'candy_cane.png',
    unique = false,
    useable = true,
    shouldClose = true
},

['snowball'] = {
    name = 'snowball',
    label = 'Snowball',
    weight = 0,
    type = 'item',
    image = 'snowball.png',
    unique = false,
    useable = true,
    shouldClose = true
},

['xmas_plush_elf'] = {
    name = 'xmas_plush_elf',
    label = 'Xmas Plush Elf',
    weight = 0,
    type = 'item',
    image = 'xmas_plush_elf.png',
    unique = false,
    useable = true,
    shouldClose = true
},
-- add xmas_plush_elf2/3/5/7 similarly
```

The script automatically registers the following QBCore usable items:

- `snowball` → placeable toy
- `xmas_plush_elf` → ped toy toggle
- `santa_cookie` → emits `xmas:food:UseSantaCookie`
- `candy_cane` → emits `xmas:food:UseCandyCane`

You can extend this behaviour or mirror it for other plush items as needed.

---

### ox_inventory

Example `items.lua` entries:

```lua
['xmas_key'] = {
    label = 'Christmas Key',
    weight = 0,
    stack = true,
    close = true,
    description = 'Unlocks today's Advent Calendar window.'
},

['xmas_ball'] = {
    label = 'Christmas Bauble',
    weight = 0,
    stack = true,
    close = false,
    description = 'Donate at the main tree to increase Christmas Spirit.'
},

['snowball'] = {
    label = 'Snowball',
    weight = 0,
    stack = true,
    close = true,
    description = 'Throw it or place it as a snowman.',
    consume = 1
},
```

If you want ox_inventory to trigger the toy or ped-toy logic directly, you can run a server-side handler like:

```lua
exports.ox_inventory:registerUsage('snowball', function(source, item)
    TriggerClientEvent('xmas:toys:UseFromInventory', source, { item = 'snowball' })
end)
```

---

For more advanced integrations (XP, food effects, UI), see [Developer API](Developers.md).
