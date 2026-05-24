# Z2M Dining Room Automation Conversion Template

**Status:** Prepared for Phase 4 (ready to apply once dimmer pairs to Z2M)  
**Date Prepared:** 2026-05-02

---

## Current Z2M Status

### Dining Room Lights (Phase 2B Complete)
- ✅ 3 of 4 lights paired to Z2M as individual devices:
  - `light.0x00178801027c1190` (Hue white ambiance BR30)
  - `light.0x00178801027c0fe4` (Hue white ambiance BR30)
  - `light.0x00178801027c11c4` (Hue white ambiance BR30)

### Dining Room Group (Phase 3 Pending)
- Will create Z2M group: `light.dining_room_lights` containing above 3 devices
- Group name will replace old Hue `light.dining_room` group entity

---

## Automation 1: Brighten Dining Room Lights

### BEFORE (Current Hue Format)

```yaml
automation.brighten_dining_room_lights:
  alias: Brighten Dining Room Lights
  trigger:
    - device_id: be9f7acf4e8cab1d5de407601ce69dab
      domain: hue
      type: initial_press
      subtype: 2  # Button 2 = Brighten
      platform: device
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room
      data:
        brightness_step: 51  # ~20% increase
  condition:
    - condition: numeric_state
      entity_id: light.dining_room
      attribute: brightness
      below: 250
  mode: single
  enabled: true
```

### AFTER (Z2M Format — Apply Once Dimmer Pairs)

```yaml
automation.brighten_dining_room_lights:
  alias: Brighten Dining Room Lights
  trigger:
    - platform: event
      event_type: zigbee2mqtt_event
      event_data:
        device_id: <DIMMER_DEVICE_ID>  # Will be populated when dimmer pairs
        event: 1002  # Z2M button 2 press event code (verify from device page)
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room_lights  # Updated to Z2M group
      data:
        brightness_step: 51  # ~20% increase (same as before)
  condition:
    - condition: numeric_state
      entity_id: light.dining_room_lights  # Updated to Z2M group
      attribute: brightness
      below: 250
  mode: single
  enabled: true
```

### Changes Required:
1. **Trigger platform:** `device` → `event`
2. **Event type:** `hue` → `zigbee2mqtt_event`
3. **Device ID:** Update to Z2M dimmer device_id (will be obtained when dimmer pairs)
4. **Event code:** `subtype: 2` → `event: 1002` (confirm from Z2M device database for Hue dimmer)
5. **Entity ID:** `light.dining_room` → `light.dining_room_lights` (both in action and condition)

---

## Automation 2: Decrease Dining Room Lights

### BEFORE (Current Hue Format)

```yaml
automation.decrease_dining_room_lights:
  alias: Decrease Dining Room Lights
  trigger:
    - device_id: be9f7acf4e8cab1d5de407601ce69dab
      domain: hue
      type: initial_press
      subtype: 3  # Button 3 = Dimmer
      platform: device
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room
      data:
        brightness_step: -51  # ~20% decrease
  condition:
    - condition: numeric_state
      entity_id: light.dining_room
      attribute: brightness
      above: 5
  mode: single
  enabled: true
```

### AFTER (Z2M Format — Apply Once Dimmer Pairs)

```yaml
automation.decrease_dining_room_lights:
  alias: Decrease Dining Room Lights
  trigger:
    - platform: event
      event_type: zigbee2mqtt_event
      event_data:
        device_id: <DIMMER_DEVICE_ID>  # Will be populated when dimmer pairs
        event: 1003  # Z2M button 3 press event code (verify from device page)
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room_lights  # Updated to Z2M group
      data:
        brightness_step: -51  # ~20% decrease (same as before)
  condition:
    - condition: numeric_state
      entity_id: light.dining_room_lights  # Updated to Z2M group
      attribute: brightness
      above: 5
  mode: single
  enabled: true
```

### Changes Required:
1. **Trigger platform:** `device` → `event`
2. **Event type:** `hue` → `zigbee2mqtt_event`
3. **Device ID:** Update to Z2M dimmer device_id (will be obtained when dimmer pairs)
4. **Event code:** `subtype: 3` → `event: 1003` (confirm from Z2M device database for Hue dimmer)
5. **Entity ID:** `light.dining_room` → `light.dining_room_lights` (both in action and condition)

---

## Next Steps

### IF Dimmer Pairs Successfully to Z2M:
1. Obtain Z2M dimmer device_id from HA > Settings > Devices & Services
2. Look up Z2M device database page for dimmer model to confirm event codes (1002, 1003)
3. Replace placeholder `<DIMMER_DEVICE_ID>` with actual device ID
4. Replace event codes if different from 1002/1003
5. Update both automations using HA UI or this template
6. Test button presses with brightness automation trace

### IF Dimmer Does NOT Pair to Z2M:
- **Option B1:** Disable both automations (user loses dimmer control)
  - Set `enabled: false` on both automations
  - Keep Z2M lights accessible via HA UI, voice, and scenes
  
- **Option B2:** Use alternative trigger (e.g., voice command, time-based, scene switch)
  - Replace dimmer trigger with time-based: `platform: time` at specific times
  - Or use voice command trigger: `platform: conversation`
  - Keep 3 Z2M lights fully functional

---

## Dimmer Pairing Status

**Current:** Awaiting Hue dimmer reset and pairing to Z2M  
**Permit Join:** ACTIVE (254 sec window)  
**Compatibility:** To be confirmed (see z2m_dimmer_event_reference.md for check procedure)

**Known Hue Dimmer Models and Z2M Support:**
- RWL021: ✅ Supported
- LCL001: ✅ Supported  
- LCL002: ✅ Supported
- RWL020: ❌ Not supported
- RWL030: ❌ Not supported

Check your dimmer model against https://www.zigbee2mqtt.io/devices/

---

**Template prepared by:** Automated migration preparation  
**Ready for Phase 4:** Once dimmer pairs in Phase 2C
