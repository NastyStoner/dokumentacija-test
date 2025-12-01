---
title: Usage
parent: stands
nav_order: 3
---


# Usage

## Player Flow
1. **Open Stand** at a placed location (via ox_target).
2. If **Unowned** → buy the stand (requires `Config.ShopPrice` cash).
3. If **Owner** → use the **Manage** tab to:
   - Add items from your inventory to stand stock
   - Set **price** and **quantity**
   - **Withdraw** stand balance
   - **Sell** the stand back to the city
4. Any player can use the **Shop** tab to purchase available items (cash).
5. Stock and balance update immediately; all changes are saved to `standovi.json`.

## Owner Tips
- Keep prices reasonable; there’s no global tax in this version.
- Withdraw often to avoid leaving large balances in the stand.
- Use blacklist and weapon blocking to keep your market clean.

## Admin Tips
- To wipe or migrate data, back up and edit `standovi/standovi.json` while the server is stopped.
- Stands are defined in `config.lua` (`Config.Stands`); restart after changes.
- If you remove a stand from the config, consider clearing it from JSON too.

## Localization
Change `Config.Locale` to `hr` for Croatian or add your own in `locales/`.
