# Phase 1: Quick-Start Checklist

**Start Date:** 2026-05-02  
**Objective:** Prepare for dining room light pairing to Z2M

---

## What to Do RIGHT NOW

### 1. Locate Hardware ⏱️ 5 min

- [ ] **Find the 4 dining room downlights**
  - Where: Dining room ceiling
  - What: Hue ambiance downlights (round fixtures)
  - Verify: Can you turn them on from the dining room wall switch?

- [ ] **Find Hue dimmer switch 1**
  - Where: Check walls near dining room, living room, or kitchen
  - What: White/black wall-mounted dimmer with 4 buttons
  - Verify: Does it control any lights?

### 2. Check Z2M System ⏱️ 5 min

In Home Assistant:
1. Go to **Settings > Devices & Services**
2. Find **Zigbee2MQTT**
3. Click on it
4. Check status: **Status should be "Loaded"** ✅

If not loaded, Z2M needs to be restarted before proceeding.

### 3. Find Dimmer Model ⏱️ 5 min

In Home Assistant:
1. Go to **Settings > Devices & Services > Hue**
2. Look for device: **"Hue dimmer switch 1"**
3. Click on it
4. Note down:
   - [ ] Model name (e.g., RWL021, LCL001, etc.)
   - [ ] IEEE address (starts with 0x...)
   - [ ] Serial number if visible

### 4. Check Dimmer Compatibility ⏱️ 5 min

Go to: https://www.zigbee2mqtt.io/devices/

Search for your dimmer model:
- [ ] Does it say "Supported: ✅ YES"?
- [ ] Note the Z2M version requirement
- [ ] Screenshot or note the button event codes

**If NOT supported:**
⚠️ Stop here and decide:
- Option B1: Disable brightness automations (simpler)
- Option B2: Find compatible Z2M dimmer replacement
- Contact me before proceeding to Phase 2

**If supported:**
✅ Continue to Phase 2!

### 5. Verify Automations Still Work ⏱️ 5 min

In Home Assistant:
1. Go to **Automations**
2. Find:
   - [ ] automation.brighten_dining_room_lights — should exist
   - [ ] automation.decrease_dining_room_lights — should exist
3. Check they're both **Enabled** (blue toggle)

---

## Summary

**Time to complete:** ~20 minutes  
**Next step:** If all checks pass → Begin Phase 2 (Pairing lights to Z2M)

---

## STOP if Any of These Are True

❌ **STOP:** Z2M is not loaded/responsive
- Solution: Restart Z2M bridge in HA

❌ **STOP:** Dimmer is NOT Z2M compatible
- Solution: Choose Option B (disable automations) or find alternative

❌ **STOP:** You can't find the 4 dining lights
- Solution: Check if lights still exist, ask Teddie where they are

❌ **STOP:** Automations don't exist or are disabled
- Solution: They should be auto-created, ask me for help

---

## When Complete

✅ All items checked  
✅ Dimmer is compatible  
✅ Z2M is responsive  
✅ Automations exist  

**Then:** Let me know you've completed Phase 1 and you're ready for Phase 2

**Phase 2 will:**
1. Enable Z2M pairing mode
2. Pair 4 dining lights to Z2M
3. Pair dimmer to Z2M
4. Create Z2M group
5. Test all connections

---

**Need help?** Check:
- `dining_room_z2m_migration_plan.md` — Full detailed plan
- `z2m_dimmer_event_reference.md` — Technical details on dimmer events
