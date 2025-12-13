# Problem
Wiring brake controllers for a car to tow a trailer can be tricky, and they can get damaged.

# Approach
Instead, let's make trailers smarter using small cameras and computers. We can use computer vision techniques to detect the actions of a tow vehicle's lights based on the brightness, location, and changes in these values over time.

Then, we can use that data to control trailer lights and brakes. This eliminate the problem of altering the wiring of a tow vehicle. In the future, we can add more computer vision capabilities.

# Challenges
- Vehicle light configurations vary
- Weather conditions vary
- Vehicle pose varies
- Other lights will be visible beyond the envelope of the tow vehicle
- Being a safety critical application, a safety testing framework must be constructed
- Failsafe behavior must achieve a minimum risk condition

# Stack

- YOLOv8
- Python
- Norfair
- Boolean logic

# Literature Search

Research on intelligent vehicle lamp signal recognition in traffic scene, Shi, Qi, Liu, Yang
    
    https://link.springer.com/article/10.1007/s42452-022-05211-9

    This approach uses YOLOv3 to detect vehicles and then uses hue-saturation-value data to improve detection. The data is fed into a DNN for processing and detection tasks.


Autonomous Tracking of Vehicle Rear Lights and Detection of Brakes and Turn Signals - Almagambetov, Casares 2012 
    
    https://www.researchgate.net/publication/235676076_Autonomous_Tracking_of_Vehicle_Rear_Lights_and_Detection_of_Brakes_and_Turn_Signals
    
    Use of Kalman Filter and Codebook to achieve very good signal detection performance.

Vision-based Driver Assistance Systems: Survey,
Taxonomy and Advances: Horgan, Hughes, McDonald, Yogamani 2021
    
    https://arxiv.org/pdf/2104.12583

    Introduces taxonomy for ADAS, includes review of historical approaches and benchmarks/limitations.

    Takeaways: 
    30hz for low speed maneuvering cameras
    15hz at night for longer exposure time. 
    Early methods: Hough Transform, Canny Edge Detection.
    CNNs displaying better performance in detection/segmentation 2020 onward. 
    Pixel segmentation: Labels each pixel into category.
    Long et al 2015 showed FCN efficacy for semantic segmentation, allowing ADAS to detect fine details
    DNN & RL great for making decisions from large volumes of data
    Region-based CNN great for object detection & collision avoidance

    Key problem is sensor fusion
    Must integrate camera, LiDAR, ultrasonic, radar; cameras suffer in poor visibility / illumination
    Kalman filter principal method for fusing inputs

    Real-time tracking key issue to to compute constraints

    Single Shot multibox Detector SSD
