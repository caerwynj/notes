can inferno be compile with 64bit arm and x86_64? Yes!

compile inferno on 64bit armv8 and amd64.
inferno64-os
https://bitbucket.org/inferno-os/inferno64-os/src/master/


# TODO for arm64 Inferno
- comp-arm64.c
- das-arm64.c
- libdynld/dynld-arm64.c
- emu/Linux/asm-arm64.c  FPsave, FPrestore.  Uses <fenv.h>
- segflush. Uses gcc builtin.
- tas. Uses ldaxr, stlxr.  A Gcc atomic builtin would work just as well.
- umult. not implemented.

## Notes on 64bit inferno
Type map for limbo modules.  Assumes 32 bits. But word size is 64 bits.
When it inits the memory for pointers it overwrites memory.

Need to change limbo compiler to generate correct type maps.  
Sizes of word needs to be 64 bit. 

64 bit inferno is not going to be compatible with 32 bit.
Unless treat size as bytes and assume wordlen is 32 bits, but need to translate that when we load and init. So that when calculating the size we look at the pointer map and assume 64 bit size.

Need to recompile using 64bit compatible limbo. The runt.h and limbo type descriptor's will be different.

Compare 32bit .dis with same 64bit .dis.  Is the only different 
the desc type descriptor.


# 9ferno
Compiled the same 9ferno repo on arm64 (rpi) amd64, armv7l (rpi), and termux on android arm64.

Could test the same emu with acme-sac to build a snap or flatpak for acme-sac on multiple platforms.

sys->filechan is different between 9ferno and acme-sac emu. Reports typecheck error.


# Recalculate sizes on load

Iinstead of redefining WORD size and pointer to be 64bit, keep WORD at 32bit,
so dis files remain the same on 32 and 64bit machines.

Instead when loading a module, if on a 64-bit machine, recalculate the memory size for types.

All the mp offsets are double in length.
fp offsets are calculated differently.
All immediate values stay the same.
For double indirect offsets the second indirection is double.

If every type size is based on WORD size. Then double.

Header section data_size could be doubled.

If address mode is small/large offset indirect from MP double the value.

Type descriptor sizes should be doubled.

Review every reference of WORD in libinterp. P(r) would need to change.
Maybe WORD changes to 8 in libinterp. Must also handle IBY2WD.
But Limbo stays the same. And the change is handled in the load.

See dec.c for decoding address mode into offsets into memory.

a linear array of bytes offset by a 32 bit pointer.

Really we only want to handle real pointers differently.

Any pointers stored in the data sections for strings, array,s and types
need to be recalculated.
grep WORD *.c
grep IBY2WD *.c
