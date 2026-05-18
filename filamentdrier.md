[Go back](./index.html)
# DIY PID controlled Filament Dryer
Filament drier designed in Freecad and printed on my Bambu Lab A1 mini using PETG. The sides are laser cut acrylic, insulated to keep the heat in. 




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

### Pictures

