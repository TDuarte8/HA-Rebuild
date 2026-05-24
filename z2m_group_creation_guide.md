# Phase 3: Z2M Group Creation Guide

**Status:** Ready to execute (waiting for Phase 2C/Phase 2B-4 to complete)  
**Date Prepared:** 2026-05-02

---

## Current Pairing Status (As of 2026-05-02)

### Devices Ready for Group:
✅ `light.0x00178801027c1190` — Hue white ambiance BR30 flood light  
✅ `light.0x00178801027c0fe4` — Hue white ambiance BR30 flood light  
✅ `light.0x00178801027c11c4` — Hue white ambiance BR30 flood light  

### Devices Still Pending:
⏳ 4th Hue white ambiance BR30 flood light — Will add to group once paired

---

## Step 1: Create Group in Z2M Admin Panel

1. Open Home Assistant > Settings > Devices & Services
2. Find **Zigbee2MQTT** > Device info or Admin Panel link
3. In Z2M admin panel, find **Groups** section
4. Click **Create new group**
5. Enter group name: `Dining Room Lights`
6. Group ID: Leave as auto-assigned (typically next sequential number)
7. Leave advanced settings at defaults
8. Click **Create**

---

## Step 2: Add Devices to Group

1. In Z2M admin panel, navigate to the newly created group
2. Click **Add device(s) to group**
3. Select all 3 paired dining room lights:
   - ✅ 0x00178801027c1190
   - ✅ 0x00178801027c0fe4
   - ✅ 0x00178801027c11c4
4. Confirm group membership
5. **Note:** Do NOT add the Hue dimmer to this group — it's a control device, not a light

---

## Step 3: Verify in Home Assistant

1. Go to Home Assistant > Settings > Devices & Services > Devices
2. Search for `dining_room` or look for new light entity
3. Verify new entity appears: **`light.dining_room_lights`**
4. Check that controlling the group affects all 3 members:
   - Turn on: All 3 lights turn on
   - Set brightness: All 3 respond
   - Set color temp: All 3 respond (if supported)

---

## Step 4: Rename Individual Lights (Optional but Recommended)

For easier reference and to match original Hue naming, rename the 3 lights:

**In Home Assistant:**

1. Go to Settings > Devices & Services > Devices
2. Find each IEEE-addressed light device
3. Click the device name to edit
4. Rename using pattern: `dining_room_{position}_{side}`

**Suggested Names:**
- `0x00178801027c1190` → `dining_room_front_left`
- `0x00178801027c0fe4` → `dining_room_front_right`
- `0x00178801027c11c4` → `dining_room_back_left`
- (4th light when paired) → `dining_room_back_right`

**In Z2M Admin Panel** (optional):
1. Z2M admin panel > Devices
2. Click each IEEE device
3. Edit friendly name to match above pattern
4. HA will auto-update entity names within ~30 seconds

---

## Step 5: Verify Group Control

Test the group before proceeding to automation updates:

1. **UI Test:** Turn on `light.dining_room_lights` from HA UI
   - [ ] All 3 lights turn on
   - [ ] No errors in HA logs

2. **Brightness Test:** Set group brightness to 75%
   - [ ] All 3 lights dim to 75%
   - [ ] Brightness attribute reflects 75%

3. **Color Temp Test:** Set color temp to 2700K (warm)
   - [ ] All 3 lights shift to warm white
   - [ ] State updates show color_temp: 370 (2700K in mired)

4. **Off Test:** Turn off `light.dining_room_lights`
   - [ ] All 3 lights turn off
   - [ ] No errors or timeout messages

---

## Expected Outcome

After Phase 3 completion:

```
Old Hue Structure:
  light.dining_room (Hue Room Group)
    ├─ light.dining_front_left (Hue)
    ├─ light.dining_back_left (Hue)
    ├─ light.dining_front_right (Hue)
    └─ light.dining_back_right (Hue)

New Z2M Structure:
  light.dining_room_lights (Z2M Group)
    ├─ light.dining_room_front_left (Z2M)
    ├─ light.dining_room_back_left (Z2M)
    ├─ light.dining_room_front_right (Z2M)
    └─ light.dining_room_back_right (Z2M) [pending]
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Group doesn't appear in HA | Reload MQTT integration: HA > Settings > System > Restart or reload Zigbee2MQTT |
| Group appears but members don't respond | Check device linkquality in Z2M admin, ensure coordinator in range, verify devices online |
| Can't select lights in group creation | Ensure lights have joined Z2M (should show in device list), wait 10 sec for discovery |
| Group controls only some lights | Verify all 3 were added to group, check Z2M admin group page for membership |

---

## Phase 4 Prerequisites

Before updating automations in Phase 4:
- [ ] Group created: `light.dining_room_lights`
- [ ] All 3 (or 4) lights added to group
- [ ] Group responds to control commands
- [ ] Dimmer paired to Z2M (for automation trigger)
- [ ] Z2M dimmer device_id obtained
- [ ] Z2M dimmer event codes confirmed from device database

---

**Next Phase:** Phase 4 — Update Automations (see z2m_dining_automation_conversion_template.md)
