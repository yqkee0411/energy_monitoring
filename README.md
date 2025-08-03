# Energy Monitoring
A DIY IoT project to monitor and analyze household electricity consumption in real-time using **ESP8266 (flash with Tasmota)**, **PZEM-004T energy meters**, and **Home Assistant**. This system provides detailed energy usage data, historical trends, and cost calculations based on local electricity tariffs.

---
## Acheivement

- Monitors **3 independent power circuits** in real-time using ESP8266 + PZEM-004T.
- Integrated with **Home Assistant dashboards** for historical trends and cost analysis.
- Calculates **daily and monthly electricity costs** with tariff rates.
- Achieved insights into high-usage appliances (e.g., refrigerator compressor cycles, AC running).
- Fully open-source, low-cost retrofit

---

## Overview
It uses three PZEM-004T modules connected via I²C to an ESP8266 running Tasmota firmware. Data is published over MQTT to Home Assistant, where dashboards display real-time usage, daily costs, and historical trends. 

## Motivation
<!-- The reason for the creation of this project is that my mom have been complaning about the electricity being expensive since i move in and i have been frustrated about it since, as I have no idea what's using the electricity and how to cut the usage, so I decided on making myself a cheap enery monitoring system to know what's using the electricity. That is why i have gone the cheapest route i am comfortable with. -->

- Electricity costs increased after I moved back home.
- No visibility into which appliances were consuming the most power.
- Wanted a **low-cost, DIY energy monitoring solution** for learning and optimization energy usage.


**Key objectives:**
- Real-time monitoring of power usage across three phase.
- Cost calculation based on Malaysia’s electricity tariff.
- Historical data visualization for energy optimization.
- Low-cost and modular hardware setup.

---

## Features

- **Real-time power monitoring:** Voltage, current, power, energy, and frequency.
- **Multi-sensor setup:** Three PZEM-004T meters connected via I²C with individual addresses.
- **Home Assistant dashboard:** Live usage charts and daily/weekly/monthly trends.
- **Cost calculation:** Automatic cost estimation based on custom tariff rates.
- **Open-source stack:** Tasmota, MQTT, and Home Assistant integration.

---

## System Architecture

### Hardware
- **ESP8266 with 220v power supply built in**
- **3× PZEM-004T v3 energy meters**
- I²C communication (custom addresses)
- Wi-Fi network for MQTT data transfer

ESP8266            |  PZEM-004T
:-------------------------:|:-------------------------:
 <img width="516" height="443" alt="ESP8266 Board" src="https://github.com/user-attachments/assets/3275f037-9959-4f23-868c-a8544d07bf31" />| <img width="516" height="443" alt="PZEM004T" src="https://github.com/user-attachments/assets/054bf498-b332-4e61-bc4d-1f7170ad167e" />

So i have gone for the ESP8266 with a 220v power supply and relay although i didn't need the relay but its the cheapest i can find at the time and it came with 220v power supply so no need external power supply, as for the PZEM-004T i have go for the Split core style CT so i don't need to messed around with disconnecting the cable, the cable was really stiff and i am not that comfortable with disconnect it and reconnect it without proper tools.

### Software
- Tasmota firmware on ESP8266
- MQTT broker (e.g., Mosquitto)
- Home Assistant for visualization and automation
- YAML configuration for sensor integration and cost calculation

---

## Wiring & Setup

### Before installation

1. Flash Tasmota firmware on the ESP8266
2. Connect each PZEM-004T to the ESP8266 using I²C and configure it with unique addresses
3. Connect all PZEM-004T to the ESP8266 and verify that all is working
4. Set up MQTT.
5. Integrate into Home Assistant via MQTT and ensure all is working.

<!--
**Installation**
As in my scenario there is 3 phase so each live / phase will need to be connect to each PZEM-004T
1. Ensure the breaker is turned off for the intended connect point (or you can turn main breaker off, but you will lose all electricity)
2. Connect each live / phase to each PZEM-004T and one of the live / phase to esp32 (what ever phase you like as it is just for powering)
3. Connect all neutral of the PZEM-004T to the neutral and also the neutral of the esp
4. Connect all CT clamp to the correct phase **(crucial as if wrong wire is clamp, reading will be incorrect)**
   -->
### Installation (3-Phase Setup)

> **⚠️ Safety Warning**  
> Working with mains electricity can be dangerous. Ensure the breaker is switched off before wiring. If unsure, turn off the **main breaker** (note: this cuts power to the entire house).

This setup uses three PZEM-004T modules — one for each phase (L1, L2, L3). One phase also powers the ESP8266.

---

#### Steps

1. **Power Off**
   - Turn off the breaker for the specific connection point.  
   - Optionally, turn off the main breaker for full isolation.

2. **Connect Live Wires**
   - Connect each live phase (L1, L2, L3) to the corresponding PZEM-004T input.  
   - Choose **any one phase** to also power the ESP8266 (only one is required for power).

3. **Connect Neutral Wires**
   - Connect all PZEM-004T neutrals together and to the house neutral.  
   - Connect the ESP8266 neutral to this common neutral as well.

4. **Attach CT Clamps**
   - Clip each CT clamp around the **matching phase wire** (L1 → PZEM1, L2 → PZEM2, L3 → PZEM3).  
   - **Important:** Incorrect pairing will result in inaccurate readings.

<!-- *(Insert wiring diagram and screenshots here)* -->

Picture            |  Description
:-------------------------:|:-------------------------:
<img width="780" height="238" alt="image" src="https://github.com/user-attachments/assets/d72e1b51-2cc0-4e31-88a3-3a0b0b5d7ab5" /> | PZEM-004T wiring diagram
<img width="516" height="443" alt="ct pic" src="https://github.com/user-attachments/assets/5f6a6435-21bb-4315-ae60-fdf6b0b4f80d" /> | How the ct clamp is mounted


---

## Home Assistant Dashboard

- Real-time power usage (per circuit & total)
- Historical energy consumption trends
- Daily and monthly cost calculation
- Customizable Lovelace cards

Overview            |  Details 
:-------------------------:|:-------------------------:
<img width="550" height="577" alt="Screenshot 2025-07-31 at 12 41 21 PM" src="https://github.com/user-attachments/assets/4ebe25a0-4866-4e89-b6a1-ee96ceabe362" /> | <img width="550" height="470" alt="Screenshot 2025-07-31 at 12 41 40 PM" src="https://github.com/user-attachments/assets/21925114-8e71-47ad-a3fd-6bebe3970412" />

So from the details of the phase 1 I can see that my fridge is constanly running with cyle of the compressor running and i am able to know roughly electricity used by it.

---

