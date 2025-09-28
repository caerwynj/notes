# Notes on architecture of Inferno OS kernel

My notes on the inferno architecture. Includes links to Inferno OS manual, especially section
10 which describes a few kernel functions. 

## Contents
0. Files
1. Inferno Sys Interface
2. Kernel Interface
3. Kernel Channels
4. Devices
5. VMachine
6. Procs
7. Kernel Startup
8. Dis interp
9. JIT compiler
9. Memory
10. Queues
11. Synchronization
12. Locks
13. Interrupts
14. Clocks
15. Mount device
16. Networking

## Files

Everything is a file. The guiding principle of Inferno is to represent everything as a file,
with a standard set of operations that can be peformed on a file, such as open, read, write,
seek, create, remove, stat.  Whether these files are local or remote, or they represent 
data on a disk, in memory, or some device, they should all appear and work just like files.

## Inferno Sys Interface

- [sys module](https://inferno-os.org/inferno/man/2/sys.html)

	/os/port/inferno.c

This implements the interface the kernel exposes via the Sys module in Limbo.
Many of these methods, after mapping from Limbo to C interfaces, make calls
to the kernel interface.

The system interface calls are here prefixed by `Sys_`, such as open, pipe, dup,
create, read, write, seek, mount, bind, remove, stat, dial, listen, export.


## Kernel Interface

The inferno interface and the rest of the kernel call the kernel interface for system operations.

	/os/port/sysfile.c

Most of the implementation of kread, kwrite, kopen, etc in sysfile use kernel `Chan` channels
to devices for I/O.

Note that C libraries called from the dis VM that wish to make system calls, such as libdraw,
use the `discall.c` interface for `libwrite()` and `libread()`.  These
routines release and acquire at the VM as needed before calling the kernel interface.


## Kernel Channels

- [newchan](https://inferno-os.org/inferno/man/10/newchan.html)

	/os/port/chan.c

A fundamental abstraction in the kernel is `Chan`, a channel to a device.  It is the I/O interface 
 from the kernel to devices that are represented as file systems. 
Inside the Chan abstraction is where the semantics of binds and mounts are implemented.
It is where all the devices are unified into a namespace.  The function `namec`
takes a name and returns a channel in the current namespace.

A kernel `kopen` will use `namec` to open a channel to a device. 
Open file descriptors in the process groups file descriptor table `Fgrp` are returned
as channels to the device.  `fd2chan` turns a file descriptor number into a Chan.

## Devices

- [devattach](https://inferno-os.org/inferno/man/10/devattach.html)

	/os/port/dev.c

The device table `devtab`  lists all the devices compiled into the kernel. 
The data structure links to the method calls for each device that can clone
and walk channels in its file system structure.
	

## VMachine

	/os/port/dis.c:disinit

Disinit opens '#c/cons' for stdin, stdout, stderr. It loads the first dis program.
Enters `vmachine()`. Inside `vmachine()` is an infinite loop as the inferno scheduler for dis Progs.

Whenever I/O needs to occur with a device the vmachine is released and another proc is handed control.
When the I/O completes the proc will acquire the vmachine where it will wait until it is handed control.
Control of vmachine moves between kernel proces as they have work to do if a dis prog.

## Procs 

- [kproc](https://inferno-os.org/inferno/man/10/kproc.html)

Inferno will run a number of kernel Procs, on different cores if available.
The vmachine can only be running within one Proc at a time. 
The other Procs are typically waiting on I/O.


### Proc Scheduler and Switching

	runq

	sched()

	ready()

	setlabel()

	gotolabel()

These functions switch between kernel procs in a native kernel. In a virtual kernel the pthreads library is used.

## Kernel Startup

	/os/port/picow/main.c

## Dis Interpreter

	/libinterp/xec.c

The dis interpreter executes instructions from a loaded dis file.

## JIT compiler

    /libinterp/comp-amd64.c
    /libinterp/comp-arm64.c

The JIT compiler will convert dis instructions into native instructions.
The compiled instructions replace the loaded dis instructions and the `xec()`
function will jump to the native instructions instead of interpreting them.

## Memory

- [xalloc](https://inferno-os.org/inferno/man/10/xalloc.html)
- [pool](https://plan9.io/magic/man2html/2/pool)
- [malloc](https://inferno-os.org/inferno/man/1/malloc.html)

Xalloc is the coarse grain allocator. 

	/os/port/xalloc.c

Pool is the memory pools used by alloctors in the rest of the kernel.
It creates three pools, main, heap, and image. The `malloc` method
used by the rest of the kernel is based upon the main pool.

	/os/port/alloc.c

Heap is the memory management for the dis interpreter. It uses
the heap pool above. 

	/libinterp/heap.c


## Queues

- [allocb](https://inferno-os.org/inferno/man/10/allocb.html)
- [qio](https://inferno-os.org/inferno/man/10/qio.html)

	/os/port/qio.c

# Synchronization

- [sleep, wakeup](https://inferno-os.org/inferno/man/10/sleep.html)

## Locks

- [lock](https://inferno-os.org/infero/man/10/lock.html)
- [qlock](https://inferno-os.org/infero/man/10/qlock.html)


## Interrupts

- [intrenable](https://inferno-os.org/inferno/man/10/intrenable.html)
- [splhi, spllo](https://inferno-os.org/inferno/man/10/spli.html)


## Clocks

- [delay](https;//inferno-os.org/inferno/man/10/delay.html)

	/os/port/portclock.c:addclock0link

This is the kernel method to add a timed interrupt. Calls a platform specific method `timerset()` 
to enable to interrupt. The interrupt will call `timerintr()` to handle the event.



## Mount Device


## Networks
