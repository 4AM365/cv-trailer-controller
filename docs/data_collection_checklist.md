# Dataset Documentation

## Data Collection Strategy

### Camera Equipment
Initial training data is captured with a cellphone set to the same aspect ratio as the deployment camera (to be determined, likely an Arducam). 

**Lens Distortion Assumption:**
We assume lens distortion is not critical for this model and aim for camera-agnostic performance. All cameras involved should be checked for automatic lens correction in video capture mode.

### Geometric Coverage Requirements

**Trailer Articulation Range:**
The tow vehicle articulates relative to the trailer-mounted camera. Data collection must cover:

- **Lateral angle:** ±45° (left/right turns, sharp corners)
- **Vertical angle:** ±20° (hills, suspension travel, loading conditions)

*Note: Coordinate system is defined from the perspective of the camera mounted on the front of the trailer, facing rearward toward the tow vehicle.*

### Lighting Conditions
Capture across boundary conditions to ensure robustness:

- Direct sunlight (sunrise/sunset low angle)
- Night time (headlights, street lights)
- High noon (overhead sun, harsh shadows)
- Overcast/diffuse lighting
- Dusk/dawn transitions

### Vehicle Lighting States
Each collection session should deliberately trigger all relevant states:

#### Primary Light States
- **Running lights only** (tail lights, no other signals)
- **Brake lights** (steady, progressive braking)
- **Left turn signal** (flashing, ~1 Hz)
- **Right turn signal** (flashing, ~1 Hz)
- **Reverse lights** (white, steady when in reverse)
- **Hazard lights** (all turn signals flashing simultaneously)

#### Combined States (Real-World Scenarios)
- **Brake + Left turn** (slowing for left turn)
- **Brake + Right turn** (slowing for right turn)
- **Running lights only** (cruising, no braking/turning)
- **All lights OFF** (engine off, key out - edge case)
- **Daytime running lights** (DRL mode, if different from running lights)

#### Edge Case Configurations
- **Sequential turn signals** (newer vehicles, LED sweep pattern)
- **Pulsing brake lights** (some vehicles pulse on hard braking)
- **One brake light out** (bulb failure, asymmetric pattern)
- **Aftermarket LED conversions** (different brightness/color temperature)
- **Trailer brake controller signal** (if integrated with vehicle lights)

**Data Collection Checklist per Session:**
For each geometric position and lighting condition, cycle through:
1. ☐ Running lights only (10 sec hold)
2. ☐ Brake on/off (3 cycles, varying intensity)
3. ☐ Left turn signal (3-5 blink cycles)
4. ☐ Right turn signal (3-5 blink cycles)
5. ☐ Brake + Left turn (2 cycles)
6. ☐ Brake + Right turn (2 cycles)
7. ☐ Hazard lights (5-7 blink cycles)
8. ☐ Reverse lights (3-5 sec hold)
9. ☐ All lights off momentarily (if safe)

### Data Processing Workflow

1. **Capture:** Record video at full resolution during test drives
2. **Extract:** Sample frames using ffmpeg (typically 1 fps from 30 fps video)
3. **Label:** Annotate full-resolution images with bounding boxes
4. **Train:** YOLOv8 handles downsampling automatically during training

**Example ffmpeg extraction command:**
```bash
ffmpeg -i input.mp4 -vf "select='not(mod(n\,30))'" -vsync 0 data/raw/session_name/frame_%04d.jpg
```

### Collection Planning

**See `data_collection_checklist.md` for:**
- Detailed coverage matrix and priority phases
- Per-session checklists for systematic data capture
- Special scenarios and edge cases to target
- Progress tracking

This document (`datasets.md`) focuses on documenting completed sessions and observations.

---

## Collection Sessions

### 2024-12-06 - Initial Test Drive (Example Template)
- **Vehicle:** [Model/year]
- **Camera:** Pixel 8 Pro, 1920x1080, mounted X ft behind trailer tongue, varied positions
- **Duration:** 15 minutes
- **Conditions:** Daytime, clear, dry pavement
- **States captured:** Brake, left turn, right turn, reverse, hazards
- **Articulation angles tested:** Lateral ±30°, vertical ±10°
- **Lighting conditions:** Mid-afternoon, some sun glare at 3:47 mark
- **Frames extracted:** 45 images (1 fps sampling)
- **Notes:** Good initial visibility test. Need to recapture with more extreme angles and evening lighting.
- **Files:** `data/raw/2024-12-06_initial/`

---

*Add new collection sessions below with date headers. Include any issues, observations, or lessons learned for future collection planning.*