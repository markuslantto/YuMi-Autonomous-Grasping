# Autonomous Tactile Grasping with ABB YuMi 

A ROS-based robotic control system enabling an ABB YuMi dual-arm robot to autonomously locate, grasp, and relocate objects using tactile feedback. The system was developed on top of ROS Noetic and integrates YuMi with a custom omni-directional mobile platform ("Sleipner").

![YuMi Grasping Demo](https://drive.google.com/file/d/12tfmOUe6nXlcd2-n-YzafMDeANJ5e0eQ/view)


## Hardware & System Architecture
The physical setup consists of complex industrial and custom-built robotics:
*   **Manipulator:** ABB YuMi, a 14-DOF dual-arm robot.
*   **Mobile Base:** Sleipner, an omni-directional moving platform.
*   **Sensory Input:** Pressure sensors mounted on YuMi's wrists for table edge detection.
*   **Communication:** Externally Guided Motion (EGM) interface bridging the ROS environment with the YuMi hardware controller.

## Software & Control Strategy
The program is built in Python on a Linux system running ROS 1. To achieve soft real-time performance, the architecture relies on concurrent sensor sampling and joint execution:

*   **Multi-threaded ROS Node:** The system is directed by a main control thread handling the sequential pipeline (Ready Pose -> Probing -> Grasping -> Relocation), supported by asynchronous callback threads for force/pressure data and real-time positional data (Coordinated Joint States).
*   **Tactile Probing:** The robot lacks a vision system and instead relies on tactile probing. YuMi executes incremental movements until the wrist pressure sensors register a physical collision exceeding 0.2N, effectively "feeling" its environment.
*   **Geometric Grip Calculation:** Once three probing points ($P_1, P_2, P_3$) are located via tactile feedback, a geometric algorithm calculates the exact orientation angle and local origin of the target object. The system then dynamically computes the required grasping coordinates for the left and right grippers based on the object's width.

## Results
The system successfully demonstrated high repeatability in identifying, grasping, and relocating the target object, even when the object was positioned with an angular misalignment relative to the robot's base. The continuous concurrent sampling ensured that the robot immediately stopped incremental motion upon contact, proving the viability of soft real-time tactile control in a Python setup.

---
*Visuals of the geometric calculation:*

![Probing and Grasping](länk-till-din-figur-3.jpg)
*Figure showing the computed gripping points adjusting to a misaligned target tray.*

**Full Documentation:** *For a detailed breakdown of the project and its code, please check out the [Full project report (PDF)(documentation/YUMI___Rapport(v2).pdf).*
