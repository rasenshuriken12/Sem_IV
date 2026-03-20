Mod 1: Fundamentals of Microprocessor, Arduino & Raspberry Pi

Q1) Provide a detailed functional block diagram of the 8051 Microcontroller and describe five
key features of it.

- *64KB Program Memory address space (ROM)
- 64KB Data Memory address space (RAM)
- 4K bytes of on-chip Program Memory
- 128 bytes of on-chip Data RAM
- On-chip clock oscillator
- *Two 16-bit timer/counters
- 32 bidirectional and individually addressable I/0 lines
- *Full duplex UART
- *6-source/5-vector interrupt structure with two priority levels

Q2) Compare and contrast the architecture of a Microprocessor and a Microcontroller. Include
a block diagram for each to illustrate your answer.

| Feature              | Microprocessor (MP)              | Microcontroller (MCU) |
| -------------------- | -------------------------------- | --------------------- |
| *Integration          | Only CPU                         | CPU + Memory + I/O    |
| RAM/ROM              | External                         | Built-in              |
| Speed                | Very high                        | Moderate              |
| *Power Consumption    | High                             | Very low              |
| Cost                 | Expensive (full system)          | Cheap                 |
| *Size                 | Large system                     | Compact               |
| OS requirement       | Usually required (Linux/Windows) | Often not required    |
| Complexity           | High (design + wiring)           | Low                   |
| *Boot Time            | Slow                             | Fast                  |
| Real-time capability | Poor                             | Excellent             |


Q3) Discuss the significance of the Address Bus, Data Bus, and the ALU in the execution of a program within a processor.

# 🧠 Big Picture (Execution Flow)

Every program execution follows this loop:

> **Fetch → Decode → Execute**

And these three components play key roles:

* **Address Bus → “Where to go?”**
* **Data Bus → “What to bring?”**
* **ALU → “What to do?”**

---

# 🧩 1️⃣ Address Bus – “The Navigator”

Function: Unidirectional path used by the CPU to specify memory locations or I/O ports to access

Significance:
- Width determines **maximum addressable memory** (16-bit = 64KB, 32-bit = 4GB)
- During program execution, the **Program Counter** (PC) places the address of the next instruction on the address bus
- Enables the CPU to **fetch instructions** from specific memory locations
- Essential for memory-mapped I/O operations

---

# 🧩 2️⃣ Data Bus – “The Carrier”

Function: Bidirectional path that carries actual data between processor, memory, and peripherals

Significance:
- Width defines the **processor's "bit-size"** (8-bit, 16-bit, 32-bit, 64-bit)
- Transfers **instruction opcodes** from memory to CPU during fetch cycle
- Carries **operands & results** during execution
- Enables data exchange with I/O devices
- Wider bus = more data transferred per cycle = better performance

---

# 🧩 3️⃣ ALU (Arithmetic Logic Unit) – “The Brain Worker”

Function: The mathematical brain that performs arithmetic and logical operations

Significance:
- Executes all arithmetic operations (ADD, SUB, MUL, DIV)
- Performs logical operations (AND, OR, XOR, NOT)
- Handles bit-shifting and rotation operations
- Sets status flags (Carry, Zero, Overflow, Sign) in PSW based on results


### ⚙️ During Execution

In **Execute stage**:

1. Data is fetched via data bus
2. ALU processes it
3. Result stored back (via data bus)


### 🔁 Step-by-Step Flow

### 1️⃣ Fetch Instruction

* Address Bus → points to instruction location
* Data Bus → brings instruction to CPU

---

### 2️⃣ Decode

* Control Unit understands operation → ADD

---

### 3️⃣ Fetch Data

* Address Bus → points to A and B
* Data Bus → transfers values

---

### 4️⃣ Execute

* ALU performs: A + B

---

### 5️⃣ Store Result

* Data Bus → sends result to memory
* Address Bus → specifies location of C

---


# 🧠 System Design Insight (This is where most fail)

### 🔥 Bottlenecks

* Narrow **address bus** → limited memory
* Narrow **data bus** → slow performance
* Weak **ALU** → poor computation

👉 Performance is not just CPU speed—it’s **bus + ALU coordination**

---

Mod 2: 8051 Microcontroller

Q1) What is the size of Internal RAM and ROM organization in the 8051 Microcontroller? Explain the register banks and special function registers (SFRs).

Internal Memory Organization:
1. ROM (Program Memory):
- Size: 4 KB (4096 bytes) on-chip ROM/EPROM
- Address Range: 0000H to 0FFFH (internal)
- Type: Non-volatile memory
- Purpose: Stores program code and constant data
- Expandable: Can access up to 64 KB external program memory

2. RAM (Data Memory):
- Size: 128 bytes on-chip RAM
- Address Range: 00H to 7FH
- Type: Volatile memory (lost on power-off)
- Purpose: Temporary data storage, variables, stack

## 128 Bytes RAM Structure:
```
Address Range    |    Usage
─────────────────┼─────────────────────────────────
00H - 1FH (32B)  | Register Banks (4 banks × 8 registers)
20H - 2FH (16B)  | Bit-addressable area (128 bits)
30H - 7FH (80B)  | General-purpose scratch pad / Stack
```

Register Banks:
The 8051 has 4 register banks (Bank 0 to Bank 3), each containing 8 registers (R0-R7):
```
Bank 0: R0-R7 at addresses 00H-07H  (Default after reset)
Bank 1: R0-R7 at addresses 08H-0FH
Bank 2: R0-R7 at addresses 10H-17H
Bank 3: R0-R7 at addresses 18H-1FH
```

Bank Selection:
- Controlled by RS1 and RS0 bits in PSW register
- Only one bank is active at a time
- Allows fast context switching in interrupt routines

PSW Bits for Bank Selection:
```
RS1 (PSW.4)  RS0 (PSW.3)  Selected Bank
     0            0         Bank 0 (Default)
     0            1         Bank 1
     1            0         Bank 2
     1            1         Bank 3
```

## Special Function Registers (SFRs):
SFRs are memory-mapped control registers located at addresses 80H to FFH. They control peripherals and CPU functions.

1. I/O Port

| Address | SFR |
| ------- | --- | 
| 80H     | P0  |
| 90H     | P1  |
| A0H     | P2  |
| B0H     | P3  |

2. Core CPU

| Address | SFR |
| ------- | --- | 
| 81H     | SP  |
| 82H     | DPL |
| 83H     | DPH |
| D0H     | PSW |
| E0H     | A   |
| F0H     | B   |

3. Power Control

| Address | SFR |
| ------- | --- | 
| 87H     | PCON |

4. Timer Control

| Address | SFR |
| ------- | --- | 
| 88H     | TCON |
| 89H     | TMOD |
| 8AH     | TL0 |
| 8BH     | TL1 |
| 8CH     | TH0 |
| 8DH     | TH1 |

5. Serial Communication

| Address | SFR |
| ------- | --- | 
| 98H     | SCON |
| 99H     | SBUF |

6. Interrupt SFRs

| Address | SFR |
| ------- | --- | 
| A8H     | IE |
| B8H     | IP |

---

Describe the internal Timer/Counter hardware of the 8051. Explain the functions of the TMOD and TCON registers in timer operations.

🔥 1️⃣ Internal Timer/Counter Hardware of 8051

The 8051 has:

2 timers/counters → Timer 0 and Timer 1

Each is 16-bit (split into two 8-bit registers)


Timer	High Byte	Low Byte

Timer 0	TH0	TL0
Timer 1	TH1	TL1


Two modes of operation:

⏱️ Timer Mode (C/T = 0)

Increments based on internal clock

Used for:

Delays

Time measurement



🔢 Counter Mode (C/T = 1)

Increments based on external pulses

Pins used:

T0 → P3.4

T1 → P3.5



Used for:

Counting events (people, pulses, signals)



---

⚙️ Internal Working (System View)

Inside each timer:

Increment logic (adder)

Control logic (start/stop)

Overflow detection


Clock source:

8051 divides oscillator by 12


👉 If crystal = 12 MHz:

Timer increments every 1 µs



---

🔁 Overflow Concept

When timer reaches max:

For 16-bit → FFFFH → 0000H

Overflow flag is set:

TF0 (Timer 0)

TF1 (Timer 1)



This is crucial for:

Interrupts

Delay loops



---

🔥 2️⃣ TMOD Register (Timer Mode Register)

Controls:

Mode selection

Timer/Counter selection

Gating


📍 Address: 89H


---

🧩 TMOD Structure

| GATE | C/T | M1 | M0 | GATE | C/T | M1 | M0 |
   T1                     T0


---

🔹 Bits Explanation

1. M1, M0 (Mode Selection)

M1	M0	Mode	Description

0	0	Mode 0	13-bit timer
0	1	Mode 1	16-bit timer
1	0	Mode 2	8-bit auto-reload
1	1	Mode 3	Split timer mode


💡 Most used:

Mode 1 → normal timing

Mode 2 → periodic interrupts



---

2. C/T (Counter/Timer Select)

Value	Meaning

0	Timer (internal clock)
1	Counter (external input)



---

3. GATE

Value	Meaning

0	Controlled by TRx only
1	Controlled by TRx + external interrupt pin


🧠 Translation:

> GATE = extra hardware control using INT pins




---

🧪 Example

MOV TMOD, #01H

👉 Timer 0:

Mode 1 (16-bit)

Timer mode

No gating



---

🔥 3️⃣ TCON Register (Timer Control Register)

Controls:

Start/Stop

Overflow flags

External interrupts


📍 Address: 88H
✅ Bit-addressable


---

🧩 TCON Structure

| TF1 | TR1 | TF0 | TR0 | IE1 | IT1 | IE0 | IT0 |


---

🔹 Timer Bits (Important)

🔸 TR0 / TR1 (Timer Run Control)

Bit	Meaning

TR0 = 1	Start Timer 0
TR0 = 0	Stop Timer 0



---

🔸 TF0 / TF1 (Overflow Flags)

Bit	Meaning

TF0 = 1	Timer 0 overflow
TF1 = 1	Timer 1 overflow


⚠️ Must be cleared manually (or by interrupt)


---

🧪 Example

SETB TR0   ; Start Timer 0

JNB TF0, $ ; Wait until overflow
CLR TF0


---

🔁 Full Timer Flow (Put it together)

Let’s say you want a delay:

Steps:

1. Configure mode



MOV TMOD, #01H

2. Load initial value



MOV TH0, #FC
MOV TL0, #66

3. Start timer



SETB TR0

4. Wait for overflow



JNB TF0, $

5. Stop & reset



CLR TR0
CLR TF0


---

🧠 Real System Thinking (This is what matters)

Timers are used for:

OS scheduling

PWM generation

Serial baud rate

Interrupt-driven systems


