# Z2M Dimmer Event Codes & Automation Trigger Reference

**Purpose:** Help understand how to convert Hue dimmer automations to Z2M format

---

## Current Hue Dimmer Setup

**Device:** Hue dimmer switch 1  
**Device ID:** be9f7acf4e8cab1d5de407601ce69dab  
**Platform:** Hue (native)  
**Integration:** hue

**Current Automations:**

```yaml
automation.brighten_dining_room_lights:
  trigger:
    device_id: be9f7acf4e8cab1d5de407601ce69dab
    domain: hue
    type: initial_press
    subtype: 2  # Button 2
    platform: device

automation.decrease_dining_room_lights:
  trigger:
    device_id: be9f7acf4e8cab1d5de407601ce69dab
    domain: hue
    type: initial_press
    subtype: 3  # Button 3
    platform: device
```

---

## Hue Dimmer Switch Button Layout

```
┌─────────────────┐
│   Button 1      │ ON (press = turn on)
│  (top)          │
├─────────────────┤
│   Button 2      │ BRIGHTEN (press = increase brightness)
│                 │
├─────────────────┤
│   Button 3      │ DIMMER (press = decrease brightness)
│                 │
├─────────────────┤
│   Button 4      │ OFF (press = turn off)
│  (bottom)       │
└─────────────────┘
```

---

## Hue Event Codes

In Hue integration:
- **subtype: 1** = Button 1 (On)
- **subtype: 2** = Button 2 (Brighten)
- **subtype: 3** = Button 3 (Dimmer)
- **subtype: 4** = Button 4 (Off)

---

## Z2M Compatibility Check

### Step 1: Identify Your Dimmer Model

Your Hue dimmer switch likely has one of these models:
- **Hue Dimmer Switch (original)** — IEEE address format: `0x0017880000XXXXXX`
- **Hue Dimmer Switch RWL021** — Common model
- **Hue Dimmer Switch LCL001** — Newer version
- **Hue Dimmer Switch LCL002** — Latest version

**Where to find model:**
1. Home Assistant > Settings > Devices & Services > Hue
2. Find "Hue dimmer switch 1" device
3. Click on it to see IEEE address and model details
4. Or check physical switch for model number

### Step 2: Check Z2M Device Database

Go to: **https://www.zigbee2mqtt.io/devices/**

Search for your exact model. Look for:
- **Supported:** Yes/No
- **Support since:** Z2M version number
- **Exposes:** What device capabilities are exposed

**Example Z2M search result for RWL021:**

```
Device: Hue Dimmer Switch (RWL021)
Supported: ✅ YES
Support since: Z2M 1.x
Exposes: 
  - button (press, hold, release)
  - linkquality
```

If supported, the search page shows:
- Available button events
- What each button press generates
- How to use in automations

### Step 3: Expected Z2M Event Codes

Common Z2M button events (varies by device):

```
Button Press Events:
- 1000 = Button 1 single press
- 1001 = Button 2 single press  
- 1002 = Button 2 single press (brightness up)
- 1003 = Button 3 single press (brightness down)
- 1004 = Button 4 single press (off)
- 2000-2004 = Double press variants
- 3000-3004 = Long press variants
- 4000-4004 = Hold variants
```

**Note:** Exact codes vary per device. Check the Z2M device page for your specific model.

---

## Converting Hue Automations to Z2M

### Conversion Template

**Old (Hue) Format:**
```yaml
automation.brighten_dining_room_lights:
  alias: Brighten Dining Room Lights
  trigger:
    - device_id: be9f7acf4e8cab1d5de407601ce69dab
      domain: hue
      type: initial_press
      subtype: 2
      platform: device
  action:
    - device_id: 04bcba7a94b0a741d75705f9eacfcd65
      domain: light
      entity_id: 66243b98ab049fcf7e372cbb2df1ab04
      type: brightness_increase
  condition:
    - condition: numeric_state
      entity_id: light.dining_room
      attribute: brightness
      below: 100
```

**New (Z2M) Format:**
```yaml
automation.brighten_dining_room_lights:
  alias: Brighten Dining Room Lights
  trigger:
    - platform: event
      event_type: zigbee2mqtt_event
      event_data:
        device_id: abc123def456  # Z2M dimmer device ID
        event: 1002             # Button 2 press (brightness up)
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room_lights
      data:
        brightness_step: 51     # Step up ~20% of 255
  condition:
    - condition: numeric_state
      entity_id: light.dining_room_lights
      attribute: brightness
      below: 250
```

### Key Changes:

1. **Trigger platform:** `device` (Hue) → `event` (Z2M)
2. **Event type:** `hue` → `zigbee2mqtt_event`
3. **Event ID:** `subtype: 2` → `event: 1002` (varies by device)
4. **Action:** `device` domain → `service` call
5. **Entity:** `entity_id` instead of `device_id`

---

## Finding Your Z2M Dimmer Device ID

After pairing dimmer to Z2M:

1. **In Home Assistant:**
   ```
   Settings > Devices & Entities > search "dimmer"
   ```

2. **Find the event entities:**
   - `event.hue_dimmer_switch_*_button_1`
   - `event.hue_dimmer_switch_*_button_2`
   - etc.

3. **Click on event entity > Details**
   - Shows the device_id needed for automation

4. **Test the event:**
   - Press dimmer button
   - Check HA event history
   - Shows the exact event code fired

---

## Creating Test Automations

### Simple Test Automation (Before migrating production)

Create a test automation first:

```yaml
automation.test_dimmer_brightness:
  alias: Test Dimmer Button 2
  trigger:
    - platform: event
      event_type: zigbee2mqtt_event
      event_data:
        device_id: <NEW_DIMMER_ID>
        event: 1002
  action:
    - service: light.turn_on
      target:
        entity_id: light.dining_room_lights
      data:
        brightness_step: 51
  mode: single
```

**Test steps:**
1. Create automation
2. Press dimmer button 2
3. Check if lights brighten
4. Check HA logs for any errors
5. If works: migrate production automations
6. If fails: check event codes, adjust, retry

---

## Brightness Step Values

For `brightness_step` in Z2M light service:

```
Brightness range: 0-255

Suggested step sizes:
- brightness_step: 25  = ~10% increase
- brightness_step: 51  = ~20% increase  
- brightness_step: 76  = ~30% increase
- brightness_step: 102 = ~40% increase
- brightness_step: 127 = ~50% increase
```

The original Hue dimmer increases brightness by ~20%, so use `51`.

---

## If Dimmer Is NOT Z2M Compatible

**Signs that it's not supported:**
- Z2M device database says "Not supported"
- Pairing fails after multiple attempts
- Device joins but no button events appear

**Alternatives:**

1. **Use different trigger:**
   ```yaml
   # Instead of button press, use time-based
   trigger:
     - platform: time
       at: "20:00:00"  # Brighten at 8pm
   ```

2. **Use automation triggers:**
   ```yaml
   trigger:
     - platform: state
       entity_id: input_number.dining_brightness
       to: "75"  # When slider set to 75
   ```

3. **Use voice commands:**
   - "Brighten dining lights"
   - Handled by Assist/voice automation

4. **Replace physical dimmer:**
   - Use Z2M-compatible button (e.g., IKEA Shortcut Button)
   - Cost: ~$10-15
   - Models to search: "Zigbee button", "Z2M compatible dimmer"

---

## Helpful Z2M Documentation Links

- Z2M Device Database: https://www.zigbee2mqtt.io/devices/
- Z2M Supported Devices: https://www.zigbee2mqtt.io/supported-devices/
- HA Z2M Integration Docs: https://www.home-assistant.io/integrations/mqtt/

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Dimmer won't pair to Z2M | Reset dimmer longer (hold 5-10 sec), check permit_join enabled, move closer to Z2M coordinator |
| Button events not firing | Check Z2M logs, verify device joined successfully, check event codes in HA event history |
| Brightness not changing | Check light entity name, verify brightness_step value, check light supports brightness |
| Condition not working | Verify entity_id exists, check brightness attribute format, test condition independently |
| Automation doesn't trigger | Enable automation, check trigger event codes, enable debug logging |

---

**Ready to migrate?** Proceed with Phase 2 of the migration plan after checking compatibility.
