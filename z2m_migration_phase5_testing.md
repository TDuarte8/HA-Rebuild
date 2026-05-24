# Phase 5: Comprehensive Testing

**Status:** Ready to execute (waiting for Phase 2-4 completion)  
**Date Prepared:** 2026-05-02  
**Duration:** ~30-45 minutes

---

## Test Overview

This phase verifies that:
1. Individual Z2M lights work correctly
2. Z2M group controls all lights together
3. Brightness automations trigger from dimmer button presses
4. Voice commands still work
5. Main bedroom lights still function (verify no Hue dependency)

---

## Pre-Test Checklist

Before starting tests:
- [ ] All 3-4 dining lights paired to Z2M
- [ ] Dining room group created: `light.dining_room_lights`
- [ ] Hue dimmer paired to Z2M
- [ ] Both automations updated to Z2M format
- [ ] Automations enabled: `automation.brighten_dining_room_lights`, `automation.decrease_dining_room_lights`

---

## Test Section 1: Individual Light Control

### Test 1A: Turn On Individual Light

**Test:** light.0x00178801027c1190

1. In HA UI: Go to Entities > Light
2. Find `light.0x00178801027c1190`
3. Click the light card to turn on
4. Verify in UI: Light shows "on"
5. Check HA logs (Settings > System > Logs) for errors

**Expected Result:** ✅ Light turns on with no errors  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 1B: Set Brightness (Individual)

**Test:** light.0x00178801027c1190 to 50% brightness

1. Click light to open controls
2. Set brightness slider to 50%
3. Verify light dims to 50%
4. Check brightness attribute = 127 (50% of 255)

**Expected Result:** ✅ Light dims to 50% with correct attribute  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 1C: Set Color Temperature (Individual)

**Test:** light.0x00178801027c1190 to 3000K (warm white)

1. Click light to open controls
2. Set color temperature to 3000K
3. Verify light shifts to warm white
4. Check color_temp attribute = ~333 mired (3000K)

**Expected Result:** ✅ Light shifts to warm white  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 1D: Turn Off Individual Light

**Test:** light.0x00178801027c1190

1. In HA UI, turn off the light
2. Verify light turns off
3. Check state = "off" in HA

**Expected Result:** ✅ Light turns off  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 1E: Repeat for All Lights (1A-1D)

- [ ] Test 1A-1D for `light.0x00178801027c0fe4`
- [ ] Test 1A-1D for `light.0x00178801027c11c4`
- [ ] Test 1A-1D for 4th light (if paired)

---

## Test Section 2: Group Control

### Test 2A: Turn On Group

**Test:** light.dining_room_lights (all 3-4 lights)

1. Find `light.dining_room_lights` in HA
2. Turn on the group
3. Verify all member lights turn on
4. Check each light entity shows "on"

**Expected Result:** ✅ All lights on  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 2B: Set Group Brightness

**Test:** light.dining_room_lights to 75%

1. Set group brightness to 75%
2. Verify all lights dim to 75%
3. Check each member light's brightness = 191 (75% of 255)

**Expected Result:** ✅ All lights at 75% brightness  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 2C: Set Group Color Temperature

**Test:** light.dining_room_lights to 2700K (warm)

1. Set group color temperature to 2700K
2. Verify all lights shift to warm white
3. Check each light's color_temp attribute

**Expected Result:** ✅ All lights warm white  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 2D: Turn Off Group

**Test:** light.dining_room_lights (all off)

1. Turn off the group
2. Verify all member lights turn off
3. Check each light state = "off"

**Expected Result:** ✅ All lights off  
**Actual Result:** ⏳ (To be filled during testing)

---

## Test Section 3: Voice Commands

### Test 3A: Turn On with Voice

**Command:** "Turn on dining room lights"

1. Say the command to Assist/voice assistant
2. Verify all dining lights turn on
3. Check no errors in HA logs

**Expected Result:** ✅ Lights turn on via voice  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 3B: Set Brightness with Voice

**Command:** "Set dining room lights to 50 percent"

1. Say the command
2. Verify lights dim to 50%
3. Check brightness attribute = 127

**Expected Result:** ✅ Lights dim to 50%  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 3C: Turn Off with Voice

**Command:** "Turn off dining room lights"

1. Say the command
2. Verify all lights turn off

**Expected Result:** ✅ Lights off via voice  
**Actual Result:** ⏳ (To be filled during testing)

---

## Test Section 4: Automation Testing (Dimmer)

### Test 4A: Brighten Automation

**Trigger:** Press dimmer button 2 (Brighten)

1. Press physical dimmer button 2
2. Verify lights brighten by ~20%
3. Check HA automation trace for successful trigger
4. Check no automation errors in logs

**Expected Result:** ✅ Lights brighten on button press  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 4B: Brighten Automation Condition

**Trigger:** Press dimmer button 2 when brightness = 100%

1. Set lights to 100% brightness
2. Press dimmer button 2
3. Verify lights do NOT brighten (condition prevents it)
4. Check automation trace shows condition: failed

**Expected Result:** ✅ Condition blocks brighten when at max  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 4C: Decrease Automation

**Trigger:** Press dimmer button 3 (Dimmer)

1. Set lights to 75% brightness
2. Press dimmer button 3
3. Verify lights dim by ~20% (to ~55%)
4. Check automation trace for successful trigger

**Expected Result:** ✅ Lights dim on button press  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 4D: Decrease Automation Condition

**Trigger:** Press dimmer button 3 when brightness = 0%

1. Set lights to 0% (off)
2. Press dimmer button 3
3. Verify lights do NOT dim further (already at min)
4. Check automation trace shows condition: failed

**Expected Result:** ✅ Condition blocks dim when at minimum  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 4E: Multiple Presses

**Trigger:** Press button 2 multiple times

1. Press button 2 (brighten) 4 times rapidly
2. Verify lights brighten in steps (not all at once)
3. Check no missed button presses in trace

**Expected Result:** ✅ Each button press increases brightness  
**Actual Result:** ⏳ (To be filled during testing)

---

## Test Section 5: Verify Main Bedroom (No Regressions)

**Critical:** Ensure main bedroom lights still work after removing Hue dependency

### Test 5A: Main Bedroom Light Control

1. In HA, turn on a main bedroom light (e.g., `light.back_left_light`)
2. Verify it turns on
3. Set brightness to 75%
4. Verify it dims
5. Turn off

**Expected Result:** ✅ Main bedroom lights work  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 5B: Main Bedroom Scene

1. In HA Automations & Scenes, find a main bedroom scene
2. Activate the scene
3. Verify scene settings apply (lights on/brightness/color)
4. Check no errors in logs

**Expected Result:** ✅ Main bedroom scene works  
**Actual Result:** ⏳ (To be filled during testing)

---

### Test 5C: Main Bedroom Voice Command

**Command:** "Turn on bed left light" (or similar)

1. Say command
2. Verify light responds
3. Check no errors in logs

**Expected Result:** ✅ Main bedroom voice commands work  
**Actual Result:** ⏳ (To be filled during testing)

---

## Test Summary Checklist

- [ ] Section 1: All individual lights control correctly (3-4 lights)
- [ ] Section 2: Group controls all lights together
- [ ] Section 3: Voice commands work
- [ ] Section 4: Dimmer brightness automations trigger and conditions work
- [ ] Section 5: Main bedroom lights unaffected by migration

---

## Troubleshooting During Testing

| Issue | Immediate Action |
|-------|------------------|
| Light doesn't turn on | Check Z2M device online, check HA logs for timeout, verify linkquality > 50 |
| Group doesn't respond | Verify all members added to group in Z2M admin, reload MQTT integration |
| Voice command fails | Check entity name matches voice assistant config, test HA UI control first |
| Dimmer button doesn't trigger | Check automation trigger event_data matches actual Z2M button event, check automation trace |
| Condition blocks all button presses | Check condition logic (below vs above values), manually test condition in automation editor |
| Main bedroom broken | Check if deletion of Hue integration happened prematurely, restore backup if necessary |

---

## If All Tests Pass

- [ ] Proceed to Phase 6: Delete Hue Integration
- [ ] Create final backup before deletion
- [ ] Document completion status

---

**Next Phase:** Phase 6 — Safe Hue Integration Deletion (see full migration plan)
