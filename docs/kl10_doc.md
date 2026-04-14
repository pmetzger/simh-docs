# PDP-10 KL10 Processor Simulator User's Guide

Revision of 20-Apr-2022

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

The DEC PDP-10/KL10 simulator was written by Richard Cornwell.

# Table of Contents

[1. Introduction](#introduction)

[2. Simulator Files](#simulator-files)

[3. KL10 Features](#kl10-features)

[3.1 CPU](#cpu)

[3.2 PDP-11 front end processor (DTE 200)](#pdp-11-front-end-processor-dte-200)

[3.2.1 Console Teletype (CTY) (on DTE0)](#console-teletype-cty-on-dte0)

[3.2.2 LP20 printer on front end (LP20) (on DTE)](#lp20-printer-on-front-end-lp20-on-dte)

[3.2.3 Front end terminal connections (TTY)](#front-end-terminal-connections-tty)

[3.3 Unit record I/O Devices](#unit-record-io-devices)

[3.3.1 Paper Tape Reader (PTR) (Device 104)](#paper-tape-reader-ptr-device-104)

[3.3.2 Paper Tape Punch (PTP) (Device 100)](#paper-tape-punch-ptp-device-100)

[3.3.3 Card Reader (CR) (Device 150)](#card-reader-cr-device-150)

[3.3.4 Card Punch (CP) (Device 110)](#card-punch-cp-device-110)

[3.3.5 Line Printer (LPT) (Device 124)](#line-printer-lpt-device-124)

[3.3.6 TM10 Magnetic Tape (MTA) (Device 340,344)](#tm10-magnetic-tape-mta-device-340344)

[3.3.7 TD10 DECtape (DT) (Device 320,324)](#td10-dectape-dt-device-320324)

[3.3.8 PD DeCoriolis clock (Device 500)](#pd-decoriolis-clock-device-500)

[3.4 Disk I/O Devices](#disk-io-devices)

[3.4.1 DPA/DPB Disk Controller (Device 250/254)](#dpadpb-disk-controller-device-250254)

[3.5 Massbus Devices](#massbus-devices)

[3.5.1 RP Disk drives](#rp-disk-drives)

[3.5.2 RS Disk drives](#rs-disk-drives)

[3.5.3 TU Tape drives](#tu-tape-drives)

[3.6 Terminal Multiplexer I/O Devices](#terminal-multiplexer-io-devices)

[3.6.1 DC10E Terminal Controller (Device 240)](#dc10e-terminal-controller-device-240)

[3.6.2 Front end terminals (TTY)](#front-end-terminals-tty)

[3.7 Network Devices](#network-devices)

[3.7.1 NIA20 Network interface (NI) (Device 564)](#nia20-network-interface-ni-device-564)

[3.7.2 IMP Interface Message Processor (Device 460)](#imp-interface-message-processor-device-460)

[3.7.3 CH Chaosnet interface (Device 470)](#ch-chaosnet-interface-device-470)

[4. Symbolic Display and Input](#symbolic-display-and-input)

[5. OS specific configurations](#os-specific-configurations)

[5.1 TOPS-10](#tops-10)

[5.2 TOPS-20](#tops-20)

[5.3 ITS](#its)

# Introduction

Originally the DEC PDP-10 computer started as the PDP-6. This was a 36
bit computer that was designed for timesharing, which was introduced
in 1964. The original goal of the machine was to allow for processing
of many 6 bit characters at a time. 36 bits were also common in
machines like the IBM 7090, GE 645, and Univac 11xx lines. Several
influences shaped the design of the PDP-6, including CTSS, Lisp, and
support for larger memory.

The PDP-6 was canceled by DEC due to production problems. The
engineers designed a smaller replacement, which happened to be a 36
bit computer that looked very much like the PDP-6. This was called the
PDP-10, later renamed to "DECSystem-10". The system supported up to
256K 36-bit words of memory.

The first PDP-10 was labeled KA10, and added a few instructions to the
PDP-6. Both the PDP-6 and PDP-10 used a base and limit relocation
scheme. The KA10 generally offered two registers, one for user data
and the second for user shared code. These were referred to as the
Low-Segment and High-Segment. The High-Segment could be shared with
several jobs.

The next version was called KI10 for Integrated. This added support
for paging and double precision floating point instructions. It also
added 4 sets of registers to improve context switching time. It could
also support up to 4 megawords of memory.

Following the KI10 was the KL10 (for Low-Cost). The KL10 added double
precision integer instructions and instructions to improve COBOL
performance. This was the first version which was microcoded. The KL10
was extended to support user programs larger than 256K.

The final version to make it to market was the KS10 (for Small). This
was a bit-slice version of the PDP-10 which used Unibus devices which
were cheaper than the KL10 devices.

The original operating system for the PDP-6/PDP-10 was just called
"Monitor". It was designed to fit into 6K words. Around the third
release swapping was added. The sixth release saw the addition of
virtual memory. Around the fourth release it was given the name
"TOPS-10". Around this time BBN was working on a paging system and
implemented it on the PDP-10. This was called "TENEX". This was later
adopted by DEC and became "TOPS-20".

During the mid 1960s a group at MIT, who were not happy with how
Multics was being developed, decided to create their own operating
system, which they called Incompatible Timesharing System, or
"ITS". The name was a play on an earlier project called the Compatible
Timesharing System, or "CTSS". CTSS was implemented by MIT on their
IBM 7090 as an experimental system that allowed multiple time sharing
users to co-exist on the same machine running batch processing, hence
the term "Compatible".

Also during the mid 1960s a group at Stanford Artificial Intelligence
Laboratory (SAIL), started with a version of TOPS-10 and heavily
modified it to become WAITS.

During the 1970s, Tymshare modified TOPS-10 to support random access
files, paging with working sets, and spawnable processes. This ran on
the KI10, KL10, and KS10.

The PDP-10 was ultimately replaced by the VAX.

# Simulator Files

To compile the DEC PDP-10/KL10 simulator, you must define `USE_INT64`
as part of the compilation command line.

| Subdirectory | File | Contains |
|--------------|------|----------|
| `PDP10/` | | |
| | `kx10_defs.h` | KL10 simulator definitions |
| | `kx10_cpu.c` | KL10 CPU. |
| | `kx10_df.c` | DF10/C controller. |
| | `kx10_rh.c` | RH10/RH20 controller. |
| | `kx10_disk.h` | Disk formatter definitions |
| | `kx10_disk.c` | Disk formatter routine. |
| | `kx10_sys.c` | KL10 system interface |
| | `kl10_fe.c` | PDP-11 front end processor and LP20 front-end printer |
| | `kl10_dn.c` | Front end terminal connections |
| | `kx10_pt.c` | Paper Tape Reader and Punch |
| | `kx10_dp.c` | RP10 Disk controller, RP01/2/3 disks. |
| | `kx10_rp.c` | RH10 Disk controller, RP04/5/6 disks. |
| | `kx10_rs.c` | RH10 Disk controller, RS04 fixed-head disks. |
| | `kx10_dt.c` | TD10 DECtape controller. |
| | `kx10_mt.c` | TM10A/B magnetic tape controller. |
| | `kx10_tu.c` | RH10/TM03 magnetic tape controller. |
| | `kx10_lp.c` | Line printer. |
| | `kx10_cr.c` | Punch card reader. |
| | `kx10_cp.c` | Punch card punch. |
| | `ka10_pd.c` | DeCoriolis clock. |
| | `kx10_dc.c` | DC10 communications controller. |
| | `kl10_nia.c` | NIA20 network interface. |
| | `kx10_imp.c` | IMP interface to Ethernet. |
| | `ka10_ch10.c` | CH10 Chaosnet interface. |

# KL10 Features

The KL10 simulator is configured as follows:

| Device Name(s) | Simulates |
|----------------|-----------|
| `CPU` | KL10 CPU with 256KW of memory |
| `CTY` | Console TTY. |
| `PTP` | Paper tape punch |
| `PTR` | Paper tape reader. |
| `LPT` | LP10 Line Printer |
| `LP20` | LP20 Line Printer on FE. |
| `MTA` | TM10A/B Tape controller. |
| `FSA` | RS04 Disk controller via RH10 |
| `DPA,DPB` | RP10 Disk controller |
| `RPA,RPB,RPC,RPD` | RH10/RH20 Disk controllers via RH10 |
| `TUA` | TM02 Tape controller via RH10/RH20 |
| `TTY` | TTY lines on FE. |
| `DT` | TD10 DECtape Controller. |
| `DC` | DC10 Communications controller. |
| `PD` | Coriolis Clock |
| `IMP` | IMP network interface |
| `NI` | NIA20 network interface |
| `CH` | CH10 Chaosnet interface |

## CPU

The CPU options include setting memory size and O/S customization.

| command | action |
|---------|--------|
| `SET CPU 16K` | Sets memory to 16K |
| `SET CPU 32K` | Sets memory to 32K |
| `SET CPU 48K` | Sets memory to 48K |
| `SET CPU 64K` | Sets memory to 64K |
| `SET CPU 96K` | Sets memory to 96K |
| `SET CPU 128K` | Sets memory to 128K |
| `SET CPU 256K` | Sets memory to 256K |
| `SET CPU 512K` | Sets memory to 512K |
| `SET CPU 768K` | Set memory to 768K |
| `SET CPU 1024K` | Set memory to 1024K |
| `SET CPU 2048K` | Set memory to 2048K |
| `SET CPU 4096K` | Set memory to 4096K |
| `SET CPU SERIAL=val` | Set serial number of system. (default: 1025) |
| `SET CPU DF10` | Set DF10 emulation to model A, max 256K memory. |
| `SET CPU DF10C` | Set DF10 emulation to model C, no max. |
| `SET CPU ITS` | Sets CPU for ITS customization |
| `SET CPU KL10A` | Sets CPU to KL10A. |
| `SET CPU KL10B` | Sets CPU to KL10B. |
| `SET CPU NOIDLE` | Disables Idle detection |
| `SET CPU IDLE` | Enables Idle detection |

CPU registers include the visible state of the processor as well as
the control registers for the interrupt system.

| Name | Size | Comments | OS Type |
|------|-----:|----------|---------|
| `PC` | 18 | Program Counter | |
| `FLAGS` | 18 | Flags | |
| `FM0-FM17` | 36 | Fast Memory | |
| `SW` | 36 | Console SW Register | |
| `MI` | 36 | Monitor Display | |
| `PIR` | 8 | Priority Interrupt Request | |
| `PIH` | 8 | Priority Interrupt Hold | |
| `PIE` | 8 | Priority Interrupt Enable | |
| `PIENB` | 1 | Enable Priority System | |
| `BYF5` | 1 | Byte Flag | |
| `UUO` | 1 | UUO Cycle | |
| `PL` | 18 | Program Limit Low | |
| `PH` | 18 | Program Limit High | |
| `RL` | 18 | Program Relation Low | |
| `RH` | 18 | Program Relation High | |
| `PFLAG` | 1 | Relocation enable | |
| `PUSHOVER` | 1 | Push overflow flag | |
| `MEMPROT` | 1 | Memory protection flag | |
| `NXM` | 1 | Non-existing memory access | |
| `CLK` | 1 | Clock interrupt | |
| `OV` | 1 | Overflow enable | |
| `FOV` | 1 | Floating overflow enable | |
| `APRIRQ` | 3 | APR Interrupt number | |
| `PAGE_ENABLE` | 1 | Paging enabled | ITS/BBN |
| `PAGE_FAULT` | 1 | Page fault | ITS/BBN |
| `AC_STACK` | 18 | AC Stack | ITS/BBN |
| `PAGE_RELOAD` | 18 | Page reload | ITS/BBN |
| `FAULT_DATA` | 36 | Page fault data | ITS/BBN |
| `TRP_FLG` | 1 | Trap flag | ITS/BBN |
| `LAST_PAGE` | 9 | Last page | ITS/BBN |
| `EXEC_MAP` | 8 | Executive mapping | BBN |
| `NXT_WR` | 1 | Map next write | BBN |
| `MON_BASE` | 8 | Monitor base | BBN |
| `USER_BASE` | 8 | User base | BBN |
| `USER_LIMIT` | 3 | User limit | BBN |
| `PER_USER` | 36 | Per user data | BBN |
| `DBR1` | 18 | Paging control register 1 | ITS |
| `DBR2` | 18 | Paging control register 2 | ITS |
| `DBR3` | 18 | Paging control register 3 | ITS |
| `JPC` | 18 | Last Jump instruction address | ITS |
| `AGE` | 4 | Age | ITS |
| `FAULT_ADDR` | 18 | Fault address | ITS |
| `OPC` | 36 | Saved PC and flags | ITS |
| `MAR` | 18 | Memory Access Register | ITS |
| `QUA_TIME` | 36 | Quantum time. | ITS |

The CPU can maintain a history of the most recently executed
instructions.

This is controlled by the `SET CPU HISTORY` and `SHOW CPU HISTORY`
commands:

| command | action |
|---------|--------|
| `SET CPU HISTORY` | clear history buffer |
| `SET CPU HISTORY=0` | disable history |
| `SET CPU HISTORY=n` | enable history, length = `n` |
| `SHOW CPU HISTORY` | print CPU history |
| `SHOW CPU HISTORY=n` | print first `n` entries of CPU history |

Instruction tracing shows the Program counter, the contents of AC
selected, the computed effective address. `AR` is generally the
contents of the memory location referenced by `EA`. `RES` is the
result of the instruction. `FLAGS` shows the flags after the
instruction is executed. `IR` shows the instruction being executed.

## PDP-11 front end processor (DTE 200)

### Console Teletype (CTY) (on DTE0)

The console station allows for communications with the operating
system.

| command | action |
|---------|--------|
| `SET CTY 7B` | 7 bit characters, space parity. |
| `SET CTY 8B` | 8 bit characters, space parity. |
| `SET CTY 7P` | 7 bit characters, space parity. |
| `SET CTY UC` | Translate lower case to upper case. |

The front end device needs to be configured for the specific operating
system being run. For TOPS-10 7 and TOPS-20 this must be `RSX20`. For
TOPS-10 6.03 set it to `RSX10`.

| command | action |
|---------|--------|
| `SET CTY RSX10` | Sets the front end to emulate a PDP-11 running RSX10. |
| `SET CTY RSX20` | Sets the front end to emulate a PDP-11 running RSX20. |

The `CTY` also supports a method for stopping the TOPS-10 operating
system.

### LP20 printer on front end (LP20) (on DTE)

The line printer (`LP20`) writes data to a disk file as ASCII text
with terminating newlines. It currently handles standard signals that
control paper advance. Currently only one line printer device is
supported.

| command | action |
|---------|--------|
| `SET LP20 LC` | Allow printer to print lower case |
| `SET LP20 UC` | Print only upper case |
| `SET LP20 LINESPERPAGE=n` | Sets the number of lines before an auto form feed is generated. There is an automatic margin of 6 lines. There is a maximum of 100 lines per page. |

### Front end terminal connections (TTY)

See section [Front end terminals (TTY)](#front-end-terminals-tty).

## Unit record I/O Devices

### Paper Tape Reader (PTR) (Device 104)

Reads a paper tape from a disk file. By default this device is
disabled.

### Paper Tape Punch (PTP) (Device 100)

Punches a paper tape to a disk file. By default this device is
disabled.

### Card Reader (CR) (Device 150)

The card reader (`CR`) reads data from a disk file. Card reader files
can either be text (one character per column) or column binary (two
characters per column). By default this device is disabled. The file
type can be specified with a `SET` command:

| command | action |
|---------|--------|
| `SET CRn FORMAT=TEXT` | Sets ASCII text mode |
| `SET CRn FORMAT=BINARY` | Sets for binary card images. |
| `SET CRn FORMAT=BCD` | Sets for BCD records. |
| `SET CRn FORMAT=CBN` | Sets for column binary BCD records. |
| `SET CRn FORMAT=AUTO` | Automatically determines format. |

or in the `ATTACH` command:

| command | action |
|---------|--------|
| `ATTACH CRn <file>` | Attaches a file |
| `ATTACH CRn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH CRn -s <file>` | Added file onto current cards to read. |
| `ATTACH CRn -e <file>` | After a file is read in, the reader will receive an end-of-file flag. |

### Card Punch (CP) (Device 110)

The card reader (`CP`) writes data to a disk file. Cards are simulated
as ASCII lines with terminating newlines. Card punch files can either
be text (one character per column) or column binary (two characters
per column). By default this device is disabled. The file type can be
specified with a `SET` command:

| command | action |
|---------|--------|
| `SET CP FORMAT=TEXT` | Sets ASCII text mode |
| `SET CP FORMAT=BINARY` | Sets for binary card images. |
| `SET CP FORMAT=BCD` | Sets for BCD records. |
| `SET CP FORMAT=CBN` | Sets for column binary BCD records. |
| `SET CP FORMAT=AUTO` | Automatically determines format. |

or in the `ATTACH` command:

| command | action |
|---------|--------|
| `ATTACH CP <file>` | Attaches a file |
| `ATTACH CP -f <format> <file>` | Attaches a file with the given format. |

### Line Printer (LPT) (Device 124)

The line printer (`LPT`) writes data to a disk file as ASCII text with
terminating newlines. It currently handles standard signals that
control paper advance.

| command | action |
|---------|--------|
| `SET LPTn LC` | Allow printer to print lower case |
| `SET LPTn UC` | Print only upper case |
| `SET LPTn UTF8` | Print control characters as UTF8 characters. |
| `SET LPTn LINESPERPAGE=n` | Sets the number of lines before an auto form feed is generated. There is an automatic margin of 6 lines. There is a maximum of 100 lines per page. |
| `SET LPTn DEV=n` | Sets device number to `n` (octal). |

These characters control the skipping of various number of lines.

| Character | Effect |
|-----------|--------|
| `014` | Skip to top of form. |
| `013` | Skip mod 20 lines. |
| `020` | Skip mod 30 lines. |
| `021` | Skip to even line. |
| `022` | Skip to every 3 line |
| `023` | Same as line feed (12), but ignore margin. |

### TM10 Magnetic Tape (MTA) (Device 340,344)

The TM10 was the most common tape controller on KA10 and KI10. The
TM10 came in two models, the TM10A and the TM10B. The B model added a
DF10 which allowed the tape data to be transferred without
intervention of the CPU. The device has 2 options. By default this
device is disabled.

| command | action |
|---------|--------|
| `SET MTA TYPE=t` | Sets the type of the controller to A or B |
| `SET MTA MPX=#` | For ITS set the MPX interrupt to channel `#` |

Each individual tape drive supports several options: `MTA` is used as
an example.

| command | action |
|---------|--------|
| `SET MTAn 7T` | Sets the magtape unit to 7 track format. |
| `SET MTAn 9T` | Sets the magtape unit to 9 track format. |
| `SET MTAn LOCKED` | Sets the magtape to be read only. |
| `SET MTAn WRITEENABLE` | Sets the magtape to be writable. |

### TD10 DECtape (DT) (Device 320,324)

The TD10 was the standard DECtape controller for KA and KIs. This
controller loads the tape into memory and uses the buffered
version. Each individual tape drive supports several options: `DT` is
used as an example. By default this device is disabled.

| command | action |
|---------|--------|
| `SET DTn LOCKED` | Sets the DECtape to be read only. |
| `SET DTn WRITEENABLE` | Sets the DECtape to be writable. |
| `SET DTn 18b` | Default, tapes are considered to be 18-bit tapes. |
| `SET DTn 16b` | Tapes are converted from 16 bit to 18 bit. |
| `SET DTn 12b` | Tapes are converted from 12 bit to 18 bit. |

### PD DeCoriolis clock (Device 500)

This is a device which keeps track of the time and date. An access
will return the number of ticks since the beginning of the year. There
are 60 ticks per second. The device was made by Paul DeCoriolis at
MIT. By default this device is disabled.

## Disk I/O Devices

The PDP-10 supported many disk controllers. The KL10 supports only
Massbus devices, but in practice could interface to older drives.
These are currently not enabled in the simulator.

### DPA/DPB Disk Controller (Device 250/254)

The RP10 disk controller used a DF10 to transfer data to memory. There
were three types of disks that could be connected to the RP10: the
RP01, RP02, and RP03. The RP01 held up to 1.2M words, the RP02 5.2M
words, and the RP03 10M words. The second controller is `DPB`. Disks
can be stored in one of several file formats, `SIMH`, `DBD9`, and
`DLD9`. The latter two are for compatibility with other simulators. By
default this device is disabled.

Each individual disk drive supports several options: `DPA` is used as
an example.

| command | action |
|---------|--------|
| `SET DPAn RP01` | Sets this unit to be an RP01. |
| `SET DPAn RP02` | Sets this unit to be an RP02. |
| `SET DPAn RP03` | Sets this unit to be an RP03. |
| `SET DPAn HEADERS` | Enables the RP10 to execute the write-headers function. |
| `SET DPAn NOHEADERS` | Prevents the RP10 from executing the write-headers function. |
| `SET DPAn LOCKED` | Sets this unit to be read only. |
| `SET DPAn WRITEENABLE` | Sets this unit to be writable. |

To attach a disk use the `ATTACH` command:

| command | action |
|---------|--------|
| `ATTACH DPAn <file>` | Attaches a file |
| `ATTACH DPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH DPAn -n <file>` | Creates a blank disk. |

Format can be `SIMH`, `DBD9`, `DLD9`.

## Massbus Devices

Massbus devices are attached via RH10s or RH20s. RH10 devices start
at device 270 and go up (`274`, `360`, `364`, `370`, `374`). RH20
devices start at 540 and go up by `+4` for each device. For TOPS-10,
devices need to be in the order `RS`, `RP`, `TU`. By default RS disks
are disabled. The first unit which is not enabled will get device 270,
and units will be assigned the next available address
automatically. The device configuration must match that which is
defined in the OS. TOPS-20 requires RH20 to work.

### RP Disk drives

Up to 4 strings of up to 8 RP drives can be configured. By default the
third and fourth controllers are disabled. These are addressed as
`RPA`, `RPB`, `RPC`, and `RPD`. Disks can be stored in one of several
file formats, `SIMH`, `DBD9`, and `DLD9`. The latter two are for
compatibility with other simulators.

| command | action |
|---------|--------|
| `SET RPAn RP04` | Sets this unit to be an RP04. (19MWords) |
| `SET RPAn RP06` | Sets this unit to be an RP06. (39MWords) |
| `SET RPAn RP07` | Sets this unit to be an RP07. (110MWords) |
| `SET RPAn LOCKED` | Sets this unit to be read only. |
| `SET RPAn WRITEENABLE` | Sets this unit to be writable. |
| `SET RPA RH10` | Sets controller to RH10 type. |
| `SET RPA RH20` | Sets controller to RH20 type. |

To attach a disk use the `ATTACH` command:

| command | action |
|---------|--------|
| `ATTACH RPAn <file>` | Attaches a file |
| `ATTACH RPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH RPAn -n <file>` | Creates a blank disk. |

Format can be `SIMH`, `DBD9`, `DLD9`.

### RS Disk drives

One string of up to 8 RS drives can be configured. These drives are
fixed-head swapping disks. By default they are disabled.

| command | action |
|---------|--------|
| `SET FSAn RS03` | Sets this unit to be an RS03. (262KWords) |
| `SET FSAn RS04` | Sets this unit to be an RS04. (262KWords) |
| `SET FSAn LOCKED` | Sets this unit to be read only. |
| `SET FSAn WRITEENABLE` | Sets this unit to be writable. |
| `SET FSA RH10` | Sets controller to RH10 type. |
| `SET FSA RH20` | Sets controller to RH20 type. |

### TU Tape drives

The `TUA` is a Massbus tape controller using a TM03 formatter. There
can be one per RH10 and it can support up to 8 drives.

| command | action |
|---------|--------|
| `SET TUAn LOCKED` | Sets the magtape to be read only. |
| `SET TUAn WRITEENABLE` | Sets the magtape to be writable. |
| `SET TUA RH10` | Sets controller to RH10 type. |
| `SET TUA RH20` | Sets controller to RH20 type. |

## Terminal Multiplexer I/O Devices

All terminal multiplexers must be attached in order to work. The
`ATTACH` command specifies the port to be used for Telnet sessions:

| command | action |
|---------|--------|
| `ATTACH <device> <port>` | set up listening port |

where `port` is a decimal number between 1 and 65535 that is not being
used for other TCP/IP activities.

Once attached and the simulator is running, the multiplexer listens
for connections on the specified port. It assumes that any incoming
connection is a Telnet connection. The connections remain open until
disconnected either by the Telnet client, a `SET <device> DISCONNECT`
command, or a `DETACH <device>` command.

| command | action |
|---------|--------|
| `SET <device> DISCONNECT=n` | Disconnect line `n` |

The `<device>` implements the following special `SHOW` commands:

| command | action |
|---------|--------|
| `SHOW <device> CONNECTIONS` | Displays current connections to the `<device>` |
| `SHOW <device> STATISTICS` | Displays statistics for active connections |
| `SHOW <device> LOG` | Displays logging for all lines. |

Logging can be controlled as follows:

| command | action |
|---------|--------|
| `SET <device> LOG=n=filename` | Log output of line `n` to `filename` |
| `SET <device> NOLOG` | Disable logging and close log file |

### DC10E Terminal Controller (Device 240)

The `DC` device was the standard terminal multiplexer for the KA10 and
KI10s. Lines came in groups of 8. For modem control there was a
second port for each line. These could be offset by any number of
groups. By default this device is disabled.

| command | action |
|---------|--------|
| `SET DC LINES=n` | Set the number of lines on the DC10, multiple of 8. |
| `SET DC MODEM=n` | Set the start of where the modem-control DC10E lines begin. |

### Front end terminals (TTY)

The front-end PDP-11 could be configured with many DH11s offering 16
lines per module. All lines supported modem control and speed
setting.

| command | action |
|---------|--------|
| `SET TTY LINES=n` | Set the number of lines on the front-end PDP-11, multiples of 16 |

## Network Devices

### NIA20 Network interface (NI) (Device 564)

The NIA20 actually sits in Slot 5 of the RH20 rack and configures
itself as an RH20 device. The system will automatically move other
conflicting RH20 devices around.

| command | action |
|---------|--------|
| `SET NI MAC=xx:xx:xx:xx:xx:xx` | Set the MAC address of the `NI` to the value given. |

### IMP Interface Message Processor (Device 460)

This allows the KA/KI/KL to connect to the Internet. Currently only
supported under ITS. ITS and other OSes that used the IMP did not
support modern protocols and typically required a complete rebuild to
change the IP address. Because of this the IMP processor includes
built-in NAT and DHCP support. For ITS the system generated IP packets
which are translated to the local network. If the `HOST` is set to
`0.0.0.0` there will be no translation. If `HOST` is set to an IP
address then it will be mapped into the address set in `IP`. If
`DHCP` is enabled the `IMP` will issue a DHCP request at startup and
set `IP` to the address that is provided. DHCP is enabled by default.

| command | action |
|---------|--------|
| `SET IMP MAC=xx:xx:xx:xx:xx:xx` | Set the MAC address of the `IMP` to the value given. |
| `SET IMP IP=ddd.ddd.ddd.ddd/dd` | Set the external IP address of the `IMP` along with the net mask in bits. |
| `SET IMP GW=ddd.ddd.ddd.ddd` | Set the Gateway address for the `IMP`. |
| `SET IMP HOST=ddd.ddd.ddd.ddd` | Sets the IP address of the PDP-10 system. |
| `SET IMP DHCP` | Allows the `IMP` to acquire an IP address from the local network via `DHCP`. Only `HOST` must be set if this feature is used. |
| `SET IMP NODHCP` | Disables the `IMP` from making `DHCP` queries |
| `SET IMP ARP=ddd.ddd.ddd.ddd=xx:xx:xx:xx:xx:xx` | Creates a static `ARP` entry for the IP address with the indicated MAC address. |
| `SET IMP MIT` | Sets the `IMP` to look like the IMP used by MIT for ITS. |
| `SET IMP MPX=#` | For ITS set the MPX interrupt to channel `#` |
| `SET IMP BBN` | Sets the `IMP` to behave like a generic BBN IMP. (Not implemented yet). |
| `SET IMP WAITS` | Sets the `IMP` for the Stanford-style IMP used by WAITS. |
| `SHOW IMP ARP` | Displays the IP address to MAC address table. |

The `IMP` device must be attached to an available Ethernet interface.
To determine which devices are available use `SHOW ETHERNET` to list
the potential interfaces. Check out `0readme_ethernet.txt` from the
top of the source directory.

The `IMP` device can be configured in several ways. Either it can
connect directly to an Ethernet port (via `TAP`) or it can be
connected via a `TUN` interface. If configured via a `TAP` interface
the `IMP` will behave like any other Ethernet interface and, if asked,
obtain its own address. In environments where this is not desired the
`TUN` interface can be used. When configured under a `TUN` interface
the simulated network is a collection of ports on the local host.
These can be mapped based on configuration options; see
`0readme_ethernet.txt` for details.

With the `IMP` interface, the IP address of the simulated system is
static, and under ITS is configured into the system at compile time.
This address should be given to the `IMP` with `SET IMP HOST=<ip>`.
The `IMP` will direct all traffic it sees to this address. If this
address is not the same as the address of the system as seen by the
network, then this address can be set with `SET IMP IP=<ip>`, and
`SET IMP GW=<ip>`, or `SET IMP DHCP`, which will allow the `IMP` to
request an address from a local `DHCP` server. The `IMP` will
translate the packets it receives and sends to look like they appeared
from the desired address. The `IMP` will also correctly translate FTP
requests in this configuration.

When running under a `TUN` interface, `IMP` is on a virtual
`10.0.2.0` network. The default gateway is `10.0.2.1`, with the
default `IMP` at `10.0.2.15`. For this mode `DHCP` can be used.

### CH Chaosnet interface (Device 470)

Chaosnet was another method of network access for ITS. Chaosnet can be
connected to another ITS system or a VAX/PDP-11 running BSD Unix or
VMS. Chaosnet runs over UDP protocol under Unix. You must specify a
node number, peer to talk to, and your local UDP listening port. All
UDP packets are sent to the peer for further processing.

| command | action |
|---------|--------|
| `SET CH NODE=n` | Set the Node number for this system. |
| `SET CH PEER=ddd.ddd.ddd.ddd:dddd` | Set the Peer address and port number. |

The `CH` device must be attached to a UDP port number. This is where
it will receive UDP packets from its peer.

# Symbolic Display and Input

The KL10 simulator implements symbolic display and input. These are
controlled by the following switches to the `EXAMINE` and `DEPOSIT`
commands:

| switch | action |
|--------|--------|
| `-v` | Looks up the address via translation, will return nothing if address is not valid. |
| `-u` | With the `-v` option, use user space instead of executive space. |
| `-a` | Display/Enter ASCII data. |
| `-p` | Display/Enter packed ASCII data. |
| `-c` | Display/Enter sixbit character data. |
| `-m` | Display/Enter symbolic instructions. |
| `default` | Display/Enter octal data. |

Symbolic instructions can be of the formats:

```text
Opcode ac,operand
Opcode operands
I/O Opcode device,address
```

Operands can be one or more of the following in order:

- Optional `@` for indirection.
- `+` or `-` to set sign.
- Octal number.
- Optional `(ac)` for indexing.

Breakpoints can be set at real memory addresses. The PDP-10 supports 3
types of breakpoints: execution, memory access, and memory modify. The
default is execution. Adding `-r` to the breakpoint command will stop
the simulator on access to that memory location, either via fetch,
indirection, or operand access. Adding `-w` will stop the simulator
when the location is modified.

The simulator can load `RIM` files, `SAV` files, `EXE` files, WAITS
octal `DMP` files, and MIT `SBLK` files.

When instruction history is enabled, the history trace shows internal
status of various registers at the start of instruction execution.

| field | meaning |
|-------|---------|
| `PC` | Shows the PC of the instruction executed. |
| `AC` | The contents of the referenced AC. |
| `EA` | The final computed Effective address. |
| `AR` | Generally the operand that was computed. |
| `RES` | The AR register after the instruction was complete. |
| `FLAGS` | The values of the `FLAGS` register before execution of the instruction. |
| `IR` | The fetched instruction followed by the disassembled instruction. |

The PDP-10 simulator allows for memory-reference and memory-modify
breakpoints with the `-r` and `-w` options given to the `BREAK`
command.

# OS specific configurations

## TOPS-10

The default configuration supports TOPS-10, so there is no extra
configuration required. TOPS-10 version 7 and up requires a KL10B
processor and the `CTY` to be set to `RSX20`.

## TOPS-20

TOPS-20 requires that all RH devices be of type `RH20`. Depending on
the version it may require a KL10B processor. Also the `CTY` must be
set to `RSX20`.

## ITS

To run ITS the CPU must be set with `SET CPU ITS 1024K`, which enables
the ITS pager.
