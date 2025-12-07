# System Architecture

## Overview
This system uses computer vision and sensor fusion to detect tow vehicle lighting states and automatically control trailer lighting, eliminating the need for traditional electrical connections.

## Design Philosophy
**Hybrid Approach: Perception + Logic**

Unlike pure machine learning approaches that require massive training datasets, this system separates concerns:
- **CV Layer**: Detects and tracks physical light sources (what exists, where, when). Based on norfair for object detection.

- **Logic Layer**: Infers semantic meaning from patterns (what it means). Rolling buffer of bayesian classification probabilities follows objects, with temporal subprocesses to populate the dict.

- **Control Layer**: Executes appropriate trailer responses (what to do)

## System Components

### 1. Perception System
**Hardware:**
- Camera module (IMX219 or equivalent) mounted on trailer, forward-facing
- Jetson Nano for real-time inference
- 3-axis accelerometer (I2C/SPI) for motion sensing

**Software:**
- YOLOv8 for light source detection and tracking
- OpenCV for preprocessing and blob analysis
- Kalman filtering for temporal smoothing of detections

**Outputs:**
- Bounding boxes for detected light sources
- Color classification (red, amber, white)
- Brightness/intensity values
- Temporal patterns (steady, flashing, pulsing)

### 2. Inference Logic Layer
**Rule-Based Reasoning:**

Takes detected light patterns and applies geometric and temporal logic:
- **Spatial reasoning**: Position relative to vehicle outline, symmetry, grouping
- **Temporal reasoning**: Simultaneous activation, flash patterns, timing
- **Correlation reasoning**: Relationship to accelerometer data

**State Machine:**
- Tracks vehicle state: stopped, moving, braking, turning left/right, reversing, hazard
- Uses hysteresis and confidence thresholds to prevent flickering
- Handles ambiguous cases (e.g., partial occlusion, sun glare)

**Example Rules:**
```
P(braking | red_lights_on, deceleration) > P(braking | red_lights_on, no_deceleration)

IF (amber_light_left AND flashing_pattern_0.5Hz AND NOT deceleration)
  THEN state = TURNING_LEFT

IF (all_amber_lights AND flashing_pattern_0.5Hz)
  THEN state = HAZARD
```

### 3. Control Layer
**Hardware:**
- GPIO on Jetson Nano
- Relay board for switching trailer light circuits
- Standard 7-pin trailer connector interface

**Safety Features:**
- Watchdog timer - defaults to safe state if system hangs
- Redundant brake light activation on high deceleration regardless of CV confidence
- Manual override capability
- Diagnostic LED indicators for system health

**Latency Requirements:**
- Brake light response: <100ms from vehicle brake activation
- Turn signal response: <150ms acceptable (human perception threshold)

### 4. Sensor Fusion
**Accelerometer Integration:**

Provides ground truth for certain states independent of vision:
- Deceleration confirms braking intent (even if brake lights obscured)
- Acceleration patterns help distinguish between stop-and-go vs true stops
- Vibration signatures can indicate rough road (reduce CV confidence thresholds)

**Fusion Strategy:**
- CV provides primary state detection
- Accelerometer validates and fills gaps
- Disagreement between sensors triggers conservative default (brake lights on)

### 5. Hitch Verification System (Future)
**Concept:**

Use spatial intelligence (computer vision + depth sensing if needed) to verify:
- Coupler latch is in locked position
- Safety chains are connected
- Breakaway cable is attached
- Proper weight distribution on hitch ball

**Alert Mechanism:**
- Visual warning on display
- Audible alarm
- Prevent vehicle movement until verified (optional integration with tow vehicle)

## Data Flow

```
Camera → Image Preprocessing → YOLOv8 Detection → Light Tracking
                                                         ↓
Accelerometer → Motion Analysis ──────────────→ Inference Logic → State Machine
                                                         ↓
                                                  Control Signals → GPIO → Relays → Trailer Lights
```

## Key Architectural Decisions

**Why not end-to-end deep learning?**
- Insufficient diverse training data for rare edge cases
- Interpretability and safety require explainable logic
- Vehicle lighting follows rules, not learned patterns
- Easier to debug and validate for safety-critical application

**Why sensor fusion?**
- Redundancy for safety-critical brake detection
- Accelerometer provides validation independent of visual occlusion
- Helps handle degraded visual conditions (fog, night, glare)

**Why Jetson Nano?**
- Sufficient GPU for real-time YOLOv8 inference
- GPIO for direct hardware control
- Appropriate for eventual production cost point
- Established ecosystem for embedded vision

**Why rules-based logic layer?**
- Vehicle lighting semantics are deterministic, not probabilistic
- Easier to extend to new vehicle configurations
- Transparent decision-making for safety validation
- Generalizes better than pure pattern matching

## Performance Targets

**Accuracy:**
- Brake detection: >99.9% (safety critical)
- Turn signal detection: >98%
- Reverse light detection: >95%

**Latency:**
- End-to-end brake response: <100ms
- Turn signal response: <150ms

**Reliability:**
- False positive rate: <0.1% (avoiding nuisance activations)
- Graceful degradation in adverse conditions
- Safe default state on any system failure

## Future Considerations

- Multi-camera system for 360° awareness
- Integration with tow vehicle CAN bus (when available)
- Cloud connectivity for fleet management
- OTA updates for logic refinements
- Expansion to other trailer safety systems (tire pressure, brake temp, etc.)

## Testing Strategy

**Progressive validation:**
1. Single vehicle, daylight, controlled conditions
2. Expand to multiple vehicle types
3. Nighttime and adverse weather testing
4. Edge cases: sun glare, partial occlusion, nearby traffic
5. Long-term reliability testing
6. Safety failure mode analysis

---

*This architecture document is living - update as design evolves and lessons are learned during implementation.*