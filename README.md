Wiring brake controllers for a tow vehicle can be tricky, and they can get damaged.

Instead, let's make trailers smarter. We can use computer vision to detect the actions of a tow vehicle based on sensor fusion of the lights and an accelerometer.

A relatively small dataset can be used to gauge vehicle intent via classical CV methods (segmentation, classification with YOLOv8)

Then, a logical rules-based layer can turn the output into actionable data.

This eliminates the problem of trailer wiring.

A further implementation can use existing hardware and new spatial intelligence techniques to double-check for correct trailer hitching.

Stack:

OpenCV YOLOv8
Python
Norfair



CV layer: detection and tracking of light sources
Logic layer: inference of intent from patterns
Control layer: trailer light actuation based on inferred state

