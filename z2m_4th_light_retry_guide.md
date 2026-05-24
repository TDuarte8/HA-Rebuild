# 4th Dining Light Retry Guide

**Status:** Pairing pending (1st attempt failed 2026-05-02 ~15:27)  
**Current:** 3 of 4 lights paired to Z2M

---

## Why This Guide

The 4th dining room downlight failed to pair in the initial Phase 2B window. This document provides a retry procedure that can be executed independently or after other Phase 2 tasks.

---

## Lights Already Paired (Reference)

✅ `light.0x00178801027c1190` — Firmware 1.116.12  
✅ `light.0x00178801027c0fe4` — Firmware 1.116.12  
✅ `light.0x00178801027c11c4` — Firmware 1.116.12  

---

## Step 1: Check Permit Join Status

Before attempting retry, verify permit_join is active:

```
Home Assistant > Settings > Devices & Services > Zigbee2MQTT
Look for: switch.zigbee2mqtt_bridge_permit_join
Status: Should be ON for new pairings to succeed
```

If OFF:
1. Go to Switches in HA
2. Find `switch.zigbee2mqtt_bridge_permit_join`
3. Turn it ON
4. You have 254 seconds before it times out

---

## Step 2: Identify the 4th Light

**Location:** Should be one of:
- Dining room ceiling (one of the 4 recessed lights not yet paired)
- If unsure: Check physical breaker switch or ask Teddie where the 4th downlight is

**Model:** Should match the 3 already paired
- Hue white ambiance BR30 flood light
- Firmware version 1.116.8 or similar

---

## Step 3: Reset the Bulb

There are two common reset procedures for Hue bulbs:

### Option A: Power Cycle Reset (Recommended)
1. Turn off the wall switch controlling this light
2. Wait 5 seconds (bulb powers down)
3. Turn wall switch back on
4. Wait 2-3 seconds for bulb to stabilize
5. You should see bulb blink or flicker (indicates ready to pair)

### Option B: Hue-Specific Reset (If Option A doesn't work)
1. Turn wall switch on (light is on)
2. Turn off for 1 second
3. Turn on for 1 second
4. Turn off for 1 second
5. Turn on
6. Repeat 4-5 times rapidly (on-off-on-off pattern)
7. On final "on", leave it on
8. Bulb should blink in pairing mode

---

## Step 4: Watch for Pairing

**In Home Assistant:**
1. Go to Settings > Devices & Services > Devices
2. Search for new lights (sort by "recently added")
3. Watch for appearance of new IEEE address starting with `0x0017880...`

**Expected appearance:** Within 30 seconds of reset

**What you'll see:**
- New device: `light.0x[hex]` (IEEE address as friendly name)
- Model: "Hue white ambiance BR30 flood light"
- Firmware: 1.116.x version

---

## Step 5: Verify the Pairing

Once new device appears:

1. Check in HA: Settings > Devices & Services > Devices
2. Verify it's the 4th light (not a duplicate or false pairing)
3. Check state: Should be "unknown" or "off" initially
4. Click the device to see details:
   - IEEE address
   - Manufacturer: Philips
   - Model: Hue white ambiance BR30 flood light

---

## Step 6: Rename (Optional)

After pairing, you can rename the light for easier reference:

1. Click the device name to edit
2. Rename to: `dining_room_back_right` (or whichever position is the 4th light)
3. In Z2M admin panel, you can also update the friendly name
4. HA will auto-update entity name within 30 seconds

---

## Step 7: Add to Group (Once Phase 3 Complete)

After Phase 3 creates the Z2M group:

1. Go to Z2M admin panel
2. Open the "Dining Room Lights" group
3. Click "Add device(s) to group"
4. Select the newly paired 4th light
5. In HA, verify controlling `light.dining_room_lights` affects the new light

---

## If Pairing Still Fails

### Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Bulb not blinking after reset | Reset not completed properly | Try Option B (rapid on-off pattern), wait longer (10 sec), try again |
| Bulb blinks but doesn't pair | Permit_join window closed | Check `switch.zigbee2mqtt_bridge_permit_join` is still ON, if not turn it on again |
| Device pairs but with wrong name/model | Wrong bulb selected | Check IEEE address matches Hue bulbs (0x0017880...), delete device and re-pair correct one |
| Bulb goes offline immediately after pairing | Out of range or weak signal | Move bulb closer to Z2M coordinator, check Zigbee interference (WiFi on 2.4GHz), try again |
| Multiple identical devices appear | Bulb paired twice | Delete duplicate, keep the one with "on" state, re-pair the missing bulb |

### Advanced Checks

1. **Check Z2M coordinator status:**
   - Settings > Devices & Services > Zigbee2MQTT > Device info
   - Coordinator should show "Status: OK"
   - If not: Restart Z2M bridge

2. **Check linkquality after pairing:**
   - Z2M admin panel > Devices > [4th light]
   - Look for "Link quality" value (1-254, higher is better)
   - If < 50: Device is too far, move closer, restart device
   - If 254 (direct connection): Device may be farthest in network, still OK

3. **Check Zigbee network interference:**
   - List all paired devices in Z2M
   - Count total: Should be < 100 (typically max 128)
   - If network is full: Delete old unused devices first

---

## Workaround: Proceed Without 4th Light

If 4th light cannot be paired:

1. **Proceed with 3 lights:**
   - Create Z2M group with 3 lights (Phase 3)
   - Update automations to use 3-light group (Phase 4)
   - Test with 3 lights (Phase 5)
   - Delete Hue integration (Phase 6)

2. **Return to 4th light later:**
   - After Hue integration deleted, permit_join can be re-enabled anytime
   - Repeat reset and pairing procedure
   - Add 4th light to existing Z2M group manually
   - No additional automation changes needed

---

## Success Criteria

✅ Pairing successful when:
- [ ] New light device appears in HA within 30 seconds of reset
- [ ] Device model shows "Hue white ambiance BR30 flood light"
- [ ] Device can be controlled from HA UI (turn on/off/brightness)
- [ ] Device joins Z2M group successfully (all 4 lights respond to group control)

---

**Retry Window:** Anytime permit_join is active  
**Next Step:** Once paired, proceed to Phase 3 (group creation) or add to existing group after Phase 3
