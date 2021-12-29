# Chromite_Bug_Bounty_RegFile_Scoreboard_Bypass
Functional Verification of Register File, Scoreboard and Bypass Logic of Chromite core by Incore Semiconductors using CocoTb and uatg tests.

This repository has the python files that generate the assembly files to test the following modules: Register file, Scoreboard and Bypass logic in chromite core.

Register file: 
This module is a set of registers in the decode stage.
Chromite core maintains two register files: one for integer and one for floating-point registers. Each of which includes 32 registers. The integer register file has 2 read ports and 1 write port while the floating-point register file requires 3 read ports and 1 write port.
Our project focuses on testing RV32i, and hence has only 32 32-bit registers.

Scoreboard:
It implements a 32-bit register for each architectural register file. Scoreboard also maintains a lock bit along with a id field to compensate the non-uniform latencies in the ex-stage

Bypass logic:
This module basically bypasses the operands in the execution stage to feed the latest value of the operands to the functional units. If the appropriate value is unavailable, then the execution stage is supposed to be stalled untill it receives the proper values to be fed.


References:
1. Chromite core documentation
https://chromite.readthedocs.io/en/latest/chromite.html

2. UATG tests documentation by Incore semiconductors
https://uatg.readthedocs.io/en/stable/index.html
