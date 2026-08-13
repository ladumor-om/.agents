# MOT Firmware Learning — Master Context

> This document is the **bridge between chats**. When a new chat references this one, read this file to understand the full project context and what to do next.

---

## What is this project?

**MOT (Modular Operation Theatre)** — A multi-controller embedded system with:
- **Jetson Nano** — Central hub (Python/Flask backend, HTML/JS frontend)
- **STM32F407VGTx** — Main PCB controller (FreeRTOS, lights/curtain/gas/climate)
- **STM8S003F3P** — Sensor boards (pressure via ELVH-F50D, temp/humidity via SHT40)
- **ESP32** — RGB LED controllers (Modbus/TCP/DMX variants)
- **STM32** — Surgeon display, OTA bootloader (learning only)

## Where is everything?

| Location | What |
|---|---|
| `MOT/Docs/` | All learning documentation (DO NOT modify firmware repos) |
| `MOT/Docs/README.md` | Learning roadmap — check firmware status here |
| `MOT/Docs/Architecture/` | System architecture, wiring, converters |
| `MOT/Docs/Firmware/<REPO>/` | Per-firmware: FIRMWARE_GUIDE.md + REAL_TIME_EXAMPLES.md |
| `MOT/Docs/Controllers/` | MCU references (STM8.md, STM32.md, ESP32.md) |
| `MOT/Docs/Protocols/` | Protocol references (I2C, UART, Modbus, RS485, etc.) |
| `MOT/Docs/Fundamentals/` | Embedded concepts (PWM, ADC, polling/interrupt, etc.) |
| `MOT/.agents/workflows/learn-firmware.md` | Step-by-step workflow for learning any firmware |

## What to do when this chat is referenced

The user will provide a prompt like:
```
@[this chat] Let's learn MOT-ELVH-STM8. Reference: @[some_file.md]
```

### Your steps:

1. **Read the workflow**: `MOT/.agents/workflows/learn-firmware.md` — follow it step by step
2. **Read the roadmap**: `MOT/Docs/README.md` — check which firmware is next and what's done
3. **Read the architecture**: `MOT/Docs/Architecture/MOT_System_Architecture.md`
4. **Read existing docs**: Check completed firmware guides, controllers, protocols, fundamentals — match their style
5. **Read the firmware code**: The actual source files in the firmware repository
6. **Create/update documentation**: Write FIRMWARE_GUIDE.md, REAL_TIME_EXAMPLES.md, and update shared docs
7. **Update status**: Mark firmware as complete in README.md

### Key rules:
- **Start from `main()`** (STM32/STM8) or **`setup()`→`loop()`** (ESP32)
- **Create controller doc FIRST** if this is the first firmware for that MCU
- **Child functions before parent functions** in explanations
- **Register BEFORE/AFTER diagrams** for every register write
- **Mermaid diagrams** for flow and architecture
- **New concepts → update Fundamentals/** (don't duplicate across firmware guides)
- **New protocol insights → update Protocols/** (reference from firmware guide)
- **NEVER modify firmware source code** — all docs go in `MOT/Docs/`

## Learning order

| # | Firmware | MCU | Status (check README.md for latest) |
|---|---|---|---|
| 1 | MOT-ELVH-STM8 | STM8 | Check README.md |
| 2 | MOT-SHT40-STM8 | STM8 | Check README.md |
| 3 | MOT-CONTROLLER-CARD | STM32 | Check README.md |
| 4 | MOT-SURGEON-DISP | STM32 | Check README.md |
| 5 | MOT-RGB-SLAVE | ESP32 | Check README.md |
| 6 | MOT-RGB-TCP | ESP32 | Check README.md |
| 7 | MOT-RGB-DMX | ESP32 | Check README.md |
| 8 | XRAY-VIEWER-STM8 | STM8 | Check README.md |
| 9 | MOT-STM32-BOOTLOADER | STM32 | Learning/testing only |
