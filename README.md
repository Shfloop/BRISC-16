# BRISC-16
BRISC-16 is a 16-bit 5-stage pipelined CPU im currently building. It uses a custom 16-bit Load/Store Architecture **[Instruction Set](#instruction-set)**<br> 


## Pipeline
  Very similar to a risc v 5 stage pipeline. This isnt the exact schematic but its close ![alt text](https://www.alrj.org/images/riscv/Pipeline_summary.png)
## Registers
  16 total registers 11 used in actual build.  
  
  Each register has an input bus connected to the write back stage and two output buses connected into 74hct574 at the decode stage for input into the alu. 
  
  -  `Data Registers` r0-r7 
  -  `Address Registers` r8-r14 - can be decremented or incremented by push and pop. 
  -  `Program Counter` r15 - sepereated from the other registers to allow for write back during the alu stage and for simultaneous fetch and memory operation. 
      -  during fetch to instruction memory, the program counter is directly connected to instruction memory until a banked load/store happens where the bank offset = 1, then the fetch is stalled 1 cycle and nop inserted 

## Memory
  2MB of total word addressed memory.
  -  0 - 0xffff - Data Memory
  -  0x10000 - 0x1ffff - Instruction Memory
  -  0x2000 - 0xfffff - extent of banked memory range can be used for anything

## Alu
  -  16-bit version of James Sharman's Alu but register register moves happen through it and flags have a seperate latch so they arent updated on every instruction.

## Write Back/Forward line
  how the cpu keeps track of what data needs to get written back and if a data forward needs to happen into the alu
  -  4 bit register write back value during the memory and write back stage is xnored with both the current LHS and RHS input register of the alu to see if the cpu is getting data from a register that hasn't been written back to yet. 
  
## Instruction Set
16 main instructions plus 15 alu functions 
- `Alu Operations`
  -  `NOP` 
  -  `ADD` 
  -  `SUB` 
  -  `MOVE` 
  -  `SHR`
  -  `SHL`
  -  `AND`
  -  `OR`
  -  `XOR`
  -  `NOT`
  -  `ADC`
  -  `SUBB`
-  `ADI`: rd += sign extended 8-bit immediate 
-  `LOAD/STORE` with offset:
   -  rd limited to first 8 registers
   -  rs2 limited to last 8 registers
   -  address = rs2 + sign extended 6-bit immediate
- `LOAD/STORE`, `PUSH/POP` with banked offset
  -  16 bit addesss is extended by 4 bits provided by the immediate
- `JZ` - Jump if zero flag
- `JNZ` - Jump if not zero flag
- `JC` - Jump if carry flag
- `JNC` - Jump if not carry flag
- `JO` - Jump if overflow flag
- `LUI` - Load Upper Immediate 
- `JUMP` - pc += 12 bit sign extended offset
- `JAL` - Jump and link with hardcoded return address register.
  
  ![alt text](Stuff/InstructionFormat.png) 
