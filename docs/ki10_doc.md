# PDP-10 KA10 Processor Simulator User's Guide

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

The DEC PDP-10/KI10 simulator was written by Richard Cornwell.

# Table of Contents

[1. Introduction](#introduction)

[2. Simulator Files](#simulator-files)

[3. KI10 Features](#ki10-features)

[3.1 CPU](#cpu)

[3.1.1 DDC10 Drum controller for TENEX](#ddc10-drum-controller-for-tenex)

[3.2 Unit record I/O Devices](#unit-record-io-devices)

[3.2.1 Console Teletype (CTY) (Device 120)](#console-teletype-cty-device-120)

[3.2.2 Paper Tape Reader (PTR) (Device 104)](#paper-tape-reader-ptr-device-104)

[3.2.3 Paper Tape Punch (PTP) (Device 100)](#paper-tape-punch-ptp-device-100)

[3.2.4 Card Reader (CR) (Device 150)](#card-reader-cr-device-150)

[3.2.5 Card Punch (CP) (Device 110)](#card-punch-cp-device-110)

[3.2.6 Line Printer (LPT) (Device 124)](#line-printer-lpt-device-124)

[3.2.7 Type 340 Graphics display(DPY) (Device 130)](#type-340-graphics-displaydpy-device-130)

[3.2.8 DK10 Timer Module (DK) (Device 070)](#dk10-timer-module-dk-device-070)

[3.2.9 TM10 Magnetic Tape (MTA) (Device 340,344)](#tm10-magnetic-tape-mta-device-340344)

[3.2.10 TD10 DECtape (DT) (Device 320,324)](#td10-dectape-dt-device-320324)

[3.3 Disk I/O Devices](#disk-io-devices)

[3.3.1 FHA Disk Controller (Device 170)](#fha-disk-controller-device-170)

[3.3.2 DPA/DPB Disk Controller (Device 250/254)](#dpadpb-disk-controller-device-250254)

[3.3.3 DDC10 Drum controller for TENEX](#ddc10-drum-controller-for-tenex-1)

[3.4 Massbus Devices](#massbus-devices)

[3.4.1 RP Disk drives](#rp-disk-drives)

[3.4.2 RS Disk drives](#rs-disk-drives)

[3.4.3 TU Tape drives.](#tu-tape-drives.)

[3.4.4 DC10E Terminal Controller (Device 240)](#dc10e-terminal-controller-device-240)

[3.5 Network Devices.](#network-devices.)

[3.5.1 IMP Interface Message Processor (Device 460)](#imp-interface-message-processor-device-460)

[4. Symbolic Display and Input](#symbolic-display-and-input)

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
256K words of memory.

The first PDP-10 was labeled KA10, and added a few instructions to the
PDP-6. Both the PDP-6 and PDP-10 used a base and limit relocation
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

The final version to make it to market was the KS10 (for Small), this
was a bit-slice version of the PDP-10 which used Unibus devices which
were cheaper then the KL10 devices.

The original operating system for the PDP-6/PDP-10 was just called
"Monitor". It was designed to fit into 6K words. Around the third
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

Also during the mid 1960s a group at Stanford Artificial Intelligence
Laboratory (SAIL), started with a version of TOPS-10 and heavily
modified it to become WAITS.

During the 1970s, Tymshare modified TOPS-10 to support random access
files, paging with working sets, and spawnable processes. This ran on
the KI10, KL10, and KS10.

The PDP-10 was ultimately replaced by the VAX.

#  Simulator Files

To compile the DEC PDP-10/KI10 simulator, you must define `USE_INT64`
as part of the compilation command line.

| Subdirectory | File | Contains |
|--------------|------|----------|
| `PDP10/`     |      |          |
|   | `kx10_defs.h`   | KA10 simulator definitions                  |
|   | `kx10_cpu.c`    | KI10 CPU.                                   |
|   | `kx10_df.c`     | DF10/C controller.                          |
|   | `kx10_rh.c`     | RH10 controller.                            |
|   | `kx10_disk.h`   | Disk formatter definitions                  |
|   | `kx10_disk.c`   | Disk formatter routine.                     |
|   | `kx10_sys.c`    | KI10 System interface                       |
|   | `kx10_cty.c`    | Console Terminal Interface.                 |
|   | `kx10_pt.c`     | Paper Tape Reader and Punch                 |
|   | `kx10_rc.c`     | RC10 Disk controller, RD10 and RM10 Disks   |
|   | `kx10_dp.c`     | RP10 Disk controller, RP01/2/3 Disks.       |
|   | `kx10_rp.c`     | RH10 Disk controller, RP04/5/6 Disks.       |
|   | `kx10_rs.c`     | RH10 Disk controller RS04 Fixed head Disks. |
|   | `kx10_dt.c`     | TD10 DECtape controller.                    |
|   | `kx10_mt.c`     | TM10A/B Magnetic tape controller.           |
|   | `kx10_tu.c`     | RH10/TM03 Magnetic tape controller.         |
|   | `kx10_lp.c`     | Line printer.                               |
|   | `kx10_cr.c`     | Punch card reader.                          |
|   | `kx10_cp.c`     | Punch card punch.                           |
|   | `kx10_dk.c`     | DK10 interval timer.                        |
|   | `kx10_dc.c`     | DC10 communications controller.             |
|   | `kx10_ddc.c`    | DDC-10 Drum controller                      |
|   | `kx10_lights.c` | Front panel interface.                      |
|   | `kx10_imp.c`    | IMP10 interface to ethernet.                |

#  KI10 Features

The KI10 simulator is configured as follows:

| Device Name(s)    | Simulates                       |
|-------------------|---------------------------------|
| `CPU`             | KI10 CPU with 256KW of memory   |
| `CTY`             | Console TTY.                    |
| `PTP`             | Paper tape punch                |
| `PTR`             | Paper tape reader.              |
| `LPT`             | LP10 Line Printer               |
| `CR`              | CR10 Card reader.               |
| `CP`              | CP10 Card punch                 |
| `MTA`             | TM10A/B Tape controller.        |
| `DPA,DPB`         | RP10 Disk controller            |
| `FSA`             | RS04 Disk controller via RH10   |
| `RPA,RPB,RPC,RPD` | RH10 Disk controllers via RH10  |
| `PMP`             | PMP IBM 3330 disk controller    |
| `TUA`             | TM02 Tape controller via RH10   |
| `FHA`             | RC10 Disk controller.           |
| `DDC`             | DDC10 Disk controller.          |
| `DT`              | TD10 DECtape Controller.        |
| `DC`              | DC10 Communications controller. |
| `DK`              | Clock timer module.             |
| `IMP`             | IMP network interface           |

##  CPU

The CPU options include setting memory size.

|                      |                                                 |
|----------------------|-------------------------------------------------|
| `SET CPU 16K`        | Sets memory to 16K                              |
| `SET CPU 32K`        | Sets memory to 32K                              |
| `SET CPU 48K`        | Sets memory to 48K                              |
| `SET CPU 64K`        | Sets memory to 64K                              |
| `SET CPU 96K`        | Sets memory to 96K                              |
| `SET CPU 128K`       | Sets memory to 128K                             |
| `SET CPU 256K`       | Sets memory to 256K                             |
| `SET CPU 512K`       | Sets memory to 512K                             |
| `SET CPU 768K`       | Set memory to 768K                              |
| `SET CPU 1024K`      | Set memory to 1024K                             |
| `SET CPU 2048K`      | Set memory to 2048K                             |
| `SET CPU 4096K`      | Set memory to 4096K                             |
| `SET CPU SERIAL=val` | Set serial number of system. (default: 514)     |
| `SET CPU DF10`       | Set DF10 emulation to model A, max 256K memory. |
| `SET CPU DF10C`      | Set DF10 emulation to model C, no max.          |
| `SET CPU NOMAOFF`    | Sets traps to default of 040                    |
| `SET CPU MAOFF`      | Move trap vectors from 040 to 0140              |
| `SET CPU NOIDLE`     | Disables Idle detection                         |
| `SET CPU IDLE`       | Enables Idle detection                          |

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

| Name          | Size | Comments                   |
|---------------|-----:|----------------------------|
| `PC`          |   18 | Program Counter            |
| `FLAGS`       |   18 | Flags                      |
| `FM0-FM17`    |   36 | Fast Memory                |
| `SW`          |   36 | Console SW Register        |
| `MI`          |   36 | Memory Indicators          |
| `AS`          |   18 | Console AS register        |
| `ABRK`        |    1 | Address break              |
| `ACOND`       |    5 | Address Condition switches |
| `PIR`         |    8 | Priority Interrupt Request |
| `PIH`         |    8 | Priority Interrupt Hold    |
| `PIE`         |    8 | Priority Interrupt Enable  |
| `PIENB`       |    1 | Enable Priority System     |
| `BYF5`        |    1 | Byte Flag                  |
| `UUO`         |    1 | UUO Cycle                  |
| `PUSHOVER`    |    1 | Push overflow flag         |
| `MEMPROT`     |    1 | Memory protection flag     |
| `NXM`         |    1 | Non-existing memory access |
| `CLK`         |    1 | Clock interrupt            |
| `OV`          |    1 | Overflow enable            |
| `FOV`         |    1 | Floating overflow enable   |
| `APRIRQ`      |    3 | APR Interrupt number       |
| `CLKIRQ`      |    3 | CLK Interrupt number       |
| `UB`          |   18 | User Base Pointer          |
| `EB`          |   18 | Executive Base Pointer     |
| `FMSEL`       |    8 | Register set select        |
| `SERIAL`      |   10 | System Serial Number       |
| `INOUT`       |    1 |                            |
| `SMALL`       |    1 |                            |
| `PAGE_ENABLE` |    1 | Paging enabled             |
| `PAGE_FAULT`  |    1 | Page fault                 |
| `AC_STACK`    |   18 | AC Stack                   |
| `PAGE_RELOAD` |   18 | Page reload                |
| `FAULT_DATA`  |   36 | Page fault data            |
| `TRP_FLG`     |    1 | Trap flag                  |

The CPU can maintain a history of the most recently executed instructions.

This is controlled by the `SET CPU HISTORY` and `SHOW CPU HISTORY`
commands:

|                      |                                      |
|----------------------|--------------------------------------|
| `SET CPU HISTORY`    | clear history buffer                 |
| `SET CPU HISTORY=0`  | disable history                      |
| `SET CPU HISTORY=n`  | enable history, length = `n`           |
| `SHOW CPU HISTORY`   | print CPU history                    |
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

|                     |                 |
|---------------------|-----------------|
| `ATTACH CRn <file>` | Attaches a file |
| `ATTACH CRn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH CRn -s <file>` | Added file onto current cards to read. |
| `ATTACH CRn -e <file>` | After file is read in, the reader will receive and end of file flag. |

###  Card Punch (`CP`) (Device 110)

The card punch (`CP`) writes data to a disk file. Cards are simulated
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

or in the ATTACH command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| `ATTACH CP <file>`               | Attaches a file                        |
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

###  DK10 Timer Module (`DK`) (Device 070)

The DK10 timer module does not have any settable options.

###  TM10 Magnetic Tape (`MTA`) (Device 340,344)

The TM10 was the most common tape controller on KA10 and KI10. The
TM10 came in two models, the TM10A and the TM10B. The B model added a
DF10 which allowed the tape data to be transferred without
intervention of the CPU. The device has 2 options.

|                |                                           |
|----------------|-------------------------------------------|
| `SET MTA TYPE=t` | Sets the type of the controller to A or B |

Each individual tape drive supports several options: `MTA` is used as
an example.

|                        |                                           |
|------------------------|-------------------------------------------|
| `SET MTAn 7T`          | Sets the Mag tape unit to 7 track format. |
| `SET MTAn 9T`          | Sets the Mag tape unit to 9 track format. |
| `SET MTAn LOCKED`      | Sets the mag tape to be read only.        |
| `SET MTAn WRITEENABLE` | Sets the mag tape to be writable.         |

###  TD10 DECtape (`DT`) (Device 320,324)

The TD10 was the standard DECtape controller for KA and KI’s. This
controller loads the tape into memory and uses the buffered
version. Each individual tape drive supports several options: `DT` is
used as an example.

|                       |                                                  |
|-----------------------|--------------------------------------------------|
| `SET DTn LOCKED`      | Sets the mag tape to be read only.               |
| `SET DTn WRITEENABLE` | Sets the mag tape to be writable.                |
| `SET DTn 18b`         | Default, tapes are considered to be 18bit tapes. |
| `SET DTn 16b`         | Tapes are converted from 16 bit to 18bit.        |
| `SET DTn 12b`         | Tapes are converted from 12 bit to 18bit.        |

##  Disk I/O Devices

The PDP-10 supported many disk controllers.

###  `FHA` Disk Controller (Device 170)

The RC10 disk controller used a DF10 to transfer data to memory. There
were two types of disks that could be connected to the RC10 the RD10
and RM10. The RD10 could hold up to 512K words of data. The RM10 could
hold up to 345K words of data.

Each individual disk drive support several options: FHA used as an example.

|                        |                                 |
|------------------------|---------------------------------|
| `SET FHAn RD10`        | Sets this unit to be an `RD10`. |
| `SET FHAn RM10`        | Sets this unit to be an `RM10`. |
| `SET FHAn LOCKED`      | Sets this unit to be read only. |
| `SET FHAn WRITEENABLE` | Sets this unit to be writable.  |

###  `DPA`/`DPB` Disk Controller (Device 250/254)

The RP10 disk controller used a DF10 to transfer data to memory. There
were three types of disks that could be connected to the RP10 the
`RP01`, `RP02`, `RP03`. The `RP01` held up to 1.2M words, the `RP02`
5.2M words, `RP03` 10M words. The second controller is `DPB`. Disks
can be stored in one of several file formats, `SIMH`, `DBD9`, and
`DLD9`. The later two are for compatibility with other simulators.

Each individual disk drive support several options: `DPA` used as an example.

|                 |                                 |
|-----------------|---------------------------------|
| `SET DPAn RP01` | Sets this unit to be an `RP01`. |
| `SET DPAn RP02` | Sets this unit to be an `RP02`. |
| `SET DPAn RP03` | Sets this unit to be an `RP03`. |
| `SET DPAn HEADERS` | Enables the RP10 to execute write headers function. |
| `SET DPAn NOHEADERS` | Prevents the RP10 to execute write headers function. |
| `SET DPAn LOCKED` | Sets this unit to be read only. |
| `SET DPAn WRITEENABLE` | Sets this unit to be writable. |

To attach a disk use the `ATTACH` command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| `ATTACH DPAn <file>`             | Attaches a file                        |
| `ATTACH DPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH DPAn -n <file>`          | Creates a blank disk.                  |

Format can be `SIMH`, `DBD9`, or `DLD9`.

###  DDC10 Drum controller for TENEX

The DEC RES-10 drum controller is used by TENEX for swapping. This device
is disabled by default.

##  Massbus Devices

Massbus devices are attached via RH10’s. Devices start a device 270
and go up (274, 360, 364, 370, 374). For TOPS-10, devices need to be
in the order `RS`, `RP`, `TU`. By default `RS` disks are disabled. The
first unit which is not enabled will get device 270, units will be
assigned the next available address automatically. The device
configuration must match that which is defined in the OS.

### `RP` Disk Drives

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
| `SET RPAn LOCKED`      | Sets this unit to be read only.            |
| `SET RPAn WRITEENABLE` | Sets this unit to be writable.             |

To attach a disk use the `ATTACH` command:

|                                  |                                        |
|----------------------------------|----------------------------------------|
| `ATTACH RPAn <file>`             | Attaches a file                        |
| `ATTACH RPAn -f <format> <file>` | Attaches a file with the given format. |
| `ATTACH RPAn -n <file>`          | Creates a blank disk.                  |

Format can be `SIMH`, `DBD9`, or `DLD9`.

### `RS` Disk Drives

One string of up to 8 `RS` drives can be configured. These drives are
fixed head swapping disks. By default they are disabled.

|                        |                                            |
|------------------------|--------------------------------------------|
| `SET FSAn RS03`        | Sets this unit to be an `RS03` (262KWords) |
| `SET FSAn RS04`        | Sets this unit to be an `RS04` (262KWords) |
| `SET FSAn LOCKED`      | Sets this unit to be read only.            |
| `SET FSAn WRITEENABLE` | Sets this unit to be writable.             |

### `TU` Tape Drives

The TUA is a Massbus tape controller using a TM03 formatter. There
can be one per RH10 and it can support up to 8 drives.

|                        |                                    |
|------------------------|------------------------------------|
| `SET TUAn LOCKED`      | Sets the mag tape to be read only. |
| `SET TUAn WRITEENABLE` | Sets the mag tape to be writable.  |

###  DC10E Terminal Controller (Device 240)

The `DC` device was the standard terminal multiplexer for the KA10 and
KI10’s. Lines came in groups of 8. For modem control there was a
second port for each line. These could be offset by any number of
groups.

|                  |                                                     |
|------------------|-----------------------------------------------------|
| `SET DC LINES=n` | Set the number of lines on the DC10, multiple of 8. |
| `SET DC MODEM=n` | Set the start of where the modem control DEC10E lines begins. |

All terminal multiplexers must be attached in order to work. The
`ATTACH` command specifies the port to be used for Telnet sessions:

|                          |                       |
|--------------------------|-----------------------|
| `ATTACH <device> <port>` | Set up listening port |

where `port` is a decimal number between 1 and 65535 that is not being
used other TCP/IP activities.

Once attached and the simulator is running, the multiplexer listens
for connections on the specified port. It assumes that any incoming
connection is a Telnet connections. The connections remain open until
disconnected either by the Telnet client, a `SET <device> DISCONNECT`
command, or a `DETACH <device>` command.

|                             |                     |
|-----------------------------|---------------------|
| `SET <device> DISCONNECT=n` | Disconnect line `n` |

The `<device>` implements the following special `SHOW` commands:

|                             |                                              |
|-----------------------------|----------------------------------------------|
| `SHOW <device> CONNECTIONS` | Displays current connections to the `<device>` |
| `SHOW <device> STATISTICS`  | Displays statistics for active connections   |
| `SHOW <device> LOG`         | Display logging for all lines.               |

Logging can be controlled as follows:

|                               |                                    |
|-------------------------------|------------------------------------|
| `SET <device> LOG=n=filename` | Log output of line `n` to filename |
| `SET <device> NOLOG`          | Disable logging and close log file |

## Network Devices.

### `IMP` Interface Message Processor (Device 460)

This allows the KA/KI to connect to the Internet. Currently only
supported under ITS. ITS and other OSes that used the IMP did not
support modern protocols and typically required a complete rebuild to
change the IP address. Because of this the IMP processor includes
built in NAT and DHCP support. For ITS the system generated IP packets
which are translated to the local network. If the `HOST` is set to
`0.0.0.0` there will be no translation. If `HOST` is set to an IP
address then it will be mapped into the address set in IP. If `DHCP`
is enabled the `IMP` will issue a DHCP request at startup and set IP
to the address that is given. By default this device is not enabled.

|   |   |
|---|---|
| `SET IMP MAC=xx:xx:xx:xx:xx:xx` | Set the `MAC` address of the `IMP` to the value given. |
| `SET IMP IP=ddd.ddd.ddd.ddd/dd` | Set the external `IP` address of the `IMP` along with the net mask in bits. |
| `SET IMP GW=ddd.ddd.ddd.ddd` | Set the Gateway address for the `IMP`. |
| `SET IMP HOST=ddd.ddd.ddd.ddd` | Sets the IP address of the PDP-10 system. |
| `SET IMP DHCP` | Allows the `IMP` to acquire an IP address from the local network via `DHCP`. Only `HOST` must be set if this feature is used. |
| `SET IMP NODHCP` | Disables the `IMP` from making DHCP queries |
| `SET IMP DHCPIP=ddd.ddd.ddd.ddd` | The address of the `DHCP` server, generally this is for display only. |
| `SET IMP MIT` | Sets the `IMP` to look like the `IMP` used by `MIT` for ITS. |
| `SET IMP BBN` | Sets the `IMP` to behave like generic `BBN` `IMP`. (Not implemented yet). |
| `SET IMP WAITS` | Sets the `IMP` for Stanford style `IMP` for `WAITS`. |

The `IMP` device must be attached to a `TAP` or `NAT` interface. To
determine which devices are available use the `SHOW ETHERNET` to list
the potential interfaces. Check out the `Readme_ethernet.txt` from the
doc directory.

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
HOST=<ip>`, the `IMP` will direct all traffic it sees to this
address. If this address is not the same as the address of the system
as seen by the network, then this address can be set with `SET IMP
IP=<ip>`, and `SET IMP GW=<ip>`, or `SET IMP DHCP` which will allow
the `IMP` to request an address from a local DHCP server. The `IMP`
will translate the packets it receives/sends to look like the appeared
from the desired address. The `IMP` will also correctly translate FTP
requests in this configuration.

When running under a `TUN` interface, `IMP` is on a virtual `10.0.2.0`
network. The default gateway is `10.0.2.1`, with the default `IMP` at
`10.0.2.15`. For this mode DHCP can be used.

# Symbolic Display and Input

The KI10 simulator implements symbolic display and input. These are
controlled by the following switches to the `EXAMINE` and `DEPOSIT`
commands:

|    |    |
|----|----|
| `-v` | Looks up the address via translation, will return nothing if address is not valid. |
| `-u` | With the `-v` option, used user space instead of executive space. |
| `-a` | Display/Enter ASCII data.                                         |
| `-p` | Display/Enter packed ASCII data.                                  |
| `-c` | Display/Enter Sixbit Character data.                              |
| `-m` | Display/Enter Symbolic instructions                               |
|      | Display/Enter Octal data.                                         |

Symbolic instructions can be of the formats:

- `opcode ac, operand`
- `opcode operands`
- `i/o_opcode device, address`

Operands can be one or more of the following in order:

- Optional `@` for indirection.
- `+` or `-` to set sign.
- Octal number
- Optional `(AC)` for indexing.

Breakpoints can be set at real memory address. The PDP-10 supports
three types of breakpoints. Execution, Memory Access and Memory
Modify. The default is Execution. Adding `-R` to the breakpoint
command will stop the simulator on access to that memory location,
either via fetch, indirection or operand access. Adding `-w` will stop
the simulator when the location is modified.

The simulator can load `RIM` files, `SAV` files, `EXE` files, WAITS
Octal `DMP` files, and MIT `SBLK` files.

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

The PDP-10 simulator allows for memory reference and memory modify
breakpoints with the `-r` and `-w` options given to the break command.
