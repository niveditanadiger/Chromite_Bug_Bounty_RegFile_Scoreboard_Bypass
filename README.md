# Chromite_Bug_Bounty_RegFile_Scoreboard_Bypass
Functional Verification of Register File, Scoreboard and Bypass Logic of Chromite core by [Incore Semiconductors](https://incoresemi.com/) using [CocoTb](https://docs.cocotb.org/en/stable/) and [UATG tests](https://uatg.readthedocs.io/en/stable/index.html).

This repository has the python files that generate the assembly files to test the following modules: Register file, Scoreboard and Bypass logic in chromite core.

**Register file:** 
This module is a set of registers in the decode stage.
Chromite core maintains two register files: one for integer and one for floating-point registers. Each of which includes 32 registers. The integer register file has 2 read ports and 1 write port while the floating-point register file requires 3 read ports and 1 write port.
Our project focuses on testing RV32i, and hence has only 32 32-bit registers.

**Scoreboard:**
It implements a 32-bit register for each architectural register file. Scoreboard also maintains a lock bit along with a id field to compensate the non-uniform latencies in the ex-stage

**Bypass logic:**
This module basically bypasses the operands in the execution stage to feed the latest value of the operands to the functional units. If the appropriate value is unavailable, then the execution stage is supposed to be stalled untill it receives the proper values to be fed.

The following python files test the above modules:

1.uatg_reg_files_r1.py

This test verifies if the register x0 gets updated for any non-zero value(since x0 is supposed to be hardwired to zero according to [RISCV ISA](https://riscv.org/)). If it does, then the ISR jumps to flag branch implying that the test fails. 

2.uatg_bypass_alu_alu.py

This test has two `raw` dependent consecutive alu instructions. It checks if the appropriate value is bypassed by the scoreboard for the calculation of error-free output. If the final value has an error, it jumps to the flag branch implying that the test fails.

3.uatg_bypass_mul_load_store.py

This test has a multiply instruction which might take multiple clock cycles in the EX-stage ([Mbox](https://gitlab.com/incoresemi/core-generators/chromite/-/tree/master/src/m_ext)), expecting the following instructions to be stalled if they are dependent on the output of `mul` instruction. If the following instruction interprets the incorrect value, then ISR jumps to flag branch.

4.uatg_bypass_regfiles.py

This test checks if the value being read from a register is correct if the same register is committed in the same cycle. According to [Hennessy and Patterson](http://home.ustc.edu.cn/~louwenqi/reference_books_tools/Computer%20Organization%20and%20Design%20RISC-V%20edition.pdf), the register is supposed to commit first and then read the same register, but this would generate alot of attributes leading to an inefficient design. To avoid this, chromite core has introduced a multiplexor to simply bypass the incoming `write_data` to the data that had to be read from the register file. This test writes the final output to the signature file.

5.uatg_bypass_mul_mul.py

This test checks the behaviour of the core if 2 consecutive instructions have multiply instructions. It verifies if the appropriate values are bypassed for `raw` dependencies for `mul` instructions.


--> Command to generate the assembly file:
      **uatg from-config -c mygenerators/uatg/config.ini -v debug**
 

References:
1. Chromite core documentation
https://chromite.readthedocs.io/en/latest/chromite.html

2. UATG tests documentation by Incore semiconductors
https://uatg.readthedocs.io/en/stable/index.html
