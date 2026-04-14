# IBM 701 Simulator Usage

Revision of 25-Jul-2018

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

The IBM 701 simulator was written by Richard Cornwell.

# Table of Contents

[1. Introduction](#introduction)
[2. Simulator Files](#simulator-files)
[3. IBM 701 Features](#ibm-701-features)
[3.1 CPU](#cpu)
[3.2 I/O Channel (CH)](#io-channel-ch)
[3.3 Peripherals](#peripherals)
[3.3.1 711 Card Reader (CDR)](#card-reader-cdr)
[3.3.2 721 Card Punch (CDP)](#card-punch-cdp)
[3.3.3 716 Line Printer (LP)](#line-printer-lp)
[3.3.4 729 Magnetic Tape (MT)](#magnetic-tape-mt)
[3.3.5 733 Drum (DR)](#drum-dr)
[4 Symbolic Display and Input](#symbolic-display-and-input)
[5 Sim Load](#sim-load)
[6 Character Codes](#character-codes)

# 1. Introduction

The IBM 701 also know as "Defense Calculator" was introduced by IBM on
April 7, 1953. This computer was start of IBM 700 and 7000 line. Memory
was 2048 36 bit words. Each instruction could be signed plus or minus,
plus would access memory as 18 bit words, minus as 36 bit words. There
was a expansion option to add another 2048 words of memory, but I can't
find documentation on how it worked. Memory cycle time was 12
microseconds. The 701 was withdrawn from the market October 1, 1954
replaced by 704 and 702. A total of 19 machines were installed.

# 2. Simulator Files

To compile the IBM 701, you must define USE_INT64 and I701 as part of
the compilation command line.

| Subdirectory | File | Contains |
|---|---|---|
| `I7000` | `i7000_defs.h` | IBM 7000 simulators general definitions |
|  | `i701_defs.h` | IBM 701 simulator specific definitions |
|  | `i7000_chan.c` | Generic channel interface |
|  | `i701_cpu.c` | 701 CPU, channel, interface |
|  | `i701_chan.c` | 701 channel |
|  | `i701_sys.c` | 701 system interface |
|  | `i7090_cdr.c` | 711 card reader |
|  | `i7090_cdp.c` | 721 card punch |
|  | `i7090_lpr.c` | 716 line printer |
|  | `i7090_drum.c` | 733 drum memory interface |
|  | `i7000_mt.c` | 729 tape controller |

# 3. IBM 701 Features

The IBM 701 simulator is configured as follows:

| Device Name(s) | Simulates |
|---|---|
| `CPU` | 701 CPU with 2KW of memory |
| `CH` | 701 channel device |
| `MT` | 729 magnetic tape controller |
| `CDR` | 711 card reader |
| `CDP` | 721 card punch |
| `LP` | 716 line printer |
| `DR0` | 733 drum |

The 701 simulator implements several unique stop condition:

- undefined CPU instruction

- divide check on a divide and halt instruction

- write select of a write protected device

The LOAD command will load a card binary image file into memory. An
octal dump file, or a pseudo assembly code.

## 3.1 CPU

Memory size is 2KW on a standard CPU.

CPU registers include the visible state of the processor as well as the
control registers for the interrupt system.

| Name | Size | Comments         |
|------------|------------|------------------------|
| IC         | 15         | Program Counter        |
| AC         | 38         | Accumulator            |
| MQ         | 36         | Multiplier-Quotient    |
| SW1..SW6   | 1          | Sense Switches 1..6    |
| SW         | 6          | Sense Switches         |
| SL1..4     | 1          | Sense Lights 1..4      |
| ACOVF      | 1          | AC Overflow Indicator  |
| DVC        | 1          | Divide Check Indicator |
| IOC        | 1          | I/O Check Indicator    |

The CPU can maintain a history of the most recently executed
instructions. This is controlled by the SET CPU HISTORY and SHOW CPU
HISTORY commands:

|                    |                                      |
|--------------------|--------------------------------------|
| SET CPU HISTORY    | clear history buffer                 |
| SET CPU HISTORY=0  | disable history                      |
| SET CPU HISTORY=n  | enable history, length = n           |
| SHOW CPU HISTORY   | print CPU history                    |
| SHOW CPU HISTORY=n | print first n entries of CPU history |

## 3.2 I/O Channel (CH)

The channel device on the 701 is only used by simulator, and has no
controls or registers.

## 3.3 Peripherals

### 3.3.1 711 Card Reader (CDR)

The card reader (CDR) reads data from a disk file. Cards are simulated
as ASCII lines with terminating newlines. Card reader files can either
be text (one character per column) or column binary (two characters per
column). The file type can be specified with a set command:

| Command | Action |
|---|---|
| `SET CDR FORMAT=TEXT` | sets ASCII text mode |
| `SET CDR FORMAT=BINARY` | sets binary card-image mode |
| `SET CDR FORMAT=BCD` | sets BCD-record mode |
| `SET CDR FORMAT=CBN` | sets column-binary BCD-record mode |
| `SET CDR FORMAT=AUTO` | automatically determines format |

or in the ATTACH command:

| Command | Action |
|---|---|
| `ATTACH CDR <file>` | attaches a file |
| `ATTACH CDR -f <format> <file>` | attaches a file with the given format |
| `ATTACH CDR -s <file>` | adds a file onto the current cards to read |
| `ATTACH CDR -e <file>` | sets an end-of-file flag after the file is read |

The card reader can be booted with the:

| Command | Action |
|---|---|
| `BOOT CDR` | loads the first 3 words of the card |

Error handling is as follows:

| Error | Processed As |
|---|---|
| not attached | report error and stop |
| end of file | out of cards |
| OS I/O error | report error and stop |

### 3.3.2 721 Card Punch (CDP)

The card reader (CDP) writes data to a disk file. Cards are simulated as
ASCII lines with terminating newlines. Card punch files can either be
text (one character per column) or column binary (two characters per
column). The file type can be specified with a set command:

| Command | Action |
|---|---|
| `SET CDP FORMAT=TEXT` | sets ASCII text mode |
| `SET CDP FORMAT=BINARY` | sets binary card-image mode |
| `SET CDP FORMAT=BCD` | sets BCD-record mode |
| `SET CDP FORMAT=CBN` | sets column-binary BCD-record mode |
| `SET CDP FORMAT=AUTO` | automatically determines format |

or in the ATTACH command:

| Command | Action |
|---|---|
| `ATTACH CDP <file>` | attaches a file |
| `ATTACH CDP -f <format> <file>` | attaches a file with the given format |

Error handling is as follows:

| Error | Processed As |
|---|---|
| not attached | report error and stop |
| OS I/O error | report error and stop |

### 3.3.3 716 Line Printer (LP)

The line printer (LP) writes data to a disk file as ASCII text with
terminating newlines. Currently set to handle standard signals to
control paper advance.

| Command | Action |
|---|---|
| `SET LP NO/ECHO` | sets echoing of line-printer output to the console |
| `SET LP LINESPERPAGE=lpp` | sets the number of lines per page |

The Printer supports the following SPRA *n* selection pulses for
controlling spacing (spacing occurs before the line is printed):

| Pulse | Action |
|---|---|
| `SPRA 1` | top of form |
| `SPRA 2` | single space |
| `SPRA 3` | double space before printing |
| `SPRA 4` | triple space before printing |
| `SPRA 9` | suppress linefeed after print; prints characters 73-120 |
| `SPT` | skips if any printer line has been pulsed |

Default with no SPRA is to single space before printing.

Error handling is as follows:

| Error | Processed As |
|---|---|
| not attached | report error and stop |
| OS I/O error | report error and stop |

### 3.3.4 729 Magnetic Tape (MT)

These come in groups of 10 units each. MT0 is unit 10.

Each individual tape drive support several options: MTA used as an example.

|                       |                                      |
|-----------------------|--------------------------------------|
| SET MT*n* REWIND      | Sets the mag tape to the load point. |
| SET MT*n* LOCKED      | Sets the mag tape to be read only.   |
| SET MT*n* WRITEENABLE | Sets the mag tape to be writable.    |
| SET MT*n* LOW         | Sets mag tape to low density.        |
| SET MT*n* HIGH        | Sets mag tape to high density.       |

Options: Density LOW/HIGH does not change format of how tapes are
written. And is only for informational purposes only.

Tape drives can be booted with:

|            |                                      |
|------------|--------------------------------------|
| BOOT MT*n* | Read in first three words of record. |

### 3.3.5 733 Drum (DR)

Up to 16 units can be attached to the CPU, all are on pseudo channel 0.
Each drum is 2048K words in size. They are all stored in one file.

|                   |                                              |
|-------------------|----------------------------------------------|
| SET DR0 UNITS=*n* | Set number of units to of storage to attach. |

Drum unit 0 can be booted with:

|           |                                      |
|-----------|--------------------------------------|
| BOOT DR0n | Read in first three words of record. |

# 4 Symbolic Display and Input

The IBM 701 simulator implements symbolic display and input. These are
controlled by the following switches to the EXAMINE and DEPOSIT
commands:

|     |                                     |
|-----|-------------------------------------|
| -m  | Display/Enter Symbolic Machine Code |
| -c  | Display/Enter BCD Characters        |
|     | Display/Enter Octal data.           |

The symbolic input/display supports one instruction-display format:

```
<opcode>,<sign><octal address>,<opcode>,<sign><octal address>
```

A negative address specifies the lower 18 bits of the given memory location.

# 5 Sim Load

The load command looks at the extension of the file to determine how to
load the file.

| Extension | Description |
|---|---|
| `.crd` | Loads a card-image file into memory in standard 709 format plus a one-card loader. |
| `.oct` | Loads an octal deck: `address <blank> octal <blank> octal...` |
| `.sym` | Loads a 709 symbolic deck: `address instruction..`, `address BCD string`, `address OCT octal`, `octal` |

# 6 Character Codes

| Commercial | Scientific | ASCII | BCD | Card   | Remark       |
|------------|------------|-------|-----|--------|--------------|
|            |            |       | 00  |        | Blank        |
| 1          |            | 0     | 01  | 1      |              |
| 2          |            | 0     | 02  | 2      |              |
| 3          |            | 0     | 03  | 3      |              |
| 4          |            | 0     | 04  | 4      |              |
| 5          |            | 0     | 05  | 5      |              |
| 6          |            | 0     | 06  | 6      |              |
| 7          |            | 0     | 07  | 7      |              |
| 8          |            | 0     | 10  | 8      |              |
| 9          |            | 0     | 11  | 9      |              |
| 0          |            | 0     | 12  | 10     |              |
| `#`        | `=`        | `=`   | 13  | 3-8    |              |
| @          | '          | '/@   | 14  | 4-8    |              |
| :          |            | :     | 15  | 5-8    |              |
| `>`        |            | `>`   | 16  | 6-8    |              |
| √          |            | "     | 17  | 7-8    | Tape Mark    |
| ƀ          |            | `_`   | 20  | 2-8    |              |
| /          |            | /     | 21  | 10-1   |              |
| S          |            | S     | 22  | 10-1   |              |
| T          |            | T     | 23  | 10-2   |              |
| U          |            | U     | 24  | 10-3   |              |
| V          |            | V     | 25  | 10-4   |              |
| W          |            | W     | 26  | 10-5   |              |
| X          |            | X     | 27  | 10-6   |              |
| Y          |            | Y     | 30  | 10-7   |              |
| Z          |            | Z     | 31  | 10-8   |              |
| `#`        |            | `#`   | 32  | 10-2-8 | Word Mark    |
| ,          |            | ,     | 33  | 10-3-8 |              |
| %          | (          | %/(   | 34  | 10-4-8 |              |
| `` ` ``    |            | `` ` `` | 35  | 10-5-8 |              |
| `\`        |            | `\`   | 36  | 10-6-8 |              |
| ⧻          |            | {     | 37  | 10-7-8 | Segment Mark |

| Commercial | Scientific | ASCII | BCD | Card   | Remark     |
|------------|------------|-------|-----|--------|------------|
| `-`        |            | `-`   | 40  | 11     | also -0    |
| J          |            | J     | 41  | 11-1   |            |
| K          |            | K     | 42  | 11-2   |            |
| L          |            | L     | 43  | 11-3   |            |
| M          |            | M     | 44  | 11-4   |            |
| N          |            | N     | 45  | 11-5   |            |
| O          |            | O     | 46  | 11-6   |            |
| P          |            | P     | 47  | 11-7   |            |
| Q          |            | Q     | 50  | 11-8   |            |
| R          |            | R     | 51  | 11-9   |            |
| !          |            | !     | 52  | 11-2-8 |            |
| `$`        |            | `$`   | 53  | 11-3-8 |            |
| `*`        |            | `*`   | 54  | 11-4-8 |            |
| `]`        |            | `]`   | 55  | 11-5-8 |            |
| ;          |            | ;     | 56  | 11-6-8 |            |
| △          |            | ^     | 57  | 11-7-8 |            |
| `&`        | `+`        | `&/+` | 60  | 12     | also +0    |
| A          |            | A     | 61  | 12-1   |            |
| B          |            | B     | 62  | 12-2   |            |
| C          |            | C     | 63  | 12-3   |            |
| D          |            | D     | 64  | 12-4   |            |
| E          |            | E     | 65  | 12-5   |            |
| F          |            | F     | 66  | 12-6   |            |
| G          |            | G     | 67  | 12-7   |            |
| H          |            | H     | 70  | 12-8   |            |
| I          |            | I     | 71  | 12-9   |            |
| ?          |            | ?     | 72  | 12-2-8 |            |
| .          |            | .     | 73  | 12-3-8 |            |
| ⌑          | )          | )     | 74  | 12-4-8 | Lozenge    |
| `[`        |            | `[`   | 75  | 12-5-8 |            |
| `<`        |            | `<`   | 76  | 12-3-8 |            |
| `⧻*`       |            | `|`   | 77  | 12-7-8 | Group Mark |
