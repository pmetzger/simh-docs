**PDP-4/7/9/15 Simulator Usage**

**28-Apr-2020**

**COPYRIGHT NOTICE**

The following copyright notice applies to the SIMH source, binary, and documentation:

> Original code published in 1993-2017, written by Robert M Supnik
>
> Copyright (c) 1993-2017 Robert M Supnik
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL ROBERT M SUPNIK BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
>
> CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
>
> Except as contained in this notice, the name of Robert M Supnik shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from Robert M Supnik.

[1 Simulator Files 3](#simulator-files)

[2 18b PDP Features 3](#b-pdp-features)

[2.1 CPU 5](#cpu)

[2.2 Floating Point Processor (FPP) 7](#floating-point-processor-fpp)

[2.3 Programmed I/O Devices 8](#programmed-io-devices)

[2.3.1 Paper Tape Reader (PTR) 8](#paper-tape-reader-ptr)

[2.3.2 Paper Tape Punch (PTP) 9](#paper-tape-punch-ptp)

[2.3.3 Terminal Input (TTI) 9](#terminal-input-tti)

[2.3.4 Terminal Output (TTO) 10](#terminal-output-tto)

[2.3.5 Line Printers (LPT, LP9) 10](#line-printers-lpt-lp9)

[2.3.6 Real-Time Clock (CLK) 12](#real-time-clock-clk)

[2.3.7 Additional Terminals (TTIX, TTOX) 12](#additional-terminals-ttix-ttox)

[2.4 RP15/RP02/RP03 Disk Pack (RP) 13](#rp15rp02rp03-disk-pack-rp)

[2.5 Type 24/RM09 Serial Drum (DRM) 14](#type-24rm09-serial-drum-drm)

[2.6 RB09 Fixed Head Disk (RB) 14](#rb09-fixed-head-disk-rb)

[2.7 RF09/RF15/RS09 Fixed Head Disk (RF) 15](#rf09rf15rs09-fixed-head-disk-rf)

[2.8 Type 550/555, TC02/TU55, and TC15/TU56 DECtape (DT) 16](#type-550555-tc02tu55-and-tc15tu56-dectape-dt)

[2.9 TC59/TU10 Magnetic Tape (MT) 17](#tc59tu10-magnetic-tape-mt)

[2.10 DR15C Parallel Interface (PDP-15/76 only) 18](#dr15c-parallel-interface-pdp-1576-only)

[3 Symbolic Display and Input 18](#symbolic-display-and-input)

[4 Character Sets 20](#character-sets)

This memorandum documents the DEC PDP-4, PDP-7, PDP-9, and PDP-15 simulators.

# Simulator Files

To compile a particular model in the 18b family, you must include the appropriate

switch in the compilation command line:

PDP-4/ PDP4

PDP-7/ PDP7

PDP-9/ PDP9

PDP-15/ PDP15

If no model is specified, the default is the PDP-15.

PDP-4 PDP-7 PDP-9 PDP-15

sim/ sim_defs.h x x x x

sim_rev.h x x x x

sim_sock.h x x x x

sim_tape.h x x

sim_tmxr.h x x x x

scp.c x x x x

scp_tty.c x x x x

sim_sock.c x x x x

sim_tape.c x x

sim_tmxr.c x x x x

sim/pdp18b/ pdp18b_defs.h x x x x

pdp18b_cpu.c x x x x

pdp18b_drm.c x x x

pdp18b_dt.c x x x x

pdp18b_fpp.c x

pdp18b_lp.c x x x x

pdp18b_mt.c x x

pdp18b_rb.c x x

pdp18b_rf.c x x

pdp18b_rp.c x

pdp18b_stddev.c x x x x

pdp18b_sys.c x x x x

pdp18b_tt1.c x x

pdp18b_dr15.c x

# 18b PDP Features

The four 18b PDP's (PDP-4, PDP-7, PDP-9, PDP-15) are very similar and are configured as follows:

system device name(s) simulates

PDP-4 CPU PDP-4 CPU with 8KW of memory

\- Type 18 extended arithmetic element (EAE)

PTR,PTP integral paper tape/Type 75 punch

TTI,TTO KSR28 console terminal (Baudot code)

LPT Type 62 line printer (Hollerith code)

CLK integral real-time clock

DT Type 550/555 DECtape

DRM Type 24 serial drum

PDP-7 CPU PDP-7 CPU with 32KW of memory

\- Type 177 extended arithmetic element (EAE)

\- Type 148 memory extension

PTR,PTP Type 444 paper tape reader/Type 75 punch

TTI,TTO KSR 33 console terminal

LPT Type 647 line printer

CLK integral real-time clock

DT Type 550/555 DECtape

DRM Type 24 serial drum

RB RB09 fixed head disk

PDP-9 CPU PDP-9 CPU with 32KW of memory

\- KE09A extended arithmetic element (EAE)

\- KF09A automatic priority interrupt (API)

\- KG09B memory extension

\- KP09A power detection

\- KX09A memory protection

PTR,PTP PC09A paper tape reader/punch

TTI,TTO KSR 33 console terminal

TTIX,TTOX 1-4 LT09A additional terminals

LP9 LP09 line printer

LPT Type 647E line printer

CLK integral real-time clock

DRM RM09 serial drum

RB RB09 fixed-head disk

RF RF09/RS09 fixed-head disk

DT TC02/TU55 DECtape

MT TC59/TU10 magnetic tape

PDP-15 CPU PDP-15 CPU with 32KW of memory

\- KE15 extended arithmetic element (EAE)

\- KA15 automatic priority interrupt (API)

\- KF15 power detection

\- KM15 memory protection

\- KT15 memory relocation and protection

\- XVM memory relocation and protection

FPP FP15 floating point processor

PTR,PTP PC15 paper tape reader/punch

TTI,TTO KSR 35 console terminal

TTIX,TTOX 1-16 LT15/LT19 additional terminals

LP9 LP09 line printer

LPT LP15 line printer

CLK integral real-time clock

RP RP15/RP02/RP03 disk pack

RF RF15/RS09 fixed-head disk

DT TC15/TU56 DECtape

MT TC59/TU10 magnetic tape

DR DR15C parallel buffer (for UC15)

Most devices can be disabled or enabled, by the commands:

SET \<dev\> DISABLED

SET \<dev\> ENABLED

The simulator allows most device numbers to be changed, by the command:

SET \<dev\> DEV=\<number\>

However, devices can only be booted with their default device numbers.

The 18b PDP simulators implement several unique stop conditions:

- An unimplemented instruction is decoded, and register STOP_INST is set

- More than XCT_MAX nested executes are detected during instruction execution

- An FP15 instruction is decoded, the FP15 is disabled, and register STOP_FPP is set

- A simulated DECtape runs off the end of its reel, and register STOP_OFFR is set

The LOAD command supports three different file formats:

- PDP-7/9/15 hardware read-in RIM format files (data only loaded into sequential addresses)

- PDP-4/7 "second stage" RIM format files (alternating DAC address instructions and data)

- PDP-9/15 binary loader format files

The load file format can be specified by switches:

- –R: hardware read-in RIM format

- –S: second stage RIM format

- –B: binary loader format

If no switch is specified, the load file format is determined from the file extension. Files ending in .RIM are assumed to be RIM format (hardware versus second stage is determined from the data); files ending in any other extension are assumed to be binary loader format. Examples:

LOAD -R file address load PDP-9/PDP-15 RIM format file

starting at address

LOAD -S file load PDP-4/PDP-7 RIM format file

LOAD file.RIM address assume file is RIM, determine type from data

LOAD -B file load PDP-9/PDP-15 BIN format file

LOAD file.BIN assume file is PDP-9/PDP-15 BIN format

If no address is given for a RIM format load, a starting address of 200 (octal) is assumed.

The DUMP command is not supported.

## CPU

The CPU options are the presence of the EAE, the presence of the API and memory protection (for the PDP-9 and PDP-15), the presence of relocation or XVM (PDP-15 only), and the size of main memory.

system option comment

all SET CPU EAE enable EAE

all SET CPU NOEAE disable EAE

9,15 SET CPU API enable API

9,15 SET CPU NOAPI disable API

9,15 SET CPU PROT enable memory protection

15 SET CPU RELOC enable memory relocation

15 SET CPU XVM enable XVM relocation

9,15 SET CPU NOPROT disable protection, relocation, XVM

4 SET CPU 4K set memory size = 4K

all SET CPU 8K set memory size = 8K

all SET CPU 12K set memory size = 12K

all SET CPU 16K set memory size = 16K

all SET CPU 20K set memory size = 20K

all SET CPU 24K set memory size = 24K

all SET CPU 28K set memory size = 28K

all SET CPU 32K set memory size = 32K

15 SET CPU 48K set memory size = 48K

15 SET CPU 64K set memory size = 64K

15 SET CPU 80K set memory size = 80K

15 SET CPU 96K set memory size = 96K

15 SET CPU 112K set memory size = 112K

15 SET CPU 128K set memory size = 128K

Memory sizes greater than 8K are only available on the PDP-7, PDP-9, and PDP-15; memory sizes greater than 32KW are only available on the PDP-15. If memory size is being reduced, and the memory being truncated contains non-zero data, the simulator asks for confirmation. Data in the truncated portion of memory is lost. Initial memory size is 8K for the PDP-4, 32K for the PDP-7 and PDP-9, and 128K for the PDP-15.

The PROT option corresponds to the KX09A on the PDP-9 and the KM15 for the PDP-15. The PROT option is required to run the Foreground/Background Monitor. The RELOC option corresponds to the KT15 on the PDP-15, and the XVM option corresponds to the XM15 on the PDP-15. ADSS-15, ADSS-15 Foreground/Background, and standard DOS-15 will \<not\> run if these options are enabled.

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

system name size comments

all PC addr program counter

all AC 18 accumulator

all L 1 link

all MQ 18 multiplier-quotient

all SC 6 shift counter

all EAE_AC_SIGN 1 EAE AC sign

all SR 18 front panel switches

all ASW addr address switches for RIM load

all INT\[0:4\] 32 interrupt requests,

0:3 = API levels 0 to 3

4 = PI level

all IORS 18 IORS register

all ION 1 interrupt enable

all ION_DELAY 2 interrupt enable delay

15 ION_INH 1 interrupt inhibit

9,15 APIENB 1 API enable

9,15 APIREQ 8 API requesting levels

9,15 APIACT 8 API active levels

9,15 BR 18 memory protection bounds

15 XR 18 index register

15 LR 18 limit register

9,15 BR 18 memory protection bounds

15 RR 18 memory protection relocation

15 MMR 18 memory protection control

9,15 USMD 1 user mode

9,15 USMDBUF 1 user mode buffer

9,15 USMDDEF 1 user mode load defer

9,15 NEXM 1 non-existent memory violation

9,15 PRVN 1 privilege violation

7,9 EXTM 1 extend mode

7,9 EXTM_INIT 1 extend mode value after reset

15 BANKM 1 bank mode

15 BANKM_INIT 1 bank mode value after reset

7 TRAPM 1 trap mode

7,9,15 TRAPP 1 trap pending

7,9 EMIRP 1 EMIR instruction pending

9,15 RESTP 1 DBR or RES instruction pending

9,15 PWRFL 1 power fail flag

all PCQ\[0:63\] addr PC prior to last JMP, JMS, CAL, or

interrupt; most recent PC change first

all STOP_INST 1 stop on undefined instruction

all XCT_MAX 8 max number of chained XCT's allowed

all WRU 8 interrupt character

"addr" signifies the address width of the system (13b for the PDP-4, 15b for the PDP-7 and PDP-9, 17b for the PDP-15).

The CPU attempts to detect when the simulator is idle. When idle, the simulator does not use any resources on the host system. Idle detection is controlled by the SET IDLE and SET NOIDLE commands:

SET CPU IDLE enable idle detection

SET CPU NOIDLE disable idle detection

Idle detection is disabled by default. At present, the CPU is considered idle if it is executing a KSF/JMP \*-1 loop with interrupts disabled (DECSYS) or a JMP \* loop (XVM/RSX). There is no idle loop detector for ADSS, F/B, or DOS.

The CPU can maintain a history of the most recently executed instructions. This is controlled by the SET CPU HISTORY and SHOW CPU HISTORY commands:

SET CPU HISTORY clear history buffer

SET CPU HISTORY=0 disable history

SET CPU HISTORY=n enable history, length = n

SHOW CPU HISTORY print CPU history

SHOW CPU HISTORY=n print first n entries of CPU history

The maximum length for the history is 65536 entries.

## Floating Point Processor (FPP)

The PDP-15 features an optional floating point processor, the FP15 (FPP). The FPP can be enabled and disabled; by default it is disabled.

The FPP implements these registers:

name size comments

FIR 12 floating instruction register

EPA 18 EPA (A exponent)

FMAS 1 FMA sign

FMAH 17 FMA\<1:17\>

FMAL 18 FMA\<18:35\>

EPB 18 EPB (B exponent)

FMBS 1 FMB sign

FMBH 17 FMB\<1:17\>

FMBL 18 FMB\<18:35\>

FGUARD 1 guard bit

FMQH 17 FMQ\<1:17\>

FMQL 18 FMQ\<18:35\>

JEA 18 exception address register

STOP_FPP 1 stop if FP15 instruction decoded

while FP15 is disabled

## Programmed I/O Devices

### Paper Tape Reader (PTR)

The paper tape reader (PTR) reads data from a disk file. The POS register specifies the number of the next data item to be read. Thus, by changing POS, the user can backspace or advance the reader.

The paper tape reader supports the BOOT command. The specific forms recognized vary from system to system:

system command comments

4,7 BOOT PTR load RIM loader and start it running

4,7 BOOT -F PTR load funny format loader and start it

running

7 BOOT -H PTR start hardware RIM load at address given

by address switches (ASW)

9,15 BOOT {-H} PTR start hardware RIM load at address

given by address switches (ASW)

The PDP-4 does not have a hardware read-in mode load capability.

The ATTACH PTR command recognizes two switches, -A for ASCII mode and –K for KSR mode. In ASCII mode, data returned by the read alphabetic command has even parity. This allows normal text files to be used as input to the paper tape reader on the PDP-9 and PDP-15. In KSR mode, data returned by the read alphabetic command has forced ones parity. This allows normal text files to be used as input to the paper tape reader on the PDP-7.

The paper tape reader implements these registers:

name size comments

BUF 8 last data item processed

INT 1 interrupt pending flag

DONE 1 device done flag

ERR 1 error flag (PDP-9, PDP-15 only)

POS 32 position in the input file

TIME 24 time from I/O initiation to interrupt

STOP_IOE 1 stop on I/O error

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 out of tape

end of file 1 report error and stop

0 out of tape

OS I/O error x report error and stop

### Paper Tape Punch (PTP)

The paper tape punch (PTP) writes data to a disk file. The POS register specifies the number of the next data item to be written. Thus, by changing POS, the user can backspace or advance the punch. The default position after ATTACH is to position at the end of an existing file. A new file can be created if you attach with the -N switch.

The ATTACH PTP command recognizes one switch, -A for ASCII mode. In ASCII mode, data is punched with the high order bit clear, and NULL and DEL characters are suppressed. This allows punch output to be processed with normal text editing utilities.

The paper tape punch implements these registers:

name size comments

BUF 8 last data item processed

INT 1 interrupt pending flag

DONE 1 device done flag

ERR 1 error flag (PDP-9, PDP-15 only)

POS 32 position in the output file

TIME 24 time from I/O initiation to interrupt

STOP_IOE 1 stop on I/O error

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 out of tape

OS I/O error x report error and stop

### Terminal Input (TTI)

On the PDP-7, PDP-9, and PDP-15, the terminal interfaces (TTI, TTO) can be set to one of four modes, KSR, 7P, 7B, or 8B. On the PDP-7 and PDP-9, “Unix v0” mode is also available:

mode input characters output characters

KSR lower case converted lower case converted to upper case

to upper case, high-order bit cleared,

high-order bit set non-printing characters suppressed

7P high-order bit cleared high-order bit cleared,

non-printing characters suppressed

7B high-order bit cleared high-order bit cleared

8B no changes no changes

UNIX high order bit set no changes

CR, LF interchanged

ESC mapped to ALTMODE

The default mode is KSR.

The console terminal operates, by default, with local echo. The terminal input can be set to FDX (full duplex), which suppresses local echo.

The terminal input (TTI) polls the console keyboard for input. It implements these registers:

name size comments

BUF 8 last data item processed

BUF2ND 5 (PDP-4 only) saved character

INT 1 interrupt pending flag

DONE 1 device done flag

POS 32 number of characters input

TIME 24 input polling interval (if 0, the keyboard

is polled synchronously with the line clock)

### Terminal Output (TTO)

The terminal output (TTO) writes to the simulator console window. It implements these registers:

name size comments

BUF 8 last data item processed

SHIFT 5 (PDP-4 only) letters/figures flag

INT 1 interrupt pending flag

DONE 1 device done flag

POS 32 number of characters output

TIME 24 time from I/O initiation to interrupt

### Line Printers (LPT, LP9)

The line printers (LPT, LP9) write data to a disk file. The POS register specifies the number of the next data item to be written. Thus, by changing POS, the user can backspace or advance the printer. The default position after ATTACH is to position at the end of an existing file. A new file can be created if you attach with the -N switch.

LPT is the "default" line printer for a CPU: Type 62 for the PDP-4, Type 647 for the PDP-7 and PDP-9, and LP15 for the PDP-15. LP9 is the LP09 line printer controller for the PDP-9. It may be needed on the PDP-15 to run certain software packages. LP9 is disabled by default.

The LP15 is a 3-cycle data break device. The current address register is in memory. It can be examined and modified with SET and SHOW commands:

SHOW LPT CA display current

SET LPT CA=value set current address to value

The Type 62 printer controller implements these registers:

name size comments

BUF 8 last data item processed

INT 1 interrupt pending flag

DONE 1 device done flag

SPC 1 spacing done flag

BPTR 6 print buffer pointer

POS 32 position in the output file

TIME 24 time from I/O initiation to interrupt

STOP_IOE 1 stop on I/O error

LBUF\[0:119\] 8 line buffer

The Type 647 printer controller implements these registers:

name size comments

BUF 8 last data item processed

INT 1 interrupt pending flag

DONE 1 device done flag

ENABLE 1 interrupt enable (PDP-9 only)

ERR 1 error flag

BPTR 7 print buffer pointer

POS 32 position in the output file

TIME 24 time from I/O initiation to interrupt

STOP_IOE 1 stop on I/O error

LBUF\[0:119\] 8 line buffer

The LP09 printer controller implements these registers:

name size comments

BUF 7 output character

INT 1 interrupt pending flag

DONE 1 device done flag

ENABLE 1 interrupt enable

ERR 1 error flag

POS 32 position in the output file

TIME 24 time from I/O initiation to interrupt

The LP15 printer controller implements these registers:

name size comments

STA 18 status register

MA 18 DMA memory address

INT 1 interrupt pending flag

ENABLE 1 interrupt enable

LCNT 8 line counter

BPTR 7 print buffer pointer

POS 32 position in the output file

TIME 24 time from I/O initiation to interrupt

STOP_IOE 1 stop on I/O error

LBUF\[0:131\] 8 line buffer

For all printers, error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 out of tape or paper

OS I/O error x report error and stop

### Real-Time Clock (CLK)

The real-time clock (CLK) frequency can be adjusted as follows:

SET CLK 60HZ set frequency to 60Hz

SET CLK 50HZ set frequency to 50Hz

The default is 60Hz.

The clock implements these registers:

name size comments

INT 1 interrupt pending flag

DONE 1 device done flag

ENABLE 1 clock enable

TIME 24 clock frequency

The real-time clock autocalibrates; the clock interval is adjusted up or down so that the clock tracks actual elapsed time.

### Additional Terminals (TTIX, TTOX)

The additional terminals consist of two independent devices, TTIX and TTOX. The entire set is modeled as a terminal multiplexer, with TTIX as the master unit. The additional terminals perform input and output through Telnet sessions connected to a user-specified port. The ATTACH command specifies the port to be used:

ATTACH TTIX \<port\> set up listening port

where port is a decimal number between 1 and 65535 that is not being used for other TCP/IP activities.

The PDP-9 supports 1-4 additional terminals. The PDP-15 supports 1-16 additional terminals. The number of additional terminals can be changed with the command:

SET TTIX LINES=n set number of lines to n

The default is one additional terminal.

The additional terminals can be set to one of four modes, KSR, 7P, 7B, or 8B:

mode input characters output characters

KSR lower case converted lower case converted to upper case

to upper case, high-order bit cleared,

high-order bit set non-printing characters suppressed

7P high-order bit cleared high-order bit cleared,

non-printing characters suppressed

7B high-order bit cleared high-order bit cleared

8B no changes no changes

The default mode is KSR. Finally, each line supports output logging. The SET TTOXn LOG command enables logging on a line:

SET TTOXn LOG=filename log output of line n to filename

The SET TTOXn NOLOG command disables logging and closes the open log file, if any.

Once TTIX is attached and the simulator is running, the terminals listen for connections on the specified port. They assume that the incoming connections are Telnet connections. The connections remain open until disconnected either by the Telnet client, a SET TTOXn DISCONNECT command, or a DETACH TTIX command.

Other special commands:

> SHOW TTIX CONNECTIONS show current connections
>
> SHOW TTIX STATISTICS show statistics for active connections
>
> SET TTOXn DISCONNECT disconnects the specified line.

The input device (TTIX) implements these registers:

name size comments

BUF\[0:3/0:15\] 8 last character received, lines 0 to 3/15

DONE 16 input ready flags, line 0 on right

INT 1 interrupt pending flag

TIME 24 keyboard polling interval

The output device (TTOX) implements these registers:

name size comments

BUF\[0:3/0:15\] 8 last character transmitted, lines 0 to 3/15

DONE 16 output ready flags, line 0 on right

INT 1 interrupt pending flag

TIME\[0:3/0:15\] 24 time from I/O initiation to interrupt,

lines 0 to 3/15

## RP15/RP02/RP03 Disk Pack (RP)

RP15 options include the ability to make units write enabled or write locked and to select the type of disk drive:

SET RPn RP02 set unit n to be an RP02 (default)

SET RPn RP03 set unit n to be an RP03

SET RPn LOCKED set unit n write locked

SET RPn WRITEENABLED set unit n write enabled

Units can also be set ENABLED or DISABLED.

The RP15 implements these registers:

name size comments

STA 18 status A

STB 18 status B

DA 18 disk address

MA 18 current memory address

WC 18 word count

INT 1 interrupt pending flag

BUSY 1 control busy flag

STIME 24 seek time, per cylinder

RTIME 24 rotational delay

STOP_IOE 1 stop on I/O error

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 disk not ready

end of file x assume rest of disk is zero

OS I/O error x report error and stop

## Type 24/RM09 Serial Drum (DRM)

The serial drum (DRM) implements these registers:

name size comments

DA 9 drum address (sector number)

MA 16 current memory address

INT 1 interrupt pending flag

DONE 1 device done flag

ERR 1 error flag

WLK 32 write lock switches

TIME 24 rotational latency, per word

STOP_IOE 1 stop on I/O error

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 disk not ready

Drum data files are buffered in memory; therefore, end of file and OS I/O errors cannot occur.

## RB09 Fixed Head Disk (RB)

The RB09 was an early fixed-head disk for the PDP-7 and PDP-9. It was superceded by the RF09/RS09. It is disabled by default.

The RB09 implements these registers:

name size comments

STA 18 status

DA 20 current disk address

WC 16 word count

MA 15 memory address

INT 1 interrupt pending flag

WLK 20 write lock switches for track groups,

10 tracks per group

TIME 24 rotational delay, per word

BURST 1 burst flag

STOP_IOE 1 stop on I/O error

The RB09 is a data break device. If BURST = 0, word transfers are scheduled individually; if BURST = 1, the entire transfer occurs in a single data break.

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 disk not ready

RB09 data files are buffered in memory; therefore, end of file and OS I/O errors cannot occur.

## RF09/RF15/RS09 Fixed Head Disk (RF)

RF09/RF15 options include the ability to set the number of platters to a fixed value between 1 and 8, or to autosize the number of platters from the attached file:

SET RF 1P one platter (256K)

SET RF 2P two platters (512K)

SET RF 3P three platters (768K)

SET RF 4P four platters (1024K)

SET RF 5P five platters (1280K)

SET RF 6P six platters (1536K)

SET RF 7P seven platters (1792K)

SET RF 8P eight platters (2048K)

SET RF AUTOSIZE autosize on ATTACH

The default is AUTOSIZE.

The RF09/RF15 is a 3-cycle data break device. The word count and current address registers are in memory. They can be examined and modified with SET and SHOW commands:

SHOW RF CA(WC) display current address (word count)

SET RF CA(WC)=value set current address (word count) to value

The RF09/RF15 implements these registers:

name size comments

STA 18 status

DA 21 current disk address

BUF 18 data buffer (diagnostic only)

INT 1 interrupt pending flag

WLK\[0:7\] 16 write lock switches for disks 0 to 7

TIME 24 rotational delay, per word

BURST 1 burst flag

STOP_IOE 1 stop on I/O error

The RF09/RF15 is a three-cycle data break device. If BURST = 0, word transfers are scheduled individually; if BURST = 1, the entire transfer occurs in a single data break.

Error handling is as follows:

error STOP_IOE processed as

not attached 1 report error and stop

0 disk not ready

RF15/RF09 data files are buffered in memory; therefore, end of file and OS I/O errors cannot occur.

## Type 550/555, TC02/TU55, and TC15/TU56 DECtape (DT)

The PDP-4 and PDP-7 use the Type 550 DECtape, a programmed I/O controller. The PDP-9 uses the TC02, and the PDP-15 uses the TC15. The TC02 and TC15 are DMA controllers and programmatically identical. Except for the first five units of the Type 550, PDP-4/7/9/15 DECtape format has 5 18b words in the block header and trailer, like all other DECtapes.

In the 550/555, DECtapes drives are numbered 1-8; in the simulator, drive 8 is unit 0. In the TX02/TC15, DECtape drives are numbered 0-7. DECtape options include the ability to make units write enabled or write locked.

SET DTn WRITEENABLED set unit n write enabled

SET DTn LOCKED set unit n write locked

Units can also be set ENABLED or DISABLED.

The Type 550, TC02, and TC15 support PDP-8 format, PDP-11 format, and 18b format DECtape images. . ATTACH assumes the image is in 18b format; the user can force other choices with switches:

-t PDP-8 format

-s PDP-11 format

-a autoselect based on file size

The DECtape controller is a data-only simulator; the timing and mark track, and block header and trailer, are not stored. Thus, the WRITE TIMING AND MARK TRACK function is not supported; the READ ALL function always returns the hardware standard block header and trailer; and the WRITE ALL function dumps non-data words into the bit bucket.

The TC02 and TC15 are 3-cycle databreak devices. The word count and current address registers are in memory. They can be examined and modified with SET and SHOW commands:

SHOW DT CA(WC) display current address (word count)

SET DT CA(WC)=value set current address (word count) to value

The DECtape controller implements these registers:

system name size comments

all DTSA 12 status register A

all DTSB 12 status register B

all DTDB 18 data buffer

all INT 1 interrupt pending flag

9,15 ENB 1 interrupt enable flag

all DTF 1 DECtape flag

7 BEF 1 block end flag

all ERF 1 error flag

all LTIME 31 time between lines

all DCTIME 31 time to decelerate to a full stop

all SUBSTATE 2 read/write command substate

all POS\[0:7\] 32 position, in lines, units 0 to 7

all STATT\[0:7\] 18 unit state, units 0 to 7

all STOP_OFFR 1 stop on off-reel error

It is critically important to maintain certain timing relationships among the DECtape parameters, or the DECtape simulator will fail to operate correctly.

- LTIME must be at least 6

- DCTIME needs to be at least 100 times LTIME

Acceleration time is set to 75% of deceleration time.

## TC59/TU10 Magnetic Tape (MT)

Magnetic tape options include the ability to make units write enabled or or write locked.

SET MTn LOCKED set unit n write locked

SET MTn WRITEENABLED set unit n write enabled

Magnetic tape units can be set to a specific reel capacity in MB, or to unlimited capacity:

SET MTn CAPAC=m set unit n capacity to m MB (0 = unlimited)

SHOW MTn CAPAC show unit n capacity in MB

Units can also be set ENABLED or DISABLED.

The TC59 is a 3-cycle data break device. The word count and current address registers are in memory. They can be examined and modified with SET and SHOW commands:

SHOW MT CA(WC) display current address (word count)

SET MT CA(WC)=value set current address (word count) to value

The magnetic tape controller implements these registers:

name size comments

CMD 18 command

STA 18 main status

INT 1 interrupt pending flag

STOP_IOE 1 stop on I/O error

TIME 24 record delay

UST\[0:7\] 24 unit status, units 0 to 7

POS\[0:7\] 32 position, units 0 to 7

Error handling is as follows:

error processed as

not attached tape not ready; if STOP_IOE, stop

end of file bad tape

OS I/O error parity error; if STOP_IOE, stop

## DR15C Parallel Interface (PDP-15/76 only)

The DR15C is a parallel interface that provides the PDP-15 side of the UC15 control interface in a PDP-15/76 system. It is disabled by default. Enabling the DR creates the shared memory and status interfaces for communicating with the UC15.

The DR15C implements these registers:

name size comments

TCBP 18 TCBP pointer

TCBACK 1 TCBP write acknowledge

IE 1 interrupt enable

REQ 4 API requests on levels 3..0

API0..3 1 interrupt request, API levels 0..3

APIVEC0..3 7 API vectors, API levels 0..3

POLL 8 polling interval for shared state changes

Usage of the DR15C is covered in a separate document on running a PDP-15/76 configuration.

# Symbolic Display and Input

The 18b PDP simulators implement symbolic display and input. Display is controlled by command line switches:

-a display as ASCII character

-b display as three DECsys Baudot packed characters

-c display as three SIXBIT packed characters

-f display as three FIODEC packed character

-m display instruction mnemonics

The PDP-7 and PDP-9 recognize one additional switch:

-u display as Unix v0 ASCII (two 7b ASCII characters

in 9b bytes, big-endian)

The PDP-15 recognizes two additional switches:

-u display as PDP11 ASCII (two 7b ASCII characters

in 8b bytes, little-endian); 16b devices only

-p display as packed ASCII (five 7b ASCII characters

> in two 18b words)

Input parsing is controlled by the first character typed in or by command line switches:

' or -a ASCII character

" or -c three packed SIXBIT characters

alphabetic instruction mnemonic

numeric octal number

The PDP-7 and PDP-9 recognize one additional input mode:

-u Unix v0 ASCII (two 7b ASCII characters in 9b bytes)

The PDP-15 also recognizes an additional input mode:

-p five character packed ASCII string in two 18b words

-u PDP11 ASCII (two 7b ASCII in 8b bytes, little-endian)

Instruction input uses standard 18b PDP assembler syntax. There are eight instruction classes: memory reference, EAE, index (PDP-15 only), IOT, operate, LAW, FP15 memory reference (PDP-15 only), and FP15 no operand (PDP-15 only).

Memory reference instructions have the format

PDP-4, PDP-7: memref {I} address

PDP-9: memref{\*} address

PDP-15: memref{\*} address{,X}

where I (PDP-4, PDP-7) /\* (PDP-9, PDP-15) signifies indirect reference, and X signifies indexing (PDP-15 in page mode only). The address is an octal number in the range 0 - 017777 (PDP-4, PDP-7, PDP-9, and PDP-15 in bank mode) or 0 - 07777 (PDP-15 in page mode).

IOT instructions consist of single mnemonics, eg, KRB, TLS. IOT instructions may be or'd together

iot iot iot...

IOT's may also include the number 10, signifying clear the accumulator

iot 10

The simulator does not check the legality of IOT combinations. IOT's for which there is no opcode may be specified as IOT n, where n is an octal number in the range 0 - 07777.

EAE instructions have the format

eae {+/- shift count}

EAE instructions may be or'd together

eae eae eae...

The simulator does not check the legality of EAE combinations. EAE's for which there is no opcode may be specified as EAE n, where n is an octal number in the range 0 - 037777.

Index instructions (PDP-15 only) have the format

index {immediate}

The immediate, if allowed, must be in the range of -0400 to +0377.

Operate instructions have the format

opr opr opr...

The simulator does not check the legality of the proposed combination. The operands for MUY and DVI must be deposited explicitly.

The LAW instruction has the format

LAW immediate

where immediate is in the range of 0 to 017777.

FP15 memory reference instructions occupy two successive words and have the format

fpmem{\*} address

where \* signifies indirect addressing. The address is a number in the range 0 - 0377777.

FP15 no operand instructions occupy two successive words and have the format

fpop

The second word is ignored on output and set to 0 on input.

# Character Sets

The PDP-4's console was an ASR-28 Teletype; its character encoding was Baudot. The PDP-4's line printer used a modified Hollerith character set. The PDP-7's and PDP-9's consoles were KSR-33 Teletypes; their character sets were basically ASCII. The PDP-7's and PDP-9's line printers used sixbit encoding (ASCII codes 040 - 0137 masked to six bits). The PDP-15's I/O devices were all ASCII. The following table provides equivalences between ASCII characters and the PDP-4's I/O devices. In the console table, FG stands for figures (upper case).

PDP-4 PDP-4

ASCII console line printer

000 - 006 none none

bell FG+024 none

010 - 011 none none

lf 010 none

013 - 014 none none

cr 002 none

016 - 037 none none

space 004 000

! FG+026 none

" FG+021 none

\# FG+005 none

\$ FG+062 none

% none none

& FG+013 none

' FG+032 none

( FG+036 057

) FG+011 055

\* none 072

\+ none 074

, FG+006 033

\- FG+030 054

. FG+007 073

/ FG+027 021

0 FG+015 020

1 FG+035 001

2 FG+031 002

3 FG+020 003

4 FG+012 004

5 FG+001 005

6 FG+025 006

7 FG+034 007

8 FG+014 010

9 FG+003 011

: FG+016 none

; FG+017 none

\< none 034

= none 053

\> none 034

? FG+023 037

@ none {MID DOT} 040

A 030 061

B 023 062

C 016 063

D 022 064

E 020 065

F 026 066

G 013 067

H 005 070

I 014 071

J 032 041

K 036 042

L 011 043

M 007 044

N 006 045

O 003 046

P 015 047

Q 035 050

R 012 051

S 024 022

T 001 023

U 034 024

V 017 025

W 031 026

X 027 027

Y 025 030

Z 021 031

\[ none none

\\ none {OVERLINE} 056

\] none none

^ none {UP ARROW} 035

\_ none UC+040

0140 - 0177 none none

DECsys Baudot packs the five bit character code, and the figure/letters flag, into six bits as follows:

bits\<0:4\> Baudot 5b character code

bit\<5\> 0 = letters, 1 = figures
