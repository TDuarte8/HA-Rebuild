# Main Bedroom Hue → Z2M Migration Requirements

**Date:** 2026-04-30  
**Status:** In progress

---

## Current State

### Light Entities (already on Z2M ✓)

| Entity | Friendly Name | Z2M Group ID | Area | Aliases |
|--------|--------------|--------------|------|---------|
| `light.main_bedroom_lamps` | Main Bedroom Lamps | 3 | bedroom | bedside lamps, bedroom lamps |
| `light.main_bedroom_lights` | Main Bedroom Lights | 2 | bedroom | bedroom lights, the bedroom lights |
| `light.main_bedroom_overhead_lights` | Main Bedroom Overhead Lights | 4 | bedroom | overhead lights, bedroom overhead |

`light.main_bedroom_lights` is a combined Z2M group containing both lamps and overhead lights.  
Physical layout: **no wall lights** — only bedside lamps and overhead ceiling lights. Lamps are the primary fixture; overhead is rarely used.

### Hue Remnants to Remove

Three Hue group devices remain in the registry, each owning a set of scenes:

| Hue Device ID | Scope | Scene count |
|---------------|-------|-------------|
| `d5d58bf78196b09cbee243470f825b83` | Room-wide (Main Bedroom) | 11 |
| `7e54c9f5efb35d78052498535942683a` | Lamps group | 11 |
| `6c7c9e944fbe2d627594de650a0bd72e` | Overhead Lights group | 11 |

**Hue scenes to remove: 33** (all `platform: hue`) — only 21 will be recreated; color/fantasy scenes are dropped

One Z2M-native scene already exists and must be kept:
- `scene.main_bedroom_lamps_1_main_bedroom_fireplace` (Fireplace, `platform: mqtt`)

---

## Requirements

### 1. Remove Hue Devices and Scenes

- Search automations for references to old Hue scene entity IDs **before** deleting anything (see §5)
- Delete the 3 Hue group devices from the device registry; their 33 scenes will be removed with them
- Keep the Z2M Fireplace scene untouched

### 2. Recreate Scenes as Native HA Scenes

Create **three sets** of scenes — lamps-only, overhead-only, and combined — to preserve the same granularity as the current Hue setup. Lamps scenes are the ones that will be used day-to-day; overhead and combined exist for completeness.

#### Scene light values

Color/fantasy Hue scenes (Arctic Aurora, Savanna Sunset, Tropical Twilight, Spring Blossom) are intentionally not recreated. Only practical white-temperature scenes are kept.

| Scene | Color Temp (mireds) | Brightness (0–255) | Notes |
|-------|--------------------|--------------------|-------|
| Bright | 233 (4300K) | 254 | Cool white, full brightness |
| Concentrate | 233 (4300K) | 219 | Cool white, slightly dimmed |
| Energize | 156 (6410K) | 254 | Daylight, full brightness |
| Read | 346 (2890K) | 240 | Warm white, high brightness |
| Relax | 447 (2237K) | 144 | Warm amber, mid brightness |
| Dimmed | 369 (2710K) | 77 | Warm, very dim |
| Nightlight | 500 (2000K) | 30 | Warm, minimal light |

#### Target entities per set

| Set | Target entity | New entity_id prefix |
|-----|--------------|----------------------|
| Lamps only | `light.main_bedroom_lamps` | `scene.bedroom_lamps_*` |
| Overhead only | `light.main_bedroom_overhead_lights` | `scene.bedroom_overhead_*` |
| Combined | `light.main_bedroom_lights` | `scene.bedroom_*` |

Each set gets all 7 scenes above → **21 new scenes total**.

### 3. Area Assignment

All three light entities are already assigned to area `bedroom` (Main Bedroom, Second floor) — no action needed.

### 4. Verify Voice Aliases

Current aliases already set on Z2M entities — confirm they work with Assist after migration:
- "bedside lamps", "bedroom lamps" → `light.main_bedroom_lamps`
- "bedroom lights", "the bedroom lights" → `light.main_bedroom_lights`
- "overhead lights", "bedroom overhead" → `light.main_bedroom_overhead_lights`

Add any additional aliases Teddie uses.

### 5. Automation Audit

Before removing Hue devices, search all automations for references to these entity IDs:

```
scene.main_bedroom_energize
scene.main_bedroom_relax
scene.main_bedroom_bright
scene.main_bedroom_dimmed
scene.main_bedroom_nightlight
scene.main_bedroom_concentrate
scene.main_bedroom_read
scene.main_bedroom_arctic_aurora
scene.main_bedroom_savanna_sunset
scene.main_bedroom_tropical_twilight
scene.main_bedroom_spring_blossom
scene.main_bedroom_lamps_relax
scene.main_bedroom_lamps_bright
scene.main_bedroom_lamps_dimmed
scene.main_bedroom_lamps_nightlight
scene.main_bedroom_lamps_energize
scene.main_bedroom_lamps_concentrate
scene.main_bedroom_lamps_read
scene.main_bedroom_lamps_arctic_aurora
scene.main_bedroom_lamps_savanna_sunset
scene.main_bedroom_lamps_tropical_twilight
scene.main_bedroom_lamps_spring_blossom
scene.main_bedroom_overhead_lights_bright
scene.main_bedroom_overhead_lights_relax
scene.main_bedroom_overhead_lights_dimmed
scene.main_bedroom_overhead_lights_energize
scene.main_bedroom_overhead_lights_concentrate
scene.main_bedroom_overhead_lights_read
scene.main_bedroom_overhead_lights_arctic_aurora
scene.main_bedroom_overhead_lights_savanna_sunset
scene.main_bedroom_overhead_lights_tropical_twilight
scene.main_bedroom_overhead_lights_spring_blossom
scene.main_bedroom_overhead_lights_nightlight
```

Note: the color scenes (arctic_aurora, savanna_sunset, tropical_twilight, spring_blossom) are not being recreated — if any automation references them, remove the reference rather than updating it.

Update any references to the new `scene.bedroom_*` entity IDs.

### 6. Open Items

- [ ] Confirm `light.main_bedroom_lights` Z2M group actually contains both lamps and overhead bulbs
- [ ] Check if any Hue physical switches/dimmers in bedroom still need Z2M migration

---

## Migration Steps (ordered)

1. Audit automations for old Hue scene entity ID references and note what needs updating
2. Create 21 new HA scenes (3 sets × 7 scenes) targeting Z2M light entities
3. Test all new scenes
4. Update automation references from old to new scene entity IDs; remove any references to color scenes
5. Delete the 3 Hue group devices from the device registry
6. Verify voice control works for all aliases (test with Assist)
