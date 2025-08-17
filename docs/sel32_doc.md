**SEL-32 Simulator Usage**

**12-Dec-2021**

**COPYRIGHT NOTICE**

The following copyright notice applies to the SE32 source, binary, and documentation:

> Copyright (c) 2018-2022, James C. Bevier
>
> Portions provided by Richard Cornwell, Geert Rolf and other SIMH contributers
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL JAMES C. BEVIER BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
>
> Except as contained in this notice, the name of James C. Bevier shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from James C. Bevier.

[1 Simulator Files 3](#simulator-files)

[2 SEL-32 Features 4](#sel-32-features)

[2.1 Central Processor (CPU) 5](#central-processor-cpu)

[2.2 Memory Mapping 6](#memory-mapping)

[2.3 Interrupt System (INT) 6](#interrupt-system-int)

[2.4 IOP/MFP 7](#iopmfp)

[2.5 IOP Console (CON) 7](#iop-console-con)

[2.6 IOP Line Printer (LPR) 7](#iop-line-printer-lpr)

[2.7 IOP Interval Timer (ITM) 7](#iop-interval-timer-itm)

[2.8 Real-Time Clock (RTC) 8](#real-time-clock-rtc)

[2.9 IOP 8-Line Asynchronous Controller (COM) 8](#iop-8-line-asynchronous-controller-com)

[2.10 2311/2314 UDP Disk Processors (DMA, DMB) 9](#udp-disk-processors-dma-dmb)

[2.11 8064 High Speed Disk Processor (DPA, DPB) 10](#high-speed-disk-processor-dpa-dpb)

[2.12 SCFI SCSI Disk Controller(SDA, SDB) 11](#scfi-scsi-disk-controllersda-sdb)

[2.13 MFP SCSI Disk/Tape Controller(SBA, SBB) 11](#mfp-scsi-disktape-controllersba-sbb)

[2.14 8051 Buffered Tape Processor (MTA & MTB) 12](#buffered-tape-processor-mta-mtb)

[2.15 8516 Ethernet Controller 13](#ethernet-controller)

[3 Symbolic Display 13](#symbolic-display)

[4 Diagnostics for SEL-32 Simulator 13](#diagnostics-for-sel-32-simulator)

This memorandum documents the SEL-32 simulator.

# Simulator Files

sim/ scp.h

scp_help.h

sim_card.h

sim_console.h

sim_defs.h

sim_disk.h

sim_ether.h

sim_fio.h

sim_rev.h

sim_serial.h

sim_sock.h

sim_tape.h

sim_timer.h

sim_tmxr.h

scp.c

sim_card.c

sim_console.c

sim_disk.c

sim_ether.c

sim_fio.c

sim_serial.c

sim_sock.c

sim_tape.c

sim_timer.c

sim_tmxr.c

sim/SEL32/ sel32_defs.h

sel32_chan.c

sel32_clk.c

sel32_com.c

sel32_con.c

sel32_cpu.c

sel32_disk.c

sel32_ec.c

sel32_fltpt.c

sel32_hsdp.c

sel32_iop.c

sel32_lpr.c

> sel32_mfp.c

sel32_mt.c

sel32_scfi.c

> sel32_scsi.c

sel32_sys.c

sim/SEL32/taptools/

cutostap.c

ddcat.c

ddump.c

deblk.c

diagcopy.c

disk2tap.c

diskload.c

eomtap.c

filelist.c

> fileread.c

fmgrcopy.c

makefile

mkdiagtape.c

mkfmtape.c

mkvmtape.c

mpxblk.c

README.md

renum.c

sdtfmgrcopy.c

small.c

tapdump.c

tape2disk.c

tapscan.c

volmcopy.c

sim/SEL32/tests

cpu.icl

diag.ini

diag.tap

sel32_test.ini

SetupNet

testcode.mem

testcode0.l

testcode0.mem

testcpu.l

testcpu.s

sim/SEL32/util

makecode.c

makefile

#  SEL-32 Features

The SEL-32 simulator is configured as follows:

> device name(s) simulates
>
> CPU 32/55, 32/75, 32/27, 32/67, 32/87, 32/97, V6, V9
>
> IOP 8000/8001 IOP controller
>
> MFP 8002 MFP controller
>
> ITM IOP/MFP interval timer
>
> CON IOP console terminal
>
> LPR IOP/MFP line printer
>
> RTC IOP/MFP real-time clock
>
> COMC IOP/MFP 8-line character communications subsystem
>
> DMA 2311/2314 disk processor with up to 8 drives
>
> DMB 2311/2314 disk processor with up to 8 drives
>
> DPA 8064 high speed disk processor with up to 8 drives
>
> DPB 8064 high speed disk processor with up to 8 drives
>
> SBA MFP SCSI bus A disk controller with up to 2 units
>
> SBB MFP SCSI bus B tape controller with up to 2 units
>
> SDA SCFI SCSI disk controller with up to 8 units
>
> SDB SCFI SCSI disk controller with up to 8 units
>
> MTA 8051 9-Trk Buffered tape processor with up to 8 drives
>
> MTB 8051 9-Trk Buffered tape processor with up to 8 drives

Most devices can be disabled or enabled with the SET \<dev\> DISABLED and SET \<dev\> ENABLED commands, respectively. The channel address of all devices can be configured and must be enabled before being able to use the device. SEL32 configurations must match valid UTX or MPX configurations.

The LOAD and DUMP commands are not implemented.

## Central Processor (CPU)

Central processor options include the CPU model, the CPU features, and the size of main memory.

SET CPU 32/55 set CPU model to SEL 32/55

SET CPU 32/75 set CPU model to SEL 32/75

SET CPU 32/27 set CPU model to SEL 32/27

SET CPU 32/67 set CPU model to SEL 32/67

SET CPU 32/87 set CPU Model to SEL 32/87

SET CPU 32/97 set CPU model to SEL 32/97

SET CPU V6 set CPU model to SEL V6

SET CPU V9 set CPU model to SEL V9

SET CPU IDLE set idle mode

SET CPU NOIDLE set no idle mode

SET CPU 256K set memory size = 256KB

SET CPU 512K set memory size = 512KB

SET CPU 1M set memory size = 1024KB

SET CPU 2M set memory size = 2048KB

SET CPU 3M set memory size = 3072KB

SET CPU 4M set memory size = 4096KB

SET CPU 6M set memory size = 6144KB

SET CPU 8M set memory size = 8192KB

SET CPU 12M set memory size = 12288KB

SET CPU 16M set memory size = 16384KB

If memory size is being reduced, and the memory being truncated contains non-zero data, the simulator asks for confirmation. Data in the truncated portion of memory is lost. Initial configuration is SEL 32/27 CPU with 2MB of memory. IOP at channel 0x7E00, Console at 0x7EFC, tape at 0x1000, and disk at 0x0800. Interval Timer at 0x7f04 and Real-Time Clock at 0x7f06.

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

name size comments

PC 24 program counter

GPR\[0..7\] 32 active general registers

BR\[0..7\] 32 active base mode registers

PSD\[2\] 32 processor status doubleword

SPAD\[256\] 32 CPU Scratchpad memory

MAPC\[1024\] 32 CPU map register cache

CC 32 condition codes bits 1-4

TLB\[2048\] 32 CPU Translation Lookaside Buffer

CPUSTATUS 32 CPU status bits

TRAPSTATUS 32 Trap status bits

INTS\[112\] 32 Interrupt Status words

BOOTR\[0..7\] 32 Register contents at boot time

CMCR 32 Cache Memory Control Register

SMCR 32 Shared Memory Control Register

CCW 32 Computer Configuration Word

CSW 32 Console Switches

BPIX 32 Map count for O/S

CPIXPL 32 Map count for current CPIX

CPIX 32 Master Process list index for CPIX

CSW 32 Console Switches

HIWM 32 Count of maps loaded on last map load

The CPU can maintain a history of the most recently executed instructions. This is controlled by the SET CPU HISTORY and SHOW CPU HISTORY commands:

SET CPU HISTORY clear history buffer

SET CPU HISTORY=0 disable history

SET CPU HISTORY=n enable history, length = n

SHOW CPU HISTORY print CPU history

SHOW CPU HISTORY=n print first n entries of CPU history

The maximum length for the history buffer is 10,000 entries before wrapping around..

## Memory Mapping

Through the late 1970’s to the early 1990’s SEL computers used a variety of memory mapping schemes. The 32/55 was the first PC board SEL computer. Previous computers (8500/8600) had wire-wrapped backplanes to interconnect components. The 32/55 had no memory mapping and had a .5 mb address space. The 32/75 was the first mapped machine and had a 1 mb address space. The 32/27 & 32/87 expanded the address space to 2 mb. The 32/67 & 32/97 had a 16 mb address space and finally the V6 & V9 added virtual memory to the system. The CPU maintains a cache of the current values in the map registers. Depending on the CPU type, an address space is constructed using the current map cache. Addresses are translated if the CPU is operating in the mapped mode. The V6 and V9 CPUs also have demand paging traps that allow a virtual address space. The TLB (Translation Lookaside Buffer) is used to lookup pages (maps) that are in memory and if not resident, execute a page request trap to the O/S. The memory map implements two types of protection when in the unprivileged mode: quarter page write protection and page read/write/execute access protection on virtual addresses. Following is a summary of the mapping/protection for each CPU type:

name maps size task size comments

32/55 0 0 512mb No mapping

32/77 32 32kb 1024mb 8kb r/w protection

32/27 256 8kb 2048mb 8kb r/w protection

32/87 256 8kb 2048mb 8kb r/w protection

32/67 2048 8kb 16384mb 8kb r/w protection

32/97 2048 8kb 16384mb 8kb r/w protection

V6 2048 8kb 16384mb 32mb r/w/x protection

V9 2048 8kb 16384mb 32mb r/w/x protection

## Interrupt System (INT)

The SEL32 implements a multi-level priority interrupt system, with a maximum of 112 interrupt levels. It also maintains interrupt status and settings in the 256 word SPAD memory maintained by the CPU and O/S software. The examine command can be used to display the contents of the INT and SPAD registers.

examine INT\[\[all\] display all INT data.

examine INT\[6\] display interrupt level 6 status.

examine SPAD\[all\] display all SPAD data.

examine SPAD\[24\] display word twenty-four of the SPAD.

## IOP/MFP

The IOP or MFP is an integrated controller controller that allows multiple controllers to be configured. At least one IOP or MFP must be defined for a system and have a RTC/ITM defined and a console TTY. Optional LPR and 8-Line devices can be controlled. The MFP can also control two SCSI disk/tape controllers.

The MPF/IOP must be enabled for it to be active. Set active by:

SET MFP enabled enable MFP or

SET IOP enabled enable IOP

## IOP Console (CON)

The console terminal (CON) consists of two units. Unit 0 is for console input, unit 1 for console output. The console shares the IOP channel controller and interrupt.

The console attention sequence (@@A) is supported via the attention trap at (0xB4). The trap code assigned to this trap level will determine the action that will take place. In MPX, this is usually the operator communications task.

The console must be enabled for it to be active. Set active by:

SET CON enabled enable console I/O

By default, the console terminal is assigned to channel 0x7EFC (input) and 0x7EFD (output) . An IOP console must be defined for the simulator to operate.

## IOP Line Printer (LPR)

The line printer is attached to the IOP at address 0x7EF8 and shares the IOP channel controller and interrupt. The line printer (LPR) writes data to a disk file that is attached to the LPR device. The number of lines per page and the device address can be set.

The line printer can be assigned to a file using the following command:

ATTACH LPR LPOUTPUT set output to file lpoutput

The lines per page can be assigned using the following command:

SET LPR LINESPERPAGE=60 set lines per page to 60

The LPR device address can be assigned using the following command:

SET LPR DEV=7EF8 set device address to 7EF8.

The LPR must be enabled for it to be active. Set active by:

SET LPR enabled enable line printer

By default, the line printer is assigned to channel LP7EF8 with 66 lines per page.

## IOP Interval Timer (ITM)

The SEL32 computer implements an interval timer that can be loaded with a down count. When the count reaches zero, the CPU will be interrupted. The timer can optionally be set to continue down counting or reload a preset values. The clock is started, stopped, read, and written by the software. The count frequency can be set to 38.4us or 76.8us per clock increment. To set the frequency:

SET ITM 3840 set count interval to 38.4us.

SET ITM 7680 set count interval to 76.8us.

The current setting can be displayed by:

SHO ITM

The ITM must be enabled for it to be active. Set active by:

SET ITM enabled enable interval timer

The default address of the interval timer is 0x7F04. The default interrupt level is 0x5f (95).

## Real-Time Clock (RTC)

The SEL32 computer implements a real-time clock. It can be set to a variety of frequencies, 50Hz, 60Hz, 100Hz, and 120hz. The frequency of the clock can be adjusted as follows:

SET RTC freq set clock to specified frequency

The frequency of the clock can be display as follows:

SHOW RTC show current clock frequency

The RTC must be enabled for it to be active. Set active by:

SET RTC enabled enable clock

The real-time clock auto-calibrates; the clock interval is adjusted up or down so that the clock tracks actual elapsed time. The default address is 0x7F06. The default interrupt level is 0x18 (24).

## IOP 8-Line Asynchronous Controller (COM)

The character-oriented communications subsystem implements 8 asynchronous interfaces per controller, with modem control. The subsystem has two controllers: COMC for the scanner and COML for the individual lines. The terminal multiplexer performs input and output through Telnet sessions connected to a user-specified port. The ATTACH command specifies the port to be used:

ATTACH COMC \<port\> set up telnet listening port

where port is a decimal number between 1 and 65535 that is not being used for other TCP/IP activities. Non-root users will require the port to be 1024-65536,

The COMC operates in ASCII. Each line (each unit of COML) supports four character processing modes: UC, 7P, 7B, and 8B.

mode input characters output characters

UC high-order bit cleared, high-order bit cleared,

lower case converted lower case converted

to upper case to upper case

7P high-order bit cleared high-order bit cleared,

non-printing characters suppressed

7B high-order bit cleared high-order bit cleared

8B no changes no changes

The COMC controller must be enabled for it to be active. Set active by:

SET COM enabled enable communications controller

The serial lines can be individually enabled/disabled and must be enabled to be active. Set active by:

SET COMLn enabled enable serial line

The default is UC. In addition, each line supports output logging. The SET CONLn LOG command enables logging on a line:

SET COMLn filename log output of line n to filename

The SET COMLn NOLOG command disables logging and closes the open log file, if any.

Once COMC is attached and enabled and the simulator is running, the multiplexer listens for connections

on the specified port. It assumes that the incoming connections are Telnet connections. The connections remain open until disconnected by the Telnet client, a SET COMC DISCONNECT command, or a DETACH COMC command.

Other special multiplexer commands:

> SHOW COMC CONNECTIONS show current connections
>
> SHOW COMC STATISTICS show statistics for active connections
>
> SET COMLn DISCONNECT disconnects the specified line.

The terminal multiplexer does not support save and restore. All open connections are lost when the simulator shuts down or COMC is detached. By default, the multiplexer is assigned to IOP channel 0x7e00 at devices 0x7EC0-0x7EC8.

##  2311/2314 UDP Disk Processors (DMA, DMB)

The SEL-32 simulator can support up to two F-class moving head disk controllers (DMA, DMB). Each controller can control up to 8 disk drives. Drive can be assigned as 768 byte sectors for MPX or as 1024 byte sectors for UTX. Drives defined within the SEL-32 simulator must match the drive that was defined by the sysgen command file. The UTX model number must match one of the drive numbers supported by UTX. UTX drives are specified by drive number, and MPX by MHXXX type. All drives on a controller must be UTX or MPX with 768 or 1024 byte sectors. Drives on the controller can be mixed by size.

The UDP disk must be enabled for it to be active. Set active by:

SET DMA enabled enable UDP disk

Use the following command to attach a specific drive:

> SET DMA0 TYPE=MH300 sets MPX drive to 768 byte 300MB disk
>
> SET DMA0 TYPE=9344 sets UTX drive to 1024 byte 300MB disk

The default for DMA0 is MH300 with 768 byte sectors with 300MB capacity.

The following drives can be configured:

Model Heads Cylinders Sect/Cyl Sectors Size

9342 5 16 80 823 80MB

8148 10 16 160 823 160MB

9346 19 16 304 815 300MB

8858 24 16 384 711 340MB

8887 10 34 340 819 337MB

8155 40 16 640 843 675MB

8888 16 43 688 865 674MB

MH040 5 20 100 411 40MB

MH080 5 20 100 823 80MB

MH160 10 20 200 823 160MB

MH300 10 34 340 823 300MB

MH600 40 20 800 843 600MB

The command to set a drive to a particular model is:

SET DPn \<drive_type\> SET unit n to the specified drive type

By default, DPA is assigned to channel 0x0800, and DPB is assigned to channel 0x0C00.

##  8064 High Speed Disk Processor (DPA, DPB)

The SEL-32 simulator can support up to two F-class High Speed Disk Processors (DPA, DPB). Each controller can control up to 8 disk drives. Drives can be assigned as 768 byte sectors for MPX or as 1024 byte sectors for UTX. Drives defined within the SEL-32 simulator must match the drive that was defined for the SYSGEN command file. The UTX model number must match one of the drive numbers supported by UTX that is specified when the disk is prepped using the PREP utility. UTX drives are specified by drive number, and MPX by MHXXX type. All drives on a controller must be UTX or MPX with 768 or 1024 byte sectors.. Drives on the controller can be mixed by size.

The HSDP disk must be enabled for it to be active. Set active by:

SET DPA enabled enable DPA HSDP disk

SET DPB enabled enable DPB HSDP disk

Use the following command to attach a specific drive:

> SET DPA0 TYPE=MH300 sets MPX drive to 768 byte 300MB disk
>
> SET DPA0 TYPE=8887 sets UTX drive to 1024 byte 300MB disk

The default for DPA0 is 8887 with 1024 byte sectors with 337MB capacity.

The following drives can be configured:

Model Heads Sectors Sect/Cyl Cyl Size

9342 5 16 80 823 80MB

8148 10 16 160 823 160MB

9346 19 16 304 823 300MB

8858 24 16 384 711 340MB

8887 10 35 350 823 337MB

8155 40 16 640 843 600MB

8888 16 43 688 865 674MB

MH040 5 20 100 411 40MB

MH080 5 20 100 823 80MB

MH160 10 20 200 823 160MB

MH300 19 20 380 823 300MB

MH337 10 45 450 823 337MB

MH600 40 20 800 843 600MB

MH689 16 54 864 865 674MB

By default, DPA is assigned to channel 0x0800, and DPB is assigned to channel 0x0C00.

##  SCFI SCSI Disk Controller(SDA, SDB)

The SEL-32 simulator can support up to two F-class SCFI SCSI Disk controllers (SDA, SDB). Each controller can control up to 8 disk drives. Drive can be assigned as 768 byte sectors for MPX. Drives defined within the SEL-32 simulator must match the drive that was defined by the SYSGEN command file. UTX does not support this disk controller, only MPX-32. MPX drives are specified by SGXXX type. All drives on a controller must be MPX with 768 byte sectors. Drives on the controller can be mixed by size.

Use the following command to attach a specific drive:

> SET SDA0 TYPE=SG300 sets MPX drive to 768 byte 300MB disk

The SCFI disk must be enabled for it to be active. Set active by:

SET SDA enabled enable SCFI disk

SET SDB enabled enable SCFI disk

The default for DPA0 is SG300 with 768 byte sectors with 300MB capacity.

The following drive can be configured:

Model Heads Cylinders Sect/Cyl Sectors Size

SG038 1 20 20 21900 380MB

SG076 1 20 20 46725 760MB

SG1GB 1 40 20 69920 1000MB

SG120 1 40 20 69940 1200MB

The command to set a drive to a particular model is:

SET SDAn \<drive_type\> SET unit n to the specified drive type

By default, SDA is assigned to channel 0x0400, and SDB is assigned to channel 0x0C00. These channel addresses can be changed with the SET SDA0 DEV=0800 command.

## MFP SCSI Disk/Tape Controller(SBA, SBB)

The SEL-32 simulator can support up to two SCSI BUS controllers (SBA, SBB). Each controller can control up to 8 disk drives. Drives can be assigned as 768 byte sectors for MPX or as 1024 byte sectors for UTX. Drives defined within the SEL-32 simulator must match the drive that was defined by the sysgen command file. The UTX model number must match one of the drive numbers supported by UTX. UTX drives are specified by drive number, and MPX by SXXXX type. All drives on a controller must be UTX or MPX with 768 or 1024 byte sectors. Drives on the controller can me mixed by size. The MFP device must be enabled to be able to use the MFP SCSI disks.

The MFP SCSI disk must be enabled for it to be active. Set active by:

SET SBA enabled enable SBA SCSI disk

SET SBB enabled enable SBB SCSI disk

Use the following command to attach a specific drive:

> SET SBA0 TYPE=SD300 sets MPX drive to 768 byte 300MB disk
>
> SET SBA0 TYPE=8821 sets UTX drive to 1024 byte 300MB disk

The default for SBA0 is 8887 with 1024 byte sectors with 300MB capacity.

The following drive can be configured:

Model Heads Cylinders Sect/Cyl Sectors Size

8820 9 18 162 967 150MB

8821 9 36 324 967 300MB

8833 18 20 360 1546 700MB

8835 18 20 360 1931 1200MB

SD150 9 24 162 967 150MB

SD300 9 32 288 1409 300MB

SD700 15 35 525 1546 700MB

SD1200 15 49 735 1931 1200MB

SD2400 19 59 1026 2707 2400MB

SH1200 15 50 750 1872 1200MB

SH2550 19 55 1045 2707 2550MB

SH5150 21 83 1745 3711 5150MB

The command to set a drive to a particular model is:

SET SBAn \<drive_type\> SET unit n to the specified drive type

By default, SDA is assigned to channel 0x0400, and SDB is assigned to channel 0x0C00. These channel addresses can be changed with the SET SDA0 DEV=0800 command.

## 2.14 8051 Buffered Tape Processor (MTA & MTB)

The magnetic tape controller supports up to eight units. MT options include the ability to make units write enabled or write locked.

SET MTAn LOCKED set unit n write locked

SET MTAn WRITEENABLED set unit n write enabled

Magnetic tape units can be set to a specific reel capacity in MB, or to unlimited capacity:

SET MTBn CAPAC=m set unit n capacity to m MB (0 = unlimited)

SHOW MTBn CAPAC show unit n capacity in MB

The tape processor must be enabled for it to be active. Set active by:

SET MTA enabled enable MTA SCSI disk

SET MTB enabled enable MTB SCSI disk

Units can also be set ENABLED or DISABLED. The magnetic tape controller supports the BOOT command. BOOT MTn simulates the standard console fill sequence for unit n.

By default, the magnetic tape is assigned to channel 0x1000 but can be changed with the SET MTA0 DEV=1800 command.

## 2.15 8516 Ethernet Controller

The Ethernet controller allows the user to connect to the internet. The SetupNet script in the tests directory allows the network device to be created on the host system. A MAC address may defined or a default one can be used. The Ethernet device can be connected to the local network using the following directive:

AT EC TAP:TAP0 attach to tap port on host

The transmission mode (0-2) can be set using the following directive:

SET EC MODE=n set mode to 0, 1, or 2

The MAC address can be set with the following directive:

SET EC MAC=XX:XX:XX:XX:XX:XX set MAC address

The Ethernet controller must be enabled for it to be active. Set active by:

SET EC enabled enable Ethernet controller

By default, the Ethernet controller is assigned to channel 0x0e00 but can be changed with the

SET EC DEV=0e00 command. For UTX the MODE needs to be set to MODE=2 and the SET EC MAC = command should not be used.

# Symbolic Display

The SEL-32 simulator implements symbolic display and input when using the EXAMINE command. Display is controlled by command line switches:

-a display as ASCII character (byte addressing)

-b display as byte (byte addressing)

-o display as octal value

-d display as decimal

-h display as hexadecimal

-m display base register instruction mnemonics

-n display non-base register instruction mnemonics

# Diagnostics for SEL-32 Simulator

The operation of the SEL-32 simulator can be verified using the standard SEL diagnostic suite. It is provided in the tests directory. The command file sel32_test.ini is run after compilation of the simulator by the makefile. It uses the diagnostic tape file diag.tap. On the tape is another diagnostic command file that runs each of the diagnostics automatically. The default CPU is the 32/27. The file diag.ini is a file that can be edited to start different simulator configurations and test them using the diag.tap file as input.

Run the diagnostic by inputting: ./sel32 diag.ini at the Linux or Windows command prompt. Output will be to the user terminal and also the sel.log file.
