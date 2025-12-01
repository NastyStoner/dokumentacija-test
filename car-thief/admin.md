---
title: Admin & Commands
parent: Car Thief
nav_order: 5
---

# Admin & Commands

## ACE permissions
- `/reloadvehprices` — reload `data/vehicle_prices.json`  
  ACE: `automafija.reloadvehprices` (or `command.reloadvehprices`)
- `/setvehprice <model> <price>` — write to JSON, reload, and apply  
  ACE: `automafija.setvehprice`
- `/getvehprice <model>` — prints current price (or default)  
  ACE: `automafija.getvehprice`

Add to your `server.cfg` (example):
```
add_ace group.admin automafija.reloadvehprices allow
add_ace group.admin automafija.setvehprice allow
add_ace group.admin automafija.getvehprice allow
```

## Discord logs
Set `Config.Webhook` to a Discord webhook URL to receive embeds for stolen/returned/scrapped events.
