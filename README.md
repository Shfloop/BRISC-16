# BRISC-16
BRISC-16 is a 16-bit 5-stage pipelined CPU im currently building. It uses a custom 16-bit Load/Store Architecture **[Instruction Set](#instruction-set)**<br> 



## Instruction Set
- `Alu rd += rs2`  opcode 0
  -  `ADD` rd += rs2
  -  `SUB` rd += !rs2 + 1
  -  `MOVE` rd = rs2 + 0
  -  `SHL`
  -  `SHR`
  -  `AND`
  -  `OR`
  -  `XOR`
  -  `NOT`
  -  `SUBB`
  -  `ADC`
-  `ADI`: rd += sign extended 8-bit immediate 
-  `LOAD/STORE` with offset:
   -  rd limited to first 8 registers
   -  rs2 limited to last 8 registers
   -  address = rs2 + sign extended 6-bit immediate
- `LOAD/STORE`, `PUSH/POP` with banked offset
  -  16 bit addesss is extended by 4 bits provided by the immediate to allow for 1 million addresses

### test
