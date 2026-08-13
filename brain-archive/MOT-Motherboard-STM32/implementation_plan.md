# Documentation Management & Linking Strategy

## The Problem

Currently there are **3 issues** in the documentation:

### Issue 1: Missing Content in CPU.md

[CPU.md](file:///c:/Users/oml/Documents/BitBucket/MOT/Docs/Fundamentals/CPU.md) is the **Fundamentals** file — it should be the complete, deep reference for CPU-related topics. But several key features explained in [STM32.md](file:///c:/Users/oml/Documents/BitBucket/MOT/Docs/Controllers/STM32.md) Section 1 are **missing** from CPU.md:

| Feature | In STM32.md? | In CPU.md? | Gap |
|---|---|---|---|
| RISC Architecture (§1.6) | ✅ Full | ❌ Missing | CPU.md should have this |
| Thumb-2 Instruction Set (§1.7) | ✅ Full | ❌ Missing | CPU.md should have this |
| DSP (§1.8) | ✅ Summary | ✅ Deep | OK — but needs linking |
| FPU (§1.9) | ✅ Full | ❌ Missing | CPU.md should have this |
| NVIC (§1.10) | ✅ Full | ⚠️ Brief mention | CPU.md should have deep explanation |
| Cortex-M Family (§1.11) | ✅ Full | ✅ Table exists | OK — minor overlap |

### Issue 2: Wrong Linking Style

Currently links are only at the **bottom of the file** as a generic "See also" list. No section-level links exist inside the content.

**Current (wrong):**
```
*See also: CPU.md · UART.md · ADC.md*  ← at the very end
```

**Wanted:**
```
### 1.8 Key Feature: DSP
...explanation...
> 📖 **Deep dive:** [DSP — How it works at register/bit level](../Fundamentals/CPU.md#7-dsp--digital-signal-processing)
```

### Issue 3: No Clear Rules for Content Distribution

There's no documented strategy for: "What goes in Fundamentals vs Controllers?" This leads to confusion about where content belongs and causes duplication.

---

## Proposed Documentation Management Strategy

### The Two-Layer Model

```
┌─────────────────────────────────────────────────────────────────┐
│  Fundamentals/CPU.md  ←  THE ENCYCLOPEDIA                      │
│                                                                 │
│  Complete, deep, project-agnostic explanations                  │
│  ─ What is it?                                                  │
│  ─ How does it work? (at bit/register/electrical level)         │
│  ─ How to use it? (generic examples)                            │
│  ─ Real-world examples (industry, not project-specific)         │
│  ─ No "In MOT" notes                                           │
│  ─ Goal: A developer can learn the topic from scratch           │
└──────────────────────────┬──────────────────────────────────────┘
                           │
                    Links (section-level)
                           │
┌──────────────────────────▼──────────────────────────────────────┐
│  Controllers/STM32.md  ←  THE PRACTICAL GUIDE                  │
│                                                                 │
│  Controller-specific: how STM32F407 implements these features   │
│  ─ Concise summary of WHAT the feature is (1-3 paragraphs)     │
│  ─ HOW this specific controller uses it                         │
│  ─ Project-specific notes: "In MOT: ..."                       │
│  ─ Code examples from actual firmware                           │
│  ─ Link to Fundamentals for deep dive                          │
│  ─ Goal: A developer can configure this feature on STM32       │
└─────────────────────────────────────────────────────────────────┘
```

### Content Distribution Rules

| Question | → Goes in Fundamentals/ | → Goes in Controllers/ |
|---|---|---|
| "What is RISC?" | ✅ Full explanation | Brief: "Cortex-M4 uses RISC" |
| "How does FPU work at hardware level?" | ✅ Deep explanation | Brief: "STM32F407 has single-precision FPU" |
| "Is FPU enabled in our firmware?" | ❌ | ✅ "In MOT: FPU disabled" |
| "What registers does FPU have?" | ✅ Register details | Reference to Fundamentals |
| "How to enable FPU in STM32CubeMX?" | ❌ | ✅ Controller-specific |
| "What is NVIC conceptually?" | ✅ Full explanation | Brief summary + STM32 IRQ table |
| "What IRQ numbers does MOT use?" | ❌ | ✅ Project-specific table |

### Rule summary:

1. **Fundamentals = WHAT + WHY + HOW (generic)**
   - Complete theory, hardware-level, bit-level, electrical-level
   - Generic real-world examples (not project-specific)
   - Industry-standard — useful for any embedded developer
   - NO "In MOT" notes

2. **Controllers = HOW (specific) + PROJECT NOTES**
   - Brief summary (1-3 paragraphs) of the concept
   - How this specific MCU implements the feature (register addresses, peripherals, configurations)
   - Project-specific notes: "In MOT: ..."
   - Code examples from actual firmware
   - Section-level link → Fundamentals for deep dive

3. **No skipping** — Both files must cover the topic. The depth differs:
   - Fundamentals: deep encyclopedia entry
   - Controllers: concise practical guide with project context

### Handling Project Notes ("In MOT")

```
In CPU.md (Fundamentals — generic):
─────────────────────────────────
  ### FPU — Floating Point Unit
  [Full explanation: what it is, how it works, registers, 
   soft-float vs hard-float, IEEE 754, examples...]
  
  ❌ No "In MOT" notes here

In STM32.md (Controller — specific):
─────────────────────────────────
  ### 1.9 Key Feature: FPU
  Brief: The Cortex-M4 in STM32F407 includes a single-precision FPU.
  
  > 📖 **Deep dive:** [FPU — How floating-point works at hardware level](../Fundamentals/CPU.md#fpu-section)
  
  **FPU in STM32F407:**
  - Single-precision only (32-bit float)
  - Register bank: S0–S31
  - Supports: add, subtract, multiply, divide, sqrt, compare
  
  > **In MOT:** FPU is present but **disabled** (configENABLE_FPU = 0 in 
  > FreeRTOSConfig.h). The firmware uses float for temperature calculations
  > but the compiler emulates them in software.
```

---

## Linking Strategy

### Section-Level Contextual Links

> [!IMPORTANT]
> Every section in Controllers/ that refers to a concept in Fundamentals/ or Protocols/ must include a **section-level link** pointing to the exact heading in the target file.

**Link format:**
```markdown
> 📖 **Deep dive:** [Topic Name](../Fundamentals/FILE.md#section-anchor)
```

**Where to place links:**
- At the **end of the section** (after the controller-specific explanation)
- OR **inline** when mentioning a concept that's explained elsewhere

### Examples of section-level links to add in STM32.md

| STM32.md Section | Link Target |
|---|---|
| 1.6 RISC Architecture | `CPU.md#risc-architecture` |
| 1.7 Thumb-2 Instruction Set | `CPU.md#thumb-2-instruction-set` |
| 1.8 DSP | `CPU.md#7-dsp--digital-signal-processing` |
| 1.9 FPU | `CPU.md#fpu--floating-point-unit` |
| 1.10 NVIC | `CPU.md#nvic--interrupt-controller` |
| 4. Clock System | `Clock_System.md` (section link) |
| 7. GPIO | `GPIO.md` (section link) |
| 8. UART | `UART.md` (section link) |
| 9. ADC | `ADC.md` + `Polling_vs_Interrupt_vs_DMA.md` |
| 10. Timers & PWM | `Timers_and_Counters.md` + `PWM.md` |
| 11. HAL Library | `HAL.md` |

### Keep the "See also" footer

The existing footer at the bottom of STM32.md can remain as a **quick navigation** reference, but the real value is in the section-level links inside each section.

---

## Proposed Changes

### 1. Expand CPU.md — Add Missing Key Features

Add 4 new sections to [CPU.md](file:///c:/Users/oml/Documents/BitBucket/MOT/Docs/Fundamentals/CPU.md):

| New Section | Content |
|---|---|
| **RISC vs CISC** | What is RISC, what is CISC, comparison, why embedded uses RISC, pipeline advantages, instruction examples |
| **Instruction Sets (Thumb-2)** | What is an ISA, ARM vs Thumb vs Thumb-2, 16/32-bit mixing, encoding examples, code density comparison |
| **FPU — Floating Point Unit** | What is floating point, IEEE 754, soft-float vs hard-float, FPU registers (S0-S31), FPU instructions, when to enable |
| **NVIC — Interrupt Controller** | What are interrupts, NVIC hardware, priority levels, nesting, vector table, context save/restore, tail-chaining |

All written **project-agnostically** — no "In MOT" notes. Deep, register-level, with diagrams.

### 2. Refactor STM32.md Section 1 — Add Section-Level Links

For each key feature subsection (1.6–1.10), add a contextual link at the end:

```markdown
> 📖 **Deep dive:** [RISC vs CISC — How instruction sets differ](../Fundamentals/CPU.md#risc-vs-cisc-architecture)
```

The existing STM32.md content stays mostly as-is (it already has good controller-specific explanations with "In MOT" notes). We only **add links**, not remove content.

### 3. Add Section-Level Links Throughout STM32.md

Add contextual links in sections 4–11 pointing to the relevant Fundamentals/Protocols files.

### 4. Update learn-firmware.md — Add Documentation Management Rules

Add a new section to [learn-firmware.md](file:///c:/Users/oml/Documents/BitBucket/MOT/.agents/workflows/learn-firmware.md) documenting:

- The Two-Layer Model
- Content Distribution Rules
- Linking Strategy (section-level, format, placement)
- "In MOT" notes handling

---

> [!IMPORTANT]
> ### Summary of the approach
> - **CPU.md** = Complete encyclopedia (RISC, Thumb-2, FPU, NVIC, DSP, all deep)
> - **STM32.md** = Practical guide (brief summary + project notes + section-level link to CPU.md)
> - **Links** = Section-level, contextual, placed inside each section (not just at bottom)
> - **No content removed** from STM32.md — only links added
> - **learn-firmware.md** updated with these rules for all future documentation

---

## Verification Plan

### Manual Verification
- After changes: navigate each link in STM32.md → verify it lands on the correct section in CPU.md
- Read CPU.md end-to-end → verify all key features are covered with deep explanations
- Read STM32.md Section 1 → verify each subsection has a section-level link
- Check that no "In MOT" notes exist in CPU.md
