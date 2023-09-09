# inferno-pico

Tiny shell to work over uart.  
Get devcons to work with keyboard input and output.
Buy pico-W and get wifi working with inferno.
Test multiple progs communicating over chans.
Test math and floating point.
Blink an LED from Limbo via devuart.
Add keyring and crypt.
Add device support for i2c, gpio, uart, spi, rtc, adc, dma, pio, pwm, clocks.
Add multicore.
Add tinyusb device, host.
Add tflite.
Add screen and mouse.
Add storage device.
Add wifi and IP.




## BUGS
/cmd does not work on amd64 / 9ferno.


a .dis file that allocates too little memory for a type desc
will overwrite memory in the pool.

# Pico Plan 9 SDK
Compile 5c, 5s binary to run on pico.
plan9 tools build environment for pico.
u2f for usb bootloader.

Start with pico-sdk blink.
Convert u2f to bin.
Use picotool to output blink to board.
Setup gdb debugging using OpenOCD and SWD.
Setup UART output.
Compile using 5a
Convert using u2f.py
Copy to tool.
Compile native inferno using gcc.
Build an inferno-os kernel image using the Pico SDK and GCC.

Plan is to use Plan 9 compilers to generate arm binary to
program RP2040.

Get blink code to run on Pico using 5c/5a, etc.
Implement SPI. Follow example from Ben Eater and
implement for RPI using 5c.
 
115200 8N1  8 bits, Odd parity, ,1 stop bit.
GPIO14 ALT0 
 
Try writing to mini-uart register instead of PL11 register.
 
minicom -b 115200 -o -D /dev/serial0


lwip has posix sockets interface.  Use emu/port/devip.c and ipif-posix.c to call
lwip and use the pico_w network stack.



5c/5a/5l Plan 9 compiler can add header for AIF files for RISCOS.

THere is a version of 8l that can output ELF.  

A plan9 toolchain running on Linux to boot native C programs
and inferno on bare metal ICs.  Plan 9 c lib.  Docker containers including compiler tool chain and std libraries. See plan9cc repo.

A docker container of Inferno-os to run on K8s clusters for serving
virtual file systems and web services. The inferno 12factor app.

Acme-SAC packaged in snap for easy use on Linux. Also try the windows and Mac package managers.

Native inferno on embedded ARM devices for IoT clusters.


Instead of modifying all the os/port code I could have used the -fplan9-extensions option for GCC.


Does dis version of limbo take into account different word size of inferno64.

## Pico instance

Initialized data: 58424
BSS: 30560
Stack: 8192
mainmem: 86016
heap: 68800
Total: 251992

Inferno-os internals

There are multiple Proc's running within their own scheduler sched().  
They use setlabel, gotolabel to do non-local jumps to execute any one
of the procs.
 
There is one vmachine running.  The vmachine can have multiple Prog threads. 
It is scheduling the work among the threads. Inside dis.c:
If there are multiple Procs, they are acquiring and releasing the vmachine
as then need to return execution to the limbo code.

Interrupts for timers are also running.


## Instructions for JTAG debugging.
$ openocd -f interface/raspberrypi-swd.cfg -f target/rp2040.cfg

$ gdb-multiarch ipico
(gdb) target remote localhost:3333
(gdb) load
(gdb) monitor reset init
(gdb) continue


32bit arm calling conventions
- r0-r3 are argument and scratch registers; r0-r1 results registers.
- r4-r8 are callee-save registers
- r9 sometimes a special register 
- r10-r11 callee-save registers
- r12-r15 special registers    r13=sp, r14=lr, r15=pc


Is it possible during calls to sched() and gotolabel/setlabel,
that callee-saved registers are not getting properly restored.
gotolabel and setlabel should really save and restore all
callee-saved registers, or just all registers.

Or is it the interrupt routine?  Other os implementations just
save pc and sp in setlabel.

Between the register r6 getting set and being used there
are function calls to unlock, sched, and splhi.

procsave()/procrestore()  FPsave / FPrestore

Need to implement procsave and procrestore.
Just store away r4-r8 for now.
Use Ureg structure defined in /n/local/home/pi/github/inferno-pico/Pico/arm/include/ureg.h

Or use FPU FPenv. FPU is already in Proc.
up->fpsave.env.regs;

The two biggest problems in the port were not having enough memory allocated to 
the right pools and the implementation of gotolabel and setlabel. I didn't
know about literal pools in arm thumb and the need to store callee-saved registers
when doing no-local gotos.
