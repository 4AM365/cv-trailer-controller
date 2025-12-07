# Data Requirements

## Collection Phases

### Phase 1: MVP (Minimum Viable Product)
**Goal:** Prove the concept works in controlled conditions

**Requirements:**
- **Lighting conditions:** Overcast daylight
- **Lateral angles:** -30°, -15°, 0°, +15°, +30° (5 stops)
- **Vertical angles:** 0° only
- **Light states:** All primary states (running, brake, L-turn, R-turn, reverse, hazard)
- **Weather:** Clear, dry
- **Target dataset size:** 100-150 labeled frames

**Success criteria:** System reliably detects brake and turn signals in good conditions

---

### Phase 2: Robustness
**Goal:** Handle real-world geometric and lighting variations

**Requirements:**
- **Add extreme angles:** ±45° lateral, ±20° vertical
- **Add lighting conditions:** 
  - High noon (harsh shadows)
  - Night with street lights
  - Dusk/dawn transitions
- **Light states:** All primary + combined states (brake+turn)
- **Weather:** Clear, dry
- **Target dataset size:** +200-300 labeled frames

**Success criteria:** System works reliably across angles and basic lighting variations

---

### Phase 3: Edge Cases
**Goal:** Handle adverse conditions and failure modes

**Requirements:**
- **Weather conditions:**
  - Rain (day and night)
  - Fog/mist
  - Wet camera lens
- **Extreme lighting:**
  - Direct sunrise/sunset (glare/backlit)
  - Minimal night lighting (rural/dark)
- **Special scenarios:** 
  - Partial occlusion
  - Dirty lights
  - Asymmetric light failures
  - Multiple vehicles in frame
- **Target dataset size:** +100-200 labeled frames focused on edge cases

**Success criteria:** System degrades gracefully, maintains safety in poor conditions

---

## Coverage Matrix

Track which scenario combinations have been captured. Update with session date when completed.

| **Lighting Condition** | **Lateral Angles** | **Vertical Angles** | **Weather** | **Priority** | **Status** | **Session** |
|------------------------|-------------------|---------------------|-------------|--------------|------------|-------------|
| Overcast Daylight | -30°, -15°, 0°, +15°, +30° | 0° | Clear | **Phase 1** | ☐ | |
| Overcast Daylight | ±45° | ±10°, ±20° | Clear | Phase 2 | ☐ | |
| High Noon (harsh shadows) | -30°, 0°, +30° | 0° | Clear | Phase 2 | ☐ | |
| Dawn/Dusk transitions | -30°, 0°, +30° | 0° | Clear | Phase 2 | ☐ | |
| Night - Street lights | -30°, 0°, +30° | 0° | Clear | Phase 2 | ☐ | |
| Night - Dark/Rural | -30°, 0°, +30° | 0° | Clear | Phase 3 | ☐ | |
| Direct Sun (backlit/glare) | -30°, 0°, +30° | 0° | Clear | Phase 3 | ☐ | |
| Rain - Daytime | -30°, 0°, +30° | 0° | Wet | Phase 3 | ☐ | |
| Rain - Night | -30°, 0°, +30° | 0° | Wet | Phase 3 | ☐ | |
| Fog/Mist | -30°, 0°, +30° | 0° | Low visibility | Phase 3 | ☐ | |

---

## Lighting Configuration Requirements

### Primary Light States (Required for All Phases)

Must capture each of these at every geometric position and lighting condition:

- **Running lights only** - Tail lights, no braking/turning
- **Brake lights** - Steady illumination, vary intensity if possible
- **Left turn signal** - Flashing pattern (~1 Hz)
- **Right turn signal** - Flashing pattern (~1 Hz)
- **Reverse lights** - White, steady when in reverse
- **Hazard lights** - All turn signals flashing simultaneously

### Combined States (Phase 2+)

Real-world scenarios where multiple light functions are active:

- **Brake + Left turn** - Slowing for left turn
- **Brake + Right turn** - Slowing for right turn
- **Running lights only** - Cruising, no signals

### Edge Case Configurations (Phase 3)

- **Sequential turn signals** - LED sweep pattern (newer vehicles)
- **Pulsing brake lights** - Some vehicles pulse on hard braking
- **One brake light out** - Bulb failure, asymmetric pattern
- **Daytime running lights (DRL)** - If different from running lights
- **Aftermarket LED conversions** - Different brightness/color temp
- **All lights OFF** - Engine off, key out (edge case)

---

## Special Scenarios Checklist

Capture these as opportunities arise or during dedicated edge-case sessions:

### Vehicle Variations
- ☐ Multiple vehicle types (sedan, truck, SUV)
- ☐ LED lights vs incandescent bulbs
- ☐ Sequential turn signals (LED sweep)
- ☐ Aftermarket light modifications
- ☐ Different vehicle colors (affects light reflection)

### Light Failure Modes
- ☐ One brake light out (asymmetric)
- ☐ One turn signal out
- ☐ Dim/failing bulb (reduced brightness)
- ☐ Dirty or mud-covered lights
- ☐ Cracked or damaged light housing

### Environmental Challenges
- ☐ Water droplets on camera lens
- ☐ Direct rain on lens
- ☐ Dust/dirt on lens
- ☐ Sun flare/glare directly in camera
- ☐ Headlight glare from oncoming traffic (night)
- ☐ Reflections from wet pavement
- ☐ Snow accumulation on lights or lens

### Occlusion and False Positives
- ☐ Partial occlusion (object between camera and vehicle)
- ☐ Multiple vehicles in frame
- ☐ Nearby vehicle lights (avoid false positives)
- ☐ Traffic lights in background
- ☐ Building/street lights that could be confused for vehicle lights

### Dynamic Conditions
- ☐ Trailer bounce/sway while driving
- ☐ Rapid angle changes (sharp turns)
- ☐ Motion blur from camera movement
- ☐ Vehicle entering/leaving frame
- ☐ Quick stop (sudden deceleration)

---

## Dataset Size Targets

### Minimum Viable Dataset (Phase 1)
- **100-150 labeled frames** covering basic scenarios
- Focus on quality over quantity
- Ensure all primary light states represented
- Good coverage of ±30° lateral angles at 0° vertical

### Production-Ready Dataset (Phase 1 + 2)
- **300-450 labeled frames** total
- Expanded geometric coverage (±45° lateral, ±20° vertical)
- Multiple lighting conditions (day, night, transitions)
- Combined light states

### Robust All-Conditions Dataset (Phase 1 + 2 + 3)
- **500-650 labeled frames** total
- Comprehensive edge case coverage
- Weather variations (rain, fog)
- Light failure modes
- Environmental challenges

---

## Progress Tracking

**Current Phase:** ☐ Phase 1 | ☐ Phase 2 | ☐ Phase 3

**Phase 1 (MVP):**
- Sessions completed: 0
- Frames extracted: 0
- Frames labeled: 0
- Status: ☐ Not Started | ☐ In Progress | ☐ Complete

**Phase 2 (Robustness):**
- Sessions completed: 0
- Frames extracted: 0
- Frames labeled: 0
- Status: ☐ Not Started | ☐ In Progress | ☐ Complete

**Phase 3 (Edge Cases):**