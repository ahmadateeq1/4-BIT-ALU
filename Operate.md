# Operating Instructions: 4-Bit MOSFET ALU & 4x4 Array Multiplier

This project contains two completely separate hardware modules operating within the same simulation. 

The **4x4 Array Multiplier** is a pure combinational logic web that requires no clocking. The **4-Bit ALU** is a stateful system that requires precise register management and clock pulses. 

---

## Module 1: The 4x4 Array Multiplier (Combinational)
The multiplier operates independently of the ALU's control bus and registers. Because it is purely combinational, there is no master clock, no memory, and no reset sequence required. Data flows directly from the input switches to the output probes.

### How to Operate:
1. **Set Input A:** Toggle the dedicated logic switches for Multiplier Input A ($A_3, A_2, A_1, A_0$).
2. **Set Input B:** Toggle the dedicated logic switches for Multiplier Input B ($B_3, B_2, B_1, B_0$).
3. **Read the Output:** Allow a brief moment for the signal to cascade through the 16-gate AND matrix and ripple through the three 4-bit adder blocks. The final 8-bit product is immediately displayed on the output probes ($P_7$ down to $P_0$).

### Example Workflow: 15 x 15 = 225 (Hardware Stress Test)
1. **SET A:** Toggle Input A to `1111` (15).
2. **SET B:** Toggle Input B to `1111` (15).
3. **READ:** Wait for the carry logic to propagate. The final output bus will display `11100001` (225).

---

## Module 2: The 4-Bit MOSFET ALU (Stateful)
Unlike the multiplier, the ALU uses a latched register for its first operand. To ensure accurate calculations, you must follow this strict procedural flow, **starting with a complete system reset before every new operation.**

### ⚠️ Phase 0: The Reset Phase (CRITICAL)
Before executing *any* new operation, you must clear the data bus, control lines, and **flush Register A**. Failing to do so will result in floating gates or residual data corrupting your next calculation.

1. **Clear Input Bus:** Set all switches on **Input B** ($B_3, B_2, B_1, B_0$) to `LOW` (0).
2. **Clear Opcodes:** Set `MODE_1`, `MODE_0`, and `SUB` switches to `01x` (Bitwise AND state) to clear out garbage values.
3. **Flush Register A:** With Input B at `0000`, trigger a single pulse on the **Master CLK** (toggle LOW $\rightarrow$ HIGH $\rightarrow$ LOW). This writes `0000` into Register A.
4. **Wait for Discharge:** Allow a few simulation frames for all transistor gates to fully discharge and the output bus to settle at `0`.

### Phase 1: Load Register A (Operand 1)
Since Input A is hardwired to a register, you must load your first number into it via the clock.

1. **Set Input Bus:** Toggle the logic switches for Input B to your first 4-bit binary value.
2. **Latch the Data:** Pulse the **Master CLK** (LOW $\rightarrow$ HIGH $\rightarrow$ LOW). 
3. Register A now holds your first operand. 

### Phase 2: Set Input B (Operand 2)
1. **Change Input Bus:** Toggle the logic switches for Input B to your second 4-bit binary value. 
*(Do NOT pulse the clock again, or you will overwrite Register A!)*

### Phase 3: Select the Operation
With Operand 1 latched in Register A and Operand 2 actively sitting on Input B, toggle the control switches according to this Opcode Table:

| `MODE_1` | `MODE_0` | `SUB` | Final Operation |
| :---: | :---: | :---: | :--- |
| 0 | 0 | 0 | **ADD** ($A + B$) |
| 0 | 0 | 1 | **SUBTRACT** ($A - B$) |
| 0 | 1 | X | **Bitwise AND** |
| 1 | 0 | X | **Bitwise OR** |
| 1 | 1 | X | **Bitwise XOR** |

*(Note: "X" means Don't Care. The state of the SUB switch does not matter for logical operations.)*

### Phase 4: Read the Output
1. **Allow for Propagation Delay:** Give Proteus a second to stabilize the discrete logic cascades.
2. **Read the Result:** The final calculation will be displayed on the 4-bit output probes/LEDs ($O_3, O_2, O_1, O_0$). 
3. **Carry/Borrow:** Check the Carry-Out ($C_{out}$) probe for arithmetic overflow during ADD/SUB operations.

### Example Workflow: 6 + 3 = 9
1. **RESET:** Set Input B to `0000` and pulse Master CLK to clear Register A.
2. **LOAD A (6):** Set Input B to `0110` (6). Pulse Master CLK. (Register A now holds 6).
3. **SET B (3):** Change Input B to `0011` (3).
4. **EXECUTE:** Set `MODE_1`=0, `MODE_0`=0, `SUB`=0. 
5. **READ:** Wait for signals to propagate. The ALU Output bus ($O_3-O_0$) will display `1001` (9).
6. **RESET:** Return all switches to `0` and pulse the clock to flush the system before the next calculation.
