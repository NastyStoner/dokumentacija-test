---
title : Developers
parent: 3M Christmas Event
nav_order: 5
---

# Developer API (Exports & Events)

This resource exposes a few useful integration points for other scripts:

- A notify export
- A tree XP helper function
- Some server events you can safely trigger
- A couple of client events you may want to listen to

---

## 1. Notify export

File: `notify.lua`

```lua
exports('Notify', Notify)
```

### Signature

```lua
Notify(src, level, key, replacements)
```

- `src`: player server ID (`number`) or `0/nil` for console-only logging.
- `level`: `'info' | 'success' | 'error'` (mapped to provider).
- `key`: locale key string (e.g. `'tree_donation_ok'`).
- `replacements`: optional table of placeholders to replace in the localized string.

Example localized string:

```lua
['tree_donation_ok'] = 'Thank you! Your donation increased the Christmas spirit (+{xp}).'
```

### Example usage

```lua
-- From another server resource:
exports['Christmas']:Notify(src, 'success', 'tree_donation_ok', { xp = 10 })

-- For errors:
exports['Christmas']:Notify(src, 'error', 'event_inactive')
```

Notification provider is controlled via `Config.Notify.provider`.

---

## 2. Tree XP helper

File: `server/main.lua`

```lua
function Xmas_AddTreeXP(src, xpGain)
    -- returns true on success, false on failure
end
```

### Behaviour

- Adds `xpGain` XP to:
  - `xmas_players.total_xp` for the given player.
  - Global tree XP (`xmas_tree_progress.xp`).
- Recalculates tree stage based on `Config.Tree.stages`.
- Persists the new state and syncs tree status to the client:
  - Triggers `xmas:client:SyncTree` for that player.

### Parameters

- `src`: player server ID.
- `xpGain`: positive number; values ≤ 0 are ignored.

### Example: give tree XP for another Christmas job

```lua
-- In some other job script:
local function OnCustomChristmasJobCompleted(src)
    -- Give the player 15 XP towards the global tree
    if Xmas_AddTreeXP then
        Xmas_AddTreeXP(src, 15)
    end
end
```

> **Note:** `Xmas_AddTreeXP` is a global function in the same runtime.  
> Make sure the `Christmas` resource is started before your script that calls this.

---

## 3. Server events you can trigger

### 3.1 Calendar

#### `xmas:server:RequestOpenCalendar`

- **Direction:** client → server  
- **Used by:** main Christmas tree target
- **Effect:**  
  - Validates event date, tree state, player key (`Config.CalendarKeyItem`) and per-day limits.
  - Sends `xmas:client:OpenCalendar` with calendar data.

You can trigger this from another client UI if you want to open the calendar from a different place:

```lua
-- client side in another script:
TriggerServerEvent('xmas:server:RequestOpenCalendar')
```

#### `xmas:server:ClaimCalendar`

- **Direction:** client → server  
- **Parameters:** `day` (number, event day index)  
- **Effect:**  
  - Validates that the day is claimable and matches the event window.
  - Gives rewards from `Config.CalendarRewards[day]`.
  - Marks the day as claimed in `xmas_players.calendar_claims`.
  - Sends `xmas:client:CalendarUpdate` with latest data.

Normally this is only triggered by the built-in NUI.  
You might use it if you are replacing the UI but want to reuse the server logic.

---

### 3.2 Donations

#### `xmas:server:DonateToTree`

- **Direction:** client → server  
- **Used by:** main tree target

Behaviour:

1. Finds the first matching item from `Config.DonationItems` in the player’s inventory.
2. Removes one unit of that item.
3. Calls `Xmas_AddTreeXP(src, itemConfig.xp)`.
4. Applies daily donation limits.

You can call this from another client script if you want a different interaction (e.g. menu):

```lua
TriggerServerEvent('xmas:server:DonateToTree')
```

---

### 3.3 Quest

#### `xmas:quest:Start`

- **Direction:** client → server  
- **Effect:**  
  - Starts the Santa delivery quest if:
    - Event is active
    - Quest is enabled in `Config.Quest`
    - Player hasn’t completed it yet today
  - Spawns the van, gives keys, sends quest data to client.

Normally triggered by the Santa NPC target.  
You can also start the quest from somewhere else:

```lua
-- client
TriggerServerEvent('xmas:quest:Start')
```

`xmas:quest:NearDrop`, `xmas:quest:StepComplete` and `xmas:quest:ReturnDone` are internal and should not be triggered externally.

---

### 3.4 Weather sync

#### `xmas:server:Weather_RequestSync`

- **Direction:** client → server  
- **Effect:**  
  - Server responds with either:
    - `xmas:client:Weather_Start` if the event is active and weather lock is enabled.
    - `xmas:client:Weather_Stop` otherwise.

Normally called automatically on client init; you generally don’t need to touch this.  
You might use it if you need to re-sync weather for a specific player after some external change.

---



## 4. Client events you may want to listen to

### `xmas:client:SyncTree`

**Parameters:** `xp`, `stageIndex`

Fired:

- On player join (initial sync).
- Whenever the global tree XP or stage changes.

Use this to display custom tree info in your own UI:

```lua
RegisterNetEvent('xmas:client:SyncTree', function(xp, stageIndex)
    -- Update your HUD / NUI with tree progress
end)
```

---

### `xmas:client:OpenCalendar`

**Parameters:** calendar data table

- Contains:
  - Available days
  - Claimed days
  - Today’s day index
  - Whether the key is present, etc.

This is used internally by the NUI app, but you can mirror the behaviour or extend it.

---

### `xmas:client:CalendarUpdate`

**Parameters:** updated calendar data

- Fired after a successful daily claim.
- Use this if you maintain your own UI alongside or on top of the included one.

---

### `xmas:client:BeginCarryGift` / `xmas:client:EndCarryGift`

Fired when:

- Player starts carrying a Santa gift (`BeginCarryGift`).
- Player stops carrying or turns in the gift (`EndCarryGift`).

You can hook into these to add:

- Custom animations
- Extra HUD elements
- Sounds

Example:

```lua
RegisterNetEvent('xmas:client:BeginCarryGift', function(spotId, model)
    -- Show custom “Carrying Christmas Gift” indicator
end)

RegisterNetEvent('xmas:client:EndCarryGift', function()
    -- Hide indicator
end)
```

---

## 5. Notes

- All database reads/writes go through `oxmysql` and the helper functions in `server/main.lua`.
- If you extend the system, reuse `Xmas_AddTreeXP` instead of writing to tables directly.
- Do not emit low-level events like `xmas:toys:Pickup` or `xmas:toys:CancelPlacement` yourself – let the built-in placement flow handle them, or call higher-level APIs like `xmas:toys:UseFromInventory` from inventory use handlers.

If you need extra hooks for a specific integration, the recommended pattern is to:

1. Create your own small wrapper resource.
2. Listen to the events above.
3. Use framework & inventory exports to adjust your own systems accordingly.
