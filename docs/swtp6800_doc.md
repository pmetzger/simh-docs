**SWTP 6800 Simulator Usage**

**6-June-2022**

COPYRIGHT NOTICES

The following copyright notice applies to the SIMH source, binary, and documentation:

Original code published in 1993-2008, written by Robert M Supnik

Copyright (c) 1993-2008, Robert M Supnik

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL ROBERT M SUPNIK BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of Robert M Supnik shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from Robert M Supnik.

The following copyright notice applies to the SWTP 6800 source, binary, and documentation:

Original code published in 2011, written by William A Beech

Copyright (c) 2011, William A Beech

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL WILLIAM A BEECH BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of William A Beech shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from William A Beech.

[1 Simulator Files 4](#simulator-files)

[2 SWTP 6800 Features 4](#swtp-6800-features)

[2.1 Motherboard 5](#motherboard)

[2.2 MP-A CPU Card 5](#mp-a-cpu-card)

[2.2.1 BOOTROM Device 6](#bootrom-device)

[2.2.2 CPU Device 7](#cpu-device)

[2.2.3 M6800 Registers 7](#m6800-registers)

[2.3 MP-A2 CPU Card 7](#mp-a2-cpu-card)

[2.3.1 BOOTROM Device 8](#bootrom-device-1)

[2.3.2 I2716 Device 9](#i2716-device)

[2.3.3 CPU Device 9](#cpu-device-1)

[2.3.4 M6800 Registers 9](#m6800-registers-1)

[2.4 Programmed I/O Devices 9](#programmed-io-devices)

[2.4.1 MP-S Serial I/O Board 9](#mp-s-serial-io-board)

[2.4.2 DC-4 Quad Double-Sided Single-/Double-Density 5-1/4” Floppy Disk Controller Board 10](#dc-4-quad-double-sided-single-double-density-5-14-floppy-disk-controller-board)

[2.4.3 LFD-400 Quad Single-Density 5-1/4” Floppy Disk Controller Board 10](#lfd-400-quad-single-density-5-14-floppy-disk-controller-board)

This memorandum documents the SWTP 6800 simulator.

# Simulator Files

sim/ scp.h

sim_console.h

sim_defs.h

sim_fio.h

sim_rev.h

sim_sock.h

sim_tape.h

sim_timer.h

sim_tmxr.h

scp.c

sim_console.c

sim_fio.c

sim_sock.c

sim_tape.c

sim_timer.c

sim_tmxr.c

sim/swtp6800/common bootrom.c boot ROM simulator

dc-4.c disk controller simulator

lfd-400.c disk controller simulator

i2716.c 2716 ROM simulator d

m6800.c m6800 CPU simulator

m6810.c m6810 RAM simulator

mp-8m.c 8K RAM board simulator

mp-a.c MP-A CPU board simulator

mp-a2.c MP-A2 CPU board simulator mp-b2.c MP-B2 Motherboard board simulator

mp-s.c serial port board simulator

sim/swtp6800/swtp6800 mp-a_sys.c system definitions for MP-A CPU board

mp-a2_sys.c system definitions for MP-A2 CPU board

swtp_defs.h system definitions for the SWTP 6800

Additional files are:

sim/swtp6800/swtp6800 swtbug.bin SWTBUG boot ROM code

swtp6800mp-a.ini Initialization for MP-A CPU

swtp6800mp-a2.ini Initialization for MP-A2 CPU

# SWTP 6800 Features

The SWTP 6800 simulator is configured as follows:

device names(s) simulates

m6800+ MP-A CPU with

m6810+ 128B of RAM and

bootrom 512 - 8192B of boot ROM

m6800+ MP-A2 CPU with

m6810+ 128B of RAM

bootrom+ 512 - 8192B of boot ROM

i2716 4 each 2716 EPROMS and

external RAM above 40K

Motherboard MP-B2 with 8 SS-30 plugs and 7 SS-50 plugs

MP-8M 6 each 8K byte memory board (0-7FFFH & A000-DFFFH)

DC-4 SS-30 5-1/4” Dual Floppy disk controller

LFD-400 SS-30 5-1/4” Dual Floppy disk controller

MP-S SS-30 Serial I/O Port

The simulator builds as two executable files, SWTP6800MP-A and SWTP6800MP-A2, one for each of the processor boards available.

Most devices can be disabled or enabled, by the commands:

SET \<dev\> DISABLED

SET \<dev\> ENABLED

The SWTP 6800 simulator implements several unique stop conditions:

- If an undefined instruction is decoded, a STOP_INST is set

- If an undefined memory or I/O address is selected and MTRAP is enabled, a STOP_INST is set

- If an undefined interrupt occurs and ITRAP is enabled, a STOP_INST is set

The LOAD command supports both S19 format and BIN format tapes. If the file extension is .S19, or the h switch is specified with LOAD, the file is assumed to be S19 format; if the file extension is .BIN, or the -b switch is specified, the file is assumed to be BIN format.

## Motherboard

The current simulator supports the MP-B2 motherboard. This board allows for inserting of the selected CPU, up to 6 MP-8M 8K byte memory boards, and one additional SS-50 board. It will allow the addition of up to 6 other SS-50 peripherals with the MP-S and DC-4.

Addresses are fixed for each of the 6 MP-8M boards as shown below:

Device Base address

bd0 0000H

bd1 2000H

bd2 4000H

bd3 6000H

bd4 0A000H

bd5 0C000H

The simulator allows each board to be enabled or disabled individually to simulate the presence or absence of a particular board. This is the standard layout of memory in a SWTP 6800.

If the LFD-400 FDC is enabled, then the bd5 MP-8M must be disabled. The LFD-400 used CC00-CC0FH for its I/O address space.

## MP-A CPU Card

The simulator for the SWTP 6800 MP-A uses several files. The simulator is depicted in Figure 1.

<img src="mdbook/src/swtp6800_doc_assets/image1.png" style="width:6.66875in;height:5.57361in" />

**Figure 1. MP-A Simulator**

The MP-A CPU has several available options.

### BOOTROM Device

The BOOTROM allows selection of the size of the ROM:

SET BOOTROM NONE No Boot PROM

SET BOOTROM 2704 0.5K PROM

SET BOOTROM 2708 1K PROM

SET BOOTROM 2716 2K PROM

SET BOOTROM 2732 4K PROM

SET BOOTROM 2764 8K PROM

The BOOTROM device assigns the base of the ROM image to 0E000H of simulated memory.

The BOOTROM image file is attached to the BOOTROM device as follows:

ATTACH BOOTROM SWTBUG.BIN

### CPU Device

The CPU device allows setting the simulated behavior for interrupts and references to unimplemented memory.

SET CPU ITRAP Trap interrupts

SET CPU NOITRAP Don’t trap interrupts

SET CPU MTRAP Trap unimplemented memory

SET CPU NOMTRAP Don’t trap unimplemented memory

SET CPU HISTORY Enable the collection of history

### M6800 Registers

The CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

name size comments

PC 16 program counter

SP 16 stack pointer

A 8 accumulator a

B 8 accumulator b

IX 16 index register

CCR 8 condition code register

The CPU display radix can be set for octal, decimal or hexadecimal. The commands are as follows:

SET CPU OCT

SET CPU DEC

SET CPU HEX

The current CPU display radix can be found with:

SHOW CPU RADIX

## MP-A2 CPU Card

The simulator for the SWTP 6800 MP-A uses several files. The simulator is depicted in Figure 2.

<img src="mdbook/src/swtp6800_doc_assets/image2.png" style="width:6.66875in;height:6.04722in" />

**Figure 2. MP-A2 Simulator**

The MP-A CPU has several available options.

### BOOTROM Device

The BOOTROM allows selection of the size of the ROM:

SET BOOTROM NONE No Boot PROM

SET BOOTROM 2704 0.5K PROM

SET BOOTROM 2708 1K PROM

SET BOOTROM 2716 2K PROM

SET BOOTROM 2732 4K PROM

SET BOOTROM 2764 8K PROM

The BOOTROM device assigns the base of the ROM image to 0E000H of simulated memory.

The BOOTROM image file is attached to the BOOTROM device as follows:

ATTACH BOOTROM SWTBUG.BIN

### I2716 Device

The i2716 device provides 4 units to simulate the 4 2716 ROM positions on the MP-A2 CPU board. They are i27160 to i27163.

The i2716 ROM image file is attached to one of the i2716 devices as follows:

ATTACH I27160 FILE0.BIN

### CPU Device

The CPU device allows setting the simulated behavior for interrupts and references to unimplemented memory.

SET CPU ITRAP Trap interrupts

SET CPU NOITRAP Don’t trap interrupts

SET CPU MTRAP Trap unimplemented memory

SET CPU NOMTRAP Don’t trap unimplemented memory

### M6800 Registers

The CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

name size comments

PC 16 program counter

SP 16 stack pointer

A 8 accumulator a

B 8 accumulator b

IX 16 index register

CCR 8 condition code register

The CPU display radix can be set for octal, decimal or hexadecimal. The commands are as follows:

SET CPU OCT

SET CPU DEC

SET CPU HEX

The current CPU display radix can be found with:

SHOW CPU RADIX

## Programmed I/O Devices

### MP-S Serial I/O Board

This driver simulates the MP-S serial I/O board for the console connection to the SWTP 6800. The console simulated is either an ANSI terminal or a Teletype Model 33 with paper tape reader and punch. The console functions work correctly but the paper tape functions do not. The simulator simulates the M6850 registers to the extent required to support the console.

Console mode can be set as follows:

SET MP-S ANSI

SET MP-S TTY

Current console status can be shown with the following command:

SHOW MP-S

The MP-S driver simulates the paper tape reader (PTR) and paper tape punch (PTP) devices. These devices need to be attached to files before use. If the file specified is not present, then a new file is created. The attach and detach commands are as follows:

ATTACH PTR TEST

ATTACH PTP TEST1

DETACH PTR

DETACH PTP

Current PTP and PTR status can be shown with the following commands:

SHOW PTP

SHOW PTR

### DC-4 Quad Double-Sided Single-/Double-Density 5-1/4” Floppy Disk Controller Board

This driver simulates the DC-4 floppy disk controller board. Normally this board connects to a dual drive DSDD 5-1/4” floppy system. In this emulation, I have provided for 4 drives, the maximum the WD1797 can support and the emulated drive images are also increased in size to 1.44 MB. FLEX can handle this size drive with no problems.

The DC-4 simulator provides for four drive units. The units are DC-40 to DC-43. These devices need to be attached to files before use. If the file specified is not present, then a new file is created. The units can be attached and detached to files as follows:

ATTACH DC-40 BOOT.IMG

DETACH DC-43

Current DC-4 status can be displayed with the following command:

SHOW DC-4

### LFD-400 Quad Single-Density 5-1/4” Floppy Disk Controller Board

This driver simulates the LFD-400 floppy disk controller board. Normally this board connects to a dual drive SSSD 5-1/4” floppy system. In this emulation, I have provided for 4 drives, the maximum the WD1791 can support.

The LDF-400 simulator provides for four drive units. The units are LDF-4000 to LDF-4003. The LDF-400 needs to be enabled before use as follows:

SET LDS-400 ENABLE

These devices need to be attached to files before use. If the file specified is not present, then a new file is created. The units can be attached and detached to files as follows:

ATTACH LDF-4000 BOOT.IMG

DETACH LDS-4002

Current DC-4 status can be displayed with the following command:

SHOW LDF-4000

### 
