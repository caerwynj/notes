# Inferno Plans

Inferno amd64
Inferno arm64
Inferno dockerfiles: i386, arm, arm64, amd64
Acme-sac dockerfiles: i386, arm, arm64, amd64
Inferno snap  amd64, arm64
Acme snap amd64, arm64
Inferno JIT: arm, arm64

Inferno Raspberry Pi Native
Pico native plan9 compilers build environment

Inferno NDS  Nintendo DS.
Compile Sqlite into inferno-os
Inferno on RiscOS
Hellaphone
Inferno on Android Termux (arm64)
Plan9 arm64 compilers on Android.
Acme SAC Windows 64bit exe
Inferno k8s: styx, owen job scheduler, venti

Inferno smart watch (Cortex M0+) Microchip SAM L22
Tiny Native Inferno on Cortex M0+ 1MB
Inferno-edge DNN running on ARM.
ArmNN Tflite
RISC-V compiler, assembler
inferno-os RISC-V
inferno-os  ESP32, STM32, RP2040, SAML22
Inferno package manager and server (filesystem)

Inferno on STM32F405 https://bitbucket.org/dboddie/inferno-os/branch/thumb2-fp-to-disinit

Inferno computer board.
FreeCAD Inferno computer case &  keyboard design.

PCB design
- Walk through tutorial from beginning to end. STM32, AVR, 
- design breakout board

Smart Watch
AR glasses
Cyberdeck
Pico

RiscOS
Plan9
Plan9port


JIT dead simple app with emu. How to debug JIT.
gdb for emu and JIT.
Test JIT on x86 linux.


Benchmark linpack on arm; interpreted versus JIT versus C.
Inferno native on pico.
Link with arm neon library.
DNN inference library. libNC


http://doc.cat-v.org/inferno/4th_edition/release_notes/install


GPU compute cluster on cloud with inferno.

python styx server for Keras or PyTorch models.


How to setup a signer in k8s?
Need password for keyring.
auth/createsignerkey caerwyn
svc/auth (requires login for /keydb/keys
auth/changelogin (requires info for user)
We don't need to create a lot of users.  We just need one or two.
So we basically need to share an already prepared signerkey, and keys file.

How to setup sytx?
Store keyring in Secret.
That is all it needs. Then run svc/net


kubectl create configmap scripts --from-file=startup

k8s persistence store using NFS.

why can't I attach or exec to an inferno pod in k8s?

use Ansible to install docker on all rpi nodes.
create docker image with docker client so I can run builds on the cluster.
create Job or Task to run in k8s.
create CronJob


inferno compiled in wasm.

Use owen as labor exchange to manage job queue
for building docker images in the k8s cluster instead
of jenkins.


I have 9p as a kernel module. How do I make use of that.
Make use of it within docker too.
What about fuse and ceph? NFS?

snap image of acme-sac
snap image of inferno-os
flatpak

docker run --rm -v /tmp/.X11-unix/:/tmp/.X11-unix -e DISPLAY -h caerwyn-rpi2 -v /home/pi/.Xauthority:/home/inferno/.Xauthority -v $HOME:$HOME acme-sac

Install raspbian lite and openmedia vault for NAS.  Windows SMB. NFS server

Backup process: 3 copies, 2 on different media, 1 at a different location.


PiHole container? DNS

Pi VPN with OpenVPN and Wireguard.  VPN in UK.

Add links to docker from acme-sac/README.md

Fixup the README in home directory.

Cloudfare tcp tunnelling to expose ports from internal network.

With venti I can identify a version of a file tree with a hash.
Run plan9port venti.


Compile 5c, 5s binary to run on pico.
plan9 tools build environment for pico.
u2f for usb bootloader.

One of the rpi's in the cluster will be taken off to be
a boot machine for kali, riscos, inferno, p9.


Inferno on Smart-Watch casio F-91W.
SAM L22 microchip: Arm Cortex M0+ 256KB Flash, 32KB RAM, 32MHz.
USB peripheral, UF2 bootloader.
12 bit ADC. 5 GPIO pins.
SERCOM peripheral with support for I2C, SPI, UART.

Compile and run Limbo on x86-64.

Currently stuck on floating point handling of 0.0/0.0 and
turning result into string. It doesn't seem to tell that
a double is Infinity or NaN. It tries to convert to string.

Why does limbo in inferno-os work but not in acme-sac?
What are the differences?





  
Linux 2022
 - kubectl  (k8s)
 - systemctl  (systemd)
 - ip  (network)
 - apt, dpkg, Dockerfile, k8s.yaml, helm charts (pkg management)
 - git
 - filesystems and mounts, overlay, tmpfs, sysfs, procfs, ext4, fuse
 - LVM
 - namespaces: mnt, pid, net, ipc, uts, user, cgroup. lsns, unshare, ip netns, nsenter


webgrab -r http://10.43.191.89:8080/magic/echo
mount -A tcp!10.43.128.85!registry /mnt/registry
mount -A tcp!10.43.120.118 /n/remote
 
 




# Pico Plan 9 SDK
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





Compile 9front for rpi
uboot or 9load rpi.
9load 9ferno to rpi


lwip has posix sockets interface.  Use emu/port/devip.c and ipif-posix.c to call
lwip and use the pico_w network stack.


