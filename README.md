# Computation-Structure-Lab3
FPGA-based ALU implementation using Lucid on Alchitry, supporting arithmetic, boolean, comparison, and shift operations with FSM-based input handling.

## Features
✅ 32-bit ALU operations  
✅ Boolean logic, arithmetic, comparison, and shifting  
✅ FSM-based input handling  
✅ Implemented using **Lucid HDL** in **Alchitry Labs**  

## How to Use
1. Clone the repository:
   git clone https://github.com/KenLZY/Computation-Structure-Lab3.git

2. Open the project in **Alchitry Labs**.
3. Flash the code to the **Alchitry Au FPGA**.
4. Use **DIP switches & buttons** to select inputs and operations.

## ALU Operations
| Operation  | ALUFN Code | Description |
|------------|-----------|-------------|
| Addition   | `000000`  | A + B |
| Subtraction | `000001` | A - B |
| MUL        | `000010`    | A & B |
| AND         | `011000`    | A \| B |
| OR        | `011110`    | A ^ B |
| XOR | `010110`    | A << B |
| "A" | `011010`   | A >> B |
| SHL    | `100000`    | A == B |
| SHR  | `100001`    | A < B |
| SRA  | `100011`    | A < B |
| CMPEQ  | `110011`    | A < B |
| CMPLT  | `110101`    | A < B |
| CMPLE  | `110111`    | A < B |

## Files & Modules
- **alu.luc** – ALU core logic  
- **boolean.luc** – Boolean logic operations  
- **adder.luc** – Arithmetic operations  
- **shift.luc** – Shift operations  
- **compare.luc** – Comparison operations  
- **alu_manual_tester.luc** – Manual Tester 

## License
This project is licensed under the **MIT License**.


