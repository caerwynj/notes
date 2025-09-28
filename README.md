#  Plans

- Inferno amd64
- Inferno arm64
- acme-sac arm64
- acme-sac amd64
- Inferno on Termux
- Inferno on MinGW
- Wayland windows 
- Inferno dockerfiles: i386, arm, arm64, amd64
- Acme-sac dockerfiles: i386, arm, arm64, amd64
- Inferno on Android Termux (arm64), Fire OS.
- Inferno snap/flatpak  amd64, arm64, arm, 386
- Acme-sac snap/flatpak amd64, arm64, arm, 386
- Inferno JIT: arm64
- Inferno JIT: amd64
- Inferno JIT: riscv64
- Inferno Raspberry Pi Native
- Inferno Pico 
- Inferno Pico W  with wifi
- Pico native plan9 compilers build environment
- Inferno on RiscOS
- Inferno NDS  Nintendo DS.
- Embed Sqlite into inferno-os; use kread/kwrite to open sqlite databases in inferno namespace.
- llama2.c styxserver
- yaml parser
- RDF parser
- post quantum encryption algos: ML-KEM, HQC, ML-DSA,SLH-DSA
- libmp Linux-arm64 asm
- libmp Linux-arm64 asm
- Hellaphone
- Plan9 arm64 compilers on Android.
- Acme SAC Windows 64bit exe
- Inferno k8s: styx, owen job scheduler, venti
- Inferno smart watch (Cortex M0+) Microchip SAM L22
- Tiny Native Inferno on Cortex M0+ 1MB
- Inferno-edge DNN running on ARM.
- ArmNN Tflite
- RISC-V compiler, assembler
- inferno-os RISC-V
- inferno-os  ESP32, STM32, RP2040, SAML22
- Inferno package manager and server (filesystem)
- Inferno on STM32F405 https://bitbucket.org/dboddie/inferno-os/branch/thumb2-fp-to-disinit
- inferno compiled in wasm.
- Benchmark linpack on arm; interpreted versus JIT versus C.
- Link with arm neon library.
- DNN inference library. libNC
- FreeCAD Inferno computer case &  keyboard design.
- GPU compute cluster on cloud with inferno.
- python styx server for Keras or PyTorch models.


http://doc.cat-v.org/inferno/4th_edition/release_notes/install



Use owen as labor exchange to manage job queue
for building docker images in the k8s cluster instead
of jenkins.

I have 9p as a kernel module. How do I make use of that?
What about fuse and ceph? NFS?

Backup process: 3 copies, 2 on different media, 1 at a different location.

Add links to docker from acme-sac/README.md
Fixup the README in home directory.

Cloudfare tcp tunnelling to expose ports from internal network.

With venti I can identify a version of a file tree with a hash.
Run plan9port venti.

One of the rpi's in the cluster will be taken off to be
a boot machine for kali, riscos, inferno, p9.

Inferno on Smart-Watch casio F-91W.
SAM L22 microchip: Arm Cortex M0+ 256KB Flash, 32KB RAM, 32MHz.
USB peripheral, UF2 bootloader.
12 bit ADC. 5 GPIO pins.
SERCOM peripheral with support for I2C, SPI, UART.

Currently stuck on floating point handling of 0.0/0.0 and
turning result into string. It doesn't seem to tell that
a double is Infinity or NaN. It tries to convert to string.

Compile 9front for rpi
uboot or 9load rpi.
9load 9ferno to rpi

