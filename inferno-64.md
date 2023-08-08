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
