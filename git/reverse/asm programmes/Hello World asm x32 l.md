
![[Pasted image 20250211122705.png]]se référer au x64 pour l'explication global

les registres sont différents 

pour call wright c'est 4 ici est pas 1
on peux pas faire syscall faut l'appeller en hexadécimal ici int 0x80

pour le compiler aussi c'est différent 

**nasm -f elf32 a32.asm -o a32.o**

ducoup c'est plus elf64 mais 32 pour x32

**ld -m elf_i386  a32.o -o a32  **

ici faut lui dire avec -m que c'est du x32
sinon ta cette erreur

``ld -m a32.o -o a32          

``ld: unrecognised emulation mode: a32.o

``Supported emulations: elf_x86_64 elf32_x86_64 elf_i386 elf_iamcu i386pep i386pe

c'est d'ailleurs avec cette erreur que tu vois qu'il faut prendre l'option i386