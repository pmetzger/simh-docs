# SIMH Magtape Representation and Handling

Bob Supnik, 30 Aug 06; revised 5 May 17

## Magtape Representation

SIMH represents magnetic tapes as disk files. Each disk file contains a series of objects. Objects are either metadata markers, like tape mark or end of medium, or they are data records. Location 0 of the file is interpreted as beginning of tape; end of file is interpreted as end of medium. Pictorially:

Location 0: +--------+

\| data \|

\| record \|

+--------+

\| data \|

\| record \|

+--------+

:

+--------+

\| tape \|

\| mark \|

+--------+

\| data \|

\| record \|

+--------+

:

end of file:

Metadata markers are 4 bytes stored in little-endian order. The currently defined metadata markers are:

0xFFFFFFFF end of medium

0xFFFFFFFE erase gap

0xFF000000:0xFFFFFFFD reserved

0x00000000 tape mark

Data records consist of an initial 4 byte record length *n*, (n + 1) & ~1 bytes of data, and a trailing 4 byte record length *n* that must be the same as the initial record length:

bytes 0:3 +--------+

\| record \|

\| length \|

+--------+

bytes 4:n+3 \| data \|

\| : \|

\| : \|

+--------+

bytes n+4:n+7 \| record \|

\| length \|

+--------+

Note that the size of the data allocation is rounded to an even number of bytes, while the record length indicates the exact number of bytes in the record. If the record length is odd, the extra byte is undefined but should be 0.

Record lengths are 4 bytes stored in little-endian order. The high order bit is a flag, indicating whether the record contains an error; the next 7 bits must be zero; the low 24 bits are the record length:

bit\<31\> 1 = record contains an error

0 = record is error-free

bits\<30:24\> must be zero

bits\<23:0\> record length, must be non-zero

The leading and trailing record lengths allow a record to be accessed either forward or backward.

## Magtape Operations

Magnetic tape drives can perform the following operations:

- Read forward

- Read backward

- Write forward

- Space forward record(s)

- Space backward record(s)

- Space forward file(s)

- Space backward file(s)

- Write tape mark

- Security erase

- Write erase gap

- Erase a data record or tape mark forward

- Erase a data record or tape mark backward

On a real magtape, all operations are implicitly sequential, that is, they start from the current position of the tape medium. SIMH implements this with the concept of the *current tape position*, kept in the pos field of the tape drive’s UNIT structure. SIMH starts all magtape operations at the current position and updates the current position to reflect the results of the operation:

- Read forward. Starting at the current position, read the next 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, read the data record and position the tape past the trailing record length. If they are a tape mark, signal tape mark and position the tape past the tape mark. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and do not change the tape position.

- Read reverse. If the current position is beginning of tape, signal BOT. Otherwise, starting at the current position, read the preceding 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, read the data record and position the tape before the initial record length. If they are a tape mark, signal tape mark and position the tape before the tape mark. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and position the tape before the end of medium marker.

- Write. Starting at the current position, write the initial record length, followed by the data record, followed by the trailing record length. Position the tape after the trailing record length.

- Space forward record(s). Starting at the current position, read the next 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, position the tape past the trailing record length and continue until operation count exhausted or metadata encountered. If those bytes are a tape mark, signal tape mark and position the tape after the tape mark. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and do not change the tape position.

- Space reverse record(s). If the current position is beginning of tape, signal BOT. Otherwise, starting at the current position, read the preceding 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, position the tape before the initial record length and continue until operation count exhausted, BOT, or metadata encountered. If they are a tape mark, signal tape mark and position the tape before the tape mark. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and position the tape before the end of medium marker.

- Space forward file(s). Starting at the current position, read the next 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, position the tape past the trailing record length and continue. If those bytes are a tape mark, signal tape mark, position the tape after the tape mark, and continue until operation count exhausted. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and do not change the tape position.

- Space reverse file(s). If the current position is beginning of tape, signal BOT. Otherwise, starting at the current position, read the preceding 4 bytes from the file, skipping any intervening gap. If those bytes are a valid record length, position the tape before the initial record length and continue. If they are a tape mark, position the tape before the tape mark and continue until operation count exhausted or BOT. If they are end of medium, or an end of file occurs, signal no more data (‘long gap’ or ‘bad tape’) and position the tape before the end of medium marker.

- Write tape mark. Starting at the current position, write a tape mark marker. Position the tape beyond the new tape mark.

- Security erase. Starting at the current position, write an end of medium marker. Do not update the tape position.

- Write erase gap. Starting at the current position, erase the amount of tape indicated by the specified length. If the end of the gap overwrites an existing record, shorten that record appropriately. Position the tape after the gap.

- Erase record forward. Starting at the current position, erase the amount of tape corresponding to a data record of the specified size or to a tape mark. Position the tape after the gap.

- Erase record backward. If the current position is beginning of tape, signal BOT. Otherwise, starting at the current position, erase in the reverse direction the amount of tape corresponding to a data record of the specified size or to a tape mark. Position the tape before the gap.

A simulator that supports writing erase gaps must first set the tape density. Once this is done, all reading and spacing operations will monitor the length of any intervening gap and will stop the operation and signal tape runaway if the length exceeds the maximum allowed by the applicable ANSI/ECMA standards for the density selected. If the tape density is not set, then the write erase gap operation is not allowed, and gaps of any length that are present in the tape image will be skipped transparently.

It is not necessary to set the tape density before erasing records, as the size of the erasure is specified in the call. However, the size specified must match the size of the record at the current tape position, or an error will occur.

## Magtape Error Handling

The following matrix defines error responses versus events for simulated magtapes. PNU signifies position not updated; PU signifies position updated.

|  | Unit not attached | Tape mark | End of medium mark or end of attached file | Tape runaway | Write locked | Data read or write error |
|----|----|----|----|----|----|----|
| Read forward | Error: unit not ready, PNU | Error: tape mark, PU | Error: bad tape or runaway tape, PNU | Error: tape runaway, PU | ok | Error: parity or data, PNU |
| Read reverse | Error: unit not ready, PNU | Error: tape mark, PU | na | Error: tape runaway, PU | ok | Error: parity or data, PNU |
| Write forward | Error: unit not ready, PNU | n/a | n/a | n/a | Error: unit write locked, PNU | Error: parity or data, PNU |
| Space records forward | Error: unit not ready, PNU | Error: tape mark, PU | Error: bad or runaway tape, PNU | Error: tape runaway, PU | ok | Error: parity or data, PNU |
| Space records reverse | Error: unit not ready, PNU | ok | na | Error: tape runaway, PU | ok | Error: parity or data, PNU if error on record length, otherwise PU |
| Space files forward | Error: unit not ready, PNU | Error: tape mark, PU | Error: bad or runaway tape, PNU | Error: tape runaway, PU | ok | Error: parity or data, PNU |
| Space files reverse | Error: unit not ready, PNU | ok | na | Error: tape runaway, PU | ok | Error: parity or data, PNU if error on record length, otherwise PU |
| Write tape mark | Error: unit not ready, PNU | n/a | n/a | n/a | Error: unit write locked, PNU | Error: parity or data, PNU |
| Erase | Error: unit not ready, PNU | n/a | n/a | n/a | Error: unit write locked, PNU | Error: parity or data, PNU |
| Write gap | Error: unit not ready, PNU | n/a | n/a | n/a | Error: unit write locked, PNU | Error: parity or data, PNU |

The behavior of simulated tapes mirrors that of real tapes, except for errors that make determination of the record length impossible. On a real tape, a read or write error would update the position of the tape. On a simulated tape, this isn’t possible; the length of the record is unknown. Real tape drivers would try to recover from the error by backspacing over the erroneous record and trying again. This won’t work on a simulated tape.

For intelligent tapes, like the TK50 and the TS11, this problem is handled by reporting “position lost”. This status tells the tape driver that tape position is no longer known, and normal error recovery isn’t possible. Older tapes do not have this status. Accordingly, these tapes implement a limited form of state “memory” for error recovery. If an error occurs on a forward operation, and the position is not updated, the simulated tape unit “remembers” this fact. If the next operation is a backspace record, the first backspace is skipped, because the simulated tape is still positioned at the start of the erroneous record. If a read is then attempted, the tape will read the record that caused the original error.

Magtape Emulation Library

SIMH provides a support library, sim_tape.c (and its header file sim_tape.h), that implements the standard tape format and functions. The library is described in detail in the associated document, “Writing A Simulator For The SIMH System”.
