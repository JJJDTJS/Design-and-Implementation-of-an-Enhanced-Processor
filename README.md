# **Design and Implementation of an Enhanced Processor**

## **Overview**

This project is part of **Lab 8** in our computer architecture course, where we are required to design and implement an **enhanced processor** in **Verilog**. The primary goal of this lab was to introduce new features such as an internal **program counter (PC)**, memory read/write capabilities, and additional arithmetic and logic instructions.

## **Processor Features**

### 🔹 **Instruction Execution & Control**

- The processor executes instructions stored in an external **16-bit wide, 256-word deep synchronous memory (SSRAM)**.
- The **instruction register (IR)** fetches and decodes instructions, while the **control finite state machine (FSM)** governs execution timing.

### 🔹 **Registers & Data Flow**

- **General-purpose registers**: r0r0 to r6r6.
- **Register r7r7 (Program Counter - PC)**: Automatically increments to fetch the next instruction.
- **ADDR & DOUT registers**: Handle memory read/write operations.

### 🔹 **Newly Implemented Instructions**

| **Instruction** | **Operation**                                           |
| --------------- | ------------------------------------------------------- |
| `ld rX, [rY]`   | Load value from memory at address stored in rY into rX. |
| `st rX, [rY]`   | Store value in rX into memory at address stored in rY.  |
| `and rX, Op2`   | Perform bitwise AND between rX and Op2.                 |
| `b{cond} Label` | Branch to `Label` if the condition is met.              |

### 🔹 **Arithmetic Logic Unit (ALU) Enhancements**

- Extended ALU capabilities to support **addition, subtraction, and bitwise AND operations**.
- Introduced **condition flags (C, N, Z)** to facilitate conditional branching.

------

## **Processor Architecture**

### **🔹 Processor Block Diagram**

The following block diagram represents our enhanced processor design:


![Processor Block Diagram](https://github.com/user-attachments/assets/2bd97f29-70cf-41d4-a0ef-a52427e003f7)


**Key components:**

1. **Register File**: Includes general-purpose registers and the program counter.
2. **Arithmetic Logic Unit (ALU)**: Supports `ADD`, `SUB`, and `AND` operations.
3. **Multiplexers**: Select between different data sources for execution.
4. **Control FSM**: Handles instruction execution sequencing.
5. **Memory Interface**: Allows interaction with external memory and peripherals.

------

## **Project Structure**

The project is structured as follows:

```
Assembler_sbasm/      # Assembler files for instruction encoding
part3.Verilog/        # Processor implementation (Part 3)
part4.Verilog/        # Processor implementation (Part 4)
part5.Verilog/        # Final implementation (Part 5)
    ├── flipflop.v        # Flip-flop register implementation
    ├── inst_mem.mif      # Instruction memory initialization file
    ├── inst_mem.v        # Instruction memory module
    ├── proc.v            # Main processor logic
    ├── part5.v           # Top-level design for final processor version
    ├── part5.qpf         # Quartus project file
    ├── part5.qsf         # Quartus settings file
    ├── part5.sdc         # Timing constraints file
    ├── sw_led.s          # Assembly test program
    ├── top.v             # Top-level processor module
ModelSim/             # ModelSim simulation files
sim/                  # Simulation setup
```

------

## **Implementation Details**

### **🔹 Program Counter (PC)**

- Implemented as an **internal counter (`r7`)**, replacing the external counter used in Lab 7.
- Automatically increments at each instruction cycle unless explicitly modified by a branch instruction.

### **🔹 Memory Read/Write Mechanism**

- The **ADDR register** is used to send addresses to external memory.
- The **DOUT register** holds data for write operations.
- The **W signal** enables writing to memory.

### **🔹 Instruction Fetch & Execution Timing**

| **Cycle** | **Operation**                                                |
| --------- | ------------------------------------------------------------ |
| **T0**    | PC copied to ADDR, memory read initiated, PC increments.     |
| **T1**    | Wait for memory response.                                    |
| **T2**    | Instruction fetched into IR.                                 |
| **T3**    | Instruction execution begins (Register fetch, ALU ops, etc.). |
| **T4-T5** | Execution completion (Memory access, register updates, etc.). |

------

## **Testing & Simulation**

### **🔹 ModelSim Verification**

- The **testbench** provided executes a sample program stored in `ModelSim` folder.
- The program reads switch values (`SW`) and displays them on LEDs.

### **🔹 FPGA Implementation (DE1-SoC Board)**

- The design was synthesized in **Quartus Prime**.
- Physical testing was conducted using **hardware switches & LEDs**.

------

## **Future Enhancements**

🔹 Implement **more complex instructions (multiplication, division, etc.)**
 🔹 Optimize **branch prediction and instruction pipelining**
 🔹 Extend memory support to **larger external memory devices**

------

## **How to Run**

### **🔹 Simulation**

1. Open **ModelSim** and load the Verilog files.
2. Compile and run the testbench (`testbench.v`).
3. Observe instruction execution in the waveform viewer.

### **🔹 FPGA Deployment**

1. Synthesize the design in **Quartus Prime**.
2. Program the **DE1-SoC board**.
3. Use **switches (SW)** to input data and observe results on **LEDs**.
