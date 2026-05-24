# Phase 3: Group Creation — EXECUTION INSTRUCTIONS

**Status:** Ready to execute — User action required  
**Date:** 2026-05-02  
**Duration:** ~10 minutes  
**Blocker:** None — 3 lights ready, dimmer paired, Z2M online

---

## Current System State

### Paired Devices ✅
- ✅ 3 dining room lights: 0x00178801027c1190, 0x00178801027c0fe4, 0x00178801027c11c4
- ✅ Hue dimmer switch: 0x0017880103c85e10
- ✅ Z2M bridge: Online and responding
- ✅ Z2M permit_join: Can be re-enabled anytime

### What's Needed
- Create Z2M group named "Dining Room Lights"
- Add 3 paired lights to group
- Verify `light.dining_room_lights` entity appears in HA

---

## QUICK START: Manual Group Creation (5 steps)

### Step 1: Open Z2M Admin Panel

**Method A (Easiest):**
1. Open Home Assistant
2. Go: **Settings > Devices & Services > Zigbee2MQTT**
3. Click: **"Open Zigbee2MQTT"** button (top right)
4. Z2M admin panel opens in new tab

**Method B (Direct URL):**
- If Method A doesn't work, try entering in address bar:
- `http://45df7312-zigbee2mqtt.local.hass.io:8099`
- Or: `http://172.30.33.4:8099`

---

### Step 2: Navigate to Groups Section

1. In Z2M admin panel, look for left sidebar menu
2. Find and click: **"Groups"** or **"Manage Groups"**
3. You should see empty groups list (or existing groups if any)

---

### Step 3: Create New Group

1. Click: **"Create new group"** or **"Add group"** button
2. Dialog or form appears with:
   - **Group name field:** Enter `Dining Room Lights`
   - **Group ID:** Leave blank/auto-assigned (Z2M will create sequential ID)
   - Other fields: Leave defaults
3. Click: **"Create"** or **"Add"** button

**Expected result:** Group appears in list with ID (likely 100, 101, or similar)

---

### Step 4: Add Devices to Group

1. Click on the new "Dining Room Lights" group (from list)
2. You should see group details page with:
   - Group name
   - Member devices section (likely empty)
3. Click: **"Add device to group"** or **"Edit members"** or similar
4. Select devices dialog appears showing available devices:
   - Look for IEEE addresses: 
     - ✅ `0x00178801027c1190`
     - ✅ `0x00178801027c0fe4`
     - ✅ `0x00178801027c11c4`
5. **SELECT ALL THREE** by:
   - Clicking checkbox next to each IEEE address, OR
   - Using "Select all" if available
6. Click: **"Add selected"** or **"Apply"** or **"Confirm"**

**Important:** Do NOT add the dimmer (0x0017880103c85e10) — it's a control device, not a light

**Expected result:** All 3 lights now show as group members

---

### Step 5: Verify in Home Assistant

1. Return to Home Assistant (or refresh)
2. Go: **Settings > Devices & Services > Devices**
3. Search for: `dining` or refresh the list
4. Look for new light entity: **`light.dining_room_lights`**

**If found ✅:**
- Click the entity to verify it shows as a "Light" type
- Check the device shows 3 members
- Proceed to testing below

**If NOT found:**
- Wait 30 seconds (HA needs to sync with Z2M)
- Refresh the page
- Check Z2M admin panel to verify group was created
- If still not appearing, see Troubleshooting section below

---

## VERIFICATION TEST (Optional but Recommended)

Once `light.dining_room_lights` appears in HA:

### Quick Control Test

1. In HA, find and click `light.dining_room_lights`
2. Use the light card to:
   - **Turn ON:** All 3 lights should turn on
   - **Set brightness to 75%:** All 3 should dim to 75%
   - **Turn OFF:** All 3 should turn off

**Expected result:** All 3 lights respond as a group with no lag

### Alternative Test (HA UI)

1. Go to: **Home > Lights**
2. Find: `dining_room_lights`
3. Click entity to expand options
4. Toggle on/off — verify all 3 lights respond

---

## TROUBLESHOOTING

| Issue | Cause | Solution |
|-------|-------|----------|
| "Can't find Z2M admin panel" | Z2M not accessible from your network | Try direct URL: http://45df7312-zigbee2mqtt.local.hass.io:8099 or restart Z2M add-on |
| "Group created but no entity in HA" | Sync delay or reload needed | Wait 30 sec, then reload MQTT integration: Settings > System > Reload MQTT or restart HA |
| "Can't see 3 lights in device selection" | Lights not fully discovered yet | Go to Z2M Devices list, verify all 3 appear there first |
| "Only some lights responding to group" | Not all lights added to group | Go back to Z2M group, verify all 3 are members |
| "Group doesn't appear in Z2M list after creation" | Creation may have failed | Try refreshing Z2M page, or create again with different name |

---

## NEXT: What Happens After Phase 3

Once `light.dining_room_lights` is verified working:

1. **Phase 4:** Update automations to use the new Z2M dimmer and group
   - See: `z2m_dining_automation_conversion_template.md`
   - Duration: ~10 min

2. **Phase 5:** Test all functionality before Hue deletion
   - See: `z2m_migration_phase5_testing.md`
   - Duration: ~30-45 min

3. **Phase 6:** Delete Hue integration safely
   - Takes: ~5-10 min

---

## Required Information for Next Phase

**Once group is created, note:**
- [ ] Group entity name: `light.dining_room_lights`
- [ ] Group ID in Z2M: ___ (usually 100+)
- [ ] All 3 lights responding to group control: Yes/No

---

## IMPORTANT: Do NOT Proceed to Phase 4 Until:

- ✅ Group "Dining Room Lights" is created in Z2M
- ✅ All 3 lights added to group
- ✅ Entity `light.dining_room_lights` appears in HA
- ✅ Group responds to on/off and brightness commands

---

**Ready to create the group?**

1. Complete the 5 steps above
2. Verify `light.dining_room_lights` appears in HA
3. Report back: "Phase 3 complete" or let me know if you encounter issues

**Time estimate:** 5-10 minutes

---

Generated: 2026-05-02  
Status: Ready for user execution
