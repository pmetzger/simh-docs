COPYRIGHT NOTICE

The following copyright notice applies to both the SIMH source and binary:

Original code published in 1993-2007, written by Robert M Supnik

Copyright (c) 1993-2016, Robert M Supnik

DEC PDP10/KA10 simulator written by Richard Cornwell

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL ROBERT M SUPNIK OR RICHARD CORNWELL BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of Robert M Supnik or Richard Cornwell shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from both Robert M Supnik and Richard Cornwell.

Table of Contents

[1. Introduction 3](#introduction)

[2. Simulator Files 4](#simulator-files)

[3. PDP6 Features 5](#pdp6-features)

[3.1 CPU 5](#cpu)

[3.2 Unit record I/O Devices 7](#unit-record-io-devices)

[3.2.1 Console Teletype (CTY) (Device 120) 7](#console-teletype-cty-device-120)

[3.2.2 Paper Tape Reader (PTR) (Device 104) 7](#paper-tape-reader-ptr-device-104)

[3.2.3 Paper Tape Punch (PTP) (Device 100) 7](#paper-tape-punch-ptp-device-100)

[3.2.4 Card Reader (CR) (Device 150) 7](#card-reader-cr-device-150)

[3.2.5 Card Punch (CP) (Device 110) 8](#card-punch-cp-device-110)

[3.2.6 Line Printer (LP) (Device 124) 8](#line-printer-lp-device-124)

[3.2.7 Type 340 Graphics display (Device 130) 9](#type-340-graphics-display-device-130)

[3.3 DCS Type 630 Terminal Multiplexer (Device 300) 9](#dcs-type-630-terminal-multiplexer-device-300)

[3.3.1 DCT Type 136 Data Control (Device 200/204) 9](#dct-type-136-data-control-device-200204)

[3.3.2 DTC Type 551 Dectape controller (Device 210) 9](#dtc-type-551-dectape-controller-device-210)

[3.3.3 MTC Type 516 Magtape controller (Device 220) 10](#mtc-type-516-magtape-controller-device-220)

[3.3.4 DSK Type 270 Disk controller (Device 270) 10](#dsk-type-270-disk-controller-device-270)

[4. Symbolic Display and Input 10](#symbolic-display-and-input)

#  Introduction

Originally the DEC PDP10 computer started as the PDP6. This was a 36 bit computer that was designed for timesharing, which was introduced in 1964. The original goal of the machine was to allow for processing of many 6 bit characters at a time. 36 bits were also common in machines like the IBM 7090, GE 645, and Univac 11xx lines. Several systems influenced the design of the PDP6, like CTSS, Lisp, support for larger memory. The PDP6 was canceled by DEC due to production problems. The engineers designed a smaller replacement, which happened to be a 36 bit computer that looked very much like the PDP6. This was called the PDP10. Later renamed to "DECSystem-10". The system supported up to 256K words of memory.

The first PDP10 was labeled KA10, and added a few instructions to the PDP6. Both the PDP6 and PDP10 used a base and limit relocation scheme. The KA10 generally offered two registers, one of user data and the second for user shared code. These were referred to the Low-Segment and High-Segment, the High-Segment could be shared with several jobs. The next version was called KI10 for Integrated. This added support for paging and double precision floating point instructions. It also added 4 sets of registers to improve context switching time. It could also support up to 4Mega words of memory. Following the KI10 was the KL10 (for Low-Cost). The KL10 added double precision integer instructions and instructions to improve COBOL performance. This was the first version which was microcoded. The KL10 was extended to support user programs larger then 256k. The final version to make it to market was the KS10 (for Small), this was a bit-slice version of the PDP10 which used UNIBUS devices which were cheaper then the KL10 devices.

The original operating system for the PDP6/PDP10 was just called "Monitor". It was designed to fit into 6K words. Around the 3rd release Swapping was added. The 6'th release saw the addition of virtual memory. Around the 4'th release it was given the name "TOPS-10". Around this time BBN was working on a paging system and implemented it on the PDP10. This was called "Tenex". This was later adopted by DEC and became "Tops-20".

During the mid 60's a group at MIT, who where not happy with how Multics was being developed decided to create their own operating system which they called Incompatible Timesharing System "ITS", was a play on the original project called Compatible Timesharing System "CTSS". CTSS was implemented by MIT on their IBM 7090 as an experimental system that allowed multiple time sharing users to co-exist on the same machine running batch processing. Hence the term Compatable.

Also during the mid 60's a group at Stanford Artificial Intelligence Laboratory (SAIL), started with a version of TOPS-10 and heavily modified it to become WAITS.

During the 70's Tymshare starting with DEC TOPS-10 system modified it to support random access files, paging with working sets and spawn-able processes. This ran on the KI10, KL10 and KS10.

The PDP10 was ultimately replaced by the VAX.

#  Simulator Files

To compile the DEC 6 simulator, you must define USE_INT64 as part of the compilation command line.

|                    |             |                               |
|--------------------|-------------|-------------------------------|
| ***Subdirectory*** | ***File***  | ***Contains***                |
| **PDP10**          | kx10_defs.h | KA10 simulator definitions    |
|                    | kx10_cpu.c  | PDP6 CPU.                     |
|                    | kx10_sys.c  | PDP6 System interface         |
|                    | kx10_cty.c  | Console Terminal Interface.   |
|                    | kx10_pt.c   | Paper Tape Reader and Punch   |
|                    | kx10_cr.c   | Punch card reader.            |
|                    | kx10_cp.c   | Punch card punch.             |
|                    | ka10_dpy.c  | DEC 340 graphics interface.   |
|                    | pdp6_dct.c  | PDP6 Data Controller          |
|                    | pdp6_dtc.c  | PDP6 551 DECtape controller   |
|                    | pdp6_mtc.c  | PDP6 556 Magtape controller   |
|                    | pdp6_dsk.c  | PDP6 270 Disk controller      |
|                    | pdp6_dcs.c  | PDP7 630 Terminal multiplexer |

#  

#  PDP6 Features

The PDP6 simulator is configured as follows:

|                    |                               |
|--------------------|-------------------------------|
| **Device Name(s)** | **Simulates**                 |
| **CPU**            | PDP6 CPU with 256KW of memory |
| **CTY**            | Console TTY.                  |
| **PTP**            | Paper tape punch              |
| **PTR**            | Paper tape reader.            |
| **LPT**            | LP10 Line Printer             |
| **CR**             | CR10 Card reader.             |
| **CP**             | CP10 Card punch               |
| **DCT**            | Data Control Type 136         |
| **DTC**            | Type 551 Dectape controller   |
| **MTC**            | Type 516 Magtape controller   |
| **DSK**            | Type 270 disk controller      |
| **DCS**            | Type 630 Terminal Multiplexer |

##  CPU

The CPU options include setting memory size, and other options.

|                 |                                    |
|-----------------|------------------------------------|
| SET CPU 16K     | Sets memory to 16K                 |
| SET CPU 32K     | Sets memory to 32K                 |
| SET CPU 48K     | Sets memory to 48K                 |
| SET CPU 64K     | Sets memory to 64K                 |
| SET CPU 96K     | Sets memory to 96K                 |
| SET CPU 128K    | Sets memory to 128K                |
| SET CPU 256K    | Sets memory to 256K                |
| SET CPU NOMAOFF | Sets traps to default of 040       |
| SET CPU MAOFF   | Move trap vectors from 040 to 0140 |
| SET CPU NOIDLE  | Disables Idle detection            |
| SET CPU IDLE    | Enables Idle detection             |

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

| ***Name*** | ***Size*** | ***Comments***             |
|------------|------------|----------------------------|
| PC         | 18         | Program Counter            |
| FLAGS      | 18         | Flags                      |
| FM0-FM17   | 36         | Fast Memory                |
| SW         | 36         | Console SW Register        |
| MI         | 36         | Memory Indicators          |
| AS         | 18         | Console AS register        |
| ABRK       | 1          | Address break              |
| ACOND      | 5          | Address Condition switches |
| PIR        | 8          | Priority Interrupt Request |
| PIH        | 8          | Priority Interrupt Hold    |
| PIE        | 8          | Priority Interrupt Enable  |
| PIENB      | 1          | Enable Priority System     |
| BYF5       | 1          | Byte Flag                  |
| UUO        | 1          | UUO Cycle                  |
| PL         | 18         | Program Limit Low          |
| RL         | 18         | Program Relation Low       |
| PUSHOVER   | 1          | Push overflow flag         |
| MEMPROT    | 1          | Memory protection flag     |
| NXM        | 1          | Non-existing memory access |
| CLK        | 1          | Clock interrupt            |
| OV         | 1          | Overflow enable            |
| PCCHG      | 1          | PC Change interrupt        |
| APRIRQ     | 3          | APR Interrupt number       |

The CPU can maintain a history of the most recently executed instructions.

This is controlled by the SET CPU HISTORY and SHOW CPU HISTORY commands:

|                    |                                      |
|--------------------|--------------------------------------|
| SET CPU HISTORY    | clear history buffer                 |
| SET CPU HISTORY=0  | disable history                      |
| SET CPU HISTORY=n  | enable history, length = n           |
| SHOW CPU HISTORY   | print CPU history                    |
| SHOW CPU HISTORY=n | print first n entries of CPU history |

Instruction tracing shows the Program counter, the contents of AC selected, the computed effective address. AR is generally the contents the memory location referenced by EA. RES is the result of the instruction. FLAGS shows the flags after the instruction is executed. IR shows the instruction being executed.

##  Unit record I/O Devices

###  Console Teletype (CTY) (Device 120)

The console station allows for communications with the operating system.

|            |                                     |
|------------|-------------------------------------|
| SET CTY 7B | 7 bit characters, space parity.     |
| SET CTY 8B | 8 bit characters, space parity.     |
| SET CTY 7P | 7 bit characters, space parity.     |
| SET CTY UC | Translate lower case to upper case. |

The CTY also supports a method for stopping TOPS10 operating system.

|              |                           |
|--------------|---------------------------|
| SET CTY STOP | Deposit 1 in location 30. |

###  Paper Tape Reader (PTR) (Device 104)

Reads a paper tape from a disk file.

###  Paper Tape Punch (PTP) (Device 100)

Punches a paper tape to a disk file.

###  Card Reader (CR) (Device 150)

The card reader (CR) reads data from a disk file. Card reader files can either be text (one character per column) or column binary (two characters per column). The file type can be specified with a set command:

|                         |                                     |
|-------------------------|-------------------------------------|
| SET CR*n* FORMAT=TEXT   | Sets ASCII text mode                |
| SET CR*n* FORMAT=BINARY | Sets for binary card images.        |
| SET CR*n* FORMAT=BCD    | Sets for BCD records.               |
| SET CR*n* FORMAT=CBN    | Sets for column binary BCD records. |
| SET CR*n* FORMAT=AUTO   | Automatically determines format.    |

or in the ATTACH command:

|  |  |
|----|----|
| ATTACH CR*n* *\<file\>* | Attaches a file |
| ATTACH CR*n* -f *\<format\> \<file\>* | Attaches a file with the given format. |
| ATTACH CR*n* -s \<*file*\> | Added file onto current cards to read. |
| ATTACH CR*n* -e \<*file\>* | After file is read in, the reader will receive and end of file flag. |

###  Card Punch (CP) (Device 110)

The card reader (CP) writes data to a disk file. Cards are simulated as ASCII lines with terminating newlines. Card punch files can either be text (one character per column) or column binary (two characters per column). The file type can be specified with a set command:

|                      |                                     |
|----------------------|-------------------------------------|
| SET CP FORMAT=TEXT   | Sets ASCII text mode                |
| SET CP FORMAT=BINARY | Sets for binary card images.        |
| SET CP FORMAT=BCD    | Sets for BCD records.               |
| SET CP FORMAT=CBN    | Sets for column binary BCD records. |
| SET CP FORMAT=AUTO   | Automatically determines format.    |

or in the ATTACH command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| ATTACH CP \<file\>               | Attaches a file                        |
| ATTACH CP -f \<format\> \<file\> | Attaches a file with the given format. |

###  Line Printer (LP) (Device 124)

The line printer (LP) writes data to a disk file as ASCII text with terminating newlines. Currently set to handle standard signals to control paper advance.

|                |                                              |
|----------------|----------------------------------------------|
| SET LP*n* LC   | Allow printer to print lower case            |
| SET LP*n* UC   | Print only upper case                        |
| SET LP*n* UTF8 | Print control characters as UTF8 characters. |

The first character of the line controls skipping.

|           |                         |
|-----------|-------------------------|
| Character | Effect                  |
| 014       | Skip to top of form.    |
| 013       | Skip mod 20 lines.      |
| 020       | Skip to next even line. |
| 021       | Skip to third line.     |
| 022       | Skip one line.          |
| 023       | Skip mod 10 lines.      |

###  Type 340 Graphics display (Device 130)

By default this device is not enabled. When enabled and commands are sent to it a graphics windows will display.

##  DCS Type 630 Terminal Multiplexer (Device 300)

Terminal multiplexers must be attached in order to work. The ATTACH command specifies the port to be used for Telnet sessions:

|                              |                       |
|------------------------------|-----------------------|
| ATTACH \<device\> *\<port\>* | set up listening port |

where port is a decimal number between 1 and 65535 that is not being used other TCP/IP activities.

Once attached and the simulator is running, the multiplexer listens for connections on the specified port. It assumes that any incoming connection is a Telnet connections. The connections remain open until disconnected either by the Telnet client, a SET \<device\> DISCONNECT command,or a DETACH \<device\> command.

|                               |                     |
|-------------------------------|---------------------|
| SET \<device\> DISCONNECT=*n* | Disconnect line *n* |

The \<device\> implements the following special SHOW commands

|  |  |
|----|----|
| SHOW \<device\> CONNECTIONS | Displays current connections to the \<device\> |
| SHOW \<device\> STATISTICS | Displays statistics for active connections |
| SHOW \<device\> LOG | Display logging for all lines |

Logging can be controlled as follows:

|                                   |                                      |
|-----------------------------------|--------------------------------------|
| SET \<device\> LOG=*n*=*filename* | Log output of line *n* to *filename* |
| SET \<device\> NOLOG              | Disable logging and close log file   |

###  DCT Type 136 Data Control (Device 200/204)

This device acted as a data multiplexer for the Dectape/Magtape and Disk. Individual devices could be assigned channels on this device. Devices which use the DCT include a DCT option with takes two octal digits, the first is the DCT device 0 or 1, the second is the channel 1 to 7.

###  DTC Type 551 Dectape controller (Device 210)

This was the standard Dectape controller for the PDP6. This controller loads the tape into memory and uses the buffered version. You need to specify which DCT unit and channel the tape is connected to.

|                |                                                  |
|----------------|--------------------------------------------------|
| SET DTC DCT=uc | Sets the DTC to connect to DCT unit and channel. |

Each individual tape drive support several options:

|                        |                                                  |
|------------------------|--------------------------------------------------|
| SET DTC*n* LOCKED      | Sets the mag tape to be read only.               |
| SET DTC*n* WRITEENABLE | Sets the mag tape to be writable.                |
| SET DTC*n* 18b         | Default, tapes are considered to be 18bit tapes. |
| SET DTC*n* 16b         | Tapes are converted from 16 bit to 18bit.        |
| SET DTC*n* 12b         | Tapes are converted from 12 bit to 18bit.        |

###  MTC Type 516 Magtape controller (Device 220)

This was the standard Magtape controller for the PDP6. This device handled only 7 track tapes. The simulator has the option to automatically translate 8 track tapes into 7 track tapes. You need to specify which DCT unit and channel the tape is connected to.

|                |                                                  |
|----------------|--------------------------------------------------|
| SET MTC DCT=uc | Sets the MTC to connect to DCT unit and channel. |

Each individual tape drive support several options:

|  |  |
|----|----|
| SET MTC*n* LOCKED | Sets this unit to be read only. |
| SET MTC*n* WRITEENABLE | Sets this unit to be writable. |
| SET MTC*n* 9T | Sets this unit to simulated 9 track format. |
| SET MTC*n* 7T | Sets this unit to read/write 7 track tapes (with parity) |

###  DSK Type 270 Disk controller (Device 270)

The 270 disk could support up to 4 units. The controller had to be connected to a type 136 Data Controller. You need to specify which DCT unit and channel the disk is connected to.

|                |                                                  |
|----------------|--------------------------------------------------|
| SET DSK DCT=uc | Sets the DSK to connect to DCT unit and channel. |

Each individual disk drive support several options:

|                        |                                 |
|------------------------|---------------------------------|
| SET DSK*n* LOCKED      | Sets this unit to be read only. |
| SET DSK*n* WRITEENABLE | Sets this unit to be writable.  |

#  Symbolic Display and Input

The KA10 simulator implements symbolic display and input. These are controlled by the following switches to the EXAMINE and DEPOSIT commands:

|     |                                      |
|-----|--------------------------------------|
| -a  | Display/Enter ASCII data.            |
| -p  | Display/Enter packed ASCII data.     |
| -c  | Display/Enter Sixbit Character data. |
| -m  | Display/Enter Symbolic instructions  |
|     | Display/Enter Octal data.            |

Symbolic instructions can be of the formats:

- Opcode ac,operand

- Opcode operands

- I/O Opcode device,address

Operands can be one or more of the following in order:

- Optional @ for indirection.

- \+ or – to set sign.

- Octal number

- Optional (ac) for indexing.

Breakpoints can be set at real memory address. The PDP10 supports 3 types of breakpoints. Execution, Memory Access and Memory Modify. The default is execution. Adding -R to the breakpoint command will stop the simulator on access to that memory location, either via fetch, indirection or operand access. Adding -w will stop the simulator when the location is modified.

The simulator can load RIM files, SAV files, EXE files, Waits Octal DMP files, MIT SBLK files.

When instruction history is enabled, the history trace shows internal status of various registers at the start of the instruction execution.

|  |  |
|----|----|
| PC | Shows the PC of the instruction executed. |
| AC | The contents of the referenced AC. |
| EA | The final computed Effective address. |
| AR | Generally the operand that was computed. |
| RES | The AR register after the instruction was complete. |
| FLAGS | The values of the FLAGS register before execution of the instruction. |
| IR | The fetched instruction followed by the disassembled instruction. |
