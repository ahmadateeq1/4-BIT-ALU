# 4-Bit Discrete MOSFET ALU & 4x4 Array Multiplier

![Simulation](https://img.shields.io/badge/Simulation-Proteus-blue)
![Architecture](https://img.shields.io/badge/Architecture-4--Bit-success)
![Logic](https://img.shields.io/badge/Logic-Transistor--Level-orange)

## Overview
A custom 4-bit Arithmetic Logic Unit (ALU) and purely combinational 4x4 Array Multiplier, designed and simulated from scratch in Proteus. Unlike standard educational ALUs that rely on pre-packaged 74-series integrated circuits, this architecture is built at the transistor level using **discrete MOSFETs**. 

This project demonstrates foundational digital design, bridging the gap between low-level semiconductor physics and high-level computational logic. It features a complete transistor-level ALU and a heavily expanded 4x4 multiplication matrix that scales quadratically and pushes the limits of analog SPICE simulation.

## Supported Operations
The system takes two 4-bit inputs (A and B) and executes the following operations:

* **Arithmetic:**
  * `ADD` (Addition)
  * `SUB` (Subtraction)
  * `MUL` (4x4 Combinational Multiplication outputting an 8-bit result)
* **Logical:**
  * `AND` (Bitwise AND)
  * `OR` (Bitwise OR)
  * `XOR` (Bitwise XOR)

## Circuit Architecture
* **Transistor-Level Logic:** Core logic gates (NAND, NOR, NOT, AND) are constructed entirely using discrete N-channel and P-channel MOSFET configurations.
* **Combinational Multiplier Array:** Multiplication is executed without a clock using a purely combinational web. It features a 16-gate discrete MOSFET AND matrix to generate partial products, seamlessly cascaded into a deep-ripple network of three 4-bit adder blocks.
* **Adder Design:** Custom full-adder blocks are cascaded for standard 4-bit arithmetic computation and heavily utilized in the multiplier's shift-and-add rows.
* **SPICE Optimization (The "Digital Wall"):** Simulating hundreds of discrete analog MOSFETs simultaneously typically causes "Singular Matrix" and `SPICESIM.DLL` access violation crashes in Proteus. To bypass the analog math overload, this architecture implements a strategic buffering layer. Digital logic buffers isolate the MOSFET partial-product matrix from the adder arrays, severing the SPICE matrix, forcing perfect digital states, and ensuring extreme engine stability even during maximum-load calculations like a 15 x 15 (`1111 x 1111`) stress test.

## Prerequisites
To view and simulate this project, you will need:
* **Proteus Design Suite** (Version 8.0 or higher recommended)

## How to Run the Simulation
1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/discrete-mosfet-alu.git](https://github.com/yourusername/discrete-mosfet-alu.git)
