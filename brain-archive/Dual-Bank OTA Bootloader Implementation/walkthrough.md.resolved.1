# Walkthrough — Dual-Bank OTA Bootloader Implementation

## What Was Built

A complete dual-bank (A/B) OTA system allowing the Jetson Nano to remotely flash new firmware onto the STM32F407 over the existing UART5 connection (9600 baud).

---

## Files Created / Modified

### Bootloader Project (`MOT-STM32-BOOTLOADER/OT_Bootloader/`)

| File | Purpose |
|------|---------|
| [boot_config.h](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Inc/boot_config.h) | Flash layout constants, `BootConfig_t` struct, sector lookup |
| [flash_ops.h](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Inc/flash_ops.h) | Flash operation function declarations |
| [flash_ops.c](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Src/flash_ops.c) | Config read/write, bank erase, flash write, CRC32 |
| [ota_protocol.h](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Inc/ota_protocol.h) | OTA protocol constants and packet structures |
| [ota_protocol.c](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Src/ota_protocol.c) | Packet reception, CRC8, 3 retries, 5s timeout, chunk writing |
| [main.c](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/Core/Src/main.c) | Bootloader main: OTA flag check → OTA mode or jump to app |
| [STM32F407VGTX_FLASH.ld](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-STM32-BOOTLOADER/OT_Bootloader/STM32F407VGTX_FLASH.ld) | Linker restricted to 16 KB (Sector 0) |

### App Modifications (`MOT-CONTROLLER-CARD/OT_RTOS/`)

| File | Change |
|------|--------|
| [main.c](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-CONTROLLER-CARD/OT_RTOS/Core/Src/main.c) | Added `TASKCODE_OTA_START (0x71)`, `BootConfig_t`, `flash_set_ota_flag()`, OTA command handler |
| [STM32F407VGTX_FLASH.ld](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-CONTROLLER-CARD/OT_RTOS/STM32F407VGTX_FLASH.ld) | Flash origin → `0x08004000`, length → 496K |
| [system_stm32f4xx.c](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-CONTROLLER-CARD/OT_RTOS/Core/Src/system_stm32f4xx.c) | Enabled `USER_VECT_TAB_ADDRESS`, VTOR offset → `0x4000` |

### Nano Script (`MOT-PROCESSOR-CARD/modules/`)

| File | Purpose |
|------|---------|
| [ota_updater.py](file:///c:/Users/oml/Documents/BitBucket/MOT/MOT-PROCESSOR-CARD/modules/ota_updater.py) | Python OTA client: sends firmware, handles retries, shows progress |

---

## How to Build & Flash (First Time)

### 1. Build the Bootloader
Open `OT_Bootloader` in STM32CubeIDE → Build (Ctrl+B) → verify output < 16 KB.

### 2. Build the App
Open `OT_RTOS` in STM32CubeIDE → Build → generates `OT_RTOS.bin` at Bank A (`0x08004000`).

### 3. Flash via ST-Link (first time only)
```bash
# Flash bootloader at 0x08000000
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg \
  -c "program OT_Bootloader.elf verify reset exit"

# Flash app at Bank A (0x08004000)
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg \
  -c "program OT_RTOS.bin 0x08004000 verify reset exit"
```

### 4. Subsequent OTA Updates (from Nano)
```bash
python3 modules/ota_updater.py /path/to/new_firmware.bin --port /dev/ttyTHS1
```

---

## OTA Flow Summary

```
Nano: python3 ota_updater.py firmware.bin
  │
  ├─ Sends [0x2B, 0x71, 0x3B] to running app
  ├─ App sets OTA flag → reboots
  ├─ Bootloader boots → sees OTA flag
  ├─ Bootloader sends ACK_READY
  ├─ Nano sends OTA_INFO (size + CRC32)
  ├─ Bootloader erases target bank → ACK_OK
  ├─ Nano sends 128-byte chunks with CRC8
  │   ├─ ACK_OK → next chunk
  │   └─ ACK_RESEND → retry (up to 3x)
  ├─ Nano sends OTA_END
  ├─ Bootloader verifies full CRC32
  ├─ ACK_SUCCESS → switches active bank
  └─ Bootloader re-enters → jumps to new app ✅
```
