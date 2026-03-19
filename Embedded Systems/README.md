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



