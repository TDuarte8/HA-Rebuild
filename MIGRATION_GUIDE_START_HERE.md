# Dining Room → Z2M Migration Guide

**Status:** Ready to begin  
**Date:** 2026-05-02  
**Goal:** Migrate dining room + dimmer to Z2M, then safely delete Hue integration

---

## 📋 What You're About to Do

1. **Pair 4 dining room downlights to Z2M** (remove from Hue)
2. **Pair Hue dimmer to Z2M** (if compatible)
3. **Update automations** to use Z2M devices instead of Hue
4. **Test everything** to ensure it works
5. **Delete Hue integration** safely
6. **Keep main bedroom working** perfectly

**Total time:** ~2 hours  
**Difficulty:** Medium (follow the guides step-by-step)

---

## 📂 Document Guide

Read these IN ORDER:

### START HERE 👈 (You are here)

**1. `migration_phase1_checklist.md` — 20 minutes**
   - Quick checklist of what to prepare
   - Verify hardware and dimmer compatibility
   - **Do this first!**

### FULL PLAN

**2. `dining_room_z2m_migration_plan.md` — Reference guide**
   - Detailed 6-phase migration plan
   - Step-by-step instructions
   - Testing procedures
   - **Read before starting Phase 2**

### TECHNICAL REFERENCE

**3. `z2m_dimmer_event_reference.md` — Automation details**
   - How to convert Hue automations to Z2M format
   - Button event codes
   - Troubleshooting
   - **Use when updating automations**

### AUDIT REPORTS (For reference)

**4. `hue_migration_audit.md` — Device audit details**
   - Full list of all Hue devices
   - Which ones migrated, which ones didn't
   - Automation dependencies

**5. `full_hue_migration_status.md` — Strategic overview**
   - Complete migration status
   - Risk assessment
   - Decision matrix

---

## 🚀 Quick Start (5 min read)

### Phase 1: Preparation (Do NOW)
```
1. Open migration_phase1_checklist.md
2. Complete all checks (20 min)
3. Verify dimmer compatibility
4. If everything checks ✅, proceed to Phase 2
```

### Phase 2: Pairing Lights (60 min)
```
1. Enable Z2M permit_join
2. Reset and pair each dining light
3. Create Z2M group
4. Test all 4 lights respond
```

### Phase 3: Pair Dimmer & Update Automations (30 min)
```
1. Pair dimmer to Z2M (if supported)
2. Update brightness automations
3. Test button presses work
```

### Phase 4: Final Testing (30 min)
```
1. Test individual lights
2. Test group control
3. Test voice commands
4. Test automations
5. Test main bedroom still works
```

### Phase 5: Delete Hue Integration (5 min)
```
1. Create HA backup
2. Delete Hue integration
3. Verify all lights still work
4. Done! ✅
```

---

## ⚠️ Important Decisions

### IF Dimmer Is Z2M Compatible ✅
- Follow the full migration plan
- Will take ~2 hours total
- Brightness automations will work

### IF Dimmer Is NOT Z2M Compatible ❌
- Choose Option B instead: Disable automations
- Takes ~30 min
- Lose dimmer button control but lights still work
- Or: Find alternative Z2M button

---

## 🎯 Success Criteria

You'll know you're done when:

✅ All 4 dining lights pair to Z2M  
✅ Dining room group created  
✅ Dimmer paired to Z2M (or automations disabled)  
✅ Brightness automations work (if dimmer migrated)  
✅ All tests pass  
✅ Hue integration deleted  
✅ No errors in HA logs  
✅ Main bedroom still perfect  

---

## 🔧 Tools You'll Need

- **Home Assistant:** Already configured
- **Z2M Admin Panel:** Access via HA Settings
- **Light bulbs:** The 4 dining downlights
- **Hue dimmer:** The physical dimmer switch
- **Physical reset:** Way to reset the bulbs (typically holding power 5 sec)

---

## 📞 If Something Goes Wrong

1. **Refer back to the relevant guide**
   - Pairing issues → `dining_room_z2m_migration_plan.md` Phase 2
   - Automation issues → `z2m_dimmer_event_reference.md`
   - General questions → `full_hue_migration_status.md`

2. **Check the troubleshooting sections**
   - Each guide has a "Troubleshooting" or "Known Issues" section

3. **Rollback option**
   - Restore HA backup if needed
   - Z2M devices stay paired, just start over

---

## 📊 Current Status (Updated 2026-05-02)

| Item | Status | Impact | Next Step |
|------|--------|--------|-----------|
| Main Bedroom | ✅ Migrated | Ready to use | Keep as-is |
| Kitchen Lights | ✅ Migrated | Ready for Hue deletion | Phase 6 after dining |
| Dining Room Lights | 🟡 3 of 4 on Z2M | Phase 2B complete | Complete Phase 2C (dimmer) |
| Hue Dimmer | ⏳ Pairing in progress | Phase 2C in progress | Awaiting dimmer reset |
| Brightness Automations | ⏳ Ready to update | Will update in Phase 4 | After dimmer pairs |

---

## 🚀 Current Phase

### Phase 2: Device Pairing to Z2M — IN PROGRESS

**What's Done:**
- ✅ Phase 2A: Permit_join enabled multiple times
- ✅ Phase 2B: 3 of 4 dining lights paired to Z2M
- ⏳ Phase 2C: Dimmer pairing in progress (permit_join active, awaiting user action)

**What's Ready:**
- All Phase 3-5 documentation prepared and ready to execute
- Group creation guide prepared
- Automation conversion templates ready
- Comprehensive testing procedures ready
- 4th light retry guide available

---

## 📝 Next Actions (Choose One)

### Option 1: Complete Dimmer Pairing (Recommended) ⏳
1. Reset the Hue dimmer switch (hold power 3-5 sec)
2. Put it in pairing mode
3. Wait for new `event.hue_dimmer_*` entities in Home Assistant
4. Let me know when dimmer appears

→ See `PHASE2_CURRENT_STATUS.md` for full instructions

### Option 2: Skip Dimmer (If Incompatible) ⚡
1. Check Z2M device database for your dimmer model
2. If not supported, proceed without dimmer automation
3. Let me know: "Dimmer not supported"

→ See `z2m_group_creation_guide.md` to proceed with 3 lights

### Option 3: Retry 4th Light (Independent) 🔄
1. Reset the 4th dining light (wall switch off/on)
2. Watch for new light entity in Home Assistant
3. Let me know when it appears

→ See `z2m_4th_light_retry_guide.md` for full procedure

---

## 📚 Documents Ready to Execute

Once Phase 2 completes:

1. **Phase 3:** `z2m_group_creation_guide.md` — Create Z2M group (10 min)
2. **Phase 4:** `z2m_dining_automation_conversion_template.md` — Update automations (10 min)
3. **Phase 5:** `z2m_migration_phase5_testing.md` — Test everything (30-45 min)
4. **Phase 6:** Full migration plan Phase 6 section — Delete Hue integration (5-10 min)

---

## ⏱️ Timing Estimate (From Here)

- **Phase 2C (Dimmer):** 5-10 minutes + your action
- **Phase 3-6 (Automated):** ~65-80 minutes total
- **TOTAL:** ~75-90 minutes to completion

---

## 🔌 Current Hardware Status

**Paired to Z2M:**
- ✅ light.0x00178801027c1190 (Hue white ambiance BR30)
- ✅ light.0x00178801027c0fe4 (Hue white ambiance BR30)
- ✅ light.0x00178801027c11c4 (Hue white ambiance BR30)

**Still Pending:**
- ⏳ Hue dimmer switch (permit_join active, awaiting reset)
- ⏳ 4th dining light (can retry independently)

---

## 💡 Questions?

- **Phase 2 status?** → See `PHASE2_CURRENT_STATUS.md`
- **Dimmer won't pair?** → Check `z2m_dimmer_event_reference.md` troubleshooting
- **4th light won't pair?** → See `z2m_4th_light_retry_guide.md`
- **Ready for Phase 3?** → See `z2m_group_creation_guide.md`

---

## ✅ Ready?

Complete Phase 2C (dimmer pairing or skip), then let me know your status. I'll execute Phases 3-6 immediately!
