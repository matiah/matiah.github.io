[Go back](./index.html)
# DIY PID controlled Filament Dryer
Filament drier designed in Freecad and printed on my Bambu Lab A1 mini using PETG. The sides are laser cut acrylic, insulated to keep the heat in. 

<img src="/img/Overview.png" alt="Layout" width="32%"> <img src="/img/Spools.png" alt="Layout" width="34%">

Two fans circulate air through a heatsink which is placed on the heated bed of the Bambu lab printer. 

<img src="/img/Fans_temp.png" alt="Layout" width="34%"><img src="/img/Sideview.png" alt="Layout" width="49%">

The ESP32C3 monitors the temperature and uses a PID controller to change the temperature of the heated pad of the A1 Mini.
Filament profiles and live data can be viewed in the Home Assistant interface.

<img src="/img/Phone1.png" alt="Layout" width="34%"><img src="/img/Phone2.png" alt="Layout" width="33%">



## Flow diagram
```
┌──────────────────────────────┐
│     Home Assistant UI        │
│  - Set target temperature    │
│  - Monitor humidity/temp     │
│  - View dryer status         │
└──────────────┬───────────────┘
               │
               │ WiFi / ESPHome API
               ▼
┌──────────────────────────────┐
│        ESP32-C3              │
│      (ESPHome Node)          │
├──────────────────────────────┤
│ - Polls temp/humidity        │
│   sensor every second        │
│ - Runs PID controller        │
│ - Calculates heater output   │
└───────┬───────────┬──────────┘
        │           │
        │           │ Reads
        │           ▼
        │    ┌─────────────────┐
        │    │ Temp + Humidity │
        │    │ Sensor          │
        │    └─────────────────┘
        │
        │ Controls heated bed
        ▼
┌──────────────────────────────┐
│      Bambu Lab A1 Mini       │
│       Heated Print Bed       │
└──────────────┬───────────────┘
               │
               │ Transfers heat
               ▼
┌──────────────────────────────┐
│        Heatsink Block        │
│   placed on heated bed       │
└──────────────┬───────────────┘
               │
               │ Warm airflow
               ▼
┌──────────────────────────────┐
│        Dual Fan System       │
│ - Circulates enclosure air   │
│ - Pushes air through sink    │
└──────────────┬───────────────┘
               │
               │ Heated dry air
               ▼
┌──────────────────────────────┐
│      Filament Enclosure      │
│ - Holds 2 filament spools    │
│ - Maintains warm dry air     │
└──────────────────────────────┘


                 Control Loop
                 ============
     Sensor ──► ESP32 PID ──► Heated Bed
        ▲                          │
        └────── Environment ◄──────┘
``` 


