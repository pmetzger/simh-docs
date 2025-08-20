# The SIMH Breakpoint Subsystem

Revision of 9-Sep-2016

**Copyright Notice**

The SIMH source code and documentation is made available under a
X11-style open source license; the precise terms are available at:

<https://github.com/open-simh/simh/blob/master/LICENSE.txt>

## Summary

SIMH provides a highly flexible and extensible breakpoint subsystem to
assist in debugging simulated code. Its features include:

- Up to 26 different kinds of breakpoints (\*)
- Unlimited numbers of breakpoints
- Proceed counts for each breakpoint
- Automatic execution of commands when a breakpoint is taken

If debugging is going to be a major activity on a simulator,
implementation of a full-featured breakpoint facility will be of
immense help to users.

\[\* Breakpoint type `C` should probably be avoided since the `–C` switch
is used by the `SHOW BREAK –C` command to display currently defined
breakpoints as commands which can be entered in a subsequent
invocation of the simulator to recreate the same breakpoint set.\]

## Breakpoint Basics

SIMH breakpoints are characterized by a type, an address, a class, a
proceed count, and an action string. Breakpoint types are arbitrary
and are defined by the virtual machine. Each breakpoint type is
assigned a unique letter. All simulators to date provide execution
(`E`) breakpoints. A useful extension would be to provide breakpoints
on read (`R`) and write (`W`) data access. Even finer gradations are
possible, e.g., physical versus virtual addressing, DMA versus CPU
access, and so on.

Breakpoints can be assigned to devices other than the CPU, but
breakpoints don’t contain a device pointer. Thus, each device must
have its own unique set of breakpoint types. For example, if a
simulator contained a programmable graphics processor, it would need a
separate instruction breakpoint type (e.g., type `G` rather than `E`).

The virtual machine defines the valid breakpoint types to SIMH through
two variables:

- `sim_brk_types` – initialized by the VM (usually in the CPU reset
  routine) to a mask of all supported breakpoints; bit 0 (low order
  bit) corresponds to type `A`, bit 1 to type `B`, etc.

- `sim_brk_dflt` – initialized by the VM to the mask for the default
  breakpoint type.

SIMH in turn provides the virtual machine with a summary of all the
breakpoint types that currently have active breakpoints:

- `sim_brk_summ` – maintained by SIMH; provides a bit mask summary of
  whether any breakpoints of a particular type have been defined.

When the virtual machine reaches the point in its execution cycle
corresponding to a breakpoint type, it tests to see if any breakpoints
of that type are active. If so, it calls `sim_brk_test` to see if a
breakpoint of a specified type (or types) is set at the current
address. Here is an example from the fetch phase, testing for an
execution breakpoint:

```
/* Test for breakpoint before fetching next instruction */

if ((sim_brk_summ & SWMASK (‘E’)) &&
     sim_brk_test (PC, SWMASK (‘E’))) /* <execution break> */
```

If the virtual machine implements only one kind of breakpoint, then
testing `sim_brk_summ` for non-zero suffices. Even if there are multiple
breakpoint types, a simple non-zero test distinguishes the
no-breakpoints case (normal run mode) from debugging mode and provides
sufficient efficiency.

When a breakpoint match is detected by `sim_brk_test` the global
variables `sim_brk_match_type` and `sim_brk_match_addr` are set to
reflect the details of the match that was found. Simulator code can
use this information directly or SIMH provides internal facilities to
report the details of breakpoints which have been matched. For
example:

```
BRKTYPTAB cpu_breakpoints [] = {
    BRKTYPE('E',"Execute Instruction at Virtual Address"),
    BRKTYPE('P',"Execute Instruction at Physical Address"),
    BRKTYPE('R',"Read from Virtual Address"),
    BRKTYPE('S',"Read from Physical Address"),
    BRKTYPE('W',"Write to Virtual Address"),
    BRKTYPE('X',"Write to Physical Address"),
    { 0 }
};

t_stat cpu_reset(DEVICE *dptr)
{
    /* ... */
    sim_brk_dflt = SWMASK ('E');
    sim_brk_types = sim_brk_dflt | SWMASK ('P') |
                    SWMASK ('R') | SWMASK ('S') |
                    SWMASK ('W') | SWMASK ('X');
    sim_brk_type_desc = cpu_breakpoints;
    /* ... */
}
```

In the breakpoint dispatch code something like:

```
reason = STOP_IBKPT;
sim_messagef(reason, "%s", sim_brk_message());
/* [and then sim_instr() returns with:] */
return reason;
```

Or, if it is desirable to suppress the standard message produced when
returning to SCP, the following may be used:

```
reason = STOP_IBKPT;
reason = sim_messagef(reason, "%s\n", sim_brk_message());
/* [and then sim_instr() returns with:] */
return reason;
```
`sim_messagef` produces a message which contains either the breakpoint
type and the matched breakpoint address (if `sim_brk_type_desc` is not
set), or the type mapped to it related description as indicated in the
BRKTYPTAB pointed to by `sim_brk_typ_desc`.

## Testing For Breakpoints

Breakpoint testing must be done at every point in the instruction
decode and execution cycle where an event relating to a breakpoint
type occurs. If a virtual machine implements data breakpoints, it
simplifies implementation if data reads and writes are centralized in
subroutines, rather than scattered throughout the code. For this
reason (among others), it is good practice to perform memory access
through subroutines, rather than by direct access to the memory array.

As an example, consider a virtual machine with a central memory read
subroutine. This routine takes an additional parameter, the type of
read (often required for memory protection):

```
#define IF 0 /* fetch */
#define ID 1 /* indirect */
#define RD 2 /* data read */
#define WR 3 /* data write */

t_statRead (uint32 addr, uint32 *dat, uint32 acctyp)
{
    static uint32 bkpt_type[4] = {
        SWMASK (‘E’), SWMASK (‘N’),
        SWMASK (‘R’), SWMASK (‘W’)
    };

    if ((sim_brk_summ & bkpt_type[acctyp]) &&
         sim_brk_test (addr, bkpt_type[acctyp]))
        return STOP_BKPT;
    else
        *dat = M[addr];

    return SCPE_OK;
}
```

This routine provides differentiated breakpoints for execution,
indirect addresses, and data reads, with a single test.

## The Replay Problem

When a breakpoint is taken, control returns to the SIMH control
package. Depending on the code structure of the simulated system and
the particular type of breakpoint, a breakpoint may be taken before or
after a specific activity has completed. If it is taken before the
operation has actually been performed, when execution resumes, the
same breakpoint will be reached and taken again immediately. This
could result in an endless loop, with the simulator never progressing
beyond a breakpoint.

To address this problem, when a breakpoint is taken, SIMH remembers
the breakpoint that was taken and the instruction executed count when
that particular breakpoint was taken. If the next breakpoint test for
that breakpoint type is to the same address and the instruction
execution count is the same, SIMH suppresses the breakpoint. Thus, the
simulator can make progress past the breakpoint but will take the
breakpoint again if control returns to the same address.

In order to properly suppress replay breakpoints it is important that
the bookkeeping that a simulator does to record the instructions
actually executed not be done when a breakpoint is taken. This
bookkeeping is done by adjustments to `sim_interval` and subsequent
calls to `sim_process_event`. If a simulator returns from
`sim_instr()` due to a breakpoint either the adjustment to
`sim_interval` should be done after all breakpoint checking or the
return logic that handles breakpoints should unwind any `sim_interval`
adjustment that may have happened.

Most simulators will implement a CPU execution breakpoint concept such
that the breakpoint appears to be taken prior to the instruction at
the breakpoint address having executed. This allows for the user to
continue execution from breakpoint and the simulator will produce
precisely the same results as if the breakpoint hadn’t been there. In
order for this to be true, when a breakpoint is taken, not only must
`sim_interval` be restored to its value prior to the breakpoint, but
all other simulator specific state must also be retained. This state
includes program counter, the contents of registers, condition codes
and memory that may have already changed prior to the call to
`sim_brk_test` that causes the breakpoint to be taken. Achieving this
is simplest with basic PC based execution breakpoints and gets more
complicated with breakpoints based on various memory reference
activities.

In most cases, *all* visible state changes made by the instruction
before the breakpoint occurs should be reverted as part of the
breakpoint processing. This avoids confusing users by having an
instruction that appears not to have executed yet, but part of its
effect is nevertheless already visible. For example, if a data write
breakpoint is set for the second word of a double-word store
instruction, the first word may have been written by the simulator
before the breakpoint is recognized. If so, that first word should be
restored. If fully restoring the visible state is impractical for some
reason, it may be acceptable to restore less. If so, this needs to be
clearly documented in the user documentation for the simulator. Also,
in every case enough state must be restored that the instruction can
be replayed correctly. For example, an “add carry” instruction that
may encounter a breakpoint must preserve the carry, otherwise the
replay will produce the wrong answer.

Some processors implement interruptible instructions, where
intermediate state is held in registers and/or memory, and there is
some state flag to indicate an interrupt occurred partway through. For
example the PDP11 CIS instructions do this, using the First Part Done
flag in the processor status word. For such instructions, breakpoint
processing may take advantage of that mechanism; in effect the
breakpoint looks like an interrupt partway through the execution.

If memory access breakpoints are implemented, write breakpoint testing
is best done before the write, so the write can be skipped (rather
than have to be explicitly reverted) on a match. The same may be
useful for read breakpoints, since this allows a read breakpoint to be
set for a memory mapped I/O device register for which reads have a
side effect.

If history recording is implemented, the instruction stopped by the
breakpoint should not appear in the history because it is supposed to
appear not to have executed yet. Depending on the flow of the
simulation, the history may already include that instruction by the
time some of the possible breakpoints are recognized. If so, the
history buffer pointer should be backed up and that record marked as
empty.

## Breakpoint Classes

SIMH implements up to 8 breakpoint classes. Each breakpoint class has
its own state. Thus, if the `E`, `R`, and `W` breakpoints are assigned
to separate classes, each will be suppressed in turn until the next
breakpoint test on that class that fails or that uses a different
address.

Breakpoint classes are arbitrary identifiers and can be assigned by
the simulator writer as desired. The class is specified as part of the
breakpoint type in the call to `sim_brk_test`:

|           |                                |
|-----------|--------------------------------|
| `<31:29>` | = class number (0 by default)  |
| `<25:0>`  | = bit mask of breakpoint types |

Note that breakpoint classes and breakpoint types are
orthogonal. Thus, classes can be used to distinguish different cases
of the same breakpoint type. For example, in an SMP system with ‘n’
processors, classes 0..n-1 could be used for `E`-breakpoints for
processors 0..n-1. Or in a VAX, classes 1..6 could be used for data
breakpoints on operands 1..6, with 0 reserved for the CPU’s
`E`-breakpoints.
