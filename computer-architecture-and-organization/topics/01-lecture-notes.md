# Computer Architecture and Organization: Lecture Notes

Study notes compiled from the lecture slides on **Computer Architecture and Organization**.

> **Big idea:** Software tells the computer what to do. Computer architecture and organization help us understand how it gets done.

## Table of Contents

1. [What is Computer Architecture and Organization?](#1-what-is-computer-architecture-and-organization-slide-03)
2. [Why study this?](#2-why-should-cs-students-study-this-slide-04)
3. [Components of a computer](#3-what-does-each-component-do-slide-07)
4. [Architecture vs. organization](#4-architecture-vs-organization-slide-08)
5. [Number systems](#5-number-systems-in-computing-slide-09)
6. [Boolean logic and logic gates](#6-boolean-logic-and-logic-gates-slide-10)
7. [The instruction cycle](#7-the-instruction-cycle-slide-12)
8. [Memory hierarchy](#8-memory-hierarchy-slide-13)
9. [Input/output and system buses](#9-inputoutput-and-system-buses-slide-14)
10. [Performance and modern processors](#10-performance-and-modern-processors-slide-15)
11. [Hands-on activities](#11-hands-on-activities)
12. [Quick knowledge check](#12-quick-knowledge-check-slide-19)
13. [Quick reference](#quick-reference)

---

## 1. What is Computer Architecture and Organization? (Slide 03)

The study of a computer's **structure, behavior, and implementation**.

| | Computer Architecture | Computer Organization |
|---|---|---|
| **Definition** | The features visible to a programmer: what the computer can do | How hardware components are arranged and work together to implement the architecture |
| **Examples** | Instruction set, data types, addressing modes | Control signals, memory technology, buses, processor design |

Architecture is the **"what"** (the programmer's view). Organization is the **"how"** (the hardware that makes it happen).

---

## 2. Why should CS students study this? (Slide 04)

Understanding the machine helps you write and evaluate better software.

- **Understand execution:** know how source code becomes operations performed by a processor
- **Make informed choices:** understand performance, memory use, and hardware limitations
- **Connect software and hardware:** relate programming, operating systems, compilers, and devices
- **Build a strong foundation:** prepare for systems programming, embedded systems, and advanced CS topics

---

## 3. What does each component do? (Slide 07)

Think of the computer as a coordinated system.

| Component | Role |
|---|---|
| **CPU** | Executes instructions and coordinates processing |
| **Main memory** | Holds active programs and data for quick access |
| **Input devices** | Send data and commands into the computer |
| **Output devices** | Present information to the user or another system |
| **Secondary storage** | Keeps files and programs when power is off |
| **System bus** | Moves data, addresses, and control signals |

### The three major parts of a CPU

- **Control Unit:** directs and coordinates the processor's operations
- **ALU (Arithmetic Logic Unit):** performs arithmetic and logic operations
- **Registers:** small, very fast storage inside the CPU

*(The three names come from the lecture. The one-line descriptions are general background, not taken from the slides.)*

---

## 4. Architecture vs. organization (Slide 08)

A useful distinction throughout the course.

| Computer Architecture | Computer Organization |
|---|---|
| Programmer-visible behavior | Internal hardware implementation |
| What the system does | How the system is built |
| Instruction set, data types, addressing | Control unit, buses, memory technology |

**Example:** the instruction set defines an `ADD` operation (architecture); the organization determines the circuitry that carries it out.

---

## 5. Number systems in computing (Slide 09)

Computers represent information using patterns of bits.

| System | Base | Digits |
|---|---|---|
| **Binary** | 2 | 0, 1 |
| **Decimal** | 10 | 0-9 |
| **Octal** | 8 | 0-7 |
| **Hexadecimal** | 16 | 0-9, A-F |

**Example conversion:** `25 (base 10) = 11001 (base 2)`
Check: 16 + 8 + 1 = 25.

**Why binary?** Digital circuits can reliably represent two states, commonly interpreted as 0 and 1.

---

## 6. Boolean logic and logic gates (Slide 10)

Digital circuits use logical operations to process binary signals.

| Gate | Output |
|---|---|
| **AND** | 1 only if both inputs are 1 |
| **OR** | 1 if at least one input is 1 |
| **NOT** | Reverses the input |
| **XOR** | 1 if the inputs are different |
| **NAND** | Opposite of AND |
| **NOR** | Opposite of OR |

**Example:** a door unlocks only when the correct code AND an authorized access card are detected.

---

## 7. The instruction cycle (Slide 12)

A simplified sequence for processing an instruction.

```
FETCH  ->  DECODE  ->  EXECUTE  ->  STORE
```

1. **Fetch:** get the next instruction from memory
2. **Decode:** interpret the operation and operands
3. **Execute:** perform the required operation
4. **Store:** write the result, when needed

The cycle repeats as the processor continues executing program instructions.

---

## 8. Memory hierarchy (Slide 13)

A trade-off among **speed, capacity, and cost**.

| Level | Description |
|---|---|
| **Registers** | Fastest, very small, inside the CPU |
| **Cache** | Very fast, small, near/inside the CPU |
| **RAM** | Main working memory, volatile |
| **SSD / HDD** | Large, long-term storage |

Moving down the hierarchy generally means **larger capacity and lower cost per bit, but slower access**.

---

## 9. Input/output and system buses (Slide 14)

How the computer communicates with devices.

- **Input / output:** keyboard, mouse, display, printer, network adapter, and other peripherals
- **Data bus:** carries the actual data being transferred
- **Address & control:** identifies locations and coordinates when and how transfers occur

**I/O mechanisms include:** programmed I/O, interrupts, and Direct Memory Access (DMA).

---

## 10. Performance and modern processors (Slide 15)

More than clock speed determines how fast a system feels.

- **Clock rate:** how many clock cycles occur per second (measured in Hz)
- **CPI:** average clock cycles required per instruction
- **Parallelism:** pipelining, multiple cores, and concurrent execution

### CPU time relationship

```
CPU time = Instruction Count x CPI x Clock Cycle Time
```

Real-world performance also depends on memory, storage, software, workload, and system design.

*Note: clock cycle time is the inverse of clock rate (cycle time = 1 / clock rate). This note is added background, not from the slide.*

---

## 11. Hands-on activities

Practice concepts using simple tools and simulations.

| Activity | What students do | Suggested tool |
|---|---|---|
| **Number system challenge** | Convert binary, decimal, and hexadecimal values | Calculator / Python |
| **Logic gate simulator** | Build circuits and complete truth tables | Logisim Evolution |
| **Binary calculator** | Write a program for binary addition | Java / Python |
| **CPU simulation** | Explore registers and instruction execution | MARIE Simulator |
| **Assembly programming** | Write and run basic assembly instructions | MARIE / RARS |
| **Performance analysis** | Compare CPU time using instruction count and CPI | Spreadsheet |

---

## 12. Quick knowledge check (Slide 19)

Discuss your answers with a partner.

1. What is the difference between computer architecture and computer organization?
2. Why do computers use binary representation?
3. What are the three major components of a CPU?
4. What are the four basic stages of the instruction cycle?
5. How is RAM different from secondary storage?

<details>
<summary>Answers (from the notes above)</summary>

1. **Architecture** is the programmer-visible behavior (what the system does). **Organization** is the internal hardware implementation (how it is built).
2. Digital circuits can reliably represent two states, commonly interpreted as 0 and 1.
3. Control Unit, ALU, Registers.
4. Fetch, decode, execute, store.
5. RAM is volatile main working memory holding active programs and data for quick access. Secondary storage (SSD/HDD) keeps files and programs when power is off.

</details>

---

## Quick reference

| Concept | One-liner |
|---|---|
| Architecture | What the system does (programmer's view) |
| Organization | How the system is built (hardware view) |
| Bit | A binary digit, 0 or 1 |
| Instruction cycle | Fetch, decode, execute, store |
| CPU parts | Control Unit, ALU, Registers |
| Memory order (fast to slow) | Registers, Cache, RAM, SSD/HDD |
| CPU time | Instruction Count x CPI x Clock Cycle Time |
| I/O mechanisms | Programmed I/O, interrupts, DMA |

## Closing thought

> Software tells the computer what to do. Computer architecture and organization help us understand how it gets done.

Reflection prompts from the final slide: *Questions? Reflections? Connections to your programming experience?*

---

### Notes on completeness

- These notes were compiled from screenshots of the slides. Slides **05, 06, 11**, and any slides between **15 and 19** were not captured, so some topics (for example, CPU internals in more detail) may be missing.
- Slide numbers for the hands-on activities and closing slides were not visible, so they are listed without numbers.
- Some small text in low-resolution screenshots (cache and SSD/HDD labels on slide 13) was a best read.