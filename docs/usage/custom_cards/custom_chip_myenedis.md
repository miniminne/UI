---
title: chip_myenedis
hide:
  - toc
---

<!-- markdownlint-disable MD046 -->

# Custom-chip "My Enedis"

The `myenedis` you can control a input_number entity

![ALL](../../assets/img/ulm_chips/chip_myenedis_all.png)

![HP / HC](../../assets/img/ulm_chips/chip_myenedis_hp_hc.png)

## Credits

Author: acesyde - 2021
Version: 1.0.0

## Changelog

<details>
<summary>1.0.0</summary>
Initial release
</details>

## Why not the default `chip_power_consumption`

- The my enedis integration doesn't split the cost and the consumption in a specific sensor
- All values are in the main sensor under the `attributes` item
- Split `Heures Creuses (HC)` and `Heures pleines (HP)`

## Usage

```yaml
- type: "custom:button-card"
  template: custom_chip_myenedis
  entity: sensor.myenedis_123456789
  variables:
    ulm_chip_separate_hp_hc: true
```

## Requirements

[My Enedis integration](https://github.com/saniho/apiEnedis)

## Variables

| Variable                | Default | Required         | Notes                      |
| ----------------------- | ------- | ---------------- | -------------------------- |
| entity                  |         | :material-check: | Your MyEnedis Sensor       |
| ulm_chip_separate_hp_hc | false   | :material-close: | Separate HC/HC in the chip |

## Template code

```yaml
---
---
### Chip My Enedis ###
custom_chip_myenedis:
  template:
    - "chips"
    - "ulm_language_variables"
  variables:
    ulm_chip_separate_hp_hc: false
    ulm_chip_unit_of_measurement: "[[[ return entity.attributes.unit_of_measurement ]]]"
  triggers_update: "all"
  label: |
    [[[
      var result = "💰 " +  parseFloat(entity.attributes.daily_cost).toFixed(1) + " €";

      if(variables.ulm_chip_separate_hp_hc) {
        result += " ☀️ " + parseFloat(entity.attributes.yesterday_HC).toFixed(1) + " " +  variables.ulm_chip_unit_of_measurement;
        result += " 🌑 " + parseFloat(entity.attributes.yesterday_HP).toFixed(1) + " " +  variables.ulm_chip_unit_of_measurement;
      } else {
        result += " ⚡ " + parseFloat(entity.attributes.yesterday_HCHP).toFixed(1) + " " +  variables.ulm_chip_unit_of_measurement;
      }

      return result;
    ]]]
```
