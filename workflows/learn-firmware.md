---
description: How to learn a MOT firmware repository step by step and update documentation in Learning-Docs/
---

# Learn Firmware Workflow

This workflow is used to learn a MOT firmware repository step by step, starting from `main()` (STM32/STM8) or `setup()`→`loop()` (ESP32), and create/update documentation in `Learning-Docs/`.

> **IMPORTANT**: Before following this workflow, read the skill file at `.agents/skills/firmware-learner/SKILL.md` for full context, path mappings, explanation quality rules, and the conversation-first approach.

---

## Goal

The primary goals during a firmware learning session:

1. **Answer questions directly in conversation** — Provide full, deep, register-level explanations in the chat FIRST. Update .md files in the background. NEVER respond with just "I've updated the .md file"
2. **Learn and document firmware** — Create FIRMWARE_GUIDE.md and REAL_TIME_EXAMPLES.md for the target firmware
3. **Maintain documentation structure** — Keep the folder layout clean, scalable, and easy to navigate. If the structure is insufficient or incorrect, propose adjustments
4. **Create new files for new topics** — When a new concept or theory is encountered (e.g., CPU architecture, DMA internals), create a dedicated file for it
5. **Update Fundamentals and Protocols** — After answering a user query, check if the related documentation file needs updating. Include industry-level content, real-world examples, and conceptual understanding beyond the current project
6. **Continuous documentation during learning** — Every question asked and answered should be reflected in the documentation. If the related file exists but is incomplete (⬜), update it. If no file exists but the topic is small, add it to the relevant parent file. If the topic deserves its own file, create one
7. **Proactive learning** — Go beyond user queries. Identify topics that are essential for an embedded software engineer and proactively add explanations
8. **Cross-reference everything** — Always link to related fundamentals/protocol files. If the linked file is not yet updated, update it before linking

---

## Path Configuration

| What | Path |
|---|---|
| Learning Docs (all .md output) | `Learning-Docs/` |
| Docs README (roadmap) | `Learning-Docs/README.md` |
| Firmware Source Code | `MIPL/BitBucket/MOT/<REPO>/` |
| Architecture Docs | `Learning-Docs/Architecture/` |
| Firmware Guides | `Learning-Docs/Firmware/<REPO>/` |
| Controller Docs | `Learning-Docs/Controllers/` |
| Protocol Docs | `Learning-Docs/Protocols/` |
| Fundamentals Docs | `Learning-Docs/Fundamentals/` |
| Library Docs | `Learning-Docs/Libraries/` |
| Chat Exports | `Learning-Docs/Chats/` |
| Full path map | `.agents/skills/firmware-learner/references/path_map.md` |

---

## Rules

These rules apply to **every** firmware learning chat session:

1. **Conversation-first** — Answer in chat with full, deep explanation. Update .md files in the background. The user should NEVER need to go read an .md file to get an answer
2. **Read completed docs first** — At the start of every session, analyze all files marked ✅ Completed in `Learning-Docs/README.md`. Use them as reference for explanation style, depth, and structure
3. **Never modify firmware source code** — All documentation goes in `Learning-Docs/`
4. **Follow consistent style** — Match the explanation style established in completed docs (register-level, memory-level, graphical, step-by-step)
5. **Never leave referenced files as placeholders** — If a topic arises during learning and the related file (e.g., `ADC.md`, `UART.md`) is still a placeholder (⬜), update it with comprehensive content before referencing it
6. **Always create a plan first** — Before executing large documentation updates, create a detailed task list/plan and submit for review
7. **Verify with source** — Never rely solely on assumptions. Verify against actual code, datasheets, or authoritative web sources
8. **Multi-tasking approach** — When learning firmware, simultaneously identify and update all related files (Fundamentals, Protocols, Controllers, HAL) not just the firmware guide
9. **Child functions before parent functions** — Always explain callees before callers
10. **Start from entry point** — Begin from `main()` (STM32/STM8) or `setup()`→`loop()` (ESP32)
11. **Register BEFORE/AFTER** — Show register state before and after every register write operation
12. **State Flow and Program Flow Diagrams** — Always include visual execution flow diagrams
13. **Mathematical derivations** — Show formulas, substitute actual values, calculate step by step
14. **Summary tables** — End detailed explanations with a compact summary table

---

## Instructions

### How the User Starts

The user typically starts with a firmware name **plus specific questions**:

```
/learn-firmware   Let's learn MOT-ELVH-STM8
Q1: What does the I2C init code do?
Q2: Why is FREQR set to 2?
```

Or they may already know parts and skip ahead:

```
/learn-firmware   Let's learn MOT-ELVH-STM8
I already understand I2C init and PWM output.
Q1: How does the main loop work?
```

Or they may start with no questions (just "Let's learn X").

### At Session Start — Questions First, Context Second

> **RULE**: If the user has questions, **answer them FIRST**. Don't make them wait while you load 10 files.

**When user has questions:**
1. Read the skill file: `.agents/skills/firmware-learner/SKILL.md` for quality rules
2. Read the firmware source code relevant to the user's questions
3. **ANSWER the questions immediately** — full, deep, ChatGPT-quality explanations in chat
4. Then load remaining context in background (README, architecture, completed docs)
5. Then update .md files based on what was discussed

**When user has NO questions (just "Let's learn X"):**
1. Read the skill file: `.agents/skills/firmware-learner/SKILL.md` for full context and quality rules
2. Read `Learning-Docs/README.md` to check firmware status and learning order
3. Read this file (`learn-firmware.md`) for the workflow
4. Read all ✅ Completed firmware guides to understand established style
5. Read the firmware's placeholder `FIRMWARE_GUIDE.md` for prerequisites
6. Identify the MCU — check if `Learning-Docs/Controllers/<MCU>.md` exists
7. Begin systematic walkthrough from `main()`

### During the Session

1. When the user asks a question about a topic:
   - **ANSWER IN CHAT FIRST** — full, deep, register-level explanation with diagrams, math, analogies
   - Then check if a related documentation file exists (in Fundamentals/, Protocols/, Controllers/)
   - If the file exists but is ⬜ (placeholder): update it with comprehensive content
   - If the file exists and is ✅: check if the new information should be added
   - If no file exists: create one if the topic is substantial; otherwise add to the parent file
   - Briefly mention what files were updated (one line, not a summary)
2. When encountering a new protocol: update `Learning-Docs/Protocols/<PROTOCOL>.md`
3. When encountering a new fundamental concept: update `Learning-Docs/Fundamentals/<TOPIC>.md`
4. When learning something new about the MCU: update `Learning-Docs/Controllers/<MCU>.md`
5. Always add cross-reference links between documents

### When user already knows some parts

- **Skip what they know** — don't re-explain unless they ask
- Focus on the parts they're curious about
- Still update .md files for completeness (docs should be complete even if the user skipped sections in chat)

### At Session End

When the user requests closure:
1. Update `Learning-Docs/README.md` — mark firmware as ✅
2. Present a summary of all files created/updated to the user for review

---

## Documentation Structure

### Current Layout

```
Learning-Docs/
├── README.md                    ← Learning roadmap and status tracker
├── Chats/                       ← Exported chat logs
├── Architecture/                ← System-level documentation
├── Firmware/                    ← One folder per firmware repo
│   ├── <REPO>/FIRMWARE_GUIDE.md    ← Complete firmware walkthrough
│   └── <REPO>/REAL_TIME_EXAMPLES.md ← Step-by-step execution traces
├── Controllers/                 ← MCU reference docs (STM8.md, STM32.md, ESP32.md)
├── Protocols/                   ← Protocol references (I2C, UART, Modbus, etc.)
│   └── OPC_UA/                  ← Multi-doc protocols get their own subfolder
├── Fundamentals/                ← Embedded theory & concepts (ADC, PWM, GPIO, etc.)
└── Libraries/                   ← Library references (HAL.md, etc.)
```

### Structure Rules

- **Flexibility** — This structure is designed based on current requirements and may evolve with learning. It does not need to be followed rigidly every time
- **Fundamentals/** — Created for core foundational topics (ADC, PWM, GPIO, Clock, CPU, etc.). All such files go here
- **Protocols/** — Each communication protocol gets its own file. Complex protocols (like OPC UA) get a subfolder
- **Libraries/** — Documentation for software libraries used in firmware (HAL, FreeRTOS wrappers)

### When to Create New Files

- A new topic or theory is encountered during firmware learning → create a new `.md` file in the appropriate folder
- The topic does not fit any existing folder → create a new folder

### Structural Changes Procedure

If files or folders need to be moved or restructured:
1. Create a plan outlining the changes
2. Submit the plan for review
3. Implement only after approval

---

## Prerequisites

The user will provide:
- **Firmware name** (e.g., `MOT-ELVH-STM8`)
- **Reference files** (optional — exported chats, datasheets, etc.)

---

## Steps

// turbo-all

### Phase 0: Context Loading

1. Read the skill file: `.agents/skills/firmware-learner/SKILL.md`
2. Read `Learning-Docs/README.md` to understand the learning roadmap and check which firmware is next
3. Read `Learning-Docs/Architecture/MOT_System_Architecture.md` to understand the full system, wiring, and data flow
4. Read the firmware's existing placeholder: `Learning-Docs/Firmware/<REPO>/FIRMWARE_GUIDE.md` — check prerequisites listed there
5. Check the firmware status in `Learning-Docs/README.md` — if previous firmware are completed, read their guides to understand established style and patterns
6. Read any completed documents in `Learning-Docs/Controllers/`, `Learning-Docs/Protocols/`, `Learning-Docs/Fundamentals/` that are relevant to this firmware
7. Read any reference files provided by the user in the prompt

### Phase 1: Controller Document (if first firmware for this MCU)

8. Check `Learning-Docs/Firmware/<REPO>/FIRMWARE_GUIDE.md` — does it say "Controller Doc First? Yes"?
9. If YES: Create the full controller reference document (`Learning-Docs/Controllers/<MCU>.md`) BEFORE learning the firmware. Cover architecture, memory map, clock system, peripheral registers, development tools, boot process
10. If NO: The controller doc already exists from a previous firmware — read it for context

### Phase 2: Firmware Analysis

11. Read all source files in the firmware repository at `MIPL/BitBucket/MOT/<REPO>/` — understand file structure, dependencies, function hierarchy
12. Identify: entry point (`main()` or `setup()/loop()`), initialization sequence, main loop logic, communication protocols used, peripherals used
13. Read related upstream/downstream firmware to understand what data this firmware receives and sends

### Phase 3: Create FIRMWARE_GUIDE.md

14. Write the firmware guide following the code execution order, starting from `main()` / `setup()`:
    - Project overview and hardware connections
    - Code architecture (file structure, module dependencies)
    - State Flow Diagram — overall firmware state machine
    - Program Flow Diagram — execution path from main()
    - Initialization sequence (each function explained in order)
    - Main loop / task functions (child functions BEFORE parent functions)
    - Data flow and timing
    - Error handling
15. Include: mermaid flow diagrams, ASCII register diagrams, code with inline annotations, timing tables
16. When explaining functions, explain in LOGICAL order (child before parent), not alphabetical
17. For register operations: show BEFORE/AFTER bit diagrams, explain WHO sets/clears each bit (hardware vs software)

### Phase 4: Create REAL_TIME_EXAMPLES.md

18. Select key scenarios to trace (typically 2-5 per firmware):
    - For simple firmware: power-on + init, one complete main loop cycle
    - For complex firmware (FreeRTOS): one command per task type, traced from packet arrival to response
19. For each example, trace the COMPLETE execution showing:
    - Initial register/memory state with address and hex value
    - Step-by-step code execution with register BEFORE/AFTER bit diagrams
    - Binary operations shown step-by-step (value in binary, operation, result)
    - Memory address snapshots at key checkpoints
    - Bus waveform diagrams where applicable (UART timing, I2C SDA/SCL)
    - Hardware effects (pin waveforms, bus signals)
    - Timing at each step
    - Final state summary

### Phase 5: Update Shared Documents

20. **Mandatory check**: Scan ALL files in `Learning-Docs/Fundamentals/` and `Learning-Docs/Protocols/` — update every file that relates to concepts used in this firmware
21. If a new protocol was encountered: create or update the relevant `Learning-Docs/Protocols/<PROTOCOL>.md`
22. If a new fundamental concept was learned: create or update the relevant `Learning-Docs/Fundamentals/<TOPIC>.md`
23. If new insights about the controller were gained: update `Learning-Docs/Controllers/<MCU>.md`
24. Cross-reference: add links from firmware guide to protocol/controller/fundamentals docs, and vice versa
25. If HAL functions were used: update `Learning-Docs/Libraries/HAL.md` with new HAL function documentation

### Fundamentals & Protocols Documentation Rules

When updating files in `Learning-Docs/Fundamentals/` and `Learning-Docs/Protocols/`:

- **Not project-limited** — Content must not be limited to the MOT project. These are foundational resources for any developer
- **Industry-level** — Include real-world use cases, practical applications, and industry-standard explanations
- **Conceptual understanding** — Focus on understanding, not just code snippets
- **Complete coverage** — Cover all aspects a developer needs to know about the topic

**Protocol documentation must include:**
- Core theory and fundamentals
- Complete data frame structure with visual diagrams
- Field-by-field descriptions
- Common function codes (if applicable)
- Protocol variants/types
- Position in OSI model
- Comparisons with related protocols (e.g., UART vs SPI vs I2C)
- Real-world usage examples

### Phase 6: Update Status

26. Update `Learning-Docs/README.md` — change the firmware status from ⬜ to ✅
27. Present a summary of all files created/updated to the user for review

---

## Multi-Layer Documentation Model

Documentation in this project follows a **multi-layer model** where knowledge flows **both top-down and bottom-up** across three layers. This is NOT a simple two-layer or top-down system — each layer continuously enriches the others.

### The Three Layers

```
Layer 1: FUNDAMENTALS (Encyclopedia)          Layer 2: CONTROLLERS (Practical Guide)
Learning-Docs/Fundamentals/CPU.md             Learning-Docs/Controllers/STM32.md
Learning-Docs/Fundamentals/GPIO.md            Learning-Docs/Controllers/STM8.md
Learning-Docs/Fundamentals/ADC.md             Learning-Docs/Controllers/ESP32.md
Learning-Docs/Protocols/UART.md
Learning-Docs/Libraries/HAL.md
     │                                              │
     │  Supports understanding of                   │  Supports understanding of
     ▼                                              ▼
                    Layer 3: FIRMWARE (Project-Specific)
                    Learning-Docs/Firmware/<REPO>/FIRMWARE_GUIDE.md
                    Learning-Docs/Firmware/<REPO>/REAL_TIME_EXAMPLES.md
```

### Bidirectional Knowledge Flow

Knowledge does NOT only flow downward. When learning firmware, new insights flow **back up** to improve Fundamentals and Controllers documentation:

```
  TOP-DOWN (learning path):
  ────────────────────────────────────────────────────
  Fundamentals (CPU, GPIO, ADC, UART theory)
       │
       ▼  "I understand the concept, now how does STM32 do it?"
  Controllers (STM32 registers, clock config, pin map)
       │
       ▼  "I understand the controller, now how does this firmware use it?"
  Firmware (FIRMWARE_GUIDE.md, REAL_TIME_EXAMPLES.md)


  BOTTOM-UP (enrichment path):
  ────────────────────────────────────────────────────
  Firmware learning reveals a new concept or behavior
       │
       ▼  "This concept should be in Fundamentals!"
  Update Fundamentals (e.g., DMA circular mode, discovered during ADC firmware)
       │
       ▼  "This controller behavior should be documented!"
  Update Controllers (e.g., STM32F407 DMA stream assignment table)

  BOTH DIRECTIONS happen in EVERY learning session.
```

### Layer Definitions

| Layer | Files | Content | "In MOT" notes? |
|---|---|---|---|
| **Fundamentals** | `Fundamentals/*.md`, `Protocols/*.md`, `Libraries/*.md` | Complete, deep, project-agnostic. What is it? How does it work at bit/register/electrical level? How to use it? Industry-standard real-world examples. | ❌ Never |
| **Controllers** | `Controllers/*.md` | Controller-specific practical guide. Brief summary of concept (1-3 paragraphs) + how this MCU implements it + configuration + project notes + section-level link to Fundamentals for deep dive. | ✅ Yes |
| **Firmware** | `Firmware/<REPO>/*.md` | Project-specific firmware walkthrough. Code analysis, execution traces, task interactions. References both Fundamentals and Controller docs. | ✅ Yes |

### Content Distribution Rules

These rules determine WHERE content belongs:

| Content Type | → Fundamentals | → Controllers | → Firmware |
|---|---|---|---|
| "What is RISC?" (concept) | ✅ Full explanation | Brief: "Cortex-M4 uses RISC" | — |
| "How does FPU work at hardware level?" | ✅ Deep explanation | Brief: "STM32F407 has FPU" | — |
| "Is FPU enabled in our project?" | — | ✅ "In MOT: FPU disabled" | ✅ reference |
| "What registers does FPU have?" | ✅ Register details | Reference → Fundamentals | — |
| "How to enable FPU in STM32CubeMX?" | — | ✅ Controller-specific | — |
| "What is NVIC conceptually?" | ✅ Full explanation | Brief + STM32 IRQ table | — |
| "What IRQ numbers does MOT use?" | — | — | ✅ Project-specific |
| "How does UART frame work?" | ✅ in Protocols/UART.md | Brief + STM32 UART registers | Reference |
| "What baud rate does MOT use?" | — | ✅ "In MOT: 9600" | ✅ code |

**Key rules:**
1. **Both layers must cover the topic** — depth differs, but neither can skip it entirely
2. **Fundamentals = WHAT + WHY + HOW (generic)** — A developer can learn the topic from scratch
3. **Controllers = HOW (specific) + PROJECT NOTES** — A developer can configure the feature on this MCU
4. **No duplication of deep content** — If Fundamentals has a deep explanation, Controllers links to it instead of repeating
5. **No "In MOT" in Fundamentals** — Fundamentals are industry-standard, usable by anyone
6. **Firmware insights flow back up** — When firmware learning reveals new concepts, update Fundamentals and Controllers

### Linking Strategy

#### Section-Level Links (mandatory)

Every section in Controllers/ that refers to a concept explained in Fundamentals/ or Protocols/ **must** include a section-level link pointing to the exact heading:

**Format:**
```markdown
> 📖 **Deep dive:** [Topic — Subtitle](../Fundamentals/FILE.md#section-anchor)
```

**Placement rules:**
- Place at the **end of the section** (after controller-specific explanation and any "In MOT" notes)
- OR **inline** when first mentioning a concept explained elsewhere

#### File-Level Links (supplementary)

A "See also" footer at the bottom of each file provides quick navigation to all related files. This is **supplementary** to section-level links, not a replacement.

### Handling Project-Specific Notes ("In MOT")

When the same concept appears in both Fundamentals and Controllers:

```
Fundamentals/CPU.md (generic):                Controllers/STM32.md (specific):
─────────────────────────────                  ─────────────────────────────────
### FPU — Floating Point Unit                  ### 1.9 Key Feature: FPU

What is floating point...                      The Cortex-M4 in STM32F407 includes
IEEE 754 standard...                           a single-precision FPU.
Soft-float vs hard-float...
FPU registers (S0–S31)...                      **FPU in STM32F407:**
FPU instructions (VADD, VMUL)...               - Single-precision only
Generic examples...                            - Register bank: S0–S31

❌ No project notes here                       > **In MOT:** FPU is **disabled**
                                               > (configENABLE_FPU = 0)

                                               > 📖 **Deep dive:** [FPU — How
                                               > floating-point works at hardware
                                               > level](../Fundamentals/CPU.md#fpu)
```

---

## Style Rules

- Use mermaid diagrams for flow and architecture
- Use ASCII box-drawing for register bit layouts
- Use tables for comparisons, pin maps, timing
- Use code blocks with inline `// ←` annotations
- Explain child functions BEFORE parent functions
- Show register BEFORE/AFTER states for every register write
- Always state WHO does what: "Hardware sets...", "Software must clear..."
- Include timing information (µs/ms) wherever possible
- When a concept is fundamental (applies across firmware), put it in Fundamentals/ and reference it — don't duplicate
- Keep explanations structured and readable — cover all key points but avoid excessive verbosity
- Use graphical representations (flow diagrams, waveforms, block diagrams) wherever they aid understanding
- In REAL_TIME_EXAMPLES: show binary operations, memory addresses, register bit diagrams, and bus waveforms step-by-step
- Always include State Flow and Program Flow Diagrams
- Always show mathematical derivations with formula → substitution → result
- Always end detailed sections with a summary table

---

## User Preferences (captured from sessions)

These preferences were observed during learning sessions and should be followed in future chats:

1. **Register-level detail** — User wants to understand what happens at the register and memory level, not just the API level
2. **Memory-level flow** — Show memory addresses, hex values, and how data moves between registers and RAM
3. **Step-by-step execution** — Trace through code one operation at a time, showing state changes
4. **Graphical representations** — Prefers ASCII diagrams, waveforms, block diagrams, flow charts over paragraph text
5. **Bit diagrams** — Show before/after bit-level register states for every hardware operation
6. **Conceptual questions** — User asks deep "why" questions (e.g., "Is NVIC hardware or firmware?", "Does CPU use assembly?"). Always address these underlying concepts
7. **Analogies** — User appreciates analogies to everyday concepts (brain/body for CPU/MCU)
8. **Completeness over brevity** — Cover all concepts, but maintain good structure and readability
9. **Theory + practice** — Explain concepts theoretically first, then show how they apply in the actual firmware code
10. **Cross-referencing** — User expects links to related documents whenever a topic is mentioned
11. **State Flow and Program Flow Diagrams** — Always include visual execution flow diagrams
12. **Mathematical derivations** — Show formulas, substitute actual values, calculate step by step
13. **Summary tables** — Always end detailed sections with a compact summary table
14. **"What if wrong?"** — Explain consequences of misconfiguration or wrong values
15. **Conversation-first** — Answer fully in chat, then update .md files. Never say "go read the file"
