
source:  https://www.root-me.org/fr/Challenges/Cracking/ELF-x86-ExploitMe?action_solution=voir&debut_affiche_solutions=7#pagination_affiche_solutions


En c on peux compiler le programme avec no blibliothèques en faisant ça on peut remplacer des fonctions existante comme ça lorsque que le programme appellera la fonction elle appellera notre librairie :

```ltrace ./Exploit_Me\(if_you_can\) test   __libc_start_main(0x8048634, 2, 0xffdfb4a4, 0x80488d0 <unfinished ...>   malloc(29)                                       = 0x9bf0008   memcpy(0x9bf0008, "_0cGj35m9V5T3\303\203\342\200\2418CJ0\303\203\342\202\2549H95h"..., 37) = 0x9bf0008   strcpy(0x804a060, "test")                        = 0x804a060   puts("V\303\203\302\251rification de votre mot de "...VÃ©rification de votre mot de passe..   ) = 40   strcmp("test", "_0cGj35m_.5T3\303\203\342\200\2418CJ0\303\203\342\202\2549H95h"...) = 1   puts("(!) L'authentification a \303\203\302\251cho"...(!) L'authentification a Ã©chouÃ©.   Try again !   ) = 54   +++ exited (status 54) +++```


on voit ici que y'a un strcmp et selon la valeur de return on a la fonction qui affiche le flag 

ducoup on va remplacer strcmp avec notre librairie a nous afin qu'elle retourne toujours 0

```
1. #include <stdio.h>
    
2. #include <stdlib.h>
    
3. int [strcmp](http://www.opengroup.org/onlinepubs/009695399/functions/strcmp.html)(const char* s1, const char* s2){
    
4.     return 0 ;
    
5. }
```

On compile avec : `gcc -m32 -shared -fPIC fake.c -o fake.so`  
et on le charge dans LD_PRELOAD : `export LD_PRELOAD=./fake.so`  
Maintenant on execute le programme :

`./Exploit_Me\(if_you_can\) test   VÃ©rification de votre mot de passe..   [+] Felicitation password de validation de l'épreuve:: 25260060504_VE_T25_*t*_`