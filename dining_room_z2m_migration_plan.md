# Dining Room & Dimmer Z2M Migration Plan

**Date Started:** 2026-05-02  
**Objective:** Migrate dining room lights + Hue dimmer to Z2M, then delete Hue integration safely

---

## Phase 1: Pre-Migration Checklist

### Verify Hardware
- [ ] Locate 4 dining room downlights (Hue ambiance BR30 or similar)
- [ ] Locate Hue dimmer switch 1 (physical wall switch or remote)
- [ ] Verify all devices power on and respond to Hue bridge

### Verify Z2M Status
- [ ] Z2M coordinator has capacity (currently 25 devices + bridge, max typically 128+)
- [ ] Z2M is responsive and healthy
- [ ] Backup current Z2M configuration

### Verify Automations
- [ ] Confirm automation.brighten_dining_room_lights exists
- [ ] Confirm automation.decrease_dining_room_lights exists
- [ ] Document current trigger/action details (already captured)

---

## Phase 2: Device Pairing to Z2M

### Step 2A: Prepare Z2M for Pairing

1. **Enable permit_join in Z2M:**
   - In Home Assistant, find `switch.zigbee2mqtt_bridge_permit_join`
   - Turn ON this switch
   - This allows Z2M to accept new device pairings for 254 seconds

2. **Monitor logs:**
   - Optional: Keep Z2M logs open to watch pairing progress
   - Look for messages like "Device joined" or "New device"

### Step 2B: Pair Dining Room Downlights (4 total)

**For each downlight:**

1. **Reset the bulb:**
   - Turn off power for 5 seconds
   - Turn on power
   - Wait for bulb to stabilize (blink pattern indicates ready)
   - Bulb should be blinking/ready to pair

2. **Pair to Z2M:**
   - Z2M should detect the bulb within 30 seconds
   - Bulb will join as a new device
   - Name it appropriately in Z2M admin panel

3. **Verify in Home Assistant:**
   - New `light.dining_*` entity should appear
   - Check connectivity (sensor shows link quality)

**Repeat for all 4 lights:**
- Dining Room Front Left
- Dining Room Back Left
- Dining Room Front Right
- Dining Room Back Right

**Expected result:** 4 new light entities in Z2M, removed from Hue

### Step 2C: Pair Hue Dimmer Switch 1

⚠️ **COMPATIBILITY ISSUE:** Hue dimmer switches have mixed Z2M support
- Some models work with Z2M
- Some models are Hue-specific and won't pair to Z2M

**Check compatibility first:**
1. Note the model/serial number on the dimmer
2. Search Z2M device database: https://www.zigbee2mqtt.io/devices/
3. Search for "Hue dimmer" to see if your model is supported

**If supported:**
1. Reset dimmer according to Hue specifications (usually hold power 3-5 seconds)
2. Enable permit_join on Z2M
3. Put dimmer in pairing mode
4. Z2M should detect it
5. Verify in Home Assistant as `event.hue_dimmer_switch_*`

**If NOT supported:**
- ⚠️ Dimmer cannot be migrated
- Alternative: Use a compatible Z2M-native dimmer (IKEA Shortcut Button, etc.)
- OR: Disable dimmer automations and use other triggers

---

## Phase 3: Create Z2M Groups

### Step 3A: Create Dining Room Lights Group

1. **In Z2M admin panel:**
   - Create new group: "Dining Room Lights"
   - Add all 4 newly paired downlights to the group
   - Assign group ID (typically auto-assigned)

2. **In Home Assistant:**
   - New group entity should appear: `light.dining_room_lights`
   - Test that controlling group affects all 4 lights

### Step 3B: Update Entity Names

Rename to match old Hue entity naming for consistency:
- `light.dining_front_left` (should match original name)
- `light.dining_back_left`
- `light.dining_front_right`
- `light.dining_back_right`
- `light.dining_room_lights` (Z2M group, replaces old `light.dining_room` Hue group)

---

## Phase 4: Update Automations

### Current Automations to Modify

**automation.brighten_dining_room_lights**
- Current trigger: Hue dimmer switch button 2
- Current action: `light.dining_room` brightness increase
- Z2M dimmer support: TBD (see Phase 2C)

**automation.decrease_dining_room_lights**
- Current trigger: Hue dimmer switch button 3
- Current action: `light.dining_room` brightness decrease
- Z2M dimmer support: TBD (see Phase 2C)

### Update Path A: If Dimmer Migrated to Z2M

**Step 4A1: Get Z2M Dimmer Event Details**
```
Find these in Home Assistant:
- event.hue_dimmer_switch_*_button_1
- event.hue_dimmer_switch_*_button_2
- event.hue_dimmer_switch_*_button_3
- event.hue_dimmer_switch_*_button_4
```

**Step 4A2: Update Trigger in Automations**

Old trigger (Hue):
```yaml
trigger:
  - device_id: be9f7acf4e8cab1d5de407601ce69dab  # Hue dimmer
    domain: hue
    type: initial_press
    subtype: 2
    platform: device
```

New trigger (Z2M):
```yaml
trigger:
  - platform: event
    event_type: zha_event  # Or zigbee2mqtt_event depending on integration
    event_data:
      device_id: <NEW_Z2M_DIMMER_ID>
      event: 1002  # Button 2 press (Z2M event code)
```

**Step 4A3: Update Action Target**

Old action:
```yaml
action:
  - device_id: 04bcba7a94b0a741d75705f9eacfcd65  # Hue dining room
    domain: light
    type: brightness_increase
```

New action:
```yaml
action:
  - service: light.turn_on
    target:
      entity_id: light.dining_room_lights  # Z2M group
    data:
      brightness_step: 51  # Increase by 20%
```

**Step 4A4: Update Conditions**

Change from:
```yaml
condition:
  - condition: numeric_state
    entity_id: light.dining_room
    attribute: brightness
    below: 100
```

To:
```yaml
condition:
  - condition: numeric_state
    entity_id: light.dining_room_lights
    attribute: brightness
    below: 100
```

### Update Path B: If Dimmer NOT Supported by Z2M

**Alternative triggers:**
- Use Home Assistant voice commands
- Use Zigbee button press if you have a Z2M-compatible button
- Use automation triggers (time-based, state-based, etc.)

**Option B1: Disable Automations**
- Accept that dimmer won't work
- Control dining lights via voice/UI/scenes
- Delete the 2 automations

**Option B2: Migrate to Different Trigger**
- Example: Use voice command "increase dining brightness"
- Example: Create script called by physical button (Z2M compatible)
- Example: Use input_number slider automation

---

## Phase 5: Testing

### Test 5A: Individual Light Control

For each dining light:
- [ ] Turn on from HA UI → light responds
- [ ] Set brightness 50% → light dims correctly
- [ ] Set color temperature 2000K → light responds
- [ ] Turn off from HA UI → light off

### Test 5B: Group Control

- [ ] Turn on `light.dining_room_lights` → all 4 lights on
- [ ] Set brightness 75% → all 4 lights respond
- [ ] Turn off → all 4 lights off

### Test 5C: Voice Control

- [ ] "Turn on dining room lights" → works
- [ ] "Set dining lights to 50 percent" → works
- [ ] "Turn off dining lights" → works

### Test 5D: Automation Testing (if dimmer migrated)

**Test brighten automation:**
- [ ] Press dimmer button 2 → lights brighten
- [ ] Check condition works (doesn't brighten if already at 100%)
- [ ] Verify no errors in automation log

**Test decrease automation:**
- [ ] Press dimmer button 3 → lights dim
- [ ] Check condition works (doesn't dim if already at 0%)
- [ ] Verify no automation errors

### Test 5E: Hue Integration Still Works

Before deletion, verify:
- [ ] Main bedroom scenes still work
- [ ] Kitchen lights still work
- [ ] Living room couch lamps still work
- [ ] All voice commands still function

---

## Phase 6: Safe Hue Integration Deletion

**After all testing passes:**

1. **Final Backup:**
   - Create HA backup before deletion
   - Snapshot any automation configs

2. **Delete Hue Integration:**
   - Settings > Devices & Services > Hue > ⋯ > Delete
   - Confirm deletion

3. **Verify Removal:**
   - [ ] No `light.dining_front_*`, etc. from Hue
   - [ ] No `scene.dining_room_*` from Hue
   - [ ] No `light.living_room_orphan`
   - [ ] 34 orphaned scenes removed
   - [ ] Hue automations listed in system still reference valid entities

4. **Final Test:**
   - Test main bedroom scenes
   - Test kitchen lights
   - Test dining room lights
   - Test dimmer (if migrated)
   - Verify no broken automations in logs

---

## Rollback Plan (If Something Breaks)

If issues occur:

1. **Restore HA backup** from before Hue deletion
2. **Keep Z2M devices** (don't reset them)
3. **Identify what went wrong:**
   - Check HA logs for error messages
   - Verify Z2M device connectivity
   - Check automation syntax

4. **Fix and retry:**
   - Fix issues in automations
   - Re-test
   - Attempt deletion again

---

## Timeline Estimate

| Phase | Duration | Notes |
|-------|----------|-------|
| Phase 1: Checklist | 15 min | Gather info, verify hardware |
| Phase 2A: Z2M Prep | 5 min | Enable permit_join |
| Phase 2B: Pair 4 lights | 20 min | ~5 min per light |
| Phase 2C: Pair dimmer | 10 min | Depends on compatibility |
| Phase 3: Create groups | 10 min | In Z2M + HA |
| Phase 4: Update automations | 20 min | Depends on dimmer support |
| Phase 5: Testing | 30 min | Thorough testing |
| Phase 6: Deletion | 5 min | Delete Hue integration |
| **TOTAL** | **~2 hours** | Can be done in one session |

---

## Success Criteria

✅ **Migration is successful when:**
- [ ] All 4 dining lights respond in Z2M
- [ ] Dining room group controls all 4 lights
- [ ] Dimmer migrated OR automations disabled/updated
- [ ] Brightness automations work (if dimmer migrated)
- [ ] All tests in Phase 5 pass
- [ ] Hue integration deleted
- [ ] No errors in HA logs
- [ ] Main bedroom still works perfectly

---

## Known Issues & Workarounds

| Issue | Likelihood | Workaround |
|-------|------------|-----------|
| Dimmer not Z2M compatible | Medium | Use alternative trigger or disable automations |
| Bulb won't pair to Z2M | Low | Reset bulb, try again, check for Zigbee interference |
| Z2M coordinator full | Very Low | Unlikely, but would need to remove old Hue devices first |
| Automation syntax error | Low | Use HA automation editor for validation |
| Bulb brightness doesn't sync | Very Low | Restart Z2M bridge or HA |

---

**Migration started:** 2026-05-02  
**Status:** Ready to begin Phase 1

Next: Run Phase 1 checklist, then proceed to Phase 2.
