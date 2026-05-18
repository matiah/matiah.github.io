[Go back](./index.html)

# Ferrybox — HAB warning system
Water sampling system for fish farming vessels, providing time series data for parameters related to (harmful) algal blooms. 
[Link to repository]((https://github.com/matiah/ferrybox-dev/tree/wagoIO))

<img src="/img/Layout.png" alt="Layout" width="49%"> <img src="/img/inference2.png" alt="Layout" width="49%">

### What:
Designing a system capable of imaging and analyzing water samples automatically, targeted for autonomous deployments. Capable of performing computer vision inference on incoming video stream. 
### Why:
To be deployed on autonomous vessels to sample regions of interest - early HAB warning
### How:
A peristaltic pump moves a water sample underneath a magnifying objective. A camera images any particles in the water sample. These images can either be stored on the device, streamed to a preview, or fed to a neural network for object detection and concentration estimation. 

## System Architecture
#### Hardware
<img src="/img/Systemoverview.png" alt="Layout" width="49%"> <img src="/img/hardware.png" alt="Layout" width="49%">
#### Software
<img src="/img/software.png" alt="Layout" width="90%">

## Technology Stack
* Backend: Golang
* Frontend/UI: Home Assistant - Grafana
* Deployment: Docker Compose
* Database: InfluxDB
* Messaging: MQTT 

## Technical Choices
- Python best suited due to the level of prototyping
  - Camera manufacturer provided SDK with Python example code.
  - Multithread runtime
    - Message handling
    - Writing data to microcontroller
    - Camera implemented as a class running in its own thread
  - Virtual camera enables webcam stream and CNN output device
- Running inside privileged ML-container supplied by NVIDIA
  - Provides Python libraries with GPU-accelerated resources (such as image saving and conversion)
  - GPU-acceleration for machine vision inference
  - Simplified deployment of a machine vision development stack
- MQTT messaging ties everything together
- Home Assistant frontend
  - Low hanging fruit for frontend development
  - Live camera output
  - Change camera and pump settings
 
## Pictures
<img src="/img/inference.png" alt="Layout" width="90%">
<img src="/img/Otterintegration.jpg" alt="Layout" width="90%">
