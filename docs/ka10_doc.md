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

[1. Introduction 4](#introduction)

[2. Simulator Files 5](#simulator-files)

[3. KA10 Features 7](#ka10-features)

[3.1 CPU 8](#cpu)

[3.2 Unit record I/O Devices 11](#unit-record-io-devices)

[3.2.1 Console Teletype (CTY) (Device 120) 11](#console-teletype-cty-device-120)

[3.2.2 Paper Tape Reader (PTR) (Device 104) 11](#paper-tape-reader-ptr-device-104)

[3.2.3 Paper Tape Punch (PTP) (Device 100) 11](#paper-tape-punch-ptp-device-100)

[3.2.4 Card Reader (CR) (Device 150) 11](#card-reader-cr-device-150)

[3.2.5 Card Punch (CP) (Device 110) 12](#card-punch-cp-device-110)

[3.2.6 Line Printer (LPT) (Device 124) 12](#line-printer-lpt-device-124)

[3.2.7 Type 340 Graphics display (DPY) (Device 130) 13](#type-340-graphics-display-dpy-device-130)

[3.2.8 Stanford Triple III Display (III) (Device 430) 13](#stanford-triple-iii-display-iii-device-430)

[3.2.9 DBK Stanford microswitch keyboard scanner (DKB) (Device 310) 13](#dbk-stanford-microswitch-keyboard-scanner-dkb-device-310)

[3.2.10 DK10 Timer Module (DK) (Device 070) 13](#dk10-timer-module-dk-device-070)

[3.2.11 TM10 Magnetic Tape (MTA) (Device 340,344) 13](#tm10-magnetic-tape-mta-device-340344)

[3.2.12 TD10 DECTape (DT) (Device 320,324) 13](#td10-dectape-dt-device-320324)

[3.2.13 PCLK Petit Calendar Clock (Device 730) 14](#pclk-petit-calendar-clock-device-730)

[3.2.14 PD DeCoriolis clock (Device 500) 14](#pd-decoriolis-clock-device-500)

[3.2.15 A/D Input Multiplexer (IMX) (Device 574) 14](#ad-input-multiplexer-imx-device-574)

[3.3 Disk I/O Devices 14](#disk-io-devices)

[3.3.1 FHA Disk Controller (Device 170) 14](#fha-disk-controller-device-170)

[3.3.2 DPA/DPB Disk Controller (Device 250/254) 15](#dpadpb-disk-controller-device-250254)

[3.3.3 PMP Disk Controller (Device 500/504) 15](#pmp-disk-controller-device-500504)

[3.3.4 Systems Concepts DC-10 Disk Controller (Device 610/614) 15](#systems-concepts-dc-10-disk-controller-device-610614)

[3.3.5 DDC10 Drum controller for Tenex 15](#ddc10-drum-controller-for-tenex)

[3.4 Massbus Devices 15](#massbus-devices)

[3.4.1 RP Disk drives 16](#rp-disk-drives)

[3.4.2 RS Disk drives 16](#rs-disk-drives)

[3.4.3 TU Tape drives. 16](#tu-tape-drives.)

[3.5 Terminal Multiplexer I/O Devices 16](#terminal-multiplexer-io-devices)

[3.5.1 DC10E Terminal Controller (Device 240) 17](#dc10e-terminal-controller-device-240)

[3.5.2 TK Knight kludge, TTY scanner (Device 0600) 17](#tk-knight-kludge-tty-scanner-device-0600)

[3.5.3 MTY Morton terminal multiplexer (Device 400) 17](#mty-morton-terminal-multiplexer-device-400)

[3.5.4 DPK DK-10 Datapoint kludge (Device 604) 17](#dpk-dk-10-datapoint-kludge-device-604)

[3.6 Network Devices. 17](#network-devices.)

[3.6.1 IMP Interface Message Processor (Device 460) 17](#imp-interface-message-processor-device-460)

[3.6.2 CH Chaosnet interface (Device 470) 19](#ch-chaosnet-interface-device-470)

[3.7 PDP6 Devices. 19](#pdp6-devices.)

[3.7.1 DCT Type 136 Data Control (Device 200/204) 19](#dct-type-136-data-control-device-200204)

[3.7.2 DTC Type 551 Dectape controller (Device 210) 19](#dtc-type-551-dectape-controller-device-210)

[3.7.3 MTC Type 516 Magtape controller (Device 220) 20](#mtc-type-516-magtape-controller-device-220)

[3.7.4 DSK Type 270 Disk controller (Device 270) 20](#dsk-type-270-disk-controller-device-270)

[3.7.5 DCS Type 630 Terminal Multiplexer (Device 300) 20](#dcs-type-630-terminal-multiplexer-device-300)

[4. Symbolic Display and Input 21](#symbolic-display-and-input)

[5. OS specific configurations 22](#os-specific-configurations)

[5.1 TOPS10 22](#tops10)

[5.2 ITS 22](#its)

[5.3 WAITS 22](#waits)

#  Introduction

Originally the DEC PDP10 computer started as the PDP6. This was a 36 bit computer that was designed for timesharing, which was introduced in 1964. The original goal of the machine was to allow for processing of many 6 bit characters at a time. 36 bits were also common in machines like the IBM 7090, GE 645, and Univac 11xx lines. Several systems influenced the design of the PDP6, like CTSS, Lisp, support for larger memory. The PDP6 was canceled by DEC due to production problems. The engineers designed a smaller replacement, which happened to be a 36 bit computer that looked very much like the PDP6. This was called the PDP10. Later renamed to "DECSystem-10". The system supported up to 256K words of memory.

The first PDP10 was labeled KA10, and added a few instructions to the PDP6. Both the PDP6 and PDP10 used a base and limit relocation scheme. The KA10 generally offered two registers, one of user data and the second for user shared code. These were referred to the Low-Segment and High-Segment, the High-Segment could be shared with several jobs. The next version was called KI10 for Integrated. This added support for paging and double precision floating point instructions. It also added 4 sets of registers to improve context switching time. It could also support up to 4Mega words of memory. Following the KI10 was the KL10 (for Low-Cost). The KL10 added double precision integer instructions and instructions to improve COBOL performance. This was the first version which was microcoded. The KL10 was extended to support user programs larger then 256k. The final version to make it to market was the KS10 (for Small), this was a bit-slice version of the PDP10 which used UNIBUS devices which were cheaper then the KL10 devices.

The original operating system for the PDP6/PDP10 was just called "Monitor". It was designed to fit into 6K words. Around the 3rd release Swapping was added. The 6'th release saw the addition of virtual memory. Around the 4'th release it was given the name "TOPS-10". Around this time BBN was working on a paging system and implemented it on the PDP10. This was called "Tenex". This was later adopted by DEC and became "Tops-20".

During the mid 60's a group at MIT, who where not happy with how Multics was being developed decided to create their own operating system which they called Incompatible Timesharing System "ITS", was a play on the original project called Compatible Timesharing System "CTSS". CTSS was implemented by MIT on their IBM 7090 as an experimental system that allowed multiple time sharing users to co-exist on the same machine running batch processing. Hence the term Compatable.

Also during the mid 60's a group at Stanford Artificial Intelligence Laboratory (SAIL), started with a version of TOPS-10 and heavily modified it to become WAITS.

During the 70's Tymshare starting with DEC TOPS-10 system modified it to support random access files, paging with working sets and spawn-able processes. This ran on the KI10, KL10 and KS10.

The PDP10 was ultimately replaced by the VAX.

#  Simulator Files

To compile the DEC 10/KA10 simulator, you must define USE_INT64 as part of the compilation command line.

|  |  |  |
|----|----|----|
| ***Subdirectory*** | ***File*** | ***Contains*** |
| **PDP10** | kx10_defs.h | KA10 simulator definitions |
|  | kx10_cpu.c | KA10 CPU. |
|  | kx10_df.c | DF10/C controller. |
|  | kx10_rh.c | RH10 Controller. |
|  | kx10_diskh | Disk formatter definitions |
|  | kx10_disk.c | Disk formatter routine. |
|  | kx10_sys.c | KA10 System interface |
|  | kx10_cty.c | Console Terminal Interface. |
|  | kx10_pt.c | Paper Tape Reader and Punch |
|  | kx10_rc.c | RC10 Disk controller, RD10 and RM10 Disks |
|  | kx10_dp.c | RP10 Disk controller, RP01/2/3 Disks. |
|  | kx10_rp.c | RH10 Disk controller, RP04/5/6 Disks. |
|  | kx10_rs.c | RH10 Disk controller RS04 Fixed head Disks. |
|  | kx10_dt.c | TD10 Dectape controller. |
|  | kx10_mt.c | TM10A/B Magnetic tape controller. |
|  | kx10_tu.c | RH10/TM03 Magnetic tape controller. |
|  | kx10_lp.c | Line printer. |
|  | kx10_cr.c | Punch card reader. |
|  | kx10_cp.c | Punch card punch. |
|  | kx10_dk.c | DK10 interval timer. |
|  | kx10_dc.c | DC10 communications controller. |
|  | kx10_ddc.c | DDC-10 Drum controller |
|  | ka10_tk10.c | Knight kludge, TTY scanner |
|  | ka10_mty.c | MTY, Morton terminal multiplexer |
|  | ka10_dpk.c | DK-10 Datapoint Kludge |
|  | ka10_dpy.c | DEC 340 graphics interface. |
|  | ka10_dkb.c | Stanford Microswitch Scanner for III display |
|  | ka10_iii.c | Stanford Triple III display. |
|  | ka10_stk.c | Stanford keyboard for 340. |
|  | ka10_pclk.c | Petit Calendar Clock. |
|  | ka10_pd.c | DeCoriolis clock. |
|  | ka10_imx.c | Analog input device. |
|  | ka10_ten11.c | PDP11 interface. |
|  | kx10_lights.c | Front panel interface. |
|  | kx10_imp.c | IMP10 interface to ethernet. |
|  | ka10_ch10.c | Chaosnet 10 interface. |
|  | ka10_pmp.c | PMP Interface to IBM 3330 drives. |
|  | ka10_ai.c | System Concepts DC10 IBM2314 disk controller. |
|  | pdp6_dct.c | PDP6 Data Controller |
|  | pdp6_dtc.c | PDP6 551 DECtape controller |
|  | pdp6_mtc.c | PDP6 556 Magtape controller |
|  | pdp6_dsk.c | PDP6 270 Disk controller |
|  | pdp6_dcs.c | PDP7 630 Terminal multiplexer |

#  

#  KA10 Features

The KA10 simulator is configured as follows:

<table>
<colgroup>
<col style="width: 29%" />
<col style="width: 70%" />
</colgroup>
<tbody>
<tr>
<td><strong>Device Name(s)</strong></td>
<td><strong>Simulates</strong></td>
</tr>
<tr>
<td><strong>CPU</strong></td>
<td>KA10 CPU with 256KW of memory</td>
</tr>
<tr>
<td><strong>CTY</strong></td>
<td>Console TTY.</td>
</tr>
<tr>
<td><strong>PTP</strong></td>
<td>Paper tape punch</td>
</tr>
<tr>
<td><strong>PTR</strong></td>
<td>Paper tape reader.</td>
</tr>
<tr>
<td><strong>LPT</strong></td>
<td>LP10 Line Printer</td>
</tr>
<tr>
<td><strong>CR</strong></td>
<td>CR10 Card reader.</td>
</tr>
<tr>
<td><strong>CP</strong></td>
<td>CP10 Card punch</td>
</tr>
<tr>
<td><strong>MTA</strong></td>
<td>TM10A/B Tape controller.</td>
</tr>
<tr>
<td><p><strong>DPA</strong></p>
<p><strong>DPB</strong></p></td>
<td>RP10 Disk controller</td>
</tr>
<tr>
<td><strong>FSA</strong></td>
<td>RS04 Disk controller via RH10</td>
</tr>
<tr>
<td><p><strong>RPA</strong></p>
<p><strong>RPB</strong></p>
<p><strong>RPC</strong></p>
<p><strong>RPD</strong></p></td>
<td>RH10 Disk controllers via RH10</td>
</tr>
<tr>
<td><strong>PMP</strong></td>
<td>PMP IBM 3330 disk controller</td>
</tr>
<tr>
<td><strong>AI</strong></td>
<td>System Concepts DC10 IBM 2314 disk controller</td>
</tr>
<tr>
<td><strong>TUA</strong></td>
<td>TM02 Tape controller via RH10</td>
</tr>
<tr>
<td><strong>FHA</strong></td>
<td>RC10 Disk controller.</td>
</tr>
<tr>
<td><strong>DDC</strong></td>
<td>DDC10 Disk controller.</td>
</tr>
<tr>
<td><strong>DT</strong></td>
<td>TD10 Dectape Controller.</td>
</tr>
<tr>
<td><strong>DC</strong></td>
<td>DC10 Communications controller.</td>
</tr>
<tr>
<td><strong>DK</strong></td>
<td>Clock timer module.</td>
</tr>
<tr>
<td><strong>PCLK</strong></td>
<td>Petit Calendar Clock.</td>
</tr>
<tr>
<td><strong>PD</strong></td>
<td>Coriolis Clock</td>
</tr>
<tr>
<td><strong>IMP</strong></td>
<td>IMP network interface</td>
</tr>
<tr>
<td><strong>CH</strong></td>
<td>CH10 Chaosnet interface</td>
</tr>
<tr>
<td><strong>IMX</strong></td>
<td>A/D input multiplexer</td>
</tr>
<tr>
<td><strong>TK</strong></td>
<td>Knight kludge, TTY scanner</td>
</tr>
<tr>
<td><strong>MTY</strong></td>
<td>MTY, Morton terminal multiplexer</td>
</tr>
<tr>
<td><strong>DPK</strong></td>
<td>DK-10 Datapoint Kludge</td>
</tr>
<tr>
<td><strong>DKB</strong></td>
<td>Stanford Microswitch Scanner.</td>
</tr>
<tr>
<td><strong>III</strong></td>
<td>Stanford Triple III Display.</td>
</tr>
<tr>
<td><strong>TEN11</strong></td>
<td>PDP11 interface</td>
</tr>
<tr>
<td><strong>AUXCPU</strong></td>
<td>PDP6 interface</td>
</tr>
<tr>
<td><strong>DCT</strong></td>
<td>PDP6 Data Control Type 136</td>
</tr>
<tr>
<td><strong>DTC</strong></td>
<td>PDP6 Type 551 Dectape controller</td>
</tr>
<tr>
<td><strong>MTC</strong></td>
<td>PDP6 Type 516 Magtape controller</td>
</tr>
<tr>
<td><strong>DSK</strong></td>
<td>PDP6 Type 270 disk controller</td>
</tr>
<tr>
<td><strong>DCS</strong></td>
<td>PDP6 Type 630 Terminal Multiplexer</td>
</tr>
</tbody>
</table>

##  CPU

The CPU options include setting memory size and O/S customization.

|                 |                                              |           |
|-----------------|----------------------------------------------|-----------|
| SET CPU 16K     | Sets memory to 16K                           |           |
| SET CPU 32K     | Sets memory to 32K                           |           |
| SET CPU 48K     | Sets memory to 48K                           |           |
| SET CPU 64K     | Sets memory to 64K                           |           |
| SET CPU 96K     | Sets memory to 96K                           |           |
| SET CPU 128K    | Sets memory to 128K                          |           |
| SET CPU 256K    | Sets memory to 256K                          |           |
| SET CPU 512K    | Sets memory to 512K                          | ITS & BBN |
| SET CPU 768K    | Set memory to 768K                           | ITS & BBN |
| SET CPU 1024K   | Set memory to 1024K                          | ITS & BBN |
| SET CPU NOMAOFF | Sets traps to default of 040                 |           |
| SET CPU MAOFF   | Move trap vectors from 040 to 0140           | WAITS     |
| SET CPU ONESEG  | Sets to one segment register                 |           |
| SET CPU TWOSEG  | Sets to 2 segment registers                  |           |
| SET CPU ITS     | Adds ITS Pager and instruction support to KA | ITS       |
| SET CPU NOMPX   | Disables MPX interrupt support for ITS       |           |
| SET CPU MPX     | Enables MPX interrupt support for ITS        | ITS       |
| SET CPU WAITS   | Add support for special WAITS instructions   | WAITS     |
| SET CPU NOWAITS | Disables special WAITS instructions.         |           |
| SET CPU BBN     | Enables BBN pager and Tenex support          | TENEX     |
| SET CPU NOIDLE  | Disables Idle detection                      |           |
| SET CPU IDLE    | Enables Idle detection                       |           |

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

| Name        | Size | Comments                      | OS Type |
|-------------|------|-------------------------------|---------|
| PC          | 18   | Program Counter               |         |
| FLAGS       | 18   | Flags                         |         |
| FM0-FM17    | 36   | Fast Memory                   |         |
| SW          | 36   | Console SW Register           |         |
| MI          | 36   | Memory Indicators             |         |
| AS          | 18   | Console AS register           |         |
| ABRK        | 1    | Address break                 |         |
| ACOND       | 5    | Address Condition switches    |         |
| PIR         | 8    | Priority Interrupt Request    |         |
| PIH         | 8    | Priority Interrupt Hold       |         |
| PIE         | 8    | Priority Interrupt Enable     |         |
| PIENB       | 1    | Enable Priority System        |         |
| BYF5        | 1    | Byte Flag                     |         |
| UUO         | 1    | UUO Cycle                     |         |
| PL          | 18   | Program Limit Low             |         |
| PH          | 18   | Program Limit High            |         |
| RL          | 18   | Program Relation Low          |         |
| RH          | 18   | Program Relation High         |         |
| PFLAG       | 1    | Relocation enable             |         |
| PUSHOVER    | 1    | Push overflow flag            |         |
| MEMPROT     | 1    | Memory protection flag        |         |
| NXM         | 1    | Non-existing memory access    |         |
| CLK         | 1    | Clock interrupt               |         |
| OV          | 1    | Overflow enable               |         |
| FOV         | 1    | Floating overflow enable      |         |
| APRIRQ      | 3    | APR Interrupt number          |         |
| PAGE_ENABLE | 1    | Paging enabled                | ITS/BBN |
| PAGE_FAULT  | 1    | Page fault                    | ITS/BBN |
| AC_STACK    | 18   | AC Stack                      | ITS/BBN |
| PAGE_RELOAD | 18   | Page reload                   | ITS/BBN |
| FAULT_DATA  | 36   | Page fault data               | ITS/BBN |
| TRP_FLG     | 1    | Trap flag                     | ITS/BBN |
| LAST_PAGE   | 9    | Last page                     | ITS/BBN |
| EXEC_MAP    | 8    | Executive mapping             | BBN     |
| NXT_WR      | 1    | Map next write                | BBN     |
| MON_BASE    | 8    | Monitor base                  | BBN     |
| USER_BASE   | 8    | User base                     | BBN     |
| USER_LIMIT  | 3    | User limit                    | BBN     |
| PER_USER    | 36   | Per user data                 | BBN     |
| DBR1        | 18   | Paging control register 1     | ITS     |
| DBR2        | 18   | Paging control register 2     | ITS     |
| DBR3        | 18   | Paging control register 3     | ITS     |
| JPC         | 18   | Last Jump instruction address | ITS     |
| AGE         | 4    | Age                           | ITS     |
| FAULT_ADDR  | 18   | Fault address                 | ITS     |
| OPC         | 36   | Saved PC and flags            | ITS     |
| MAR         | 18   | Memory Access Register        | ITS     |
| QUA_TIME    | 36   | Quantum time.                 | ITS     |

The CPU can maintain a history of the most recently executed instructions.

This is controlled by the SET CPU HISTORY and SHOW CPU HISTORY commands:

|                      |                                      |
|----------------------|--------------------------------------|
| SET CPU HISTORY      | clear history buffer                 |
| SET CPU HISTORY=0    | disable history                      |
| SET CPU HISTORY=*n*  | enable history, length = n           |
| SHOW CPU HISTORY     | print CPU history                    |
| SHOW CPU HISTORY=*n* | print first n entries of CPU history |

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

###  Line Printer (LPT) (Device 124)

The line printer (LPT) writes data to a disk file as ASCII text with terminating newlines. Currently set to handle standard signals to control paper advance.

|  |  |
|----|----|
| SET LPT*n* LC | Allow printer to print lower case |
| SET LPT*n* UC | Print only upper case |
| SET LPT*n* UTF8 | Print control characters as UTF8 characters. |
| SET LPT*n* LINESPERPAGE=*n* | Sets the number of lines before an auto form feed is generated. There is an automatic margin of 6 lines. There is a maximum of 100 lines per page. |
| SET LPT*n* DEV=*n* | Sets device number to *n*(octal). |

These characters control the skipping of various number of lines.

|           |                                            |
|-----------|--------------------------------------------|
| Character | Effect                                     |
| 014       | Skip to top of form.                       |
| 013       | Skip mod 20 lines.                         |
| 020       | Skip mod 30 lines.                         |
| 021       | Skip to even line.                         |
| 022       | Skip to every 3 line                       |
| 023       | Same as line feed (12), but ignore margin. |

###  Type 340 Graphics display (DPY) (Device 130)

Primarily useful under ITS, TOPS10 does have some limited support for this device. By default this device is not enabled. When enabled and commands are sent to it a graphics windows will display.

###  Stanford Triple III Display (III) (Device 430)

Primarily useful under WAITS. By default this device is not enabled. Used DBK device for keyboard input.

###  DBK Stanford microswitch keyboard scanner (DKB) (Device 310)

Used by III to handle input. Currently only one keyboard is supported.

###  DK10 Timer Module (DK) (Device 070)

The DK10 timer module does not have any settable options.

###  TM10 Magnetic Tape (MTA) (Device 340,344)

The TM10 was the most common tape controller on KA10 and KI10. The TM10 came in two models, the TM10A and the TM10B. The B model added a DF10 which allowed the tape data to be transferred without intervention of the CPU. The device has 2 options.

|                |                                             |
|----------------|---------------------------------------------|
| SET MTA TYPE=t | Sets the type of the controller to A or B   |
| SET MTA MPX=#  | For ITS set the MPX interrupt to channel \# |

Each individual tape drive support several options: MTA used as an example.

|                        |                                           |
|------------------------|-------------------------------------------|
| SET MTA*n* 7T          | Sets the Mag tape unit to 7 track format. |
| SET MTA*n* 9T          | Sets the Mag tape unit to 9 track format. |
| SET MTA*n* LOCKED      | Sets the mag tape to be read only.        |
| SET MTA*n* WRITEENABLE | Sets the mag tape to be writable.         |

###  TD10 DECTape (DT) (Device 320,324)

The TD10 was the standard Dectape controller for KA and KI’s. This controller loads the tape into memory and uses the buffered version. For ITS you needed to connect it to an MPX channel to handle I/O.

|              |                                             |
|--------------|---------------------------------------------|
| SET DT MPX=# | For ITS set the MPX interrupt to channel \# |

Each individual tape drive support several options: DT used as an example.

|                       |                                                  |
|-----------------------|--------------------------------------------------|
| SET DT*n* LOCKED      | Sets the mag tape to be read only.               |
| SET DT*n* WRITEENABLE | Sets the mag tape to be writable.                |
| SET DT*n* 18b         | Default, tapes are considered to be 18bit tapes. |
| SET DT*n* 16b         | Tapes are converted from 16 bit to 18bit.        |
| SET DT*n* 12b         | Tapes are converted from 12 bit to 18bit.        |

###  PCLK Petit Calendar Clock (Device 730)

This device keeps track of time and date for WAITS. It was custom designed by Peter Petit. The device supports two settings “ON” and “OFF”. By default this device is disabled.

### PD DeCoriolis clock (Device 500)

This is a device which keeps track of the time and date. An access will return the number of ticks since the beginning of the year. There are 60 ticks per second. The device was made by Paul DeCoriolis at MIT. By default this device is disabled.

###  A/D Input Multiplexer (IMX) (Device 574)

Provides 128 channels of 12-bit analog to digital converters. Currently only input from USB game controllers are supported. To map game inputs to channels, use

SET IMX CHANNEL=123;UNIT0;AXIS1

Where 123 is the A/D channel in octal notation, UNIT0 is first USB game controller, and AXIS1 is the second axis on that controller. Add ;NEGATE to invert the signal.

##  Disk I/O Devices

The PDP10 supported many disk controllers.

###  FHA Disk Controller (Device 170)

The RC10 disk controller used a DF10 to transfer data to memory. There were two types of disks that could be connected to the RC10 the RD10 and RM10. The RD10 could hold up to 512K words of data. The RM10 could hold up to 345K words of data.

Each individual disk drive support several options: FHA used as an example.

|                        |                                 |
|------------------------|---------------------------------|
| SET FHA*n* RD10        | Sets this unit to be an RD10.   |
| SET FHA*n* RM10        | Sets this unit to be an RM10.   |
| SET FHA*n* LOCKED      | Sets this unit to be read only. |
| SET FHA*n* WRITEENABLE | Sets this unit to be writable.  |

###  DPA/DPB Disk Controller (Device 250/254)

The RP10 disk controller used a DF10 to transfer data to memory. There were three types of disks that could be connected to the RP10 the RP01, RP02, RP03. The RP01 held up to 1.2M words, the RP02 5.2M words, RP03 10M words. The second controller is DPB. Disks can be stored in one of several file formats, SIMH, DBD9 and DLD9. The later two are for compatibility with other simulators.

Each individual disk drive support several options: DPA used as an example.

|  |  |
|----|----|
| SET DPA*n* RP01 | Sets this unit to be an RP01. |
| SET DPA*n* RP02 | Sets this unit to be an RP02. |
| SET DPA*n* RP03 | Sets this unit to be an RP03. |
| SET DPA*n* HEADERS | Enables the RP10 to execute write headers function. |
| SET DPA*n* NOHEADERS | Prevents the RP10 to execute write headers function. |
| SET DPA*n* LOCKED | Sets this unit to be read only. |
| SET DPA*n* WRITEENABLE | Sets this unit to be writable. |

To attach a disk use the ATTACH command:

|  |  |
|----|----|
| ATTACH DPA*n* *\<file\>* | Attaches a file |
| ATTACH DPA*n* -f *\<format\> \<file\>* | Attaches a file with the given format. |
| ATTACH DPA*n* -n \<file\> | Creates a blank disk. |

Format can be SIMH, DBD9, DLD9.

###  PMP Disk Controller (Device 500/504)

The PMP disk controller allowed for IBM type drives to be connected to the PDP10. This is the controller used by WAITS. This devices is disabled by default.

Each individual disk drive support several options: DPA used as an example.

|                      |                                                      |
|----------------------|------------------------------------------------------|
| SET PMP*n* TYPE=type | Sets this unit to be of type type. Generally 3330-2  |
| SET PMP*0* DEV=##    | Sets the addresses of all disks to start at \## hex. |
| SET PMP*n* DEV=##    | Sets this unit to be at address \##.                 |

###  Systems Concepts DC-10 Disk Controller (Device 610/614)

Systems Concepts DC-10 disk controller allowed IBM 2314 compatible disk drives to be attached to the MIT AI Lab PDP-10 running ITS. This device is disabled by default.

###  DDC10 Drum controller for Tenex

DEC RES-10 drum controller, is used by Tenex for swapping. This device is disabled by default.

##  Massbus Devices

Massbus devices are attached via RH10’s. Devices start a device 270 and go up (274,360,364,370,374). For Tops10 devices need to be in a the order RS, RP, TU. By default RS disks are disabled. The first unit which is not enabled will get device 270, units will be assigned the next available address automatically. The device configuration must match that which is defined in the OS.

###  RP Disk drives 

Up to 4 strings of up to 8 RP drives can be configured. By default the 3<sup>rd</sup> and 4<sup>th</sup> controllers are disabled. These are addresses as RPA, RPB, RPC, RPD. Disks can be stored in one of several file formats, SIMH, DBD9 and DLD9. The later two are for compatibility with other simulators.

|                        |                                          |
|------------------------|------------------------------------------|
| SET RPA*n* RP04        | Sets this unit to be an RP04.(19MWords)  |
| SET RPA*n* RP06        | Sets this unit to be an RP06.(39MWords)  |
| SET RPA*n* RP07        | Sets this unit to be an RP07.(110MWords) |
| SET RPA*n* LOCKED      | Sets this unit to be read only.          |
| SET RPA*n* WRITEENABLE | Sets this unit to be writable.           |

### 

To attach a disk use the ATTACH command:

|  |  |
|----|----|
| ATTACH RPA*n* *\<file\>* | Attaches a file |
| ATTACH RPA*n* -f *\<format\> \<file\>* | Attaches a file with the given format. |
| ATTACH RPA*n* -n \<file\> | Creates a blank disk. |

Format can be SIMH, DBD9, DLD9.

### RS Disk drives

One string of up to 8 RS drives can be configured. These drives are fixed head swapping disks. By default they are disabled.

|                        |                                          |
|------------------------|------------------------------------------|
| SET FSA*n* RS03        | Sets this unit to be an RS03.(262KWords) |
| SET FSA*n* RS04        | Sets this unit to be an RS04.(262KWords) |
| SET FSA*n* LOCKED      | Sets this unit to be read only.          |
| SET FSA*n* WRITEENABLE | Sets this unit to be writable.           |

### 

###  TU Tape drives.

The TUA is a Mass bus tape controller using a TM03 formatter. There can be one per RH10 and it can support up to 8 drives.

|                        |                                    |
|------------------------|------------------------------------|
| SET TUA*n* LOCKED      | Sets the mag tape to be read only. |
| SET TUA*n* WRITEENABLE | Sets the mag tape to be writable.  |

##  Terminal Multiplexer I/O Devices

All terminal multiplexers must be attached in order to work. The ATTACH command specifies the port to be used for Telnet sessions:

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
| SHOW \<device\> LOG | Display logging for all lines. |

Logging can be controlled as follows:

|                                   |                                      |
|-----------------------------------|--------------------------------------|
| SET \<device\> LOG=*n*=*filename* | Log output of line *n* to *filename* |
| SET \<device\> NOLOG              | Disable logging and close log file   |

###  DC10E Terminal Controller (Device 240)

The DC device was the standard terminal multiplexer for the KA10 and KI10’s. Lines came in groups of 8. For modem control there was a second port for each line. These could be offset by any number of groups.

|  |  |
|----|----|
| SET DC LINES=*n* | Set the number of lines on the DC10, multiple of 8. |
| SET DC MODEM=*n* | Set the start of where the modem control DEC10E lines begins. |

###  TK Knight kludge, TTY scanner (Device 0600)

This is a device with 16 terminal ports. It's specific to the MIT AI lab and Dynamic Modeling PDP-10s. By default this device is disabled.

###  MTY Morton terminal multiplexer (Device 400)

This is a device with 32 high-speed terminal lines. It's specific to the MIT Mathlab and Dynamic Modeling PDP-10s. By default this device is disabled.

###  DPK DK-10 Datapoint kludge (Device 604)

The System Concepts DK-10, also known as the Datapoint Kludge is a device with 16 terminal ports. This device is specific to ITS, and is disabled by default.

##  Network Devices.

###  IMP Interface Message Processor (Device 460)

This allows the KA/KI to connect to the Internet. Currently only supported under ITS. ITS and other OS that used the IMP did not support modern protocols and typically required a complete rebuild to change the IP address. Because of this the IMP processor includes built in NAT and DHCP support. For ITS the system generated IP packets which are translated to the local network. If the HOST is set to 0.0.0.0 there will be no translation. If HOST is set to an IP address then it will be mapped into the address set in IP. If DHCP is enabled the IMP will issue a DHCP request at startup and set IP to the address that is provided. DHCP is enabled by default.

|  |  |
|----|----|
| SET IMP MAC=*xx:xx:xx:xx:xx:xx* | Set the MAC address of the IMP to the value given. |
| SET IMP IP=*ddd.ddd.ddd.ddd*/*dd* | Set the external IP address of the IMP along with the net mask in bits. |
| SET IMP GW=*ddd.ddd.ddd.ddd* | Set the Gateway address for the IMP. |
| SET IMP HOST=*ddd.ddd.ddd.ddd* | Sets the IP address of the PDP10 system. |
| SET IMP DHCP | Allows the IMP to acquire an IP address from the local network via DHCP. Only HOST must be set if this feature is used. |
| SET IMP NODHCP | Disables the IMP from making DHCP queries |
| SET IMP ARP=*ddd.ddd.ddd.ddd= xx:xx:xx:xx:xx:xx* | Creates a static ARP entry for the IP address with the indicated MAC address. |
| SET IMP MIT | Sets the IMP to look like the IMP used by MIT for ITS. |
| SET IMP MPX=# | For ITS set the MPX interrupt to channel \# |
| SET IMP BBN | Sets the IMP to behave like generic BBN IMP. (Not implemented yet). |
| SET IMP WAITS | Sets the IMP for Stanford style IMP for WAITS. |
| SHOW IMP ARP | Displays the IP address to MAC address table. |

The IMP device must be attached to an available Ethernet interface. To determine which devices are available use the “SHOW ETHERNET” to list the potential interfaces. Check out the “0readme_ethernet.txt” from the top of the source directory.

The IMP device can be configured in several ways. Ether it can connect directly to an ethernet port (via TAP) or it can be connected via a TUN interface. If configured via TAP interface the IMP will behave like any other ethernet interface and if asked grab it’s own address. In environments where this is not desired the TUN interface can be used. When configured under a TUN interface the simulated network is a collection of ports on the local host. These can be mapped based on configuration options, see the 0readme_ethernet.txt file as to options.

With the IMP interface, the IP address of the simulated system is static, and under ITS is configured into the system at compile time. This address should be given to the IMP with the “set imp host=\<ip\>”, the IMP will direct all traffic it sees to this address. If this address is not the same as the address of the system as seen by the network, then this address can be set with “set imp ip=\<ip\>”, and “set imp gw=\<ip\>”, or “set imp dhcp” which will allow the IMP to request an address from a local DHCP server. The IMP will translate the packets it receives/sends to look like the appeared from the desired address. The IMP will also correctly translate FTP requests in this configuration.

When running under a TUN interface, IMP is on a virtual 10.0.2.0” network. The default gateway is “10.0.2.1”, with the default IMP at “10.0.2.15”. For this mode DHCP can be used.

###  CH Chaosnet interface (Device 470)

Chaos net was another methods of network access for ITS. Chaosnet can be connected to another ITS system or a VAX/PDP11 running BSD Unix or VMS. Chaosnet runs over UDP protocol under UNIX. You must specify a node number, peer to talk to and your local UDP listening port. All UDP packets are sent to the peer for further processing.

|                                    |                                       |
|------------------------------------|---------------------------------------|
| SET CH NODE=*n*                    | Set the Node number for this system.  |
| SET CH PEER=*ddd.ddd.ddd.ddd:dddd* | Set the Peer address and port number. |

The CH device must be attached to a UDP port number. This is where it will receive UDP packets from it’s peer.

##  PDP6 Devices.

These devices could be attached to the KA10. Stanford WAITS used them. Early versions of TOPS10 could also run with these devices. By default these devices are disabled.

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

###  DCS Type 630 Terminal Multiplexer (Device 300)

See section on terminal multiplexers on generic setup.

#  Symbolic Display and Input

The KA10 simulator implements symbolic display and input. These are controlled by the following switches to the EXAMINE and DEPOSIT commands:

|  |  |
|----|----|
| -v | Looks up the address via translation, will return nothing if address is not valid. |
| -u | With the -v option, used user space instead of executive space. |
| -a | Display/Enter ASCII data. |
| -p | Display/Enter packed ASCII data. |
| -c | Display/Enter Sixbit Character data. |
| -m | Display/Enter Symbolic instructions |
|  | Display/Enter Octal data. |

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

The PDP10 simulator allows for Memory reference and memory modify breakpoints with the -r and -w options given to the break command.

#  OS specific configurations

##  TOPS10

The default configuration supports Tops10 so there is no extra configuration required.

##  ITS

To run ITS the CPU must be “set cpu ITS MPX 1024K”, this will enable the ITS pager, special instructions and the interrupt multiplexer. DK should be disabled, along with all Mass bus devices. The following devices should be enabled, PD, (also optionally) DPY, STK, TK, MTY, TEN11, AUXCPU, IMP, CH. ITS used a second interrupt multiplexer so some devices need to be configured to specific channels. TM10 (MTA) was at MPX=7, Dectapes (DT) was at MPX=6, The IMP was at MPX=4.

##  WAITS

Waits has two configurations, one uses the default two relocation registers. This can be done with “set cpu waits maoff”, The PMP disk is the only supported disk. Waits used some PDP6 devices (DTC,MTC,DCS,DTC). Also the following special devices for Waits should be enabled (DKB, III). The DTC should be configured to “dct=02”, and the MTC should be configured to “dct=01”. After 1975 Stanford added a BBN pager unit to their KA, to run later versions of WAITS, “set cpu waits maoff bbn 512k”. The “waits” flag enables the FIX and XCTR instructions for waits. “maoff” moves the trap vectors to the 0140. For systems numbers over 49, BBN pager needs to also be enabled on the CPU.
