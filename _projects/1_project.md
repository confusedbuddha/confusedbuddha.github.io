---
layout: page
title: 9 DoF Data Glove 
description: Built off Pollen Robotics design, however with different electronic hardware. 
img: assets/img/glove_full.jpeg
importance: 1
category: work
related_publications: true
---
During my research internship at the Machines in Motion Laboratory, I engineered a 9 Degrees of Freedom (DoF) data collection glove to facilitate imitation learning. 

### 1. Concept: Universal Manipulation Interface (UMI)
Training robotic policies via teleoperation is often slow and expensive[cite: 9]. To solve this, I helped develop a system that uses a human hand as a stand-in for a robot's end-effector[cite: 9]. The device records the gripper state, visual context, and trajectory of a robotic end-effector without requiring actual motors or actuators[cite: 9].

### 2. Hardware Architecture
While the physical chassis was adopted from Pollen Robotics (using a PLA body and TPU grips), I integrated entirely custom electronics to capture high-fidelity telemetry[cite: 9]:
* **Intel RealSense T265:** Combines two fisheye cameras with a built-in IMU to provide real-time 6 DoF pose estimation (X, Y, Z, roll, pitch, yaw)[cite: 9]. 
* **RGB Camera:** Captures the visual scene so the robotic policy can learn the visual context of when and where to act[cite: 9].
* **AS5600 Magnetic Position Sensors:** Record angle data using a small magnet positioned below each encoder[cite: 9].
* **Processing Units:** An Arduino Teensy reads the AS5600 data and passes it to a Raspberry Pi[cite: 9]. The Pi acts as the central brain to route the incoming camera and encoder data[cite: 9].

### 3. Data Synchronization Pipeline
A critical requirement for training an accurate policy is synchronizing data, as the T265, RGB camera, and AS5600 sensors all sample at different rates[cite: 9]. 
* To solve this, the Raspberry Pi assumes all sensor readings within a single loop iteration occur simultaneously and stamps them with a single exact timestamp[cite: 9]. 

### 4. Verification & Visualization
Using the glove, I executed and recorded telemetry for simple manipulation tasks, such as placing a ball inside a basket and cleaning a whiteboard. 
* **Pose Estimation:** Extracted directly from the T265 onboard processing[cite: 9].
* **Visualization:** The synchronized pose coordinates and encoder data were graphed and visualized using ReRun[cite: 9].
* **Simulation:** The data was verified inside MuJoCo physics environments before being processed into Parquet shards to train AI policies via PyTorch LeRobot[cite: 9].

### 5. Engineering Challenges
* **Design Pivot:** I initially designed a highly form-fitted gripper, but pivoted to the current design due to the difficulty of cleanly embedding the encoders[cite: 9]. 
* **Hardware Compatibility:** The system required troubleshooting communication pipelines, as the Raspberry Pi was fundamentally incompatible with the T265 initially, and a lack of available USB ports caused hardware conflicts[cite: 9].