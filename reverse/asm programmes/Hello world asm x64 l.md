
![[Pasted image 20250211121011.png]]

bits 64 en haut indique que c'est en x64

message  db prend un string hello world, 10 = /n et l'associe a message

les instructions de la section .text vont print "message" syscall c'est pour exécuter la fonction

en dessous c'est juste un exit 

pour le lancer :

nasm -f elf64 a64.asm -o a64.o

nasm = compilateur

-f indique le format + de quel fichier

-o nom du fichier compilé

le pc va pas aimer lancer un .o ducoup faut le reconvertir 

ld a64.o -o a.64

ld produit un fichier exectuable 

ld "nom fichier" -o "nom de sortie"
