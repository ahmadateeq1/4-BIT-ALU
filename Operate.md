# Operating Instructions: 4-Bit MOSFET ALU

To ensure accurate calculations, you must follow this strict procedural flow, **starting with a complete system reset before every new operation.**

---

## ⚠️ Phase 0: The Reset Phase (CRITICAL)
Before executing *any* new operation, you must clear the data bus, control lines, and **flush Register A**. Failing to do so will result in floating gates or residual data corrupting your next calculation.

1. **Clear Input Bus:** Set all switches on **Input B** ($B_3, B_2, B_1, B_0$) to `LOW` (0).
2. **Clear Opcodes:** Set `MODE_1`, `MODE_0`, and `SUB` switches to `01X` (BITWISE AND) (0).
3. **Flush Register A:** With Input B at `0000`, trigger a single pulse on the **Master CLK** (toggle LOW $\rightarrow$ HIGH $\rightarrow$ LOW). This writes `0000` into Register A.
4. **Wait for Discharge:** Allow a few simulation frames for all transistor gates to fully discharge and the output bus to settle at `0000`.

---

## Phase 1: Load Register A (Operand 1)
Since Input A is hardwired to a register, you must load your first number into it via the clock.

1. **Set Input B:** Toggle the logic switches for Input B to your first 4-bit binary value.
2. **Latch the Data:** Pulse the **Master CLK** (LOW $\rightarrow$ HIGH $\rightarrow$ LOW). 
3. Register A now holds your first operand. 

---

## Phase 2: Set Input B (Operand 2)
1. **Change Input B:** Toggle the logic switches for Input B to your second 4-bit binary value. 
*(Do NOT pulse the clock again, or you will overwrite Register A!)*

---

## Phase 3: Select the Operation
With Operand 1 latched in Register A and Operand 2 actively sitting on Input B, assert the correct opcode to route the data through the discrete logic blocks.

Toggle the control switches according to this Opcode Table:

| `MODE_1` | `MODE_0` | `SUB` | Final Operation |
| :---: | :---: | :---: | :--- |
| 0 | 0 | 0 | **ADD** ($A + B$) |
| 0 | 0 | 1 | **SUBTRACT** ($A - B$) |
| 0 | 1 | X | **Bitwise AND** |
| 1 | 0 | X | **Bitwise OR** |
| 1 | 1 | X | **Bitwise XOR** |

*(Note: "X" means Don't Care. The state of the SUB switch does not matter for logical operations.)*

---

## Phase 4: Read the Output
1. **Allow for Propagation Delay:** Because this is a transistor-level simulation, there is a realistic propagation delay as the logic cascades through the MOSFET layers and the ripple-carry adder. Give Proteus a second to stabilize.
2. **Read the Result:** The final calculation will be displayed on the 4-bit output probes/LEDs ($O_3, O_2, O_1, O_0$). 
3. **Carry/Borrow:** Check the Carry-Out ($C_{out}$) probe for arithmetic overflow during ADD/SUB operations.

---

### Example Workflow: $6 + 3 = 9$
1. **RESET:** Set Input B to '0' and Opcodes to `01X`. Pulse Master CLK to clear Register A.
2. **LOAD A (6):** Set Input B to `0110` (6). Pulse Master CLK. (Register A now holds 6).
3. **SET B (3):** Change Input B to `0011` (3).
4. **EXECUTE:** Set `MODE_1`=0, `MODE_0`=0, `SUB`=0. 
5. **READ:** Wait for signals to propagate. Output bus will display `1001` (9).
6. **RESET:** Return all switches to `0` and pulse the clock to flush the system before the next calculation.
