COPYRIGHT NOTICE

The following copyright notice applies to both the SIMH source and binary:

Original code published in 1993-2007, written by Robert M Supnik

Copyright (c) 1993-2021, Robert M Supnik

DEC PDP10/KS10 simulator written by Richard Cornwell

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL ROBERT M SUPNIK OR RICHARD CORNWELL BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Except as contained in this notice, the name of Robert M Supnik or Richard Cornwell shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from both Robert M Supnik and Richard Cornwell.

Table of Contents

[1. Introduction 3](#introduction)

[2. Simulator Files 4](#simulator-files)

[3. KS10 Features 5](#ks10-features)

[3.1 CPU 5](#cpu)

[3.2 KS10 front end processor 7](#ks10-front-end-processor)

[3.2.1 Console Teletype (CTY) 7](#console-teletype-cty)

[3.3 Unit record I/O Devices 7](#unit-record-io-devices)

[3.3.1 LP20 printer (LP20) 7](#lp20-printer-lp20)

[3.5 Disk I/O Devices 8](#disk-io-devices)

[3.6 Massbus Devices 8](#massbus-devices)

[3.6.1 RP Disk drives 8](#rp-disk-drives)

[3.6.2 TUA Tape drives. 8](#tua-tape-drives.)

[3.7 Terminal Multiplexer I/O Devices 9](#terminal-multiplexer-io-devices)

[3.7.1 DZ11 Terminal Controller 9](#dz11-terminal-controller)

[3.8 Network Devices. 9](#network-devices.)

[3.8.1 IMP Interface Message Processor 9](#imp-interface-message-processor)

[3.8.2 CH11 Chaosnet interface 11](#ch11-chaosnet-interface)

[3.8.3 KMC11 communications controller. 11](#kmc11-communications-controller.)

[3.8.4 DUP11 network interface 11](#dup11-network-interface)

[4. Symbolic Display and Input 12](#symbolic-display-and-input)

[5. OS specific configurations 13](#os-specific-configurations)

[5.1 ITS 13](#its)

#  Introduction

Originally the DEC PDP10 computer started as the PDP6. This was a 36 bit computer that was designed for timesharing, which was introduced in 1964. The original goal of the machine was to allow for processing of many 6 bit characters at a time. 36 bits were also common in machines like the IBM 7090, GE 645, and Univac 11xx lines. Several systems influenced the design of the PDP6, like CTSS, Lisp, support for larger memory. The PDP6 was canceled by DEC due to production problems. The engineers designed a smaller replacement, which happened to be a 36 bit computer that looked very much like the PDP6. This was called the PDP10. Later renamed to "DECSystem-10". The system supported up to 256K words of memory.

The first PDP10 was labeled KA10, and added a few instructions to the PDP6. Both the PDP6 and PDP10 used a base and limit relocation scheme. The KA10 generally offered two registers, one of user data and the second for user shared code. These were referred to the Low-Segment and High-Segment, the High-Segment could be shared with several jobs. The next version was called KI10 for Integrated. This added support for paging and double precision floating point instructions. It also added 4 sets of registers to improve context switching time. It could also support up to 4Mega words of memory. Following the KI10 was the KL10 (for Low-Cost). The KL10 added double precision integer instructions and instructions to improve COBOL performance. This was the first version which was microcoded. The KL10 was extended to support user programs larger then 256k. The final version to make it to market was the KS10 (for Small), this was a bit-slice version of the PDP10 which used UNIBUS devices which were cheaper then the KL10 devices.

The original operating system for the PDP6/PDP10 was just called "Monitor". It was designed to fit into 6K words. Around the 3rd release Swapping was added. The 6'th release saw the addition of virtual memory. Around the 4'th release it was given the name "TOPS-10". Around this time BBN was working on a paging system and implemented it on the PDP10. This was called "Tenex". This was later adopted by DEC and became "Tops-20".

During the mid 60's a group at MIT, who where not happy with how Multics was being developed decided to create their own operating system which they called Incompatible Timesharing System "ITS", was a play on the original project called Compatible Timesharing System "CTSS". CTSS was implemented by MIT on their IBM 7090 as an experimental system that allowed multiple time sharing users to co-exist on the same machine running batch processing. Hence the term Compatable.

Also during the mid 60's a group at Stanford Artificial Intelligence Laboratory (SAIL), started with a version of TOPS-10 and heavily modified it to become WAITS.

During the 70's Tymshare starting with DEC TOPS-10 system modified it to support random access files, paging with working sets and spawn-able processes. This ran on the KI10, KL10 and KS10.

The PDP10 was ultimately replaced by the VAX.

#  Simulator Files

To compile the DEC 10/KS10 simulator, you must define USE_INT64 as part of the compilation command line.

|  |  |  |
|----|----|----|
| ***Subdirectory*** | ***File*** | ***Contains*** |
| **PDP10** | kx10_defs.h | KL10 simulator definitions |
|  | kx10_cpu.c | KL10 CPU. |
|  | kx10_disk.h | Disk formatter definitions |
|  | kx10_disk.c | Disk formatter routine. |
|  | kx10_sys.c | KS10 System interface |
|  | kx10_rp.c | RH10/RH20 Disk controller, RP04/5/6 Disks. |
|  | kx10_tu.c | RH10/RH20 TM03 Magnetic tape controller. |
|  | kx10_rh.c | RH11 Controller. |
|  | ks10_lp.c | LP 10 Line printer. |
|  | ks10_dz.c | DZ11 communications controller. |
|  | ks10_cty.c | KS10 front panel. |
|  | ks10_uba.c | KS10 Unibus interface. |
|  | kx10_imp.c | IMP11 interface to ethernet. |
|  | ks10_ch11.c | Chaosnet 11 interface. |
|  | ks10_dup.c | DUP11 interface. |
|  | ks10_dup.h | Header file for DUP11 interface |
|  | pdp11_ddcmp.h | Header to define interface for Decnet. |
|  | ks10_kmc.c | KMC communications controller. |

#  

#  KS10 Features

The KS10 simulator is configured as follows:

|                    |                                     |
|--------------------|-------------------------------------|
| **Device Name(s)** | **Simulates**                       |
| **CPU**            | KS10 CPU with 256KW of memory       |
| **CTY**            | Console TTY.                        |
| **LP20**           | LP10 Line Printer                   |
| **RPA**            | RH10/RH20 Disk controllers via RH10 |
| **TUA**            | TM02 Tape controller via RH10/RH20  |
| **DZ**             | DC10 Communications controller.     |
| **IMP**            | IMP network interface               |
| **CH**             | CH10 Chaosnet interface             |
| **DUP**            | DUP11 interface.                    |
| **KDP**            | KMC11 interface.                    |

##  CPU

The CPU options include setting memory size and O/S customization.

|                |                                              |     |
|----------------|----------------------------------------------|-----|
| SET CPU 256K   | Sets memory to 256K                          |     |
| SET CPU 512K   | Sets memory to 512K                          |     |
| SET CPU 1024K  | Set memory to 1024K                          |     |
| SET CPU ITS    | Adds ITS Pager and instruction support to KS | ITS |
| SET CPU NOIDLE | Disables Idle detection                      |     |
| SET CPU IDLE   | Enables Idle detection                       |     |

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

| Name        | Size | Comments                   | OS Type |
|-------------|------|----------------------------|---------|
| PC          | 18   | Program Counter            |         |
| FLAGS       | 18   | Flags                      |         |
| FM0-FM17    | 36   | Fast Memory                |         |
| SW          | 36   | Console SW Register        |         |
| MI          | 36   | Monitor Display            |         |
| PIR         | 8    | Priority Interrupt Request |         |
| PIH         | 8    | Priority Interrupt Hold    |         |
| PIE         | 8    | Priority Interrupt Enable  |         |
| PIENB       | 1    | Enable Priority System     |         |
| BYF5        | 1    | Byte Flag                  |         |
| UUO         | 1    | UUO Cycle                  |         |
| NXM         | 1    | Non-existing memory access |         |
| CLK         | 1    | Clock interrupt            |         |
| OV          | 1    | Overflow enable            |         |
| FOV         | 1    | Floating overflow enable   |         |
| APRIRQ      | 3    | APR Interrupt number       |         |
| UB          | 22   | User Base Pointer          |         |
| EB          | 22   | Executive Base Pointer     |         |
| FMSEL       | 8    | Register set selection     |         |
| SERIAL      | 10   | System serial number       |         |
| PAGE_ENABLE | 1    | Paging system enabled      |         |
| PAGE_FAULT  | 1    | Page fault flag.           |         |
| FAULT_DATA  | 36   | Page fault data            |         |
| SPT         | 18   | Special Page table         |         |
| CST         | 18   | Memory Status table        |         |
| PU          | 36   | User data                  |         |
| CSTM        | 36   | Status Mask                |         |

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

##  **K**S10 front end processor

###  Console Teletype (CTY)

The console station allows for communications with the operating system.

|            |                                     |
|------------|-------------------------------------|
| SET CTY 7B | 7 bit characters, space parity.     |
| SET CTY 8B | 8 bit characters, space parity.     |
| SET CTY 7P | 7 bit characters, space parity.     |
| SET CTY UC | Translate lower case to upper case. |

The CTY also supports a method for stopping TOPS10 operating system.

##  Unit record I/O Devices

###  LP20 printer (LP20)

The line printer (LP20) writes data to a disk file as ASCII text with terminating newlines. Currently set to handle standard signals to control paper advance. Currently only one line printer device is supported.

|  |  |
|----|----|
| SET LP20 LC | Allow printer to print lower case |
| SET LP20 UC | Print only upper case |
| SET LP20 LINESPERPAGE=*n* | Sets the number of lines before an auto form feed is generated. There is an automatic margin of 6 lines. There is a maximum of 100 lines per page. |
| SET LP20 NORMAL | Allows the VFU to be loading into the printer. |
| SET LP20 OPTICAL | VFU is fixed in the printer. |
| SET LP20 ADDR=value | Sets the Unibus address of device. (default: 775400) |
| SET LP20 VECT=value | Sets the interrupt vector of device. (default: 754) |
| SET LP20 BR=value | Sets the interrupt priority of device. (default: 5) |
| SET LP20 CTL=value | Sets the Unibus adapter number. (default: 3) |

## 

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

##  Disk I/O Devices

The PDP10 supported many disk controllers. The KS10 supports only mass bus devices.

##  Massbus Devices

Massbus devices are attached via RH11’s.

###  RP Disk drives 

The KS10 supports one RH11 controller with up to 8 RP drives can be configured. These are addresses as RP Disks can be stored in one of several file formats, SIMH, DBD9 and DLD9. The later two are for compatibility with other simulators.

|  |  |
|----|----|
| SET RPA*n* RP04 | Sets this unit to be an RP04.(19MWords) |
| SET RPA*n* RP06 | Sets this unit to be an RP06.(39MWords) |
| SET RPA*n* RP07 | Sets this unit to be an RP07.(110MWords) |
| SET RPA*n* LOCKED | Sets this unit to be read only. |
| SET RPA*n* WRITEENABLE | Sets this unit to be writable. |
| SET RPA ADDR=value | Sets the Unibus address of device. (default: 776700) |
| SET RPA VECT=value | Sets the interrupt vector of device. (default: 254) |
| SET RPA BR=value | Sets the interrupt priority of device. (default: 6) |
| SET RPA CTL=value | Sets the Unibus adapter number. (default: 1) |

To attach a disk use the ATTACH command:

|  |  |
|----|----|
| ATTACH RP*An* *\<file\>* | Attaches a file |
| ATTACH RA*n* -f *\<format\> \<file\>* | Attaches a file with the given format. |
| ATTACH RPA*n* -n \<file\> | Creates a blank disk. |

Format can be SIMH, DBD9, DLD9.

###  TUA Tape drives.

The TU is a Mass bus tape controller using a TM03 formatter.

|  |  |
|----|----|
| SET TUA*n* LOCKED | Sets the mag tape to be read only. |
| SET TUA*n* WRITEENABLE | Sets the mag tape to be writable. |
| SET TUA ADDR=value | Sets the Unibus address of device. (default: 772440) |
| SET TUA VECT=value | Sets the interrupt vector of device. (default: 224) |
| SET TUA BR=value | Sets the interrupt priority of device. (default: 6) |
| SET TUA CTL=value | Sets the Unibus adapter number. (default: 3) |

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

###  D**Z11** Terminal Controller 

The DZ device was the standard terminal multiplexer for the KS10. Lines came in groups of 8. A max of 32 lines can be supported.

|                   |                                                      |
|-------------------|------------------------------------------------------|
| SET DZ LINES=*n*  | Set the number of lines on the DZ10, multiple of 8.  |
| SET DZ ADDR=value | Sets the Unibus address of device. (default: 760000) |
| SET DZ VECT=value | Sets the interrupt vector of device. (default: 340)  |
| SET DZ BR=value   | Sets the interrupt priority of device. (default: 5)  |
| SET DZ CTL=value  | Sets the Unibus adapter number. (default: 3)         |

##  Network Devices.

###  IMP Interface Message Processor

This allows the KS to connect to the Internet. Currently only supported under ITS. ITS and other OS that used the IMP did not support modern protocols and typically required a complete rebuild to change the IP address. Because of this the IMP processor includes built in NAT and DHCP support. For ITS the system generated IP packets which are translated to the local network. If the HOST is set to 0.0.0.0 there will be no translation. If HOST is set to an IP address then it will be mapped into the address set in IP. If DHCP is enabled the IMP will issue a DHCP request at startup and set IP to the address that is provided. DHCP is enabled by default.

|  |  |
|----|----|
| SET IMP MAC=*xx:xx:xx:xx:xx:xx* | Set the MAC address of the IMP to the value given. |
| SET IMP IP=*ddd.ddd.ddd.ddd*/*dd* | Set the external IP address of the IMP along with the net mask in bits. |
| SET IMP GW=*ddd.ddd.ddd.ddd* | Set the Gateway address for the IMP. |
| SET IMP HOST=*ddd.ddd.ddd.ddd* | Sets the IP address of the PDP10 system. |
| SET IMP DHCP | Allows the IMP to acquire an IP address from the local network via DHCP. Only HOST must be set if this feature is used. |
| SET IMP NODHCP | Disables the IMP from making DHCP queries |
| SET IMP ARP=*ddd.ddd.ddd.ddd= xx:xx:xx:xx:xx:xx* | Creates a static ARP entry for the IP address with the indicated MAC address. |
| SET IMP ADDR=value | Sets the Unibus address of device. (default: 767600) |
| SET IMP VECT=value | Sets the interrupt vector of device. (default: 250) |
| SET IMP BR=value | Sets the interrupt priority of device. (default: 6) |
| SET IMP CTL=value | Sets the Unibus adapter number. (default: 3) |
| SHOW IMP ARP | Displays the IP address to MAC address table. |

The IMP device must be attached to an available Ethernet interface. To determine which devices are available use the “SHOW ETHERNET” to list the potential interfaces. Check out the “0readme_ethernet.txt” from the top of the source directory.

The IMP device can be configured in several ways. Ether it can connect directly to an ethernet port (via TAP) or it can be connected via a TUN interface. If configured via TAP interface the IMP will behave like any other ethernet interface and if asked grab it’s own address. In environments where this is not desired the TUN interface can be used. When configured under a TUN interface the simulated network is a collection of ports on the local host. These can be mapped based on configuration options, see the 0readme_ethernet.txt file as to options.

With the IMP interface, the IP address of the simulated system is static, and under ITS is configured into the system at compile time. This address should be given to the IMP with the “set imp host=\<ip\>”, the IMP will direct all traffic it sees to this address. If this address is not the same as the address of the system as seen by the network, then this address can be set with “set imp ip=\<ip\>”, and “set imp gw=\<ip\>”, or “set imp dhcp” which will allow the IMP to request an address from a local DHCP server. The IMP will translate the packets it receives/sends to look like the appeared from the desired address. The IMP will also correctly translate FTP requests in this configuration.

When running under a TUN interface, IMP is on a virtual 10.0.2.0” network. The default gateway is “10.0.2.1”, with the default IMP at “10.0.2.15”. For this mode DHCP can be used.

###  CH11 Chaosnet interface

Chaos net was another methods of network access for ITS. Chaosnet can be connected to another ITS system or a VAX/PDP11 running BSD Unix or VMS. Chaosnet runs over UDP protocol under UNIX. You must specify a node number, peer to talk to and your local UDP listening port. All UDP packets are sent to the peer for further processing.

|  |  |
|----|----|
| SET CH NODE=*n* | Set the Node number for this system. |
| SET CH PEER=*ddd.ddd.ddd.ddd:dddd* | Set the Peer address and port number. |
| SET CH ADDR=value | Sets the Unibus address of device. (default: 764140) |
| SET CH VECT=value | Sets the interrupt vector of device. (default: 270) |
| SET CH BR=value | Sets the interrupt priority of device. (default: 6) |
| SET CH CTL=value | Sets the Unibus adapter number. (default: 3) |

The CH device must be attached to a UDP port number. This is where it will receive UDP packets from it’s peer.

###  KMC11 communications controller.

The KMC11-A is a general purpose microprocessor that is used in several DEC products. The KDP is an emulation of one of those products: COMM IOP-DUP. The COMM IOP-DUP microcode controls and supervises 1 - 16 DUP-11 synchronous communications line interfaces, providing scatter/gather DMA, message framing, modem control, CRC validation, receiver resynchronization, and address recognition. The DUP-11 lines are assigned to the KMC11 by the (emulated) operating system, but SimH must be told how to connect them.

###  **DUP11 network** interface

Dup11 devices are used by DECNET to link machines together. They are a synchronous serial interface. This is implemented by UDP to send packets to the remote machine.

|  |  |
|----|----|
| SET DUP LINES=*n* | Sets number of DUP 11 lines supported current max is 2. |
| SET DUPn SPEED=bits/sec | Set speed of link, default of 0 means no restrictions. |
| SET DUPn CORRUPTION=factor | Specify corruption factor. |
| SET DUPn W3 | Enable Reset option |
| SET DUPn NOW3 | Disable reset option |
| SET DUPn W5 | Enable A dataset control option. |
| SET DUPn NOW5 | Disable A dataset control option. |
| SET DUPn W6 | Enable A & B dataset control option. |
| SET DUPn NOW6 | Disable A & B dataset control option. |
| SET DUP ADDR=value | Sets the Unibus address of device. (default: 760300) |
| SET DUP VECT=value | Sets the interrupt vector of device. (default: 570) |
| SET DUP BR=value | Sets the interrupt priority of device. (default: 5) |
| SET DUP CTL=value | Sets the Unibus adapter number. (default: 3) |

The DUP device must be attached to a UDP port number. This is where it will receive UDP packets from it’s peer.

Attach

The communication line performs input and output through a TCP session (or UDP session) connected to a user-specified port. The ATTACH command specifies the port to be used as well as the peer address:

sim\> ATTACH DUP0 {interface:}port{,UDP},Connect=peerhost:port

where port is a decimal number between 1 and 65535 that is not being used for other TCP/IP activities.

Specifying symmetric attach configuration (with both a listen port and a peer address) will cause the side receiving an incoming connection to validate that the connection actually comes from the connecction destination system.

A symmetric attach configuration is required when using UDP packet transport.

The default connection uses TCP transport between the local system and the peer. Alternatively, UDP can be used by specifying UDP on the ATTACH command.

For more connection options, view the Help attach command for the DUP.

#  Symbolic Display and Input

The KL10 simulator implements symbolic display and input. These are controlled by the following switches to the EXAMINE and DEPOSIT commands:

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

##  ITS

To run ITS the CPU must be “set cpu ITS”, this will enable the ITS pager.
