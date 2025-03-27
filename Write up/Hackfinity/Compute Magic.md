![[Pasted image 20250323140721.png]]Première étape comme toujours il faut télécharger le fichier, on vérifie ce que c’est avec file on voit que c’est un binaire en ELF-64bit, ensuite on fait un strings dessus la on va voir des trucs intéréssant
![[Pasted image 20250323140736.png]]On peut comprendre avec ça que le programme doit lire un fichier contenant le flag si l’input est correct, “Spell check successful” est une string importante elle apparait sûrement au niveau de la fonction qui nous donne le flag, on à aussi une strings chelou ave flag dedans “AhhhF1ag1571GHDS” on peut imaginer que notre input sera comparée avec cette valeur d’une manière ou d’une autre(j’ai essayer cette valeur ça marche pas)

Run le programme localement ne fonctionne pas on peut donc dire adieu à ltrace et strace et passer directement sur un décompileur.

Chacun son favori moi j’ai choisi ghidra

En regardant le main on peux voir que le début sert a initialiser un handler sur le port 9003
![[Pasted image 20250323140825.png]]
ci la fonction qui nous intérresse c’est “checkSpell” elle initalise la valeur local_44 qui sert de comparaison juste en dessous allons voir à quoi elle ressemble cette fameuse fonction
![[Pasted image 20250323140856.png]]Ok on à un switch et une fonction par lettre, selon la première lettre de notre input la fonction associé à cette lettre se lancera, on regarde dans l’assembleur voir si il y’a une fonction qui nous saute aux yeux.
![[Pasted image 20250323140933.png]]
La voila, toutes les fonctions call magic_fail sauf elle, la fonction call check_other et quand on regarde le code en C
![[Pasted image 20250323140956.png]]Ce bout de code va modifier notre input puis la comparer avec Ahh jsp quoi ma couille droite.

Maintenant notre job c’est de trouver une input qui une fois transformer donne Ahh jsp quoi ma couille droite, on a une loop qui pour chaque caractère va ajouter 4 et effectuer un xor avec 0xd en simplifié ça donne:

Pour chaque lettre dans input(16x)

Resultat[i] += (char + 4)XOR 0xd(13)

Ducoup pour le résultat on a juste a faire

Ahh jsp ma couille droite[i] XOR 0xd - 4 = Resultat

Pourquoi ?

XOR c’est une opération bitwise et elle est réversible si A XOR B = C alors B XOR B = A

On fait un petit script python qui nous fait le flag
``` Python
Ahh_jsp_ma_couille_droite = "AhhF1ag1571GHFDS"
original = "" 
for c in Ahh_jsp_ma_couille_droite:
original += chr((ord(c) ^ 0xD) - 4)
print(original)
```

ça donne

HaaG8hf8468FAGEZ
![[Pasted image 20250323141145.png]]

#reverse_engineering 