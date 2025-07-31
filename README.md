# energy_monitoring
An energy monitoring solution for small home

# Home Energy Monitoring System with ESP8266 & Home Assistant

A DIY IoT project to monitor and analyze household electricity consumption in real-time using **ESP8266**, **PZEM-004T energy meters**, and **Home Assistant**. This system provides detailed energy usage data, historical trends, and cost calculations based on local electricity tariffs.

---

## Overview

I developed this project to track my home's power consumption across multiple circuits. It uses three PZEM-004T modules connected via I²C to an ESP8266 running Tasmota firmware. Data is published over MQTT to Home Assistant, where dashboards display real-time usage, daily costs, and historical trends.

**Key objectives:**
- Real-time monitoring of power usage across three circuits.
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
- **ESP8266 (NodeMCU)**
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

1. Connect each PZEM-004T to the ESP8266 using I²C with unique addresses.
2. Flash Tasmota firmware and configure I²C addresses in Tasmota console.
3. Set up MQTT topics for each sensor.
4. Integrate into Home Assistant via MQTT discovery or manual YAML configuration.

*(Insert wiring diagram and screenshots here)*

---

## Home Assistant Dashboard

- Real-time power usage (per circuit & total)
- Historical energy consumption trends
- Daily and monthly cost calculation
- Customizable Lovelace cards

*(Insert screenshots of dashboards here)*

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
