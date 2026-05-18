[Go back](./index.html)

# AFTI-Scope — Autonomous Flow-Through Imaging System
A system capable of imaging suspended particles in water samples under magnification.
[Link to repository](https://github.com/matiah/aftiscope/tree/python_runtime)

### What:
Designing a system capable of imaging and analyzing water samples automatically, targeted for autonomous deployments. Capable of performing computer vision inference on incoming video stream. 
### Why:
To be deployed on autonomous vessels to sample regions of interest - early HAB warning
### How:
A peristaltic pump moves a water sample underneath a magnifying objective. A camera images any particles in the water sample. These images can either be stored on the device, streamed to a preview, or fed to a neural network for object detection and concentration estimation. 

## System Architecture
helhetlig figur

## Technology Stack
* Backend: Python
* Frontend/UI: Home Assistant
* Deployment: Docker Compose
* Embedded control: Microcontroller running GRBL - CNC firmware
* Messaging: MQTT 

## 
