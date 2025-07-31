# energy_monitoring
A DIY IoT project to monitor and analyze household electricity consumption in real-time using **ESP8266 (flash with Tasmota)**, **PZEM-004T energy meters**, and **Home Assistant**. This system provides detailed energy usage data, historical trends, and cost calculations based on local electricity tariffs.

---

## Overview
The reason for the creation of this project is that my mom have been complaning about the electricity being expensive since i move in and i have been frustrated about it since, as I have no idea what's using the electricity and how to cut the usage, so I decided on making myself a cheap enery monitoring system to know what's using the electricity. That is why i have gone the cheapest route i am comfortable with.
It uses three PZEM-004T modules connected via I²C to an ESP8266 running Tasmota firmware. Data is published over MQTT to Home Assistant, where dashboards display real-time usage, daily costs, and historical trends. 

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

### Software
- Tasmota firmware on ESP8266
- MQTT broker (e.g., Mosquitto)
- Home Assistant for visualization and automation
- YAML configuration for sensor integration and cost calculation

---

## Wiring & Setup

1. Flash Tasmota firmware on the ESP8266
2. Connect each PZEM-004T to the ESP8266 using I²C and configure it with unique addresses
3. Connect all PZEM-004T to the ESP8266 and verify that all is working
4. Set up MQTT topics for each sensor.
5. Integrate into Home Assistant via MQTT.

*(Insert wiring diagram and screenshots here)*

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




## Example Cost Calculation

```yaml
# Example HA template sensor
- platform: template
  sensors:
    daily_energy_cost:
      friendly_name: "Daily Energy Cost"
      unit_of_measurement: 'RM'
      value_template: "{{ (states('sensor.daily_energy_kwh')|float * 0.218) | round(2) }}"
