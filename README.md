# Nand2Tetris Capstone: Bootable Hack Computer (HDL) & Two-Pass Python Assembler

Complete implementation of the Nand2Tetris Capstone System:
1. **Low-Level & Top-Level HDL Architecture (`hdl/`)**:
   - **01_gates**: `Not.hdl`, `And.hdl`, `Or.hdl`, `Xor.hdl`, `Mux.hdl`, `DMux.hdl`, `Not16.hdl`, `And16.hdl`, `Or16.hdl`, `Mux16.hdl`, `Or8Way.hdl`, `Mux4Way16.hdl`, `DMux4Way.hdl`
   - **02_arithmetic**: `HalfAdder.hdl`, `FullAdder.hdl`, `Add16.hdl`, `Inc16.hdl`, `ALU.hdl`
   - **03_sequential**: `Bit.hdl`, `Register.hdl`, `PC.hdl`
   - **05_computer**: `Memory.hdl`, `CPU.hdl`, `Computer.hdl`
2. **Two-Pass Hack Assembler & Cycle-Accurate Simulator (`assembler/hack_assembler.py`)**:
   - Seeds the symbol table with `R0`–`R15`, `SP`, `LCL`, `ARG`, `THIS`, `THAT`, `SCREEN` (`16384`), and `KBD` (`24576`).
   - **Pass 1**: Scans `(Label)` declarations and binds each to its target ROM instruction address without emitting code.
   - **Pass 2**: Resolves `@symbol` and `@value` A-instructions (allocating new user variables starting at `RAM[16]`), splits C-instructions on `=` and `;` into `dest`, `comp`, and `jump` mnemonics, encodes them into 16-bit machine words, and outputs `.hack` files.
   - **Built-in Simulator**: Executes `.hack` machine code on a cycle-accurate behavioral model of `Computer.hdl` (`CPU` + `ROM32K` + `Memory` with `Screen` & `Keyboard` I/O).
3. **Keyboard-Echo Monitor (`programs/KeyboardEcho.asm` & `programs/KeyboardEcho.hack`)**:
   - Continuously polls `KBD` (`RAM[24576]`), echoes the 16-bit binary key pattern to the top-left `SCREEN` status word (`RAM[16384]`), and dynamically fills/clears the screen echo bar (`RAM[16416..16895]`).

## Quick Start

```bash
# Assemble KeyboardEcho.asm -> KeyboardEcho.hack and run simulation + self-tests
python3 assembler/hack_assembler.py programs/KeyboardEcho.asm --self-test --simulate
```
