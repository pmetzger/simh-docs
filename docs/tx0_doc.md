**TX-0 Simulator Usage**

**09-Nov-2012**

**COPYRIGHT NOTICE**

The following copyright notice applies to the SIMH source, binary, and documentation:

> Original code published in 1993-2008, written by Robert M Supnik
>
> TX-0 Simulator Copyright (c) 2009-2012, Howard M. Harte
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL HOWARD M HARTE BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
>
> CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
>
> Except as contained in this notice, the name of Howard M Harte shall not be used in advertising or otherwise to promote the sale, use or other dealings in this Software without prior written authorization from Howard M Harte.

[1 Simulator Files 3](#simulator-files)

[2 TX-0 Features 3](#tx-0-features)

[2.1 CPU 4](#cpu)

[2.2 I/O Devices 5](#io-devices)

[2.2.1 Photoelectric Tape Reader (PETR) 5](#photoelectric-tape-reader-petr)

[2.2.2 Paper Tape Punch (PTP) 5](#paper-tape-punch-ptp)

[2.2.3 Console Typewriter Input (TTI), Output (TTO) 6](#console-typewriter-input-tti-output-tto)

[2.2.4 Display (DPY) 6](#display-dpy)

[2.2.5 Light Pen (PEN) 6](#light-pen-pen)

[3 TX-0 Usage Examples 6](#tx-0-usage-examples)

[3.1.1 Tic-Tac-Toe Game 7](#tic-tac-toe-game)

[3.1.2 MOUSE Game 7](#mouse-game)

This memorandum documents the TX-0 simulator.

# Simulator Files

sim/ scp.h

sim_console.h

sim_defs.h

sim_fio.h

sim_rev.h

sim_sock.h

sim_timer.h

sim_tmxr.h

scp.c

sim_console.c

sim_fio.c

sim_sock.c

sim_timer.c

sim_tmxr.c

sim/TX-0/ tx0_defs.h

tx0_cpu.c

tx0_stddev.c

tx0_sys.c

tx0_sys_orig.c

# TX-0 Features

The TX-0 is configured as follows:

> device name(s) simulates
>
> CPU TX-0 CPU with up to 64KW of memory
>
> PETR Photo Electric Tape Rader
>
> PTP Paper Tape Punch
>
> TTI FlexoWriter input
>
> TTO FlexoWriter output
>
> DPY 512x512 7” high-persistence phosphor CRT Display

The TX-0 simulator implements the following unique stop conditions:

- An unimplemented operator is referenced, and register STOP_OPR is set

- An invalid interrupt request is made

The LOAD commands has an optional argument to specify the load address:

LOAD \<filename\> {\<starting address\>}

The LOAD command loads a paper-tape bootstrap format file at the specified address. If no address is specified, loading starts at location 200. The DUMP command is not supported.

## CPU

The TX-0 was upgraded over the years from the original 1956 “Standard” instruction set to a later “Extended” instruction set completed in 1962. In addition, the TX-0 CPU can operate in one of three operating modes:

Read In Mode Instruction words are fetched from the tape.

Test Mode Instruction words are fetched from the TBR.

Normal Mode Instruction words are fetched from memory.

only CPU options are the presence of the extended arithmetic operator and the size of main memory.

SET CPU TX0STD set CPU model to TX-0 “Standard.”

SET CPU TX0EXT set CPU model to TX-0 “Extended.”

SET CPU NORMAL set CPU mode to Normal Mode

SET CPU TEST set CPU Mode to Test Mode

SET CPU READIN set CPU Mode to Read In Mode

SET CPU 4K set memory size = 4K (T-Memory)

SET CPU 8K set memory size = 8K (T-Memory)

SET CPU 64K set memory size = 64K (Core Memory)

If memory size is being reduced, and the memory being truncated contains non-zero data, the simulator asks for confirmation. Data in the truncated portion of memory is lost. Initial memory size is 64K. The default configuration is a TX-0 with AO, EAO, and GPR.

CPU registers include the visible state of the processor as well as the control registers for the interrupt system.

name size comments

MBR 18 Memory Buffer Register

AC 18 Accumulator

MAR 16 Memory Address Register

PC 16 Program Counter

IR 5 Instruction Register (5 bits in Extended

> Mode, 2 bits in Standard Mode)

LR 18 Live Register

TBR 18 Toggle Switch Buffer Register

TAC 18 Toggle Switch Accumulator

XR 14 Index Register (Extended Mode Only)

T 1 Test Mode flip-flop (Read Only)

R 1 Read In Mode flip-flop (Read Only)

LP 2 Light Pen / Light Gun flip-flops.

## I/O Devices

The TX-0 includes several I/O devices, and unlike more modern machines, these devices are not memory or I/O mapped, but rather have specific CPU operate orders to access them.

### Photoelectric Tape Reader (PETR)

The PETR is a 250 line per minute Ferranti photoelectric paper tape reader using standard seven-hole Flexowriter tape that was modified to solid state circuitry. Lines without seventh hole punched are ignored by the PETR. As each line of the tape is read in, the data is stored into an 18-bit BUF register with bits mapped as follows:

Tape BUF

1.  0

2.  3

3.  6

4.  9

5.  12

6.  15

Up to three lines of tape may be read into a single the single BUF register. Before subsequent lines are read, the BUF register is cycled one bit right.

The PETR reads data from or a disk file. The POS register specifies the number of the next data item to be read. Thus, by changing POS, the user can backspace or advance the reader.

The PETR supports the BOOT command. BOOT PETR switches the CPU to Read-In mode, and starts the processor running.

The paper tape reader implements these registers:

name size comments

BUF 18 18-bit buffer to store up to three lines of

> Paper tape input

POS 32 position in the input file

### Paper Tape Punch (PTP)

The paper tape punch (PTP) punches standard seven-hole Flexowriter tape. The POS register specifies the number of the next data item to be written. Thus, by changing POS, the user can backspace or advance the punch.

The paper tape punch implements these registers:

name size comments

BUF 8 last data item processed

POS 32 position in the output file

### Console Typewriter Input (TTI), Output (TTO)

The Typewriter is a half-duplex electric Friden Flexowriter typewriter. The typewriter input (TTI) polls the console keyboard for input. The typewriter output (TTO) writes to the simulator console window. On input, TTI converts the ASCII character received from the keyboard to Flexowriter code. On output, the TTO converts the Flexowriter code to ASCII for display on the simulator console window.

The typewriter input implements these registers:

name size comments

BUF 6 typewriter buffer (shared)

UC 1 upper case/lower case state (shared)

DONE 1 input ready flag

POS 32 number of characters input

TIME 24 keyboard polling interval

The typewriter output implements these registers:

name size comments

BUF 6 typewriter buffer (shared)

UC 1 upper case/lower case state (shared)

RPLS 1 return restart pulse flag

DONE 1 output done flag

POS 32 number of characters output

TIME 24 time from I/O initiation to interrupt

### Display (DPY)

In 1957, a 10 inch, electro-static deflection, cathode ray tube, having 512 by 512 addressable locations, in a 7 by 7 inch raster, point by point display system was installed on the TX-0. In simulation, the display is accomplished using a separate graphical display window.

The display is accessed via the DIS order, with the coordinate specified in the AC. DIS will intensify a point with x and y coordinates where X is specified by AC digits 0-8, and Y is specified by AC digits 9-17. Bit 0 is the sign for X, and bit 9 is the sign for Y. The complement system is in effect when signs are negative.

### Light Pen (PEN)

In 1958, a Light-Pen, a solid state version of an idea being developed for the Sage System, was added to the TX-0. The light pen status is read using the PEN order, with light pen flip-flops 1 and 2 being read into AC positions 0 and 1 respectively.

In simulation, the light pen is implemented using a computer mouse or touch screen.

# TX-0 Usage Examples

Several example tapes can be used to test the TX-0 simulation.

### Tic-Tac-Toe Game

The tic-tac-toe game can be run using the tic.simh startup script:

> ; TX-0 Initialization file for the tic-tac-toe game
>
> set dpy enable
>
> att petr bin_tic-tac-toe_new_code_12-16-61.bin
>
> boot petr
>
> g

In this game, you simply use the light pen (mouse) to select where you want to place the “X” (the TX-0 is “O.”). You must click right on top of or very close to the dot in the center of the square you want to place your “X” into.

### MOUSE Game

The MOUSE game, written by John E. Ward is a more complex game with requires involvement not only with the light pen, but also the TAC. In simulation, the TAC can be accessed by stopping the simulation and using the Deposit command to change the value of the TAC to select various modes.

Here is the configuration file, mouse.simh:

> ; TX-0 Initialization file for the Mouse Maze Game
>
> set dpy enable
>
> att petr bin_newMouse_3-22-66.bin
>
> ; The mouse maze game mode is manipulated under TAC control.
>
> ; 400014 = Erase Wall mode.
>
> ; 400024 = Write Wall mode.
>
> ; 400011 = Erase Cheese mode.
>
> ; 400021 = Write Cheese mode.
>
> ; 400012 = Erase Mouse mode.
>
> ; 400022 = Write Mouse mode.
>
> ; 400002 = "do mouse" (start the mouse searching.)
>
> ; 400017 = "do over"
>
> ; Start in "Erase Wall" mode
>
> d tac 400014
>
> boot petr

When this game starts, it is in “Erase Wall” mode. Use this mode to create the maze, by using the computer mouse to erase unwanted walls from the grid. When done, press Control-E to get to the simulator command prompt, and change to “Write Cheese” mode:

> C:\\TX-0.exe mouse.simh
>
> TX-0 simulator V4.0-0
>
> mouse.simh-16\> boot petr
>
> Simulation stopped, PC: 000161 (trn 00275 (Transfer Negative))
>
> sim\> d tac 400021
>
> sim\> g

Next place the cheese with the computer mouse, and then repeat the procedure above to change the TAC to “do mouse” mode. The TX-0 should then solve the maze. Sometimes the mouse gets stuck and is not able to find the cheese. I tend to believe this is a bug in the simulation, but I have not been able to find it yet.
