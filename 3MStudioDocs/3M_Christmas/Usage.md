---
title : Usage
parent: 3M Christmas Event
nav_order: 4
---

# Usage (Gameplay Flow)

This section explains how the script works from the player perspective.

---

## 1. Global Christmas Tree

Location is set via `Config.Tree.coords`.

### Interactions (via target)

- **Open Christmas calendar**  
  Label: `Open Christmas calendar` (`target_open_calendar`)  
  → Tries to open the Advent Calendar NUI for today’s window.

- **Donate ornaments**  
  Label: `Donate ornaments` (`target_donate`)  
  → Consumes one donation item from `Config.DonationItems` and adds XP to the tree.

### Rules

- Each donation:
  - Consumes *one* ornament item from the player’s inventory.
  - Adds XP to **global tree** and to the player’s personal total.
- Donating is limited by `Config.DonationsPerDay` per player per real day.
- Tree stage changes automatically based on total XP and `Config.Tree.stages`.

---

## 2. Santa Spots (local trees & gifts)

Santa spots are defined in `Config.SantaSpots`.

At each spot:

1. A small decorative tree is spawned.
2. Several `3m_xmas_gift` props are placed around it.
3. Using target on a gift:

   - Prompts the player to **pick up the gift**.
   - Attaches a carry prop defined by `Config.CarryGiftModel`.
   - Player can only carry one gift at a time.

4. The player must walk to the **main Christmas tree** and use the gift target:
   - The script turns the gift in and rewards:
     - A **random ornament item** (configured server-side).
     - XP is not directly added here; instead ornaments can be donated to the tree.

5. Each player can receive only `Config.SantaSpotDailyLimit` rewards per real day.

---

## 3. Advent Calendar

The calendar is opened from the main tree (first target option).

### Requirements

- The global event must be active (date within `Config.TimeWindow` or `Config.Debug.ForceActive = true`).
- The player must have the **calendar key item** defined by `Config.CalendarKeyItem` for the current event day.
- The player must not have already claimed that day’s reward.

### UI Behaviour

- Shows 25 windows:
  - Locked / available / claimed states per day.
- On claim:
  - Server validates eligibility.
  - Rewards items defined in `Config.CalendarRewards[day]`.
  - Marks the day as claimed and updates NUI.

---

## 4. Santa Delivery Quest

The quest is controlled by `Config.Quest`.

### Flow

1. **Talk to the Santa NPC**

   - NPC is spawned at `Config.Quest.NPC.coords`.
   - Target label: `Help Santa Claus` (`target_santa_work`).
   - The player can start the quest once per event day.

2. **Receive the van**

   - Vehicle spawns at `Config.Quest.Vehicle.coords`.
   - Keys are handled by `Config.Keys.provider`.

3. **Deliver gifts**

   - There are multiple delivery points in `Config.Quest.Deliveries`.
   - At each point:
     - Player drives into the radius.
     - Leaves the van, picks up a gift from the van (prompt / target).
     - Walks to the exact drop point and delivers the package.

4. **Return phase**

   - After all deliveries, the objective switches to returning the van.
   - Player must park inside `Config.Quest.Vehicle.returnRadius` near `returnCoords`.
   - Once done, the quest completes.

5. **Rewards**

   - Global tree XP: `Config.Quest.TreeXP`.
   - Random reward items: `Config.Quest.RewardItems`.
   - Chance to receive `Config.Quest.KeyReward.item` (calendar key) with `chance` %.

Quest progress and daily completion are stored in the `xmas_players` table.

---

## 5. Toys (placeable props)

Configured under `Config.Toys`.

### Using a toy (example: `snowball`)

- When used (via inventory / QBCore usable item / your own code), client event `xmas:toys:UseFromInventory` runs.
- If `interactive = true`, the player enters **placement mode**:

  - **Controls:**
    - Arrow keys – move the previewed prop.
    - `Q` / `R` – rotate.
    - `PageUp` / `PageDown` – adjust height.
    - `ENTER` – confirm placement.
    - `BACKSPACE` – cancel (prop is removed and item refunded).

- On confirm, a server-side record of the prop is stored so it can be cleaned up properly.

- When interacting with a placed toy (e.g. snowman), the player can pick it up to get the item back.

---

## 6. Shoulder Plush Toys

Ped toys configured via `Config.PedToys`.

### Behaviour

- When the player uses a plush item (e.g. `xmas_plush_elf`):

  - If no plush is active:
    - A prop is attached to a shoulder bone with configured offset/rotation.
  - If the same plush is already active:
    - It is removed from the shoulder.
  - If a different plush is active:
    - A notification is shown (“you already have a plush equipped”) and nothing changes.

State is cleaned up when the player disconnects or the resource stops.

---

## 7. Weather

If `Config.ChangeWeather.enabled = true`:

- When the event is active:
  - Server locks weather to `Config.ChangeWeather.weatherType`.
  - Time freeze / dynamic weather / blackout are handled based on the provider.

- Clients ask the server for the current weather status on join (`xmas:server:Weather_RequestSync`).

If you don’t want weather to be affected, set:

```lua
Config.ChangeWeather.enabled = false
-- or:
Config.ChangeWeather.provider = 'none'
```

---

That’s all players need to know. For scripting integrations (XP hooks, custom buffs for cookies/candy cane, custom UI, etc.) see [Developers.md](Developers.md).
