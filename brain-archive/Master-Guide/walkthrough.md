# Walkthrough — MOT Firmware Learning Setup

## Phase 1: Documentation Architecture

Created **37 files** inside `MOT/Docs/`:

| Folder | Files | Purpose |
|---|---|---|
| `Architecture/` | 1 | System diagram, wiring, converters, data flow |
| `Firmware/` (×9 repos) | 18 | FIRMWARE_GUIDE.md + REAL_TIME_EXAMPLES.md per repo |
| `Controllers/` | 3 | STM8.md, STM32.md, ESP32.md |
| `Protocols/` | 7 | I2C, UART, SPI, PWM_Signal, Modbus, RS485, TCP_WiFi |
| `Fundamentals/` | 8 | Serial Basics, PWM, ADC, Polling/Int/DMA, Timers, Clock, Memory, GPIO |

Key documents: [README.md](file:///c:/Users/oml/Documents/BitBucket/MOT/Docs/README.md) (learning roadmap) and [MOT_System_Architecture.md](file:///c:/Users/oml/Documents/BitBucket/MOT/Docs/Architecture/MOT_System_Architecture.md) (full system overview).

---

## Phase 2: Cross-Chat Workflow

Created two files so future chats can automatically follow the learning process:

### 1. [learn-firmware.md](file:///c:/Users/oml/Documents/BitBucket/MOT/.agents/workflows/learn-firmware.md) — Antigravity Workflow

Location: `MOT/.agents/workflows/learn-firmware.md`

Triggered via `/learn-firmware` slash command. Contains 6 phases:
- Phase 0: Context Loading (read roadmap, architecture, existing docs)
- Phase 1: Controller Document (if first firmware for that MCU)
- Phase 2: Firmware Analysis (read source code)
- Phase 3: Create FIRMWARE_GUIDE.md
- Phase 4: Create REAL_TIME_EXAMPLES.md
- Phase 5: Update shared docs (protocols, fundamentals, controllers)
- Phase 6: Update status in README.md

### 2. [learning_prompt.md](file:///C:/Users/oml/.gemini/antigravity/brain/ab6a6844-3f91-4898-900b-d180ce04e82d/learning_prompt.md) — Brain Context

Location: This chat's brain directory. Read when this chat is referenced via `@`.

Contains: project overview, file locations, step-by-step instructions, style rules, and learning order.

---

## How to Start a New Firmware Learning Session

Open a new chat and send:

```
@[this chat] /learn-firmware

Firmware: MOT-ELVH-STM8
Reference: @[optional_file.md]
```

The model will:
1. Read brain `learning_prompt.md` → understand full project context
2. Follow `/learn-firmware` workflow → step-by-step process
3. Read `Docs/README.md` → check what's done, what's next
4. Read existing docs → match style and avoid duplication
5. Analyze firmware code → create documentation
6. Update all relevant files → firmware guide, examples, protocols, fundamentals
