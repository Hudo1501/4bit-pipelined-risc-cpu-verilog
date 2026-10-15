# 4-bit 3-Stage Pipelined RISC CPU (Verilog HDL)

![Language](https://img.shields.io/badge/language-Verilog-blue)
![Architecture](https://img.shields.io/badge/architecture-Harvard-green)
![Pipeline](https://img.shields.io/badge/pipeline-3--stage-orange)
![ISA](https://img.shields.io/badge/ISA-12--bit-purple)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

RTL design and verification of a **4-bit, 3-stage pipelined RISC CPU** with a **Harvard architecture** and a **12-bit instruction set**, written in Verilog HDL.

> **Note:** Items marked `TODO` must be filled in with details from your own design before publishing.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Key Features](#2-key-features)
3. [Architecture](#3-architecture)
4. [Instruction Set Architecture (ISA)](#4-instruction-set-architecture-isa)
5. [Pipeline Operation](#5-pipeline-operation)
6. [Hazard Handling](#6-hazard-handling)
7. [Repository Structure](#7-repository-structure)
8. [Module Descriptions](#8-module-descriptions)
9. [Getting Started](#9-getting-started)
10. [Verification](#10-verification)
11. [Results](#11-results)
12. [Example Program](#12-example-program)
13. [Limitations and Future Work](#13-limitations-and-future-work)
14. [Tools and Environment](#14-tools-and-environment)
15. [Author](#15-author)
16. [License](#16-license)

---

## 1. Overview

This project implements a small educational RISC processor from scratch in Verilog. It demonstrates the core ideas of modern CPU design in a compact form:

- A **4-bit datapath** (registers, ALU, and data memory words are 4 bits wide).
- A **12-bit instruction word** stored in a separate instruction memory.
- A **3-stage pipeline** that overlaps instruction fetch, decode, and execution to raise throughput.
- A **Harvard architecture**, meaning instruction memory and data memory are separate, so an instruction fetch and a data access can happen in the same cycle.

The design is intended for learning computer architecture, RTL coding, and functional verification, and can be synthesized for an FPGA.

## 2. Key Features

| Feature | Description |
|---|---|
| Datapath width | 4 bits |
| Instruction width | 12 bits |
| Architecture | Harvard (separate instruction and data memories) |
| Pipeline depth | 3 stages: Fetch, Decode, Execute |
| Register file | `TODO` registers x 4 bits |
| Instruction memory | `TODO` words x 12 bits |
| Data memory | `TODO` words x 4 bits |
| ALU operations | `TODO` (e.g., ADD, SUB, AND, OR, XOR, shifts) |
| Hazard handling | `TODO` (forwarding / stall / flush) |
| Language | Verilog HDL |
| Verification | Self-checking testbench, waveform inspection |

## 3. Architecture

### 3.1 Block Diagram

Add your diagram to `docs/` and reference it here:

```
![CPU Block Diagram](docs/block_diagram.png)
```

### 3.2 High-Level Structure

```
        +---------+      +----------+      +-----------+
PC ---> | FETCH   | ---> | DECODE   | ---> | EXECUTE   | ---> Write-back
        | (IF)    |      | (ID)     |      | (EX)      |
        +---------+      +----------+      +-----------+
            |                 |                  |
       Instruction        Register            ALU / Data
         Memory             File               Memory
```

### 3.3 Harvard Architecture

Instructions and data live in separate memories with independent buses. The Fetch stage reads the instruction memory while the Execute stage may simultaneously access the data memory, which avoids the structural hazard found in single-memory (von Neumann) designs.

## 4. Instruction Set Architecture (ISA)

### 4.1 Instruction Format

Each instruction is 12 bits wide. Describe the exact field layout of your design here.

`TODO: replace with your real format. Example layout:`

```
 11      8 7      4 3      0
+---------+--------+--------+
| opcode  |  field |  field |
|  [11:8] | [7:4]  | [3:0]  |
+---------+--------+--------+
```

### 4.2 Instruction Table

`TODO: fill in every instruction your CPU supports.`

| Mnemonic | Opcode | Format | Operation | Notes |
|---|---|---|---|---|
| `TODO` | `TODO` | `TODO` | `TODO` | `TODO` |

Suggested groups to document: arithmetic and logic, data movement (load/store), immediate, branch/jump, and NOP/halt.

### 4.3 Registers

| Register | Width | Purpose |
|---|---|---|
| PC | `TODO` bits | Program counter |
| R0 to R`TODO` | 4 bits | General-purpose registers |
| Flags | `TODO` | e.g., Zero, Carry, Negative (if implemented) |

## 5. Pipeline Operation

### 5.1 Stage Responsibilities

| Stage | Name | Work performed |
|---|---|---|
| 1 | **Fetch (IF)** | Read the instruction at PC from instruction memory; compute next PC. |
| 2 | **Decode (ID)** | Decode the opcode, read source registers, generate control signals. |
| 3 | **Execute (EX)** | Run the ALU operation, access data memory, write the result back. |

`TODO: adjust if your write-back or memory access happens in a different stage.`

### 5.2 Pipeline Timing

Once the pipeline is full, one instruction completes per clock cycle (ideal CPI = 1).

```
Cycle:     1    2    3    4    5
Instr 1:   IF   ID   EX
Instr 2:        IF   ID   EX
Instr 3:             IF   ID   EX
Instr 4:                  IF   ID   EX
```

The first instruction needs 3 cycles to complete (pipeline latency); after that, throughput is one instruction per cycle.

## 6. Hazard Handling

Pipelining introduces hazards. Document how your design handles each one.

| Hazard type | Example | Solution in this design |
|---|---|---|
| Data (RAW) | An instruction reads a register written by the previous instruction | `TODO` (forwarding / stall / none) |
| Control | Branch or jump changes PC after later instructions were fetched | `TODO` (flush / delay slot / stall) |
| Structural | Two stages need the same resource | Avoided by the Harvard architecture |

## 7. Repository Structure

Recommended layout (create the folders as you add content):

```
4bit-pipelined-risc-cpu-verilog/
├── rtl/                 # Synthesizable Verilog source
│   ├── cpu_top.v
│   ├── alu.v
│   ├── register_file.v
│   ├── control_unit.v
│   ├── instruction_memory.v
│   ├── data_memory.v
│   └── pipeline_registers.v
├── tb/                  # Testbenches
│   ├── cpu_tb.v
│   └── alu_tb.v
├── programs/            # Machine code / .hex / .mem test programs
├── sim/                 # Simulation scripts (Makefile, .do, .tcl)
├── docs/                # Diagrams, waveforms, ISA notes
├── constraints/         # FPGA constraints (.xdc / .sdc), optional
├── .gitignore
├── LICENSE
└── README.md
```

`TODO: update file names to match your real files.`

## 8. Module Descriptions

| Module | File | Description |
|---|---|---|
| `cpu_top` | `rtl/cpu_top.v` | Top level; connects all stages and memories. |
| `alu` | `rtl/alu.v` | 4-bit arithmetic and logic unit. |
| `register_file` | `rtl/register_file.v` | General-purpose registers with read and write ports. |
| `control_unit` | `rtl/control_unit.v` | Decodes opcodes into control signals. |
| `instruction_memory` | `rtl/instruction_memory.v` | 12-bit-wide program memory (ROM). |
| `data_memory` | `rtl/data_memory.v` | 4-bit-wide data memory (RAM). |
| `pipeline_registers` | `rtl/pipeline_registers.v` | Registers between pipeline stages. |

`TODO: add or remove rows to match your design.`

## 9. Getting Started

### 9.1 Prerequisites

Any one of the following simulators:

- **Icarus Verilog** and **GTKWave** (free, open source)
- **ModelSim / Questa**
- **Vivado Simulator**

### 9.2 Clone the Repository

```bash
git clone https://github.com/Hudo1501/4bit-pipelined-risc-cpu-verilog.git
cd 4bit-pipelined-risc-cpu-verilog
```

### 9.3 Run a Simulation

**Icarus Verilog + GTKWave:**

```bash
iverilog -o sim/cpu_sim rtl/*.v tb/cpu_tb.v
vvp sim/cpu_sim
gtkwave sim/cpu_tb.vcd
```

**ModelSim / Questa:**

```tcl
vlib work
vlog rtl/*.v tb/cpu_tb.v
vsim -c cpu_tb -do "run -all; quit"
```

`TODO: adjust file names and commands to match your project.`

### 9.4 Load a Program

Place your machine code in `programs/` and load it into instruction memory using `$readmemh` or `$readmemb`:

```verilog
initial $readmemh("programs/test_program.hex", imem);
```

## 10. Verification

### 10.1 Strategy

| Level | What is tested | Method |
|---|---|---|
| Unit | ALU, register file, control unit | Directed testbenches |
| Integration | Full CPU running programs | Program-level testbench |
| Pipeline | Hazards, branches, back-to-back dependencies | Targeted instruction sequences |

### 10.2 Test Cases

`TODO: list your real test cases.`

| Test | Purpose | Result |
|---|---|---|
| ALU operations | Verify every ALU function and flag | `TODO` |
| Register read/write | Verify register file behavior | `TODO` |
| Load/store | Verify data memory access | `TODO` |
| Back-to-back dependency | Verify data hazard handling | `TODO` |
| Branch/jump | Verify control hazard handling | `TODO` |

### 10.3 Expected Output

Show a representative console log or pass/fail summary here, for example:

```
[PASS] ALU ADD test
[PASS] Register file test
...
All tests passed.
```

## 11. Results

### 11.1 Simulation Waveforms

```
![Waveform](docs/waveform.png)
```

Explain what the waveform shows (instructions flowing through IF, ID, EX).

### 11.2 FPGA Synthesis (optional)

`TODO: fill in if you synthesized the design.`

| Metric | Value |
|---|---|
| Target device | `TODO` |
| LUTs | `TODO` |
| Registers | `TODO` |
| Max frequency (Fmax) | `TODO` MHz |
| Tool and version | `TODO` |

## 12. Example Program

Show a short program with its machine code and expected result so readers can try the CPU quickly.

`TODO: replace with a real example.`

```
Address  Machine code  Assembly        Comment
0x00     xxx            ...             Load first operand
0x01     xxx            ...             Load second operand
0x02     xxx            ...             Add
0x03     xxx            ...             Store result
```

## 13. Limitations and Future Work

- `TODO: list real limitations, e.g., small address space, 4-bit arithmetic range, no interrupts.`
- Possible extensions:
  - Add full data forwarding and branch prediction
  - Extend the ISA (more ALU operations, subroutine calls)
  - Add an assembler script that converts assembly to `.hex`
  - Increase datapath width (8-bit or 16-bit)
  - Add automated regression tests and CI with GitHub Actions

## 14. Tools and Environment

| Item | Version |
|---|---|
| HDL | Verilog (`TODO`: Verilog-2001 / SystemVerilog) |
| Simulator | `TODO` |
| Synthesis tool | `TODO` |
| OS | `TODO` |

## 15. Author

**Truong Dong Hung (Donghung)**
Software Engineering (Semiconductor & Automotive Software), FPT University Ho Chi Minh City
Vice-President, Integrated Circuit Design (ICD) Club

GitHub: [@Hudo1501](https://github.com/Hudo1501)

## 16. License

This project is released under the MIT License. See the [LICENSE](LICENSE) file for details.
