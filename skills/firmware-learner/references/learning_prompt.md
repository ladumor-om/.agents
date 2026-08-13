# MOT Firmware Learning — Master Context

> This document provides full project context for the firmware learning skill. Read this when starting a new firmware learning session.

---

## What is this project?

**MOT (Modular Operation Theatre)** — A multi-controller embedded system for hospital operation theatres, controlling lights, curtains, gas monitoring, climate, RGB LEDs, and surgeon displays.

### Controllers

| Controller | MCU | Role | Communication |
|---|---|---|---|
| **Jetson Nano** | ARM (Linux) | Central hub — Python/Flask backend, HTML/JS frontend | WiFi, UART, USB |
| **Main PCB** | STM32F407VGTx | FreeRTOS app — lights, curtain, gas, climate control | UART to Jetson |
| **Pressure Sensor Board** | STM8S003F3P + ELVH-F50D | Reads differential pressure via I2C, outputs PWM | PWM → 0-10V → STM32 ADC |
| **Temp/Humidity Board** | STM8S003F3P + SHT40 | Reads temp/humidity via I2C, outputs PWM | PWM → 0-10V → STM32 ADC |
| **Surgeon Display** | STM32 + LCD | Shows OT parameters to surgeon | RS-485 from Jetson |
| **RGB Modbus Slave** | ESP32 | Controls RGB LEDs via Modbus | RS-485 from Jetson |
| **RGB TCP Slave** | ESP32 | Controls RGB LEDs via TCP | WiFi from Jetson |
| **RGB DMX Slave** | ESP32 | Controls RGB LEDs via DMX | DMX from Jetson |
| **Bootloader** | STM32F407VGTx | OTA firmware update (learning/testing only) | UART5 from Jetson |

### STM8-to-STM32 Signal Path

```
┌───────────────────────────┐                        ┌──────────────────────────────────┐
│   STM8 Sensor Board       │   0–10 V analog signal │   STM32 Main PCB                 │
│                           │ ──────────────────────► │                                  │
│  Sensor ──I2C──► STM8 MCU │    (PWM smoothed to    │  Voltage Divider (10V → 3.3V)    │
│           ↓               │     analog by external │       ↓                          │
│  STM8 generates PWM       │     RC low-pass filter │  ADC Pin (12-bit, 0–3.3V input)  │
│  representing 0–10V range │     or DAC circuit)    │       ↓                          │
│                           │                        │  Digital value 0–4095            │
└───────────────────────────┘                        └──────────────────────────────────┘
```

### Complete Wiring Table

| Connection | From | To | Signal | Hardware |
|---|---|---|---|---|
| Sensor → STM8 | ELVH-F50D / SHT40 | STM8 I2C pins | I2C bus | 4.7 kΩ pull-ups |
| STM8 → STM32 | STM8 PWM output | STM32 ADC input | PWM → 0-10V | RC filter + voltage divider |
| Jetson → STM32 | Jetson GPIO 8/10 | STM32 UART | UART 3.3V | Direct or level shifter |
| Jetson → STM32 OTA | Jetson UART | STM32 UART5 | UART 9600 baud | Direct |
| Jetson → Surgeon Disp | Jetson USB | STM32 UART | RS-485 | USB-to-RS-485 dongle |
| Jetson → RGB Slaves | Jetson USB | ESP32 UART | RS-485 Modbus | USB-to-RS-485 dongle |
| Jetson → RGB TCP | WiFi | ESP32 WiFi | TCP socket | No converter |

---

## Where is everything?

| What | Path |
|---|---|
| All learning documentation | `Learning-Docs/` |
| Learning roadmap & status | `Learning-Docs/README.md` |
| System architecture | `Learning-Docs/Architecture/` |
| Per-firmware guides | `Learning-Docs/Firmware/<REPO>/` |
| MCU references | `Learning-Docs/Controllers/` |
| Protocol references | `Learning-Docs/Protocols/` |
| Embedded concepts | `Learning-Docs/Fundamentals/` |
| Library references | `Learning-Docs/Libraries/` |
| Firmware source code | `MIPL/BitBucket/MOT/<REPO>/` |
| This skill | `.agents/skills/firmware-learner/` |
| Workflow | `.agents/workflows/learn-firmware.md` |

---

## Learning Order

| # | Firmware | MCU | Controller Doc First? | Key Protocols | Notes |
|---|---|---|---|---|---|
| 1 | MOT-ELVH-STM8 | STM8S003F3P | Yes → STM8.md | I2C, PWM, UART | Simplest — start here |
| 2 | MOT-SHT40-STM8 | STM8S003F3P | No | I2C, PWM, UART | Similar to #1 |
| 3 | MOT-CONTROLLER-CARD | STM32F407VGTx | Yes → STM32.md | UART, ADC, PWM | ✅ Already done |
| 4 | MOT-SURGEON-DISP | STM32 | No | UART, RS-485 | Display + serial |
| 5 | MOT-RGB-SLAVE | ESP32 | Yes → ESP32.md | Modbus, RS-485 | Arduino/ESP-IDF |
| 6 | MOT-RGB-TCP | ESP32 | No | TCP, WiFi | Same MCU, different protocol |
| 7 | MOT-RGB-DMX | ESP32 | No | DMX | Same MCU, different protocol |
| 8 | XRAY-VIEWER-STM8 | STM8 | No | GPIO, Relay | Simple I/O |
| 9 | MOT-STM32-BOOTLOADER | STM32F407VGTx | No | UART, OTA | Learning/testing only |

> **Excluded**: MOT-PROCESSOR-CARD, Udaipur Showroom (already known by user)
