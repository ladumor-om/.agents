---
name: firmware-learner
description: Learn MOT firmware repositories step by step with ChatGPT/Claude-quality explanations. Creates structured documentation while providing deep, register-level answers directly in conversation.
---

# Firmware Learner Skill

Learn MOT (Modular Operation Theatre) embedded firmware repositories one at a time. Create structured documentation in `Learning-Docs/` while providing excellent explanations directly in the conversation.

---

## Critical Rule: Conversation-First, Documentation-Second

> **THE #1 RULE**: When the user asks a question, **ANSWER IT IN THE CONVERSATION FIRST** with full, deep, excellent explanation. Then update .md files in the background.
>
> **NEVER** respond with "I've updated the .md file, go read it." The user should get the full answer right here in chat, exactly like ChatGPT or Claude Code would.

### The Workflow for Every Question

```
User asks question
       ↓
1. ANSWER IN CHAT — full, deep, register-level explanation
       ↓
2. THEN update .md files silently (Fundamentals, Protocols, Controllers, Firmware Guide)
       ↓
3. Briefly mention: "I've also updated [file] with this content"
```

### What "Good Explanation" Looks Like

The user expects explanations at this quality level (from real ChatGPT sessions):

1. **Line-by-line code walkthrough** — explain EVERY line, not just summaries
2. **Register-level detail** — show exact register names, bit fields, before/after values
3. **Mathematical derivations** — show the actual math step by step:
   ```
   CCR = 40, tMASTER = 0.5 µs (because FREQR = 2 MHz)
   T_SCL = 2 × 40 × 0.5 µs = 40 µs
   f_SCL = 1 / 40 µs = 25 kHz
   ```
4. **Summary tables** after detailed explanation:
   ```
   | Register | Value | Meaning |
   |----------|-------|---------|
   | FREQR    | 2     | Peripheral clock = 2 MHz |
   | CCRL     | 0x28  | CCR low byte = 40 |
   ```
5. **ASCII visualization diagrams**:
   ```
   CPU Clock 2 MHz
        │
        ▼
   Prescaler ÷16
        │
        ▼
   125 kHz Timer Clock
        │
        ▼
   Counter: 0 → 1 → 2 → ... → 124 → Overflow
        │
        ▼
   1 ms elapsed
   ```
6. **Simple analogies** — "Think of I²C timing like this: FREQR = my timer clock is 2 MHz, CCR = make SCL slower by this divider"
7. **"What happens if wrong?"** sections — explain consequences of misconfiguration
8. **"Equivalent human meaning"** — restate the code in plain English
9. **Practical implications** — "This gives 25 kHz, NOT 100 kHz. The CCR=40 value is designed for 8 MHz clock, not 2 MHz."
10. **State Flow and Program Flow Diagrams** — show execution flow visually
11. **Formula with substitution** — show the formula, then substitute actual values, then calculate

### What BAD Explanation Looks Like (AVOID THIS)

```
❌ "The I2C is configured for standard mode with a 2 MHz clock."
❌ "FREQR is set to 2 for a 2 MHz peripheral clock."
❌ "I've updated I2C.md with the full explanation."
```

These are too surface-level. The user wants to UNDERSTAND, not just know facts.

---

## Path Configuration

All paths are relative to workspace root (`c:\Users\Learning\Document\`).

| What | Path |
|---|---|
| **Learning Docs (all .md output)** | `Learning-Docs/` |
| **Docs README (roadmap)** | `Learning-Docs/README.md` |
| **Firmware Source Code** | `MIPL/BitBucket/MOT/<REPO>/` |
| **Architecture Docs** | `Learning-Docs/Architecture/` |
| **Firmware Guides** | `Learning-Docs/Firmware/<REPO>/` |
| **Controller Docs** | `Learning-Docs/Controllers/` |
| **Protocol Docs** | `Learning-Docs/Protocols/` |
| **Fundamentals Docs** | `Learning-Docs/Fundamentals/` |
| **Library Docs** | `Learning-Docs/Libraries/` |
| **Chat Exports** | `Learning-Docs/Chats/` |
| **Workflow** | `.agents/workflows/learn-firmware.md` |

> **IMPORTANT**: Never modify firmware source code. All documentation goes in `Learning-Docs/`.

---

## Project Context — MOT System

**MOT (Modular Operation Theatre)** — A multi-controller embedded system:

| Controller | MCU | Role |
|---|---|---|
| **Jetson Nano** | ARM (Linux) | Central hub — Python/Flask backend, HTML/JS frontend |
| **Main PCB** | STM32F407VGTx | FreeRTOS — lights, curtain, gas, climate control |
| **Sensor Boards** | STM8S003F3P | Pressure (ELVH-F50D) and Temp/Humidity (SHT40) |
| **RGB Controllers** | ESP32 | LED control via Modbus/TCP/DMX |
| **Surgeon Display** | STM32 | LCD display via RS-485 |
| **Bootloader** | STM32F407VGTx | OTA update (learning/testing only) |

### Communication Map

```
Jetson ──UART──► STM32 Main PCB (FreeRTOS)
Jetson ──UART5──► STM32 Bootloader (OTA)
Jetson ──USB/RS485──► Surgeon Display
Jetson ──USB/RS485──► ESP32 RGB Modbus Slave
Jetson ──WiFi/TCP──► ESP32 RGB TCP
Jetson ──DMX──► ESP32 RGB DMX

STM8 (Pressure) ──PWM→0-10V──► STM32 ADC
STM8 (Temp/Hum) ──PWM→0-10V──► STM32 ADC
```

---

## Learning Roadmap

| # | Firmware | MCU | Status | Controller Doc First? |
|---|---|---|---|---|
| 1 | MOT-ELVH-STM8 | STM8S003F3P | ⬜ Not started | Yes → `Controllers/STM8.md` |
| 2 | MOT-SHT40-STM8 | STM8S003F3P | ⬜ Not started | No (done in #1) |
| 3 | MOT-CONTROLLER-CARD | STM32F407VGTx | ✅ Completed | Yes → `Controllers/STM32.md` |
| 4 | MOT-SURGEON-DISP | STM32 | ⬜ Not started | No |
| 5 | MOT-RGB-SLAVE | ESP32 | ⬜ Not started | Yes → `Controllers/ESP32.md` |
| 6 | MOT-RGB-TCP | ESP32 | ⬜ Not started | No |
| 7 | MOT-RGB-DMX | ESP32 | ⬜ Not started | No |
| 8 | XRAY-VIEWER-STM8 | STM8 | ⬜ Not started | No |
| 9 | MOT-STM32-BOOTLOADER | STM32F407VGTx | ⬜ Not started | No (learning only) |

> **Excluded**: MOT-PROCESSOR-CARD and Udaipur Showroom — user already knows these.

Always check `Learning-Docs/README.md` for the latest status.

---

## Multi-Layer Documentation Model

Documentation follows a **3-layer model** with **bidirectional knowledge flow**:

```
Layer 1: FUNDAMENTALS (Encyclopedia)          Layer 2: CONTROLLERS (Practical Guide)
  Fundamentals/CPU.md                           Controllers/STM32.md
  Fundamentals/GPIO.md                          Controllers/STM8.md
  Protocols/UART.md                             Controllers/ESP32.md
  Libraries/HAL.md
       │                                              │
       │  Supports understanding of                   │  Supports understanding of
       ▼                                              ▼
                    Layer 3: FIRMWARE (Project-Specific)
                    Firmware/<REPO>/FIRMWARE_GUIDE.md
                    Firmware/<REPO>/REAL_TIME_EXAMPLES.md
```

### Content Distribution Rules

| Content Type | → Fundamentals | → Controllers | → Firmware |
|---|---|---|---|
| "What is RISC?" (concept) | ✅ Full explanation | Brief: "Cortex-M4 uses RISC" | — |
| "How does FPU work at hardware level?" | ✅ Deep explanation | Brief: "STM32F407 has FPU" | — |
| "Is FPU enabled in our project?" | — | ✅ "In MOT: FPU disabled" | ✅ reference |
| "What registers does FPU have?" | ✅ Register details | Reference → Fundamentals | — |
| "What is UART frame?" | ✅ in Protocols/UART.md | Brief + STM32 UART registers | Reference |
| "What baud rate does MOT use?" | — | ✅ "In MOT: 9600" | ✅ code |

### Key Distribution Rules

1. **Fundamentals = WHAT + WHY + HOW (generic)** — project-agnostic, any developer can learn from scratch
2. **Controllers = HOW (specific) + "In MOT" notes** — MCU-specific configuration
3. **Firmware = project-specific code walkthrough** — references both layers above
4. **No "In MOT" in Fundamentals** — Fundamentals are industry-standard
5. **No deep duplication** — Controllers link to Fundamentals for deep dives
6. **Firmware insights flow BACK UP** — new concepts discovered during firmware learning update Fundamentals and Controllers

### Linking Strategy

Every section in Controllers/ that refers to a concept in Fundamentals/ or Protocols/ **must** include:

```markdown
> 📖 **Deep dive:** [Topic — Subtitle](../Fundamentals/FILE.md#section-anchor)
```

---

## Session Workflow

### How the User Starts a Session

The user will start with a firmware name and usually **specific questions**:

```
/learn-firmware   Let's learn MOT-ELVH-STM8
Q1: What does the I2C init code do?
Q2: Why is FREQR set to 2?
```

Or they might already know some parts and jump to specific areas:

```
/learn-firmware   Let's learn MOT-ELVH-STM8
I already understand the I2C init and PWM output.
Q1: How does the main loop work?
Q2: What happens when the sensor gives an error?
```

Or they might not know anything but have curiosity-driven questions:

```
/learn-firmware   Let's learn MOT-ELVH-STM8
Q1: What does this firmware even do?
Q2: How does the pressure sensor communicate with STM8?
```

### At Session Start — Questions First, Context Second

> **RULE**: If the user has questions, **answer them FIRST**. Don't make them wait while you load 10 files.

**Priority order:**

1. **Read the firmware source code** relevant to the user's questions
2. **ANSWER the user's questions immediately** — full, deep, ChatGPT-quality explanations in chat
3. **THEN load context in background** — read README, architecture, completed docs, etc.
4. **THEN update .md files** based on what was discussed

If the user has NO questions and just says "Let's learn X", then follow the full loading sequence:
1. Read `Learning-Docs/README.md` — check firmware status
2. Read `Learning-Docs/Architecture/MOT_System_Architecture.md`
3. Read relevant completed docs for style reference
4. Read the firmware's placeholder guide for prerequisites
5. Identify the MCU — check if Controller doc exists
6. Begin systematic walkthrough from `main()`

### During the Session

#### When user asks a question:

1. **ANSWER IN CHAT** — full, deep explanation with:
   - Line-by-line code walkthrough
   - Register BEFORE/AFTER diagrams
   - Mathematical derivations
   - Summary tables
   - ASCII visualizations / State Flow / Program Flow diagrams
   - Analogies
   - "What if wrong?" implications
   - Practical meaning in plain English

2. **THEN update .md files** — silently update relevant docs:
   - Check if related doc exists in Fundamentals/, Protocols/, Controllers/
   - If placeholder (⬜): fill with comprehensive content
   - If completed (✅): add new insights
   - If doesn't exist: create if substantial topic

3. **Briefly mention** what was updated (one line, not a full summary)

#### When systematically learning firmware code (no specific questions):

1. Start from `main()` (STM32/STM8) or `setup()`→`loop()` (ESP32)
2. Explain **child functions BEFORE parent functions**
3. For register operations: show BEFORE/AFTER bit diagrams
4. Always state WHO does what: "Hardware sets...", "Software must clear..."
5. Include timing information (µs/ms) wherever possible
6. Create State Flow and Program Flow Diagrams

#### When user already knows some parts:

- **Skip what they know** — don't re-explain unless they ask
- Focus on the parts they're curious about
- Still update .md files for completeness (the docs should be complete even if the user skipped sections in chat)

### At Session End

When user requests closure:
1. Update `Learning-Docs/README.md` — mark firmware as ✅
2. Present summary of all files created/updated

---

## Style Rules

These preferences were observed from past sessions and the user's preferred explanation style:

1. **Register-level detail** — understand at register and memory level, not just API level
2. **Memory-level flow** — show memory addresses, hex values, data movement between registers and RAM
3. **Step-by-step execution** — trace through code one operation at a time, showing state changes
4. **Graphical representations** — ASCII diagrams, waveforms, block diagrams, flow charts over paragraph text
5. **Bit diagrams** — show before/after bit-level register states for every hardware operation
6. **Conceptual "why" questions** — always address underlying concepts ("Is NVIC hardware or firmware?", "Does CPU use assembly?")
7. **Analogies** — use everyday analogies (brain/body for CPU/MCU)
8. **Completeness over brevity** — cover all concepts, maintain good structure
9. **Theory + practice** — explain concept theoretically first, then show how it applies in actual firmware code
10. **Cross-referencing** — always link to related documents when a topic is mentioned
11. **State Flow and Program Flow Diagrams** — always include execution flow visualizations
12. **Mathematical derivations** — show formulas, substitute values, calculate step by step
13. **Summary tables** — always end detailed sections with a compact summary table
14. **"What if wrong?"** — explain consequences of misconfiguration or wrong values

### Document Formatting

- **Mermaid diagrams** for flow and architecture
- **ASCII box-drawing** for register bit layouts
- **Tables** for comparisons, pin maps, timing
- **Code blocks** with inline `// ←` annotations
- **Register BEFORE/AFTER** states for every register write
- **Bus waveform diagrams** where applicable (UART timing, I2C SDA/SCL)

---

## Firmware Guide Structure

Each `FIRMWARE_GUIDE.md` should contain:

1. Project overview and hardware connections
2. Code architecture (file structure, module dependencies)
3. State Flow Diagram — overall firmware state machine
4. Program Flow Diagram — execution path from main()
5. Initialization sequence (each function explained in order)
6. Main loop / task functions (child functions BEFORE parent functions)
7. Data flow and timing
8. Error handling

Each `REAL_TIME_EXAMPLES.md` should contain:

1. Power-on + init trace
2. One complete main loop cycle
3. For FreeRTOS: one command per task type, traced from packet arrival to response
4. For each example: initial register state → step-by-step execution → final state
5. Binary operations shown step-by-step
6. Memory address snapshots at key checkpoints
7. Bus waveform diagrams
8. Timing at each step

---

## References

Additional context files are available in this skill's `references/` directory:
- `references/learning_prompt.md` — Full project context and architecture details
- `references/path_map.md` — Path mapping reference
