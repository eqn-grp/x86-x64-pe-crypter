# x86-x64-pe-crypter

The PE crypter works as follow:
- It decrypts and decompresses the embedded payload PE/DLL binary from its .data section.
- If no key is provided via command line, it uses hardcoded key to decrypt.
- the embedded PE payload is special in sense that it uses custom doublepulsar style pre-pended shellcode.
- so the decrypted PE payload is allocated in memory, changed protection to RX.

- so the main function decrypts and decompresses embededded payload, reads own PE header to find image size,
changes whole memory protection of own to RWX, clears it with kernel32 function RtlZeroMemory and jumps to 
the payload start address via ROP gadget. (remember the Reflective Dll based shellcode is prepended to payload).

- This way our main crypter program don't need to handle manual memory mapping of embedded PE payload in
main function, and the shellcode reads the actual malicious PE appended to it, maps it to main PE
image, changes protection, clears itself and jumps to the entry point via some ROP trickery. 

- RDI.c program was initially written in C, later i rewrote it in assembly for clean call stack and graceful exit
  when embedded internal PE payload returns. also added support for EXE, Dll, cobalt strike PEs. 

The codebase is for learning and archival purpose. expect no support.
