[Go back](./index.html)

# Attendance Tracker

Card reader positioned by office door, reading the card ID and writes it to my local database at home using a simple back end written in Go. Connection established through Tailscale. 
Data aggregation at end of day provides (useless) metrics to help overworked PhD-candidates adhere to Arbeidsmiljøloven.
<img src="/img/attendancetrack.png" alt="Layout" width="100%">


```
┌─────────────────────┐
│   RFID Card Reader  │
│  (Office Entrance)  │
└──────────┬──────────┘
           │
           │ Reads card_id
           ▼
┌─────────────────────┐
│ Raspberry Pi        │
│ Go Backend Service  │
└──────────┬──────────┘
           │
           │ Adds timestamp and direction
           │
           │ Sends data through
           │ Tailscale VPN
           ▼
┌─────────────────────┐
│ Home Raspberry Pi   │
│ InfluxDB Database   │
└──────────┬──────────┘
           │
           │ Stores raw events:
           │ - card_id
           │ - direction (in/out)
           │ - timestamp
           ▼
┌─────────────────────┐
│ End-of-Day Job      │
│ (Go Backend)        │
└──────────┬──────────┘
           │
           │ Fetches today's events
           │
           │ Checks:
           │ - at least 1 entry
           │ - at least 1 exit
           │
           │ Calculates:
           │ - work duration
           ▼
┌─────────────────────┐
│ user_activity_      │
│ summary             │
│ (InfluxDB Measure.) │
└──────────┬──────────┘
           │
           │ Queried by Grafana
           ▼
┌─────────────────────┐
│ Grafana Dashboard   │
│ - Work hours        │
└─────────────────────┘

```
