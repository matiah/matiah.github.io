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
A membrane pump pulls water from the surface into the measuring chamber, where the sensors are located. Solenoid valves in conjunction with a pump enable filling and emptying of the measuring chamber. A pressure pump can rinse the optical sensors with freshwater after measurements to prevent/reduce biofouling. 
## System Architecture
#### Control flow
```
Home Assistant (UI + automations)
        ↓
MQTT Broker
        ↓
Go Backend (command handler / logic)
        ↓
WAGO PLC / Relay Controller
        ↓
Pumps + Solenoid Valves
```

#### Data flow
```
Sensors (Chl-a, temperature, oxygen, etc.)
        ↓
Go Backend 
        ↓
InfluxDB (time-series storage)
        ↓
Grafana (visualization & analysis)
```

## Technology Stack
* Backend: Golang
* Frontend/UI: Home Assistant - Grafana
* Deployment: Docker Compose
* Database: InfluxDB
* Messaging: MQTT 

## Technical Choices
- Golang
  - Compiles into small binaries, ideal for computers with little resources
  - Goroutines and channels allows concurrent actions such as sensor readings and pump actuation
- TOML
  - Simplifies setup, sensors are defined in a .toml
    ```
    [devices.dosensor]
    name = "Dissolved Oxygen sensor"
    io_type = "input"
    modbus_type = "RTU"
    usb_path = "/dev/ttyUSB0"
    baud_rate = 9600
    data_bits = 8
    parity = "N"
    stop_bits = 1
    slave_id = 10
    mqtt_topic = "sensor/do"
        [[devices.dosensor.measurements]]
        name = "Temperature"
        address = 2
        length = 1
        unit = "C"
        data_type = "uint32"
            [[devices.dosensor.measurements.offsets]] # subtracting 10000 
            operation="s"
            value=10000
            [[devices.dosensor.measurements.offsets]] # and dividing the result by 10
            operation="d"
            value=10
    ```
  - Allows for different types such as outputs (pumps, solenoids)
    ```
    [devices.wagoIOmodule]
    name = "WAGO IO-module"
    io_type = "output"
    modbus_type = "TCP"
    ip = "192.168.1.6"
    port = 502
        [[devices.wagoIOmodule.outputs]]
        name = "Seawater pump"
        address = 0
        mqtt_topic = "ferrybox/control/do0"
        [[devices.wagoIOmodule.outputs]]
        name = "Rinsing pump"
        address = 2
        mqtt_topic = "ferrybox/control/do1"
    ```
- MQTT
  - Ties back end together with front end.
  - Broker hosted through Docker Compose
- InfluxDB
  - Easily deployable via Docker
  - API allows subscribing directly to MQTT-topics (Current implementation uses Go-API however)
  - tags for metadata/filtering
- Homeassistant - Frontend
  - Status interface, as well as debugging and manually actuating relays
  - Automations used to provide scheduling, such as sampling at specific times
 
## Pictures
<img src="/img/ferryboxio.png" alt="Layout" width="80%">
