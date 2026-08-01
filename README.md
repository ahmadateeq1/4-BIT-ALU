# 4-Bit Discrete MOSFET ALU

![Simulation](https://img.shields.io/badge/Simulation-Proteus-blue)
![Architecture](https://img.shields.io/badge/Architecture-4--Bit-success)
![Logic](https://img.shields.io/badge/Logic-Transistor--Level-orange)

## Overview
A custom 4-bit Arithmetic Logic Unit (ALU) designed and simulated entirely from scratch in Proteus. Unlike standard educational ALUs that rely on pre-packaged 74-series integrated circuits, this architecture is built at the transistor level using **discrete MOSFETs**. 

This project demonstrates foundational digital design, bridging the gap between low-level semiconductor physics and high-level computational logic by building custom logic gates, adders, and multiplexers directly from transistors.

## Supported Operations
The ALU takes two 4-bit inputs (A and B) and performs five core operations based on the selected opcode:

* **Arithmetic:**
  * `ADD` (Addition)
  * `SUB` (Subtraction)
* **Logical:**
  * `AND` (Bitwise AND)
  * `OR` (Bitwise OR)
  * `XOR` (Bitwise XOR)

## Circuit Architecture
* **Transistor-Level Logic:** All core logic gates (NAND, NOR, NOT, etc.) are constructed using discrete N-channel and P-channel MOSFET configurations.
* **Adder Design:** Custom full-adder blocks cascaded for 4-bit arithmetic computation.
* **Control Unit:** An integrated control bus selects the active operation and routes the correct calculation to the final 4-bit output bus.
* **Buffering:** Output buffering is implemented to maintain signal integrity and ensure simulation stability across complex transistor networks.

## Prerequisites
To view and simulate this project, you will need:
* **Proteus Design Suite** (Version 8.0 or higher recommended)

## How to Run the Simulation
1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/discrete-mosfet-alu.git](https://github.com/yourusername/discrete-mosfet-alu.git)
