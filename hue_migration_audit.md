# Hue Integration Audit — Unmigrated Devices

**Date:** 2026-04-30  
**Scope:** Full audit of Hue integration vs Z2M equivalents  
**Status:** 🔴 **Multiple devices NOT yet migrated to Z2M**

---

## Summary

**Hue Integration Status:**
- Total Hue devices: **18**
- Hue devices with control capability: **13**
- Hue devices UNMIGRATED (not on Z2M): **9**
- Hue devices MIGRATED to Z2M: **4** (Main Bedroom complete)

**Z2M Integration Status:**
- Total Z2M devices: **25**
- Z2M light devices: **24**
- Z2M bridge: **1**

---

## ✅ COMPLETED MIGRATIONS

### Main Bedroom (FULLY MIGRATED)

| Device | Entity | Status | Z2M Equivalent |
|--------|--------|--------|---|
| Right Lamp | light.right_lamp | ✅ Z2M | Yes |
| Dresser Lamp | light.dresser_lamp | ✅ Z2M | Yes |
| Left Lamp | light.left_lamp | ✅ Z2M | Yes |
| Back Left Light | light.back_left_light | ✅ Z2M | Yes |
| Bed Left Light | light.bed_left_light | ✅ Z2M | Yes |
| Back Right Light | light.back_right_light | ✅ Z2M | Yes |
| Bed Right Light | light.bed_right_light | ✅ Z2M | Yes |
| Main Bedroom Lamps (group) | light.main_bedroom_lamps | ✅ Z2M (Group) | Yes |
| Main Bedroom Overhead Lights (group) | light.main_bedroom_overhead_lights | ✅ Z2M (Group) | Yes |

**Note:** Main Bedroom also has 3 Z2M groups created:
- light.main_bedroom_lamps (Z2M Group - right lamp, dresser lamp, left lamp)
- light.main_bedroom_overhead_lights (Z2M Group - 4 BR30 flood lights)
- light.main_bedroom_lights (Z2M Group - combines lamps + overhead)

---

## 🔴 UNMIGRATED DEVICES (Must migrate before Hue deletion)

### 1. Dining Room Lights (4 downlights)

| Device | Entity | Model | Status | Z2M Equivalent |
|--------|--------|-------|--------|---|
| Dining Front Left | light.dining_front_left | Hue ambiance downlight | ❌ Hue only | NOT FOUND |
| Dining Back Left | light.dining_back_left | Hue ambiance downlight | ❌ Hue only | NOT FOUND |
| Dining Front Right | light.dining_front_right | Hue ambiance downlight | ❌ Hue only | NOT FOUND |
| Dining Back Right | light.dining_back_right | Hue ambiance downlight | ❌ Hue only | NOT FOUND |

**Action Required:** These 4 lights need to be either:
- Paired to Z2M2MQTT and removed from Hue, OR
- Checked if they're already paired to Z2M but not paired to Hue bridge

**Verify by:** Checking the bulbs physically - if they're still connected to Hue hub, they need Z2M pairing

---

### 2. Dining Room Room/Group

| Device | Entity | Type | Status |
|--------|--------|------|--------|
| Dining Room | light.dining_room | Hue Room (group) | ❌ Hue only |

**Contains:** Scenes only (no individual light control) + the 4 downlights above

**Action:** Will be removed with Hue integration deletion (scene container only)

---

### 3. Living Room Downlight (Orphan)

| Device | Entity | Model | Status | Notes |
|--------|--------|-------|--------|-------|
| Living Room Orphan | light.living_room_orphan | Hue ambiance downlight | ❌ UNAVAILABLE | Cannot control, marked unavailable |

**Area:** Living Room  
**Issue:** The light is listed as "unavailable" - likely unpaired or offline on Hue bridge

**Action Required:**
- If the bulb is dead/broken: just delete when removing Hue integration
- If the bulb exists and should work: may need to re-pair to Hue or migrate to Z2M

---

### 4. Living Room Room/Group (Light)

| Device | Entity | Type | Status |
|--------|--------|------|--------|
| Living Room | light.living_room | Hue Room (group) | ❌ Hue only |

**Contains:** 
- Scenes (scene.living_room_*)
- One light entity: light.living_room

**Note:** The 3 couch lamps (Top, Middle, Bottom) ARE on Z2M, but the Room/group light entity is still on Hue

**Action:** Could remain on Hue as a group, or delete with Hue integration

---

### 5. Downstairs Zone/Group (Light)

| Device | Entity | Type | Status |
|--------|--------|------|--------|
| Downstairs | light.downstairs | Hue Zone (group) | ❌ Hue only |

**Contains:**
- Scenes (scene.downstairs_*)
- One light entity: light.downstairs

**Action:** Will be removed with Hue integration deletion (scene container only)

---

### 6. Hue Dimmer Switch 1 (Physical Device) 🔴 **IMPORTANT**

| Device | Entity | Model | Status | Purpose |
|--------|--------|-------|--------|---------|
| Hue dimmer switch 1 | Multiple event/sensor entities | Hue dimmer switch | ❌ **NOT MIGRATED** | Physical wall switch for Hue automations |

**Entities Associated:**
- event.hue_dimmer_switch_1_button_1 (Button 1)
- event.hue_dimmer_switch_1_button_2 (Button 2)
- event.hue_dimmer_switch_1_button_3 (Button 3)
- event.hue_dimmer_switch_1_button_4 (Button 4)
- sensor.hue_dimmer_switch_1_battery (Battery status)
- sensor.hue_dimmer_switch_1_zigbee_connectivity

**Current Use:** Any automations using this dimmer switch will break after Hue deletion

**Action Required:**
1. Check if any automations reference this dimmer switch
2. If yes: either pair to Z2MQTT or remove the automations
3. If no: can be deleted with Hue integration

---

## 🟡 KITCHEN LIGHTS (MIGRATED)

Kitchen lights are **already migrated to Z2M**:

| Device | Entity | Status |
|--------|--------|--------|
| Corner Light | light.corner_light | ✅ Z2M |
| Fridge Light | light.fridge_light | ✅ Z2M |
| Sink Light | light.sink_light | ✅ Z2M |
| Oven Light | light.oven_light | ✅ Z2M |
| Coffee Light | light.coffee_light | ✅ Z2M |
| Pantry Light | light.pantry_light | ✅ Z2M |
| Kitchen Lights (group) | light.kitchen_lights | ✅ Z2M (Group) |

**Status:** These are ready. Hue integration still has the Kitchen, Back Kitchen, and Front Kitchen Room/Zone containers for scenes, but all controllable lights are on Z2M.

---

## 🟡 LIVING ROOM COUCH LAMPS (MIGRATED)

Couch lamps are **already migrated to Z2M**:

| Device | Entity | Status |
|--------|--------|--------|
| Top Couch Lamp | light.top_couch_lamp | ✅ Z2M |
| Middle Couch Lamp | light.middle_couch_lamp | ✅ Z2M |
| Bottom Couch Lamp | light.bottom_couch_lamp | ✅ Z2M |
| Couch Lamps (group) | light.couch_lamps | ✅ Z2M (Group) |

**Status:** These are ready. But `light.living_room` (Hue Room) and `light.living_room_orphan` are still on Hue.

---

## ⚠️ HUES SCENE CONTAINERS (Hue-only, no migration needed)

These are scene containers only (not controllable lights), but they will be removed when Hue integration is deleted:

| Device | Type | Area |
|--------|------|------|
| Kitchen | Room | kitchen |
| Back Kitchen | Zone | kitchen |
| Front Kitchen | Zone | kitchen |
| Office | Room | tiago_office |
| Upstairs | Room | (unassigned, empty) |

---

## Migration Blockers

### ⚠️ CRITICAL: Dining Room Lights (4) — Unknown Z2M Status

**Problem:** The 4 dining room downlights are only on Hue, no Z2M equivalents found

**Possible explanations:**
1. The bulbs are physically paired to the Hue hub and need to be reset and paired to Z2M
2. The bulbs are not present in the home anymore
3. The bulbs are on Z2M but under different names

**How to resolve:**
1. Check Z2M device list for any "Dining" lights (might be under different names)
2. Physically locate the 4 downlights in dining room
3. Reset each bulb and re-pair to Z2MQTT (if they exist)
4. Create Z2M group once all are paired

---

### 🔴 CRITICAL: Hue Dimmer Switch — AUTOMATION DEPENDENCY FOUND

**Problem:** Physical Hue dimmer switch is not on Z2MQTT, and **2 automations depend on it**

**Automations Using This Switch:**

1. **automation.brighten_dining_room_lights**
   - Trigger: Hue dimmer switch 1, button 2 (initial press)
   - Action: Increase brightness of `light.dining_room`
   - Status: ❌ WILL BREAK on Hue deletion

2. **automation.decrease_dining_room_lights**
   - Trigger: Hue dimmer switch 1, button 3 (initial press)
   - Action: Decrease brightness of `light.dining_room`
   - Status: ❌ WILL BREAK on Hue deletion

**Combined Blocker:** Both the trigger (dimmer) and target (dining_room light) are on Hue only

**Action required (choose one):**
1. **Option A (Recommended):** Migrate Hue dimmer to Z2M
   - Pair dimmer to Z2MQTT
   - Update automations to use Z2M dimmer events
   - Ensure dining room lights are also on Z2M
   
2. **Option B:** Disable/remove these automations
   - Automations will no longer control dining room brightness via dimmer
   - Dining lights can still be controlled via voice/UI
   
3. **Option C:** Keep Hue integration indefinitely
   - Don't delete Hue integration
   - Keep these automations and devices on Hue

---

## Pre-Deletion Checklist

### 🔴 CRITICAL BLOCKERS (MUST RESOLVE)

- [ ] **Hue Dimmer Switch:** Used by 2 automations for dining room brightness
  - Automations: `brighten_dining_room_lights`, `decrease_dining_room_lights`
  - Decision: Migrate to Z2M, disable automations, or keep Hue
  
- [ ] **Dining Room Lights (4):** Target of the dimmer automations
  - Lights: light.dining_front_left, light.dining_back_left, light.dining_front_right, light.dining_back_right, light.dining_room (group)
  - Decision: Migrate to Z2M or accept loss of dimmer brightness control

### 🟡 IMPORTANT (Recommended)

- [ ] **Living room orphan:** Confirm if bulb exists/works or if it should be deleted
- [ ] **Final scan:** Verify no other Hue entity references remain in automations

---

## Recommended Approach

**Phase 1 (this week):**
1. Physically inspect the 4 dining room downlights
2. Verify Hue dimmer switch location and if it's still in use
3. Check automations for dimmer switch references

**Phase 2 (decision point):**
- If dining lights needed → pair to Z2M, create groups, test scenes
- If dimmer needed → pair to Z2M or disable related automations
- If either not needed → proceed to deletion

**Phase 3 (safe deletion):**
- Once dining/dimmer handled, delete Hue integration
- All orphaned scenes auto-removed
- All Hue Room/Zone containers removed

---

## Current State Summary

| Category | Count | Status |
|----------|-------|--------|
| **Hue devices total** | 18 | Requires action |
| **Unmigrated lights** | 5 | Dining (4) + Orphan (1) |
| **Unmigrated control** | 1 | Dimmer switch |
| **Ready for deletion** | 8 | Rooms/Zones/scenes |
| **Z2M migrated** | 19+ | Ready |

---

**Generated by:** Automated audit  
**Generated:** 2026-04-30
