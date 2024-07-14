# ha-sessy-examples
Example automations for using Sessy with Home Assistant

## Sessy X-on the meter (XOM) automation
Controls Sessy to maintain value X on the grid meter.
- Load balancing across multiple Sessy's
- Except certain load via offset entities (e.g. EV charger)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/gigawebs/ha-sessy-examples/blob/main/blueprints/automation/sessy/sessy-xom.yaml)


## Sessy Full charge script
Charges Sessy to the specified amount, then activates a certain strategy.
- Supports multiple Sessy's
- Script ends when all Sessy's report full, above set percentage, or after timeout passes.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fgigawebs%2Fha-sessy-examples%2Fblob%2Fmain%2Fblueprints%2Fscript%2Fsessy%2Fsessy-full-charge.yaml)


[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fgigawebs%2Fha-sessy-examples%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fsessy%2Fsessy-xom.yaml)

### YAML-code voor het aanmaken van de helpers

Voeg het volgende toe aan je `configuration.yaml`:

```yaml
automation:
  - alias: Create Helpers for Sessy Battery Control
    trigger:
      - platform: homeassistant
        event: start
    action:
      - service: input_number.set_value
        data:
          entity_id: input_number.setpoint_smoothing_value
          value: 100
      - service: input_number.set_value
        data:
          entity_id: input_number.setpoint_optimal_value
          value: 1200
      - service: input_number.set_value
        data:
          entity_id: input_number.setpoint_min
          value: -12000
      - service: input_number.set_value
        data:
          entity_id: input_number.setpoint_max
          value: 12000
      - service: input_number.set_value
        data:
          entity_id: input_number.setpoint_target_value
          value: 0
      - service: input_datetime.set_datetime
        data:
          entity_id: input_datetime.update_interval
          time: "00:01:00"
      - service: input_datetime.set_datetime
        data:
          entity_id: input_datetime.update_timeout
          time: "00:05:00"

input_number:
  setpoint_smoothing_value:
    name: Smoothing value
    initial: 100
    min: 50
    max: 2000
    step: 1
    mode: slider

  setpoint_optimal_value:
    name: Max optimal power
    initial: 1200
    min: 50
    max: 2200
    step: 1
    mode: slider

  setpoint_min:
    name: Minimum power setpoint
    initial: -12000
    min: -12000
    max: 12000
    step: 1

  setpoint_max:
    name: Maximum power setpoint
    initial: 12000
    min: -12000
    max: 12000
    step: 1

  setpoint_target_value:
    name: Target value
    initial: 0
    min: -3600
    max: 3600
    step: 1

input_datetime:
  update_interval:
    name: Update interval
    has_time: true
    has_date: false
    initial: "00:01:00"

  update_timeout:
    name: Update timeout
    has_time: true
    has_date: false
    initial: "00:05:00"
```

## Sessy Dynamic Schedule with ApexCharts
Display the power schedule and energy prices for the dynamic strategy within Home Assistant using ApexCharts. 

[![Dynamic schedule in a graph](cards/DynamicSchedule.png)](cards/DynamicSchedule.md)

[Learn More](cards/DynamicSchedule.md)