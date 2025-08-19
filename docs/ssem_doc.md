# SSEM Simulator Usage

Revised: 14-May-2011

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

# Table of Contents

[1 Simulator Files 3](#simulator-files)

[2 SSEM Features 3](#ssem-features)

[2.1 CPU 3](#cpu)

[3 Symbolic Display and Input 4](#symbolic-display-and-input)

This memorandum documents the SSEM simulator.

# Simulator Files

| directory   | files           |
|-------------|-----------------|
| `sim/`      | `scp.h`         |
|             | `sim_console.h` |
|             | `sim_defs.h`    |
|             | `sim_fio.h`     |
|             | `sim_rev.h`     |
|             | `sim_sock.h`    |
|             | `sim_timer.h`   |
|             | `sim_tmxr.h`    |
|             | `scp.c`         |
|             | `sim_console.c` |
|             | `sim_fio.c`     |
|             | `sim_sock.c`    |
|             | `sim_timer.c`   |
|             | `sim_tmxr.c`    |
|             |                 |
| `sim/ssem/` | `ssem_defs.h`   |
|             | `ssem_cpu.c`    |
|             | `ssem_sys.c`    |

# SSEM Features

The SSEM is configured as follows:

| device names | simulates                        |
|--------------|----------------------------------|
| CPU          | SSEM CPU with 32 words of memory |

The `LOAD` and `DUMP` commands are implemented. They use a binary file
with the extension `.st` (“store”).

## CPU

The CPU implements Manchester SSEM (Small Scale Experimental Machine):

Memory size is fixed at 32 words. Memory is 32b wide.

CPU registers include the visible state of the processor system.

| name | size | comments            |
|------|------|---------------------|
| CI   | 5    | current instruction |
| A    | 32   | accumulator         |

# Symbolic Display and Input

The SSEM simulator implements symbolic display and input. Display is
controlled by command line switches:

| switch | description                                               |
|--------|-----------------------------------------------------------|
| `-h`   | display as hexadecimal                                    |
| `-d`   | display as decimal                                        |
| `-b`   | display as backwards binary. Least Significant Bit first. |
| `-m`   | display instruction mnemonics                             |

Input parsing is controlled by the first character typed in or by
command line switches:

|        |                      |
|--------|----------------------|
| opcode | instruction mnemonic |

There is only one instruction format:

|    |         |
|----|---------|
| op | address |

Instructions follow the mnemonic style used in the 1998 competition
reference manual:

"The Manchester University Small Scale Experimental Machine
Programmer's Reference manual"

<http://www.computer50.org/mark1/prog98/ssemref.html>

'op' is one of the following `JMP`, `JRP`, `LDN`, `STO`, `SUB`, `CMP`,
or `STOP`.

A linear address is one decimal number between `0` and `31`. For example:

```
sim> d -m 31 LDN 21
sim> ex -m 31
31: LDN 21
sim> ex -h 31
31: 00004015
sim> ex -b 31
31: 10101000000000100000000000000000
sim> ex -d 31
31: 16405
```
