[Go back](./index.html)

# Ferrybox — HAB warning system
Water sampling system for fish farming vessels, providing time series data for parameters related to (harmful) algal blooms. 
[Link to repository](https://github.com/matiah/ferrybox-dev/tree/wagoIO)

<img src="/img/ferrybox.png" alt="Layout" width="80%">

### What:
Systems that pulls water from the ocean into a measuring chamber where sensors measure HAB-related parameters such as Chl-a, oxygen, temperature, etc. Time series data is stored in a database (InfluxDB) and is displayed using Grafana. 
### Why:
Time series data of HAB related parameters may provide an early warning or greater observation of trends that may be detrimental to fish health. 
### How:
A membrane pump pulls water from the surface into the measuring chamber, where the sensors are located. Solenoid valves enable filling and emptying of the measuring chamber. A pressure pump can rinse the optical sensors with freshwater after measurements to prevent/reduce biofouling. 
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
- Golang 
 
## Pictures
<img src="/img/inference.png" alt="Layout" width="90%">
<img src="/img/Otterintegration.jpg" alt="Layout" width="90%">
