---
title : Installation
parent: beekeeper
nav_order: 5
---

# Installation

## Requirements
- **Es_extended 1.12.4 or above**
- **oxmysql**
- **ox_lib**
- **Ox_inventory**
- **Ox_Target**

## How to Install Script

- download resource from **[CFX Portal](https://portal.cfx.re/assets)** unzip and put in your resources folder

## Server.cfg
```
ensure oxmysql
ensure ox_lib
ensure ox_inventory
ensure ox_target
ensure beekeeper
```

- add items to your ox inventory 
## Ox Inventory items 
```

['beehive_box'] = {
    label = 'Beehive Box',
    weight = 1000,
    stack = true,
    close = true,
    description = 'Plave Beehive Box',
    client = {
        event = 'beekeeping:placeBeehive' 
    }
},

['bee_food'] = {
    label = 'Bee Food',
    weight = 300,
    stack = true,
    close = true,
    description = 'Specific Bee Food',
},

['water_bottle'] = {
    label = 'Water for bees',
    weight = 200,
    stack = true,
    close = true,
    description = 'Bee water.',
},

['cleaning_agent'] = {
    label = 'Cleaning Agent',
    weight = 250,
    stack = true,
    close = true,
    description = Cleaning Agent',
},

['bee_heal'] = {
    label = 'Bee Heal',
    weight = 200,
    stack = true,
    close = true,
    description = 'Bee Heal',
},

['glass_jar'] = {
    label = 'Empty Jar',
    weight = 100,
    stack = true,
    close = true,
    description = 'Used for honey',
},
['honey_jar'] = {
    label = 'Honey Yar',
    weight = 100,
    stack = true,
    close = true,
    description = 'Honey Yar.',
},

['golden_honey'] = {
    label = 'Golden Honey',
    weight = 300,
    stack = true,
    close = true,
    description = 'Golden Honey',
},

['dark_honey'] = {
    label = 'Dark Honey',
    weight = 300,
    stack = true,
    close = true,
    description = Dark honey',
},
['propolis'] = {
    label = 'Propolis',
    weight = 100,
    stack = true,
    close = true,
    description = 'Propolis extracted from Bumblebee hives',
},
['propolis_jar'] = {
    label = 'Tegla Propolisa',
    weight = 20,
    stack = true,
    close = true,
    description = 'Tegla Propolisa ',
},
['royal_jelly'] = {
    label = 'matična mliječ',
    weight = 20,
    stack = true,
    close = true,
    description = 'matična mliječ',
},
['bee_venom'] = {
    label = 'Pčeljinji otrov',
    weight = 20,
    stack = true,
    close = true,
    description = 'Pčeljinji otrov',
},
['worker_bumbar'] = {
    label = 'Bumblebee',
    weight = 100,
    stack = true,
    close = true,
    description = 'Worker Bumblebee.',
},

['queen_bee'] = {
    label = 'Queen Bee',
    weight = 150,
    stack = false,
    close = true,
    description = Queen Bee.',
},
['bumbar_queen'] = {
    label = 'Queen Bumblebee',
    weight = 150,
    stack = false,
    close = true,
    description = Queen Bumblebee.',
},

['worker_bee'] = {
    label = 'Bees',
    weight = 100,
    stack = true,
    close = true,
    description = 'Worker Bee.',
},
['worker_bumbar'] = {
    label = 'Bumblebee',
    weight = 100,
    stack = true,
    close = true,
    description = 'Worker Bumblebee.',
},


```

## Import Sql
```
CREATE TABLE IF NOT EXISTS `beehives` (
  `id` INT AUTO_INCREMENT PRIMARY KEY,
  `owner_identifier` VARCHAR(64) NOT NULL,
  `x` DOUBLE NOT NULL, `y` DOUBLE NOT NULL, `z` DOUBLE NOT NULL,
  `heading` DOUBLE NOT NULL DEFAULT 0,
  `type` ENUM('bee','bumbar') NOT NULL DEFAULT 'bee',
  `queen` TINYINT(1) NOT NULL DEFAULT 0,
  `bees` INT NOT NULL DEFAULT 0,
  `food` INT NOT NULL DEFAULT 0,
  `water` INT NOT NULL DEFAULT 0,
  `clean` INT NOT NULL DEFAULT 0,
  `hive_hp` INT NOT NULL DEFAULT 100,
  `bee_hp` INT NOT NULL DEFAULT 100,
  `product` INT NOT NULL DEFAULT 0,
  `golden_honey` INT NOT NULL DEFAULT 0,
  `dark_honey` INT NOT NULL DEFAULT 0
);
```
# Restart Server and enjoy product




