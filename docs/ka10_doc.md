# PDP-10 KA10 Processor Simulator User's Guide

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

The DEC PDP-10/KA10 simulator was written by Richard Cornwell.

# Table of Contents

[1. Introduction](#introduction)

[2. Simulator Files](#simulator-files)

[3. KA10 Features](#ka10-features)

[3.1 CPU](#cpu)

[3.2 Unit record I/O Devices](#unit-record-io-devices)

[3.2.1 Console Teletype (CTY) (Device 120)](#console-teletype-cty-device-120)

[3.2.2 Paper Tape Reader (PTR) (Device 104)](#paper-tape-reader-ptr-device-104)

[3.2.3 Paper Tape Punch (PTP) (Device 100)](#paper-tape-punch-ptp-device-100)

[3.2.4 Card Reader (CR) (Device 150)](#card-reader-cr-device-150)

[3.2.5 Card Punch (CP) (Device 110)](#card-punch-cp-device-110)

[3.2.6 Line Printer (LPT) (Device 124)](#line-printer-lpt-device-124)

[3.2.7 Type 340 Graphics display (DPY) (Device 130)](#type-340-graphics-display-dpy-device-130)

[3.2.8 Stanford Triple III Display (III) (Device 430)](#stanford-triple-iii-display-iii-device-430)

[3.2.9 DBK Stanford microswitch keyboard scanner (DKB) (Device 310)](#dbk-stanford-microswitch-keyboard-scanner-dkb-device-310)

[3.2.10 DK10 Timer Module (DK) (Device 070)](#dk10-timer-module-dk-device-070)

[3.2.11 TM10 Magnetic Tape (MTA) (Device 340,344)](#tm10-magnetic-tape-mta-device-340344)

[3.2.12 TD10 DECTape (DT) (Device 320,324)](#td10-dectape-dt-device-320324)

[3.2.13 PCLK Petit Calendar Clock (Device 730)](#pclk-petit-calendar-clock-device-730)

[3.2.14 PD DeCoriolis clock (Device 500)](#pd-decoriolis-clock-device-500)

[3.2.15 A/D Input Multiplexer (IMX) (Device 574)](#ad-input-multiplexer-imx-device-574)

[3.3 Disk I/O Devices](#disk-io-devices)

[3.3.1 FHA Disk Controller (Device 170)](#fha-disk-controller-device-170)

[3.3.2 DPA/DPB Disk Controller (Device 250/254)](#dpadpb-disk-controller-device-250254)

[3.3.3 PMP Disk Controller (Device 500/504)](#pmp-disk-controller-device-500504)

[3.3.4 Systems Concepts DC-10 Disk Controller (Device 610/614)](#systems-concepts-dc-10-disk-controller-device-610614)

[3.3.5 DDC10 Drum controller for TENEX](#ddc10-drum-controller-for-tenex)

[3.4 Massbus Devices](#massbus-devices)

[3.4.1 RP Disk drives](#rp-disk-drives)

[3.4.2 RS Disk drives](#rs-disk-drives)

[3.4.3 TU Tape drives.](#tu-tape-drives.)

[3.5 Terminal Multiplexer I/O Devices](#terminal-multiplexer-io-devices)

[3.5.1 DC10E Terminal Controller (Device 240)](#dc10e-terminal-controller-device-240)

[3.5.2 TK Knight kludge, TTY scanner (Device 0600)](#tk-knight-kludge-tty-scanner-device-0600)

[3.5.3 MTY Morton terminal multiplexer (Device 400)](#mty-morton-terminal-multiplexer-device-400)

[3.5.4 DPK DK-10 Datapoint kludge (Device 604)](#dpk-dk-10-datapoint-kludge-device-604)

[3.6 Network Devices.](#network-devices.)

[3.6.1 IMP Interface Message Processor (Device 460)](#imp-interface-message-processor-device-460)

[3.6.2 CH Chaosnet interface (Device 470)](#ch-chaosnet-interface-device-470)

[3.7 PDP6 Devices.](#pdp6-devices.)

[3.7.1 DCT Type 136 Data Control (Device 200/204)](#dct-type-136-data-control-device-200204)

[3.7.2 DTC Type 551 Dectape controller (Device 210)](#dtc-type-551-dectape-controller-device-210)

[3.7.3 MTC Type 516 Magtape controller (Device 220)](#mtc-type-516-magtape-controller-device-220)

[3.7.4 DSK Type 270 Disk controller (Device 270)](#dsk-type-270-disk-controller-device-270)

[3.7.5 DCS Type 630 Terminal Multiplexer (Device 300)](#dcs-type-630-terminal-multiplexer-device-300)

[4. Symbolic Display and Input](#symbolic-display-and-input)

[5. OS specific configurations](#os-specific-configurations)

[5.1 TOPS-10](#tops-10)

[5.2 ITS](#its)

[5.3 WAITS](#waits)

#  Introduction

Originally the DEC PDP-10 computer started as the PDP-6. This was a 36
bit computer that was designed for timesharing, which was introduced
in 1964. The original goal of the machine was to allow for processing
of many 6 bit characters at a time. 36 bits were also common in
machines like the IBM 7090, GE 645, and Univac 11xx lines. Several
systems influenced the design of the PDP-6, like CTSS, Lisp, support
for larger memory.

The PDP-6 was canceled by DEC due to production problems. The
engineers designed a smaller replacement, which happened to be a 36
bit computer that looked very much like the PDP-6. This was called the
PDP-10, later renamed to "DECSystem-10". The system supported up to
256K 36-bit words of memory.

The first PDP-10 was labeled the KA10, and added a few instructions to
the PDP-6. Both the PDP-6 and PDP-10 used a base and limit relocation
scheme. The KA10 generally offered two registers, one of user data and
the second for user shared code. These were referred to the
Low-Segment and High-Segment, the High-Segment could be shared with
several jobs.

The next version was called KI10 for Integrated. This added support
for paging and double precision floating point instructions. It also
added 4 sets of registers to improve context switching time. It could
also support up to 4Mega words of memory.

Following the KI10 was the KL10 (for Low-Cost). The KL10 added double
precision integer instructions and instructions to improve COBOL
performance. This was the first version which was microcoded. The KL10
was extended to support user programs larger then 256k.

The final version to make it to market was the KS10 (for Small). This
was a bit-slice version of the PDP-10 which used Unibus devices which
were cheaper then the KL10 devices.

The original operating system for the PDP-6/PDP-10 was just called
"Monitor". It was designed to fit into 6K words. Around the 3rd
release swapping was added. The sixth release saw the addition of
virtual memory. Around the fourth release it was given the name
"TOPS-10". Around this time BBN was working on a paging system and
implemented it on the PDP-10. This was called "TENEX". This was later
adopted by DEC and became "TOPS-20".

During the mid 1960s a group at MIT, who where not happy with how
Multics was being developed, decided to create their own operating
system, which they called Incompatible Timesharing System, or
"ITS". The name was a play on an earlier project called the Compatible
Timesharing System, or "CTSS". CTSS was implemented by MIT on their
IBM 7090 as an experimental system that allowed multiple time sharing
users to co-exist on the same machine running batch processing, hence
the term "Compatible".

Also during the mid 60's a group at Stanford Artificial Intelligence
Laboratory (SAIL), started with a version of TOPS-10 and heavily
modified it to become WAITS.

During the 1970s, Tymshare modified TOPS-10 to support random access
files, paging with working sets and spawnable processes. This ran on
the KI10, KL10, and KS10.

The PDP-10 was ultimately replaced by the VAX.

#  Simulator Files

To compile the DEC PDP-10/KA10 simulator, you must define `USE_INT64`
as part of the compilation command line.

| Subdirectory | File | Contains |
|--------------|------|----------|
| `PDP10/`     |      |          |
|   | `kx10_defs.h`   | KA10 simulator definitions                    |
|   | `kx10_cpu.c`    | KA10 CPU.                                     |
|   | `kx10_df.c`     | DF10/C controller.                            |
|   | `kx10_rh.c`     | RH10 Controller.                              |
|   | `kx10_disk.h`   | Disk formatter definitions                    |
|   | `kx10_disk.c`   | Disk formatter routine.                       |
|   | `kx10_sys.c`    | KA10 System interface                         |
|   | `kx10_cty.c`    | Console Terminal Interface.                   |
|   | `kx10_pt.c`     | Paper Tape Reader and Punch                   |
|   | `kx10_rc.c`     | RC10 Disk controller, RD10 and RM10 Disks     |
|   | `kx10_dp.c`     | RP10 Disk controller, RP01/2/3 Disks.         |
|   | `kx10_rp.c`     | RH10 Disk controller, RP04/5/6 Disks.         |
|   | `kx10_rs.c`     | RH10 Disk controller RS04 Fixed head Disks.   |
|   | `kx10_dt.c`     | TD10 Dectape controller.                      |
|   | `kx10_mt.c`     | TM10A/B Magnetic tape controller.             |
|   | `kx10_tu.c`     | RH10/TM03 Magnetic tape controller.           |
|   | `kx10_lp.c`     | Line printer.                                 |
|   | `kx10_cr.c`     | Punch card reader.                            |
|   | `kx10_cp.c`     | Punch card punch.                             |
|   | `kx10_dk.c`     | DK10 interval timer.                          |
|   | `kx10_dc.c`     | DC10 communications controller.               |
|   | `kx10_ddc.c`    | DDC-10 Drum controller                        |
|   | `ka10_tk10.c`   | Knight kludge, TTY scanner                    |
|   | `ka10_mty.c`    | MTY, Morton terminal multiplexer              |
|   | `ka10_dpk.c`    | DK-10 Datapoint Kludge                        |
|   | `ka10_dpy.c`    | DEC 340 graphics interface.                   |
|   | `ka10_dkb.c`    | Stanford Microswitch Scanner for III display  |
|   | `ka10_iii.c`    | Stanford Triple III display.                  |
|   | `ka10_stk.c`    | Stanford keyboard for 340.                    |
|   | `ka10_pclk.c`   | Petit Calendar Clock.                         |
|   | `ka10_pd.c`     | DeCoriolis clock.                             |
|   | `ka10_imx.c`    | Analog input device.                          |
|   | `ka10_ten11.c`  | PDP-11 interface.                              |
|   | `kx10_lights.c` | Front panel interface.                        |
|   | `kx10_imp.c`    | IMP10 interface to ethernet.                  |
|   | `ka10_ch10.c`   | Chaosnet 10 interface.                        |
|   | `ka10_pmp.c`    | PMP Interface to IBM 3330 drives.             |
|   | `ka10_ai.c`     | System Concepts DC10 IBM2314 disk controller. |
|   | `pdp6_dct.c`    | PDP-6 Data Controller                         |
|   | `pdp6_dtc.c`    | PDP-6 551 DECtape controller                  |
|   | `pdp6_mtc.c`    | PDP-6 556 Magtape controller                  |
|   | `pdp6_dsk.c`    | PDP-6 270 Disk controller                     |
|   | `pdp6_dcs.c`    | PDP7 630 Terminal multiplexer                 |

#  KA10 Features

The KA10 simulator is configured as follows:

| Device Name(s)    | Simulates                                     |
|-------------------|-----------------------------------------------|
| `CPU`             | KA10 CPU with 256KW of memory                 |
| `CTY`             | Console TTY.                                  |
| `PTP`             | Paper tape punch                              |
| `PTR`             | Paper tape reader.                            |
| `LPT`             | LP10 Line Printer                             |
| `CR`              | CR10 Card reader.                             |
| `CP`              | CP10 Card punch                               |
| `MTA,DPA`         | TM10A/B Tape controller.                      |
| `DPB`             | RP10 Disk controller                          |
| `FSA,RPA,RPB,RPC` | RS04 Disk controller via RH10                 |
| `RPD`             | RH10 Disk controllers via RH10                |
| `PMP`             | PMP IBM 3330 disk controller                  |
| `AI`              | System Concepts DC10 IBM 2314 disk controller |
| `TUA`             | TM02 Tape controller via RH10                 |
| `FHA`             | RC10 Disk controller.                         |
| `DDC`             | DDC10 Disk controller.                        |
| `DT`              | TD10 Dectape Controller.                      |
| `DC`              | DC10 Communications controller.               |
| `DK`              | Clock timer module.                           |
| `PCLK`            | Petit Calendar Clock.                         |
| `PD`              | Coriolis Clock                                |
| `IMP`             | IMP network interface                         |
| `CH`              | CH10 Chaosnet interface                       |
| `IMX`             | A/D input multiplexer                         |
| `TK`              | Knight kludge, TTY scanner                    |
| `MTY`             | MTY, Morton terminal multiplexer              |
| `DPK`             | DK-10 Datapoint Kludge                        |
| `DKB`             | Stanford Microswitch Scanner.                 |
| `III`             | Stanford Triple III Display.                  |
| `TEN11`           | PDP-11 interface                              |
| `AUXCPU`          | PDP-6 interface                               |
| `DCT`             | PDP-6 Data Control Type 136                   |
| `DTC`             | PDP-6 Type 551 Dectape controller             |
| `MTC`             | PDP-6 Type 516 Magtape controller             |
| `DSK`             | PDP-6 Type 270 disk controller                |
| `DCS`             | PDP-6 Type 630 Terminal Multiplexer           |

##  CPU

The CPU options include setting memory size and O/S customization.

|                   |                                              |           |
|-------------------|----------------------------------------------|-----------|
| `SET CPU 16K`     | Sets memory to 16K                           |           |
| `SET CPU 32K`     | Sets memory to 32K                           |           |
| `SET CPU 48K`     | Sets memory to 48K                           |           |
| `SET CPU 64K`     | Sets memory to 64K                           |           |
| `SET CPU 96K`     | Sets memory to 96K                           |           |
| `SET CPU 128K`    | Sets memory to 128K                          |           |
| `SET CPU 256K`    | Sets memory to 256K                          |           |
| `SET CPU 512K`    | Sets memory to 512K                          | ITS & BBN |
| `SET CPU 768K`    | Set memory to 768K                           | ITS & BBN |
| `SET CPU 1024K`   | Set memory to 1024K                          | ITS & BBN |
| `SET CPU NOMAOFF` | Sets traps to default of 040                 |           |
| `SET CPU MAOFF`   | Move trap vectors from 040 to 0140           | WAITS     |
| `SET CPU ONESEG`  | Sets to one segment register                 |           |
| `SET CPU TWOSEG`  | Sets to 2 segment registers                  |           |
| `SET CPU ITS`     | Adds ITS Pager and instruction support to KA | ITS       |
| `SET CPU NOMPX`   | Disables MPX interrupt support for ITS       |           |
| `SET CPU MPX`     | Enables MPX interrupt support for ITS        | ITS       |
| `SET CPU WAITS`   | Add support for special WAITS instructions   | WAITS     |
| `SET CPU NOWAITS` | Disables special WAITS instructions.         |           |
| `SET CPU BBN`     | Enables BBN pager and TENEX support          | TENEX     |
| `SET CPU NOIDLE`  | Disables Idle detection                      |           |
| `SET CPU IDLE`    | Enables Idle detection                       |           |

CPU registers include the visible state of the processor as well as
the control registers for the interrupt system.

| Name          | Size | Comments                      | OS Type |
|---------------|-----:|-------------------------------|---------|
| `PC`          |   18 | Program Counter               |         |
| `FLAGS`       |   18 | Flags                         |         |
| `FM0-FM17`    |   36 | Fast Memory                   |         |
| `SW`          |   36 | Console SW Register           |         |
| `MI`          |   36 | Memory Indicators             |         |
| `AS`          |   18 | Console AS register           |         |
| `ABRK`        |    1 | Address break                 |         |
| `ACOND`       |    5 | Address Condition switches    |         |
| `PIR`         |    8 | Priority Interrupt Request    |         |
| `PIH`         |    8 | Priority Interrupt Hold       |         |
| `PIE`         |    8 | Priority Interrupt Enable     |         |
| `PIENB`       |    1 | Enable Priority System        |         |
| `BYF5`        |    1 | Byte Flag                     |         |
| `UUO`         |    1 | UUO Cycle                     |         |
| `PL`          |   18 | Program Limit Low             |         |
| `PH`          |   18 | Program Limit High            |         |
| `RL`          |   18 | Program Relation Low          |         |
| `RH`          |   18 | Program Relation High         |         |
| `PFLAG`       |    1 | Relocation enable             |         |
| `PUSHOVER`    |    1 | Push overflow flag            |         |
| `MEMPROT`     |    1 | Memory protection flag        |         |
| `NXM`         |    1 | Non-existing memory access    |         |
| `CLK`         |    1 | Clock interrupt               |         |
| `OV`          |    1 | Overflow enable               |         |
| `FOV`         |    1 | Floating overflow enable      |         |
| `APRIRQ`      |    3 | APR Interrupt number          |         |
| `PAGE_ENABLE` |    1 | Paging enabled                | ITS/BBN |
| `PAGE_FAULT`  |    1 | Page fault                    | ITS/BBN |
| `AC_STACK`    |   18 | AC Stack                      | ITS/BBN |
| `PAGE_RELOAD` |   18 | Page reload                   | ITS/BBN |
| `FAULT_DATA`  |   36 | Page fault data               | ITS/BBN |
| `TRP_FLG`     |    1 | Trap flag                     | ITS/BBN |
| `LAST_PAGE`   |    9 | Last page                     | ITS/BBN |
| `EXEC_MAP`    |    8 | Executive mapping             | BBN     |
| `NXT_WR`      |    1 | Map next write                | BBN     |
| `MON_BASE`    |    8 | Monitor base                  | BBN     |
| `USER_BASE`   |    8 | User base                     | BBN     |
| `USER_LIMIT`  |    3 | User limit                    | BBN     |
| `PER_USER`    |   36 | Per user data                 | BBN     |
| `DBR1`        |   18 | Paging control register 1     | ITS     |
| `DBR2`        |   18 | Paging control register 2     | ITS     |
| `DBR3`        |   18 | Paging control register 3     | ITS     |
| `JPC`         |   18 | Last Jump instruction address | ITS     |
| `AGE`         |    4 | Age                           | ITS     |
| `FAULT_ADDR`  |   18 | Fault address                 | ITS     |
| `OPC`         |   36 | Saved PC and flags            | ITS     |
| `MAR`         |   18 | Memory Access Register        | ITS     |
| `QUA_TIME`    |   36 | Quantum time.                 | ITS     |

The CPU can maintain a history of the most recently executed instructions.

This is controlled by the `SET CPU HISTORY` and `SHOW CPU HISTORY`
commands:

|                      |                                        |
|----------------------|----------------------------------------|
| `SET CPU HISTORY`    | clear history buffer                   |
| `SET CPU HISTORY=0`  | disable history                        |
| `SET CPU HISTORY=n`  | enable history, length = `n`           |
| `SHOW CPU HISTORY`   | print CPU history                      |
| `SHOW CPU HISTORY=n` | print first `n` entries of CPU history |

Instruction tracing shows the Program Counter, the contents of `AC`
selected, the computed effective address. `AR` is generally the
contents the memory location referenced by `EA`. `RES` is the result
of the instruction. `FLAGS` shows the flags after the instruction is
executed. `IR` shows the instruction being executed.

##  Unit Record I/O Devices

###  Console Teletype (`CTY`) (Device 120)

The console station allows for communications with the operating system.

|              |                                     |
|--------------|-------------------------------------|
| `SET CTY 7B` | 7 bit characters, space parity.     |
| `SET CTY 8B` | 8 bit characters, space parity.     |
| `SET CTY 7P` | 7 bit characters, space parity.     |
| `SET CTY UC` | Translate lower case to upper case. |

The `CTY` also supports a method for stopping TOPS-10 operating system.

|                |                           |
|----------------|---------------------------|
| `SET CTY STOP` | Deposit 1 in location 30. |

###  Paper Tape Reader (`PTR`) (Device 104)

Reads a paper tape from a disk file.

###  Paper Tape Punch (`PTP`) (Device 100)

Punches a paper tape to a disk file.

###  Card Reader (`CR`) (Device 150)

The card reader (`CR`) reads data from a disk file. Card reader files
can either be text (one character per column) or column binary (two
characters per column). The file type can be specified with a set
command:

|                         |                                     |
|-------------------------|-------------------------------------|
| `SET CRn FORMAT=TEXT`   | Sets ASCII text mode                |
| `SET CRn FORMAT=BINARY` | Sets for binary card images.        |
| `SET CRn FORMAT=BCD`    | Sets for BCD records.               |
| `SET CRn FORMAT=CBN`    | Sets for column binary BCD records. |
| `SET CRn FORMAT=AUTO`   | Automatically determines format.    |

or in the `ATTACH` command:

|                                 |                                        |
|---------------------------------|----------------------------------------|
| `ATTACH CRn <file>`             | Attaches a file                        |
| `ATTACH CRn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH CRn -s <file>`          | Added file onto current cards to read. |
| `ATTACH CRn -e <file>` | After file is read in, the reader will receive and end of file flag. |

###  Card Punch (`CP`) (Device 110)

The card reader (`CP`) writes data to a disk file. Cards are simulated
as ASCII lines with terminating newlines. Card punch files can either
be text (one character per column) or column binary (two characters
per column). The file type can be specified with a set command:

|                        |                                     |
|------------------------|-------------------------------------|
| `SET CP FORMAT=TEXT`   | Sets ASCII text mode                |
| `SET CP FORMAT=BINARY` | Sets for binary card images.        |
| `SET CP FORMAT=BCD`    | Sets for BCD records.               |
| `SET CP FORMAT=CBN`    | Sets for column binary BCD records. |
| `SET CP FORMAT=AUTO`   | Automatically determines format.    |

or in the `ATTACH` command:

|                                |                                        |
|--------------------------------|----------------------------------------|
| `ATTACH CP <file>`             | Attaches a file                        |
| `ATTACH CP -f <format> <file>` | Attaches a file with the given format. |

###  Line Printer (`LPT`) (Device 124)

The line printer (`LPT`) writes data to a disk file as ASCII text with
terminating newlines. Currently set to handle standard signals to
control paper advance.

|                 |                                              |
|-----------------|----------------------------------------------|
| `SET LPTn LC`   | Allow printer to print lower case            |
| `SET LPTn UC`   | Print only upper case                        |
| `SET LPTn UTF8` | Print control characters as UTF8 characters. |
| `SET LPTn LINESPERPAGE=n` | Sets the number of lines before an auto form feed is generated. There is an automatic margin of 6 lines. There is a maximum of 100 lines per page. |
| `SET LPTn DEV=n` | Sets device number to `n` (octal). |

These characters control the skipping of various number of lines.

| Character | Effect                                     |
|-----------|--------------------------------------------|
| `014`     | Skip to top of form.                       |
| `013`     | Skip mod 20 lines.                         |
| `020`     | Skip mod 30 lines.                         |
| `021`     | Skip to even line.                         |
| `022`     | Skip to every 3 line                       |
| `023`     | Same as line feed (12), but ignore margin. |

###  Type 340 Graphics Display (`DPY`) (Device 130)

Primarily useful under ITS, TOPS-10 does have some limited support for
this device. By default this device is not enabled. When enabled and
commands are sent to it a graphics windows will display.

###  Stanford Triple III Display (`III`) (Device 430)

Primarily useful under WAITS. By default this device is not
enabled. Used `DBK` device for keyboard input.

###  DBK Stanford Microswitch Keyboard Scanner (`DKB`) (Device 310)

Used by `III` to handle input. Currently only one keyboard is supported.

###  DK10 Timer Module (`DK`) (Device 070)

The DK10 timer module does not have any settable options.

###  TM10 Magnetic Tape (`MTA`) (Device 340,344)

The TM10 was the most common tape controller on KA10 and KI10. The
TM10 came in two models, the TM10A and the TM10B. The B model added a
DF10 which allowed the tape data to be transferred without
intervention of the CPU. The device has 2 options.

|                  |                                               |
|------------------|-----------------------------------------------|
| `SET MTA TYPE=t` | Sets the type of the controller to `A` or `B` |
| `SET MTA MPX=#`  | For ITS set the MPX interrupt to channel \#   |

Each individual tape drive supports several options: `MTA` is used as
an example.

|                        |                                           |
|------------------------|-------------------------------------------|
| `SET MTAn 7T`          | Sets the Mag tape unit to 7 track format. |
| `SET MTAn 9T`          | Sets the Mag tape unit to 9 track format. |
| `SET MTAn LOCKED`      | Sets the mag tape to be read only.        |
| `SET MTAn WRITEENABLE` | Sets the mag tape to be writable.         |

###  TD10 DECTape (`DT`) (Device 320,324)

The TD10 was the standard Dectape controller for KAs and KIs. This
controller loads the tape into memory and uses the buffered
version. For ITS you needed to connect it to an `MPX` channel to
handle I/O.

|                |                                             |
|----------------|---------------------------------------------|
| `SET DT MPX=#` | For ITS set the MPX interrupt to channel \# |

Each individual tape drive supports several options: `DT` is used as
an example.

|                       |                                                  |
|-----------------------|--------------------------------------------------|
| `SET DTn LOCKED`      | Sets the mag tape to be read only.               |
| `SET DTn WRITEENABLE` | Sets the mag tape to be writable.                |
| `SET DTn 18b`         | Default, tapes are considered to be 18bit tapes. |
| `SET DTn 16b`         | Tapes are converted from 16 bit to 18bit.        |
| `SET DTn 12b`         | Tapes are converted from 12 bit to 18bit.        |

###  `PCLK` Petit Calendar Clock (Device 730)

This device keeps track of time and date for WAITS. It was custom
designed by Peter Petit. The device supports two settings `ON` and
`OFF`. By default this device is disabled.

### `PD` DeCoriolis Clock (Device 500)

This is a device which keeps track of the time and date. An access
will return the number of ticks since the beginning of the year. There
are 60 ticks per second. The device was made by Paul DeCoriolis at
MIT. By default this device is disabled.

###  A/D Input Multiplexer (`IMX`) (Device 574)

Provides 128 channels of 12-bit analog to digital
converters. Currently only input from USB game controllers are
supported. To map game inputs to channels, use

```
SET IMX CHANNEL=123;UNIT0;AXIS1
```

Where `123` is the A/D channel in octal notation, `UNIT0` is first USB
game controller, and `AXIS1` is the second axis on that
controller. Add `;NEGATE` to invert the signal.

##  Disk I/O Devices

The PDP-10 supported many disk controllers.

###  `FHA` Disk Controller (Device 170)

The `RC10` disk controller used a `DF10` to transfer data to
memory. There were two types of disks that could be connected to the
`RC10`, the `RD10` and `RM10`. The `RD10` could hold up to 512K words
of data. The `RM10` could hold up to 345K words of data.

Each individual disk drive supports several options: `FHA` is used as
an example.

|                        |                                 |
|------------------------|---------------------------------|
| `SET FHAn RD10`        | Sets this unit to be an `RD10`. |
| `SET FHAn RM10`        | Sets this unit to be an `RM10`. |
| `SET FHAn LOCKED`      | Sets this unit to be read only. |
| `SET FHAn WRITEENABLE` | Sets this unit to be writable.  |

###  DPA/DPB Disk Controller (Device 250/254)

The `RP10` disk controller used a `DF10` to transfer data to
memory. There were three types of disks that could be connected to the
`RP10`, the `RP01`, `RP02`, `RP03`. The `RP01` held up to `1.2M`
words, the RP02 `5.2M` words, and the `RP03` 10M words. The second
controller is `DPB`. Disks can be stored in one of several file
formats, SIMH, `DBD9` and `DLD9`. The later two are for compatibility
with other simulators.

Each individual disk drive supports several options: `DPA` is used as
an example.

|                 |                                 |
|-----------------|---------------------------------|
| `SET DPAn RP01` | Sets this unit to be an `RP01`. |
| `SET DPAn RP02` | Sets this unit to be an `RP02`. |
| `SET DPAn RP03` | Sets this unit to be an `RP03`. |
| `SET DPAn HEADERS` | Enables the `RP10` to execute write headers function. |
| `SET DPAn NOHEADERS` | Prevents the `RP10` to execute write headers function. |
| `SET DPAn LOCKED`      | Sets this unit to be read only. |
| `SET DPAn WRITEENABLE` | Sets this unit to be writable.  |

To attach a disk use the `ATTACH` command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| `ATTACH DPAn <file>`             | Attaches a file                        |
| `ATTACH DPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH DPAn -n <file>`          | Creates a blank disk.                  |

Format can be `SIMH`, `DBD9`, `DLD9`.

###  `PMP` Disk Controller (Device 500/504)

The `PMP` disk controller allowed for IBM type drives to be connected
to the PDP-10. This is the controller used by WAITS. This devices is
disabled by default.

Each individual disk drive supports several options: `PMP` is used as
an example.

|                      |                                                       |
|----------------------|-------------------------------------------------------|
| `SET PMPn TYPE=type` | Sets this unit to be of type type. Generally 3330-2   |
| `SET PMP0 DEV=##`    | Sets the addresses of all disks to start at `##` hex. |
| `SET PMPn DEV=##`    | Sets this unit to be at address `##`.                 |

###  Systems Concepts DC-10 Disk Controller (Device 610/614)

The Systems Concepts DC-10 disk controller allowed IBM 2314 compatible
disk drives to be attached to the MIT AI Lab PDP-10 running ITS. This
device is disabled by default.

###  DDC10 Drum Controller for TENEX

DEC's RES-10 drum controller is used by TENEX for swapping. This device
is disabled by default.

##  Massbus Devices

Massbus devices are attached via RH10s. Devices start a device 270 and
go up (274, 360, 364, 370, 374). For TOPS-10 devices need to be in a the
order `RS`, `RP`, `TU`. By default `RS` disks are disabled. The first
unit which is not enabled will get device 270, units will be assigned
the next available address automatically. The device configuration
must match that which is defined in the OS.

###  `RP` Disk drives

Up to 4 strings of up to 8 `RP` drives can be configured. By default
the third and fourth controllers are disabled. These are addresses as
`RPA`, `RPB`, `RPC`, `RPD`. Disks can be stored in one of several file
formats, `SIMH`, `DBD9`, and `DLD9`. The later two are for
compatibility with other simulators.

|                        |                                            |
|------------------------|--------------------------------------------|
| `SET RPAn RP04`        | Sets this unit to be an `RP04` (19MWords)  |
| `SET RPAn RP06`        | Sets this unit to be an `RP06` (39MWords)  |
| `SET RPAn RP07`        | Sets this unit to be an `RP07` (110MWords) |
| `SET RPAn LOCKED`      | Sets this unit to be read only             |
| `SET RPAn WRITEENABLE` | Sets this unit to be writable              |

To attach a disk use the `ATTACH` command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| `ATTACH RPAn <file>`             | Attaches a file                        |
| `ATTACH RPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH RPAn -n <file>`          | Creates a blank disk.                  |

Format can be `SIMH`, `DBD9`, `DLD9`.

### RS Disk drives

One string of up to 8 `RS` drives can be configured. These drives are
fixed head swapping disks. By default they are disabled.

|                        |                                            |
|------------------------|--------------------------------------------|
| `SET FSAn RS03`        | Sets this unit to be an `RS03` (262KWords) |
| `SET FSAn RS04`        | Sets this unit to be an `RS04` (262KWords) |
| `SET FSAn LOCKED`      | Sets this unit to be read only             |
| `SET FSAn WRITEENABLE` | Sets this unit to be writable              |

###  `TU` Tape drives.

The `TUA` is a Mass bus tape controller using a TM03 formatter. There
can be one per RH10 and it can support up to 8 drives.

|                        |                                    |
|------------------------|------------------------------------|
| `SET TUAn LOCKED`      | Sets the mag tape to be read only. |
| `SET TUAn WRITEENABLE` | Sets the mag tape to be writable.  |

##  Terminal Multiplexer I/O Devices

All terminal multiplexers must be attached in order to work. The
`ATTACH` command specifies the port to be used for Telnet sessions:

|                          |                       |
|--------------------------|-----------------------|
| `ATTACH <device> <port>` | set up listening port |

where `port` is a decimal number between 1 and 65535 that is not being
used other TCP/IP activities.

Once attached and the simulator is running, the multiplexer listens
for connections on the specified port. It assumes that any incoming
connection is a Telnet connection. The connections remain open until
disconnected either by the Telnet client, a `SET <device> DISCONNECT`
command, or a `DETACH <device>` command.

|                             |                     |
|-----------------------------|---------------------|
| `SET <device> DISCONNECT=n` | Disconnect line `n` |

The `<device>` implements the following special `SHOW` commands:

|                             |                                                |
|-----------------------------|------------------------------------------------|
| `SHOW <device> CONNECTIONS` | Displays current connections to the `<device>` |
| `SHOW <device> STATISTICS`  | Displays statistics for active connections     |
| `SHOW <device> LOG`         | Display logging for all lines.                 |

Logging can be controlled as follows:

|                               |                                      |
|-------------------------------|--------------------------------------|
| `SET <device> LOG=n=filename` | Log output of line `n` to `filename` |
| `SET <device> NOLOG`          | Disable logging and close log file   |

###  DC10E Terminal Controller (Device 240)

The `DC` device was the standard terminal multiplexer for the KA10s
and KI10s. Lines came in groups of 8. For modem control there was a
second port for each line. These could be offset by any number of
groups.

|                  |                                                     |
|------------------|-----------------------------------------------------|
| `SET DC LINES=n` | Set the number of lines on the DC10, multiple of 8. |
| `SET DC MODEM=n` | Set the start of where the modem control DEC10E lines begins. |

###  `TK` Knight Kludge TTY Scanner (Device 0600)

This is a device with 16 terminal ports. It's specific to the MIT AI
Lab and Dynamic Modeling PDP-10s. By default this device is disabled.

###  `MTY` Morton Terminal Multiplexer (Device 400)

This is a device with 32 high-speed terminal lines. It's specific to
the MIT Mathlab and Dynamic Modeling PDP-10s. By default this device
is disabled.

###  `DPK` DK-10 Datapoint Kludge (Device 604)

The System Concepts DK-10, also known as the Datapoint Kludge is a
device with 16 terminal ports. This device is specific to ITS, and is
disabled by default.

##  Network Devices.

###  `IMP` Interface Message Processor (Device 460)

This allows the KA/KI to connect to the Internet. Currently only
supported under ITS. ITS and other OS that used the `IMP` did not
support modern protocols and typically required a complete rebuild to
change the IP address. Because of this the `IMP` processor includes
built in NAT and DHCP support. For ITS the system generated IP packets
which are translated to the local network. If the `HOST` is set to
`0.0.0.0` there will be no translation. If `HOST` is set to an IP
address then it will be mapped into the address set in `IP`. If `DHCP`
is enabled the `IMP` will issue a DHCP request at startup and set `IP`
to the address that is provided. DHCP is enabled by default.

|   |   |
|---|---|
| `SET IMP MAC=xx:xx:xx:xx:xx:xx` | Set the MAC address of the `IMP` to the value given. |
| `SET IMP IP=ddd.ddd.ddd.ddd/dd` | Set the external IP address of the `IMP` along with the net mask in bits. |
| `SET IMP GW=ddd.ddd.ddd.ddd` | Set the Gateway address for the `IMP`. |
| `SET IMP HOST=ddd.ddd.ddd.ddd` | Sets the IP address of the PDP-10 system. |
| `SET IMP DHCP` | Allows the `IMP` to acquire an IP address from the local network via DHCP. Only HOST must be set if this feature is used. |
| `SET IMP NODHCP` | Disables the `IMP` from making DHCP queries |
| `SET IMP ARP=ddd.ddd.ddd.ddd= xx:xx:xx:xx:xx:xx` | Creates a static ARP entry for the IP address with the indicated MAC address. |
| `SET IMP MIT` | Sets the `IMP` to look like the `IMP` used by MIT for ITS. |
| `SET IMP MPX=#` | For ITS set the MPX interrupt to channel `#` |
| `SET IMP BBN` | Sets the `IMP` to behave like generic BBN `IMP`. (Not implemented yet). |
| `SET IMP WAITS` | Sets the `IMP` for Stanford style `IMP` for WAITS. |
| `SHOW IMP ARP` | Displays the IP address to MAC address table. |

The `IMP` device must be attached to an available Ethernet
interface. To determine which devices are available use the `SHOW
ETHERNET` to list the potential interfaces. Check out the
`0readme_ethernet.txt` from the top of the source directory.

The `IMP` device can be configured in several ways. Ether it can
connect directly to an ethernet port (via `TAP`) or it can be
connected via a `TUN` interface. If configured via `TAP` interface the
`IMP` will behave like any other ethernet interface and if asked grab
it’s own address. In environments where this is not desired the `TUN`
interface can be used. When configured under a `TUN` interface the
simulated network is a collection of ports on the local host. These
can be mapped based on configuration options, see the
`0readme_ethernet.txt` file as to options.

With the `IMP` interface, the IP address of the simulated system is
static, and under ITS is configured into the system at compile
time. This address should be given to the `IMP` with the `SET IMP
HOST=<ip>”`, the `IMP` will direct all traffic it sees to this
address. If this address is not the same as the address of the system
as seen by the network, then this address can be set with `SET IMP
IP=<ip>`, and `SET IMP GW=<ip>`, or `SET IMP DHCP` which will allow
the `IMP` to request an address from a local DHCP server. The `IMP` will
translate the packets it receives/sends to look like the appeared from
the desired address. The `IMP` will also correctly translate FTP
requests in this configuration.

When running under a `TUN` interface, `IMP` is on a virtual `10.0.2.0`
network. The default gateway is `10.0.2.1`, with the default `IMP` at
`10.0.2.15`. For this mode DHCP can be used.

###  `CH` Chaosnet Interface (Device 470)

Chaos net was another methods of network access for ITS. Chaosnet can
be connected to another ITS system or a VAX/PDP-11 running BSD Unix or
VMS. Chaosnet runs over UDP protocol under UNIX. You must specify a
node number, peer to talk to and your local UDP listening port. All
UDP packets are sent to the peer for further processing.

|                                    |                                       |
|------------------------------------|---------------------------------------|
| `SET CH NODE=n`                    | Set the Node number for this system.  |
| `SET CH PEER=ddd.ddd.ddd.ddd:dddd` | Set the Peer address and port number. |

The `CH` device must be attached to a UDP port number. This is where
it will receive UDP packets from it’s peer.

##  PDP-6 Devices.

These devices could be attached to the KA10. Stanford WAITS used
them. Early versions of TOPS-10 could also run with these devices. By
default these devices are disabled.

###  `DCT` Type 136 Data Control (Device 200/204)

This device acted as a data multiplexer for the DECtapes/magtapes and
disks. Individual devices could be assigned channels on this
device. Devices which use the `DCT` include a `DCT` option with takes
two octal digits, the first is the `DCT` device 0 or 1, the second is
the channel 1 to 7.

###  `DTC` Type 551 DECtape controller (Device 210)

This was the standard DECtape controller for the PDP-6. This
controller loads the tape into memory and uses the buffered
version. You need to specify which `DCT` unit and channel the tape is
connected to.

|                  |                                                      |
|------------------|------------------------------------------------------|
| `SET DTC DCT=uc` | Sets the `DTC` to connect to `DCT` unit and channel. |

Each individual tape drive supports several options:

|                        |                                                  |
|------------------------|--------------------------------------------------|
| `SET DTCn LOCKED`      | Sets the mag tape to be read only.               |
| `SET DTCn WRITEENABLE` | Sets the mag tape to be writable.                |
| `SET DTCn 18b`         | Default, tapes are considered to be 18bit tapes. |
| `SET DTCn 16b`         | Tapes are converted from 16 bit to 18bit.        |
| `SET DTCn 12b`         | Tapes are converted from 12 bit to 18bit.        |

###  `MTC` Type 516 Magtape controller (Device 220)

This was the standard magtape controller for the PDP-6. This device
handled only 7 track tapes. The simulator has the option to
automatically translate 8 track tapes into 7 track tapes. You need to
specify which `DCT` unit and channel the tape is connected to.

|                  |                                                      |
|------------------|------------------------------------------------------|
| `SET MTC DCT=uc` | Sets the `MTC` to connect to `DCT` unit and channel. |

Each individual tape drive supports several options:

|                   |                                 |
|-------------------|---------------------------------|
| `SET MTCn LOCKED` | Sets this unit to be read only. |
| `SET MTCn WRITEENABLE` | Sets this unit to be writable. |
| `SET MTCn 9T` | Sets this unit to simulated 9 track format.              |
| `SET MTCn 7T` | Sets this unit to read/write 7 track tapes (with parity) |

###  `DSK` Type 270 Disk controller (Device 270)

The 270 disk controller could support up to 4 units. The controller
had to be connected to a type 136 Data Controller. You need to specify
which `DCT` unit and channel the disk is connected to.

|                  |                                                      |
|------------------|------------------------------------------------------|
| `SET DSK DCT=uc` | Sets the `DSK` to connect to `DCT` unit and channel. |

Each individual disk drive supports several options:

|                        |                                 |
|------------------------|---------------------------------|
| `SET DSKn LOCKED`      | Sets this unit to be read only. |
| `SET DSKn WRITEENABLE` | Sets this unit to be writable.  |

###  `DCS` Type 630 Terminal Multiplexer (Device 300)

See section on terminal multiplexers on generic setup.

#  Symbolic Display and Input

The KA10 simulator implements symbolic display and input. These are
controlled by the following switches to the `EXAMINE` and `DEPOSIT`
commands:


|      |                                      |
|------|--------------------------------------|
| `-v` | Looks up the address via translation,<br>will return nothing if address is not valid. |
| `-u` | With the `-v` option, used user space instead of<br>executive space. |
| `-a` | Display/Enter ASCII data.            |
| `-p` | Display/Enter packed ASCII data.     |
| `-c` | Display/Enter Sixbit Character data. |
| `-m` | Display/Enter Symbolic instructions  |
|      | Display/Enter Octal data.            |

Symbolic instructions can be of the formats:

- `opcode ac, operand`
- `opcode operands`
- `i/o_opcode device, address`

Operands can be one or more of the following in order:

- Optional `@` for indirection
- `+` or `-` to set sign
- Octal number
- Optional `(AC)` for indexing

Breakpoints can be set at real memory address. The PDP-10 supports
three types of breakpoints. Execution, Memory Access and Memory
Modify. The default is Execution. Adding `-R` to the breakpoint
command will stop the simulator on access to that memory location,
either via fetch, indirection or operand access. Adding `-w` will stop
the simulator when the location is modified.

The simulator can load `RIM` files, `SAV` files, `EXE` files, Waits
Octal `DMP` files, MIT `SBLK` files.

When instruction history is enabled, the history trace shows internal
status of various registers at the start of the instruction execution.

|       |                                                     |
|-------|-----------------------------------------------------|
| `PC`  | Shows the PC of the instruction executed.           |
| `AC`  | The contents of the referenced AC.                  |
| `EA`  | The final computed Effective address.               |
| `AR`  | Generally the operand that was computed.            |
| `RES` | The AR register after the instruction was complete. |
| `FLAGS` | The values of the FLAGS register before execution of the instruction. |
| `IR` | The fetched instruction followed by the disassembled instruction. |

The PDP-10 simulator allows for Memory reference and memory modify
breakpoints with the `-r` and `-w` options given to the break command.

#  OS specific configurations

##  TOPS-10

The default configuration supports TOPS-10, so there is no extra
configuration required.

##  ITS

To run ITS, the CPU must be `SET CPU ITS MPX 1024K`, this will enable
the ITS pager, special instructions, and the interrupt
multiplexer.

`DK` should be disabled, along with all Massbus devices.

The following devices should be enabled, `PD`, (also optionally)
`DPY`, `STK`, `TK`, `MTY`, `TEN11`, `AUXCPU`, `IMP`, `CH`.

ITS used a second interrupt multiplexer so some devices need to be
configured to specific channels. `TM10` (`MTA`) was at `MPX=7`, DECtape
(`DT`) was at `MPX=6`, the `IMP` was at `MPX=4`.

##  WAITS

WAITS has two configurations, one uses the default two relocation
registers. This can be done with `SET CPU WAITS MAOFF`. The PMP disk
is the only supported disk.

WAITS used some PDP-6 devices (`DTC`, `MTC`, `DCS`, `DTC`).

Also the following special devices for WAITS should be enabled (`DKB`,
`III`). The `DTC` should be configured to `DCT=02`, and the `MTC` should be
configured to `DCT=01`.

After 1975 Stanford added a BBN pager unit to their `KA`, so to run
later versions of WAITS, `SET CPU WAITS MAOFF BBN 512K`.

The “WAITS” flag enables the `FIX` and `XCTR` instructions for
WAITS. `MAOFF` moves the trap vectors to the 0140.

For systems numbers over 49, the BBN pager needs to also be enabled on
the CPU.
