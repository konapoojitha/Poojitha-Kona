## ARM Processor:

### What is an Instruction?

An instruction is a single operation that a CPU (processor) can execute.

Examples:

* `MOV R0, #5` Move the value `5` into register `R0`
* `ADD R1, R2, R3` Add the contents of `R2` and `R3`, store in `R1`
* `STR R0, [R1]` Store the value in `R0` into the memory address pointed by `R1`

It tells the CPU what to do (move data, do arithmetic, jump)

### What is an Instruction Set?

An instruction set is the complete collection of instructions that a specific processor can understand and execute.

Also called: Instruction Set Architecture (ISA)

It defines:

* The operations a CPU can perform (add, load, jump)
* The formats of instructions (e.g., binary encoding)
* The registers used
* Addressing modes (how memory is accessed)

### Examples of Instruction Sets:

| Processor Type | Instruction Set |
| -------------- | --------------- |
| ARM Cortex-A   | ARM + Thumb     |
| Intel x86 CPU  | x86 / x86-64    |
| MIPS processor | MIPS            |
| RISC-V CPU     | RISC-V          |


### Instruction Sets (ARM & THUMB)
* ARM Instruction Set:
  * 32-bit fixed-length instructions.
  * Rich set: arithmetic, logic, memory, control, system.
* THUMB Instruction Set:
  * 16-bit compressed instructions.
  * Better code density, used in constrained environments.
* Thumb-2: Combines 16-bit & 32-bit instructions for performance + efficiency.

### What is Thumb Instruction Set in ARM?
The Thumb instruction set is a compressed, 16-bit version of the standard 32-bit ARM instruction set, designed to improve code density and reduce memory usage, especially in embedded systems.

### Why Thumb Was Introduced
* Reduce memory footprint: Useful where Flash/ROM is limited.
* Save power: Less memory access = lower energy.
* Improve performance on low-bandwidth memory systems.

### How It Works
* Thumb uses a subset of ARM instructions, encoded in 16 bits.
* The CPU switches modes between ARM and Thumb using the T bit in the CPSR (Current Program Status Register).
* When in Thumb mode, the processor fetches and executes 16-bit Thumb instructions.

### Switching Between ARM and Thumb
In assembly, the switch is typically done using:

```assembly
BX R0; Branch and exchange — switches mode depending on LSB of R0
```

* If R0's least significant bit = 1, switch to Thumb.
* If R0's LSB = 0, switch to ARM.

---

### Example: ARM vs Thumb

| ARM Instruction   | `MOV R1, #1` → 32 bits |
| ------------------| ---------------------- |
| Thumb Instruction | `MOV R1, #1` → 16 bits |

Both do the same thing, but Thumb uses fewer bits.

---
### Thumb-2
* Introduced in ARMv6T2 / ARMv7.
* Code is written using a mixture of 16-bit and 32-bit instructions.
* Most modern Cortex-M and Cortex-A cores support Thumb-2.
```
// Thumb (16-bit)
MOVS R0, #1
ADDS R1, R0, #2

// Thumb-2 (mixed)
MOVS R0, #1          ; 16-bit
ADD  R1, R0, R2, LSL #2 ; 32-bit (more complex operation)
```

### Interview Q: What is CISC?
CISC (Complex Instruction Set Computer) is a CPU architecture design that uses a large set of complex instructions, where a single instruction can perform multiple low-level operations (like memory access, arithmetic, and control flow).

## Example CISC Instruction

```assembly
MOV AX, [1234]     ; Move data directly from memory to register AX
```

* In CISC, this single instruction might load the memory address, fetch the value, and write it to a register — all in one instruction.
* CISC focuses on reducing the number of instructions per program, even if it makes each instruction more complex and slower.
* Popular in general-purpose computers and desktops (like Intel and AMD CPUs).

---

## Examples of CISC Architectures

| Architecture       | Notes                                                  |
| ------------------ | ------------------------------------------------------ |
| x86 / x86-64   | Most common CISC architecture, used in PCs and servers |

### Interview Q: What is RISC?

RISC (Reduced Instruction Set Computer) is a CPU design philosophy that uses a small, highly optimized set of simple instructions, each designed to execute very quickly — typically in one clock cycle.

## Example of RISC Instructions (Assembly)

```assembly
LDR R1, [R0]      ; Load value from memory to register
ADD R2, R1, R3    ; Add contents of R1 and R3, store in R2
STR R2, [R0, #4]  ; Store result back to memory
```
## Examples of RISC Architectures
| Architecture | Notes                                                                          |
| ------------ | ---------------------------------------------------------------------------    |
| ARM          | Most popular RISC architecture, used in smartphones, tablets, embedded systems |
| MIPS         | Widely used in routers, IoT                                                    |
| RISC-V       | Open-source RISC architecture gaining popularity                               |

* RISC = simple instructions, fast execution.
* Ideal for systems where power efficiency, speed, and performance per watt matter — like smartphones, IoT, and embedded systems.
* ARM is the most popular RISC implementation.

### Interview Q: What is the diff b/w RISC and CISC?

```
## Difference Between CISC and RISC (Qualcomm Interview Context)

**CISC (Complex Instruction Set Computer)** and **RISC (Reduced Instruction Set Computer)** are two types of CPU architecture designs.

### CISC:
- Has complex instructions that can perform multiple operations in a single instruction.
- Instructions are often **variable in length** and may take **multiple clock cycles** to execute.
- Focuses on reducing the number of instructions per program, at the cost of more complex hardware.

**Example**:  
- **x86 architecture** (used in Intel processors) is CISC.  
- Instruction like `MUL` can directly multiply two memory operands.

### RISC:
- Uses a smaller set of **simple instructions**, each typically executed in **one clock cycle**.
- Instructions are of **uniform size** and follow a **load/store** architecture.
- Emphasizes compiler optimization and simpler hardware design.

**Example**:  
- **ARM architecture** (used in Qualcomm Snapdragon) is RISC.  
- Multiplication would use multiple instructions like:
  - `LDR` (load data)
  - `MUL` (multiply)
  - `STR` (store result)
```
### ARM vs x86 Instruction Sets (ISA)
```
                     ┌────────────────────────────┐
                     │   CISC vs RISC Architectures  │
                     └────────────────────────────┘
                               ▲
                               │
          ┌──────────────────────────────┬─────────────────────────────┐
          │              CISC            │              RISC           │
          └──────────────────────────────┴─────────────────────────────┘
          │                              │                             │
 ┌────────────────┐           ┌────────────────┐            ┌────────────────┐
 │ Complex        │           │ Simple         │            │ Few            │
 │ Instructions   │           │ Instructions   │            │ Instruction    │
 │ (Multi-step)   │           │ (One-step)     │            │ Formats        │
 └────────────────┘           └────────────────┘            └────────────────┘
          │                              │                             │
 ┌────────────────┐           ┌────────────────┐            ┌────────────────┐
 │ Fewer lines of │           │ More lines of  │            │ Load/Store     │
 │ assembly code  │           │ assembly code  │            │ Architecture   │
 └────────────────┘           └────────────────┘            └────────────────┘
          │                              │                             │
 ┌────────────────┐           ┌────────────────┐            ┌────────────────┐
 │ Variable-length│           │ Fixed-length   │            │ Fast execution │
 │ instructions   │           │ instructions   │            │ (one cycle)    │
 └────────────────┘           └────────────────┘            └────────────────┘
          │                              │                             │
 ┌────────────────┐           ┌────────────────┐            ┌────────────────┐
 │ Examples:      │           │ Examples:      │            │ Uses pipelining│
 │ x86, VAX       │           │ ARM, RISC-V    │            │ efficiently    │
 └────────────────┘           └────────────────┘            └────────────────┘
```

```
### Interview Q: In CISC and RISC Which executes faster and why?

In most modern systems, RISC architectures typically execute instructions faster than CISC.

### RISC Executes Faster – Here's Why:

| Feature                        | RISC                         | CISC                                 |
| -------------------------------| ---------------------------- | ------------------------------------ |
| Instruction Complexity         | Simple, uniform instructions | Complex, multi-step instructions     |
| Execution Time per Instruction | 1 clock cycle (usually)      | Multiple clock cycles                |
| Pipelining Efficiency          | Very efficient               | Harder to pipeline                   |
| Instruction Decoding           | Simple (fixed size)          | Complex (variable size)              |
| Hardware Requirements          | Simpler, optimized for speed | More complex logic (e.g., microcode) |


### Example: Add Two Numbers

#### ARM Assembly (RISC)

```asm
MOV R0, #5
MOV R1, #3
ADD R2, R0, R1

#### x86 Assembly (CISC)

```asm
MOV AX, 5
ADD AX, 3


* ARM: Each step is simple and explicit.
* x86: Instructions are powerful and compact (ADD can take immediate value).
```
### What is a Processor?

A processor (also called a CPU – Central Processing Unit) is the brain of a computer or embedded system. It performs calculations, logical decisions, and controls the execution of programs by processing instructions one step at a time.

### Key Functions of a Processor:

1. Fetch – Gets the next instruction from memory.
2. Decode – Interprets what the instruction means.
3. Execute – Performs the instruction using ALU or control units.
4. Store – Writes back the result into memory or a register.

### Main Components Inside a Processor:

| Component         | Purpose                                             |
| ----------------- | --------------------------------------------------- |
| ALU           | Performs arithmetic and logic operations            |
| Control Unit  | Directs data flow and manages instruction execution |
| Registers     | Small fast storage used during execution            |
| Cache         | Very fast memory to reduce access time to RAM       |
| Bus Interface | Connects processor to memory and I/O devices        |

### Types of Processors:

| Type                  | Use Case Example                      |
| --------------------- | ------------------------------------- |
| General-purpose   | PCs, laptops (Intel, AMD CPUs)        |
| Microcontrollers  | Embedded systems (ARM Cortex-M)       |
| DSP Processors    | Audio and signal processing           |
| Mobile Processors | Smartphones (ARM Cortex-A, Apple M1)  |
| Server CPUs       | Data centers (ARM Neoverse, AMD EPYC) |

### Real-World Example:

* When you click a button in an app, the processor:

  * Fetches the instruction
  * Decodes it (update screen)
  * Executes a series of math/logical tasks
  * Stores the result (change display, play sound)

### 1. Introduction to ARM

* ARM stands for Advanced RISC Machines.
* It’s a family of reduced instruction set computing (RISC) architectures.
* Originally developed by Acorn Computers, now licensed by Arm Holdings.
* Known for: high performance, low power consumption.
* Dominates in smartphones, embedded systems, IoT, and microcontrollers.

### Interview Q: What does ARM stand for?

Originally Acorn RISC Machine, later renamed to Advanced RISC Machine.

### Interview Q: What is ARM processor?

An ARM processor is a small, power-efficient CPU that runs instructions very quickly using a simplified (RISC-based) instruction set — perfect for mobile devices and embedded systems.

### Block Diagram:

```
         +----------------+
         | ARM Processor  |
         +----------------+
                 |
         +----------------+
         |    Memory      |
         +----------------+
                 |
   +--------+    +------+    +--------+
   |  UART  |    | ADC  |    | Timer  |
   +--------+    +------+    +--------+

```

## 2. RISC vs CISC

| Feature      | RISC (e.g., ARM)            | CISC (e.g., x86)             |
| ------------ | --------------------------- | ---------------------------- |
| Instructions | Simple, fixed-length        | Complex, variable-length     |
| Execution    | Mostly 1 clock cycle        | Can take multiple cycles     |
| Code Size    | Larger (more instructions)  | Smaller (fewer instructions) |
| Efficiency   | High performance, low power | Complex but flexible         |
| Decoding     | Easier to pipeline          | Harder to decode             |

ARM = RISC → Efficient, low power → Ideal for mobile and embedded.

## 3. ARM Core Families
```

                   ┌────────────────────────────────────┐
                   │          ARM Core Families         │
                   └────────────────────────────────────┘
                                 ▲
                                 │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
 ┌─────────────┐         ┌──────────────┐         ┌─────────────┐
 │  Cortex-M   │         │  Cortex-R    │         │  Cortex-A   │
 │ (Microcontroller) │   │ (Real-time)  │         │ (Application)│
 └─────────────┘         └──────────────┘         └─────────────┘
       ▲                        ▲                        ▲
       │                        │                        │
 ┌─────────────┐         ┌──────────────┐         ┌─────────────┐
 │ M0 / M0+    │         │ R5 / R7 / R8 │         │ A5 / A7 / A9│
 │ M3 / M4     │         │              │         │ A15 / A17   │
 │ M23 / M33   │         │              │         │ A53 / A57   │
 │ M35P / M55  │                                │ A72 / A73   │
 │ M85         │                                │ A75 / A76   │
                                                  │ A77 / A78   │
                                                  │ X1 / X2     │
                                                  └─────────────┘

       ┌─────────────────────────────┐
       │     Neoverse (Infrastructure CPUs)     │
       └─────────────────────────────┘
                   ▲
        ┌────────────┬────────────┬────────────┐
        │   E-Series │   N-Series │   V-Series │
        │ (Edge)     │ (Cloud)    │ (High Perf)│
        └────────────┴────────────┴────────────┘
```

* Cortex-A: Application processors (smartphones, tablets)
* Cortex-R: Real-time processors (automotive, industrial)
* Cortex-M: Microcontrollers (IoT, wearables, sensors)
* There are three primary lines in the Neoverse portfolio:
| Family            | Focus                   | Key Features                             | Target Workloads                                |
| ------------------| ------------------------| ---------------------------------------- | ------------------------------------|
| Neoverse V-Series | Performance             | Out-of-order, high IPC, large caches     | AI/ML, cloud workloads              |
| Neoverse N-Series | General Compute         | Balanced performance and efficiency      | Cloud servers, storage              |
| Neoverse E-Series | Efficiency & Throughput | In-order, high core count, dense compute | Edge devices, networking appliances |


## 4. Interview Q: ARM Architecture Overview
```
                    ┌────────────────────────────┐
                    │      ARM Processor Core    │
                    └────────────────────────────┘
                                  ▲
            ┌────────────────────┼────────────────────┐
            │                    │                    │
  ┌────────────────┐   ┌────────────────────┐  ┌────────────────────┐
  │ Instruction     │   │   Data Processing   │  │   Control Logic     │
  │ Fetch & Decode  │   │ (ALU, Shifter, MAC) │  │ (FSM, Control Unit) │
  └────────────────┘   └────────────────────┘  └────────────────────┘
            ▲                    ▲                    ▲
            │                    │                    │
  ┌────────────────────────────────────────────────────────┐
  │                Register File (General Purpose + SP, PC)│
  └────────────────────────────────────────────────────────┘
                                 ▲
                                 │
                 ┌────────────────────────────────┐
                 │     Load/Store Unit (LSU)      │
                 └────────────────────────────────┘
                                 ▲
                 ┌────────────────────────────────┐
                 │      Memory Interface Unit     │
                 └────────────────────────────────┘
                            ▲             ▲
                            │             │
                ┌────────────────┐ ┌────────────────────┐
                │   Instruction  │ │     Data Memory     │
                │     Memory     │ │   (RAM/Cache/MMU)   │
                └────────────────┘ └────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │        Interrupt Controller / Exception Handler        │
       └────────────────────────────────────────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │             Coprocessor Interface (e.g., FPU)          │
       └────────────────────────────────────────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │              System Control Block / Debug              │
       └────────────────────────────────────────────────────────┘
```
* Load/Store architecture – memory access only through load/store.
* Pipelined execution – 3-stage to 8-stage pipelines.
* Modes: Multiple execution modes for security and interrupt handling.
* Instruction sets: Supports ARM, THUMB, and Thumb-2.
* ISA versions: ARMv7 (32-bit), ARMv8 (adds 64-bit), ARMv9 (security + AI)

## 5. Key Features
* Low Power Consumption – ideal for mobile/embedded.
* High Code Density – THUMB mode improves efficiency.
* Scalability – suitable from small MCUs to powerful SoCs.
* Conditional Execution – reduces branch instructions.
* Exception Modes & Banked Registers – fast interrupt handling.
* TrustZone – secure world for sensitive operations.

### Interview Q: What are modes in ARM?

## 6. ARM Processor Modes
```
                   ┌─────────────────────────────┐
                   │     ARM Processor Modes     │
                   └─────────────────────────────┘
                                 ▲
                                 │
                   ┌─────────────────────────┐
                   │     Two Main Levels     │
                   └─────────────────────────┘
                          ▲             ▲
                          │             │
              ┌───────────────┐   ┌───────────────┐
              │  User Mode    │   │ Privileged    │
              │ (Unprivileged)│   │    Modes      │
              └───────────────┘   └───────────────┘
                                        ▲
         ┌──────────────┬───────────────┬──────────────┬──────────────┬──────────────┬──────────────┐
         │ FIQ Mode     │ IRQ Mode      │ Supervisor   │ Abort Mode   │ Undefined    │ System Mode  │
         │ (Fast Int.)  │ (IRQ Handler) │ (OS Kernel)  │ (Mem aborts) │ (Undef Inst.)│ (Privileged) │
         └──────────────┴───────────────┴──────────────┴──────────────┴──────────────┴──────────────┘

```
## 1. User Mode (usr)
* Privilege Level: Unprivileged
* Purpose: Normal application (user-level) code runs here
* Access: Cannot access privileged system features or control registers
* Registers: Uses general-purpose registers (R0–R15), no banked registers

Key Point: Most user programs run in this mode — no access to hardware or critical resources.

## 2. FIQ Mode (fiq) – Fast Interrupt Request
* Privilege Level: Privileged
* Purpose: Handles high-priority, time-sensitive interrupts
* Registers: Has banked registers R8–R14 and SPSR\_fiq

Key Point: Designed for fast response and minimal overhead in time-critical tasks.

## 3. IRQ Mode (irq) – Interrupt Request
* Privilege Level: Privileged
* Purpose: Handles normal hardware interrupts (slower than FIQ)
* Registers: Banked R13 (SP), R14 (LR), and SPSR\_irq

Key Point: Used for most peripheral interrupt handling (timers, UART).

## 4. Supervisor Mode (svc)
* Privilege Level: Privileged
* Purpose: Handles OS-level operations and system calls
* Registers: Banked SP (R13), LR (R14), and SPSR\_svc

Key Point: The OS kernel usually runs in this mode.

## 5. Abort Mode (abt)
* Privilege Level: Privileged
* Purpose: Handles memory access violations (accessing bad memory)
* Registers: Banked SP (R13), LR (R14), and SPSR\_abt

Key Point: Used for memory fault handling like segmentation faults or page faults.

## 6. Undefined Mode (und)
* Privilege Level: Privileged
* Purpose: Handles illegal or unsupported instructions
* Registers: Banked SP (R13), LR (R14), and SPSR\_und

Key Point: Useful for emulation or capturing software bugs.

## 7. System Mode (sys)
* Privilege Level: Privileged
* Purpose: Runs privileged OS code, but without triggering exceptions
* Registers: Shares user-mode registers (no banking), full system access

Key Point: Allows the OS to run user-mode code with full access but no mode switch.

## 8. Monitor Mode (mon) (TrustZone only)
* Privilege Level: Privileged
* Purpose: Handles secure world operations in ARM TrustZone
* Registers: Own banked SP, LR, and SPSR\_mon

Key Point: Part of ARM's security extension to separate secure vs. non-secure execution.

## 8. Registers & Banked Registers


* 16 General Purpose Registers (R0–R15):
  * R13 = Stack Pointer (SP)
  * R14 = Link Register (LR)
  * R15 = Program Counter (PC)
* CPSR/SPSR: Current/Saved Program Status Registers (flags, mode, interrupts)
* Banked registers are special copies of registers that only exist in certain CPU modes (FIQ, IRQ, Supervisor). They allow fast context switching by avoiding the need to save/restore shared registers during exceptions.
* Banked Registers: FIQ, IRQ, SVC, etc., have private SP/LR/SPSR
  * Enables fast, efficient interrupt handling without saving all registers.
 

## What Are SPSR and CPSR?
* CPSR: Current Program Status Register — holds flags, mode bits, interrupt masks
* SPSR: Saved Program Status Register — each privileged mode has its own copy

  * When an exception occurs, CPSR is copied to SPSR
  * On returning from exception, SPSR is copied back to CPSR

## 9. Use Cases & Applications

* Mobile devices – ARM Cortex-A (Android, iOS)
* Embedded systems – ARM Cortex-M (IoT, medical devices)
* Automotive – ARM Cortex-R (ABS, ECU)
* Consumer electronics – TVs, cameras, appliances
* Networking & servers – Neoverse cores (cloud-native workloads)
* AI & Security – ARMv9 features TrustZone, SVE (Scalable Vector Extensions)
